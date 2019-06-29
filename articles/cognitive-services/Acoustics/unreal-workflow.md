---
title: Proje akustik Unreal tasarım Öğreticisi
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, Unreal ve Wwise proje akustik için tasarımı iş akışı açıklanır.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 1692032b093cd6189cac3ea3f63c563d9accd8ed
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477824"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Proje akustik Unreal/Wwise tasarım Öğreticisi
Bu öğreticide, Unreal ve Wwise proje akustik için tasarım Kurulum ve iş akışı açıklanır.

Yazılım önkoşulları:
* Proje akustik Wwise ve Unreal eklentileri ile Unreal bir proje

Proje akustik Unreal bir projeyle almak için şunları yapabilirsiniz:
* İzleyin [proje akustik Unreal tümleştirme](unreal-integration.md) proje akustik Unreal projenize eklemek için yönergeleri
* Ya da kullanmak [proje akustik kodunuzla](unreal-quickstart.md).

## <a name="setup-project-wide-wwise-properties"></a>Kurulum projesi genelinde Wwise özellikleri
Genel anlamsızdır ve proje akustik eklentisi Wwise ses DSP nasıl sürücüleri etkileyen kapatma eğrileri Wwise sahiptir.

### <a name="design-wwise-occlusion-curves"></a>Wwise kapatma eğrileri tasarlama
Proje akustik etkin olduğunda düşük geçişli filtre (LPF), kapatma toplu yanıtlar ve yüksek geçiş filtre (HPF) içinde Wwise ayarlamanıza Eğriler. Doğrusal bir kapatma değer için-100 dB 100 değerini içeren birim eğri türünüz ayarlanması önerilir.

Bir kapatma-18 DB proje akustik benzetim hesaplar, bu ayar, bu giriş için eğri X = 18 oluşturacak ve karşılık gelen Y uygulanan zayıflama değerdir. Yarı kapatma yapmak için uç nokta-50 dB-100 dB yerine veya -200 kapatma exaggerate için dB ayarlayın. Uyumlu hale getirmenizi ve en iyi şekilde oyununuzu çalışır herhangi bir eğri ince ayar yapma.
 
![Ekran görüntüsü, Wwise kapatma eğrisi Düzenleyicisi](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Wwise anlamsızdır eğrileri devre dışı bırak
Kuru yalıtım düzeyinde Wwise anlamsızdır eğrileri etkiler ancak proje akustik ıslak/kuru oranları zorlamak için tasarım denetimleri ve simülasyon kullanır. Engel birim eğri devre dışı bırakılması önerilir. Wetness tasarlamak için daha sonra açıklanan Wetness ayarlamak denetimi kullanın.
 
Diğer amaçlar için anlamsızdır LPF/HPF eğrileri kullanıyorsanız, bunları Y = 0 X = 0 olarak ayarladığınızdan emin olun (diğer bir deyişle, LPF veya yoktur HPF hiçbir engel olduğunda).

![Ekran görüntüsü, Wwise anlamsızdır eğrisi Düzenleyicisi](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Proje akustik mixer parametreleri tasarlama
Proje akustik Bus mixer eklenti sekmesini ziyaret ederek genel yankı özelliklerini denetleyebilirsiniz. "Project akustik Mixer üzerinde (özel)" mixer eklenti ait ayarları panelini açmak için çift tıklayın.

Ayrıca mixer eklentisi "Spatialization gerçekleştirmek" seçeneği olduğunu da görebilirsiniz. Bunun yerine Proje akustik'ın yerleşik spatialization kullanmayı tercih ediyorsanız, "Spatialization gerçekleştirmek" onay kutusunu işaretleyin ve HRTF veya kaydırma seçin. Ayarlamış olduğunuz tüm kuru yedek veri yolları devre dışı bıraktığınızdan emin olun, aksi takdirde doğrudan yolunu iki kez dinleyeceksiniz. Yankı karışımına genel denetim uygulamak amacıyla "Wetness ayarlama" ve "Yankı zaman ölçek faktörü" kullanın. Unreal'ı yeniden başlatın ardından soundbanks mixer eklentisi yapılandırma değişiklikleri gibi 'Gerçekleştirmek Spatialization' onay kutusunu seçmek için play ulaşmaktan önce yeniden unutmayın.

![Proje akustik Wwise ekran mixer eklentisi seçenekleri](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Wwise aktör mixer hiyerarşideki proje akustik tasarım denetimleri ayarlayın
Tek bir aktör mixer parametre denetimi, üzerinde aktör Mixer çift tıklayın, sonra kendi Mixer eklenti sekmesine tıklayın. Burada herhangi bir parametre ses başına düzeyinde değiştirmek mümkün olacaktır. Bu değerleri (aşağıda açıklanmıştır) Unreal yan kümeden olanları birleştirin. Örneğin, proje akustik Unreal eklentisi 0,5 ve Wwise kümeleri -0.25, sonuçta elde edilen Outdoorness ayarlama için uygulanan bir nesne üzerinde Outdoorness ayarlama ayarlarsa 0,25 ses olacaktır.

![Ses karıştırıcı ayarları Wwise aktör mixer hiyerarşideki her ekran görüntüsü](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Kuru gönderme yedek veri yolu vardır ve çıktı veri yolu, gönderme ıslak olun
Gerekli aktör mixer Kurulumu normal kuru ve ıslak Wwise içinde yönlendirme değişimlerinin unutmayın. Bu, aktör-mixer'ın çıkış yolundaki (Proje akustik Bus kümesine) Yankı sinyal ve yedek kullanıcı tanımlı yol boyunca kuru sinyal üretir. Bu yönlendirme, proje akustik Wwise eklentisi Wwise mixer eklentisi API özelliklerini nedeniyle gereklidir.

![Ses tasarım yönergeleri için proje akustik gösteren ekran görüntüsü, Wwise Düzenleyicisi](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Uzaklık zayıflama eğrileri ayarlayın
Aktör-kullanarak Karıştırıcılar tarafından kullanılan tüm zayıflama eğrisi olmak proje akustik sahip kullanıcı tanımlı yedek kümesi "veri yolu birimi çıkarmak için." Gönder Wwise yeni oluşturulan zayıflama eğrileri için varsayılan olarak, bunu yapar. Mevcut bir projeyi geçiriyorsanız eğri ayarlarınızı kontrol edin.

Varsayılan olarak, proje akustik benzetimi player konum etrafındaki bir RADIUS 45 ölçütlerden vardır. Genellikle, zayıflama eğri-200 dB bu uzaklığı geçici olarak ayarlanması önerilir. Bu uzaklık, sabit bir kısıtlaması değil. Bazı Silah benzer için daha büyük bir RADIUS isteyebilirsiniz. Böyle durumlarda, uyarı yalnızca geometrisi 45 m player konumun içinde yer alacak olur. Oyuncu bir odada bulunur ve ses kaynak odası ve 100 milyon dışında ise, bu düzgün occluded. Bir odada kaynaktır ve oyuncu dışına ve 100 milyon ise, bu düzgün occluded gerekmez.

![Ekran görüntüsü, Wwise zayıflama Eğriler](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>POST Mixer eşitleme ###
 Yapmak isteyebileceğiniz başka bir şey post mixer dengeleyici eklemektir. Proje akustik veri yolu bir tipik Yankı yolunda (varsayılan Yankı modu) olarak kabul et ve bir filtre üzerinde eşitleme yapmak için yerleştirin. Bu örnek proje akustik Wwise örnek projesinde görebilirsiniz.

![Ekran görüntüsü, Wwise sonrası mixer EQ](media/wwise-post-mixer-eq.png)

Örneğin, yüksek geçişi filtre boomy, kullanışsız Yankı yield yakın alan kayıtları gelen bas işlemek yardımcı olabilir. Daha fazla sonrası hazırlama denetim RTPCs, oyun zamanında Yankı rengini değiştirmek için izin verme aracılığıyla EQ ayarlayarak da elde edebilirsiniz.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Sahne genelinde akustik proje özelliklerini ayarlama

Akustik alanı aktör sistem davranışını değiştirmek ve hata ayıklamaya faydalı olan birçok denetim sunar.

![Unreal akustik alanı denetimlerin ekran görüntüsü](media/acoustics-space-controls.png)

* **Akustik verileri:** Bu alan gerektiğinden akustik varlık içeriği/akustik dizinden atanması gerekir. Proje akustik eklentisi, projenizin paketlenmiş dizinlere içeriği/akustik dizini otomatik olarak eklenir.
* **Döşeme Boyutu:** RAM yüklenen akustik verilerini istediğiniz dinleyici etrafında bölgenin kapsam. Dinleyici etrafında hemen araştırmaları sürece player yüklenir, sonuçları tüm araştırmaları için akustik verilerini yükleme olarak aynı. Daha büyük kutucukları, daha fazla RAM kullanın, ancak disk g/ç
* **Otomatik Stream:** Otomatik olarak yüklenen bir bölgenin edge dinleyici ulaştığında olarak etkinleştirildiğinde, kutucuklar için yeni yükler. Devre dışı bırakıldığında kutucuklar için yeni kod veya planlar aracılığıyla el ile yüklemeniz gerekir
* **Önbellek ölçeklendirme:** akustik sorgular için kullanılan önbellek boyutunu denetler. Daha küçük bir önbellek daha az RAM kullanır, ancak her sorgu için CPU kullanımını artırabilir.
* **Akustik: etkin** Hızlı bir etkinleştirmek için hata ayıklama denetim / B akustik benzetimi geçiş. Bu denetim, yapılandırmaları sevkiyat içinde göz ardı edilir. Denetim, belirli bir ses hata akustik hesaplamaları veya başka bir sorun Wwise projedeki kaynaklanıyorsa bulmak için yararlıdır.
* **Uzaklıkları güncelleştirin:** Uzaklık sorgular için önceden oluşturulan akustik bilgileri kullanmak istiyorsanız bu seçeneği kullanın. Bu sorguları ray atamaların benzerdir, ancak önceden hesaplanmış çok daha az CPU şekilde yararlanın. Ayrık yansımalar en yakın yüzeyinden dinleyicisi örnek kullanım içindir. Bu tam olarak yararlanmak için kod veya şemaları sorgu Mesafeler için kullanılacak gerekir.
* **Çizim istatistikleri:** While UE'ın `stat Acoustics` , CPU bilgilerle bu durum görüntüsü şu anda yüklü ACE dosyanın RAM kullanımını gösterir ve başka bir durum bilgisi üst ekranın sol sağlayabilir.
* **Çizim Voxels:** Voxels kapatmak için çalışma zamanı ilişkilendirme sırasında kullanılan voxel kılavuz gösteren dinleyici yer. Bir çalışma zamanı voxel içinde bir verici ise akustik sorguları başarısız olur.
* **Araştırmalar Çiz:** Bu görünüm için tüm araştırmaları gösterir. Uygulanırsa, yük durumlarına bağlı olarak farklı bir renk olur.
* **Uzaklıkları Çiz:** Güncelleştirme uzaklıkları etkinleştirilirse, bu dinleyici etrafında quantized yönde dinleyicisi en yakın çalışma yüzeyinde bir kutu gösterir.

## <a name="actor-specific-acoustics-design-controls"></a>Aktör özel akustik tasarım denetimleri
Bu tasarım denetimleri Unreal tek bir ses bileşenine kapsamına eklenir.

![Unreal ses bileşeni denetimlerin ekran görüntüsü](media/audio-component-controls.png)

* **Kapatma Çarpanı:** Kapatma etkisi denetler. Değer 1 > kapatma artırmasına. Değerleri < 1 bunu azaltmak.
* **Wetness ayarlama:** Ek Yankı dB
* **Decay zaman Çarpanı:** Denetimleri RT60 akustik benzetim üzerinde çıkışını multiplicatively, temel
* **Outdoorness ayarlama:** Nasıl dışarıda reverberation olduğunu denetler. 0 yakın değerler daha içeride, 1 yakın daha dışarıda. Bu düzeltmenin eklenebilir, -1 olarak ayarlandığında içeride, zorunlu kılacak şekilde + 1 için ayarlama dışarıda zorlar.
* **İletim Db:** Bir ek aracılığıyla--wall ses satırı görüş göre uzaklığı zayıflama ile birlikte bu ses yüksekliği ile işleyin.
* **Uzaklık Warp ıslak oranı:** Hemen yakınında ve daha doğrudan yolunu etkilemeden gibi kaynak reverberation özelliklerini ayarlar.
* **Başlat menüsünde Yürüt:** Sesi otomatik olarak sahnenin başlangıç çalmak olup olmadığını belirlemek için Değiştir'i tıklatın. Varsayılan olarak etkindir.
* **Akustik parametreleri göster:** Oyun içi Bileşen en üstünde görünen hata ayıklama bilgileri. (yalnızca. teslimat olmayan yapılandırmalar için)

## <a name="blueprint-functionality"></a>Blueprint işlevi
Akustik alanı Aktör bir eşleme yükleniyor veya komut dosyası düzeyi aracılığıyla ayarlarını değiştirme gibi işlevsellik sağlayan şema erişilebilir. Burada iki örnek sunuyoruz.

### <a name="add-finer-grained-control-over-streaming-load"></a>Akış yük boyunca daha ayrıntılı denetim ekleme
Otomatik olarak player konumuna bağlı Akış yerine kendiniz akış akustik verilerini yönetmek için zorla yük kutucuk blueprint işlevi kullanabilirsiniz:

![Unreal seçeneklerinde Blueprint akışını ekran görüntüsü](media/blueprint-streaming.png)

* **Hedef:** AcousticsSpace aktör
* **Merkezi konum:** Yüklenen verilerin bölgenin Merkezi
* **Kutucuk dışında araştırmaları kaldırın:** Eğer denetlenmişse yeni bölgede değil tüm araştırmaları RAM kaldırılır. Araştırma bırakarak da belleğe yüklenirken işaretlenmemişse, yeni bölge belleğe yüklenir
* **Blok tamamlandığında:** Eşzamanlı bir işlem yük kutucuk yapar

Döşeme boyutu zaten zorla yük kutucuk çağrılmadan önce ayarlanmalıdır. Örneğin, şunun gibi bir ACE dosya yüklemek, döşeme boyutunu ayarlama ve bir bölgede akışını yapabilirsiniz:

![Unreal seçeneklerinde akış Kurulum ekran görüntüsü](media/streaming-setup.png)

Bu örnekte kullanılan yükleme akustik verilerini blueprint işlevi aşağıdaki parametrelere sahiptir:

* **Hedef:** AcousticsSpace aktör.
* **Yeni hazırlama:** Yüklenecek akustik veri varlığı. Bu boş /, null ayarı bırakarak geçerli hazırlama, yeni bir yüklemeden boşaltacak.

### <a name="optionally-query-for-surface-proximity"></a>İsteğe bağlı olarak sorgu yüzeyi yakınlık için
Ne kadar yakın görmek istiyorsanız yüzeyleri dinleyici etrafında belirli bir yönde, sorgu Distance işlevi kullanabilirsiniz. Bu işlev, tek yönlü Gecikmeli yansımalar kullanımını veya oyun yüzeyi yakınlık tarafından yönetilen başka bir mantık için yararlı olabilir. Akustik arama tablodan sonuçlar alınır için sorgu ray atama daha ucuzdur.

![Örnek şema uzaklık sorgusunun ekran görüntüsü](media/distance-query.png)

* **Hedef:** AcousticsSpace aktör
* **Görünüm Yönü:** Ortalanmış konumundaki dinleyici, sorgulama için yönü
* **Uzaklık:** En yakın yüzeyine uzaklık sorgu başarılı olursa
* **Dönüş değeri:** Boole - sorgu başarılı olursa true, aksi durumda false

## <a name="next-steps"></a>Sonraki adımlar
* Kavramları keşfedin [tasarım işlemi](design-process.md)
* [Bir Azure hesabı oluşturun](create-azure-account.md) kendi Sahne hazırlama için


