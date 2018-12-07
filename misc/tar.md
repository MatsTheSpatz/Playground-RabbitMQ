# Install the .NET SDK

- Install .NET SDK for Ubuntu 16.4 (https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)

````
wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb

sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
````

- Docu of dotnet CLI Tools: https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x
  - ````dotnet new console myApp````
  - ````dotnet run myApp.csproj```` 

---