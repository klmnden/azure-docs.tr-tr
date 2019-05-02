---
title: Proje akustik Unity hazırlama Öğreticisi
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, Unity projesi akustik ile saklanacağı akustik açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 2f0fcdcdf781c86179b67eeef0223d46da0fc65b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916994"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Proje akustik Unity hazırlama Öğreticisi
Bu öğreticide, Unity projesi akustik ile saklanacağı akustik açıklanmaktadır.

Yazılım gereksinimleri:
* [Unity 2018.2 +](https://unity3d.com) Windows için
* [Unity projeniz içindeki proje akustik eklentisi tümleşik](unity-integration.md) veya [proje akustik Unity örnek içerik](unity-quickstart.md)
* İsteğe bağlı: Bir [Azure Batch hesabı](create-azure-account.md) bakes hızlandırmak için kullanarak bulut bilgi işlem

## <a name="open-the-project-acoustics-bake-window"></a>Açık proje akustik penceresi hazırlama
Seçin **penceresi > akustik** Unity menüsünde:

![Akustik penceresinde menü seçeneğinin vurgulandığı ekran görüntüsü, Unity Düzenleyicisi](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Bir gezinti ağı oluşturun
Proje akustik Gezinti kafes simülasyonu için dinleyici araştırma noktaları yerleştirmek için kullanır. Unity'nın kullanabilirsiniz [Gezinti kafes iş akışı](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html), veya başka bir 3B modelleme paketi kendi kafes tasarlamak için kullanabilirsiniz. 

## <a name="mark-acoustic-scene-objects"></a>Akustik görünüm nesnelerini işaretle
Görünüm nesnelerini simülasyonu için iki tür proje akustik dayanır: gösterecek ve ses benzetim ve simülasyon dinleyicisi yoklama noktaları kısıtlar player Gezinti kafes occlude nesneleri. Her iki nesne türlerini kullanarak işaretlenen **nesneleri** sekmesi. 

Çünkü nesneleri işaretleme yalnızca ekler **AcousticsGeometry** veya **AcousticsNavigation** bileşenleri de kullanabilirsiniz nesnesine [standart Unity bileşen iş akışı](https://docs.unity3d.com/Manual/UsingComponents.html)işaretleyin veya nesneleri işaretini kaldırın. Yalnızca Mesh oluşturucular ve Terrains işaretlenebilir. Diğer tüm nesne türlerinin göz ardı edilir. Onay kutularını işaretleyin veya tüm etkilenen nesneler işaretini kaldırın.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Akustik kapatma ve yansıma geometri işaretle
Açık **nesneleri** sekmesinde **akustik** penceresi. Herhangi bir nesne olarak işaretlemek **akustik geometri** occlude, yansıtacak veya ses devralarak. Akustik geometri, baştan sonra duvarlarının, çatılar, windows & Pencere Cam, rugs ve büyük Mobilya gibi içerebilir. Bu nesneler için rastgele bir karmaşıklık düzeyi kullanabilirsiniz. Sahne voxelized benzetimi önce olduğundan, ağaçlar olan çok sayıda küçük bırakır gibi yüksek oranda ayrıntılı kafesleri Basitleştirilmiş nesneleri hazırlama daha maliyetli değildir.

Görünmez çakışma kafesleri gibi akustik Etkilenme şeyleri dahil değildir.

Bir nesnenin Dönüştür araştırma hesaplama zaman (aracılığıyla **araştırmaları** sekmesinde, aşağıdaki) hazırlama sonuçları sabittir. İşaretli nesnelerden herhangi birini sahnede taşıma araştırma hesaplama yineleme ve Sahne rebaking gerektirir.

### <a name="mark-the-navigation-mesh"></a>Gezinti kafes işaretle
Unity'nın iş akışı ile oluşturulan gezinti kafesleri akustik sistem tarafından seçilir. Kendi kafesleri kullanmak için onlardan işaretlemek **nesneleri** sekmesi.

### <a name="for-reference-the-objects-tab-parts"></a>Başvuru için: Nesneleri sekmesini bölümleri
Sekme sayfasındaki bölümleri şunlardır:

1. Sekme Seçimi düğmelerinin (**nesneleri** sekmesi seçili). Soldan sağa doğru bir akustik hazırlama yapmanın çeşitli adımlarında yol için bu düğmeleri kullanın.
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması.
3. Kullanılabilir filtrelerin hiyerarşisi penceresi için. Belirtilen türe ait nesneleri için hiyerarşi penceresinde daha kolay işaretlerken şekilde filtrelemek için bunu kullanın. Henüz herhangi bir şey akustik için işaretlenen değil, son iki seçenekleri belirleyerek, hiçbir şey gösterilmektedir. Ancak, bunlar yaptıktan sonra işaretli nesneleri bulmak yararlı olabilir.
4. Hiçbir nesne seçili olduğunda, bu bölümde sahnedeki tüm nesneleri durumunu gösterir:
    * Toplam - etkin, gizli olmayan nesneleri Sahne içinde toplam sayısı.
    * Yoksayıldı - Mesh oluşturucu veya Terrains olmayan nesne sayısı.
    * Mesh - Mesh Oluşturucu sayısını sahnedeki nesneler
    * Sahnedeki Arazi - nesnelerinin Arazi sayısı
    * Geometri - kafes veya Arazi sayısı "Akustik geometri" işaretlenmiş sahnedeki nesneleri.
    * Gezinti - kafes veya Arazi sayısı "Akustik gezinti" işaretlenmiş sahnedeki nesneler. Bu sayı, Unity'nın NavMesh içermez.
5. Yalnızca Mesh oluşturucular ve Terrains görünümde 'işareti mümkün' nesne toplam sayısını gösterir. Onay kutularını işaretleyin için kullanabileceğiniz gösterir (uygun bileşenine ekleme) nesnelerle geometri veya akustik gezinme
6. Bu Not, hiçbir şey seçili olduğunda, gerekirse işaretleme nesneleri seçmenizi anımsatır. Ayrıca, hiçbir şey seçmeden sahnedeki tüm nesneleri işaretlemek için bir veya iki onay kutularını kontrol edebilirsiniz.
7. Bu bölüm, nesne seçiliyken, yalnızca seçilen nesnelerin durumunu gösterir.
8. 'İşareti mümkün' seçili nesneler toplam sayısını gösterir. Denetleme veya onay kutularının işaretini işaretleyin veya yalnızca seçili nesneler işaretini kaldırın.

Hiçbir şey, sahnede seçili varsa, nesneler sekmesini aşağıdaki resim gibi görünecektir:

![Seçim sekmesiyle akustik nesnelerin ekran görüntüsü](media/objects-tab-no-selection-detail.png)

Sahne veya hiyerarşi penceresinde seçilen bir şey varsa, aşağıdaki resim gibi görünecektir:

![Ekran görüntüsü, akustik nesnelerin sekmesiyle gösterilen seçimi](media/objects-tab-selection-detail.png)

Bazı nesneler işaretlenir ve bazı değil, uygun onay kutusunu "karma" bir değer gösterir:

![Vurgulanan karma seçim simgesi sekmesiyle akustik nesnelerin ekran görüntüsü](media/mixed-object-selection-detail.png)

Onay kutusuna tıklayarak işaretlenmesi için tüm nesneleri zorlar ve tıklayarak tekrar tüm nesneleri işaretini.

Nesneleri, geometri ve gezinti için işaretlenebilir.

## <a name="select-acoustic-materials"></a>Akustik malzemeleri seçin
Nesnelerinizi işaretlenmiş bitince **malzemeleri** düğmesi ve akustik malzemeleri atayın. Proje akustik malzemeleri sistem Unity visual malzemeleri sisteme bağlıdır: ayrı akustik malzemeler iki nesne için ayrı görsel malzeme olması gerekir.

Akustik malzemeleri her yüzeyinden yansımasını ses enerji miktarını denetler. İçe alma için somut benzer varsayılan akustik malzeme vardır. Proje akustik görsel malzeme adına dayalı malzemeleri önerir. Akustik malzeme 'Custom' bir içe alma katsayısı kaydırıcı etkinleştirmek için bir malzeme atayabilirsiniz.

Verilen bir malzeme odasında reverberation süresini 0,01 için 0,20 aralığında içe alma değerlerine sahip çoğu malzemelerle, içe alma katsayısı için inversely ilişkilidir. Bu aralığın dışında kalan içe alma katsayıları malzemelerle çok absorbent.

![İçe alma katsayısı ile reverberation zaman negatif bir bağıntı gösteren grafik](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Başvuru için: Malzemeleri sekmesinin bölümleri
![Unity sekmede akustik malzemeleri ekran görüntüsü](media/materials-tab-detail.png)

1. **Malzemeleri** sekmesini düğmesi, bu sayfasını getirmek için kullanılır.
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması.
3. Bu onay kutusu işaretlendiğinde, nesneler tarafından kullanılan malzemeleri olarak işaretlenmiş **akustik geometri** listelenir. Aksi takdirde, sahnedeki kullanılan tüm malzemeleri listelenir.
4. Açılan liste akustik sütunun (6) altında tıkladığınızda görüntülenen açılır menüsünde sırasını değiştirmek için bu seçenekleri kullanın. **Adı** akustik malzemeleri ada göre sıralar. "Absorptivity" bunları absorptivity düşükten yükseğe sırasına göre sıralar.
5. Alfabetik olarak sıralanmış sahnede kullanılan malzeme listesi. Varsa **yalnızca Göster işaretlenmiş** onay kutusu işaretli (#3), olarak işaretlenmiş nesneler tarafından kullanılan malzemeleri **akustik geometri** gösterilir. Bir ortama buraya tıklayarak bu malzemeyi kullanan sahnedeki tüm nesneleri seçer.
6. Akustik malzeme Sahne malzeme için atanmış olduğunu gösterir. Farklı bir akustik malzeme için Sahne malzeme atamak için bir açılan tıklayın. Burada kullanarak bir öğe tıkladığınızda gösterilen menüsünün sıralama düzenini değiştirebilirsiniz **akustik sıralama ölçütü:** (4) yukarıda seçenekleri.
7. Önceki sütununda seçilen malzeme akustik içe alma katsayısı gösterir. Değeri sıfır mükemmel yansıtıcı anlamına gelir (hiçbir içe alma), değeri 1 anlamına gelir mükemmel absorptive (yansıma hiçbir) oluştu. İçe alma katsayısı, seçili malzeme "Özel" olmadığı sürece değiştirilemez.
8. "Özel", kaydırıcıyı atanmış bir malzeme artık devre dışıdır ve kaydırıcıyı kullanarak içe alma katsayısı seçebilir veya bir değer yazarak.

## <a name="calculate-and-review-listener-probe-locations"></a>Hesaplamak ve dinleyici sondası konumlarını gözden geçirin
Malzemeleri atadıktan sonra geçiş **araştırmaları** sekmesine **Calculate** simülasyonu için dinleyici araştırma noktaları yerleştirmek için.

### <a name="what-the-calculate-button-calculates"></a>"Hesapla..." düğmesini hesaplar
**Hesaplamak...**  düğmesi sahneniz benzetimi için hazırlamak için seçilen akustik Sahne geometri kullanır:

1. Sahne kafesleri geometrisini alır ve voxel toplu hesaplar. Voxel birimin tamamını sahneniz barındırır ve küçük üçüncü dereceden "voxels" yapılan 3 boyutlu bir birimdir. Ayarlanan tarafından simülasyon sıklığı voxels boyutu belirlenir **benzetimi çözümleme** ayarı. Her voxel işaretlenmiş ya da olacak şekilde "havadan"açık"ya da Sahne geometri içeren. Bir voxel geometri içeriyorsa voxel bu geometriye atanmış malzemeleri içe alma katsayısı ile etiketlenir.
2. Gezinti mesh(es) dinleyicisi yoklama noktaları yerleştirmek için kullanır. Algoritma corridors daraltmak rakip kaygıları sağlarken uzamsal kapsamı ve simülasyon saat ve dosya boyutu, dengeler ve küçük alanları her zaman miktar kapsama sahip. Tipik bir araştırma noktası birkaç bin büyük sahneler için küçük sahneler için 100'den aralığa sayar.

Sahneniz boyutunu ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

### <a name="review-voxel-and-probe-placement"></a>Gözden geçirme voxel ve araştırma yerleştirme
Hem voxel verileri hem de sahneniz hazırlama hazır olmak için araştırma noktası konumu önizlemesini görüntüleyin. Tamamlanmamış Gezinti kafes ya da eksik veya ek akustik geometri genellikle önizlemede hızlı bir şekilde görünür. Voxel ve araştırma yerleştirme etkinleştirilebilir veya şeyler menüsünü kullanarak devre dışı:

![Unity menüde, ekran şeyler](media/gizmos-menu.png)

Akustik geometri içeren Voxels yeşil küpleri olarak gösterilir. Sahneniz keşfedin ve geometri olması gereken her şeyi voxels sahip olduğunu doğrulayın. Kamerayı sahnenin yaklaşık 5 ölçümleri gösterilecek voxels için nesnenin içinde olması gerekir.

Kaba çözümleme vs iyi çözüm ile oluşturulan voxels karşılaştırırsanız kaba voxels iki kez büyük olduğunu görürsünüz.

![Unity Düzenleyicisi'nde kaba voxels önizleme görüntüsü](media/voxel-cubes-preview.png)

Simülasyon sonuçlarını, çalışma zamanında dinleyicisi yoklama noktası konumu arasında ilişkilendirilmiş. Oyuncu sahnede seyahat beklenen araştırma noktası herhangi bir yerde yakın denetleyin.

![Unity Düzenleyicisi önizleme görüntüsü araştırmaları](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Sahne yeniden adlandırma ile ilgileniriz
Görünüm adı, Sahne voxelization ve araştırma noktası yerleştirme depolama dosyalara bağlanmak için kullanılır. Araştırma noktaları hesaplandıktan sonra Sahne adlandırılırsa malzeme atama ve yerleştirme veriler kaybedilir ve yeniden çalıştırılmalıdır.

### <a name="for-reference-parts-of-the-probes-tab"></a>Başvuru için: Araştırmalar sekmesinin bölümleri
![Unity sekmesinde akustik araştırmaları, ekran görüntüsü](media/probes-tab-detail.png)

1. **Araştırmaları** bu sayfasını getirmek için kullanılan sekmesini düğmesi
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması
3. Bu, bir kaba veya iyi benzetimi çözüm seçmek için kullanın. Kaba daha hızlıdır, ancak belirli Artıları ve eksileri vardır. Bkz: [hazırlama çözümleme](bake-resolution.md) altındaki ayrıntılar için.
4. Konum seçin olduğunda bu alanı kullanarak akustik veri dosyalarını yerleştirilmelidir. Klasör Seçici kullanmak için "..." düğmesine tıklayın. Varsayılan değer **varlıklar/AcousticsData**. Bir **Düzenleyicisi** alt bu konumu altında aynı zamanda oluşturulur. Veri dosyaları hakkında daha fazla bilgi için bkz. [veri dosyalarını](#Data-Files) aşağıda.
5. Burada sağlanan ön ek kullanarak bu görünüm için veri dosyalarının adlandırılacaktır. "[Sahne adı] Acoustics_" varsayılandır.
6. Araştırmaları hesapladıktan sonra yukarıdaki denetimleri devre dışı bırakılır. Tıklayın **Temizle** düğmesine hesaplamaları silmek ve denetimleri etkinleştirin; böylelikle yeni ayarlarla yeniden hesaplayın.
7. Tıklayın **hesaplamak...**  Sahne voxelize için düğme ve araştırma noktası konumu hesaplayın. Makinenizde yerel olarak gerçekleştirilir ve bir hazırlama yapmadan önce yapılmalıdır.

Proje akustik'ın bu sürümünde, el ile yerleştirildiğinde araştırmalarla sağlanan otomatik sürecinde yerleştirilmelidir **araştırmaları** sekmesi.

Bkz: [hazırlama çözümleme](bake-resolution.md) kaba vs hakkında daha fazla ayrıntı için çözüm ince.

## <a name="bake-your-scene-using-azure-batch"></a>Azure Batch kullanırken görünümünüze hazırlama
Azure Batch hizmetini kullanarak bulutta bilgi işlem kümesi ile sahneniz hazırlama. Proje akustik Unity eklenti oluşturmak, yönetmek ve bir Azure Batch kümesi için her hazırlama kaldırmak için Azure Batch doğrudan bağlanır. Üzerinde **hazırlama** sekmesinde, Azure kimlik bilgilerinizi girin, bir küme makine türü ve boyut seçin ve tıklayın **hazırlama**.

### <a name="for-reference-parts-of-the-bake-tab"></a>Başvuru için: Hazırlama sekmesinin bölümleri
![Unity sekmesinde akustik, ekran görüntüsü hazırlama](media/bake-tab-details.png)

1. Bu sayfasını getirmek için kullanılan hazırlama için sekmesinde düğme.
2. Kısa bir açıklamasını bu sayfada yapmanız gerekenler.
3. Azure hesabınız oluşturulduktan sonra Azure kimlik bilgilerinizi girmek için alanlar. Daha fazla bilgi için [bir Azure Batch hesabı oluşturma](create-azure-account.md).
4. Akustik araç takımı için docker görüntü etiketi.
5. Aboneliklerinizi yönetin, kullanımını izlemek ve faturalandırma bilgileri vb. görüntülemek için Azure portalını başlatın. 
6. Azure batch işlem düğümü türü hesaplama için kullanılacak. Düğüm türü, Azure veri merkezi konumu tarafından desteklenmesi gerekir. Emin değilim, tutulacaksa **Standard_F8s_v2**.
7. Bu hesaplama için kullanmak için düğüm sayısı. Buraya girdiğiniz numara, Azure Batch çekirdek ayırma ile sınırlıdır ve hazırlama tamamlanma süresi etkiler. Varsayılan ayırma yalnızca iki 8 çekirdek düğümleri veya bir 16 çekirdek düğümü sağlar, ancak genişletilebilir. Çekirdek ayırma kısıtlamaları hakkında daha fazla bilgi için bkz. [bir Azure Batch hesabı oluşturma](create-azure-account.md).
8. Kullanılacak işlem havuzunuzu yapılandırmak için bu onay kutusunu seçin [düşük öncelikli düğümler](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Düşük öncelikli işlem düğümleri çok daha düşük bir maliyet vardır ancak bunlar her zaman kullanılabilir olmayabilir veya hiçbir zaman etkisiz hale getirilebilir.
9. Sahneniz hesaplanır olarak için yoklama sayısı **araştırmaları** sekmesi. Yoklama sayısını bulutta çalıştırılması gereken simülasyonlar sayısını belirler. Araştırmalar sayısından daha fazla düğüm belirtemezsiniz.
10. Geçen süreyi, bulutta çalıştırmak işinizi olması beklenir. Bu düğümü başlangıç süresini içermez. İş çalışmaya başladıktan sonra sonuçlar ulaşırsınız önce hakkında ne kadar süreyle olmalıdır budur. Bu yalnızca bir tahmin olduğunu unutmayın.
11. Simülasyonlar çalıştırılması için gereken bilgi işlem zamanı toplam miktarı. Toplam süre Azure'da kullanılacak düğümü işlem budur. Bkz: [Estimating hazırlama maliyet](#Estimating-bake-cost) aşağıda bu değeri kullanma hakkında daha fazla bilgi için.
12. Bu ileti, iş tamamlandığında hazırlama sonuçlarını kaydedileceği bildirir.
13. (Yalnızca Gelişmiş kullanım) Bir hazırlama hakkında unutmak çok Unity zorlamak için gereksinim duyduğunuz herhangi bir nedenle gönderilmesi durumunda (örneğin başka bir makineyi kullanmayı sonuçları yüklenir), tıklayın **Temizle durumu** gönderilen işiyle ilgili unutmak çok düğmesi. Bu sonuç dosyası, hazır olduğunuzda anlamına gelir, olur **değil** indirebilir, ve **bu işi iptal ediliyor dosyasındakiyle aynı olmaması**. İşi çalışıyorsa, bulutta çalışmaya devam eder.
14. Tıklayın **hazırlama** buluta hazırlama gönderme düğmesi. Bir iş çalışırken bu gösterir **işi iptal et** yerine.
15. İşleme için hazırlar [akustik benzetimi PC'nizde](#Local-bake).
16. Bu alan hazırlama durumunu gösterir. Tamamlandığında, göstermelidir **yüklenen**.

Her zaman etkin işler, işlem havuzlarını ve depolama alanı ile ilgili eksiksiz bilgi alabileceğiniz [Azure portalında](https://portal.azure.com).

Bir iş çalışırken **hazırlama** düğmesi değişiklikleri **işi iptal et**. Devam eden işi iptal etmek için bu düğmeyi kullanın. İşi iptal edilmeden önce onaylayın istenir. Bir işi iptal ediliyor alınamaz, sonuç kullanıma sunulacak ve hala kullanılan herhangi bir Azure işlem süresi için ücretlendirilirsiniz.

Bir hazırlama başladıktan sonra Unity kapatabilirsiniz. Bir bulut hazırlama, proje, düğüm türü ve düğüm sayısına bağlı olarak birkaç saat sürebilir. Projeyi yeniden yükleyin ve akustik penceresini hazırlama iş durumu güncelleştirilir. İş tamamlandığında, çıkış dosyası indirilir.

Azure kimlik bilgileri yerel makinenize güvenli bir şekilde depolanır ve Unity editor'ınızdaki ile ilişkili. Bunlar, yalnızca Azure güvenli bir bağlantı kurmak için kullanılır.

### <a name="Estimating-bake-cost"></a> Azure hazırlama maliyet tahmini

Ne verilen hazırlama maliyetini tahmin etmek için gösterilen değer ele **işlem maliyeti tahmini**bir süresi olan ve birden çok kez tarafından saatlik maliyet yerel para **VM düğüm türü** seçtiniz. Düğümleri duruma getirmek için gereken ve çalışan düğümü zaman sonucu dahil edilmez. Örneğin, **Standard_F8s_v2** düğüm türünüz için olan 0.40 $/ SA maliyeti ve işlem tahmini maliyeti 3 saat 57 dakika, işi çalıştırmak için tahmini maliyeti $0.40 * ~ 4 saat olacak = ~ 1,60$. Gerçek maliyet büyük olasılıkla çalışmaya düğümleri almak için ek süreyi nedeniyle biraz daha yüksek olacaktır. Maliyeti saatlik düğümü bulabilirsiniz [Azure Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux) sayfası ("select işlem için iyileştirilmiş" veya "yüksek performanslı işlem" kategorisi için).

## <a name="Local-bake"></a> Sahneniz PC'nizde hazırlama
Kendi bilgisayarınıza sahneniz hazırlama. Bu, bir Azure Batch hesabı oluşturmadan önce akustik küçük sahneler ile denemek için yararlı olabilir. Not akustik benzetim Sahne boyutuna bağlı olarak uzun zaman alabilir.

### <a name="minimum-hardware-requirements"></a>En düşük donanım gereksinimleri
* En az 8 çekirdek ve 32 GB RAM ile bir x86 64 işlemcisi

8 çekirdek makineye sahip Intel Xeon E5-1660 @ 3 GHz ve 32 GB RAM test işlemlerimizi içinde örnek olarak-
* 100 araştırmaları ile küçük bir Sahneyi yaklaşık 2 saat kaba bir hazırlama veya 32 saate kadar iyi hazırlama sürebilir.
* Orta ölçekli bir Sahne 1000 araştırmaları ile yaklaşık 20 saat kaba bir hazırlama için veya iyi hazırlama 21 gün sürebilir.

### <a name="setup-docker"></a>Docker Kurulumu
Yükleme ve Docker benzetim işleyen bilgisayarda yapılandırma-
1. Yükleme [Docker araç takımı](https://www.docker.com/products/docker-desktop).
2. Docker ayarları başlatın, "Gelişmiş" seçeneğine gidin ve en az 8 GB RAM'e sahip için kaynakları yapılandırma. Daha fazla CPU'ları için Docker ayırabilirsiniz, hazırlama daha hızlı tamamlanır. ![Örnek Docker ayarları görüntüsü](media/docker-settings.png)
3. "Paylaşılan sürücüler için" gidin ve işleme için kullanılan sürücü için paylaşımı etkinleştirin.![Paylaşılan Docker ekran sürücü seçenekleri](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Yerel çalışma hazırlama
1. "Hazırlama yerel Hazırlama" düğmesine tıklayın **hazırlama** sekme ve burada yürütme komut dosyaları ve giriş dosyalarının kaydedileceği klasörü seçin. Ardından hazırlama en düşük donanım gereksinimlerini karşıladığından ve bu makineye klasörüne kopyalayarak Docker yüklü olduğu sürece herhangi bir makinede çalıştırabilirsiniz.
2. "Runlocalbake.bat" komut dosyası kullanarak bir simülasyon başlatın. Bu betik, proje akustik Docker görüntüsünü simülasyon işlemi için gerekli araç takımıyla getirmek ve benzetimi Başlat. 
3. Benzetim tamamlandıktan sonra elde edilen .ace dosyası, Unity projenize kopyalayın. Unity bu ikili dosya olarak tanır emin olmak için dosya uzantısı için (örneğin, "Scene1.ace.bytes") ".bytes" ekleyin. Simülasyonu için ayrıntılı günlük "AcousticsLog.txt" depolanır Herhangi bir sorunla karşılaşırsanız çalıştırırsanız, tanı koymaya yardımcı olmak için bu dosyayı paylaşın.

## <a name="Data-Files"></a> Hazırlama işlemi tarafından eklenen veri dosyaları

Hazırlama işlemi sırasında oluşturulan dört veri dosyası vardır. Benzetim sonuçlarını içeren ve başlığınızı ile birlikte gelir. Diğerleri, Unity Editor ile ilgili verileri depolar.

Benzetim sonucu:
* **Varlıklar/AcousticsData/akustik\_[SceneName].ace.bytes**: Bu çalışma zamanı arama tablosundan ve simülasyon sonuçlarını ve voxelized akustik Sahne öğeleri içerir. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.

Benzetim sonuç dosyası silmemeyi dikkat edin. Sahne rebaking tarafından büyük dışında kurtarılabilir değildir.

Düzenleyici veri dosyaları:
* **Varlıklar/Düzenleyici / [SceneName]\_AcousticsParameters.asset**: Bu dosya akustik UI alanlarında girdiğiniz verileri depolar. Bu dosyanın adını ve konumunu değiştirilemez.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox**: Bu dosya voxelized akustik geometri ve malzeme özellikleri kullanılarak hesaplanır depolar **hesapla...**  araştırmaları sekmesindeki düğmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.
* **Varlıklar/AcousticsData/Düzenleyicisi/akustik\_[SceneName]\_config.xml**: Bu dosya kullanılarak hesaplanır benzetimi parametreleri depolar **hesapla...**  düğmesini **araştırmaları** sekmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.

## <a name="set-up-the-acoustics-lookup-table"></a>Akustik arama tablosu ayarlama
Sürükle ve bırak **proje akustik** sahneniz içine Proje panelinden prefab:

![Ekran görüntüsü, akustik prefab Unity](media/acoustics-prefab.png)

Tıklayarak **ProjectAcoustics** Game nesne ve denetçisi panelini Git. Hazırlama sonuç (. konumunu belirtin ACE dosyası **varlıklar/AcousticsData**) sürükleme ve bırakma tarafından akustik Manager betiğe veya metin kutusunun yanındaki daireye düğmesine tıklayarak.

![Unity prefab akustik Yöneticisi ekran görüntüsü](media/acoustics-manager.png)  

## <a name="next-steps"></a>Sonraki adımlar
* Keşfedin [Unity için denetimleri tasarım](unity-workflow.md)
* Keşfedin [proje akustik tasarım kavramları](design-process.md)

