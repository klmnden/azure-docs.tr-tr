---
title: Azure Stack'te Azure App Service sunucu rolleri için planlama kapasite | Microsoft Docs
description: Kapasite Azure Stack'te Azure App Service sunucu rolleri için planlama
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/13/2019
ms.openlocfilehash: 06bafbcf3e668ba17b1245b9352e942e02569997
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57852381"
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Kapasite Azure Stack'te Azure App Service sunucu rolleri için planlama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure App Service'in bir üretime hazır dağıtımı ayarlamak için desteklemek için sistem beklediğiniz kapasiteyi planlamanız gerekir.  

Bu makalede, bilgi işlem örnekleri ve işlem SKU'ları herhangi bir üretim dağıtımı için kullanması gereken en düşük sayısı için yönergeler sağlar.

Bu yönergeleri kullanarak App Service kapasite stratejinizi planlayabilirsiniz.

| App Service sunucu rolü | En az örnek sayısı önerilir | Önerilen işlem SKU|
| --- | --- | --- |
| Denetleyici | 2 | A1 |
| Ön Uç | 2 | A1 |
| Yönetim | 2 | A3 |
| Yayımcı | 2 | A1 |
| Web çalışanı - paylaşılan | 2 | A1 |
| Web çalışanı - adanmış | her katman 2 | A1 |

## <a name="controller-role"></a>Denetleyici rolü

**Önerilen en az**: Standart A1'in iki örneği

Azure App Service denetleyicisi genellikle CPU, bellek ve ağ kaynaklarını düşük oranda tüketir. Ancak, yüksek kullanılabilirlik için iki denetleyicisi olmalıdır. İki denetleyici ayrıca izin denetleyicileri sayısı değildir. İkinci web siteleri denetleyicisi doğrudan Yükleyici'den dağıtım sırasında oluşturabilirsiniz.

## <a name="front-end-role"></a>Ön uç rolü

**Önerilen en az**: Standart A1'in iki örneği

Ön uç yolları istekleri web çalışanı kullanılabilirliğine bağlı olarak web çalışanları için. Yüksek kullanılabilirlik için birden fazla ön uç olması gerekir ve ikiden daha fazla olabilir. Kapasite planlama için her çekirdek saniyede yaklaşık 100 isteği işleyebileceğini düşünün.

## <a name="management-role"></a>Yönetim rolü

**Önerilen en az**: Standart A3 iki örneği

App Service Azure Resource Manager ve API uç noktaları, portal Uzantıları (yönetici, Kiracı, işlevleri portalına) ve veri hizmeti için Azure App Service yönetim rolü sorumludur. Yönetim sunucusu rolü genellikle yalnızca hakkında bir üretim ortamında 4 GB RAM gerektirir. Ancak, çoğu yönetim görevi (örneğin, web sitesi oluşturma) gerçekleştirildiğinde yüksek CPU düzeyleri karşılaşabilirsiniz. Yüksek kullanılabilirlik için birden fazla sunucu bu role atanmış olması gerekir ve sunucu başına en az iki çekirdek.

## <a name="publisher-role"></a>Yayımcı rolü

**Önerilen en az**: Standart A1'in iki örneği

Birçok kullanıcının aynı anda yayımlıyorsa, yayımcı rolü aşırı CPU kullanımını karşılaşabilirsiniz. Yüksek kullanılabilirlik için birden fazla yayımcı rolünü kullanılabilir olduğundan emin olun. Yayımcı, FTP/FTPS trafiğinin yalnızca işler.

## <a name="web-worker-role"></a>Web çalışanı rolü

**Önerilen en az**: Standart A1'in iki örneği

Yüksek kullanılabilirlik için en az dört web çalışanı rolü, ikisi paylaşılan web sitesi modu için ve iki sunmayı planladığınız her adanmış çalışan katmanı için olmalıdır. İşlem paylaşılan ve ayrılmış modları, kiracılar için farklı hizmet düzeyleri sağlar. Pek çok müşteriniz varsa daha fazla web çalışanı gerekebilir:

- (Kaynak kullanımı yoğun olan) adanmış işlem modu çalışan katmanı kullanıyor.
- Paylaşılan işlem modunda çalışıyor.

Bu App Service planında belirtilen web worker(s) sayısı, artık bir kullanıcı için adanmış bir işlem moduna SKU bir App Service planı oluşturduktan sonra kullanıcılar için kullanılabilir.

Azure işlevleri tüketim planı modelinde kullanıcılara sağlamak için paylaşılan web çalışanı dağıtmanız gerekir.

Paylaşılan web çalışanı rolü sayısına kullanmaya karar verdiğinizde, bu konuları gözden geçirin:

- **Bellek**: Bellek bir web çalışanı rolü için en kritik kaynaktır. Yetersiz bellek, sanal bellek diskten değiştirildiğinde web sitesi performansını etkiler. Her bir sunucu işletim sistemi için yaklaşık 1,2 GB RAM gerektirir. Bu eşiğin üzerinde RAM, web sitelerini çalıştırmak için kullanılabilir.
- **Etkin web sitelerinin yüzdesi**: Genellikle, yaklaşık yüzde 5 ' Azure Stack dağıtım üzerinde bir Azure App Service'te uygulamaları etkindir. Ancak, yüzdesi, belirli bir anda etkin olan uygulamalar daha yüksek veya düşük olabilir. Bir uygulamanın oranı yüzde 5'lik ile Azure Stack dağıtım üzerinde bir Azure App Service'te yerleştirmek için uygulamaların sayısı, etkin web siteleri (5 x 20 = 100) sayısının 20 katından olmalıdır.
- **Ortalama bellek kaplama alanı**: Üretim ortamlarında gözlemlenen uygulamalar için ortalama bellek kaplama alanı yaklaşık 70 MB'dir. Bu Ayak izi kullanmak, tüm web çalışanı rolü bilgisayarları veya Vm'leri ayrılan bellek şu şekilde hesaplanabilir:

   `Number of provisioned applications * 70 MB * 5% - (number of web worker roles * 1044 MB)`

   Örneğin, 10 web çalışanı rolü çalıştıran bir ortamda 5.000 uygulamalar varsa, her web çalışanı rolü VM 7060 MB RAM olmalıdır:

   `5,000 * 70 * 0.05 – (10 * 1044) = 7060 (= about 7 GB)`

   Daha fazla çalışan örneğinden ekleme hakkında daha fazla bilgi için bkz: [daha fazla çalışanı rolü ekleme](azure-stack-app-service-add-worker-roles.md).

### <a name="additional-considerations-for-dedicated-workers-during-upgrade-and-maintenance"></a>Yükseltme ve bakım sırasında adanmış çalışanları için ek hususlar

Yükseltme ve çalışan bakımı sırasında Azure Stack'te Azure App Service üzerinde her çalışan katmanı %20 herhangi bir anda Bakımı gerçekleştirin.  Bu nedenle bulut yöneticileri her zaman % 20 havuzu başına çalışan katmanı ayrılmamış çalışan kiracıları hizmetinin herhangi bir kayıp yükseltme ve bakım sırasında yaşamamasını sağlamaya sürdürmeniz gerekir.  Örneğin, bir çalışan katmanı 10 çalışanları varsa 2 yükseltmeye izin ayrılmamış ve Bakım tam 10 çalışanları ayrılan olur, çalışan katmanı ayrılmamış çalışan havuzu sağlamak için ölçeği emin olmalısınız. Yükseltme ve bakım sırasında Azure App Service Kiracı İş yükü olası iş yükleri vardır ancak sırasında kullanılabilir ayrılmamış çalışanlar ardından var. yükseltme çalışmaya devam eder emin olmak için ayrılmamış çalışan iş yüklerini olacaktır taşınır kesinti süresi.  Paylaşılan çalışan bakımından service Kiracı uygulamaları kullanılabilir çalışanlar içinde otomatik olarak yoktur ancak yüksek kullanılabilirlik için en düşük gereksinimdir iki çalışanları için bu ayırır gibi ek çalışanları sağlamak müşterilerin gerekmez katmanı.

Bulut yöneticileri kendi çalışan katmanı ayırma Azure Stack yönetim portalındaki App Service Yönetim alanında izleyebilirsiniz.  App Service'e gidin ve ardından sol bölmedeki çalışan katmanları seçin.  Çalışan katmanları tablo çalışan katmanı adı, boyutu, kullanılan görüntüyü, (ayrılmamış), kullanılabilir çalışanlar çalışan her katmanında toplam sayısı ve çalışan katmanı genel durumu sayısını gösterir.

![Uygulama hizmet yönetimi - çalışan katmanları][1]

## <a name="file-server-role"></a>Dosya sunucusu rolü

Dosya sunucusu rolü için bir tek başına dosya sunucusu, geliştirme ve test için kullanabilirsiniz; Örneğin, Azure App Service Azure Stack geliştirme Seti'ni (ASDK) üzerinde dağıtırken bu kullanabileceğiniz [şablon](https://aka.ms/appsvconmasdkfstemplate).  Üretim için önceden yapılandırılmış bir Windows dosya sunucusu veya önceden yapılandırılmış Windows olmayan dosya sunucusu kullanmanız gerekir.

Üretim ortamlarında dosya sunucusu rolü yoğun disk g/ç ile karşılaşır. Tüm kullanıcı web siteleri için içerik ve uygulama dosyalarını barındırdığından bu rol için aşağıdaki kaynaklardan birini önceden yapılandırmanız gerekir:

- Windows dosya sunucusu
- Windows dosya sunucusu kümesi
- Windows olmayan dosya sunucusu
- Windows olmayan dosya sunucusu kümesi
- NAS (ağa bağlı depolama) cihaz

Daha fazla bilgi edinmek için şu makaleye bakın [bir dosya sunucusu sağlama](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için şu makaleye bakın:

[Azure Stack üzerinde App Service ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-capacity-planning/worker-tier-allocation.png