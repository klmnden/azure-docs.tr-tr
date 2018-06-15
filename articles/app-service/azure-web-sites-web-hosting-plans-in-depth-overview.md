---
title: Azure uygulama hizmeti planı genel bakış | Microsoft Docs
description: Azure uygulama hizmeti iş için nasıl uygulama hizmeti planları ve bunların, yönetim deneyimi nasıl yararlı öğrenin.
keywords: uygulama hizmeti, azure app service, Ölçek, ölçeklenebilir, ölçeklenebilirlik, uygulama hizmeti planı, app service maliyeti
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 268844eae8dc06937529e79d52515cad2f6da3f4
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
ms.locfileid: "27862368"
---
# <a name="azure-app-service-plan-overview"></a>Azure uygulama hizmeti planı genel bakış

Bir uygulamayı App Service'te çalışan bir _uygulama hizmeti planı_. Bir uygulama hizmeti planı çalıştırmak bir web uygulaması için işlem kaynaklarını kümesini tanımlar. Bu kaynaklar benzer işlem [ _sunucu grubu_ ](https://wikipedia.org/wiki/Server_farm) geleneksel web barındırma. Bir veya daha fazla uygulama aynı bilgi işlem kaynakları (veya, aynı uygulama hizmeti planındaki) çalıştırmak için yapılandırılabilir.

(Örneğin, Batı Avrupa) belirli bir bölgede bir uygulama hizmeti planı oluşturduğunuzda, bir işlem kaynak grubu bu bölgede Bu plan için oluşturulur. Uygulama hizmeti planınızı tarafından tanımlandığı şekilde işlem kaynaklarını hangi uygulamaları, bunları çalıştırmak bu uygulama hizmeti planı yerleştirin. Her uygulama hizmeti planı tanımlar:

- Bölge (Batı ABD, Doğu ABD, vb.)
- VM örneği sayısı
- VM örneklerinin (küçük, Orta, büyük)
- Fiyatlandırma Katmanı (serbest, paylaşılan, temel, standart, Premium, PremiumV2, Isolated, kullanımı)

_Fiyatlandırma katmanı_ size hangi uygulama hizmeti özellikleri ve plan için ödeme ne kadar bir uygulama hizmeti planı belirler. Fiyatlandırma katmanlarını birkaç kategoriye ayrılır:

- **Paylaşılan işlem**: **serbest** ve **paylaşılan**, iki temel katmanları, bir uygulama aynı Azure VM diğer müşteri uygulamalar dahil olmak üzere diğer uygulama hizmeti uygulamalar gibi çalışır. Bu katmanlar CPU kotaları paylaşılan kaynakları üzerinde çalışan her uygulamanın ayırabilir ve kaynakları ölçeğini olamaz.
- **Adanmış bir işlem**: **temel**, **standart**, **Premium**, ve **PremiumV2** katmanları ayrılmış Azure üzerinde uygulamaları çalıştırma VM'ler. Yalnızca aynı uygulama hizmeti planındaki uygulamaları aynı işlem kaynakları paylaşır. Daha yüksek katman daha fazla VM örnekleri genişleme için kullanılabilir.
- **Yalıtılmış**: Bu katmanı ayrılmış Azure sanal ağlarda ağ yalıtımı işlem yalıtım uygulamalarınızı en üstünde sağlayan, ayrılmış Azure sanal makineleri çalıştırır. En büyük genişletme yetenekleri sağlar.
- **Tüketim**: Bu katmanda yalnızca kullanılabilir [işlev uygulamaları](../azure-functions/functions-overview.md). İşlevlerini iş yükü dinamik olarak bağlı olarak ölçeklendirir. Daha fazla bilgi için bkz: [Azure işlevleri barındırma planları karşılaştırma](../azure-functions/functions-scale.md).

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Her katman, belirli bir alt kümesini App Service özellikleri de sağlar. Bu özellikler, özel etki alanları ve SSL sertifikaları, otomatik ölçeklendirmeyi, dağıtım yuvaları, yedeklemeler, trafik Yöneticisi tümleştirme ve daha fazla bilgi içerir. Daha yüksek katman daha fazla özelliği kullanılabilir. Her fiyatlandırma katmanının hangi özelliklerin desteklendiği bulmak için bkz: [uygulama hizmeti plan ayrıntılarını](https://azure.microsoft.com/pricing/details/app-service/plans/).

<a name="new-pricing-tier-premiumv2"></a>

> [!NOTE]
> Yeni **PremiumV2** fiyatlandırma katmanı sağlar [Dv2-serisi VM'ler](../virtual-machines/windows/sizes-general.md#dv2-series) daha hızlı işlemcilerde, SSD depolama ve Karşılaştırılan çift bellek çekirdek oranı ile **standart** katmanı. **PremiumV2** hala standart plana bulunan tüm gelişmiş özellikleri sağlarken daha yüksek ölçek artan örnek sayısı üzerinden de destekler. Varolan kullanılabilen tüm özellikleri **Premium** katmanı dahil edilmiştir **PremiumV2**.
>
> Benzer şekilde diğer ayrılmış katmanları, üç VM boyutları için bu katmanı kullanılabilir:
>
> - Küçük (bir CPU çekirdek, 3.5 Gib'den bellek) 
> - Orta (iki CPU çekirdek, 7 Gib'den bellek) 
> - Büyük (dört CPU çekirdek, 14 Gib'den bellek)  
>
> İçin **PremiumV2** fiyatlandırma bilgileri, bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).
>
> Yeni ile çalışmaya başlamak için **PremiumV2** fiyatlandırma katmanı, bkz: [uygulama hizmetin yapılandırma PremiumV2 katmanı](app-service-configure-premium-tier.md).

## <a name="how-does-my-app-run-and-scale"></a>Nasıl Uygulamam, Ölçek çalıştırmak ve?

İçinde **serbest** ve **paylaşılan** katmanları, bir uygulama CPU dakika paylaşılan VM örneği üzerinde alır ve ölçeğini olamaz. Diğer katmanlarında uygulama çalışır ve şu şekilde ölçeklendirir.

App Service içinde bir uygulama oluşturduğunuzda, bir uygulama hizmeti planı konur. Uygulama çalıştırıldığında, uygulama hizmeti planında yapılandırılmış tüm VM örnekleri üzerinde çalışır. Birden çok uygulama aynı uygulama hizmeti planında varsa, bunların tümü aynı VM örnekleri paylaşır. Bir uygulama için birden çok dağıtım yuvası varsa, tüm dağıtım yuvalarını de aynı VM örnekleri üzerinde çalıştırın. Tanılama günlüklerini etkinleştirme, yedeklemeler gerçekleştirmek veya Web işleri çalıştırmak, ayrıca CPU döngülerini ve bellek bu VM örneklerinde kullanırlar.

Bu şekilde, uygulama hizmeti planı App Service uygulamalarının ölçek birimidir. Plan beş VM örnekleri çalıştırmak için yapılandırılmışsa, plandaki tüm uygulamaları tüm beş örneklerinde çalıştırın. Plandaki tüm uygulamaları çıkışı birlikte ölçeklenir sonra planı otomatik ölçeklendirmeyi için yapılandırılmışsa, otomatik ölçeklendirme ayarlara göre.

Bir uygulama ölçeklendirme hakkında daha fazla bilgi için bkz [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>Nasıl my uygulama hizmeti planı maliyeti nedir?

Bu bölümde, App Service uygulamalarının nasıl faturalandırılır açıklanmaktadır. Ayrıntılı, bölgeye özgü fiyatlandırma bilgileri için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

Dışında **serbest** katmanı, bir uygulama hizmeti planı taşıyan saatlik bir ücret işlem kaynakları kullanır.

- İçinde **paylaşılan** katmanı, her uygulama alır kota CPU dakika, bu nedenle _her uygulama_ saatlik CPU kotasına doludur.
- Ayrılmış katmanları işlem (**temel**, **standart**, **Premium**, **PremiumV2**), uygulama hizmeti planı tanımlar VM sayısı uygulamalar için ölçeklenir örnekleri için _her VM örneği_ App Service'te bir saatlik ücret planı vardır. Bu VM örnekleri aynı bakılmaksızın kaç uygulamaları bunlar üzerinde çalışan sizden ücret kesilir. Beklenmeyen ücretlerden kaçınmak için bkz: [bir uygulama hizmeti planı temiz](app-service-plan-manage.md#delete).
- İçinde **Isolated** katmanı, uygulama hizmeti ortamı tanımlar, uygulamalarınızı çalıştırmak yalıtılmış çalışan sayısı ve _her çalışan_ saatlik doludur. Ayrıca, çalıştırmak için saatlik bir temel ücret yoktur uygulama hizmeti ortamı kendisi. 
- (Yalnızca azure işlevleri) **Tüketim** katmanı dinamik olarak VM örnekleri işlevi uygulamanın iş yükü hizmet ayırır ve saniyede Azure tarafından dinamik olarak ücretlendirilir. Daha fazla bilgi için bkz: [Azure işlevleri fiyatlandırma](https://azure.microsoft.com/pricing/details/functions/).

(Özel etki alanlarını, SSL sertifikaları, dağıtım yuvaları, yedeklemeler vb. yapılandırılıyor.) kullanılabilen App Service özellikleri kullanmak için ücret yok. Özel durumlar şunlardır:

- Azure ve ne zaman, her yıl yenilemeniz birinde satın aldığınızda app Service etki alanları -, ücret ödersiniz.
- Azure ve ne zaman, her yıl yenilemeniz birinde satın aldığınızda uygulama hizmeti sertifikaları -, ücret ödersiniz.
- IP tabanlı SSL bağlantılarını - orada kullanıcının her IP tabanlı SSL bağlantısı, ancak bazı saatlik bir ücret alınmaz **standart** katmanı veya yukarıdaki size bir IP tabanlı SSL bağlantısı ücretsiz. SNI tabanlı SSL bağlantılarını ücretsizdir.

> [!NOTE]
> Uygulama hizmeti başka bir Azure hizmeti ile tümleştirirseniz, diğer hizmetlerin ücretlerden dikkate almanız gerekebilir. Örneğin, Azure trafik uygulamanızı coğrafi olarak ölçeklendirmek için Yöneticisi Azure Traffic Manager ücretleri de kullanıyorsanız, kullanımınıza bağlı. Azure arası Hizmetleri maliyetini tahmin etmek için bkz: [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/). 
>
>

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>Ne Uygulamam özellik veya yeteneklerini gerekiyor?

Uygulama hizmeti planınızı yukarı ve aşağı herhangi bir zamanda genişletilebilir. Planının fiyatlandırma katmanı değiştirilirse olarak kadar basittir. İlk daha düşük bir fiyatlandırma katmanı seçin ve daha fazla App Service özellikleri gerektiğinde daha sonra ölçeği.

Örneğin, web uygulamanızı test etme başlatabilir bir **serbest** uygulama hizmeti planı ve hiçbir şey ödeme. Eklemek istediğiniz zaman, [özel DNS ad](app-service-web-tutorial-custom-domain.md) web uygulaması için yalnızca planınızı kadar ölçeklendirin **paylaşılan** katmanı. Daha sonra eklemek istediğiniz zaman bir [özel SSL sertifikası](app-service-web-tutorial-custom-ssl.md), planınızı kadar ölçeklendirin **temel** katmanı. Olmasını istediğinizde [hazırlık ortamları](web-sites-staged-publishing.md), en fazla ölçek **standart** katmanı. Daha fazla çekirdekleri, bellek veya depolamayla gerektiğinde, daha büyük bir VM boyutuyla aynı katmanındaki ölçeklendirme.

Aynı geriye doğru çalışır. Artık eşitleyerek daha yüksek bir katmanı özellikleri ve yetenekleri gerekir,, tasarruf bir alt katmanına ölçeklendirebilirsiniz.

Uygulama hizmeti planı ölçeklendirme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).

Uygulamanızı diğer uygulamalarla aynı uygulama hizmeti planındaki ise, işlem kaynaklarını yalıtarak uygulamanın performansını artırmak isteyebilirsiniz. Bu uygulamayı ayrı bir uygulama hizmeti planına taşıyarak yapabilirsiniz. Daha fazla bilgi için bkz: [bir uygulamayı başka bir uygulama hizmeti planına taşıma](app-service-plan-manage.md#move).

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>Bir uygulamayı yeni bir plan veya var olan bir planı koyabilir?

Uygulama hizmeti planınız için bilgi işlem kaynaklarını ödeme beri ayırır (bkz [ne kadar my uygulama hizmeti planı maliyet mu?](#cost)), bir uygulama hizmeti planına birden fazla uygulama koyarak para potansiyel olarak kaydedebilirsiniz. Plan yükünü işlemek için yeterli kaynağa sahip olduğu sürece var olan bir planı uygulamaları eklemeye devam edebilirsiniz. Ancak, işlem kaynaklarını aynı herkes aynı uygulama hizmeti planındaki uygulamaları göz önünde bulundurun. Yeni uygulama gerekli kaynaklara sahip olup olmadığını belirlemek için var olan uygulama hizmeti planı beklenen yükü ve kapasite yeni uygulama için anlamanız gerekir. Bir uygulama hizmeti planı aşırı yüklemesi, yeni ve mevcut uygulamalar için kapalı kalma süresi neden olabilir.

Yeni bir uygulama hizmeti uygulamanıza planlama ayır:

- Yoğun bir kaynak uygulamasıdır.
- Varolan bir planı bağımsız olarak diğer uygulamalardan uygulama ölçeklendirme istiyorsunuz.
- Uygulamanın farklı bir coğrafi bölgede kaynak gerekir.

Böylece uygulamanız için yeni bir kaynak kümesini ayırmak ve uygulamalarınızın daha fazla denetim kazanmak.

## <a name="manage-an-app-service-plan"></a>Bir uygulama hizmeti planı yönetme

> [!div class="nextstepaction"]
> [Bir uygulama hizmeti planı yönetme](app-service-plan-manage.md)
