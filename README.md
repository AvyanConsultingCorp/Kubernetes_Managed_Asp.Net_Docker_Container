# Kubernetes_Managed_Asp.Net_Docker_Container

# install docker  
1.  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
2.  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
3. sudo apt-get update
5. apt-get install -y docker-ce
6. systemctl status docker
# install .net core
19. curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
20. mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
21. sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/
apt/sources.list.d/dotnetdev.list'
22. sudo apt-get update
23. sudo apt-get install dotnet-sdk-2.0.0
# Modify and sample app
26. cd dotnet-docker-samples/aspnetapp/wwwroot/Controllers/
37. vi HomeController.cs
# build a container
24. usermod -a -G docker rlazarev
42. docker build -t aspnetapp .
# Install Azure Cli
51. echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sourc
es.list.d/azure-cli.list
52. sudo apt-key adv --keyserver packages.microsoft.com --recv-keys 417A0893
53. sudo apt-get install apt-transport-https
54. sudo apt-get update && sudo apt-get install azure-cli
## login Azure cli
57. az login
58. az account list
59. az account set --subscription "0312c7cb-e240-45dd-b57c-a9d9beb45741"
# Login to Azure container Registry (created in Azure Porta)
60. az acr list
61. az acr login --name "cloudneeti"
63. az acr show --name "cloudneeti" --query loginServer --output table
# Tag our code container & push container to ACR
64. docker tag aci-tutorial-app cloudneeti.azurecr.io/aspnetapp:v1
65. docker tag aspnetapp cloudneeti.azurecr.io/aspnetapp:v1
66. docker images
67. docker push cloudneeti.azurecr.io/aspnetapp:v1
# verify container push successful
68. az acr repository list --name "cloudneeti" --output table
69. az acr repository show-tags --name "cloudneeti" --repository aspnetapp --output table
71. az acr show --name "cloudneeti" --query loginServer
72. az acr credential show --name "cloudneeti" --query "passwords[0].value"
# Create aci
74. az container create --name aspnetapp-00 --image cloudneeti.azurecr.io/aspnetapp:v1 --cpu 1 --memory 1 --registry-p
assword "dFmRCsAI/vVWhx6l8VOh0YgZItbWOs5V" --ip-address public -g cloudneeti-aci
75. az container create --name aspnetapp-01 --image cloudneeti.azurecr.io/aspnetapp:v1 --cpu 1 --memory 1 --registry-p
assword "dFmRCsAI/vVWhx6l8VOh0YgZItbWOs5V" --ip-address public -g cloudneeti-aci
