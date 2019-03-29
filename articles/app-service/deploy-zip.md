---
title: Kod bir ZIP veya WAR dosyası ile - Azure App Service'e dağıtma | Microsoft Docs
description: Uygulamanızı bir ZIP dosyası (veya Java geliştiricileri için bir WAR dosyası) ile Azure App Service'e dağıtma konusunda bilgi edinin.
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
ms.custom: seodec18
ms.openlocfilehash: ef313ea631a963aa7893bf15e826e591c9d9cfc3
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619807"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Uygulamanızı bir ZIP veya WAR dosyası ile Azure App Service'e dağıtma

Bu makalede bir ZIP dosyası veya WAR dosyasını web uygulamanıza dağıtmak için nasıl kullanılacağını gösteren [Azure App Service](overview.md). 

Bu ZIP dosyası dağıtım aynı Kudu hizmet bu powers sürekli tümleştirme tabanlı dağıtımlar kullanır. Kudu ZIP dosyası dağıtım için aşağıdaki işlevi destekler: 

- Önceki bir dağıtımın kalan dosyaları silme işlemi.
- Paket geri yükleme içeren varsayılan yapı işlemi üzerinde etkinleştirmek için seçenek.
- [Dağıtım özelleştirme](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings)de dahil olmak üzere dağıtım betikleri çalıştırma.  
- Dağıtım günlükleri. 
- 512 MB'lık dosya boyutu sınırı.

Daha fazla bilgi için [Kudu belgeleri](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

WAR dosyası dağıtım dağıtır, [WAR](https://wikipedia.org/wiki/WAR_(file_format)) Java web uygulamanızı çalıştırmak için App Service'e dosya. Bkz: [dağıtma WAR dosyasını](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için:

* [Bir App Service uygulaması oluşturun](/azure/app-service/) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.

## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları bir ZIP dosyasında indirdiyseniz, dosyaları ilk ayıklayın. Örneğin, Github'dan bir ZIP dosyası yüklediyseniz, bu dosya olarak dağıtamazsınız-olduğu. GitHub, App Service ile çalışmıyor ek iç içe geçmiş dizinleri ekler. 
>

Yerel terminal penceresinde, uygulama projesinin kök dizinine gidin. 

Bu dizin web uygulamanıza girişi dosya da içermelidir _index.html_, _index.php_, ve _app.js_. Paket Yönetimi dosyaları gibi içerebilir _project.json_, _composer.json_, _package.json_, _bower.json_ve _requirements.txt_.

Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>ZIP dosyası Azure CLI ile dağıtma

Sonraki veya, Azure CLI Sürüm 2.0.21 olduğundan emin olun. Hangi sürümü varsa çalıştırdığınız görmek için `az --version` terminal pencerenizde komutu.

Kullanarak yüklenen ZIP dosyasını web uygulamanıza dağıtın [az webapp deployment kaynak config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutu.  

Aşağıdaki örnek, karşıya yüklenen ZIP dosyasını dağıtır. Azure CLI'ın yerel bir yüklemesi kullanırken, yerel, ZIP dosyasının yolunu belirtin `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Bu komut ZIP içindeki dosyaları ve dizinleri App Service uygulama klasörünüze (`\home\site\wwwroot`) dağıtır ve uygulamayı yeniden başlatır. Ek özel derleme işlemi yapılandırılmışsa bu işlemler de çalışır. Daha fazla bilgi için [Kudu belgeleri](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR dosyasını dağıtma

App Service'e bir WAR dosyasını dağıtmak için https://<app_name>.scm.azurewebsites.net/api/wardeploy bir POST isteği gönderin. POST isteğinin ileti gövdesinde .war dosyası bulunmalıdır. Uygulamanızın dağıtım kimlik bilgileri, HTTP BASIC kimlik doğrulaması kullanılarak istekte belirtilir. 

HTTP temel kimlik doğrulaması için App Service dağıtım kimlik bilgileriniz gerekir. Dağıtım kimlik bilgilerinizi ayarlamak hakkında bilgi için bkz. [ayarlayın ve kullanıcı düzeyi kimlik bilgilerini sıfırlama](deploy-configure-credentials.md#userscope).

### <a name="with-curl"></a>CURL ile

Aşağıdaki örnek, .war dosyasını dağıtmak için cURL aracını kullanır. Yer tutucuları değiştirmeniz `<username>`, `<war_file_path>`, ve `<app_name>`. CURL tarafından istendiğinde parolayı yazın.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnekte [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) .war dosyasını içeren bir istek gönderebilirsiniz. Yer tutucuları değiştirmeniz `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, ve `<app_name>`.

```powershell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](deploy-local-git.md). Git tabanlı azure'a dağıtım, sürüm denetimi, paket geri yükleme, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Diğer kaynaklar

* [Kudu: Zip dosyasından dağıtma](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Azure App Service'e dağıtım kimlik bilgileri](deploy-ftp.md)
