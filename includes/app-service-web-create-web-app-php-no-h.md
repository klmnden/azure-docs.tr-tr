Bulut Kabuğu'nda bir web uygulaması oluşturmak `myAppServicePlan` uygulama hizmeti planıyla [az webapp oluşturmak](/cli/azure/webapp#create) komutu. Değiştirmeyi unutmayın `<app_name>` benzersiz bir uygulama adına sahip.

Aşağıdaki komutta çalışma kümesine `PHP|7.0`. Desteklenen tüm çalışma zamanları görmek için çalıştırın [az webapp listesi-çalışma zamanları](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0" --deployment-local-git
```

Web uygulaması oluşturduğunuzda Azure CLI çıktı aşağıdaki örneğe benzer şekilde gösterir:

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

Etkin git dağıtımı ile bir boş yeni web uygulaması oluşturduğunuzu düşünün.

> [!NOTE]
> Git uzak URL'sini gösterilen `deploymentLocalGitUrl` özelliğiyle biçimi `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Bu URL, daha sonra ihtiyacınız olacak şekilde kaydedin.
>
