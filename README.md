# helm
It seems like you want to set up Helm and Tiller, create Helm charts, and organize your Helm chart project on a Kubernetes cluster. Below are the steps and commands to accomplish these tasks:

**On the Master Instance:**

1. Install Helm 3:

   ```bash
   curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
   chmod 700 get_helm.sh
   ./get_helm.sh
   ```

   This installs Helm 3, which doesn't require Tiller.

2. Create a Kubernetes service account for Tiller:

   ```bash
   kubectl -n kube-system create serviceaccount tiller
   ```

3. Bind the `cluster-admin` role to the Tiller service account:

   ```bash
   kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
   ```

4. Verify that the Tiller service account is created:

   ```bash
   kubectl get serviceaccount -n kube-system | grep -i tiller
   ```

5. Copy the Kubernetes configuration file (`admin.conf`) to your Linux slave machine:

   ```bash
   scp /etc/kubernetes/admin.conf <linux_slave_username>@<linux_slave_ip>:~/.kube/config
   ```

   Replace `<linux_slave_username>` and `<linux_slave_ip>` with your Linux slave's username and IP address.

**On the Linux Slave:**

1. Create the `.kube` directory and edit the `config` file:

   ```bash
   mkdir ~/.kube
   vi ~/.kube/config
   ```

   Copy the content from the `admin.conf` file you copied from the master.

2. Initialize Helm with the Tiller service account:

   ```bash
   helm init --service-account tiller
   ```

3. Check the Helm repositories:

   ```bash
   helm repo list
   ```

4. Create a directory for your Helm charts (e.g., `mycharts`) and navigate into it:

   ```bash
   mkdir mycharts
   cd mycharts
   ```

5. Create a Helm chart (e.g., `abcd`) using the Helm CLI:

   ```bash
   helm create abcd
   ```

6. You can then edit the Helm chart in the `abcd` directory, including the `README.md` file for GitHub documentation.

   ```bash
   cd abcd
   vi README.md
   ```

7. You can also create additional Kubernetes manifest files like `deployment.yaml`, `service.yaml`, `secrets.yaml`, `configmaps.yaml`, and any others required for your application in the `templates` directory.

8. Organize your Helm chart project, and when you're ready, you can package and deploy your Helm chart to your Kubernetes cluster.

   ```bash
   helm package .
   helm install <release_name> <chart_name>-<chart_version>.tgz
   ```

Remember to replace `<release_name>` and `<chart_name>-<chart_version>` with your desired release name and the name/version of your Helm chart package.
