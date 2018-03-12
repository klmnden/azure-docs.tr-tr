## <a name="rest"></a>ZIP dosyası REST API'leri ile dağıtma 

Kullanabileceğiniz [dağıtım hizmeti REST API'leri](https://github.com/projectkudu/kudu/wiki/REST-API) .zip dosyasını Azure uygulamanızı dağıtmak için. Dağıtmak için https://<app_name>.scm.azurewebsites.net/api/zipdeploy bir POST isteği gönderin. POST isteğini ileti gövdesinde .zip dosyası içermelidir. Uygulamanız için dağıtım kimlik bilgileri İstek HTTP temel kimlik doğrulaması kullanılarak sağlanır. Daha fazla bilgi için bkz: [.zip itme dağıtım başvurusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

HTTP temel kimlik doğrulaması için uygulama hizmeti dağıtım kimlik bilgilerinizi gerekir. Dağıtım kimlik bilgilerinizi ayarlamak hakkında bilgi için bkz: [ayarlamak ve kullanıcı düzeyinde kimlik bilgilerini sıfırlama](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>CURL ile

Aşağıdaki örnek, bir .zip dosyası dağıtmak için cURL Aracı'nı kullanır. Yer tutucuları değiştirmek `<username>`, `<password>`, `<zip_file_path>`, ve `<app_name>`. CURL tarafından istendiğinde parolayı yazın.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Bu istek karşıya yüklenen .zip dosyası itme dağıtımından tetikler. Geçerli ve geçmiş dağıtımları https://<app_name>.scm.azurewebsites.net/api/deployments endpoint kullanarak aşağıdaki cURL örnekte gösterildiği gibi gözden geçirebilirsiniz. Yeniden Değiştir `<app_name>` , uygulamanızın adıyla ve `<deployment_user>` dağıtım kimlik bilgilerinizi kullanıcı.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnek kullanır [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) .zip dosyasını içeren bir isteği göndermek için. Yer tutucuları değiştirmek `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, ve `<app_name>`.

```PowerShell
#PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<zip_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/zipdeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

Bu istek karşıya yüklenen .zip dosyası itme dağıtımından tetikler. Geçerli ve geçmiş dağıtımları gözden geçirmek için aşağıdaki komutları çalıştırın. Yeniden Değiştir `<app_name>` yer tutucu.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
