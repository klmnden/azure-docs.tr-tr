[az webapp create](/cli/azure/webapp#create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../articles/app-service-web/app-service-web-overview.md) oluşturun. 

Web uygulaması, kodunuz için bir barındırma alanı ve dağıtılan uygulamayı görüntülemek için bir URL sağlar.

Aşağıdaki komutta *\<uygulama_adı>* kısmını benzersiz bir adla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). `<app_name>` benzersiz değilse "Belirtilen <app_name> adına sahip web sitesi zaten var" hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<app_name>.azurewebsites.net` şeklindedir. 

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

Yeni oluşturduğunuz web uygulamasını görmek için siteye göz atın.

```bash
http://<app_name>.azurewebsites.net
```
