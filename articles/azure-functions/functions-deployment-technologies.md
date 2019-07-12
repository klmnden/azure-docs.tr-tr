---
title: Azure işlevleri'nde dağıtım teknolojileri | Microsoft Docs
description: Azure işlevleri kodu dağıtabileceğiniz farklı yollarını öğrenin.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 47d8bf33fd686942326db3b1cc606978bf47a1bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594387"
---
# <a name="deployment-technologies-in-azure-functions"></a>Azure işlevleri'nde dağıtım teknolojileri

Azure işlevleri proje kodunuzu Azure'a dağıtmanın birkaç farklı teknolojileri kullanabilirsiniz. Bu makalede bu teknolojilerinin kapsamlı bir liste sağlar, hangi işlevleri türü için hangi teknolojileri kullanılabilir açıklar, her bir yöntemin kullandığınızda neler olacağını açıklayan ve çeşitli senaryolarda kullanmak için en iyi yöntem önerileri sağlar . Azure işlevleri'ne dağıtma destekleyen çeşitli araçları, bağlama göre sağa teknoloji için ayarlanmış.

## <a name="deployment-technology-availability"></a>Dağıtım teknolojisi kullanılabilirlik

> [!IMPORTANT]
> Azure işlevleri, platformlar arası yerel geliştirme ve barındırma Windows ve Linux üzerinde destekler. Şu anda üç barındırma planı bulunmaktadır: [Tüketim](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), ve [(Azure App Service) adanmış](functions-scale.md#app-service-plan). Her plan farklı davranışları vardır. Tüm dağıtım teknolojileri, Azure işlevleri'nin her özellik için kullanılabilir.

| Dağıtım teknolojisi | Windows tüketim | Windows Premium (Önizleme) | Özel Windows  | Linux tüketim (Önizleme) | Ayrılmış Linux |
|-----------------------|:-------------------:|:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| Dış paket URL'si<sup>1</sup> |✔|✔|✔|✔|✔|
| Zip dağıtma |✔|✔|✔| |✔|
| Docker kapsayıcısı | | | | |✔|
| Web Dağıtımı |✔|✔|✔| | |
| Kaynak denetimi |✔|✔|✔| |✔|
| Yerel Git<sup>1</sup> |✔|✔|✔| |✔|
| Bulut eşitleme<sup>1</sup> |✔|✔|✔| |✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|
| Portal düzenleme |✔|✔|✔| |✔<sup>2</sup>|

<sup>1</sup> gerektiren dağıtım teknolojisi [el ile tetikleyici eşitleniyor](#trigger-syncing).  
<sup>2</sup> portalı düzenleme etkin yalnızca HTTP ve Zamanlayıcı tetikleyici özel planı kullanarak Linux'ta işlevler için.

## <a name="key-concepts"></a>Önemli kavramlar

Bazı temel kavramları, dağıtımları Azure işlevleri'nde nasıl çalıştığını anlamak için önemlidir.

### <a name="trigger-syncing"></a>Tetikleyici eşitleniyor

Kendi Tetikleyicileri değiştirdiğinizde, İşlevler altyapı değişikliklerden haberdar olması gerekir. Eşitleme, birçok dağıtım teknolojileri için otomatik olarak gerçekleşir. Ancak, bazı durumlarda, el ile Tetikleyiciler eşitlemeniz gerekir. Bir dış paket URL'si, yerel Git, bulut eşitleme veya FTP başvurarak, güncelleştirmeleri dağıtırken, tetikleyici el ile eşitlemeniz gerekir. Üç yoldan biriyle Tetikleyicileri eşitleyebilirsiniz:

* Azure portalında işlev uygulamanızı yeniden başlatın
* Bir HTTP POST isteği gönderin `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` kullanarak [ana anahtarı](functions-bindings-http-webhook.md#authorization-keys).
* Bir HTTP POST isteği gönderin `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Yer tutucuları, abonelik Kimliğiniz, kaynak grubu adı ve işlev uygulamanızın adı ile değiştirin.

## <a name="deployment-technology-details"></a>Dağıtım teknolojisi ayrıntıları 

Aşağıdaki dağıtım yöntemleri, Azure işlevleri'nde kullanılabilir.

### <a name="external-package-url"></a>Dış paket URL'si

İşlevi uygulamanızı içeren bir uzaktan paket (.zip) dosyasına başvurmak için bir dış paket URL'si kullanabilirsiniz. Sağlanan URL'den dosya indirilir ve uygulamanın çalıştığı [paketi çalıştırmak](run-functions-from-deployment-package.md) modu.

>__Nasıl kullanılacağını:__ Ekleme `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarlarınızı için. Bu ayarın değerini (çalıştırmak istediğiniz belirli paket dosyasının konumu) URL olmalıdır. Ayarları ekleyebilirsiniz ya da [portalında](functions-how-to-use-azure-function-app-settings.md#settings) veya [Azure CLI kullanarak](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). 
>
>Azure Blob Depolama hizmetini kullanıyorsanız, özel bir kapsayıcı ile kullanan bir [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) paketi işlevleri erişmenizi sağlayacak. Uygulama yeniden başlatılmadan için istediğiniz zaman, içeriğin bir kopyasını getirir. Başvuru uygulama ömrü boyunca geçerli olması gerekir.

>__Ne zaman kullanılmalı:__ Dış paket URL'si, Azure işlevleri tüketim planı (Önizleme) Linux'ta çalışan için yalnızca desteklenen dağıtım yöntemidir. Bir işlev uygulaması başvuran paket dosyası güncelleştirdiğinizde gerekir [Tetikleyicileri'el ile eşitleme](#trigger-syncing) Azure uygulamanızı değişmiş olduğunu söylemek için.

### <a name="zip-deploy"></a>Zip dağıtma

Kullanım zip dağıtma-Azure işlevi uygulamanızı içeren bir .zip dosyası göndermek için. Başlamak için uygulamanızı isteğe bağlı olarak ayarlayabileceğiniz [paketi çalıştırmak](run-functions-from-deployment-package.md) modu.

>__Nasıl kullanılacağını:__ Sık kullanılan bir istemci araç kullanarak dağıtın: [VS Code'u](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), veya [Azure CLI](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Bir .zip dosyası işlev uygulamanıza el ile dağıtmak için yönergeleri izleyin. [Dağıt bir .zip dosyası veya URL'si](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).
>
>Zip kullanarak dağıttığınızda dağıtmayı, uygulamanızı çalıştırmak için ayarlayabileceğiniz [paketi çalıştırmak](run-functions-from-deployment-package.md) modu. Paket çalıştırma modu ayarlamak için ayarlayın `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarının değerine `1`. Zip dağıtım önerilir. Uygulamalarınız için daha hızlı yükleme süreleri verir ve VS Code, Visual Studio ve Azure CLI için varsayılan değerdir.

>__Ne zaman kullanılmalı:__ Zip dağıtma adanmış planında Linux'ta çalışan Azure işlevleri ve Windows üzerinde çalışan Azure işlevleri için önerilen dağıtım teknolojisidir.

### <a name="docker-container"></a>Docker kapsayıcısı

İşlevi uygulamanızı içeren bir Linux kapsayıcı görüntünüzü dağıtabilirsiniz.

>__Nasıl kullanılacağını:__ Özel planı içinde bir Linux işlev uygulaması oluşturmak ve çalıştırmak için hangi kapsayıcı görüntüsünü belirtin. Bunu iki şekilde yapabilirsiniz:
>
>* Azure portalında bir Azure App Service planı üzerinde bir Linux işlev uygulaması oluşturun. İçin **Yayımla**seçin **Docker görüntüsü**ve ardından kapsayıcıya yapılandırın. Görüntü barındırıldığı konumu girin.
>* Linux işlev uygulaması, Azure CLI kullanarak bir App Service planında oluşturun. Bilgi edinmek için bkz. nasıl [özel bir görüntü kullanarak Linux üzerinde bir işlev oluşturma](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Mevcut bir uygulamaya özel bir kapsayıcı kullanarak dağıtmak için [Azure işlevleri çekirdek Araçları](functions-run-local.md), kullanın [ `func deploy` ](functions-run-local.md#publish) komutu.

>__Ne zaman kullanılmalı:__ Linux ortamı hakkında daha fazla denetime ihtiyacınız olduğunda işlev uygulamanızın çalıştığı Docker kapsayıcısını seçeneğini kullanın. Bu dağıtım mekanizması, yalnızca bir App Service planında Linux'ta çalışan işlevler için kullanılabilir.

### <a name="web-deploy-msdeploy"></a>Web dağıtımı (MSDeploy)

Web dağıtımı paketleri ve Windows uygulamalarınızı Windows azure'da çalışan işlev uygulamalarınızı dahil olmak üzere, herhangi bir IIS sunucusuna dağıtır.

>__Nasıl kullanılacağını:__ Kullanım [Azure işlevleri için Visual Studio Araçları](functions-create-your-first-function-visual-studio.md). NET **Çalıştır (önerilen) paket dosyasından** onay kutusu.
>
>Ayrıca yükleyebilirsiniz [Web dağıtma 3.6](https://www.iis.net/downloads/microsoft/web-deploy) ve çağrı `MSDeploy.exe` doğrudan.

>__Ne zaman kullanılmalı:__ Web dağıtımı desteklenmez ve herhangi bir sorun yok ancak tercih edilen yoludur [zip çalıştırma gelen etkin paketiyle dağıtmak](#zip-deploy). Daha fazla bilgi için bkz. [Visual Studio geliştirme Kılavuzu](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Kaynak denetimi

Kaynak denetimi, işlev uygulamanızı bir Git deposuna bağlamak için kullanın. Bir depodaki kod için bir güncelleştirme dağıtımı tetikler. Daha fazla bilgi için [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Nasıl kullanılacağını:__ Kaynak denetimi yayımlamayı ayarlamak için Azure işlevleri portalındaki dağıtım Merkezi'ni kullanın. Daha fazla bilgi için [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

>__Ne zaman kullanılmalı:__ Kaynak denetimini kullanarak, işlev uygulamaları üzerinde işbirliği takımlar için en iyi uygulamadır. Kaynak denetimi daha karmaşık bir dağıtım işlem hatları sağlayan bir iyi bir dağıtım seçeneğidir.

### <a name="local-git"></a>Yerel Git

Yerel Git kodu Azure işlevleri, yerel makinenizde Git kullanarak kullanabilirsiniz.

>__Nasıl kullanılacağını:__ Bölümündeki yönergeleri [Azure uygulama Hizmeti'nde yerel Git dağıtımı](../app-service/deploy-local-git.md).

>__Ne zaman kullanılmalı:__ Genel olarak, farklı dağıtım yöntemi kullanmanızı öneririz. Yerel Git deposundan yayımladığınızda, şunları yapmalısınız [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="cloud-sync"></a>Bulut eşitleme

Dropbox ve OneDrive içeriğinizi Azure işlevleri eşitleme için eşitleme bulut kullanın.

>__Nasıl kullanılacağını:__ Bölümündeki yönergeleri [içerikleri bulut klasöründen eşitleyin](../app-service/deploy-content-sync.md).

>__Ne zaman kullanılmalı:__ Genel olarak, diğer dağıtım yöntemleri öneririz. Bulut eşitleme kullanarak yayımladığınızda, şunları yapmalısınız [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="ftp"></a>FTP

FTP, doğrudan Azure işlevleri'ne dosyaları aktarmak için kullanabilirsiniz.

>__Nasıl kullanılacağını:__ Bölümündeki yönergeleri [FTP/s olarak içerik dağıtmanızı](../app-service/deploy-ftp.md).

>__Ne zaman kullanılmalı:__ Genel olarak, diğer dağıtım yöntemleri öneririz. FTP kullanarak yayımladığınızda, şunları yapmalısınız [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="portal-editing"></a>Portal düzenleme

Portal tabanlı Düzenleyicisi'nde (temelde yaptığınız değişiklikleri her kaydettiğinizde dağıtma) işlev uygulamanızda dosyaları doğrudan düzenleyebilirsiniz.

>__Nasıl kullanılacağını:__ Azure portalında işlevlerinizi düzenleyebilmek için olmalıdır [işlevlerinizi portalda oluşturulan](functions-create-first-azure-function.md). Tek gerçeklik kaynağı korumak için herhangi bir dağıtım yöntemi kullanarak, işlevinizi salt okunur hale getirir ve sürekli portal düzenleme önler. Dosyalarınızı Azure portalında, düzenleyebileceğiniz bir duruma döndürmek için el ile düzenleme modunu etkinleştirebilirsiniz geri `Read/Write` ve uygulama dağıtımıyla ilgili ayarları kaldırın (gibi `WEBSITE_RUN_FROM_PACKAGE`). 

>__Ne zaman kullanılmalı:__ Portal, Azure işlevleri ile kullanmaya başlamak için iyi bir yoludur. Daha güçlü geliştirme çalışması için istemci araçları kullanmanızı öneririz:
>
>* [VS Code kullanmaya başlama](functions-create-first-function-vs-code.md)
>* [Azure işlevleri çekirdek Araçları'nı kullanmaya başlama](functions-run-local.md)
>* [Visual Studio kullanmaya başlama](functions-create-your-first-function-visual-studio.md)

Aşağıdaki tabloda, düzenleme, destek portalı diller ve işletim sistemleri gösterilmektedir:

| | Windows tüketim | Windows Premium (Önizleme) | Özel Windows | Linux tüketim (Önizleme) | Ayrılmış Linux |
|-|:-----------------: |:-------------------------:|:-----------------:|:---------------------------:|:---------------:|
| C# | | | | | |
| C#Komut dosyası |✔|✔|✔| |✔<sup>*</sup>|
| F# | | | | | |
| Java | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>*</sup>|
| Python (Önizleme) | | | | | |
| PowerShell (Önizleme) |✔|✔|✔| | |
| TypeScript (Node.js) | | | | | |

<sup>*</sup> Portal düzenleme yalnızca HTTP ve Zamanlayıcı tetikleyici özel planı kullanarak Linux'ta işlevler için etkinleştirilir.

## <a name="deployment-slots"></a>Dağıtım yuvaları

İşlev uygulamanızı Azure'a dağıtırken, doğrudan üretime dağıtmak yerine ayrı bir dağıtım yuvası dağıtabilirsiniz. Dağıtım yuvaları hakkında daha fazla bilgi için bkz: [Azure App Service yuvası](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Dağıtım yuvaları düzeyde destek

İki dağıtım yuvası için destek düzeyi vardır:

* **Genel kullanılabilirlik (GA)** : Tam olarak desteklenen ve üretim sırasında kullanım için onaylı.
* **Önizleme**: Henüz desteklenir, ancak genel kullanım durumu gelecekte kullanıma ulaşması bekleniyor.

| İşletim sistemi ve barındırma planı | Destek düzeyi |
| --------------- | ------ |
| Windows tüketim | Önizleme |
| Windows Premium (Önizleme) | Önizleme |
| Özel Windows | Genel kullanılabilirlik |
| Linux tüketim | Desteklenmiyor |
| Ayrılmış Linux | Genel kullanılabilirlik |

## <a name="next-steps"></a>Sonraki adımlar

İşlev uygulamalarınızı dağıtma hakkında daha fazla bilgi için bu makaleleri okuyun: 

+ [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)
+ [Azure DevOps kullanarak sürekli teslim](functions-how-to-azure-devops.md)
+ [Azure işlevleri için zip dağıtımları](deployment-zip-push.md)
+ [Bir paket dosyasından, Azure işlevleri'ni çalıştırma](run-functions-from-deployment-package.md)
+ [Azure işlevleri'nde işlev uygulamanız için kaynak dağıtımını otomatikleştirme](functions-infrastructure-as-code.md)
