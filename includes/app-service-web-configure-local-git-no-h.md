[az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git) komutuyla yerel Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın.

App Service, web uygulamasına içerik dağıtmak için FTP, yerel Git, GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yolları destekler. Bu hızlı başlangıç için yerel Git’i kullanarak dağıtım gerçekleştirirsiniz. Bu, yerel bir depodan Azure’daki bir depoya gönderim yapmak için bir Git komutunu kullanarak dağıtım yaptığınız anlamına gelir. 

Aşağıdaki komutta *\<uygulama_adı>* kısmını web uygulamanızın adıyla değiştirin.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Çıktı aşağıdaki biçimdedir:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

`<username>`, önceki adımlardan birinde oluşturduğunuz [dağıtım kullanıcısıdır](#configure-a-deployment-user).

Gösterilen URI'yi kaydedin, bir sonraki adımda kullanacaksınız. 
