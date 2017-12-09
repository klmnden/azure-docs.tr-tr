## <a name="rest"></a>REST API'lerini kullanarak dağıtma 
 
Kullanabileceğiniz [dağıtım hizmeti REST API'leri](https://github.com/projectkudu/kudu/wiki/REST-API) .zip dosyasını Azure uygulamanızı dağıtmak için. Yalnızca bir POST isteği Gönder `https://<app_name>.scm.azurewebsites.net/api/zipdeploy` ileti gövdesinde .zip dosyası içerir. Uygulamanız için dağıtım kimlik bilgileri İstek HTTP temel kimlik doğrulaması kullanılarak sağlanır. Daha fazla bilgi için bkz: [.zip itme dağıtım başvuru konusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Aşağıdaki örnek, .zip dosyasını içeren bir isteği göndermek için cURL Aracı'nı kullanır. Mac veya Linux bilgisayarda veya Bash kullanarak Windows terminal durumundan cURL çalıştırabilirsiniz. Değiştir `<zip_file_path>` proje .zip dosyanızın konumunun yolu ile yer tutucu. Ayrıca değiştirin `<app_name>` uygulamanızı benzersiz adı.

Değiştir `<deployment_user>` yer tutucusunu dağıtım kimlik bilgilerinizi kullanıcı adı ile. CURL tarafından istendiğinde parolayı yazın. Uygulamanız için dağıtım kimlik bilgileri ayarlama hakkında bilgi edinmek için bkz: [ayarlamak ve kullanıcı düzeyinde kimlik bilgilerini sıfırlama](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Bu istek karşıya yüklenen .zip dosyası itme dağıtımından tetikler. Kullanarak geçerli ve geçmiş dağıtımları gözden geçirebilirsiniz `https://<app_name>.scm.azurewebsites.net/api/deployments` aşağıdaki cURL örnekte gösterildiği gibi endpoint. Yeniden değiştirin `<app_name>` , uygulamanızın adıyla ve `<deployment_user>` dağıtım kimlik bilgilerinizi kullanıcı.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```