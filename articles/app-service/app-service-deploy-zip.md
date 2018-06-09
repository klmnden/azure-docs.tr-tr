---
title: Uygulamanızı Azure App Service'e bir ZIP ya da WAR dosyası dağıtma | Microsoft Docs
description: ZIP dosyası (veya bir WAR dosyası Java geliştiricilerinin) ile uygulamanızı Azure App Service'e dağıtma öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.openlocfilehash: a3178d5cb09087a243a51e20567895d03ce1f7fb
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234143"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Uygulamanızı Azure App Service'e bir ZIP ya da WAR dosyası dağıtma

Bu makalede, bir ZIP dosyası veya WAR dosyası web uygulamanıza dağıtmak için nasıl kullanılacağı gösterilmektedir [Azure App Service](app-service-web-overview.md). 

Bu ZIP dosyası dağıtım o powers sürekli tümleştirme tabanlı dağıtımlar aynı Kudu hizmeti kullanır. Kudu ZIP dosyası dağıtım için aşağıdaki işlevi destekler: 

- Dosyaların silinmesini önceki bir dağıtımın kalan.
- Paket geri yüklemesi içerir varsayılan derleme işlemini üzerinde bırakma seçeneği.
- [Dağıtım özelleştirme](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings)gibi dağıtım betikleri çalıştırma.  
- Dağıtım günlükleri. 

Daha fazla bilgi için bkz: [Kudu belgelerine](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

WAR dosyası dağıtım dağıtır, [WAR](https://wikipedia.org/wiki/WAR_(file_format)) Java web uygulamanızı çalıştırmak için uygulama hizmeti dosyasına. Bkz: [dağıtmak WAR dosyasını](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için:

* [Bir App Service uygulaması oluşturun](/azure/app-service/) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.

## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları ZIP dosyasında yüklediyseniz, ilk dosyaları ayıklayın. Örneğin, bir ZIP dosyası Github'dan yüklediyseniz, bu dosya olarak dağıtamazsınız-değil. GitHub ile uygulama hizmeti çalışmıyor ek iç içe geçmiş dizinler ekler. 
>

Yerel bir terminal penceresi içinde uygulaması projenize kök dizinine gidin. 

Bu dizin web uygulamanız için giriş dosyası gibi içermelidir _index.html_, _php için index.php'dir_, ve _app.js_. Paket Yönetimi dosyaları gibi içerebilir _project.json_, _composer.json_, _package.json_, _bower.json_ve _requirements.txt_.

Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>ZIP dosyasının Azure CLI ile dağıtma

Sonraki veya Azure CLI Sürüm 2.0.21 olduğundan emin olun. Varsa çalıştırdığınız hangi sürümü görmek için `az --version` terminal pencerenizde komutu.

Karşıya yüklenen ZIP dosyasını kullanarak web uygulamanıza dağıtmak [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) komutu.  

Aşağıdaki örnek, yüklediğiniz ZIP dosyası dağıtır. Azure CLI yerel yüklemesi kullanırken, yerel ZIP dosyası yolunu belirtin `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Bu komut ZIP içindeki dosyaları ve dizinleri App Service uygulama klasörünüze (`\home\site\wwwroot`) dağıtır ve uygulamayı yeniden başlatır. Ek özel derleme işlemi yapılandırılmışsa bu işlemler de çalışır. Daha fazla bilgi için bkz: [Kudu belgelerine](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR dosyasını dağıtın

Bir WAR dosyası App Service'e dağıtma için https://<app_name>.scm.azurewebsites.net/api/wardeploy bir POST isteği gönderin. POST isteğinin ileti gövdesinde .war dosyası bulunmalıdır. Uygulamanızın dağıtım kimlik bilgileri, HTTP BASIC kimlik doğrulaması kullanılarak istekte belirtilir. 

HTTP temel kimlik doğrulaması için uygulama hizmeti dağıtım kimlik bilgilerinizi gerekir. Dağıtım kimlik bilgilerinizi ayarlamak hakkında bilgi için bkz: [ayarlamak ve kullanıcı düzeyinde kimlik bilgilerini sıfırlama](app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>CURL ile

Aşağıdaki örnek, .war dosya dağıtmak için cURL Aracı'nı kullanır. Yer tutucuları değiştirmek `<username>`, `<war_file_path>`, ve `<app_name>`. CURL tarafından istendiğinde parolayı yazın.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnek kullanır [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) .war dosyasını içeren bir istek göndermek için. Yer tutucuları değiştirmek `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, ve `<app_name>`.

```PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Azure Git tabanlı dağıtımına sürüm denetimi, paket geri yüklemesi, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Diğer kaynaklar

* [Kudu: zip dosyasından dağıtma](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deploy-ftp.md)
