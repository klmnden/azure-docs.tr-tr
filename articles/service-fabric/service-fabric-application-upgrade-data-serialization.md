---
title: 'Uygulama yükseltme: verileri serileştirme | Microsoft Docs'
description: Veri seri hale getirme ve uygulama yükseltmelerini etkilemesi için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 55cbd869e7434469ebddd7af493c91bfedafc594
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614438"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Veri seri hale getirme, bir uygulama yükseltmesi nasıl etkiler?
İçinde bir [sıralı yükseltme uygulama](service-fabric-application-upgrade.md), düğümlerinin bir alt kümesi, aynı anda bir yükseltme etki alanı yükseltme uygulanır. Bu işlem sırasında bazı yükseltme etki alanları, uygulamanızın daha yeni sürümüne ve bazı yükseltme etki alanlarında, uygulamanın eski sürümünü temel. Dağıtım sırasında uygulamanızın yeni sürümünü verilerinizi eski sürümü okuyabilir ve uygulamanızın eski sürümünü verilerinizi yeni sürümünü okuyabilir olmalıdır. Veri biçimi ileriye ve geriye dönük olarak uyumlu değilse, yükseltme başarısız olabilir veya daha da kötüsü, veriler kaybolursa veya bozulursa. Hangi veri biçiminiz oluşturan ve verilerinizin ileriye ve geriye doğru olmasını sağlamaya yönelik en iyi uygulamaları sunar anlatılmaktadır uyumlu.

## <a name="what-makes-up-your-data-format"></a>Veri biçiminiz yapan nedir?
Kalıcı ve çoğaltılan veriler, Azure Service Fabric gelir, C# sınıfları. Kullanan uygulamalar için [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md), kuyruklar ve güvenilir bir sözlük nesneleri verilerdir. Kullanan uygulamalar için [Reliable Actors](service-fabric-reliable-actors-introduction.md), yani bir aktör yedekleme durumu. Bunlar C# sınıfları kalıcı ve çoğaltılması için Serileştirilebilir olmalıdır. Bu nedenle, veri biçimi serileştirilme şeklini nasıl, de serileştirilme şeklini özellikleri ve alanları tarafından tanımlanır. Örneğin, bir `IReliableDictionary<int, MyClass>` seri hale getirilmiş bir veridir `int` ve seri hale getirilmiş bir `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Bir veri biçimi değişikliği ile sonuçlanan kod değişiklikleri
Veri biçimi tarafından belirlenen bu yana C# sınıfları sınıfları değişiklikleri neden olabilir bir veri biçimi değişikliği. Sıralı yükseltme veri biçimi değişikliği kaldırabileceğinden emin olmak için dikkatli olunması gerekir. Bu örnekler veri biçimi değişikliklere neden olabilir:

* Ekleme veya alanlar ve Özellikler'i kaldırma
* Alanlar ve Özellikler yeniden adlandırma
* Alanlar ve Özellikler türlerini değiştirme
* Sınıf adı veya ad alanı değiştirme

### <a name="data-contract-as-the-default-serializer"></a>Varsayılan serileştirici olarak veri anlaşması
Veri eski bir olsa bile seri hale getirici veri okumak ve geçerli sürüme seri durumdan çıkarmak için genellikle sorumlu olduğu veya *yeni* sürümü. Varsayılan serileştirici olan [veri sözleşmesi serileştiricisi](https://msdn.microsoft.com/library/ms733127.aspx), iyi tanımlanmış sürüm kuralları vardır. Güvenilir koleksiyonlar geçersiz kılınacak seri hale getirici izin ver, ancak Reliable Actors şu anda yapın. Veri serileştirici yükseltmeyi etkinleştirme önemli bir rol oynar. Veri sözleşmesi serileştiricisi için Service Fabric uygulamaları öneririz seri hale getirici ' dir.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Veri biçimi sıralı yükseltme nasıl etkiler?
Sıralı yükseltme sırasında burada seri hale getirici çalışılırsa, eski bir iki ana senaryo vardır veya *yeni* verilerinizin sürümü:

1. Bir düğüm yükseltilir ve yedeklemeyi başlattıktan sonra yeni seri hale getirici trvalý veri yükleyecek diske eski sürümü.
2. Küme sıralı yükseltmesi sırasında kodunuzu eski ve yeni sürümleri bir karışımını içerir. Çoğaltmaları farklı yükseltme etki alanlarında yer alabilir ve çoğaltmaları veri birbirlerine göndermek olduğundan, verilerinizin yeni ve/veya eski sürümü, seri hale getirici yeni ve/veya eski sürümü tarafından karşılaşılabilir.

> [!NOTE]
> "Eski sürümü" ve "yeni sürümü" Burada çalışan kod sürümüne bakın. "Yeni seri hale getirici", uygulamanızın yeni sürümünü içinde yürütülen seri hale getirici kod ifade eder. "Yeni veri" serileştirilmiş ifade eder C# uygulamanızın yeni sürümünü sınıfı.
> 
> 

İki kod ve veri biçimi sürümü hem ileriye hem geriye dönük uyumlu. Uyumlu değillerse, sıralı yükseltme başarısız olabilir veya veri kaybı olabilir. Başka bir sürüm ile karşılaştığında kod veya seri hale getirici özel durumlar veya bir hata oluşturabilecek sıralı yükseltme başarısız olabilir. Örneğin, yeni özellik eklenmiştir, ancak eski seri hale getirici seri durumundan çıkarma sırasında atar, veriler kaybolabilir.

Veri anlaşması verilerinizi uyumlu olmasını sağlamaya yönelik önerilen çözümdür. Ekleme, kaldırma ve alanları değiştirmek için iyi tanımlanmış sürüm kuralları var. Ayrıca, sınıf devralma ile ilgili bilinmeyen alanlarla ilgilenme ve Serileştirme ve seri durumundan çıkarma işlemine takma desteği bulunur. Daha fazla bilgi için [kullanarak veri anlaşması](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamayı kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltmesi size yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltmesi size yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Uygulamanızı yükseltilirken başvurarak gelişmiş işlevselliği kullanmanıza öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Yaygın sorunlar uygulama yükseltmeleri adımları başvurarak düzeltme [uygulama yükseltme sorunlarını giderme](service-fabric-application-upgrade-troubleshooting.md).

