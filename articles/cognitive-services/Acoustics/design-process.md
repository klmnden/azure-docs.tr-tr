---
title: Tasarım işlemine genel bakış için akustik - Bilişsel hizmetler
description: Bu belge, tasarım amacı proje akustik iş akışı, tüm üç aşamada express açıklar.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 8f594be67c4677fae00cb01598d3899e30dae1e8
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433233"
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

## <a name="post-bake-design"></a>Tasarım sonrası hazırlama
Hazırlama sonuçları Sahne boyunca tüm dinleyici kaynak konum çiftleri için kapatma ve reverberation parametreler olarak ACE dosyasında depolanır. Bu fiziksel olarak doğru sonuç, projeniz için kullanılabilir- ve tasarımı için harika bir başlangıç noktası. Hazırlama sonrası tasarım süreci zamanında hazırlama sonucu parametreleri dönüştürme kuralları belirtir.

### <a name="distance-based-attenuation"></a>Uzaklık tabanlı zayıflama
Ses DSP sağladığı **Microsoft Acoustics** Unity spatializer eklentisinin Unity düzenleyicide yerleşik olarak bulunan kaynak başına uzaklık tabanlı zayıflama dikkate alır. Denetimler için uzaklık tabanlı zayıflama bulunduğunuz **ses kaynak** bileşeni bulunan **denetçisi** altında ses panelinden kaynakları **3B ses ayarları**:

![Uzaklık Zayıflama](media/distanceattenuation.png)

### <a name="tuning-scene-parameters"></a>Sahne parametrelerini ayarlama
Kanal Şerit Unity'nın tüm kaynakları için parametrelerini ayarlamak için tıklayın **ses Mixer**ve parametreleri ayarlamasına **akustik Mixer** efekt.

![Mixer özelleştirme](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Kaynak parametrelerini ayarlama
İliştirme **AcousticsSourceCustomization** betik bir kaynak için bu kaynak için ayar parametreleri sağlar. Komut dosyası eklemek için tıklatın **Bileşen Ekle** alt kısmındaki **denetçisi** gidin ve panel **betikleri > akustik kaynak özelleştirme**. Betik, üç parametrelere sahiptir:

![Kaynak özelleştirme](media/SourceCustomization.png)

* **Yankı Power ayarlamak** -DB'de Yankı power ayarlar. Negatif değerler ses daha kuru yaparken pozitif değerlere ses daha reverberant olun.
* **Zaman ölçeğini decay** -çarpanı decay saatini ayarlar. Örneğin, hazırlama sonucu 750 milisaniye decay süresini belirtir, ancak bu değeri 1,5 olarak ayarlanır, kaynak decay uygulanırken 1,125 milisaniyedir.
* **Akustik etkinleştirme** -bu kaynak için akustik uygulanıp uygulanmayacağını denetler. İşaretlenmediğinde, kaynak HRTFs ile ancak akustik anlamsızdır, kapatma ve düzeyi ve decay zamanı gibi dinamik reverberation parametreleri olmadan anlamına gelir, spatialized. Reverberation yine de bir sabit düzeyi ve decay saat ile uygulanır.

Farklı kaynaklar estetik veya oyun bazı efektler elde etmek farklı ayarlar gerektirebilir. İletişim bir olası bir örnektir. İletişim genellikle oyun için anlaşılır olması gerekiyor ancak İnsan kulak daha konuşma, reverberation attuned. Bunun için aşağı yönde Yankı power ayarlayarak iletişim olmayan-diegetic yapmadan hesap.
