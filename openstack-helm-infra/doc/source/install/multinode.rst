====================
Multinode Deployment
====================

Deploy Local Docker Registry
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: ../../../tools/deployment/multinode/010-deploy-docker-registry.sh
    :language: shell
    :lines: 1,17-

Alternatively, this step can be performed by running the script directly:

.. code-block:: shell

  ./tools/deployment/multinode/010-deploy-docker-registry.sh

  Deploy Cluster and Namespace Ingress Controllers
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/020-ingress.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/020-ingress.sh

  Deploy NFS Provisioner for LMA Services
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/030-lma-nfs-provisioner.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/030-lma-nfs-provisioner.sh

  Deploy LDAP
  ^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/040-ldap.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/040-ldap.sh

  Deploy Prometheus
  ^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/050-prometheus.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/050-prometheus.sh

  Deploy Alertmanager
  ^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/060-alertmanager.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/060-alertmanager.sh

  Deploy Kube-State-Metrics
  ^^^^^^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/070-kube-state-metrics.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/070-kube-state-metrics.sh

  Deploy Node Exporter
  ^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/080-node-exporter.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/080-node-exporter.sh

  Deploy OpenStack Exporter
  ^^^^^^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/090-openstack-exporter.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/090-openstack-exporter.sh

  Deploy Grafana
  ^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/100-grafana.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/100-grafana.sh

  Deploy Nagios
  ^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/110-nagios.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/110-nagios.sh

  Deploy Elasticsearch
  ^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/120-elasticsearch.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/120-elasticsearch.sh

  Deploy Fluent-Logging
  ^^^^^^^^^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/130-fluent-logging.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/130-fluent-logging.sh

  Deploy Kibana
  ^^^^^^^^^^^^^

  .. literalinclude:: ../../../tools/deployment/multinode/140-kibana.sh
      :language: shell
      :lines: 1,17-

  Alternatively, this step can be performed by running the script directly:

  .. code-block:: shell

    ./tools/deployment/multinode/140-kibana.sh
