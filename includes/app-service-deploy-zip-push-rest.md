---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: ed771d1817e7ee1161e0f94ec9eefb5d2dfb3e23
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50737261"
---
## <a name="rest"></a>ZIP dosyası REST API'leri ile dağıtma 

Kullanabileceğiniz [dağıtım hizmeti REST API'leri](https://github.com/projectkudu/kudu/wiki/REST-API) .zip dosyası, uygulamanızı Azure'a dağıtmak için. Dağıtmak için https://<app_name>.scm.azurewebsites.net/api/zipdeploy bir POST isteği gönderin. POST isteğinin ileti gövdesinde .zip dosyasını içermesi gerekir. Uygulamanızın dağıtım kimlik bilgileri, HTTP BASIC kimlik doğrulaması kullanılarak istekte belirtilir. Daha fazla bilgi için [.zip anında dağıtım başvurusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

HTTP temel kimlik doğrulaması için App Service dağıtım kimlik bilgileriniz gerekir. Dağıtım kimlik bilgilerinizi ayarlamak hakkında bilgi için bkz. [ayarlayın ve kullanıcı düzeyi kimlik bilgilerini sıfırlama](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>CURL ile

Aşağıdaki örnek, .zip dosyasını dağıtmak için cURL aracını kullanır. Yer tutucuları değiştirmeniz `<username>`, `<password>`, `<zip_file_path>`, ve `<app_name>`. CURL tarafından istendiğinde parolayı yazın.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Bu istek anında dağıtım karşıya yüklenen .zip dosyasından tetikler. Kullanarak, güncel ve geçmiş dağıtımlar inceleyebilirsiniz `https://<app_name>.scm.azurewebsites.net/api/deployments` aşağıdaki cURL örnekte gösterildiği gibi uç nokta. Yeniden değiştirin `<app_name>` uygulamanızın adı ile ve `<deployment_user>` kullanıcı adıyla dağıtım kimlik bilgilerinizi.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnekte [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) .zip dosyasını içeren bir istek gönderebilirsiniz. Yer tutucuları değiştirmeniz `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, ve `<app_name>`.

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

Bu istek anında dağıtım karşıya yüklenen .zip dosyasından tetikler. Güncel ve geçmiş dağıtımlar gözden geçirmek için aşağıdaki komutları çalıştırın. Yeniden değiştirin `<app_name>` yer tutucu.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
