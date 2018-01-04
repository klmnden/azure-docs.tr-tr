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
<<<<<<< HEAD
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
=======
ms.openlocfilehash: faddb73522200f60f18294dc43e8d235943f8bbb
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>Zip itme dağıtımı için Azure işlevleri 
Bu makalede, Azure için bir .zip (sıkıştırılmış) dosyasından işlevi uygulama projesi dosyaları dağıtmayı açıklar. Azure CLI kullanarak hem REST API'lerini kullanarak anında iletme dağıtım yapmak öğrenin. 

Azure işlevleri, Azure App Service tarafından sağlanan sürekli dağıtım ve tümleştirme seçeneklerini tam aralığını sahiptir. Daha fazla bilgi için bkz: [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). 

Geliştirme sırasında daha hızlı yineleme için işlevi uygulama projesi dosyalarınızdan doğrudan sıkıştırılmış .zip dosyası dağıtmak kolaydır. Bu .zip dosyası dağıtım aynı Kudu hizmeti de dahil olmak üzere bu powers sürekli tümleştirme tabanlı dağıtımlar, kullanır:

+ Önceki dağıtımlarından kalan dosyaları silme.
+ Dağıtım betikleri çalıştırma dahil dağıtım özelleştirme.
+ Dağıtım günlükleri.
+ İşlev Tetikleyicileri eşitleniyor bir [tüketim planı](functions-scale.md) işlev uygulaması.

Daha fazla bilgi için bkz: [.zip itme dağıtım başvurusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Dağıtım .zip dosyası gereksinimleri
Anında iletme dağıtımı için kullandığınız .zip dosyası tüm proje dosyalarını işlevi kodunuzu dahil olmak üzere işlevi uygulamanızda içermelidir. 

>[!IMPORTANT]
> .Zip itme dağıtım kullandığınızda, .zip dosyasında bulunan olmayan mevcut bir dağıtım dosyalarından işlevi uygulamanızdan silinir.  
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

### <a name="function-app-folder-structure"></a>İşlev uygulama klasör yapısı

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

<<<<<<< HEAD
### <a name="downloading-your-function-app-project"></a>İşlev uygulama projenizi indirme

Yerel geliştirme yaparken bir .zip dosyası işlevi uygulama projesi klasörünün geliştirme bilgisayarınızda oluşturmak kolaydır. Ancak, ayrıca portala Düzenleyicisi'ni kullanarak işlevlerinizi oluşturmuş olabileceğiniz. İşlev uygulaması projenize portalından karşıdan yüklemek için: 

1. Oturum [Azure portal](https://portal.azure.com) ve işlev uygulamanıza gidin.

2. İçinde **genel bakış** sekmesine **uygulama içeriğini indirmek**Yükleme seçeneklerinizi belirtin ve tıklatın **karşıdan**.     

    ![İşlev uygulaması projesi indirme](./media/deployment-zip-push/download-project.png)

.Zip itme dağıtım kullanarak işlevi uygulamanıza yayımlanmak üzere doğru biçimde indirilen .zip dosyasıdır.

GitHub, ayrıca bir depodan bir .zip dosyası indirme olanak tanır. GitHub depo bir .zip dosyası olarak yüklediğinizde, GitHub fazladan bir klasör ekler dağıtamazsınız anlamına .zip dosyası olarak Github'dan indirilen şube düzeyini unutmayın. İşlev uygulamanızı korumak için GitHub deposunu kullanıyorsanız, kullanmanız gereken [sürekli tümleştirme](functions-continuous-deployment.md) uygulamanızı dağıtmak için.  

## <a name="cli"></a>Azure CLI kullanarak dağıtın

Bir itme dağıtımı tetiklemek için Azure CLI kullanın. Anında iletme bir .zip dosyası işlevi uygulamanızı kullanarak dağıtın [az functionapp dağıtım kaynağı config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) komutu. Azure CLI Sürüm 2.0.21 kullanın veya sonraki bir sürümü. Kullanım `az --version` kullanmakta olduğunuz hangi sürümünün görmek için komutu.
=======
### <a name="download-your-function-app-project"></a>İşlev uygulama projenizi indirin

Yerel bir bilgisayarda geliştirirken, geliştirme bilgisayarınızda bir .zip dosyası işlevi uygulama projesi klasörünün oluşturmak kolaydır. 

Ancak, işlevlerinizi Azure portalında Düzenleyicisi'ni kullanarak oluşturduğunuz. İşlev uygulaması projenize portalından karşıdan yüklemek için: 

1. Oturum [Azure portal](https://portal.azure.com)ve ardından işlevi uygulamanıza gidin.

2. Üzerinde **genel bakış** sekmesine **uygulama içeriğini indirmek**. Yükleme seçeneklerinizi belirleyin ve ardından **karşıdan**.     

    ![İşlev uygulaması projesi indirme](./media/deployment-zip-push/download-project.png)

İşlev uygulamanıza .zip itme dağıtımı kullanarak yeniden yayımlanması için doğru biçimde indirilen .zip dosyasıdır.

Ayrıca, Github'da depodan bir .zip dosyası indirebilirsiniz. GitHub depo bir .zip dosyası olarak yüklediğinizde, GitHub dal için bir ek klasör düzeyinde ekler aklınızda bulundurun. Github'dan karşıdan doğrudan olarak .zip dosyası dağıtılamıyor ek klasör düzeyinde anlamına gelir. İşlev uygulamanızı korumak için GitHub deposunu kullanıyorsanız, kullanmanız gereken [sürekli tümleştirme](functions-continuous-deployment.md) uygulamanızı dağıtmak için.  

## <a name="cli"></a>Azure CLI kullanarak dağıtma

Bir itme dağıtımı tetiklemek için Azure CLI kullanın. Anında iletme bir .zip dosyası işlevi uygulamanızı kullanarak dağıtın [az functionapp dağıtım kaynağı config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) komutu. Bu komutu kullanmak için Azure CLI Sürüm 2.0.21 kullanın veya sonraki bir sürümü. Kullanmakta olduğunuz hangi Azure CLI sürümünün görmek için `az --version` komutu.
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3

Aşağıdaki komutta, `<zip_file_path>` .zip dosyanızın konumunun yolu ile yer tutucu. Ayrıca, değiştirin `<app_name>` işlevi uygulamanızı benzersiz adı. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
<<<<<<< HEAD
Bu komut işlevi uygulamanızda Azure indirilen .zip dosyasından proje dosyalarına dağıtır ve uygulamayı yeniden başlatır. Bu işlev uygulaması dağıtımlarda listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI kullanırken `<zip_file_path>` bilgisayarınızdaki .zip dosyasının yolu. Azure CLI ayrıca komutunu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md). Bulut Kabuğu'nu kullanarak, bulut Kabuğunuzu ile ilişkili Azure dosya hesabı için önce dağıtım .zip dosyanızın yüklemeniz gerekir. Bu durumda, `<zip_file_path>` bulut Kabuk hesabınız tarafından kullanılan depolama konumu. Daha fazla bilgi için bkz: [kalıcı Azure bulut Kabuk dosyalarında](../cloud-shell/persisting-shell-storage.md).
=======
Bu komut, azure'da işlevi uygulamanız için proje dosyalarını indirilen .zip dosyasından dağıtır. Sonra uygulamayı yeniden başlatır. Bu işlev uygulaması dağıtımlarda listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI kullanırken `<zip_file_path>` bilgisayarınızdaki .zip dosyasının yolu. Azure CLI ayrıca komutunu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md). Bulut Kabuk kullandığınızda, bulut kabuğu ile ilişkili Azure dosyaları hesabına ilk dağıtım .zip dosyanızın yüklemeniz gerekir. Bu durumda, `<zip_file_path>` bulut Kabuk hesabınızı kullanan depolama konumu. Daha fazla bilgi için bkz: [kalıcı Azure bulut Kabuk dosyalarında](../cloud-shell/persisting-shell-storage.md).
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

<<<<<<< HEAD
[.zip itme dağıtım başvuru konusu]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
=======
[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
