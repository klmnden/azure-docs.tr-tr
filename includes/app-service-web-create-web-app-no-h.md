Cloud Shell’de, [az webapp create](/cli/azure/webapp#create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../articles/app-service/app-service-web-overview.md) oluşturun. 

Aşağıdaki örnekte  *\<app_name >* bir genel benzersiz uygulama adıyla (geçerli karakterler `a-z`, `0-9`, ve `-`). 

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

Yeni oluşturulan web uygulaması'na göz atın.

```bash
http://<app_name>.azurewebsites.net
```
