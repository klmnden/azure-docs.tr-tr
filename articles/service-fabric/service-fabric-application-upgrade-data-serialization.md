---
title: 'Uygulama yükseltme: veri seri hale getirme | Microsoft Docs'
description: Veri seri hale getirme ve uygulama yükseltmelerini nasıl etkilediği için en iyi uygulamalar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 2f6fad0ecca09ff9210b5961301fea3446a88f11
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Veri seri hale getirme uygulama yükseltme nasıl etkiler
İçinde bir [uygulama yükseltmesinde](service-fabric-application-upgrade.md), alt düğümler, aynı anda bir yükseltme etki alanı yükseltme uygulanır. Bu işlem sırasında bazı yükseltme etki alanlarının uygulamanızı sürümü, ve bazı yükseltme etki alanları, uygulamanızın eski sürümünde. Piyasaya çıkma sırasında uygulamanın yeni sürümü verilerinizin eski sürümü okuyabilir ve uygulamanızın eski sürümünü verilerinizin yeni sürümü okuyabilir. Veri biçimi ileriye ve geriye dönük olarak uyumlu değilse, yükseltme başarısız olabilir ya da kötüsü, veriler kaybolmuş veya bozulmuş. Hangi veri biçimi oluşturduğunu ve verilerinizi ileriye ve geriye doğru olduğundan emin olmanın en iyi yöntemleri sunar bu makalede ele uyumlu.

## <a name="what-makes-up-your-data-format"></a>Veri biçiminiz ne yapar?
Azure Service Fabric içinde kalıcı ve çoğaltılan veriler, C# sınıflardan gelir. Kullanan uygulamalar için [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md), kuyruklar ve güvenilir sözlükler nesneleri verilerdir. Kullanan uygulamalar için [Reliable Actors](service-fabric-reliable-actors-introduction.md), yani aktör yedekleme durumu. Bu C# sınıfları kalıcı ve çoğaltılması için Serileştirilebilir olmalıdır. Bu nedenle, veri biçimi alanlar ve bunlar serileştirilir nasıl, de seri duruma özellikleri tarafından tanımlanır. Örneğin, bir `IReliableDictionary<int, MyClass>` seri hale getirilmiş bir veridir `int` ve seri hale getirilmiş bir `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Veri biçimi değişiklik sonucunda ortaya çıkan kod değişiklikleri
Veri biçimi C# sınıfları tarafından belirlenir. bu yana değişiklikler sınıfları için bir veri biçimi değişmesine neden olabilir. Bir yükseltme veri biçimi değişikliği işleyebildiğinden emin dikkatli olunması gerekir. Veri biçimi değişiklik neden olabilir Örnekler:

* Ekleyerek veya kaldırarak alanları veya özellikleri
* Alanlar ve Özellikler yeniden adlandırma
* Alanlar ve Özellikler türlerini değiştirme
* Sınıf adı veya ad alanı değiştirme

### <a name="data-contract-as-the-default-serializer"></a>Varsayılan serileştirici olarak veri sözleşmesi
Verileri eski bir olsa bile seri hale getirici veri okumak ve geçerli sürüme, seri durumdan çıkarmak için genellikle sorumlu olduğu veya *yeni* sürümü. Varsayılan serileştirici olan [veri sözleşmesi seri hale getirici](https://msdn.microsoft.com/library/ms733127.aspx), iyi tanımlanmış sürüm kuralları vardır. Geçersiz kılınacak seri hale getirici güvenilir koleksiyonları izin ancak Reliable Actors şu anda yapın. Veri serileştirici yükseltmeyi etkinleştirme önemli bir rol oynar. Veri sözleşmesi seri hale getirici için Service Fabric uygulamaları öneririz seri hale getirici ' dir.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Veri biçimi çalışırken yükseltme nasıl etkiler
Çalışırken yükseltme sırasında burada seri hale getirici karşılaşabilir eski bir iki ana senaryo vardır veya *yeni* verilerinizin sürümü:

1. Bir düğüm yükseltilir ve yedekleme başladıktan sonra yeni seri hale getirici kalıcı veri yük diske eski sürümü.
2. Çalışırken yükseltme sırasında küme kodunuzu eski ve yeni sürümleri bir karışımını içerir. Çoğaltmaları farklı yükseltme etki alanlarında yerleştirilebilir ve çoğaltmaları veri birbirlerine göndermek için verilerinizin yeni ve/veya eski sürümü, seri hale getirici yeni ve/veya eski sürümü tarafından karşılaşılabilir.

> [!NOTE]
> "Yeni sürümü" ve "eski sürümü" Buraya çalıştıran kodunuzu sürümüne bakın. "Yeni seri hale getirici" uygulamanızı yeni sürümde yürütüyor seri hale getirici kod ifade eder. "Yeni veri" gelen yeni sürümü, uygulamanızın serileştirilmiş C# sınıfına başvuruyor.
> 
> 

Kod ve veri biçimi iki sürümü ileriye ve geriye doğru olmalıdır uyumlu. Uyumlu değillerse, yükseltme başarısız olabilir veya veri kaybı olabilir. Başka bir sürüm ile karşılaştığında kod veya seri hale getirici özel durumları veya bir hata çünkü yükseltme başarısız olabilir. Örneğin, yeni bir özellik eklendi ancak isteğe bağlı olarak eski seri hale getirici seri durumdan çıkarma sırasında atar, veriler kaybolabilir.

Veri sözleşmesi verilerinizi uyumlu olduğundan emin olmanın için önerilen çözümdür. Ekleme, kaldırma ve alanları değiştirme için iyi tanımlanmış sürüm kuralları vardır. Ayrıca, bilinmeyen alanlarla ilgilenme, seri hale getirme ve seri durumdan çıkarma işlemine takma ve sınıf devralma ile ilgilenen desteğe sahiptir. Daha fazla bilgi için bkz: [kullanarak veri sözleşmesi](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama kullanarak Visual Studio yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme yol gösterir.

[Uygulama Powershell kullanarak yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) PowerShell kullanarak bir uygulama yükseltme yol gösterir.

Uygulamanızı kullanarak yükseltme nasıl kontrol [yükseltme parametreleri](service-fabric-application-upgrade-parameters.md).

Gelişmiş işlevselliği başvurarak uygulamanızı yükseltirken kullanmayı öğrenin [Gelişmiş konular](service-fabric-application-upgrade-advanced.md).

Adımlarına bakarak uygulama yükseltmeleri sık karşılaşılan sorunları düzeltmek [sorun giderme uygulama yükseltmelerini ](service-fabric-application-upgrade-troubleshooting.md).

