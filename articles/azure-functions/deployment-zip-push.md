---
title: Zip iletme dağıtımı için Azure işlevleri | Microsoft Docs
description: Kudu dağıtım hizmetinin .zip dosyası dağıtım özellikleri, Azure işlevleri yayımlamak için kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/12/2018
ms.author: glenga
ms.openlocfilehash: 2762e5c4f2b67415a0e42e80a34ae5b34c57adc9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62111211"
---
# <a name="zip-deployment-for-azure-functions"></a>Azure işlevleri için zip dağıtım

Bu makalede, işlev uygulaması proje dosyalarınızı (sıkıştırılmış) .zip dosyasından Azure'a dağıtmayı açıklar. Azure CLI kullanarak ve REST API'leri kullanarak bir anında iletme dağıtımı gerçekleştirmeyi öğrenin. [Azure işlevleri temel araçları](functions-run-local.md) ayrıca yerel bir projeyi Azure'da yayımlarken bu dağıtım API'leri kullanır.

Azure işlevleri, çeşitli Azure App Service tarafından sağlanan sürekli dağıtım ve tümleştirme seçenekleri vardır. Daha fazla bilgi için [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

Geliştirme hızlandırmak için işlev uygulaması proje dosyalarınızı bir .zip dosyasından doğrudan dağıtmak daha kolay. .Zip dağıtım API'si .zip dosyasının içeriğini alır ve içeriği ayıklar `wwwroot` işlev uygulamanızın klasör. Bu .zip dosyası dağıtımı dahil olmak üzere bu powers sürekli tümleştirme tabanlı dağıtımlar, aynı Kudu hizmet kullanır:

+ Önceki dağıtımları kalan dosyaları silme işlemi.
+ Dağıtım özelleştirme, dağıtım betikleri çalıştırmak dahil.
+ Dağıtım günlükleri.
+ İşlev Tetikleyicileri eşitleniyor bir [tüketim planı](functions-scale.md) işlev uygulaması.

Daha fazla bilgi için [.zip dağıtım başvurusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="deployment-zip-file-requirements"></a>Dağıtım .zip dosyası gereksinimleri

Anında iletme dağıtımı için kullandığınız bir .zip dosyası, tüm işlevinizi çalıştırmak için gerekli dosyaları içermelidir.

>[!IMPORTANT]
> .Zip dağıtım kullandığınızda, işlev uygulamanızı .zip dosyasında bulunan olmayan mevcut bir dağıtımı dosyalarından silinir.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bir işlev uygulaması tüm dosyaları ve klasörleri içeren `wwwroot` dizin. Bir .zip dosyası dağıtım içeriğini içeren `wwwroot` dizin, ancak dizin kendisi değil. Bir C# sınıf kitaplığı projesi dağıtılırken derlenmiş kitaplık dosyaları ve bağımlılıkları içermelidir bir `bin` .zip paketinizdeki alt.

## <a name="download-your-function-app-files"></a>İşlevi uygulama dosyalarınızı indirin

Yerel bir bilgisayarda geliştirirken, geliştirme bilgisayarınızda işlevi uygulaması proje klasörünün bir .zip dosyası oluşturmak kolay bir işlemdir.

Ancak, işlevleriniz Azure portalında Düzenleyicisi'ni kullanarak oluşturmuş olabilir. Bu şekilde var olan bir işlev uygulama projesi indirebilirsiniz:

+ **Azure portalından:**

  1. Oturum [Azure portalında](https://portal.azure.com)ve ardından işlev uygulamanıza gidin.

  2. Üzerinde **genel bakış** sekmesinde **uygulama içeriği karşıdan**. Yükleme seçeneklerinizi belirleyin ve ardından **indirme**.

      ![İşlev uygulaması projenizi indirin](./media/deployment-zip-push/download-project.png)

     İşlev uygulamanızı .zip anında iletme dağıtımı kullanarak yeniden yayımlanması doğru biçimde indirilen .zip dosyasıdır. Portal indirme, işlev uygulamanızı doğrudan Visual Studio'da açmak için gerekli dosyaları da ekleyebilirsiniz.

+ **REST API'lerini kullanarak:**

    Dosyaları indirmek için aşağıdaki dağıtım alma API'sini kullanın, `<function_app>` proje: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Dahil olmak üzere `/site/wwwroot/` zip dosyanızın yalnızca işlev uygulaması proje dosyalarını ve tüm site içerdiğinden emin olur. Zaten Azure'da oturum değil, bunu yapmak istenir.  

Ayrıca, bir .zip dosyası bir GitHub deposundan indirebilirsiniz. Bir GitHub deposu bir .zip dosyası olarak karşıdan yüklerken, GitHub dal için bir ek klasör düzeyinde ekler. Github'dan indirdiği .zip dosyasını doğrudan olarak dağıtamazsınız ek klasör düzeyi anlamına gelir. İşlev uygulamanızı korumak için bir GitHub deposu kullanıyorsanız, kullanmanız gereken [sürekli tümleştirme](functions-continuous-deployment.md) uygulamanızı dağıtmak için.  

## <a name="cli"></a>Azure CLI kullanarak dağıtma

Bir anında iletme dağıtımı tetiklemek için Azure CLI'yı kullanabilirsiniz. Anında iletme dağıtma bir .zip dosyası işlev uygulamanızı kullanarak [az functionapp deployment kaynak config-zip](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config-zip) komutu. Bu komutu kullanmak için Azure CLI Sürüm 2.0.21 kullanın veya üzeri. Kullanmakta olduğunuz hangi Azure CLI sürümünü görmek için `az --version` komutu.

Aşağıdaki komutta `<zip_file_path>` , .zip dosyasının konumu yolu ile yer tutucu. Ayrıca, değiştirin `<app_name>` işlev uygulamanızı benzersiz adı. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```

Bu komutu azure'daki işlev uygulamanızın proje dosyalarına indirilen .zip dosyasından dağıtır. Sonra uygulamayı yeniden başlatır. Bu işlev uygulaması için dağıtım listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI'yı kullanırken `<zip_file_path>` bilgisayarınızdaki .zip dosyasının yolu. Azure CLI de çalıştırmak [Azure Cloud Shell](../cloud-shell/overview.md). Cloud Shell kullandığınızda, Cloud Shell ile ilişkili Azure dosyaları hesap için ilk dağıtım .zip dosyanızın yüklemeniz gerekir. Bu durumda, `<zip_file_path>` Cloud Shell hesabınızı kullanan depolama konumu. Daha fazla bilgi için [kalıcı dosyaları Azure Cloud shell'de](../cloud-shell/persisting-shell-storage.md).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

## <a name="run-functions-from-the-deployment-package"></a>Dağıtım paketinden işlevleri çalıştırma

İşlevlerinizi doğrudan dağıtım paket dosyasından çalıştırmayı seçebilirsiniz. Bu yöntem paketi dosyaları kopyalama dağıtım adımı atlanıyor `wwwroot` işlev uygulamanızın dizin. Bunun yerine, paket dosyası işlevler çalışma zamanı ve içeriğini takılı `wwwroot` dizin salt okunur hale gelir.  

Zip dağıtım tümleşen işlev uygulama ayarını etkinleştirebilirsiniz. Bu özellik ile `WEBSITE_RUN_FROM_PACKAGE` değerini `1`. Daha fazla bilgi için [bir dağıtım paketi dosyasından işlevlerinizin çalıştığı](run-functions-from-deployment-package.md).

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
