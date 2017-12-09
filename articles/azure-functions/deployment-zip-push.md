---
title: "Zip Azure işlevleri için anında iletme dağıtımı | Microsoft Docs"
description: "Azure işlevleri yayımlamak için kullanılan .zip dosyası dağıtım olanakları Kudu dağıtım hizmetinin kullanın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: 5c8c608126f20aa5f5dc52bb8e24cfec14fd00f5
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>Zip itme dağıtımı için Azure işlevleri 
Bu konuda, bir .zip (sıkıştırılmış) dosyasından Azure'a işlevi uygulama projesi dosyaları dağıtmayı açıklar. Azure CLI ve REST API'ler kullanarak anında iletme dağıtım yapmak öğrenin. Azure işlevleri sahip sürekli dağıtım tam aralığını ve daha fazla bilgi için Azure App Service tarafından sağlanan tümleştirme seçeneklerini görmek [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). Ancak, geliştirme sırasında daha hızlı yineleme için genellikle işlevi uygulama projesi dosyalarınızdan doğrudan sıkıştırılmış .zip dosyası dağıtmak daha kolay olur.

Bu .zip dosyası dağıtım aynı Kudu hizmeti bu powers sürekli tümleştirme tabanlı dağıtımlar, aşağıdaki işlevleri içeren kullanır:

+ Önceki bir dağıtımın kalan dosyaları silme.
+ Dağıtım betikleri çalıştırma dahil dağıtım özelleştirme.
+ Dağıtım günlükleri.
+ İşlev Tetikleyicileri eşitleme bir [tüketim planı](functions-scale.md) işlev uygulaması.

Daha fazla bilgi için bkz: [.zip itme dağıtım başvuru konusu]. 

## <a name="requirements-of-the-deployment-zip-file"></a>Dağıtım .zip dosyası gereksinimleri 
İtme dağıtımı için kullandığınız .zip dosyası gerekir içeren tüm proje dosyalarını işlevi kodunuzu dahil olmak üzere işlevi uygulamanızda. 

>[!IMPORTANT]
> .Zip itme dağıtım kullandığınızda, tüm dosyaları .zip dosyasında bulunamadı varolan dağıtımından işlevi uygulamanızdan silinir.  

### <a name="function-app-folder-structure"></a>İşlev uygulama klasör yapısı

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="downloading-your-function-app-project"></a>İşlev uygulama projenizi indirme

Yerel geliştirme yaparken bir .zip dosyası işlevi uygulama projesi klasörünün geliştirme bilgisayarınızda oluşturmak kolaydır. Ancak, ayrıca portala Düzenleyicisi'ni kullanarak işlevlerinizi oluşturmuş olabileceğiniz. İşlev uygulaması projenize portalından karşıdan yüklemek için: 

1. Oturum [Azure portal](https://portal.azure.com) ve işlev uygulamanıza gidin.

2. İçinde **genel bakış** sekmesine **uygulama içeriğini indirmek**Yükleme seçeneklerinizi belirtin ve tıklatın **karşıdan**.     

    ![İşlev uygulaması projesi indirme](./media/deployment-zip-push/download-project.png)

.Zip itme dağıtım kullanarak işlevi uygulamanıza yayımlanmak üzere doğru biçimde indirilen .zip dosyasıdır.

GitHub, ayrıca bir depodan bir .zip dosyası indirme olanak tanır. GitHub depo bir .zip dosyası olarak yüklediğinizde, GitHub fazladan bir klasör ekler dağıtamazsınız anlamına .zip dosyası olarak Github'dan indirilen şube düzeyini unutmayın. İşlev uygulamanızı korumak için GitHub deposunu kullanıyorsanız, kullanmanız gereken [sürekli tümleştirme](functions-continuous-deployment.md) uygulamanızı dağıtmak için.  

## <a name="cli"></a>Azure CLI kullanarak dağıtın

Bir itme dağıtımı tetiklemek için Azure CLI kullanın. Anında iletme bir .zip dosyası işlevi uygulamanızı kullanarak dağıtın [az functionapp dağıtım kaynağı config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) komutu. Azure CLI Sürüm 2.0.21 kullanın veya sonraki bir sürümü. Kullanım `az --version` kullanmakta olduğunuz hangi sürümünün görmek için komutu.

Aşağıdaki komutta, `<zip_file_path>` .zip dosyanızın konumunun yolu ile yer tutucu. Ayrıca, değiştirin `<app_name>` işlevi uygulamanızı benzersiz adı. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Bu komut işlevi uygulamanızda Azure indirilen .zip dosyasından proje dosyalarına dağıtır ve uygulamayı yeniden başlatır. Bu işlev uygulaması dağıtımlarda listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI kullanırken `<zip_file_path>` bilgisayarınızdaki .zip dosyasının yolu. Azure CLI ayrıca komutunu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md). Bulut Kabuğu'nu kullanarak, bulut Kabuğunuzu ile ilişkili Azure dosya hesabı için önce dağıtım .zip dosyanızın yüklemeniz gerekir. Bu durumda, `<zip_file_path>` bulut Kabuk hesabınız tarafından kullanılan depolama konumu. Daha fazla bilgi için bkz: [kalıcı Azure bulut Kabuk dosyalarında](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[.zip itme dağıtım başvuru konusu]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
