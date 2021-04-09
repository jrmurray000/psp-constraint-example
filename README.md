#Example Conversion of SCC Policy to Gatekeeper PSP Constraints

Converts the following 'restricted' SCC to Gatekeeper PSP constraints.

See "restricted_constraints.yaml" for the equivalent Gatekeeper constraints, which use ConstraintTemplates available in the Gatekeeper project library.

```
apiVersion: v1
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: restricted denies access to all host features and requires
      pods to be run with a UID, and SELinux context that are allocated to the namespace.  This
      is the most restrictive SCC.
  creationTimestamp: null
  name: restricted
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegedContainer: false
allowedCapabilities: null
allowedFlexVolumes: null
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
priority: null
readOnlyRootFilesystem: false
requiredDropCapabilities:
- KILL
- MKNOD
- SYS_CHROOT
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
- configMap
- downwardAPI
- emptyDir
- nfs
- persistentVolumeClaim
- projected
- secret
users: []
groups:
- system:authenticated
```

Namespaces with the label "gke.io/podsecurity":"exempt" are excluded from these Constraints

Constraints exclude these settings:
* allowHostDirVolumePlugin - hostpath is not an allowed volume type, so this is redundant
*  defaultAddCapabilities - null, no rule required
*  allowedFlexVolumes - null, no rule required
*  defaultAddCapabilities - null, no rule required
*  priority - null, also proiority is unsupported
*  readOnlyRootFilesystem - false, no rule required
*  seLinuxContext - SCC looks up the value from the namespace annotation. not supported see: --- https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/authentication/managing-pod-security-policiessecurity-context-constraints-pre-allocated-values_configuring-internal-oauth
*  Values set by namespace level annotation
*  supplementalgroups - RunAsAny, no rule required
