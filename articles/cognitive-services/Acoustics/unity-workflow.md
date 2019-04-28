---
title: Proje akustik Unity tasarım Öğreticisi
titlesuffix: Azure Cognitive Services
description: Bu öğretici için Unity proje akustik tasarım iş akışını açıklar.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 01783aa12f586f61583b1503c796f9b523770104
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61433095"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Proje akustik Unity tasarım Öğreticisi
Bu öğreticide, Unity projesi akustik için Tasarım araçları ve iş akışı açıklanır.

Ön koşullar:
* Unity 2018.2 + Windows için
* Oluşturulan akustik varlık ile Unity Sahne

Bu öğreticide, bir iki yolla oluşturulan akustik varlık ile Unity Sahne alabilirsiniz:
* [Unity projeniz için proje akustik ekleme](unity-integration.md), ardından [bir Azure Batch hesabı edinin](create-azure-account.md), ardından [Unity sahneniz hazırlama](unity-baking.md)
* Ya da kullanmak [proje akustik Unity örnek içerik](unity-quickstart.md).

## <a name="review-design-process-concepts"></a>Tasarım işlemi kavramları gözden geçirin
Proje akustik, kaynakları ve kapatma ve ıslak/kuru karışımı reverberation kuyruk uzunluğu (RT60) dahil olmak üzere alışık olduğunuz akustik özellikler üzerinde denetim sağlar işlemek için ortak ses dijital sinyal işleme (DSP) yöntemlerini kullanır. Ancak temel [proje akustik tasarım işlemi kavramı](design-process.md) yerine doğrudan bu özellikleri ayarlama, simülasyon sonuçlarını sürücü bu özellikleri için nasıl kullanılacağını denetleme. Her denetim için varsayılan ayarlar, fiziksel olarak doğru akustik temsil eder.

## <a name="design-acoustics-for-each-source"></a>Her kaynak için tasarım akustik
Proje akustik birkaç kaynak özel akustik tasarım denetimleri sağlar. Bu görünümde karışımı bazı kaynakları vurgulama ve diğer bir yinelenenleri emphasizing denetlemenize olanak tanır.

### <a name="adjust-distance-based-attenuation"></a>Uzaklık tabanlı zayıflama Ayarla
Ses DSP sağladığı **proje akustik** Unity spatializer eklentisinin Unity düzenleyicide yerleşik olarak bulunan kaynak başına uzaklık tabanlı zayıflama dikkate alır. Denetimler için uzaklık tabanlı zayıflama bulunduğunuz **ses kaynak** bileşeni bulunan **denetçisi** altında ses panelinden kaynakları **3B ses ayarları**:

![Ekran görüntüsü, Unity uzaklık zayıflama Seçenekleri paneli](media/distance-attenuation.png)

Akustik player konum etrafındaki ortalanmış bir "benzetimi bölge" kutusu hesaplama gerçekleştirir. Bir ses kaynak Player'dan bu simülasyon bölge dışında bulunan uzak ise yalnızca geometri kutusundaki occluders kapsamına player olduğunda epey iyi çalışan (örneğin, kapatma neden) ses yayılmasını etkiler. Ancak, durumlarda player boş bir alanı ancak occluders uzak ses kaynak sesi unrealistically disoccluded haline. Bizim önerilen çözüm Böyle durumlarda ses zayıflama 0 yaklaşık 45 m, kutusunun Player'a yatay uzaklığını varsayılan olarak olduğundan emin olun olabilir.

![Unity SpeakerMode ekran seçenek bölmesi](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Kapatma ve iletim ayarlama
İliştirme **AcousticsAdjust** betik bir kaynak için bu kaynak için ayar parametreleri sağlar. Komut dosyası eklemek için tıklatın **Bileşen Ekle** alt kısmındaki **denetçisi** gidin ve panel **betikleri > akustik ayarlamak**. Betik altı denetimlerine sahiptir:

![Unity AcousticsAdjust ekran komut dosyası](media/acoustics-adjust.png)

* **Akustik etkinleştirme** -bu kaynak için akustik uygulanıp uygulanmayacağını denetler. İşaretlenmediğinde, kaynak HRTFs veya kaydırma spatialized ancak hiçbir akustik olacaktır. Bu, hiçbir engel, kapatma veya dinamik reverberation parametreleri düzeyi ve decay zamanı gibi anlamına gelir. Reverberation yine de bir sabit düzeyi ve decay saat ile uygulanır.
* **Kapatma** -çarpanı akustik sistem tarafından hesaplanan kapatma dB düzeyinde uygulanır. Bu çarpan 1'den büyükse, kapatma exaggerated değerleri 1'den küçük kapatma etkisini daha hafif olun ve kapatma 0 değerini devre dışı bırakır.
* **İletim (dB)** -geometri iletim kaynaklanan zayıflama (veritabanında) ayarlayın. Bu kaydırıcı iletimini devre dışı bırakmak için en düşük düzeyine ayarlayın. Akustik Sahne geometri (portaling) geçici olarak gelen olarak ilk kuru ses spatializes. İletim satırı görüş yönde spatialized ek bir kuru varış sağlar. Uzaklık zayıflama eğrinin kaynağı için de geçerli olduğunu unutmayın.

### <a name="adjust-reverberation"></a>Reverberation Ayarla
* **Wetness (dB)** -Yankı güç, dB, kaynaktan daha fazla mesafe göre ayarlar. Negatif değerler ses daha kuru yaparken pozitif değerlere ses daha reverberant olun. Eğri Düzenleyicisi ' getirmek için eğri denetim (yeşil bir çizgi) tıklayın. Eğriyi noktaları eklemek için sol tıklatma ve istediğiniz işlev oluşturmak için bu noktaları sürükleyerek değiştirin. X ekseni kaynak uzaklığı ve y ekseni DB'de Yankı ayarlama. Eğrileri düzenleme hakkında daha fazla bilgi için bkz. Bu [Unity el ile](https://docs.unity3d.com/Manual/EditingCurves.html). Eğriyi geri varsayılana sıfırlamak için sağ tıklayın **Wetness** seçip **sıfırlama**.
* **Zaman ölçeğini decay** -çarpanı decay saatini ayarlar. Örneğin, hazırlama sonucu 750 milisaniye decay süresini belirtir, ancak bu değeri 1,5 olarak ayarlanır, kaynak decay uygulanırken 1,125 milisaniyedir.
* **Outdoorness** -kaynak üzerinde reverberation nasıl "dışarıda" ses bir ek ayarlama akustik sistemin tahmini. -1 olarak ayarlandığında bir kaynak tamamen içeride ses hale getirir ancak bu değer 1 olarak ayarlandığında bir kaynağı her zaman tamamen dışarıda ses hale getirir.

İliştirme **AcousticsAdjustExperimental** betik bir kaynak için bu kaynak için ek Deneysel ayarlama parametrelerini sağlar. Komut dosyası eklemek için tıklatın **Bileşen Ekle** alt kısmındaki **denetçisi** gidin ve panel **betikleri > akustik ayarlamak Deneysel**. Şu anda Deneysel bir denetimdir:

![Unity AcousticsAdjustExperimental ekran komut dosyası](media/acoustics-adjust-experimental.png)

* **Algısal uzaklık Warp** -üstel kuru ıslak oranını hesaplamak için kullanılan uzaklık çarpıtma uygulayın. Akustik sistem alanı boyunca düzgün uzaklığı ile değişir ve Algısal uzaklık ipuçlarını sağlama ıslak düzeylerine hesaplar. Çarpıtma değerleri 1'den büyük "uzak" ses yapmadan uzaklık ilgili reverberation düzeyleri, artırarak Bu etkiyi exaggerate. Değer 1'den az yapma çarpıtma uzaklık tabanlı reverberation daha hafif, yapmayı ses daha fazla "var" değiştirin.

## <a name="design-acoustics-for-all-sources"></a>Tüm kaynakları için tasarım akustik
Kanal Şerit Unity'nın tüm kaynakları için parametrelerini ayarlamak için tıklayın **ses Mixer**ve parametreleri ayarlamasına **proje akustik Mixer** efekt.

![Ekran proje akustik Unity Mixer özelleştirme paneli](media/mixer-parameters.png)

* **Wetness Ayarla** -üzerinde kaynak dinleyici uzaklığı sahnedeki tüm kaynaklarını Yankı güç, dB, ayarlar. Negatif değerler ses daha kuru yaparken pozitif değerlere ses daha reverberant olun.
* **RT60 ölçek** - çarpma skaler Yankı kez.
* **Kaydırma kullanın** -denetimleri ses olup çıkış binaural (0) veya çok kanallı (1) yatay olarak. 1 yanı sıra herhangi bir değer binaural gösterir. Çıkış ile HRTFs kulaklık ile kullanılmak üzere spatialized ve çok kanallı VBAP ile çok kanallı saran Konuşmacı sistemleriyle kullanılacak spatialized çıkış binaural. Çok kanallı panlayıcı kullanarak, cihaz ayarlarınızı eşleşen hoparlör modu seçtiğinizden emin altında bulunan **proje ayarları** > **ses**.

## <a name="check-proper-sound-source-placement"></a>Onay uygun ses kaynak yerleştirme
Ses kaynakları dolu voxels yerleştirilen akustik işleme almazsınız. Görünür Sahne geometri voxels genişletmek için bir voxel içinde bir kaynak tarafından visual geometri unoccluded görünürken yerleştirmek olasıdır. Proje akustik voxels voxel kılavuz onay kutusunu kapatarak görüntüleyebileceğiniz **şeyler** sağ üst kısmındaki menüde **Sahne** görünümü.

![Unity şeyler ekran menü](media/gizmos-menu.png)  

Voxel görünen, görsel bileşenleri oyununda bir dönüştürme uygulanmış olup olmadığını belirlemek de yardımcı olabilir. Bu durumda, aynı dönüşüm GameObject barındırmak üzere uygulama **akustik Manager**.

### <a name="bake-time-vs-run-time-voxels"></a>Çalışma zamanı voxels karşılaştırması zaman hazırlama
Düzenleyici penceresinde oyun tasarım zamanında ve oyun penceresinde çalışma zamanında voxels görüntülemek mümkündür. Bu görünümlerde voxels boyutunu farklıdır. Akustik çalışma zamanı ilişkilendirme için daha sorunsuz yükseltmelere ilişkilendirme sonuçları daha hassas bir voxel kılavuz kullanıyor olmasıdır. Çalışma zamanı voxels kullanarak ses kaynak yerleştirme doğrulanması gerekir.

Tasarım zamanı voxels:

![Tasarım zamanında voxels proje akustik ekran görüntüsü](media/voxels-design-time.png)

Çalışma zamanı voxels:

![Çalışma zamanı sırasında voxels proje akustik ekran görüntüsü](media/voxels-runtime.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kavramları vurgulama incelemelerini keşfedin [tasarım işlemi](design-process.md)

