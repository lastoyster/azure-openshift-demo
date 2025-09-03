📋 Prerequisites:

An Azure subscription
Azure CLI installed and logged in
OC(OpenShift CLI) and virtctl installed locally

⚙️ Step 1: Create an ARO Cluster

Use the Azure CLI to provision an OpenShift cluster:

🔑 Step 2: Log In to OpenShift

Fetch cluster credentials and log in with the oc CLI:

az aro list-credentials --name myAROCluster --resource-group myResourceGroup

oc login https://api.<cluster_name>.<region>.aroapp.io:6443 \
  -u kubeadmin -p <password>

📦 Step 3: Enable OpenShift Virtualization

Install the OpenShift Virtualization Operator either via OperatorHub in the OpenShift Web Console or by applying the CR manifest:


🖥️ Step 4: Create a Virtual Machine
1. Define the VM Manifest
Save the following YAML as rhel9-vm.yaml:

apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: rhel9-vm
  labels:
    app: rhel9-vm
spec:
  running: false
  dataVolumeTemplates:
    - metadata:
        name: rhel9-vm-dv
      spec:
        sourceRef:
          kind: DataSource
          name: rhel9
          namespace: openshift-virtualization-os-images
        storage:
          resources:
            requests:
              storage: 30Gi
  template:
    spec:
      domain:
        cpu:
          cores: 2
        resources:
          requests:
            memory: 8Gi
        devices:
          disks:
            - name: rootdisk
              disk:
                bus: virtio
            - name: cloudinitdisk
              disk:
                bus: virtio
          interfaces:
            - name: default
              masquerade: {}
      networks:
        - name: default
          pod: {}
      volumes:
        - name: rootdisk
          dataVolume:
            name: rhel9-vm-dv
        - name: cloudinitdisk
          cloudInitNoCloud:
            userData: |
              #cloud-config
              user: cloud-user
              password: "MySecurePass123"
              chpasswd: { expire: False }

2. Deploy the VM
oc create -f rhel9-vm.yaml

3. Start the VM
virtctl start rhel9-vm

✅ Verification

Check VM status


Access VM console:
virtctl console rhel9-vm

📚 Resources:
Azure Red Hat OpenShift Documentation
OpenShift Virtualization Documentation
