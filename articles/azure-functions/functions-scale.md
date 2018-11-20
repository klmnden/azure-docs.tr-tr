---
title: Azure işlevleri'ni ölçeklendirme ve barındırma | Microsoft Docs
description: Azure işlevleri tüketim planı ve App Service planı arasında seçim yapma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, tüketim planı, app service planı, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfdd0c647021c453095ec4e05c042992011389b9
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51975899"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure işlevlerini ölçeklendirme ve barındırma

Azure işlevleri iki farklı modda çalışır: Tüketim planı ve Azure App Service planı. Kodunuzu çalıştırırken tüketim planı otomatik olarak bilgi işlem gücü ayırır. Uygulamanız, gerektiğinde yükü işlemek için ölçeği ve kod çalışmadığı zamanlarda ölçeği. Boş Vm'leri için kullandıkları kadar ödemeyi veya yedek kapasite önceden gerekmez.

> [!NOTE]  
> [Linux barındırma](functions-create-first-azure-function-azure-cli-linux.md) şu anda yalnızca bir App Service planı üzerinde kullanılabilir.

Azure işlevleri ile ilgili bilgi sahibi değilseniz bkz [Azure işlevlerine genel bakış](functions-overview.md).

Bir işlev uygulaması oluşturduğunuzda uygulama barındırma planı işlevleri için seçin. Ya da planı örneği *Azure işlevleri konak* işlevleri yürütür. Plan denetimleri türü:

* Barındırma örnekleri kullanıma nasıl ölçeklendirilir.
* Her konak için kullanılabilir kaynaklar.

> [!IMPORTANT]
> İşlev uygulaması oluşturma sırasında barındırma planı türünü seçmeniz gerekir. Daha sonra değiştiremezsiniz.

Bir App Service planında, farklı kaynaklarının miktarını ayırmak için Katmanlar arasında ölçeklendirebilirsiniz. Azure işlevleri, tüketim planında, tüm kaynak ayırma otomatik olarak işler. 

## <a name="consumption-plan"></a>Tüketim planı

Bir tüketim planı kullanırken, Azure işlevleri konak örneklerini dinamik olarak eklenir ve gelen olayların sayısına dayalı kaldırıldı. Bu sunucusuz planı otomatik olarak ölçeklenen ve yalnızca işlevlerinizin çalıştırırken işlem kaynakları için ücretlendirilirsiniz. Bir tüketim planında bir işlev yürütmeye yapılandırılabilir bir süre sonunda zaman aşımına uğradı.

> [!NOTE]
> Tüketim planlarındaki işlevler için varsayılan zaman aşımı, 5 dakikadır. Değer en fazla 10 dakika kadar işlev uygulaması için özelliği değiştirilerek artırılabilir `functionTimeout` içinde [host.json](functions-host-json.md#functiontimeout) proje dosyası.

Faturalandırma, yürütme, yürütme süresini ve kullanılan bellek sayısına göre belirlenmektedir. Faturalandırma, bir işlev uygulaması içindeki tüm işlevleri üzerinden toplanır. Daha fazla bilgi için [Azure işlevleri fiyatlandırması sayfası].

Tüketim planı barındırma planı varsayılandır ve aşağıdaki avantajları sunar:

* Yalnızca işlevlerinizin çalışırken için ödeme yaparsınız.
* Otomatik ölçeklendirme, hatta yüksek dönemleri sırasında yük.

## <a name="app-service-plan"></a>App Service planı

Adanmış App Service planında işlev uygulamalarınızı ayrılmış sanal makineler üzerinde temel, standart, Premium ve yalıtılmış SKU'ları, diğer App Service uygulamalarını aynı olduğu çalıştırın. Özel VM'ler işlevleri konak olabilir anlamına gelir, işlev uygulaması için ayrılan [her zaman çalışır durumda](#always-on). App Service planları Linux desteği.

Aşağıdaki durumlarda bir App Service planı göz önünde bulundurun:

* Diğer App Service örneği zaten çalışıyor var olan ve az kullanılan sanal makine var.
* İşlev uygulamalarınızı sürekli olarak veya sürekli olarak neredeyse çalıştırın. Bu durumda, bir App Service planı daha uygun maliyetli olabilir.
* Tüketim planı üzerinde sağlanan değerinden daha fazla CPU veya bellek seçenekleri ihtiyacınız vardır.
* Kodunuzu 10 dakikaya kadar olan tüketim planında, izin verilen en uzun yürütme süresi uzun çalıştırılmamalıdır.
* App Service planı, App Service ortamı VNET/VPN bağlantısı ve büyük VM boyutları için destek gibi şirket yalnızca kullanılabilen özellikleri gerektirir.
* Linux üzerinde işlev uygulamanızı çalıştırmak istediğiniz veya özel bir görüntüyü işlevlerinizi çalıştırılacağı istiyorsunuz.

Bir VM sayısını yürütme, yürütme süresini ve kullanılan bellek maliyetinden ayırır. Sonuç olarak, kullandığınız en fazla tahsis VM örneği maliyetini ödeme olmaz. App Service planı nasıl çalıştığı hakkında daha fazla ayrıntı için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Bir App Service planı, el ile daha fazla VM örneği ekleyerek genişletebilir veya otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/monitoring-autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Ayrıca farklı bir App Service planı seçerek ölçeğini artırabilirsiniz. Daha fazla bilgi için [azure'da uygulamanın ölçeğini](../app-service/web-sites-scale.md). 

JavaScript işlevleri bir App Service planı üzerinde çalışırken, daha az Vcpu olan bir planı seçmeniz gerekir. Daha fazla bilgi için [seçin tek çekirdekli App Service planları](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### <a name="always-on"></a>Her Zaman Açık

Bir App Service planı üzerinde çalıştırırsanız, etkinleştirmelisiniz **her zaman** işlev uygulamanızın düzgün çalıştığını ayarını. Yalnızca HTTP Tetikleyicileri "işlevlerinizi uyandır şekilde" bir App Service planı üzerinde işlevler çalışma zamanı boşta kalma birkaç dakika sonra gider. Her zaman üzerinde yalnızca bir App Service planı üzerinde kullanılabilir. Bir tüketim planında, platform işlev uygulamaları otomatik olarak etkinleştirir.

## <a name="what-is-my-hosting-plan"></a>Barındırma planı kullandığımı nedir

İşlev uygulamanız tarafından kullanılan bir barındırma planı belirlemek için bkz: **App Service planı / fiyatlandırma katmanı** içinde **genel bakış** işlev uygulamasına için sekmesinde [Azure portalında](https://portal.azure.com). App Service planları için fiyatlandırma katmanını da gösterilir. 

![Ölçeklendirme planı portalda görüntüleme](./media/functions-scale/function-app-overview-portal.png)

Azure CLI, planı, şu şekilde belirlemek için de kullanabilirsiniz:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Bu komutun çıktısı olduğunda `dynamic`, tüketim planında, işlev uygulamasının aynısıdır. Diğer tüm değerler bir App Service planı katmanı gösterir.

Hatta Always On özellikli ile tek tek işlevler için kullanılacak yürütme zaman aşımı tarafından denetlenir `functionTimeout` ayarı [host.json](functions-host-json.md#functiontimeout) proje dosyası.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Tüketim planı ya bir App Service planı üzerinde bir işlev uygulaması, Azure Blob, kuyruk, dosya ve tablo Depolama'yı destekleyen genel bir Azure depolama hesabı gerektirir. İşlevler Tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Azure depolama alanında kullanır, ancak bazı depolama hesapları kuyrukları ve tabloları desteklemez nedeni budur. (Premium depolama dahil) yalnızca blob depolama hesapları ve bölgesel olarak yedekli depolama çoğaltması ile genel amaçlı depolama hesapları dahil, bu hesaplar filtrelenmiş varolan genişletme **depolama hesabı** bir işlev uygulaması oluşturduğunuzda seçim.

<!-- JH: Does using a Premium Storage account improve perf? -->

Depolama hesabı türleri hakkında daha fazla bilgi için bkz: [Azure depolama hizmetlerine giriş](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Tüketim planı nasıl çalışır?

Tüketim planında ölçek denetleyicisi otomatik olarak CPU ve bellek kaynakları işlevleri konağının işlevleri üzerinde tetiklenen olayların sayısına dayalı olarak ek örnekleri ekleyerek ölçeklendirir. Her bir örneğini işlevleri konak 1,5 GB bellekle sınırlıdır.  Konak bir örneği ve ölçek içinde bir işlev uygulaması paylaşımı kaynaktaki tüm İşlevler, aynı anda anlamına gelir, bir işlev uygulaması örneğidir. Tüketim planı paylaşan işlev uygulamaları birbirinden bağımsız olarak ölçeklenir.  

Tüketim barındırma planını kullandığınızda, işlev kod dosyaları işlevin ana depolama hesabındaki Azure dosya paylaşımlarını depolanır. İşlev uygulamasının ana depolama hesabını sildiğinizde, işlev kod dosyaları silinir ve kurtarılamaz.

> [!NOTE]
> Blob tetikleyicisi bir tüketim planında kullanırken, olabilir bir 10 dakikaya kadar yeni BLOB'ları işleme. Bu gecikme, bir işlev uygulaması boşta geçti oluşur. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Bu soğuk başlangıç gecikmeyi önlemek için bir App Service planıyla kullanmak **Always On** etkin veya Event Grid tetikleyicisinin kullanın. Daha fazla bilgi için [blob tetikleyicisi bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Çalışma zamanı ölçeklendirme

Azure işlevleri kullanan adlı bir bileşen *ölçek denetleyicisi* olaylarının hızı izlemek ve ölçeği veya ölçeklendirme verilip verilmeyeceğine karar vermek için. Ölçek denetleyicisi her tetikleyici türü için buluşsal yöntemler kullanır. Örneğin, bir Azure kuyruk depolama tetikleyicisi kullanırken, kuyruk uzunluğu ve eski kuyruk iletisi yaşını göre ölçeklendirir.

İşlev uygulaması ölçek birimidir. İşlev uygulaması ölçeği, Azure işlevleri ana bilgisayarın birden çok örneğini çalıştırmak için ek kaynaklar ayrılır. Buna karşılık, isteğe bağlı olarak sınırlı bilgi işlem gibi ölçek denetleyicisi işlevi konak örneklerini kaldırır. Hiçbir işlevler bir işlev uygulaması içinde çalışırken örneklerinin sonunda sıfır olarak ölçeklendirilir.

![Ölçek denetleyicisi örnekleri oluşturma ve izleme olayları](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Ölçeklendirme davranışlarını anlama

Ölçeklendirme faktörleri ve farklı şekilde tetikleyici ve seçili dil göre ölçeği sayısına farklılık gösterebilir. Ancak, ölçeklendirme, birkaç unsur vardır sistemde bugün mevcut:

* Tek bir işlev uygulaması yalnızca en fazla 200 örnek için ölçeklendirilebilir. Hiç için eş zamanlı yürütme sayısı bir set sınır tek bir örneği birden fazla ileti veya isteği aynı anda yine de işleyebilir.
* Yeni örnekleri yalnızca 10 saniyede en fazla bir kez ayrılır.

Ayrıca farklı ölçeklendirme limitleri yanı sıra aşağıda belgelenmiş farklı tetikleyicilere sahip olabilir:

* [Olay Hub’ı](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>En iyi yöntemler ve ölçeklenebilir uygulamaları için desenler

Birçok yönden ne kadar iyi Bu, ana bilgisayar yapılandırması, çalışma zamanı Ayak izi ve kaynak verimliliği de dahil olmak üzere ölçeklendirecek etkileyen bir işlev uygulaması yok.  Daha fazla bilgi için [performans değerlendirmeleri makalede ölçeklenebilirlik bölümünü](functions-best-practices.md#scalability-best-practices). Bağlantılar, işlev uygulaması ölçekler nasıl davranacağını farkında olmalıdır. Daha fazla bilgi için [Azure işlevleri'nde bağlantılarını yönetme](manage-connections.md).

### <a name="billing-model"></a>Faturalandırma modeli

Tüketim planı üzerinde ayrıntılı olarak açıklanan için faturalama [Azure işlevleri fiyatlandırması sayfası]. Kullanım işlevi uygulama düzeyinde toplanır ve işlev kodunu yürütülen zaman sayar. Faturalandırma birimler şunlardır:

* **Kaynak tüketimi, gigabayt saniye (GB-s) cinsinden**. Bellek boyutu ve yürütme zamanı içinde bir işlev uygulaması tüm işlevler için bir birleşimi olarak hesaplanır. 
* **Yürütme**. Bir işlev, yanıt olarak bir olay tetikleyicisi yürütülür her zaman sayılır.

[Azure işlevleri fiyatlandırması sayfası]: https://azure.microsoft.com/pricing/details/functions
