---
title: Azure işlevleri Premium planı (Önizleme) | Microsoft Docs
description: Ayrıntıları ve Azure işlevleri Premium için yapılandırma seçenekleri (VNet, Hayır hazırlıksız başlatma, sınırsız yürütme süresi) planlayın.
services: functions
author: jeffhollan
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: jehollan
ms.openlocfilehash: d327146c4a1fa61e55bb904308038c1ce717123d
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543773"
---
# <a name="azure-functions-premium-plan-preview"></a>Azure işlevleri Premium planı (Önizleme)

Azure işlevleri Premium planı, işlev uygulamaları için barındırma bir seçenektir. Premium planı, VNet bağlantısı, hiçbir hazırlıksız başlatma ve premium donanım gibi özellikler sağlar.  Birden fazla işlev uygulaması için aynı Premium planı dağıtılabilir ve planı işlem örneği boyutu, temel plan boyutunu ve en fazla planı boyutu yapılandırmanıza olanak tanır.  Premium planı ve diğer plan ve türleri barındırma karşılaştırması için bkz. [ölçeklendirme ve barındırma seçenekleri işlevi](functions-scale.md).

> [!NOTE]
> Premium planı önizlemesi şu anda Windows altyapısı aracılığıyla, .NET, düğümü veya Java çalışan işlevleri destekler.

## <a name="create-a-premium-plan"></a>Bir Premium planı oluşturma

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Azure CLI üzerinden bir Premium planı da oluşturabilirsiniz

```azurecli-interactive
az functionapp plan create -g <resource-group> -n <plan-name> -l <region> --number-of-workers 1 --sku EP1
```

## <a name="features"></a>Özellikler

Aşağıdaki özellikleri, dağıtılan bir Premium plana işlev uygulamaları için kullanılabilir.

### <a name="pre-warmed-instances"></a>Önceden warmed örnekleri

Hiçbir olay ve yürütmeleri tüketim planında bugün meydana gelirse, sıfır örneklerine uygulamanız ölçeği azaltın. Yeni olaylar geldiğinizde, yeni bir örneği üzerinde çalışan uygulamanızda özel gerekir.  Özelleştirilmiş yeni örnekleri, uygulamaya bağlı olarak biraz zaman alabilir.  Bu ek gecikme ilk çağrıda genellikle uygulama hazırlıksız başlatma olarak adlandırılır.

Premium planı içinde belirtilen örnekleri, en az bir plan boyutunuzu en fazla sayıdaki önceden warmed uygulamanız olabilir.  Önceden warmed örnekleri yüksek yük önce bir uygulamayı önceden ölçek sağlar. Uygulama Ölçeklendirmesi eşitlenene gibi ilk önceden warmed örneklerine ölçeklendirir. Ek örnekleri kullanıma ve sonraki ölçeklendirme işlemi hemen hazırlığında sıcak arabellek geçin. Önceden warmed örneklerinin bir arabellek sağlayarak, soğuk başlangıç gecikmeleri etkili bir şekilde önleyebilirsiniz.  Önceden warmed örnekleri Premium planı, bir özelliğidir, çalışan en az bir örneğinin çalışır durumda bulundurmanıza gerek ve tüm saatler planı adresinde etkindir.

Azure portalında önceden warmed örneklerinin seçerek yapılandırabilirsiniz **ölçeği genişletme** içinde **Platform özellikleri** sekmesi.

![Esnek ölçek ayarları](./media/functions-premium-plan/scale-out.png)

Azure CLI ile bir uygulama için önceden warmed örnekleri de yapılandırabilirsiniz

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Özel ağ bağlantısı

Azure işlevleri için bir Premium planı dağıtılan yararlanır [web uygulamaları için yeni sanal ağ tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md#new-vnet-integration).  Yapılandırıldığında, uygulamanız, sanal ağ içindeki kaynaklarla iletişim kurabilir veya hizmet uç noktaları güvenli.  IP kısıtlamaları, uygulamanın gelen trafiği kısıtlamak için de kullanılabilir.

Bir alt ağ bir Premium planı işlev uygulamanızı atarken, olası her örneği için yeterli IP adreslerine sahip bir alt ağ gerekir. Önizleme sırasında en fazla örnek sayısı değişebilir ancak en az 100 kullanılabilir adresleri olan bir IP bloğu kılarız.

Daha fazla bilgi için bkz. [işlevi uygulamanızı bir sanal ağ ile tümleştirme](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Hızlı, esnek ölçeklendirme

Ek bilgi işlem örnekleri, uygulamanızın bir tüketim planı aynı hızlı ölçeklendirme mantığı kullanarak otomatik olarak eklenir.  Ölçeklendirme nasıl çalıştığı hakkında daha fazla bilgi edinmek için [işlev ölçeklendirme ve barındırma](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="unbounded-run-duration"></a>Unbounded çalıştırma süresi

Azure işlevleri tüketim planı içinde tek bir yürütme için 10 dakika sınırlıdır.  Premium planda çalıştırma süresi kaçan yürütmeleri önlemek için 30 dakika ile varsayılan olarak. Ancak, [host.json yapılandırmasını değiştirmek](./functions-host-json.md#functiontimeout) bu Premium planı uygulamalar için sınırsız yapma.

Önizleme aşamasında, süresi 12 dakika geçe garanti edilmez ve uygulamanızı, en az çalışan sayısı değil ölçeklenirse, 30 dakikadan uzun çalıştırmanın en iyi şansına sahip olabilirsiniz.

## <a name="plan-and-sku-settings"></a>Plan ve SKU ayarları

Bir plan oluşturduğunuzda, iki ayarlarını yapılandırın: en az sayıda örneklerinin (veya planı boyutu) ve en yüksek patlama sınırı.  Bir Premium planı için en düşük örnek 1'dir ve en yüksek patlama Önizleme sırasında 20'dir.  En düşük örnekleri, ayrılmış ve her zaman çalışır durumda.

> [!IMPORTANT]
> İçindeki en az örnek sayısı ne olursa olsun yürütülüyorsa işlevleri veya ayrılmış her örneği için ücretlendirilir.

Uygulamanızı plan boyutunuzu örnekle gerektiriyorsa, ölçeği genişletme örneklerinin en yüksek patlama sınırı sayısına ulaşana kadar devam edebilirsiniz.  Plan boyutunuzu örnekle ancak için faturalandırılırsınız çalışıyor ve siz kiralanmış.  Uygulamanız için en az bir plan örneği garanti edilir ancak uygulamanız için tanımlanan en yüksek sınırına ölçeklendirme en iyi yapacağız.

Planı boyutu yapılandırabilirsiniz ve tarafından Azure portalında üst sınırları seçilen **ölçeği genişletme** seçeneklerinde plana veya bu plan için Dağıtılmış bir işlev uygulaması (altında **Platform özellikleri**).

Ayrıca, Azure clı'dan en yüksek patlama sınırı artırabilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Kullanılabilir örnek SKU'ları

Planınızı ölçeklendirme oluştururken, üç örnek boyutları arasında seçim yapabilirsiniz.  Toplam çekirdek sayısını ve saniye başına tüketilen bellek miktarı için faturalandırılır.  Uygulamanızı otomatik olarak birden fazla örneğe gerektiği şekilde genişletebilir.  

|SKU|Çekirdek|Bellek|Depolama|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7GB|250 GB|
|EP3|4|14GB|250 GB|

## <a name="regions"></a>Bölgeler

Şu anda desteklenen bölgelerde genel Önizleme aşamasında aşağıdadır.

|Bölge|
|--|
|Avustralya Doğu|
|Avustralya Güneydoğu|
|Orta Kanada|
|Orta ABD|
|Doğu Asya|
|Doğu ABD 2|
|Fransa Orta|
|Japonya Batı|
|Kore Orta|
|Kuzey Avrupa|
|Orta Güney ABD|
|Güney Hindistan|
|Güneydoğu Asya|
|Birleşik Krallık Batı|
|Batı Avrupa|
|Batı Hindistan|
|Batı ABD|

## <a name="known-issues"></a>Bilinen Sorunlar

Bilinen sorunları durumunu izleyebilirsiniz [GitHub'te genel önizlemesi](https://github.com/Azure/Azure-Functions/wiki/Premium-plan-known-issues).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri'ni ölçeklendirme ve barındırma seçenekleri anlama](functions-scale.md)
