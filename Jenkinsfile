def waitingIP(String vm_name) {
        println ("Waiting IP...")
        for (i=0; i<12; i++) {
                def vm_ip = sh(returnStdout: true,
                        script: "virsh --quiet domifaddr ${vm_name} | grep vnet | awk '{print \$4}' | sed 's/\\/..//g'").trim()
                if (vm_ip) {
                        println("vm_ip: ${vm_ip} // waiting time ${i}0s")
                        return vm_ip
                }
                sleep 10
        }
        error('Time out wating IP')
}

def sshCall(String command){
        sh "ssh root@${VM_IP} ${command}"
}

def getLog(String pod, String namespace) {
        def logs = sh(returnStdout: true,
                script: "ssh root@${VM_IP} kubectl logs ${pod} --namespace ${namespace}")

        if (pod.contains('rally')) {
                println "Got log from rally pod."
                log_with_title = "[RALLY-RESULT from ${pod}]\n${logs}\n"
        }
        println log_with_title
        return log_with_title
}


def collectLog(String namespace="openstack", String keyword="", String extparam="") {
        def pods = sh(returnStdout: true,
                script: "ssh root@${VM_IP} kubectl get pods -n ${namespace} --show-all | grep ${keyword}")

        if (pods) {
                pod_list = pods.tokenize('\n')
                for (line in pod_list) {
                        pod_name = line.split(' ')[0]
                        if (pod_name) {
                                println "${pod_name} found. Getting log from the pod.."
                                log_str = getLog(pod_name, namespace)
                        }
                }
        }
}


pipeline {
        agent { node 'slave-vm' }

        options {
                timeout(time: 1000, unit: 'MINUTES')
                timestamps()
        }

        environment {
                VM_NAME = 'TACO'
                VM_IP = ''
                ERROR_RES = ''
                RES = ''
        }
        stages {
                stage('Create VM') {
                        steps {
                                script {
                                        sh """cp /var/lib/libvirt/images/backup.qcow2 /var/lib/libvirt/images/${VM_NAME}.qcow2
                                        virt-install \
                                        --channel unix,mode=bind,target_type=virtio,name=org.qemu.guest_agent.0 \
                                        --cpu host \
                                        --disk path=/var/lib/libvirt/images/${VM_NAME}.qcow2,format=qcow2,device=disk,bus=virtio,size=40 \
                                        --graphics none \
                                        --memory 16384 \
                                        --name ${VM_NAME} \
                                        --network default,model=virtio \
                                        --noautoconsole \
                                        --os-type linux \
                                        --os-variant ubuntu16.04 \
                                        --vcpus 6 \
                                        --import
                                        """
                                        VM_IP = waitingIP(VM_NAME)
                                        println ("VM_IP: ${VM_IP} success")

                                        def regHost = sh(returnStdout: true,
                                                script: "cat ~/reg_ip")
                                        sshCall("\"echo \'${regHost}\' >> /etc/hosts\"")
                                        sshCall("cat /etc/hosts")
                                }
                        }
                }

                stage('Init') {
                        steps {
                                //sshCall("echo \"nameserver 8.8.8.8\" >> /etc/resolvconf/resolv.conf.d/head && resolvconf -u")
                                sshCall("git clone https://github.com/sktelecom-oslab/taco-scripts.git")
                        }
                }

                stage('Run 010.sh _ init env') {
                        steps {
                                sshCall("apt-get install -y python-subprocess32")
                                sshCall("taco-scripts/010-init-env.sh")
                        }
                }
                stage('Run 020.sh _ install k8s') {
                        steps {
                                //sshCall("echo \"nameserver 8.8.8.8\" >> /etc/resolv.conf")
                                sshCall("taco-scripts/020-install-k8s.sh")
                        }
                }
                stage('Run 030.sh _ intall armada') {
                        steps {
                                sshCall("taco-scripts/030-install-armada.sh")
                        }
                }
                stage('Run 040.sh _ deploy openstack') {
                        steps {
                                sshCall("taco-scripts/040-deploy-openstack.sh")
                        }
                }
                stage('Run 041.sh _ deploy monitoring') {
                        steps {
                                sshCall("taco-scripts/041-deploy-mon.sh")
                        }
                }
                stage('Run 050.sh _ create os resources') {
                        steps {
                                sshCall("taco-scripts/050-create-os-resources.sh")
                        }
                }
/*
                stage('Test os') {
                        steps {
                                script{
                                        //def res = sshisError("kubectl get po --all-namespaces | grep -Ev 'Running|STATUS' || true")
                                        def res = sshisError("kubectl get po --all-namespaces | grep -v Running")
                                        if (res) {
                                                ERROR_RES = res + "\n\n" + sshisError("kubectl get po --all-namespaces | awk '\$5>0'")
                                                //error("K8s Error: some pods is wrong ")
                                        }


                                        try {
                                                sshCall("\". ~/taco-scripts/adminrc && openstack service list > os_list\"")
                                                sshCall("\". ~/taco-scripts/adminrc && openstack network list >> os_list\"")
                                                sshCall("\". ~/taco-scripts/adminrc && openstack server list >> os_list\"")
                                                sshCall("cat os_list")
                                                RES = sh(returnStdout: true,
                                                         script: "ssh root@${VM_IP} cat os_list").trim()
                                        } catch (Exception e) {
                                                sh "pwd"
                                                error("Openstack Error: ${e}")
                                        }
                                }
                                
                        }
                }
*/
                stage ('Run Rally') {
                        steps {
                                script {
                                        def cirros_ID = sh(returnStdout: true, 
                                            script: "ssh root@${VM_IP} \". ~/taco-scripts/adminrc && openstack image list | grep Cirros | awk '{print \\\$2}'\"")
                                        sshCall("\". ~/taco-scripts/adminrc && openstack image set --public ${cirros_ID}\"")

                                        sh "scp -r ~/workspace/TACO_pipe/* root@${VM_IP}:~/"
                                        sshCall("""\" cp ~/workspace/rally_values.yaml openstack-helm/rally/values.yaml 
                                                cd openstack-helm
                                                sed -i \'s/http:\\/\\/localhost:8879\\/charts/file:\\/\\/..\\/..\\/openstack-helm-infra\\/helm-toolkit/g\' rally/requirements.yaml
                                                sed -i \'s/rally-manage/rally/g\' rally/templates/bin/_manage-db.sh.tpl
                                                sed -i \'s/ceph/general/g\' ~/workspace/values-overrides.yaml
                                                helm dep up rally
                                                helm install ./rally --name=openstack-rally --namespace=openstack -f ~/workspace/values-overrides.yaml -f ~/workspace/integration.yaml
                                                \"""")
                                        
                                        sleep 10
                                        println("Waiting for rally task")

                                        def rallyStatus=false
                                        for (i=0;i <240;i++){
                                              def rallyCheck = sh(returnStdout: true,
                                                script: "ssh root@${VM_IP} kubectl get po -n openstack")
                                              println rallyCheck

                                              if (rallyCheck.contains("rally-run-task")){
                                                println("Waiting for rally task to be finished for ${i}0s..")
                                              } else {
                                                println("Rally task is completed in ${i}0s.")
                                                rallyStatus=true
                                                break;
                                              }
                                              sleep 10
                                        }

                                        collectLog('openstack', 'rally-run-task')

                                        if (rallyStatus==false) {
                                              error("Timed out waiting for rally task to finish.")
                                        }
                        
                                }
                        }
                }
        }
        post {
                always {
                    script {
                            sshCall("pwd")
                            sshCall("ls -al")

                            sh """virsh destroy ${VM_NAME} && virsh undefine ${VM_NAME}
                            rm -rf /var/lib/libvirt/images/${VM_NAME}.qcow2
                            """
                    }
                }
                
                success {
                        slackSend (
                                color: '#ACFA58',
                                message: "VM COMPLETED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
                        )
                        //slackSend (message: "```[Completed create OS VM]\n${RES}```")

                }
                failure {
                        slackSend (
                                color: '#FA5882',
                                message: "VM FAILED: job '${env.JOB_NAME} [${env.BUILD_NUMBER}]] (${env.BUILD_URL})"
                                )
                        //slackSend (message: "```[Error pods]\n${ERROR_RES}```")
                        
                }
        }
}
