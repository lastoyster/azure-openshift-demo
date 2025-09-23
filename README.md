📋 Prerequisites:

An Azure subscription
Azure CLI installed and logged in
OC(OpenShift CLI) and virtctl installed locally

⚙️ Step 1: Create an ARO Cluster

Use the Azure CLI to provision an OpenShift cluster:

🔑 Step 2: Log In to OpenShift

Fetch cluster credentials and log in with the oc CLI:

📦 Step 3: Enable OpenShift Virtualization

Install the OpenShift Virtualization Operator either via OperatorHub in the OpenShift Web Console or by applying the CR manifest:


🖥️ Step 4: Create a Virtual Machine
1. Define the VM Manifest
Save the following YAML as rhel9-vm.yaml:

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
