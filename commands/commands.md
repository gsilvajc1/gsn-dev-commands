# Useful Commands

### SpaceLift Commands
- Login Command:

  ```sh
  # the endpoint is any endpoint just using jumpclouds for now as a placeholder
  spacectl profile login --method browser --endpoint https://thejumpcloud.app.spacelift.io/ thejumpcloud
  ```

- Test doing a local preview in spacelift

  ```sh
  # the stack id can be gotten from spacelift UI
  spacectl stack local-preview --id <STACK ID>
  ```

### Shell Commands
- Show shells that are allow to be use

  ```sh
  cat /etc/shells
  ```

- Change shell for terminal

  ```sh
  # you can get the shell path from the previous command to see shells
  chsh -s <SHELL_PATH>
  ```

- See Current Shell

  ```sh
  echo $SHELL
  ```

### DevSpace
- Deploy local changes into kubernetes pod

  ```sh
  # as a note it works with a yaml file that has the config to do this
  # sample: devspace dev webui
  devspace dev <SERVICE NAME TO SYNC>
  ```

### Kubectl
- Get Secrets in a cluster

  ```sh
  kubectl get secrets
  ```

- Get Secret

  ```sh
  kubectl get secret <SECRET_NAME>
  ```

- Get Secret in nice yaml format

  ```sh
  kubectl get secret <SECRET_NAME> -o yaml
  ```

- Access specific Secret and decoded

  ```sh
  kubectl get secret <SECRET_NAME> -o jsonpath='{.data.<SECRET_KEY>}' | base64 --decode
  ```

- See Pods

  ```sh
  kubecolor get pods
  ```

- Find specific pods

  ```sh
  kubecolor get pods | grep <SERVICE_NAME>
  ```

- Elevation in kubectl, extra power to see stuff
  ```sh
  # sample => kubectl elevate gustavo.silva1@jumpcloud.com -c readonly/stg01-use2-core-00 -n si -m 'split sdk needs to monitore some of the secrets to check for a bug'

  kubectl elevate <EMAIL> -c <CONTEXT> -n <NAMESPACE> -m '<REASON>'
  ```

- Get ConfigMap (soma env variable are within configMaps)
  ```sh
  kubectl get configmap
  ```

- Get specific configmap in a nice format (yaml)
  ```sh
  # sample: kubectl get configmap webui-env-config -o yaml
  kubectl get configmap <CONFIGMAP_YOU_WANT> -o yaml
  ```

### Stern

- Check Logs for some pods

  ```sh
  # sample stern webui
  stern <SERVICE>
  ```

- Check Logs for only the pods no istio
  ```sh
  #sample stern webui -c webui
  stern <SERVICE_NAME> -c <CONTAINER_NAME>
  ```

- Logs with specific logs number

  ```sh
  stern <SERVICE_NAME> -c <CONTAINER_NAME> --tail 10
  ```

### WOK2 (this is only specific to jumpcloud)
- Update wok2

  ```sh
  brew upgrade wok2
  ```

- Start wok2
  ```sh
  # this setups wok2 in the dev environment and then starts the service
  kubectx admin/dev-usw2-p02
  kubens wok-gustavo-silva1
  wok2 up
  ```

- Purges wok2

  ```sh
  wok2 purge -n wok-gustavo-silva1
  ```

### AWS CLI
- See active configuration settings

  ```sh
  aws configure list
  ```

- Login using sso

  ```sh
  aws sso login
  ```

- AWS Elevation

  ```sh
  # sample: aws-elevate gustavo.silva1@jumpcloud.com -p secrets-management -s admin -a 330105897241 -d 2h -m "Check split Secrets in stg for webui"

  aws-elevate <EMAIL> -p secrets-management -s <ROLE> -a <ACCOUNT_NUMBER> -d <TIME> -m "<REASON>"
  ```
### Docker commands
- Copy the node_modules folder for old versions of node

  ```sh
  # need to know the image and it should be built
  # sample CONTAINER_ID=$(docker create jumpcloud/si-builder:development)
  CONTAINER_ID=$(docker create <IMAGE_NAME:IMAGE_TAG>) && \
  docker cp $CONTAINER_ID:/opt/jumpcloud/si/node_modules . && \
  docker rm $CONTAINER_ID
  ```
