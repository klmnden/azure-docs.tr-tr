---
title: Proje akustik tasarım işlemi genel bakış
titlesuffix: Azure Cognitive Services
description: Bu belge, tasarım amacı proje akustik iş akışı, tüm üç aşamada express açıklar.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 46e2e276086f836ff881fde1db6462f6e7788e22
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901371"
---
# <a name="design-process-overview"></a>Tasarım işlemine genel bakış
Proje akustik iş akışı, tüm üç aşamada tasarım amacınızla ifade edebilirsiniz: önceden hazırlama Sahne Kurulum, ses kaynak yerleştirme ve sonrası hazırlama tasarım. Tasarımcı denetime nasıl Sahne sesleri korurken Yankı birimler yerleştirme ile ilişkili daha az biçimlendirme işlemi gerektirir.

## <a name="pre-bake-design"></a>Tasarım önceden hazırlama
Önceden hazırlama Sahne Kurulum işlemi Sahne ve Sahne öğeleri occlusions, yansıma ve reverberation sağlamak için benzetim katılacak seçerek içeren ses wave simülasyonu için kullanılan meta veriler üretir. Sahne için meta veriler, her Sahne öğesi için akustik malzemelerin seçimdir. Akustik malzemeleri her yüzeyinden yansımasını ses enerji miktarını denetler.

Varsayılan içe alma tüm yüzeyleri için 0.04, yüksek oranda yansıtıcı olduğu katsayısıdır. Bunlar bir çalışma alanından diğerine geçiş sahnenin duyduğunuzda, dinleyicilere özellikle önemli olan farklı malzeme Sahne boyunca içe alma katsayıları ayarlayarak estetik ve oyun efektleri elde edebilirsiniz. Örneğin, bir koyu reverberant odasından parlak bir geçiş, reverberant olmayan dış Sahne geçişin etkisini geliştirir. Bu etkiyi elde etmek için daha yüksek dış Sahne malzemeleri üzerinde içe alma katsayıları ayarlayın.

Verilen bir malzeme odasında reverberation süresini 0,01 için 0,20 aralığında içe alma değerlerine sahip çoğu malzemelerle, içe alma katsayısı için inversely ilişkilidir. Bu aralığın dışında kalan içe alma katsayıları malzemelerle çok absorbent.

![Yankı Süresi grafiği](media/ReverbTimeGraph.png)

[UI izlenecek yol hazırlama](bake-ui-walkthrough.md) ön hazırlama denetimleri ayrıntılı açıklanmaktadır.

## <a name="sound-source-placement"></a>Ses kaynak yerleştirme
Çalışma zamanında voxels ve araştırma noktalarına görüntülemeyi içinde voxelized geometri takılması ses kaynaklarıyla sorunlarında hata ayıklama yardımcı olabilir. Görüntü işaret voxel kılavuz ve araştırma geçiş, şeyler menüde sağ üst kısmındaki Sahne görünümünde karşılık gelen onay kutusuna tıklayın. Ses kaynak içinde bir duvar voxel ise, bu dosyayı bir hava voxel başına taşıyın.

![Şeyler menüsü](media/GizmosMenu.png)  

Voxel görünen, görsel bileşenleri oyununda bir dönüştürme uygulanmış olup olmadığını belirlemeye yardımcı olabilir. Bu durumda, aynı dönüşüm GameObject barındırmak üzere uygulama **akustik Manager**.

### <a name="voxel-size-discrepancies"></a>Voxel boyutu tutarsızlıklar
Akustik hazırlama içinde kafesleri katılmak, sahnenin göstermek için kullanılan voxels boyutunun tasarım zamanı ve çalışma zamanı görünümlerinde farklı olduğunu fark edebilirsiniz. Bu fark, kalite/ayrıntı düzeyi, seçili benzetimi sıklık etkilemez ancak yerine biproduct voxelized sahnenin çalışma zamanı kullanım değildir. Çalışma zamanında benzetimi voxels "kaynak konumları arasında bir ilişkilendirme desteklemek için daraltılmış". Bu, aynı zamanda bu yana hiçbir ses bir acoustically treated kafes içeren kaynakları bir voxel iç yapmayın benzetimi voxel boyutu – izin verdiğinden tasarım Sahne kafesleri ses kaynakları yakın zaman konumlandırmasını etkinleştirir.

Unity eklenti tarafından görselleştirilen olarak tasarım (ön hazırlama) voxels ve çalışma zamanı (sonrası hazırlama) voxels arasındaki farkı gösteren iki görüntü şunlardır:

Tasarım zamanı voxels:

![VoxelsDesignTime](media/VoxelsDesignTime.png)

Çalışma zamanı voxels:

![VoxelsRuntime](media/VoxelsRuntime.png)

Tasarım modu voxels daraltılmış voxels değil çalışma zamanı görselleştirmesini kullanarak voxel kafes mimari/yapısal Sahne kafesleri doğru olup olmadığını gösteren şirket kararı yapılmalıdır.

## <a name="post-bake-design"></a>Tasarım sonrası hazırlama
Hazırlama sonuçları Sahne boyunca tüm dinleyici kaynak konum çiftleri için kapatma ve reverberation parametreler olarak ACE dosyasında depolanır. Bu fiziksel olarak doğru sonuç, projeniz için kullanılabilir- ve tasarımı için harika bir başlangıç noktası. Hazırlama sonrası tasarım süreci zamanında hazırlama sonucu parametreleri dönüştürme kuralları belirtir.

### <a name="distance-based-attenuation"></a>Uzaklık tabanlı zayıflama
Ses DSP sağladığı **Microsoft Acoustics** Unity spatializer eklentisinin Unity düzenleyicide yerleşik olarak bulunan kaynak başına uzaklık tabanlı zayıflama dikkate alır. Denetimler için uzaklık tabanlı zayıflama bulunduğunuz **ses kaynak** bileşeni bulunan **denetçisi** altında ses panelinden kaynakları **3B ses ayarları**:

![Uzaklık Zayıflama](media/distanceattenuation.png)

Akustik player konum etrafındaki ortalanmış bir "benzetimi bölge" kutusu hesaplama gerçekleştirir. Bir ses kaynak Player'dan bu simülasyon bölge dışında bulunan uzak ise yalnızca geometri kutusundaki occluders kapsamına player olduğunda epey iyi çalışan (örneğin, kapatma neden) ses yayılmasını etkiler. Ancak, durumlarda player boş bir alanı ancak occluders uzak ses kaynak sesi unrealistically disoccluded haline. Bizim önerilen çözüm Böyle durumlarda ses zayıflama 0 yaklaşık 45 m, kutusunun Player'a yatay uzaklığını varsayılan olarak olduğundan emin olun olabilir.

### <a name="tuning-scene-parameters"></a>Sahne parametrelerini ayarlama
Kanal Şerit Unity'nın tüm kaynakları için parametrelerini ayarlamak için tıklayın **ses Mixer**ve parametreleri ayarlamasına **akustik Mixer** efekt.

![Mixer özelleştirme](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Kaynak parametrelerini ayarlama
İliştirme **AcousticsAdjust** betik bir kaynak için bu kaynak için ayar parametreleri sağlar. Komut dosyası eklemek için tıklatın **Bileşen Ekle** alt kısmındaki **denetçisi** gidin ve panel **betikleri > akustik ayarlamak**. Betik altı denetimlerine sahiptir:

![AcousticsAdjust](media/AcousticsAdjust.png)

* **Akustik etkinleştirme** -bu kaynak için akustik uygulanıp uygulanmayacağını denetler. İşaretlenmediğinde, kaynak HRTFs ile ancak akustik anlamsızdır, kapatma ve düzeyi ve decay zamanı gibi dinamik reverberation parametreleri olmadan anlamına gelir, spatialized. Reverberation yine de bir sabit düzeyi ve decay saat ile uygulanır.
* **Kapatma** -çarpanı akustik sistem tarafından hesaplanan kapatma dB düzeyinde uygulanır. Bu çarpan 1'den büyükse, kapatma exaggerated değerleri 1'den küçük kapatma etkisini daha hafif olun ve kapatma 0 değerini devre dışı bırakır.
* **İletim (dB)** -geometri iletim kaynaklanan zayıflama (veritabanında) ayarlayın. Bu kaydırıcı iletimini devre dışı bırakmak için en düşük düzeyine ayarlayın. Akustik Sahne geometri (portaling) geçici olarak gelen olarak ilk kuru ses spatializes. İletim satırı görüş yönde spatialized ek bir kuru varış sağlar. Uzaklık zayıflama eğrinin kaynağı için de geçerli olduğunu unutmayın.
* **Wetness (dB)** -Yankı güç, dB, kaynaktan daha fazla mesafe göre ayarlar. Negatif değerler ses daha kuru yaparken pozitif değerlere ses daha reverberant olun. Eğri Düzenleyicisi ' getirmek için eğri denetim (yeşil bir çizgi) tıklayın. Eğriyi noktaları eklemek için sol tıklatma ve istediğiniz işlev oluşturmak için bu noktaları sürükleyerek değiştirin. X ekseni kaynak uzaklığı ve y ekseni DB'de Yankı ayarlama. Bkz. Bu [Unity el ile](https://docs.unity3d.com/Manual/EditingCurves.html) eğrilerini düzenleme hakkında daha fazla bilgi. Eğriyi geri varsayılana sıfırlamak için sağ tıklayın **Wetness** seçip **sıfırlama**.
* **Zaman ölçeğini decay** -çarpanı decay saatini ayarlar. Örneğin, hazırlama sonucu 750 milisaniye decay süresini belirtir, ancak bu değeri 1,5 olarak ayarlanır, kaynak decay uygulanırken 1,125 milisaniyedir.
* **Outdoorness** -kaynak üzerinde reverberation nasıl "dışarıda" ses bir ek ayarlama akustik sistemin tahmini. Bu ayarı 1 bir kaynağı her zaman tamamen dışarıda ses yapar, ayar çalışırken, -1'e bir kaynak içeride ses bulabilmesini sağlar.

Farklı kaynaklar estetik veya oyun bazı efektler elde etmek farklı ayarlar gerektirebilir. İletişim bir olası bir örnektir. İletişim genellikle oyun için anlaşılır olması gerekiyor ancak İnsan kulak daha konuşma, reverberation attuned. Bu iletişim olmayan-diegetic taşıyarak yapmadan hesap **Wetness** aşağı yönde, ayarlama **Algısal uzaklık Warp** bazı ekleme, aşağıda açıklanan parametresi  **İletim** duvarları yayılan ve/veya azaltma kuru bazı ses boost için **kapatma** portallar aracılığıyla ulaşması daha fazla ses 1.

İliştirme **AcousticsAdjustExperimental** betik bir kaynak için bu kaynak için ek Deneysel ayarlama parametrelerini sağlar. Komut dosyası eklemek için tıklatın **Bileşen Ekle** alt kısmındaki **denetçisi** gidin ve panel **betikleri > akustik ayarlamak Deneysel**. Şu anda Deneysel bir denetimdir:

![AcousticsAdjustExperimental](media/AcousticsAdjustExperimental.png)

* **Algısal uzaklık Warp** -üstel kuru ıslak oranını hesaplamak için kullanılan uzaklık çarpıtma uygulayın. Akustik sistem alanı boyunca düzgün uzaklığı ile değişir ve Algısal uzaklık ipuçlarını sağlama ıslak düzeylerine hesaplar. Daha fazla mesafe ilgili reverberation düzeyleri artırarak Bu etkiyi 1 exaggerate değerleri çarpıtma, yapmayı ses "uzak", 1'den az yapma sesi yapmanın daha hafif uzaklık tabanlı reverberation değişiklik değerleri çarpıtma sırasında "var."

