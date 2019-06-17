---
title: Akustik Simülasyon ile Tasarım Kavramları
titlesuffix: Azure Cognitive Services
description: Bu kavramsal genel bakış, proje akustik ses tasarım süreci akustik benzetimi nasıl içerir açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 4a1a0b15da091a1c020eb132f6b14b9ee14d334c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61335431"
---
# <a name="project-acoustics-design-process-concepts"></a>Proje akustik tasarım işlemi kavramları

Nasıl proje akustik ses tasarımı işlemine aktarılır fiziksel akustik benzetimi içerir. Bu kavramsal genel bakış açıklanmaktadır.

## <a name="sound-design-with-audio-dsp-parameters"></a>Ses DSP parametrelerle ses tasarım

3B etkileşimli başlıkları ses sayısal sinyal (DSP) blokları ses altyapının barındırılan işlem kullanarak, belirli bir ses elde edin. Bu karmaşıklığı reverberation, echo, gecikme, eşitleme, sıkıştırma ve sınırlama, basit karıştırma ve başka etkileri blokları aralığında. Seçme, düzenleme ve bu etkileri parametreleri ayarlanıyor kimin estetik ve oyun hedeflerinden biri deneyimi elde eden bir ses grafiği oluşturur ses tasarımcının sorumluluğundadır.

Dinleyici ve sesler 3B alanda taşımak gibi etkileşimli bir başlık nasıl bu parametreleri değişen koşullara uyum? Ses Tasarımcı genellikle dinleyici sahnenin bir bölümünden hareket ettikçe reverberation etkileri değişiklikleri örneğin veya duck ses karışımındaki elde etmek için parametre değişiklikleri tetiklemek için programlanmış birimleri boşluğu boyunca kapatılmasını düzenler. Akustik sistemleridir de kullanılabilir, bu etkilerle ilgili bazı otomatik hale getirebilirsiniz.

3B başlıklar fizik motive ancak bir karışımını ımmersion ve oyun hedefleri başarmanın Tasarımcısı olarak ayarlanmış olan aydınlatma ve kinematik fizik sistemleri kullanın. Bir görsel tasarımcı bağımsız piksel değerleri ayarlayıp ayarlamadığını, ancak bunun yerine, 3B modelleri, malzemeler ve tüm fiziksel olarak tabanlı görsel estetik ve CPU maliyetleri dengenin açık aktarım sistemleri ayarlar. Ne eşdeğer işlem ses için geçerli olacak? Proje akustik bu sorunun incelenmesi bir ilk adımdır. İlk biz acoustical enerji boşluk üzerinden aktarım ne demek üzerinde touch.

![Yankı bölgelerle yayılan, ekran AltSpace Sahne](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>İmpulse yanıtlar: İki nokta alanında acoustically bağlanma

Ses tasarım ile ilgili bilgi sahibi değilseniz akustik impulse yanıtlarıyla ilgili bilgi sahibi olabilir. Akustik impulse yanıt ses kaynağından bir dinleyici taşınmasını modeller. Bu nedenle impulse yanıt odası akustik kapatma ve reverberation gibi ilginç her etkisini yakalayabilirsiniz. İmpulse yanıtları de ölçeklendirmek ses DSP etkileri tanıyan belirli güçlü özellikler var. İki ses sinyal birbirine ekleyip bir impulse yanıtıyla işleme impulse yanıta ayrı ayrı her bir sinyali uygulanması ve sonuçları aynı sonucu verir. Akustik doldurma ve impulse yanıtlar da modellenmiş Sahne üzerinde işlenen ses ve kaynak ve dinleyici konumların bağlıdır yok. Kısacası, ses yayma sahnenin etkisi impulse yanıtı oluşturur.

Her ilgi çekici odası akustik efekti impulse yanıtı yakalar ve biz bunu ses verimli bir şekilde filtre uygulayabilirsiniz ve biz ölçüm ya da simülasyonuna impulse yanıtlar elde edebilirsiniz. Ancak ne biz yoksa oldukça fizik tam olarak eşleşmesi akustik istediğiniz, bunun yerine bir Sahne duygusal taleplerini eşleşecek şekilde hayallerindeki? Ancak çok piksel değerleri gibi impulse yanıt yalnızca sayılar binlerce listesi, nasıl biz büyük olasılıkla estetik gereksinimlerini karşılayacak şekilde değiştirebilir miyim? Ve ne sorunsuz açılan kapılar veya engelleri arkasında geçirilirken kaç impulse yanıtları kesintisiz etkiyi görmek ihtiyacımız değişir kapatma/engel olmasını istiyoruz? Peki kaynak hızlı taşır? Nasıl biz enterpolasyon?

Akustik etkileşimli başlıklarında bazı yönleri için benzetim ve impulse yanıtları kullanmak zor geliyor. Ancak biz yine de biz impulse yanıtımız bizim tanıdık ses DSP efekt parametreleri ile benzetim bağlanabilir, Tasarımcı ayarlamalar destekleyen bir ses taşıma sistemi oluşturabilir.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Ses DSP parametrelerle benzetimi bağlanma

Her ilgi çekici bir impulse yanıtını içeren (ve her sizi ilgilendirmeyen) acoustical efekt. Parametrelerinin doğru şekilde ayarlandığında, ses DSP blokları, ilginç acoustical etkisi işleyebilirsiniz. 3B görünümde Ses Aktarım otomatikleştirmek için bir ses DSP bloğu sürücüsüne acoustical benzetimi kullanarak ölçü impulse yanıt ses DSP Parametreler yalnızca bir konudur. Bu ölçüm, kapatma, anlamsızdır, portalling ve reverberation dahil olmak üzere bazı yaygın ve önemli acoustical etkileri için iyi anlaşılmalıdır.

Ancak, benzetim doğrudan ses DSP parametrelerine bağlıysa, Tasarımcı ayarlama nerede? Ne biz elde? De size önemli miktarda bellek geri impulse yanıtları atılıyor ve birkaç DSP parametreleri koruma elde edin. Tasarımcı nihai sonucu üzerinde bazı güç vermek için biz yalnızca Tasarımcı benzetim ses DSP arasındaki eklemenin bir yolu bulmak ve.

![Yayılan parametrelerle stilize impulse yanıtıyla grafiği](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Benzetim ilgili ses DSP parametrelerinden dönüştürme ses tasarım

Güneş gözlüğü görünümünüzü dünyanın etkisini göz önünde bulundurun. Parlak bir günde gözlük bir şey daha rahat parlama efektlerini azaltabilir. Koyu bir odada hiç görmek mümkün olmayabilir. Gözlük parlaklık belirli bir düzeyde tüm durumlarda ayarlamanız gerekmez; Bunlar yalnızca koyu her şey yapın.

Kapatma ve reverberation parametreleri kullanarak, ses DSP sürücü için benzetim kullanıyoruz, sonra simülatör ' DSP gördüğü ' parametrelerini ayarlamak için bir filtre ekleyebiliriz. Filtre mıydı kapatma belirli bir düzeyde uygulamak veya gözlüğü her yer aynı parlaklık yapmayın gibi kuyruk uzunluğu çok Yankı. Filtreyi yalnızca daha az occlude her occluder çalışmasına neden olabilir. Veya daha fazla occlude. Açılan kapılar bir ortamdan güçlü kapatma efekti Düzgünlüğü yürürlükte korurken geçişleri artırır ancak ekleme ve bir 'koyulaştırıcı' kapatma parametresi filtre ayarlayarak, büyük, açık odaları hala az Hayır kapatma etkiye sahip benzetim olmasını sağlar.

Bu programlamada tasarımcının görevini her durum için akustik parametreleri benzetim gelen en önemli DSP parametreleri uygulamak için seçme ve ayarlama filtreleri seçmesini değiştirir. Öğesinden daha yüksek endişelere yer bırakmadan kesintisiz geçişleri kapatma ve reverberation efektleri yoğunluğu ve kaynakları karışımını varlığını ayarlama dar endişelere yer bırakmadan tasarımcının etkinlikleri yükseltir. Durum talep ettiğinde, bir filtre her zaman kullanılabilir yalnızca geri DSP parametreler belirli bir kaynak için belirli bir durumda seçme aşağıdadır.

## <a name="sound-design-in-project-acoustics"></a>Proje akustik ses tasarımı

Yukarıda belirtilen bileşenlerin her birini proje akustik paket tümleştirilir: simülatör, parametreleri ayıklar ve akustik varlık, ses DSP ve Filtre Seçimi oluşturan bir kodlayıcı. Proje akustik ses tasarımla benzetim türetilmiş ve oyun Düzenleyicisi ve Ses altyapısı içinde kullanıma sunulan dinamik denetimlerle ses DSP için uygulanan kapatma ve reverberation parametrelerini ayarlamak filtreleri seçme parametrelerini kapsar.

## <a name="next-steps"></a>Sonraki adımlar
* Tasarım using paradigma deneyin [Unity proje akustik hızlı](unity-quickstart.md) veya [Unreal için proje akustik hızlı başlangıç](unreal-quickstart.md)
* Keşfedin [proje akustik Unity için denetimleri tasarım](unity-workflow.md) veya [proje akustik Unreal için denetimleri tasarım](unreal-workflow.md)

