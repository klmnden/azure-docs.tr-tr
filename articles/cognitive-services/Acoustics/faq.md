---
title: Proje akustik sık sorulan sorular
titlesuffix: Azure Cognitive Services
description: Bu sayfayı içeren proje akustik hakkında sık sorulan soruların yanıtlarını yükleme yönergeleri ve hazırlama işlemi sağlar.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 6e979db29f4a223b61580c48101c0d242fdbebbf
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616327"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Proje akustik sık sorulan sorular

## <a name="what-is-project-acoustics"></a>Project Acoustics nedir?

Eklenti projesi akustik paketini ses wave davranışı önce çalışma zamanı statik aydınlatma yakındır hesaplayan bir akustik sistemidir. Çalışma zamanı CPU maliyeti düşük olacak şekilde cloud wave kaynaklanan ağır yüklerden fizik hesaplamalar yapar.  

## <a name="where-can-i-download-the-plugin"></a>Eklenti nereden indirebilirim?

İndirebileceğiniz [proje akustik Unity eklentisi](https://www.microsoft.com/download/details.aspx?id=57346) veya [proje akustik Unreal eklentisi](https://www.microsoft.com/download/details.aspx?id=58090).

## <a name="does-project-acoustics-support-ltxgt-platform"></a>Proje akustik desteklemiyor &lt;x&gt; platform?

Platform desteği geliştikçe akustik müşteri gereksinimlerine göre proje. Lütfen bize ulaşın [proje akustik forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics) desteği hakkında ek platformlar için sorgulamak için.

## <a name="is-azure-used-at-runtime"></a>Azure, çalışma zamanında kullanılır?

Hayır, bulut tümleştirmesi yalnızca precompute aşamasında Sahne kurulumunun bir parçası kullanılır.
 
## <a name="what-is-simulation-input"></a>Benzetim giriş nedir? 

3B Sahne, sanal ortama veya oyun düzeyi bir simülasyon giriş olabilir. Proje akustik kesintisiz kapatma ve saçılma dahil olmak üzere ses fizik yakın model 3B volumetric wave simülasyonlar gerçekleştirir.
 
## <a name="what-is-the-runtime-cost"></a>Çalışma zamanı maliyeti ne kadardır?

Akustik çerçeve başına kaynak başına yaklaşık CPU %0,01 alır. RAM kullanımını Sahne boyutuna bağlıdır ve 10'dan 100 MB arasında değişebilir.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Düzey geometri basitleştirmek gerekiyor mu? Denetim üçgen sayısı? Kafes watertight hale getirilsin mi?

Hayır. Sistem düzeyinde ayrıntılı geometri doğrudan alın. Bu, iç işleme voxelized olacaktır.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Çalışma zamanı arama tablosunda nedir?

ACE dosya içeren bir tablodur akustik parametreler arasında çok sayıda kaynak ve dinleyici konum çiftleri yanı voxelized Sahne geometriyi parametre enterpolasyon için kullanılır.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>Proje akustik taşıma kaynağını işleyebilir?

Evet, proje akustik arama tablosuna bakar ve kaynakları ve dinleyici taşıma işleyebilmesi her değer çizgisi üzerinde ses DSP güncelleştirir.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Proje akustik dinamik geometri işleyebilirsiniz? Kapılar kapatıyor? Hemen attı duvarlarının?

Hayır. Akustik statik bir oyun düzeyi durumuna bağlı filtrelerinde parametrelerdir. Kapı geometri akustik dışında bırakın ve ardından ek kapatma durumuna yıkıcı bağlı uygulama öneririz ve teknikleri kullanarak taşınabilir oyun nesne oluşturulur.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>Proje akustik akustik malzemeleri kullanıyor mu?

Evet. Malzemeleri absorptivity sürüş fiziksel malzeme adlarından sizin düzeyinizde olarak seçilir.
 
## <a name="what-do-the-probes-represent"></a>"Araştırmaları" ne temsil eder?

Araştırmalar olası player konumların bir örnekleme ' dir. Her araştırma bir araştırma konumda kaynaklanan Sahne ayrı wave benzetimi temsil eder. Çalışma zamanında, dinleyici konumu için akustik parametreleri yakındaki araştırma konumlardan piksele.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Neden bulutta çok fazla işlem harcama? Ne bana satın?

Proje akustik doğru ve güvenilir akustik parametreleri bile son derece karmaşık sanal ortamları için her mimari açıdan dikkate alarak sağlar. Bu, kesintisiz kapatma ve engel ve birimler çizim dinamik Yankı değişim el ile çalışma yapmadan sağlar. Tüm bunları yaparken kalan çalışma zamanı sırasında CPU üzerinde açık.

## <a name="what-exactly-happens-during-baking"></a>Tam olarak "saklanacağı sırasında" ne olacak?

Akustik wave benzetimleri cuboid benzetimi bölgelerin her dinleyici araştırma sırasında ortalanmış bir hazırlama oluşur.

## <a name="next-steps"></a>Sonraki adımlar
* Deneyin [proje akustik Unity örnek içerik](unity-quickstart.md) veya [Unreal örnek içerik](unreal-quickstart.md)

