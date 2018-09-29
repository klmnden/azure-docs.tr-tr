---
title: Akustik - Bilişsel hizmetler hakkında sık sorulan sorular
description: Bu sayfayı içeren proje akustik hakkında sık sorulan soruların yanıtlarını yükleme yönergeleri ve hazırlama işlemi sağlar.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: a71e867bd23cf64b2ac7fc8cd1c54c55d92ce924
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431797"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

## <a name="what-is-project-acoustics"></a>Project Acoustics nedir?

Proje akustik Unity eklentisi ses wave davranışı önce çalışma zamanı statik aydınlatma yakındır hesaplayan bir akustik sistemidir. Çalışma zamanı CPU maliyeti düşük olacak şekilde cloud wave kaynaklanan ağır yüklerden fizik hesaplamalar yapar.  

## <a name="where-can-i-download-the-plugin"></a>Eklenti nereden indirebilirim?

Akustik eklentisini değerlendirmek istiyorsanız [buraya](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) tıklayarak Tasarımcı Önizlemesine katılabilirsiniz.

## <a name="is-azure-used-at-runtime"></a>Azure, çalışma zamanında kullanılır?

Hayır, bulut tümleştirmesi yalnızca precompute aşamasında Sahne kurulumunun bir parçası kullanılır.
 
## <a name="what-is-simulation-input"></a>Benzetim giriş nedir? 

3B Sahne, sanal ortama veya oyun düzeyi bir simülasyon giriş olabilir. Proje akustik kesintisiz kapatma ve saçılma dahil olmak üzere ses fizik yakın model 3B volumetric wave simülasyonlar gerçekleştirir.
 
## <a name="what-is-the-runtime-cost"></a>Çalışma zamanı maliyeti ne kadardır?

Akustik çerçeve başına kaynak başına yaklaşık CPU %0,01 alır. RAM kullanımını Sahne boyutuna bağlıdır ve 10'dan 100 MB arasında değişebilir.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Düzey geometri basitleştirmek gerekiyor mu? Denetim üçgen sayısı? Kafes watertight hale getirilsin mi?

Hayır. Sistem düzeyinde ayrıntılı geometri doğrudan alın. Bu, iç işleme voxelized olacaktır.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Çalışma zamanı arama tablosunda nedir?

ACE dosyanın çok sayıda kaynak ve dinleyici konum çiftleri arasındaki akustik parametrelerinin bir tablodur.
 
## <a name="can-it-handle-moving-sources"></a>Kaynakları taşıma işleyebileceği?

Evet, **Microsoft Acoustics** Unity spatializer eklentisi her ses işleme değer çizgisi geçerli kaynak ve dinleyici konumlarıyla arama tablosunda danışır. Spatializer'ın DSP her değer çizgisi akustik işleme parametreler sorunsuz bir şekilde güncelleştirir.
 
## <a name="can-it-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Dinamik geometri işleyebileceği? Kapılar kapatıyor? Hemen attı duvarlarının?

Hayır. Akustik statik bir oyun düzeyi durumuna bağlı filtrelerinde parametrelerdir. Biz kapı geometri akustik dışında bırakmak önermek ve ardından kurulan teknikleri kullanarak yıkıcı ve taşınabilir oyun nesnelerine durumuna bağlı olarak ek kapatma uygulayın.
 
## <a name="does-it-handle-materials"></a>Bu malzemeleri işliyor?

Evet. Malzemeleri absorptivity sürüş fiziksel malzeme adlarından sizin düzeyinizde olarak seçilir.
 
## <a name="what-do-the-probes-represent"></a>"Araştırmaları" ne temsil eder?

Araştırmalar olası player konumların bir örnekleme ' dir. Her araştırma bir araştırma konumda kaynaklanan Sahne ayrı wave benzetimi temsil eder. Çalışma zamanında, dinleyici konumu için akustik parametreleri yakındaki araştırma konumlardan piksele.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Neden bulutta çok fazla işlem harcama? Ne bana satın?

Proje akustik doğru ve güvenilir akustik parametreleri bile son derece karmaşık sanal ortamları için her mimari açıdan dikkate alarak sağlar. Bu, kesintisiz kapatma/engel olmadan el ile tüm iş ve birimler çizim olmadan dinamik Yankı değişim sağlar. Tüm bunları yaparken kalan çalışma zamanı sırasında CPU üzerinde açık.

## <a name="what-exactly-happens-during-baking"></a>Tam olarak "saklanacağı sırasında" ne olacak?

Sistem "araştırma" Eşit aralıklı örnek konumları kümesini oluşturmak için olası player konumları göz önünde bulundurur. Her araştırma bağımsız görevlerde düzeyi için bir hazırlama oluşur: Sistem bir cuboid "Benzetimi araştırma sırasında ortalanmış bölge" olarak kabul eder ve bu bölgede en fazla 25 cm çözünürlükte ayrıntılı wave benzetimini yapar.

## <a name="next-steps"></a>Sonraki Adımlar
* Keşfedin [örnek Sahne](sample-walkthrough.md)

