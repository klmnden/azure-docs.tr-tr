---
title: "Uygulamanızı Azure App Service'e bir ZIP dosyası ile dağıtma | Microsoft Docs"
description: "Uygulamanızı Azure App Service'e bir ZIP dosyasıyla dağıtmayı öğrenin."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: 9838f0810f4827df3eb4f9407d4d4fbc1ad0ff4d
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Uygulamanızı Azure App Service'e bir ZIP dosyası ile dağıtma

Bu makalede, bir ZIP dosyası web uygulamanıza dağıtmak için nasıl kullanılacağı gösterilmektedir [Azure App Service](app-service-web-overview.md). 

Bu ZIP dosyası dağıtım o powers sürekli tümleştirme tabanlı dağıtımlar aynı Kudu hizmeti kullanır. Kudu ZIP dosyası dağıtım için aşağıdaki işlevi destekler: 

- Önceki bir dağıtımın kalan dosyaları silme.
- Paket geri yüklemesi içerir varsayılan derleme işlemini üzerinde bırakma seçeneği.
- [Dağıtım özelleştirme](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings)gibi dağıtım betikleri çalıştırma.  
- Dağıtım günlükleri. 

Daha fazla bilgi için bkz: [Kudu belgelerine](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için:

* [Bir App Service uygulaması oluşturma](/azure/app-service/), veya başka bir öğretici için oluşturduğunuz bir uygulama kullanın.

## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

>[!NOTE]
> Dosyaları ZIP dosyasında yüklediyseniz, ilk dosyaları ayıklayın. Örneğin, bir ZIP dosyası Github'dan yüklediyseniz, bu dosya olarak dağıtamazsınız-değil. GitHub ile uygulama hizmeti çalışmıyor ek iç içe geçmiş dizinler ekler. 
>

Yerel bir terminal penceresi içinde uygulaması projenize kök dizinine gidin. 

Bu dizin web uygulamanız için giriş dosyası gibi içermelidir _index.html_, _php için index.php'dir_, ve _app.js_. Paket Yönetimi dosyaları gibi içerebilir _project.json_, _composer.json_, _package.json_, _bower.json_ve _requirements.txt_.

ZIP arşivini her şeyin projenizde oluşturun. Aşağıdaki komut, terminal varsayılan Aracı'nı kullanır:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>Bulut Kabuk ZIP dosyasını karşıya yükle

Bunun yerine yerel terminal durumundan Azure CLI çalıştırmayı seçerseniz, bu adımı atlayın.

Burada bulut Kabuk ZIP dosyasını karşıya yüklemek için adımları. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Daha fazla bilgi için bkz: [kalıcı Azure bulut Kabuk dosyalarında](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>ZIP dosyası dağıtma

Karşıya yüklenen ZIP dosyasını kullanarak web uygulamanıza dağıtmak [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) komutu. Bulut Kabuğu'nu kullanmayı seçerseniz, Azure CLI Sürüm 2.0.21 olduğundan emin olun veya sonraki bir sürümü. Varsa çalıştırdığınız hangi sürümü görmek için `az --version` yerel terminal penceresinde komutu. 

Aşağıdaki örnek, yüklediğiniz ZIP dosyası dağıtır. Azure CLI yerel yüklemesi kullanırken, yerel ZIP dosyası yolunu belirtin `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Bu komut dosyalarını dağıtır ve ZIP dizinlerden dosya varsayılan uygulama hizmeti uygulaması klasörünüze (`\home\site\wwwroot`) ve uygulamayı yeniden başlatır. Herhangi bir ek özel derleme işlem yapılandırılmış olsa da çalıştırılır. Daha fazla bilgi için bkz: [Kudu belgelerine](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Bu uygulama için dağıtımları listesini görüntülemek için (sonraki bölüme bakın) REST API'lerini kullanmanız gerekir. 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için deneyin [Git ile azure'a dağıtma](app-service-deploy-local-git.md). Azure Git tabanlı dağıtımına sürüm denetimi, paket geri yüklemesi, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Daha Fazla Kaynak

* [Kudu: zip dosyasından dağıtma](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Azure uygulama hizmeti dağıtım kimlik bilgileri](app-service-deploy-ftp.md)
