---
title: Azure işlevleri'ni ölçeklendirme ve barındırma | Microsoft Docs
description: Azure işlevleri tüketim planı ile Premium planı arasında seçim yapma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, tüketim planı, premium planı, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/27/2019
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3253cc7e379ae63880d533f14bc76e7af5a4425a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050564"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure işlevlerini ölçeklendirme ve barındırma

Azure'da bir işlev uygulaması oluşturduğunuzda, uygulamanız için bir barındırma planı seçmeniz gerekir. Azure işlevleri için üç barındırma planlarını vardır: [Tüketim planı](#consumption-plan), [Premium planı](#premium-plan), ve [App Service planı](#app-service-plan).

Seçtiğiniz barındırma planına aşağıdaki davranışları belirler:

* İşlev uygulamanızı nasıl ölçeklendirilir.
* Her işlev uygulaması örneği için kullanılabilir kaynaklar.
* Sanal ağ bağlantısı gibi gelişmiş özellikleri için destek.

Kodunuzu çalıştırırken hem tüketim hem de Premium planlar, bilgi işlem gücü otomatik olarak ekleyin. Uygulamanız, gerektiğinde yükü işlemek için ölçeği ve kod çalışmayı durdurduğunda ölçeği. Tüketim planı için boşta sanal makineler için ödeme veya önceden yedek kapasite yok.  

Premium işlem örnekleri, örnekleri süresiz olarak sıcak tutmak olanağı ve sanal ağa bağlantı gibi premium planı, ek özellikler sağlar.

App Service planı, yönettiğiniz adanmış altyapınızdan yararlanmanızı sağlar. İşlev uygulamanızı ölçeklendirilemez olaylara göre hangi hiçbir zaman sıfıra ölçekler yoludur. (Gerektiren [her zaman](#always-on) etkindir.)

> [!NOTE]
> İşlev uygulaması kaynak plan özelliğini değiştirerek tüketim ve Premium planlar arasında geçiş yapabilirsiniz.

## <a name="hosting-plan-support"></a>Barındırma planı desteği

Özellik desteği, aşağıdaki iki kategoride döner:

* _Genel kullanıma (GA)_ : tam olarak desteklenen ve üretim kullanımı için onaylandı.
* _Önizleme_: değil henüz tam olarak desteklenen ve üretim kullanımı için onaylandı.

Aşağıdaki tabloda, Windows veya Linux'ta çalışan üç barındırma planı için destek geçerli düzeyini gösterir:

| | Tüketim planı | Premium planı | Özel planı |
|-|:----------------:|:------------:|:----------------:|
| Windows | GA | önizleme | GA |
| Linux | önizleme | Yok | GA |

## <a name="consumption-plan"></a>Tüketim planı

Tüketim planı kullanırken, Azure işlevleri konak örneklerini dinamik olarak eklenir ve gelen olayların sayısına dayalı kaldırıldı. Bu sunucusuz planı otomatik olarak ölçeklenen ve yalnızca işlevlerinizin çalıştırırken işlem kaynakları için ücretlendirilirsiniz. Bir tüketim planında bir işlev yürütmeye yapılandırılabilir bir süre sonunda zaman aşımına uğradı.

Faturalandırma, yürütme, yürütme süresini ve kullanılan bellek sayısına göre belirlenmektedir. Faturalandırma, bir işlev uygulaması içindeki tüm işlevleri üzerinden toplanır. Daha fazla bilgi için [Azure fiyatlandırma sayfasını işlevleri](https://azure.microsoft.com/pricing/details/functions/).

Tüketim planı barındırma planı varsayılandır ve aşağıdaki avantajları sunar:

* Yalnızca işlevlerinizin çalışırken ödeme yapın
* Otomatik ölçeklendirme, hatta yüksek dönemleri sırasında yük

İşlev uygulamaları aynı bölgede aynı tüketim planı olarak atanabilir. Dezavantajı veya birden çok uygulama aynı tüketim planında çalıştırmayı sahip etkilemeden yoktur. Aynı tüketim planı için birden fazla uygulama atama, dayanıklılık, ölçeklenebilirlik veya her uygulama güvenilirliğini herhangi bir etkisi yoktur.

## <a name="premium-plan"></a>Premium planı (Önizleme)

Premium planı kullanırken, Azure işlevleri konak örneklerini eklenmiş ve kaldırılmış tüketim planı gibi gelen olay sayısı temel alınarak.  Premium planı, aşağıdaki özellikleri destekler:

* Tüm hazırlıksız başlatma önlemek için sürekli olarak sıcak örnekleri
* Sanal ağa bağlantı
* Sınırsız yürütme süresi
* Premium örnek boyutları (tek çekirdek, iki çekirdekli ve dört çekirdek örnekleri)
* Daha tahmin edilebilir fiyatlandırma
* Birden fazla işlev uygulaması ile planları için yüksek yoğunluklu uygulama ayırma

Bu seçenekler nasıl yapılandırabileceğiniz hakkında daha fazla bilgi bulunabilir [Azure işlevleri premium planı belge](functions-premium-plan.md).

Yürütme ve tüketilen bellek başına faturalama yerine için Premium planı, yürütme süresi ve gerekli ve ayrılmış örnekler kullanılan bellek ve çekirdek saniye sayısı göre faturalandırılır.  En az bir örnek sıcak at, her zaman olmalıdır. Bu, yürütme sayısı ne olursa olsun, etkin bir plan başına sabit bir aylık maliyete olduğu anlamına gelir.

Azure işlevleri premium planı aşağıdaki durumlarda göz önünde bulundurun:

* İşlev uygulamalarınızı sürekli olarak veya sürekli olarak neredeyse çalıştırın.
* Tüketim planı tarafından sağlanan değerinden daha fazla CPU veya bellek seçenekleri ihtiyacınız vardır.
* Kodunuzu daha uzun çalıştırması gereken [izin verilen en uzun yürütme süresi](#timeout) tüketim planı üzerinde.
* VNET/VPN bağlantısı gibi bir Premium planı üzerinde yalnızca kullanılabilen özellikleri gerektirir.

> [!NOTE]
> Premium planı önizlemesi şu anda yalnızca Azure işlevleri üzerinde Windows desteklemektedir.

JavaScript işlevleri bir Premium planı üzerinde çalışırken, daha az Vcpu olan örneği seçmeniz gerekir. Daha fazla bilgi için [tek çekirdekli Premium planı seçin](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Özel (App Service) planı

İşlev uygulamalarınızı aynı adanmış VM'ler (temel, standart, Premium ve yalıtılmış SKU'ları) diğer App Service uygulamaları olarak da çalıştırabilirsiniz.

Aşağıdaki durumlarda bir App Service planı göz önünde bulundurun:

* Diğer App Service örneği zaten çalışıyor var olan ve az kullanılan sanal makine var.
* İşlevlerinizi çalıştırmak üzere özel bir görüntü sunmak istiyorsunuz.

Web apps gibi diğer App Service kaynakları için yaptığınız gibi aynı bir App Service planı içinde işlev uygulamaları için ücret ödersiniz. App Service planı nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/overview-hosting-plans.md).

Bir App Service planıyla el ile daha fazla VM örneği ekleyerek ölçeği genişletebilirsiniz. Otomatik ölçeklendirme de etkinleştirebilirsiniz. Daha fazla bilgi için [örnek sayısını elle veya otomatik olarak ölçeklendirme](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Ayrıca farklı bir App Service planı seçerek ölçeğini artırabilirsiniz. Daha fazla bilgi için [azure'da uygulamanın ölçeğini](../app-service/web-sites-scale.md). 

JavaScript işlevleri bir App Service planı üzerinde çalışırken, daha az Vcpu olan bir planı seçmeniz gerekir. Daha fazla bilgi için [seçin tek çekirdekli App Service planları](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Her zaman açık

Bir App Service planı üzerinde çalıştırırsanız, etkinleştirmelisiniz **her zaman** işlev uygulamanızın düzgün çalıştığını ayarını. Yalnızca HTTP Tetikleyicileri "işlevlerinizi uyandır şekilde" bir App Service planı üzerinde işlevler çalışma zamanı boşta kalma birkaç dakika sonra gider. Her zaman üzerinde yalnızca bir App Service planı üzerinde kullanılabilir. Bir tüketim planında, platform işlev uygulamaları otomatik olarak etkinleştirir.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Hatta Always On özellikli ile tek tek işlevler için kullanılacak yürütme zaman aşımı tarafından denetlenir `functionTimeout` ayarı [host.json](functions-host-json.md#functiontimeout) proje dosyası.

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Var olan bir uygulama barındırma planı belirleme

İşlev uygulamanız tarafından kullanılan bir barındırma planı belirlemek için bkz: **App Service planı / fiyatlandırma katmanı** içinde **genel bakış** işlev uygulamasına için sekmesinde [Azure portalında](https://portal.azure.com). App Service planları için fiyatlandırma katmanını da gösterilir.

![Ölçeklendirme planı portalda görüntüleme](./media/functions-scale/function-app-overview-portal.png)

Azure CLI, planı, şu şekilde belirlemek için de kullanabilirsiniz:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Bu komutun çıktısı olduğunda `dynamic`, tüketim planında, işlev uygulamasının aynısıdır. Bu komutun çıktısı olduğunda `ElasticPremium`, işlev uygulamanızı Premium plandır. Diğer tüm değerler, bir App Service planı farklı katmanlarda gösterir.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Herhangi bir plan üzerinde bir işlev uygulaması, Azure Blob, kuyruk, dosya ve tablo depolamayı destekleyen genel bir Azure depolama hesabı gerektirir. İşlevler Tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Azure depolama alanında kullanan, ancak bazı depolama hesapları kuyrukları ve tabloları desteklemez nedeni budur. (Premium depolama dahil) yalnızca blob depolama hesapları ve bölgesel olarak yedekli depolama çoğaltması ile genel amaçlı depolama hesapları dahil, bu hesaplar filtrelenmiş varolan genişletme **depolama hesabı** bir işlev uygulaması oluşturduğunuzda seçim.

<!-- JH: Does using a Premium Storage account improve perf? -->

Depolama hesabı türleri hakkında daha fazla bilgi için bkz: [Azure depolama hizmetlerine giriş](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>Tüketim ve premium planları nasıl çalışır

Tüketim ve premium planları, Azure işlevleri altyapı işlevleri konağının işlevleri üzerinde tetiklenen olayların sayısına dayalı olarak ek örnekleri ekleyerek CPU ve bellek kaynakları ölçeklendirir. Tüketim planı işlevleri ana bilgisayarda her örneği bir CPU ve bellek 1,5 GB ile sınırlıdır.  Aynı anda bir örneği ve ölçek içinde bir işlev uygulaması paylaşımı kaynaktaki tüm işlevleri anlamı olan tüm işlev uygulaması, konak örneğidir. Tüketim planı paylaşan işlev uygulamaları birbirinden bağımsız olarak ölçeklenir.  Premium planına plan boyutunuzu kullanılabilir bellek ve CPU tüm uygulamalar için bu plandaki örneğine belirler.  

İşlev kod dosyaları, işlevin ana depolama hesabındaki Azure dosya paylaşımları üzerinde depolanır. İşlev uygulamasının ana depolama hesabını sildiğinizde, işlev kod dosyaları silinir ve kurtarılamaz.

> [!NOTE]
> Blob tetikleyicisi bir tüketim planında kullanırken, olabilir bir 10 dakikaya kadar yeni BLOB'ları işleme. Bu gecikme, bir işlev uygulaması boşta geçti oluşur. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Bu soğuk başlangıç gecikmeyi önlemek için Premium planı kullanın veya kullanmak [Event Grid tetikleyicisinin](functions-bindings-event-grid.md). Daha fazla bilgi için [blob tetikleyicisi bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Çalışma zamanı ölçeklendirme

Azure işlevleri kullanan adlı bir bileşen *ölçek denetleyicisi* olaylarının hızı izlemek ve ölçeği veya ölçeklendirme verilip verilmeyeceğine karar vermek için. Ölçek denetleyicisi her tetikleyici türü için buluşsal yöntemler kullanır. Örneğin, bir Azure kuyruk depolama tetikleyicisi kullanırken, kuyruk uzunluğu ve eski kuyruk iletisi yaşını göre ölçeklendirir.

İşlev uygulaması için Azure işlevleri ölçek birimidir. İşlev uygulaması ölçeği, Azure işlevleri ana bilgisayarın birden çok örneğini çalıştırmak için ek kaynaklar ayrılır. Buna karşılık, isteğe bağlı olarak sınırlı bilgi işlem gibi ölçek denetleyicisi işlevi konak örneklerini kaldırır. Hiçbir işlevler bir işlev uygulaması içinde çalışırken örneklerinin sonunda sıfır olarak ölçeklendirilir.

![Ölçek denetleyicisi örnekleri oluşturma ve izleme olayları](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Ölçeklendirme davranışlarını anlama

Ölçeklendirme faktörleri ve farklı şekilde tetikleyici ve seçili dil göre ölçeği sayısına farklılık gösterebilir. Ayrıntılı olarak, ölçekleme davranışı, dikkat edilmesi gereken birkaç incelenmektedir vardır:

* Tek bir işlev uygulaması en fazla 200 örneğe ölçeklendirilebilir. Hiç için eş zamanlı yürütme sayısı bir set sınır tek bir örneği birden fazla ileti veya isteği aynı anda yine de işleyebilir.
* HTTP Tetikleyicileri için yeni örnekleri yalnızca 1 saniyede en fazla bir kez ayrılır.
* HTTP olmayan Tetikleyiciler için yeni örnekleri yalnızca her 30 saniyede en fazla bir kez ayrılır.

Ayrıca farklı ölçeklendirme limitleri yanı sıra aşağıda belgelenmiş farklı tetikleyicilere sahip olabilir:

* [Olay Hub’ı](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>En iyi yöntemler ve ölçeklenebilir uygulamaları için desenler

Birçok yönden ne kadar iyi Bu, ana bilgisayar yapılandırması, çalışma zamanı Ayak izi ve kaynak verimliliği de dahil olmak üzere ölçeklendirecek etkileyen bir işlev uygulaması yok.  Daha fazla bilgi için [performans değerlendirmeleri makalede ölçeklenebilirlik bölümünü](functions-best-practices.md#scalability-best-practices). Bağlantılar, işlev uygulaması ölçekler nasıl davranacağını farkında olmalıdır. Daha fazla bilgi için [Azure işlevleri'nde bağlantılarını yönetme](manage-connections.md).

### <a name="billing-model"></a>Faturalandırma modeli

Farklı planları için faturalandırma hakkında ayrıntılı olarak açıklanmıştır [Azure fiyatlandırma sayfasını işlevleri](https://azure.microsoft.com/pricing/details/functions/). Kullanım işlevi uygulama düzeyinde toplanır ve işlev kodunu yürütülen zaman sayar. Faturalandırma birimler şunlardır:

* **Kaynak tüketimi, gigabayt saniye (GB-s) cinsinden**. Bellek boyutu ve yürütme zamanı içinde bir işlev uygulaması tüm işlevler için bir birleşimi olarak hesaplanır. 
* **Yürütme**. Bir işlev, yanıt olarak bir olay tetikleyicisi yürütülür her zaman sayılır.

Kullanışlı sorgular ve tüketim faturanızı anlama konusunda bilgi bulunabilir [fatura SSS](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Hizmet sınırlamaları

Aşağıdaki tabloda çeşitli barındırma planlarında çalıştırırken işlev uygulamaları için geçerli olan limitler gösterir:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]
