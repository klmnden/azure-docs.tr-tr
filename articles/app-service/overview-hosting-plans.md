---
title: App Service planı genel bakış - Azure | Microsoft Docs
description: Nasıl Azure App Service iş için App Service planları ve bunların yönetimi deneyiminizi nasıl avantaj öğrenin.
keywords: App service, azure app service, Ölçek, ölçeklenebilir, ölçeklenebilirlik, app service planı, app service maliyeti
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
ms.custom: seodec18
ms.openlocfilehash: ab04d1288eb3a851774128b8aaaae03868c2ffa7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60839020"
---
# <a name="azure-app-service-plan-overview"></a>Azure App Service planı genel bakış

App Service'teki uygulamalar bir _App Service planında_ çalışır. App Service planı, bir web uygulamasının birlikte çalıştırılacağı işlem kaynakları kümesini tanımlar. Bunlar alınmak üzere bilgi işlem kaynakları [ _sunucu grubu_ ](https://wikipedia.org/wiki/Server_farm) geleneksel web barındırma. Bir veya daha fazla uygulama, aynı işlem kaynakları (veya aynı App Service planı) çalıştırmak için yapılandırılabilir.

Bir App Service planı (örneğin, Batı Avrupa) belirli bir bölgede oluşturduğunuzda, bu bölgede Bu plan için bir dizi işlem kaynakları oluşturulur. App Service planınızı tarafından tanımlanan işlem kaynakları üzerinde çalıştırmak bu App Service planı içine yerleştirdiğiniz yükleyebileceğiniz tüm uygulamalar. Her bir App Service planı tanımlar:

- Bölge (Batı ABD, Doğu ABD, vb.)
- Sanal makine örneği sayısını
- VM örneklerinin (küçük, Orta, büyük)
- Fiyatlandırma Katmanı (ücretsiz, paylaşılan, temel, standart, Premium, PremiumV2, yalıtılmış, kullanımı)

_Fiyatlandırma katmanı_ bir App Service planı size hangi App Service özellikleri ve ne kadar plan için ödeme yaparsınız belirler. Fiyatlandırma katmanları birkaç kategorisi vardır:

- **İşlem paylaşılan**: **Ücretsiz** ve **paylaşılan**, iki temel katmanları, diğer müşterilerin uygulamaları dahil olmak üzere diğer App Service uygulamaları olarak aynı Azure sanal makinesinde bir uygulama çalıştırır. Bu katmanlar paylaşılan kaynaklar üzerinde çalışan her uygulama için CPU kotaları belirleyin ve kaynakların ölçeğini olamaz.
- **Ayrılmış işlem**: **Temel**, **standart**, **Premium**, ve **PremiumV2** katmanları uygulamaları ayrılmış Azure sanal makineler üzerinde çalıştırın. Yalnızca aynı App Service planı aynı işlem kaynaklarını paylaşıp. Daha yüksek bir katman, daha fazla sanal makine örneklerine ölçek genişletme için kullanılabilir.
- **Yalıtılmış**: Bu katman, ayrılmış Azure sanal ağlarda ağ yalıtımı üzerine uygulamalarınıza işlem yalıtım sağlayan, ayrılmış Azure VM'lerin çalıştırır. Bu, en fazla ölçeğini genişletme işlevlerini sağlar.
- **Tüketim**: Bu katman yalnızca kullanılabilir [işlev uygulamaları](../azure-functions/functions-overview.md). Bu işlevler iş yüküne bağlı olarak dinamik olarak ölçeklendirir. Daha fazla bilgi için [Azure işlevleri barındırma planları karşılaştırması](../azure-functions/functions-scale.md).

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Her katman aynı zamanda belirli bir alt kümesini App Service özellikleri sağlar. Bu özellikler, özel etki alanları ve SSL sertifikaları, otomatik ölçeklendirme, dağıtım yuvaları, yedeklemeler, Traffic Manager tümleştirmesi ve daha fazla bilgi içerir. Daha yüksek bir katman, daha fazla özelliği kullanılabilir. Her fiyatlandırma katmanında hangi özelliklerin desteklendiğine dair bulmak için bkz: [App Service planı ayrıntıları](https://azure.microsoft.com/pricing/details/app-service/plans/).

<a name="new-pricing-tier-premiumv2"></a>

> [!NOTE]
> Yeni **PremiumV2** fiyatlandırma katmanı sağlar [Dv2 serisi Vm'leri](../virtual-machines/windows/sizes-general.md#dv2-series) daha hızlı işlemcilere, SSD depolama ve Karşılaştırılan çift bellek / çekirdek oranı **standart** katmanı. **PremiumV2** ayrıca hala standart planda bulunan Gelişmiş özelliklerin tümünü sağlamanın yanı sıra artırılan örnek sayısı daha yüksek ölçeği destekler. Mevcut tüm özellikleri **Premium** katmanı dahil edilecek **PremiumV2**.
>
> Benzer şekilde diğer ayrılmış katmanları, üç VM boyutları için bu katmanı kullanılabilir:
>
> - Küçük (bir CPU çekirdek, 3,5 GiB bellek) 
> - Orta (iki CPU çekirdek, 7 GiB bellek) 
> - Büyük (dört CPU çekirdek, 14 GiB bellek)  
>
> İçin **PremiumV2** fiyatlandırma bilgileri bkz [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).
>
> Yeni kullanmaya başlamak için **PremiumV2** fiyatlandırma katmanı, bkz: [App Service için yapılandırma PremiumV2 katmanını](app-service-configure-premium-tier.md).

## <a name="how-does-my-app-run-and-scale"></a>Nasıl uygulamamı çalıştırma ölçeklendirin ve?

İçinde **ücretsiz** ve **paylaşılan** katmanları, uygulama bir paylaşılan VM örneğindeki CPU dakikası alır ve ölçeği olamaz. Diğer Katmanlar uygulama çalışır ve şu şekilde ölçeklendirir.

App Service'te bir uygulama oluşturduğunuzda, bir App Service planına koyulur. Uygulama çalıştırıldığında App Service planında yapılandırılmış tüm VM örnekleri üzerinde çalışır. Aynı App Service planında birden çok uygulamanız varsa bunların tümü aynı VM örneklerini paylaşın. Bir uygulama için birden çok dağıtım yuvası varsa, tüm dağıtım yuvalarını de aynı VM örneklerinde çalıştırın. Tanılama günlüklerini etkinleştirme, yedeklemeler veya Web işleri çalıştırmak, ayrıca CPU döngülerini ve bellek bu VM örneklerine kullanırlar.

Bu şekilde, App Service planı App Service uygulamaları ölçek birimidir. Plan, beş sanal makine örnekleri çalıştırmak için yapılandırılmışsa, plandaki tüm uygulamaları beş tüm örneklerinde çalıştırın. Tüm uygulamalarında planla birlikte ölçeği sonra planı için otomatik ölçeklendirme, yapılandırılması durumunda, otomatik ölçeklendirme ayarları temel alarak.

Bir uygulamayı kullanıma ölçeklendirme hakkında daha fazla bilgi için bkz: [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>App Service planımı nin ücreti ne kadardır?

Bu bölümde, App Service uygulamalarını nasıl faturalandırılır açıklanmaktadır. Ayrıntılı, bölgeye özgü fiyatlandırma bilgileri için bkz. [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).

Dışında **ücretsiz** katmanı, App Service planı izleme saatlik ücret üzerinden bilgi işlem kaynaklarını kullanır.

- İçinde **paylaşılan** her uygulama katmanı alır bir kota CPU dakikası, bu nedenle _her uygulama_ CPU kotasının saatlik olarak ücretlendirilir.
- Ayrılmış işlem katmanları (**temel**, **standart**, **Premium**, **PremiumV2**), App Service planı VM sayısını tanımlar uygulamalar için ölçeklenir örnekleri için _her sanal makine örneği_ App Service Planı saatlik bir ücret sahiptir. Bu sanal makine örnekleri aynı bakılmaksızın kaç uygulamalar üzerinde çalışan ücretlendirilir. Beklenmeyen ücretlerden kaçınmak için bkz: [temiz bir App Service planı](app-service-plan-manage.md#delete).
- İçinde **yalıtılmış** katmanı, App Service ortamı, uygulamalarınızı çalıştırmak yalıtılmış çalışanların sayısını tanımlar ve _her çalışan_ saatlik olarak ücretlendirilir. Ayrıca, çalıştırmak için saatlik bir taban ücreti yok App Service ortamı kendisi. 
- (Yalnızca azure işlevleri) **Tüketim** katmanı dinamik olarak ayıran bir işlev uygulamasının yükünü hizmet vermek için sanal makine örnekleri ve saniye başına Azure tarafından dinamik olarak ücretlendirilir. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/).

(Özel etki alanlarını, SSL sertifikaları, dağıtım yuvaları, yedeklemeler vb. yapılandırma.), kullanılabilir olan App Service özellikleri kullanmak için ücret yok. Özel durumlar şunlardır:

- Azure ve ne zaman, her yıl yenilemeniz birinde satın aldığınızda app Service etki alanları -, ödeme yaparsınız.
- Azure ve ne zaman, her yıl yenilemeniz birinde satın aldığınızda app Service sertifikaları -, ödeme yaparsınız.
- IP tabanlı SSL bağlantıları - burada kullanıcının her IP tabanlı SSL bağlantısı, ancak bazı için saatlik bir ücret **standart** katmanı veya üstü, size bir IP tabanlı SSL bağlantısı ücretsiz. SNI tabanlı SSL bağlantıları ücretsizdir.

> [!NOTE]
> Başka bir Azure hizmeti ile App Service'ı tümleştirirseniz diğer hizmetlerden bu ücretler göz önüne almanız gerekebilir. Örneğin, Azure Traffic uygulamanızı coğrafi olarak ölçeklemek için Manager Azure Traffic Manager ücretleri de kullanırsanız, kullanımınıza göre. Azure Hizmetleri arası maliyet tahmininde bulunmak için bkz: [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/). 
>
>

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>Ne özellik veya yeteneklerini uygulamam gerekiyor?

App Service planınızı, herhangi bir zamanda yukarı ve aşağı ölçeklendirilebilir. Planının fiyatlandırma katmanını değiştirmek kadar basittir. Başta daha düşük bir fiyatlandırma katmanı seçin ve daha sonra daha fazla App Service özellikleri gerektiğinde ölçeği.

Örneğin, web uygulamanızı test başlayabilirsiniz bir **ücretsiz** App Service planı ve hiçbir ödeme yapmazsınız. Eklemek istediğiniz zaman, [özel DNS adını](app-service-web-tutorial-custom-domain.md) web uygulamasına, yalnızca planınız'ın ölçeği **paylaşılan** katmanı. Daha sonra eklemek istediğiniz zaman bir [özel SSL sertifikası](app-service-web-tutorial-custom-ssl.md), planınızı kadar ölçeklendirme **temel** katmanı. Olmasını istediğinizde [hazırlık ortamları](deploy-staging-slots.md),'ın ölçeği **standart** katmanı. Daha fazla çekirdekleri, bellek veya depolamayla gerektiğinde daha büyük bir VM boyutu aynı katman içinde ölçeği artırabilirsiniz.

Aynı tersten çalışır. Bundan böyle düşündüğünüzü özelliklerini veya daha yüksek bir katmana özelliklerinin gerekir, paradan tasarruf daha düşük bir katmana için ölçeği azaltabilirsiniz.

App Service planınızın ölçeğini hakkında daha fazla bilgi için bkz: [azure'da uygulamanın ölçeğini](web-sites-scale.md).

Uygulamanızın diğer uygulamalarla aynı App Service planında ise, işlem kaynaklarını yalıtarak uygulamanın performansını artırmak isteyebilirsiniz. Ayrı bir App Service planına uygulama taşıyarak bunu yapabilirsiniz. Daha fazla bilgi için [başka bir App Service planına uygulama taşıma](app-service-plan-manage.md#move).

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>Bir uygulama yeni bir plan veya var olan bir planı koymanız gerekir?

App Service planınızı bilgi işlem kaynakları için ödeme beri ayırır (bkz [my App Service planı maliyeti ne kadar mu?](#cost)), birden fazla uygulama bir App Service planına yerleştirmeyi para potansiyel olarak kaydedebilirsiniz. Plan yükü işlemek için yeterli kaynağa sahip olduğu sürece, var olan bir planı uygulamalar eklemeye devam edebilirsiniz. Ancak, işlem kaynaklarını aynı herkes aynı App Service planındaki uygulamaları göz önünde bulundurun. Yeni uygulama gerekli kaynaklara sahip olup olmadığını belirlemek için yeni uygulama için var olan bir App Service planı ve beklenen yükü kapasitesini anlamak gerekir. Bir App Service planı aşırı yüklemesi, yeni ve mevcut uygulamalarınızın kapalı kalma süresi neden olabilir.

Uygulamanıza yeni bir App Service planı ayır:

- Kaynak Kullanımı Yoğun bir uygulamadır.
- Var olan bir planı olan diğer uygulamaların uygulamadan bağımsız olarak ölçeklemek istersiniz.
- Uygulamanın farklı bir coğrafi bölgede kaynak gerekir.

Böylece uygulamanız için yeni bir kaynak kümesini ayırmak ve uygulamalarınızın daha fazla denetim elde edebilirsiniz.

## <a name="manage-an-app-service-plan"></a>Bir App Service planı yönetme

> [!div class="nextstepaction"]
> [Bir App Service planı yönetme](app-service-plan-manage.md)
