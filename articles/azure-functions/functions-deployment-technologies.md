---
title: Azure işlevleri'nde dağıtım teknolojileri | Microsoft Docs
description: Azure işlevleri kodu dağıtabileceğiniz farklı yollarını yönüyle öğrenin.
services: functions
documentationcenter: .net
author: ColbyTresness
manager: dariac
ms.service: azure-functions
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: cotresne
ms.openlocfilehash: 118daf02ab59646f2926071763aa4d7e97846e04
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508225"
---
# <a name="deployment-technologies-in-azure-functions"></a>Azure işlevleri'nde dağıtım teknolojileri

Azure işlevleri proje kodunuzu Azure'a dağıtmak için kullanabileceğiniz birkaç farklı teknolojiler vardır. Bu makalede bu teknolojilerinin kapsamlı bir liste sağlar, hangi teknolojileri hangi işlevleri türü için kullanılabilir olduğunu bildirir, her yöntem kullanıldığında ve kullanmak en iyi yöntem önerileri sağlar aslında neler olduğunu açıklar. çeşitli senaryolar için. Azure işlevleri'ne dağıtma destekleyen çeşitli araçları, bağlama göre sağa teknoloji için ayarlanmış.

## <a name="deployment-technology-availability"></a>Dağıtım teknolojisi kullanılabilirlik

> [!IMPORTANT]
> Azure işlevleri destekler, yerel platform geliştirme ve barındırma iki işletim sistemlerinde çapraz: Windows ve Linux. Üç barındırma planı şu anda kullanılabilir her biri farklı davranışları - [tüketim](functions-scale.md#consumption-plan), [Premium](functions-scale.md#premium-plan), ve [(App Service) adanmış](functions-scale.md#app-service-plan). Tüm dağıtım teknolojileri, Azure işlevleri'nin her özellik için kullanılabilir.

| Dağıtım teknolojisi | Windows tüketim | Windows premium (Önizleme) | Özel Windows  | Linux tüketim (Önizleme) | Ayrılmış Linux |
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
<sup>2</sup> portalı düzenleme etkin yalnızca HTTP ve Zamanlayıcı tetikleyici adanmış planı kullanarak Linux'ta işlevler için.

## <a name="key-concepts"></a>Önemli kavramlar

Devam etmeden önce dağıtımları Azure işlevleri'nde nasıl çalıştığını anlamak için kritik bazı temel kavramları öğrenmeniz önemlidir.

### <a name="trigger-syncing"></a>Tetikleyici eşitleniyor

Kendi Tetikleyicileri değiştirdiğinizde, İşlevler altyapı bu değişikliklerden haberdar olması gerekir. Bu eşitleme, birçok dağıtım teknolojileri için otomatik olarak gerçekleşir. Ancak, bazı durumlarda, Tetikleyiciler elle eşitlemelidir. Bir dış paket URL'si, yerel Git, bulut eşitleme veya FTP kullanarak, güncelleştirmeleri dağıtırken, Tetikleyiciler elle eşitlemek emin olmanız gerekir. Üç yoldan biriyle Tetikleyicileri eşitleyebilirsiniz:

* Azure portalında işlev uygulamanızı yeniden başlatın
* Bir HTTP POST isteği gönderin `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` kullanarak [ana anahtarı](functions-bindings-http-webhook.md#authorization-keys).
* Bir HTTP POST isteği gönderin `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`. Yer tutucuları, abonelik Kimliğiniz, kaynak grubu adı ve işlev uygulamanızın adı ile değiştirin.

## <a name="deployment-technology-details"></a>Dağıtım teknolojisi ayrıntıları  

Bu aşağıdaki dağıtım yöntemleri, Azure işlevleri tarafından desteklenir.

### <a name="external-package-url"></a>Dış paket URL'si

İşlevi uygulamanızı içeren bir uzaktan paket (.zip) dosya başvuru sağlar. Sağlanan URL'den dosya indirilir ve uygulamanın çalıştığı [çalışma alanından paket](run-functions-from-deployment-package.md) modu.

>__Nasıl kullanılacağını:__ Ekleme `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarlarınızı için. Bu ayarın değerini adresa URL – çalıştırmak istediğiniz belirli paket dosyasının konumu olmalıdır. Ayarları ekleyebilirsiniz ya da [portalında](functions-how-to-use-azure-function-app-settings.md#settings) veya [Azure CLI kullanarak](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set). Azure blob depolama kullanıyorsanız, özel bir kapsayıcı ile kullanmalısınız bir [paylaşılan erişim imzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) paketi işlevleri erişmenizi sağlayacak. Dilediğiniz zaman uygulama yeniden başlatılmadan başvurunuz uygulama ömrü boyunca geçerli olması gerektiği anlamına gelir içeriğin bir kopyasını getirir.

>__Ne zaman kullanılmalı:__ Bu, Azure işlevleri tüketim planı (Önizleme) Linux'ta çalışan için desteklenen tek dağıtım yöntemidir. Bir işlev uygulamasına başvuruyor paket dosyası güncelleştirilirken gerekir [Tetikleyicileri'el ile eşitleme](#trigger-syncing) Azure uygulamanızı değişmiş olduğunu söylemek için.

### <a name="zip-deploy"></a>Zip dağıtma

Azure işlevi uygulamanızı içeren bir zip dosyası göndermenize izin verir. İsteğe bağlı olarak, başlangıç sağlayacağınızı belirtebilirsiniz [çalışma alanından paket](run-functions-from-deployment-package.md) modu.

>__Nasıl kullanılacağını:__ En sık kullandığınız dağıtmanızı istemci aracı - [VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure), [Visual Studio](functions-develop-vs.md#publish-to-azure), veya [Azure CLI](functions-create-first-azure-function-azure-cli.md#deploy-the-function-app-project-to-azure). Bir zip dosyası işlev uygulamanıza el ile dağıtmak için konusunda bulunan yönergeleri izleyerek [bir zip dosyası veya URL'si dağıtma](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url).
>
>Zip dağıtma dağıttığınızda, ayrıca, kullanıcılar kendi uygulama içinden çalıştırmak için belirtebilir [çalışma alanından paket](run-functions-from-deployment-package.md) ayarlayarak modu `WEBSITE_RUN_FROM_PACKAGE` uygulama ayarı değeri olarak `1`. Bu seçenek önerilir ve uygulamalarınız için daha hızlı yükleme süreleri verir. Bu, yukarıdaki istemci araçları için varsayılan olarak gerçekleştirilir.

>__Ne zaman kullanılmalı:__ Bu Windows üzerinde çalışan Azure işlevleri ve özel planı Linux üzerinde çalışan Azure işlevleri için önerilen dağıtım teknolojisidir.

### <a name="docker-container"></a>Docker kapsayıcısı

İşlevi uygulamanızı içeren bir Linux kapsayıcı görüntünüzü dağıtmak.

>__Nasıl kullanılacağını:__ Özel planı içinde bir Linux işlev uygulaması oluşturmak ve çalıştırmak için hangi kapsayıcı görüntüsünü belirtin. Bunu iki şekilde yapabilirsiniz:
>
>* Azure portalında App Service planı üzerinde bir Linux işlev uygulaması oluşturun. Seçin **Docker görüntüsü** için **Yayımla**ve kapsayıcısına görüntü barındırıldığı konumu yapılandırın.
>* Azure CLI aracılığıyla bir App Service planı üzerinde bir Linux işlev uygulaması oluşturun. Bilgi inceleyerek nasıl [Linux üzerinde özel görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md#create-and-deploy-the-custom-image).
>
>Mevcut bir uygulamayı kullanarak özel bir kapsayıcı dağıtmak için [ `func deploy` ](functions-run-local.md#publish) komutu [Azure işlevleri çekirdek Araçları](functions-run-local.md).

>__Ne zaman kullanılmalı:__ Linux ortamı hakkında daha fazla denetime ihtiyacınız olduğunda işlev uygulamanızın çalıştığı bu seçeneği kullanın. Bu dağıtım mekanizması, yalnızca bir App Service planında Linux'ta çalışan işlevler için kullanılabilir.

### <a name="web-deploy-msdeploy"></a>Web dağıtımı (MSDeploy)

Paketler ve Windows uygulamalarınızı Windows azure'da çalışan işlev uygulamalarınızı dahil olmak üzere, herhangi bir IIS sunucusuna dağıtır.

>__Nasıl kullanılacağını:__ Kullanım [Azure işlevleri için Visual Studio Araçları](functions-create-your-first-function-visual-studio.md), kaldırın `Run from package file (recommended)` kutusu.
>
> Ayrıca yükleyebilirsiniz [Web dağıtma 3.6](https://www.iis.net/downloads/microsoft/web-deploy) ve çağrı `MSDeploy.exe` doğrudan.

>__Ne zaman kullanılmalı:__ Bu dağıtım teknolojisi desteklenmez ve herhangi bir sorun yok, ancak tercih edilen mekanizması şimdi [Zip dağıtımı etkin paket gelen çalıştırma ile](#zip-deploy). Daha fazla bilgi için ziyaret [Visual Studio geliştirme Kılavuzu](functions-develop-vs.md#publish-to-azure).

### <a name="source-control"></a>Kaynak denetimi

Tetikleyen kod bir depodaki herhangi bir güncelleştirme dağıtımı, işlev uygulamanızı bir git deposuna bağlanmanıza olanak sağlar. Daha fazla bilgi için göz atın [Kudu Wiki](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments).

>__Nasıl kullanılacağını:__ Kaynak denetimi yayımlamayı ayarlamak için Azure işlevleri portalındaki dağıtım Merkezi'ni kullanın. Daha fazla bilgi için [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md).

>__Ne zaman kullanılmalı:__ Kaynak denetimi kullanmaktır işlevi uygulamaları üzerinde işbirliği takımlar için en iyi ve daha karmaşık bir dağıtım işlem hatları sağlayan harika bir seçenek budur.

### <a name="local-git"></a>Yerel git

Sağlayan yerel makinenizde Git kullanarak Azure işlevleri için kod gönderin.

>__Nasıl kullanılacağını:__ Konumundaki yönergeleri [Azure uygulama Hizmeti'nde yerel Git dağıtımı](../app-service/deploy-local-git.md).

>__Ne zaman kullanılmalı:__ Genel olarak, diğer dağıtım yöntemlerini kullanmanız önerilir. Yerel git deposundan yayımlama sırasında gerekir [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="cloud-sync"></a>Bulut eşitleme

Azure işlevleri için Dropbox'ı ve OneDrive içeriğinizi eşitleme sağlar.

>__Nasıl kullanılacağını:__ Bölümündeki yönergeleri [içerikleri bulut klasöründen eşitleyin](../app-service/deploy-content-sync.md).

>__Ne zaman kullanılmalı:__ Genel olarak, diğer dağıtım yöntemlerini kullanmanız önerilir. Bulut eşitleme ile yayımlarken gerekir [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="ftp"></a>FTP

Azure işlevleri aktarım dosyalarını doğrudan olanak sağlar.

>__Nasıl kullanılacağını:__ Bölümündeki yönergeleri [FTP/s kullanarak içerik dağıtma](../app-service/deploy-ftp.md).

>__Ne zaman kullanılmalı:__ Genel olarak, diğer dağıtım yöntemlerini kullanmanız önerilir. FTP ile yayımlarken gerekir [Tetikleyicileri'el ile eşitleme](#trigger-syncing).

### <a name="portal-editing"></a>Portal düzenleme

Portal tabanlı Düzenleyicisi'ni kullanarak, sağlar, işlev uygulamanızı dosyaları doğrudan düzenlemek (tıkladığınız zaman temelde dağıtma **Kaydet**).

>__Nasıl kullanılacağını:__ Azure portalında işlevlerinizi düzenleyebilmek için olmalıdır [işlevlerinizi portalda oluşturulan](functions-create-first-azure-function.md). Herhangi bir dağıtım yöntemi kullanarak, işlevinizi salt okunur hale getirir ve sürekli portal tek gerçeklik kaynağı korumak için düzenleme, engeller. Azure portalını kullanarak dosyalarınızı, düzenleyebileceğiniz bir duruma döndürmek için el ile düzenleme modunu etkinleştirebilirsiniz geri `Read/Write` ve uygulama dağıtımıyla ilgili ayarları kaldırın (gibi `WEBSITE_RUN_FROM_PACKAGE`). 

>__Ne zaman kullanılmalı:__ Portal'ı Azure işlevleri ile kullanmaya başlamak için harika bir yoludur ancak istemcinin kullandığı tüm daha güçlü geliştirme çalışması için Araçlar önerilir:
>
>* [VS Code kullanmaya başlama](functions-create-first-function-vs-code.md)
>* [Azure işlevleri çekirdek Araçları'nı kullanmaya başlama](functions-run-local.md)
>* [Visual Studio kullanmaya başlama](functions-create-your-first-function-visual-studio.md)

Aşağıdaki tabloda için portal düzenleme desteklenen diller ve işletim sistemleri gösterilmektedir:

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

<sup>*</sup> Portal düzenleme yalnızca HTTP ve Zamanlayıcı tetikleyici adanmış planı kullanarak Linux'ta işlevler için etkinleştirilir.

## <a name="deployment-slots"></a>Dağıtım yuvaları

İşlev uygulamanızı Azure'a dağıtırken, doğrudan üretim için ayrı bir dağıtım yuvası yerine dağıtım yapabilirsiniz. Dağıtım yuvaları hakkında daha fazla bilgi için bkz. [Azure App Service yuvası belgeleri](../app-service/deploy-staging-slots.md).

### <a name="deployment-slots-levels-of-support"></a>Dağıtım yuvaları düzeyde destek

İki destek düzeyi vardır:

* _Genel kullanıma (GA)_ - tam olarak desteklenen ve üretim kullanımı için onaylandı.
* _Önizleme_ - henüz desteklenir, ancak genel kullanım durumu gelecekte kullanıma ulaşması bekleniyor.

| İşletim sistemi ve barındırma planı | Destek düzeyi |
| --------------- | ------ |
| Windows tüketim | Önizleme |
| Windows Premium (Önizleme) | Önizleme |
| Özel Windows | Genel kullanıma sunuldu |
| Linux tüketim | Desteklenmiyor |
| Ayrılmış Linux | Genel kullanıma sunuldu |

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde, işlev uygulamalarınızı dağıtma hakkında daha fazla bilgi edinin: 

+ [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)
+ [Azure DevOps kullanarak sürekli teslim](functions-how-to-azure-devops.md)
+ [Azure işlevleri için zip dağıtımları](deployment-zip-push.md)
+ [Bir paket dosyasından, Azure işlevleri'ni çalıştırma](run-functions-from-deployment-package.md)
+ [Azure işlevleri'nde işlev uygulamanız için kaynak dağıtımını otomatikleştirme](functions-infrastructure-as-code.md)
