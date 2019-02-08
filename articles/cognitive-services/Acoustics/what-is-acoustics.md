---
title: Project Acoustics nedir?
titlesuffix: Azure Cognitive Services
description: Project Acoustics Unity eklentisi, VR ve geleneksel ekranları hedefleyen projeler için kapatma, yansıma ve mekansallaştırma sunar.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 8305eca478854eeff29268a86e4e49b697261ca2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868268"
---
# <a name="what-is-project-acoustics"></a>Project Acoustics nedir?
Project Acoustics Unity eklentisi, VR ve geleneksel ekranları hedefleyen projeler için kapatma, yansıma ve mekansallaştırma sunar. Oyun akustiği tasarımı için tasarımcı amaçlarını fizik tabanlı dalga benzetimi üzerine bir katman olarak yerleştiren bir yöntem sunar.

## <a name="why-use-acoustics-in-virtual-environments"></a>Sanal ortamlarda neden akustik kullanılması gerekir?
İnsanlar etraflarında olan biteni anlamak için sesli ve görsel ipuçlarından faydalanır. Sanal dünyalarda uzamsal sesin akustikle birleştirilmesi sonucunda kullanıcı için derinliğin artırılması sağlanır. Burada anlatılan akustik aracı sanal dünyaları analiz ederek gerçekçi bir akustik benzetimi oluşturur ve benzetim sonrası tasarım işlemini destekler. Analiz, sanal dünyadaki tüm yüzeyler için hem geometri hem de malzeme bilgilerini içerir. Simülasyon varış yönü (portal oluşturma), yankı gücü, dağılma türleri, kapatma ve engelleme efektleri gibi parametreler içerir.

## <a name="how-does-this-approach-to-acoustics-work"></a>Bu akustik yaklaşımı nasıl çalışır?
Sistem, sanal dünyada çevrimdışı hesaplama yaparak çalışma zamanında gerçekleştirilen analizden daha karmaşık bir benzetim sağlar. Çevrimdışı işlem, akustik parametrelerden oluşan bir arama tablosu üretir. Tasarımcı, parametrelere çalışma zamanında uygulanan kuralları belirtir. Bu kuralların ayarlanmasıyla yüksek duygusal yoğunluk için üstün gerçekçiliğe sahip efektler veya daha fazla arka plan sesi için üstün gerçekçiliğe sahip sahneler oluşturulabilir.

## <a name="design-process-comparison"></a>Tasarım işlemi karşılaştırması
Project Acoustics eklentisi, Unity sahnelerinde akustik için yeni bir tasarım sürecini destekler. Bu yeni tasarım sürecini açıklamak için günümüzün popüler akustik yaklaşımlarından biriyle karşılaştıralım.

### <a name="typical-approach-to-acoustics-today"></a>Günümüzün tipik akustik yaklaşımı
Günümüzün tipik akustik yaklaşımında yankı düzeylerini çizersiniz:

![Tasarım Görünümü](media/reverbZonesAltSPace2.png)

Ardından her bölge için parametreleri ayarlarsınız:

![Tasarım Görünümü](media/TooManyReverbParameters.png)

Son olarak sahnede doğru kapatma ve engelleme filtrelemesine ulaşmak için ışın izleme mantığını, portal oluşturmak için de yol arama mantığı eklersiniz. Bu kod çalışma zamanında ek maliyet yaratabilir. Aynı zamanda köşelerde düzgünlük ve düzensiz şekle sahip sahnelerde kenar sorunlarına neden olur.

### <a name="an-alternative-approach-with-physics-based-design"></a>Fizik tabanlı tasarımla alternatif yaklaşım
Project Acoustics Unity eklentisi ile sunulan yaklaşım sayesinde statik sahnenin şeklini ve malzemelerini belirtirsiniz. Sahne voksalleştirildiğinden ve süreçte ışın izleme kullanılmadığından basitleştirilmiş veya sıkı bir akustik ağ sağlamanız gerekmez. Sahnede ses yankılama işaretlemeleri de yapmanız gerekmez. Eklenti sahneyi buluta yükler ve fizik tabanlı dalga benzetimi kullanır. Sonuç, projenize arama tablosu olarak tümleştirilir ve estetik veya oyun efekti amacıyla değiştirilebilir.

![Tasarım Görünümü](media/GearsWithVoxels.jpg)

## <a name="requirements"></a>Gereksinimler
* Akustik işlemler için Unity 2018.2+, ses tasarım ve dağıtım için Unity 5.2+
* Windows 64 bit Unity Düzenleyicisi
* Akustik işlemleri için Azure Batch aboneliği
* Unity betik çalışma zamanı '.NET 4.x eşdeğeri' olarak ayarlanmalıdır

## <a name="platform-support"></a>Platform desteği
* Windows masaüstü (x86 ve AMD64)
* Windows UWP (x86, AMD64 ve ARM)
* Android (x86 ve ARM64)

## <a name="download"></a>İndirme
Akustik eklentisini değerlendirmek istiyorsanız [buraya](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) tıklayarak Tasarımcı Önizlemesine katılabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Tasarım süreci](design-process.md) hakkında daha fazla bilgi edinin
* [Unity projeniz için akustik tümleştirmesine](getting-started.md) başlayın

