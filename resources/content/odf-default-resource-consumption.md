# ODF default resource consumption

We can see the ODF operator's default resource consumption in the source code on [GitHub](https://github.com/red-hat-storage/ocs-operator/blob/bfb00a97a999ebb7446ff58da1db88a2c105dfa6/controllers/defaults/resources.go). In the tables described in this document, `R` refers to `requests` and `L` refers to `limits`. For further information about resource management in Kubernetes, please refer to the [upstream documentation](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

## ODF Internal

### ODF 4.15

Since ODF 4.15, different performance profiles have been provided to enhance performance in the ODF cluster. Further information can be found in the [Red Hat official documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_data_foundation/4.15/html-single/planning_your_deployment/index#resource-requirements-for-performance-profiles_rhodf).

#### Core

These are the core pods we have when running ODF in internal mode and their resource consumption:

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|   Operators   |           ocs-operator          |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|   Operators   |           odf-console           |   deployment   |             1             | R:100m / L:100m | R:512Mi / L:512Mi |
|   Operators   | odf-operator-controller-manager |   deployment   |             1             | R:200m / L:200m | R:200Mi / L:300Mi |
|   Operators   |        rook-ceph-operator       |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|   Operators   |         noobaa-operator         |   deployment   |             1             |   R:- / L:250m  |   R:- / L:512Mi   |
|    Helpers    |        ux-backend-server        |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|    Helpers    |       ocs-metrics-exporter      |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|      CSI      |          csi-rbdplugin          |    daemonset   |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|      CSI      |    csi-rbdplugin-provisioner    |   deployment   |             2             |    R:- / L:-    |     R:- / L:-     |
|      CSI      |        csi-cephfsplugin         |    daemonset   |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|      CSI      |  csi-cephfsplugin-provisioner   |   deployment   |             2             |    R:- / L:-    |     R:- / L:-     |
|      CSI      |  csi-addons-controller-manager  |   deployment   |             1             |   R:10m / L:1   |  R:64Mi / L:512Mi |
|    NooBaa     |         noobaa-endpoint         |       HPA      |      Min: 1 - Max: 2      | R:999m / L:999m |   R:2Gi / L:2Gi   |
|    NooBaa     |           noobaa-core           |   statefulset  |             1             | R:999m / L:999m |   R:4Gi / L:4Gi   |
|    NooBaa     |           noobaa-db-pg          |   statefulset  |             1             | R:500m / L:500m |   R:4Gi / L:4Gi   |
|    NooBaa     |       noobaa-backing-store      |       pod      | As many as backing stores | R:100m / L:100m | R:400Mi / L:400Mi |

#### Additional ODF features

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|      NFS      |                nfs              |   deployment   |             1             |    R:3 / L:3    |   R:8Gi / L:8Gi   |
|      CSI      |          csi-nfsplugin          |    daemonset   |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|      CSI      |    csi-nfsplugin-provisioner    |   deployment   |             2             |    R:- / L:-    |     R:- / L:-     |
|      Ceph     |            rbd-mirror           |   deployment   |             1             |    R:1 / L:1    |   R:2Gi / L:2Gi   |
|     Tools     |         rook-ceph-tools         |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |

#### Lean

These are the core pods we have when running ODF in internal mode using the `lean` performance profile and their resource consumption:

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|     Ceph      |          rook-ceph-mgr          |   deployment   |             2             |   R:0.5 / L:1   |   R:1Gi / L:2Gi   |
|     Ceph      |          rook-ceph-mon          |   deployment   |             3             |  R:0.5 / L:0.5  |   R:1Gi / L:1Gi   |
|     Ceph      |          rook-ceph-osd          |   deployment   |    As many as devices     |  R:1.5 / L:1.5  |   R:3Gi / L:3Gi   |
|     Ceph      |          rook-ceph-mds          |   deployment   |             2             |    R:1 / L:1    |   R:2Gi / L:2Gi   |
|     Ceph      |          rook-ceph-rgw          |   deployment   |             1             |    R:1 / L:1    |   R:1Gi / L:1Gi   |
|    Crash      |    rook-ceph-crashcollector     |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|   Exporter    |       rook-ceph-exporter        |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |

#### Balanced

These are the core pods we have when running ODF in internal mode using the `balanced` performance profile and their resource consumption:

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|     Ceph      |          rook-ceph-mgr          |   deployment   |             2             |    R:1 / L:2    |  R:1.5Gi / L:3Gi  |
|     Ceph      |          rook-ceph-mon          |   deployment   |             3             |    R:1 / L:1    |   R:2Gi / L:2Gi   |
|     Ceph      |          rook-ceph-osd          |   deployment   |    As many as devices     |    R:2 / L:2    |   R:5Gi / L:5Gi   |
|     Ceph      |          rook-ceph-mds          |   deployment   |             2             |    R:2 / L:2    |   R:6Gi / L:6Gi   |
|     Ceph      |          rook-ceph-rgw          |   deployment   |             1             |    R:2 / L:2    |   R:2Gi / L:2Gi   |
|    Crash      |    rook-ceph-crashcollector     |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|   Exporter    |       rook-ceph-exporter        |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |

#### Performance

These are the core pods we have when running ODF in internal mode using the `performance` performance profile and their resource consumption:

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|     Ceph      |          rook-ceph-mgr          |   deployment   |             2             |   R:1.5 / L:3   |   R:2Gi / L:4Gi   |
|     Ceph      |          rook-ceph-mon          |   deployment   |             3             |  R:1.5 / L:1.5  |   R:3Gi / L:3Gi   |
|     Ceph      |          rook-ceph-osd          |   deployment   |    As many as devices     |    R:4 / L:4    |   R:8Gi / L:8Gi   |
|     Ceph      |          rook-ceph-mds          |   deployment   |             2             |    R:3 / L:3    |   R:8Gi / L:8Gi   |
|     Ceph      |          rook-ceph-rgw          |   deployment   |             1             |    R:2 / L:2    |   R:4Gi / L:4Gi   |
|    Crash      |    rook-ceph-crashcollector     |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|   Exporter    |       rook-ceph-exporter        |   daemonset    |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |

## ODF External

### ODF 4.15

These are the core pods we have when running ODF in external mode and their resource consumption:

| **Component** |             **Name**            | **OCP object** |     **Number of pods**    |     **CPU**     |     **Memory**    |
|:-------------:|:-------------------------------:|:--------------:|:-------------------------:|:---------------:|:-----------------:|
|   Operators   |           ocs-operator          |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|   Operators   |           odf-console           |   deployment   |             1             | R:100m / L:100m | R:512Mi / L:512Mi |
|   Operators   | odf-operator-controller-manager |   deployment   |             1             | R:200m / L:200m | R:200Mi / L:300Mi |
|   Operators   |        rook-ceph-operator       |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|   Operators   |         noobaa-operator         |   deployment   |             1             |   R:- / L:250m  |   R:- / L:512Mi   |
|    Helpers    |        ux-backend-server        |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|    Helpers    |       ocs-metrics-exporter      |   deployment   |             1             |    R:- / L:-    |     R:- / L:-     |
|      CSI      |          csi-rbdplugin          |    daemonset   |  As many as worker nodes  |    R:- / L:-    |     R:- / L:-     |
|      CSI      |    csi-rbdplugin-provisioner    |   deployment   |             2             |    R:- / L:-    |     R:- / L:-     |
|      CSI      |  csi-addons-controller-manager  |   deployment   |             1             |   R:10m / L:1   |  R:64Mi / L:512Mi |
|     NooBaa    |         noobaa-endpoint         |       HPA      |      Min: 1 - Max: 2      | R:999m / L:999m |   R:2Gi / L:2Gi   |
|     NooBaa    |           noobaa-core           |   statefulset  |             1             | R:999m / L:999m |   R:4Gi / L:4Gi   |
|     NooBaa    |           noobaa-db-pg          |   statefulset  |             1             | R:500m / L:500m |   R:4Gi / L:4Gi   |
|     NooBaa    |       noobaa-backing-store      |       pod      | As many as backing stores | R:100m / L:100m | R:400Mi / L:400Mi |
