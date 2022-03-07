# Terraform

Any Kubernetes code should be placed in their own respective folders under this folder.

Example:

```
.
└── exchange-21c-centos/
    ├── Terraform/
    │   └── ANewFeature/
    │       ├── Dev/
    │       │   └── code.tf
    │       ├── Production/
    │       │   └── code.tf
    │       └── modules/
    │           └── ANewFeatureModule/
    │               └── code.tf
    ├── Kubernetes/
    │   └── NewKubernetesFeature/
    │       ├── manifest.yaml
    │       └── manifest2.yaml
    ├── api/
    └── web/
```