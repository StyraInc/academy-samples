# Kubernetes Local Validation using Styra

## Directions
### Lab Preparation
1. Clone the required repository:
    ```
    git clone https://github.com/StyraInc/academy-samples.git
    cd academy-samples/kubernetes/repo
    ```

### Administrator setting up the manifests repo with validation configs
1. Create the file `.styra.yaml` with the following content:
    ```yaml
    checks:
      dev_deployments:
        system_id: e9136611bd7842cf92d1e508c15e78c4
        files:
            - k8s/deployments/*.yaml
    ```
2. Commit change to the repo.
    ```
    git add .styra.yaml
    git commit -m "Styra config to check deployment YAMLs"
    ```
3. (Pretend for training purpose) Merge the change into remote so that this configuration will be checked out with the repo.
4. (Trainer) Add a validating policy to Styra workspace.
5. (Trainer) Create API token for local validation.

### Developer adding manifests to the repo
#### Set-up local toolchain for validation
1. Download the styra CLI from the docs page: [https://docs.styra.com/das/reference/cli/install-use-cli](https://docs.styra.com/das/reference/cli/install-use-cli)
2. Confirm the CLI is working:
    ```
    styra version
    ```
3. Configure the connection to Styra workpsace by creating this file in your user directory `~/.styra/config`:
    ```yaml
    organization_id: edu-das.styra.com
    token: <secret token to be provided by trainer>
    url: https://edu-das.styra.com
    ```
#### Create and validate manifest
1. Create a resource manifest `k8s/deployments/nginx.yaml` to add an nginx ingress deployment. View the file that is already created for you.
   ```cat k8s/deployments/nginx.yaml```
2. Locally validate new manifest before submission for review.
    ```bash
    styra validate check-local --repo-path . --output out.json
    cat out.json
    ```

#### Update and validate manifest
1. Add a readiness probe to the new deployment to pass validation. Uncomment the following section in `k8s/deployments/nginx.yaml`:
    ```yaml
    # readinessProbe:
    #   httpGet:
    #     path: /nginx-ready
    #     port: readiness-port
    #   periodSeconds: 1
    ```

2. Validate the updated manifest.
    ```bash
    styra validate check-local --repo-path . --output out.json
    cat out.json
    ```

3. (Pretend for training purpose) Submit the manifest for review.