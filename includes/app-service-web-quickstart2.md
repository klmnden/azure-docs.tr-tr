Web Uygulamasını oluşturmak için [az appservice web create](/cli/azure/appservice/web#create) komutunu kullanın. Aşağıdaki komutta, `<app_name>` yer tutucusunu benzersiz bir uygulama adıyla değiştirin. Web uygulamasının varsayılan DNS sitesinde `<app_name>` kullanılır. `<app_name>` benzersiz değilse "Belirtilen <app_name> adına sahip web sitesi zaten var." hata iletisi görüntülenir.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Web Uygulaması oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

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

Yeni oluşturduğunuz web uygulamasını görmek için siteye (`http://<app_name>.azurewebsites.net`) göz atın.

![app-service-web-service-created](../articles/app-service-web/media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)


## <a name="configure-local-git-deployment"></a>Yerel Git dağıtımını yapılandırma

App Service, web uygulamasına içerik dağıtmak için FTP, yerel Git, GitHub, Visual Studio Team Services ve Bitbucket gibi çeşitli yolları destekler. 

Bu hızlı başlangıç için yerel Git’i kullanarak dağıtım gerçekleştirirsiniz. Bu, yerel bir depodan Azure’daki bir depoya gönderim yapmak için bir Git komutunu kullanarak dağıtım yaptığınız anlamına gelir. 

Web uygulamasına yerel Git erişimini yapılandırmak için [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) komutunu kullanın.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Çıktı aşağıdaki biçimdedir:

```
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Gösterilen URI’yi kaydedin. Bir sonraki adımda kullanacaksınız. `<username>`, önceki adımlardan birinde oluşturduğunuz [dağıtım kullanıcısıdır](#configure-a-deployment-user).

## <a name="push-to-azure-from-git"></a>Git’ten Azure'a gönderme

Yerel Git deponuza bir Azure uzak deposu ekleyin.

```bash
git remote add azure <URI from previous step>
```

Uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısını oluştururken oluşturduğunuz parola istenir. Azure portalında oturum açarken kullandığınız parolayı değil [Dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user) adımında oluşturduğunuz parolayı girdiğinizden emin olun.

```azurecli
git push azure master
```

Önceki komut, aşağıdaki örneğe benzer bilgiler görüntüler:

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma


Dağıtılan uygulamaya gidin:

```
http://<app_name>.azurewebsites.net
```

