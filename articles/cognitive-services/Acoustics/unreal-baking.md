---
title: Proje akustik Unreal hazırlama Öğreticisi
titlesuffix: Azure Cognitive Services
description: Bu belgede, Unreal Düzenleyici uzantısı kullanarak bir akustik hazırlama gönderme işlemi açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: michem
ms.openlocfilehash: 6b49a6b9e235414cd63eacdbad523bbda8646963
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304305"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Proje akustik Unreal hazırlama Öğreticisi
Bu belgede, Unreal Düzenleyici uzantısı kullanarak bir akustik hazırlama gönderme işlemi açıklanmaktadır.

Bir hazırlama yapmaya beş adım vardır:

1. Oluşturma veya, oyuncu Gezinti kafes etiketi
2. Etiket akustik geometri
3. Geometriye akustik malzemeleri özellikler atama
4. Önizleme araştırma yerleştirme
5. Hazırlama

## <a name="open-the-project-acoustics-editor-mode"></a>Proje akustik Düzenleyicisi modunu açın

Proje akustik eklenti paketi projenize içeri aktarın. Bununla ilgili Yardım için bkz. [Unreal tümleştirme](unreal-integration.md) konu. Eklenti tümleştirildiğinde yeni akustik modu simgesini tıklatarak akustik kullanıcı arabirimini açın.

![Unreal Düzenleyicisi akustik modu ekran seçeneği](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Etiket aktörler için akustik

Akustik modu açtığınızda görüntülenen ilk sekme nesneleri sekmesidir. Ekler düzeyinizi içinde etiketi aktörler için bu sekmeyi kullanın **AcousticsGeometry** veya **AcousticsNavigation** aktörler için etiketler.

Dünya anahat Düzenleyicisi içinde bir veya daha fazla nesne seçin veya kullanın **toplu seçim** belirli bir kategorideki tüm nesnelerin seçmenize yardımcı olması için bölümü. Seçilen nesneler sonra kullanın **etiketleme** seçilen nesneler için istenen etiket uygulamak için bölüm.

Sorun ne varsa **AcousticsGeometry** ya da **AcousticsNavigation** etiketi benzetime yoksayılacak. Yalnızca statik kafesleri nav kafesleri ve ortamlarını desteklenir. Başka bir şey etiketlerseniz göz ardı edilir.

### <a name="for-reference-the-objects-tab-parts"></a>Başvuru için: Nesneleri sekmesini bölümleri

![Unreal sekmede akustik nesnelerin ekran görüntüsü](media/unreal-objects-tab-details.png)

1. Sekme Seçimi düğmelerinin (**nesneleri** sekmesi seçili). Yukarıdan bir akustik hazırlama yapmanın çeşitli adımlarında yol için bu düğmeleri kullanın.
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması.
3. Aktörler düzeyinde kullanılabilir seçicileri.
4. Tıklayarak **seçin** işaretli aktör türlerinden en az birini karşılayan düzeyinde tüm nesneleri seçer.
5. Tıklayarak **Tü** geçerli seçimi temizler. Bu escape tuşuna basmak ile aynıdır.
6. Geometri veya gezinti etiketi için seçilen aktörleri uygulanıp uygulanmayacağını seçin için bu radyo düğmelerini kullanın.
7. Tıklayarak **etiketi** tüm seçili aktörler için seçili etiketi ekler.
8. Tıklayarak **Etiketi Kaldır** seçili etiketi tüm seçili aktörler kaldırır.
9. Tıklayarak **seçin etiketli** geçerli seçimi temizleyin ve şu anda seçili etiketi ile tüm aktörler seçin.
10. Kaç tane aktörler her etiket türü ile etiketlenir istatistiklerine gösterir.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Akustik kapatma ve yansıma geometri etiketi

Akustik penceresinin nesneler sekmesini açın. Bunlar occlude, yansıtacak veya ses etkisini azaltmak, herhangi bir nesne akustik geometri işaretleyin. Akustik geometri, baştan sonra duvarlarının, çatılar, windows & Pencere Cam, rugs ve büyük Mobilya gibi içerebilir. Bu nesneler için rastgele bir karmaşıklık düzeyi kullanabilirsiniz. Sahne voxelized benzetimi önce olduğundan, ağaçlar olan çok sayıda küçük bırakır gibi yüksek oranda ayrıntılı kafesleri Basitleştirilmiş nesneleri hazırlama daha maliyetli değildir.

Görünmez çakışma kafesleri gibi akustik Etkilenme şeyleri dahil değildir.

Bir nesnenin Dönüştür araştırma hesaplama zaman (araştırmaları sekmesi aracılığıyla aşağıda) hazırlama sonuçları sabittir. İşaretli nesnelerden herhangi birini sahnede taşıma araştırma hesaplama yineleme ve Sahne rebaking gerektirir.

### <a name="create-or-tag-a-navigation-mesh"></a>Oluşturma veya gezinti kafes etiketi

Gezinti kafes simülasyonu için araştırma noktaları yerleştirmek için kullanılır. Unreal'ın kullanabileceğiniz [Nav Mesh sınırları birim](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html), ya da kendi gezinti kafes belirtebilirsiniz. En az bir nesne olarak etiketlemeniz gerekir **akustik Gezinti**. Unreal'ın gezinti kafes kullanıyorsanız, öncelikle oluşturduktan emin olun.

### <a name="acoustics-volumes"></a>Akustik birimleri ###

Daha fazla, Gelişmiş özelleştirme, gezinti alanlara olun yoktur **akustik birimleri**. **Akustik birimleri** Gezinti kafes yoksay ve dahil etmek için alanları seçmenize olanak sağlayan görünümünüze ekleyebileceğiniz birer aktördür. Aktör değiştirilebilir bir özellik sunan "Ekle" ve "Dışarıda bırak" arasında. "Ekleme" birimleri, yalnızca alanlarının Gezinti kafes içlerindeki olarak kabul edilir ve yok sayılacak bu alanların "Dışarıda bırak" birimleri işaretleyin emin olun. Birimleri "Dışarıda bırak" sonra "Ekle" birimleri her zaman uygulanır. Etikete emin **akustik birimleri** olarak **akustik Gezinti** nesneleri sekmesindeki her zamanki sürecinde. Bu aktörler ***değil*** otomatik olarak etiketlenmiş.

![Unreal özelliklerinde akustik birim ekran görüntüsü](media/unreal-acoustics-volume-properties.png)

Birimleri "Dışarıda bırak" ağırlıklı olarak nereye yerleştirileceğini olmayan araştırmaları için kaynak kullanım sıkılaştırma üzerinde ayrıntılı denetim vermek yöneliktir.

![Dışlama akustik Unreal biriminde ekran görüntüsü](media/unreal-acoustics-volume-exclude.png)

Birimleri "Ekleme" gibi birden çok akustik bölgelere sahneniz bölmeniz istiyorsanız bir Sahne el ile oluşturma bölümlerini için kullanışlıdır. Örneğin, büyük bir Sahne varsa, birçok kilometre kare ve iki üzerinde akustik hazırlama istediğiniz ilgi alanlarına sahip. Sahnedeki iki büyük "Ekle" birimi çizmek ve bunların her biri için ACE dosyaları üretmek teker teker. Ardından, oyun player her kutucuk yaklaştığında uygun ACE dosya yüklemek için tetikleyici birimleri şema çağrıları ile birlikte kullanabilirsiniz.

**Akustik birimleri** yalnızca Gezinti kısıtlamak ve ***değil*** geometrisi. Her araştırma bir "ekleme" içindeki **akustik birim** hala birim dışında tüm gerekli geometri içinde wave benzetimleri yaparken çeker. Bu nedenle, tüm discontinuities kapanması veya bir bölümünden diğerine geçmeden player kaynaklanan diğer akustik olmamalıdır.

## <a name="select-acoustic-materials"></a>Akustik malzemeleri seçin

Nesnelerinizi etiketlendikten sonra tıklayın **malzemeleri** düğmesini malzemeleri sekmesine gidin. Bu sekme düzeyinde malzemelerin her malzeme özelliklerini belirtmek için kullanılır. Herhangi bir aktör etiketlenir önce boş olur.

Akustik malzemeleri her yüzeyinden yansımasını ses enerji miktarını denetler. İçe alma için somut benzer varsayılan akustik malzeme vardır. Proje akustik Sahne malzeme adına dayalı malzemeleri önerir.

Verilen bir malzeme odasında reverberation süresini 0,01 için 0,20 aralığında içe alma değerlerine sahip çoğu malzemelerle, içe alma katsayısı için inversely ilişkilidir. Bu aralık yukarıda içe alma katsayıları malzemelerle çok absorbent. Örneğin, bir oda sesleri çok reverberant duvarlarının, kat veya tavan akustik malzeme sorun daha yüksek absorptivity tıklarsanız. Akustik malzeme atama, Sahne malzemeyi kullanan tüm aktörler için geçerlidir.

![İçe alma katsayısı ile reverberation zaman negatif bir bağıntı gösteren grafik](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Başvuru için: Malzemeleri sekmesinin bölümleri

![Unreal sekmede akustik nesnelerin ekran görüntüsü](media/unreal-materials-tab-details.png)

1. **Malzemeleri** sekmesini düğmesi, bu sayfasını getirmek için kullanılır.
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması.
3. Aktörleri alınan düzeyinde kullanılan malzeme listesini olarak etiketlenmiş **AcousticsGeometry**. Bir ortama buraya tıklayarak bu malzemeyi kullanan sahnedeki tüm nesneleri seçer.
4. Akustik malzeme Sahne malzeme için atanmış olduğunu gösterir. Farklı bir akustik malzeme için Sahne malzeme atamak için bir açılan tıklayın.
5. Önceki sütununda seçilen malzeme akustik içe alma katsayısı gösterir. Değeri sıfır mükemmel yansıtıcı anlamına gelir (hiçbir içe alma), değeri 1 anlamına gelir mükemmel absorptive (yansıma hiçbir) oluştu. Bu değeri değiştirmek için akustik malzeme (#4. adım) güncelleştirir **özel**.

Malzeme, sahnede değişiklik yaparsanız, sekmeler olarak yansıyan şu değişiklikleri görmek için proje akustik eklentisi anahtarının gerekecektir **malzemeleri** sekmesi.

## <a name="calculate-and-review-listener-probe-locations"></a>Hesaplamak ve dinleyici sondası konumlarını gözden geçirin

Malzemeleri atadıktan sonra geçiş **araştırmaları** sekmesi.

### <a name="for-reference-parts-of-the-probes-tab"></a>Başvuru için: Araştırmalar sekmesinin bölümleri

![Unreal sekmesinde akustik araştırmaları, ekran görüntüsü](media/unreal-probes-tab-details.png)

1. **Araştırmaları** bu sayfasını getirmek için kullanılan sekmesini düğmesi
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması
3. Bu, bir kaba veya iyi benzetimi çözüm seçmek için kullanın. Kaba daha hızlıdır, ancak belirli Artıları ve eksileri vardır. Bkz: [hazırlama çözümleme](bake-resolution.md) altındaki ayrıntılar için.
4. Konum seçin olduğunda bu alanı kullanarak akustik veri dosyalarını yerleştirilmelidir. Klasör Seçici kullanmak için "..." düğmesine tıklayın. Veri dosyaları hakkında daha fazla bilgi için bkz. [veri dosyalarını](#Data-Files) aşağıda.
5. Burada sağlanan ön ek kullanarak bu görünüm için veri dosyalarının adlandırılacaktır. "[Düzeyi adı] _AcousticsData" varsayılandır.
6. Tıklayın **Calculate** Sahne voxelize için düğme ve araştırma noktası konumu hesaplayın. Makinenizde yerel olarak gerçekleştirilir ve bir hazırlama yapmadan önce yapılmalıdır. Araştırmaları hesaplanan, yukarıdaki denetimleri devre dışı bırakılır ve bu düğme söylemek değiştirecek sonra **Temizle**. Tıklayın **Temizle** düğmesine hesaplamaları silmek ve denetimleri etkinleştirin; böylelikle yeni ayarlarla yeniden hesaplayın.

Araştırmalar aracılığıyla sağlanan otomatik işlem yerleştirilmelidir **araştırmaları** sekmesi.


### <a name="what-the-calculate-button-calculates"></a>"Hesapla" düğme hesaplar

**Calculate** düğmesi şimdiye sağladığınızdan tüm verileri alır (geometri, gezinti, malzemeler ve kaba/ince ayar) ve çeşitli adımlarda geçer:

1. Sahne kafesleri geometrisini alır ve voxel toplu hesaplar. Voxel birimin tamamını sahneniz barındırır ve küçük üçüncü dereceden "voxels" yapılan 3 boyutlu bir birimdir. Ayarlanan tarafından simülasyon sıklığı voxels boyutu belirlenir **benzetimi çözümleme** ayarı. Her voxel işaretlenmiş ya da olacak şekilde "havadan"açık"ya da Sahne geometri içeren. Bir voxel geometri içeriyorsa voxel bu geometriye atanmış malzemeleri içe alma katsayısı ile etiketlenir.
2. Player nereye acoustically ilginç konumları hesaplamak için Gezinti verileri kullanır. Daha küçük alanları gibi açılan kapılar ve koridorlarda ve alanları açmak için odalarını içerir makul küçük bir kümesi, bu konumlardan bulmak çalışır. Büyük sahneler kadar bin olabilir, ancak küçük sahneler için genellikle 100'den az konumları budur.
3. Konumların her biri, hesaplar son dinleyici için bu parametreleri nasıl "açık" gibi bir dizi boyutu vb. içinde yer alan olduğunu belirler.
4. Bu hesaplamaların sonuçlarını dosyaları belirttiğiniz konuma depolanır (bkz [veri dosyalarını](#Data-Files) aşağıda)

Sahneniz boyutunu ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

Bu hesaplamalar tamamlandıktan sonra hem voxel verileri hem de hazırlama iyi sonuç verecektir sağlamaya yardımcı olmak için araştırma noktası konumu önizleyebilirsiniz. Hatalı Gezinti kafes gibi şeyler veya Bunu düzeltmek için eksik ek geometri genellikle önizlemede kolayca görülebilir.


## <a name="debug-display"></a>Görüntü hata ayıklama

Araştırma hesaplama tamamlandıktan sonra yeni bir aktör dünya adlı anahat Düzenleyicisi içinde görünür **AcousticsDebugRenderer**. Denetimi **işleme araştırmaları** ve **işleme Voxels** Düzenleyicisi görünüm hata ayıklama görünen onay kutularını etkinleştirir.

![Gösteren ekran görüntüsü akustik hata ayıklama Oluşturucu aktör Unreal Düzenleyicisi'nde](media/acoustics-debug-renderer.png)

Herhangi bir voxels veya üzerinde sizin düzeyinizde yayılan bir araştırmalarla görmüyorsanız, görünüm penceresinin içinde gerçek zamanlı işleme etkin olduğundan emin olun.

![Unreal gerçek zamanlı işleme seçeneğinin ekran görüntüsü](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Voxels

Voxels Sahne penceresinde yeşil küpler katılımcı geometri geçici olarak gösterilir. Yalnızca hava içeren Voxels gösterilmez. Benzetim kullanılacak tam voxel birimi gösterir tüm sahneniz çevresinde bir büyük yeşil kutu yoktur.
Sahneniz taşıyın ve acoustically occluding geometri voxels sahip olduğunu doğrulayın. Ayrıca, bu akustik olmayan çakışma kafesleri voxelized etkinleştirilmemiş gibi nesneleri denetleyin. Kamerayı sahnenin yaklaşık 5 ölçümleri gösterilecek voxels için nesnenin içinde olması gerekir.

Kaba çözümleme vs iyi çözüm ile oluşturulan voxels karşılaştırırsanız kaba voxels iki kez büyük olduğunu görürsünüz.

![Unreal Düzenleyicisi'nde, ekran akustik voxels Önizleme](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Araştırma noktaları

Araştırma noktaları olası oynatıcı (dinleyici) konumları ile eşanlamlıdır. Benzetim, saklanacağı, tüm olası kaynak konumları her araştırma noktasına bağlanmayı akustik hesaplar. Çalışma zamanında dinleyici konumun yakınında araştırma noktaları arasında ilişkilendirilir.

Araştırma noktaları player sahnede seyahat beklenen her yerde mevcut olduğunu denetlemek önemlidir. Araştırma noktaları üzerinde Gezinti kafes proje akustik altyapısı tarafından yerleştirilir ve taşınamaz veya düzenlendi, bu nedenle Gezinti kafes kapsar tüm olası player konumları araştırma noktaları inceleyerek emin olun.

![Ekran görüntüsü, akustik Unreal önizlemede araştırmaları](media/unreal-probes-preview.png)

Bkz: [hazırlama çözümleme](bake-resolution.md) kaba vs hakkında daha fazla ayrıntı için çözüm ince.

## <a name="bake-your-level-using-azure-batch"></a>Azure Batch kullanarak düzeyinizi hazırlama

Azure Batch hizmetini kullanarak bulutta bilgi işlem kümesi ile sahneniz hazırlama. Proje akustik Unreal eklenti oluşturmak, yönetmek ve bir Azure Batch kümesi için her hazırlama kaldırmak için Azure Batch doğrudan bağlanır. Hazırlama sekmesinde, Azure kimlik bilgilerinizi girin, bir küme makine türü ve boyut seçin ve hazırlama'a tıklayın.

### <a name="for-reference-parts-of-the-bake-tab"></a>Başvuru için: Hazırlama sekmesinin bölümleri

![Unreal sekmesinde akustik, ekran görüntüsü hazırlama](media/unreal-bake-tab-details.png)

1. Bu sayfasını getirmek için kullanılan hazırlama için sekmesinde düğme.
2. Kısa bir açıklamasını bu sayfada yapmanız gerekenler.
3. Azure hesabınız oluşturulduktan sonra Azure kimlik bilgilerinizi girmek için alanlar. Daha fazla bilgi için [bir Azure Batch hesabı oluşturma](create-azure-account.md).
4. Aboneliklerinizi yönetin, kullanımını izlemek ve faturalandırma bilgileri vb. görüntülemek için Azure portalını başlatın. 
5. Azure batch işlem düğümü türü hesaplama için kullanılacak. Düğüm türü, Azure veri merkezi konumu tarafından desteklenmesi gerekir. Emin değilim, tutulacaksa **Standard_F8s_v2**.
6. Bu hesaplama için kullanmak için düğüm sayısı. Buraya girdiğiniz numara, Azure Batch çekirdek ayırma ile sınırlıdır ve hazırlama tamamlanma süresi etkiler. Varsayılan ayırma yalnızca iki 8 çekirdek düğümleri veya bir 16 çekirdek düğümü sağlar, ancak genişletilebilir. Çekirdek ayırma kısıtlamaları hakkında daha fazla bilgi için bkz. [bir Azure Batch hesabı oluşturma](create-azure-account.md).
7. Kullanılacak işlem havuzunuzu yapılandırmak için bu onay kutusunu seçin [düşük öncelikli düğümler](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Düşük öncelikli işlem düğümleri çok daha düşük bir maliyet vardır ancak bunlar her zaman kullanılabilir olmayabilir veya hiçbir zaman etkisiz hale getirilebilir.
8. İşinizi bulutta çalıştırmak için için beklenen süre miktarı. Bu düğümü başlangıç süresini içermez. İş çalışmaya başladıktan sonra sonuçlar ulaşırsınız önce hakkında ne kadar süreyle olmalıdır budur. Bu yalnızca bir tahmin olduğunu unutmayın.
9. Simülasyonlar çalıştırılması için gereken bilgi işlem zamanı toplam miktarı. Toplam süre Azure'da kullanılacak düğümü işlem budur. Bkz: [Estimating hazırlama maliyet](#Estimating-bake-cost) aşağıda bu değeri kullanma hakkında daha fazla bilgi için.
10. Buluta hazırlama göndermek için hazırlama düğmesine tıklayın. Bir iş çalışırken bu gösterir **işi iptal et** yerine. Bu sekmede hatalar varsa veya iş akışı **araştırmaları** sekme tamamlanmamış, bu düğmeyi devre dışı bırakılır.
11. Sahneniz hesaplanır olarak için yoklama sayısı **araştırmaları** sekmesi. Yoklama sayısını bulutta çalıştırılması gereken simülasyonlar sayısını belirler. Araştırmalar sayısından daha fazla düğüm belirtemezsiniz.
12. Bu ileti, işin geçerli durumu bildirir veya bu sekmede hatalar varsa, bu hataları nelerdir.

Her zaman etkin işler, işlem havuzlarını ve depolama alanı ile ilgili eksiksiz bilgi alabileceğiniz [Azure portalında](https://portal.azure.com).

Bir iş çalışırken **hazırlama** düğmesi değişiklikleri **işi iptal et**. Devam eden işi iptal etmek için bu düğmeyi kullanın. Bir işi iptal ediliyor alınamaz, sonuç kullanıma sunulacak ve yine de iptal önce kullanılan herhangi bir Azure işlem süresi için ücretlendirilirsiniz.

Bir hazırlama başladıktan sonra Unreal kapatabilirsiniz. Bir bulut hazırlama, proje, düğüm türü ve düğüm sayısına bağlı olarak birkaç saat sürebilir. Projeyi yeniden yükleyin ve akustik penceresini hazırlama iş durumu güncelleştirilir. İş tamamlandığında, çıkış dosyası indirilir.

Azure kimlik bilgileri yerel makinenize güvenli bir şekilde depolanır ve Unreal projenizle ilişkili. Bunlar, yalnızca Azure güvenli bir bağlantı kurmak için kullanılır.

### <a name="Estimating-bake-cost"></a> Azure hazırlama maliyet tahmini

Ne verilen hazırlama maliyetini tahmin etmek için gösterilen değer ele **işlem maliyeti tahmini**bir süresi olan ve birden çok kez tarafından saatlik maliyet yerel para **VM düğüm türü** seçtiniz. Düğümleri duruma getirmek için gereken ve çalışan düğümü zaman sonucu dahil edilmez. Örneğin, **Standard_F8s_v2** düğüm türünüz için olan 0.40 $/ SA maliyeti ve işlem tahmini maliyeti 3 saat 57 dakika, işi çalıştırmak için tahmini maliyeti $0.40 * ~ 4 saat olacak = ~ 1,60$. Gerçek maliyet büyük olasılıkla çalışmaya düğümleri almak için ek süreyi nedeniyle biraz daha yüksek olacaktır. Maliyeti saatlik düğümü bulabilirsiniz [Azure Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux) sayfası ("select işlem için iyileştirilmiş" veya "yüksek performanslı işlem" kategorisi için).

### <a name="reviewing-the-bake-results"></a>Hazırlama sonuçlarını gözden geçirme

Hazırlama işlemi tamamlandıktan sonra çalışma zamanı eklentisi çalıştırarak voxels ve araştırma noktaları beklenen konumlarına olduğunu kontrol edin.

## <a name="Data-Files"></a>Veri dosyaları

Bu eklenti çeşitli noktalarında tarafından oluşturulan dört veri dosyası vardır. Yalnızca bir tanesi çalışma zamanında gereken ve projenizin paketleme yolu otomatik olarak eklenir, projenizin içerik/akustik klasöre yerleştirilir. Diğer üç akustik veri klasörü içinde olan ve olmayan paketlenir.

* **[Project]/Config/ProjectAcoustics.cfg**: Bu dosya akustik modu UI alanlarında girdiğiniz verileri depolar. Bu dosyanın adını ve konumunu değiştirilemez. Hazırlama etkileyen, bu dosyada depolanan diğer değerleri vardır, ancak bunlar Gelişmiş kullanıcılar için ve değiştirilmemesi gerekir.
* **[Project] / içerik/akustik / [LevelName]\_AcousticsData.ace**: Bu, hangi hazırlama benzetimi sırasında oluşturulur ve, sahnenin akustik işlemek için çalışma zamanı tarafından kullanılan arama verileri içeren dosyadır. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi. Oluşturulduktan sonra bu dosyayı yeniden adlandırmak isterseniz, Unreal projenizden UAsset silin, dışında Unreal dosya Gezgini'nde dosyayı yeniden adlandırın ve ardından bu dosyayı yeni UAsset üretmek için Unreal yeniden içeri. UAsset kendisi tarafından yeniden adlandırma çalışmaz.
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData.vox**: Bu dosya, voxelized akustik geometri ve malzeme özelliklerine depolar. Kullanarak hesaplanan **Calculate** düğmesini **araştırmaları** sekmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.
* **[Project] / Plugins/ProjectAcoustics/AcousticsData / [LevelName]\_AcousticsData\_config.xml**: Bu dosya kullanılarak hesaplanır parametreleri depolayan **Calculate** düğmesini **araştırmaları** sekmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.

Azure'dan indirilen *.ace dosyasını silmek için dikkatli olun. Bu dosya Sahne rebaking tarafından dışında kurtarılamaz.

## <a name="next-steps"></a>Sonraki adımlar
* Keşfedin [Unreal için denetimleri tasarım](unreal-workflow.md)
* Keşfedin [proje akustik tasarım kavramları](design-process.md)

