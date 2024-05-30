# ODF default resource consumption

We can see the ODF operator's default resource consumption in the source code on [GitHub](https://github.com/red-hat-storage/ocs-operator/blob/bfb00a97a999ebb7446ff58da1db88a2c105dfa6/controllers/defaults/resources.go)

## ODF Internal

### ODF 4.15

Since ODF 4.15, different performance profiles have been provided to enhance performance in the ODF cluster. Further information can be found in the [Red Hat official documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_data_foundation/4.15/html-single/planning_your_deployment/index#resource-requirements-for-performance-profiles_rhodf)

#### Lean

#### Balanced

#### Performance

## ODF External

### ODF 4.15

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |          **CPU**          |          **Memory**         |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:-------------------------:|:---------------------------:|
|   Operators   |           ocs-operator          |   deployment   |             1             |    Request: - Limit: -    |     Request: - Limit: -     |
|   Operators   |           odf-console           |   deployment   |             1             | Request: 100m Limit: 100m | Request: 512Mi Limit: 512Mi |
|   Operators   | odf-operator-controller-manager |   deployment   |             1             | Request: 200m Limit: 200m | Request: 200Mi Limit: 300Mi |
|   Operators   |        rook-ceph-operator       |   deployment   |             1             |    Request: - Limit: -    |     Request: - Limit: -     |
|   Operators   |         noobaa-operator         |   deployment   |             1             |   Request: - Limit: 250m  |   Request: - Limit: 512Mi   |
|    Helpers    |        ux-backend-server        |   deployment   |             1             |    Request: - Limit: -    |     Request: - Limit: -     |
|    Helpers    |       ocs-metrics-exporter      |   deployment   |             1             |    Request: - Limit: -    |     Request: - Limit: -     |
|      CSI      |          csi-rbdplugin          |    daemonset   |  As many as worker nodes  |    Request: - Limit: -    |     Request: - Limit: -     |
|      CSI      |    csi-rbdplugin-provisioner    |   deployment   |             2             |    Request: - Limit: -    |     Request: - Limit: -     |
|      CSI      |  csi-addons-controller-manager  |   deployment   |             1             |   Request: 10m Limit: 1   |  Request: 64Mi Limit: 512Mi |
|     NooBaa    |         noobaa-endpoint         |       HPA      |      Min: 1 - Max: 2      | Request: 999m Limit: 999m |   Request: 2Gi Limit: 2Gi   |
|     NooBaa    |           noobaa-core           |   statefulset  |             1             | Request: 999m Limit: 999m |   Request: 4Gi Limit: 4Gi   |
|     NooBaa    |           noobaa-db-pg          |   statefulset  |             1             | Request: 500m Limit: 500m |   Request: 4Gi Limit: 4Gi   |
|     NooBaa    |       noobaa-backing-store      |       pod      | As many as backing stores | Request: 100m Limit: 100m | Request: 400Mi Limit: 400Mi |
