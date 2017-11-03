Cloud Shell’de, [az webapp create](/cli/azure/webapp#create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../articles/app-service/app-service-web-overview.md) oluşturun. 

Web uygulaması, kodunuz için bir barındırma alanı ve dağıtılan uygulamayı görüntülemek için bir URL sağlar.

Aşağıdaki komutta *\<uygulama_adı>* kısmını benzersiz bir adla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). `<app_name>` benzersiz değilse "Belirtilen <app_name> adına sahip web sitesi zaten var" hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<app_name>.azurewebsites.net` şeklindedir. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Etkin git dağıtımı ile bir boş web uygulaması oluşturduğunuzu düşünün.

> [!NOTE]
> Git uzak URL'sini gösterilen `deploymentLocalGitUrl` özelliğiyle biçimi `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Bu URL, daha sonra ihtiyacınız olacak şekilde kaydedin.
>

Yeni oluşturduğunuz web uygulamasını görmek için siteye göz atın.

```bash
http://<app_name>.azurewebsites.net
```
