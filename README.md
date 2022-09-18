# To Do #
[] Make Namespace
[] Make baseline script
[] Configure MetalLB endpoint
[] Apply Helm for each service
[] Add Volume support
[] Github pipeline to make/publish VPN image
[] Apply VPN sidecar

## Project Details ##
This repo is to standardize and simplify my media box cluster setup using traefik and helm.


# Prepare your environment

## Mac
Just use brew... it saves time:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/{YOUR USERNAME}/.zprofile                  
eval "$(/opt/homebrew/bin/brew shellenv)"                                                   
brew update        
```

To Install Tools
```
brew install --cask docker
brew install microk8s && microk8s install
brew install git
brew install helm
```

## Perpare k8s
First apply get the k8s config files
```
microk8s enable metallb:127.0.0.1-127.0.0.1
microk8s config > ~/.kube/config
```

Now that we have microk8s set to the kubectl default we can start using native helm and kubectl commands.

Next step, add traefik
```
helm repo add traefik https://helm.traefik.io/traefik
helm repo update
helm install traefik traefik/traefik
```

Optional - Dashboard config
```
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard
```

Then apply the baseline and base pages
```
kubectl apply -f systembaseline.yaml
helm upgrade --install -f BasePage.yaml basepage ./.helm
kubectl cp $PWD/lander.html lander:/usr/share/nginx/html/index.html
```

## Usage

Update the dev yaml to reflect the correct image name, ports, pull policy, and rewrite path (domain). Run the following command to deploy against an eks (after defining/declaring the variables)
```
export MYSERVICENAME=portainer
helm upgrade --install -f ./$MYSERVICENAME.yaml $MYSERVICENAME .helm
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT License](/LICENSE)