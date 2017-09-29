
[az webapp create](/cli/azure/appservice/web#create) komutuyla `myAppServicePlan` App Service planında bir uygulama oluşturun. 

Web uygulaması, API'niz için bir barındırma alanı sağlar ve dağıtılan uygulamanın görüntülenebileceği bir URL sunar.

Aşağıdaki komutta *\<app_name>* kısmını benzersiz bir adla değiştirin. `<app_name>` benzersiz değilse "Belirtilen <app_name> adına sahip web sitesi zaten var" hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<app_name>.azurewebsites.net` şeklindedir. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```