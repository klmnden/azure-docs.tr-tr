---
title: Akustik proje akustik - Bilişsel hizmetler ile hazırlama
description: Bu belgede, Unity editor uzantısını kullanarak bir akustik hazırlama gönderme işlemi açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: a82472ccd5524e7cbe3d92070a6d2b583d8eb4d5
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249307"
---
# <a name="bake-acoustics"></a>Akustik hazırlama

Bu belgede, Unity editor uzantısını kullanarak bir akustik hazırlama gönderme işlemi açıklanmaktadır. Akustik hakkında daha fazla bilgiler için bkz. [akustik nedir](what-is-acoustics.md). Hızlı Başlangıç Kılavuzu için bkz: [Başlarken](getting-started.md).

## <a name="import-the-plugin"></a>Eklenti Al

Akustik eklenti paketi projenize içeri aktarın. Akustik UI seçerek açılacağını **penceresi > akustik** Unity menüsünde:

![Açık akustik penceresi](media/WindowAcoustics.png)

## <a name="principles"></a>İlkeleri

Akustik araç penceresi altyapısı, Sahne için akustik hesaplamak gereken bilgileri akustik toplar. Bir hazırlama yapmaya beş adım vardır:

1. Oluşturma veya, oyuncu Gezinti kafes işaretleyin
2. Akustik geometri işaretle
3. Geometriye akustik malzemeleri özellikler atama
4. Önizleme araştırma yerleştirme
5. Hazırlama

Hazırlama işlemi tamamlandıktan sonra bkz [tasarım işlemine genel bakış için akustik](design-process.md) sonrası hazırlama tasarım isteğe bağlı adımlar.

## <a name="create-or-mark-a-navigation-mesh"></a>Oluşturma veya gezinti kafes işaretleyin

Gezinti kafes simülasyonu için araştırma noktaları yerleştirmek için kullanılır. Unity'nın kullanabilirsiniz [Gezinti kafes iş akışı](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html), ya da kendi gezinti kafes belirtebilirsiniz. Unity'nın iş akışı ile oluşturulan gezinti kafesleri akustik sistem tarafından seçilir. Kendi kafesleri kullanmak için onlardan işaretlemek **nesneleri** sekmesinde sonraki adımda açıklandığı gibi.

## <a name="objects-tab"></a>Nesneleri sekmesi

Açık **nesneleri** sekmesinde **akustik** penceresi. Yalnızca ekler, sahnedeki nesneler işaretlemek için bu sekmeyi kullanın **AcousticsGeometry** veya **AcousticsNavigation** nesnesine bileşenleri. Ayrıca [standart Unity bileşen iş akışı](https://docs.unity3d.com/Manual/UsingComponents.html) işaretleyin veya nesneleri işaretini kaldırın.

Sahne penceresinde veya hiyerarşi içinde bir veya daha fazla nesne seçin ve ardından işaretleme veya bunlar için işaretini **AcousticsGeometry** veya **AcousticsNavigation** aşağıda açıklandığı gibi. Hiçbir şey seçili işaretleyin veya sahnedeki her şey tek seferde işaretini kaldırın.

Yalnızca Mesh oluşturucular ve Terrains işaretlenebilir. Diğer tüm nesne türlerinin göz ardı edilir. Onay kutularını işaretleyin veya tüm etkilenen nesneler işaretini kaldırın.

Hiçbir şey, sahnede seçili varsa, aşağıdaki resim gibi görünecektir:

![Seçim nesneleri sekmesi](media/ObjectsTabNoSelectionDetail.png)

Sahne veya hiyerarşi penceresinde seçilen bir şey varsa, aşağıdaki resim gibi görünecektir:

![Seçim nesneleri sekmesi](media/ObjectsTabWithSelectionDetail.png)

### <a name="objects-tab-parts"></a>Nesne bölümleri sekmesi

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

Bazı nesneler işaretlenir ve bazı değil, uygun onay kutusunu "karma" bir değer gösterir:

![Karma değer onay kutusu](media/MixedObjectSelectionDetail.png)

Onay kutusuna tıklayarak işaretlenmesi için tüm nesneleri zorlar ve tıklayarak tekrar tüm nesneleri işaretini.

Nesneleri, geometri ve gezinti için işaretlenebilir.

### <a name="guidelines-for-marking-objects"></a>Nesneleri işaretlemek için yönergeleri

Herhangi bir nesne olarak işaretlediğinizden emin olun **akustik geometri** occlude, yansıtacak veya ses devralarak. Akustik geometri, baştan sonra duvarlarının, çatılar, windows & Pencere Cam, rugs ve büyük Mobilya gibi içerebilir. Maliyet hazırlama appreciably artırmaz gibi ampul, dekoratif öğeleri aydınlatma armatürleri gibi daha küçük nesneleri eklemek uygundur. Hızlı bir başlangıç veya bir tavan gibi önemli öğeleri eksik önemlidir. Ayrıca, çakışma kafesleri gibi akustik Etkilenme şeyleri dahil değildir.

Bir nesnenin Dönüştür araştırma hesaplama zaman (aracılığıyla **araştırmaları** sekmesinde, aşağıdaki) hazırlama sonuçları sabittir. İşaretli nesnelerden herhangi birini sahnede taşıma araştırma hesaplama yineleme ve Sahne rebaking gerektirir.

## <a name="materials-tab"></a>Malzemeleri sekmesi

Nesnelerinizi işaretlenmiş bitince **malzemeleri** düğmesini malzemeleri sekmesine gidin.

### <a name="parts-of-the-materials-tab"></a>Malzemeleri sekmesinin bölümleri

![Malzemeleri sekmesi ayrıntısı](media/MaterialsTabDetail.png)

1. **Malzemeleri** sekmesini düğmesi, bu sayfasını getirmek için kullanılır.
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması.
3. Bu onay kutusu işaretlendiğinde, nesneler tarafından kullanılan malzemeleri olarak işaretlenmiş **akustik geometri** listelenir. Aksi takdirde, sahnedeki kullanılan tüm malzemeleri listelenir.
4. Açılan liste akustik sütunun (6) altında tıkladığınızda görüntülenen açılır menüsünde sırasını değiştirmek için bu seçenekleri kullanın. **Adı** akustik malzemeleri ada göre sıralar. "Absorptivity" bunları absorptivity düşükten yükseğe sırasına göre sıralar.
5. Alfabetik olarak sıralanmış sahnede kullanılan malzeme listesi. Varsa **yalnızca Göster işaretlenmiş** onay kutusu işaretli (#3), olarak işaretlenmiş nesneler tarafından kullanılan malzemeleri **akustik geometri** gösterilir. Bir ortama buraya tıklayarak bu malzemeyi kullanan sahnedeki tüm nesneleri seçer.
6. Akustik malzeme Sahne malzeme için atanmış olduğunu gösterir. Farklı bir akustik malzeme için Sahne malzeme atamak için bir açılan tıklayın. Burada kullanarak bir öğe tıkladığınızda gösterilen menüsünün sıralama düzenini değiştirebilirsiniz **akustik sıralama ölçütü:** (4) yukarıda seçenekleri.
7. Önceki sütununda seçilen malzeme akustik içe alma katsayısı gösterir. Değeri sıfır mükemmel yansıtıcı anlamına gelir (hiçbir içe alma), değeri 1 anlamına gelir mükemmel absorptive (yansıma hiçbir) oluştu. İçe alma katsayısı, seçili malzeme "Özel" olmadığı sürece değiştirilemez.
8. "Özel", kaydırıcıyı atanmış bir malzeme artık devre dışıdır ve kaydırıcıyı kullanarak içe alma katsayısı seçebilir veya bir değer yazarak. Malzeme özellikleri hakkında daha fazla bilgi için bkz. [tasarım işlemine genel bakış için akustik](design-process.md).

### <a name="guidelines-for-assigning-materials-or-absorption-values"></a>Malzeme (veya içe alma değerleri) atama yönergeleri

Bu sekme, içe alma, malzemelerin nedir tahmin dener adına göre. Örneğin, Sahne malzeme adınız LivingRoom_WoodTable ise, kendisine atanmış ilk akustik malzeme "Ahşap" olacaktır. Burada bilinen malzeme belirlenemiyor malzemeler, içe alma için somut benzer olan "Varsayılan" malzemesini atanır.

Sahne malzemelerin her akustik Materyaller yeniden atayabilirsiniz. Örneğin, bir oda sesleri çok reverberant duvarlarının, kat veya tavan akustik malzeme sorun daha yüksek absorptivity tıklarsanız. Akustik malzeme atama, Sahne malzemeyi kullanan tüm nesnelere uygulanır.

## <a name="probes-tab"></a>Araştırmalar sekmesi

Malzemeleri atadıktan sonra geçiş **araştırmaları** sekmesi.

### <a name="parts-of-the-probes-tab"></a>Araştırmalar sekmesinin bölümleri

![Araştırmalar sekmesi ayrıntısı](media/ProbesTabDetail.png)

1. **Araştırmaları** bu sayfasını getirmek için kullanılan sekmesini düğmesi
2. Bu sayfayı kullanarak yapmanız gerekenler kısa bir açıklaması
3. Bu, bir kaba veya iyi benzetimi çözüm seçmek için kullanın. Kaba daha hızlıdır, ancak belirli Artıları ve eksileri vardır. Bkz: ["kaba vs ince Çözümleme"](#Coarse-vs-Fine-Resolution) altındaki ayrıntılar için.
4. Konum seçin olduğunda bu alanı kullanarak akustik veri dosyalarını yerleştirilmelidir. Klasör Seçici kullanmak için "..." düğmesine tıklayın. Varsayılan değer **varlıklar/AcousticsData**. Bir **Düzenleyicisi** alt bu konumu altında aynı zamanda oluşturulur. Veri dosyaları hakkında daha fazla bilgi için bkz. ["Veri dosyaları"](#Data-Files) aşağıda.
5. Burada sağlanan ön ek kullanarak bu görünüm için veri dosyalarının adlandırılacaktır. "[Sahne adı] Acoustics_" varsayılandır.
6. Araştırmaları hesapladıktan sonra yukarıdaki denetimleri devre dışı bırakılır. Tıklayın **Temizle** düğmesine hesaplamaları silmek ve denetimleri etkinleştirin; böylelikle yeni ayarlarla yeniden hesaplayın.
7. Tıklayın **hesaplamak...**  Sahne voxelize için düğme ve araştırma noktası konumu hesaplayın. Makinenizde yerel olarak gerçekleştirilir ve bir hazırlama yapmadan önce yapılmalıdır.

Proje akustik'ın bu sürümünde, el ile yerleştirildiğinde araştırmalarla sağlanan otomatik sürecinde yerleştirilmelidir **araştırmaları** sekmesi.

### <a name="what-the-calculate-button-calculates"></a>"Hesapla..." düğmesini hesaplar

**Hesaplamak...**  düğmesi şimdiye sağladığınızdan tüm verileri alır (geometri, gezinti, malzemeler ve kaba/ince ayar) ve çeşitli adımlarda geçer:

1. Sahne kafesleri geometrisini alır ve voxel toplu hesaplar. Voxel birimin tamamını sahneniz barındırır ve küçük üçüncü dereceden "voxels" yapılan 3 boyutlu bir birimdir. Ayarlanan tarafından simülasyon sıklığı voxels boyutu belirlenir **benzetimi çözümleme** ayarı. Her voxel işaretlenmiş ya da olacak şekilde "havadan"açık"ya da Sahne geometri içeren. Bir voxel geometri içeriyorsa voxel bu geometriye atanmış malzemeleri içe alma katsayısı ile etiketlenir.
2. Player nereye acoustically ilginç konumları hesaplamak için Gezinti verileri kullanır. Daha küçük alanları gibi açılan kapılar ve koridorlarda ve alanları açmak için odalarını içerir makul küçük bir kümesi, bu konumlardan bulmak çalışır. Büyük sahneler kadar binlerce olabilir, ancak küçük sahneler için genellikle 100'den az konumları budur.
3. Konumların her biri, hesaplar son dinleyici için bu parametreleri nasıl "açık" gibi bir dizi boyutu vb. içinde yer alan olduğunu belirler.
4. Bu hesaplamaların sonuçlarını dosyaları belirttiğiniz konuma depolanır (bkz ["Veri dosyaları"](#Data-Files) aşağıda)

Sahneniz boyutunu ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

Bu hesaplamalar tamamlandıktan sonra hem voxel verileri hem de hazırlama iyi sonuç verecektir sağlamaya yardımcı olmak için araştırma noktası konumu önizleyebilirsiniz. Hatalı Gezinti kafes gibi şeyler veya Bunu düzeltmek için eksik ek geometri genellikle önizlemede kolayca görülebilir.

### <a name="scene-rename"></a>Sahne yeniden adlandırma

Görünüm adı, Sahne voxelization ve araştırma noktası yerleştirme depolama dosyalara bağlanmak için kullanılır. Araştırma noktaları hesaplandıktan sonra Sahne adlandırılırsa malzeme atama ve yerleştirme veriler kaybedilir ve yeniden çalıştırılmalıdır.

## <a name="debug-display-through-gizmos"></a>Görünen şeyler aracılığıyla hata ayıklama

Varsayılan olarak, hem **araştırmaları** ve **Voxels** şeyler etkinleştirilir. Bunlar, araştırma ve voxels hesaplanan konumları noktası gösterir. Bunlar, etkinleştirilebilir veya şeyler menüsünü kullanarak devre dışı:

![Şeyler menüsü](media/GizmosMenu.png)

### <a name="voxels"></a>Voxels

Voxels Sahne penceresinde yeşil küpler katılımcı geometri geçici olarak gösterilir. Yalnızca hava içeren Voxels gösterilmez. Benzetim kullanılacak tam voxel birimi gösterir tüm sahneniz çevresinde bir büyük yeşil kutu yoktur.
Sahneniz taşıyın ve geometri olması gereken her şeyi voxels sahip olduğunu doğrulayın. Kamerayı sahnenin yaklaşık 5 ölçümleri gösterilecek voxels için nesnenin içinde olması gerekir.

Kaba çözümleme vs iyi çözüm ile oluşturulan voxels karşılaştırırsanız kaba voxels iki kez büyük olduğunu görürsünüz.

![Voxel Önizleme](media/VoxelCubesPreview.png)

### <a name="probe-points"></a>Araştırma noktaları

Araştırma noktaları olası oynatıcı (dinleyici) konumları ile eşanlamlıdır. Akustik Sahne herhangi bir yerindeki bir ses kaynağı için bir hazırlama yaparken, her biri bu araştırma noktaları için hesaplanır. Player doğrudan bir araştırma noktası konumda değilse, veri araştırma noktalarından player en yakın olanında o konumda parametreleri enterpolasyon için kullanılır.

Bu nedenle araştırma noktaları herhangi bir yerde varolan sağlamanın önemli player sahnede seyahat beklenir ve küçük alanları ve açılan kapılar yeterince gösterilir gereklidir. Araştırma noktaları, sahnenin geometri yorumu üzerinde temel proje akustik altyapısı tarafından yerleştirilir ve taşınamaz veya bu Tasarımcı Önizleme sürümünde düzenlendi. Bunun yerine, geometri işaretleme ve gezinti ipucu verilerin doğruluğunu doğrulamak için bunları kullanın.

![Araştırmalar Önizleme](media/ProbesPreview.png)

### <a name="Coarse-vs-Fine-Resolution"></a>Kaba vs ayrıntılı çözümleme

Kaba ve ince çözümleme ayarları arasındaki tek fark, benzetim gerçekleştirildiği sıklığıdır. İnce bir sıklık kullanan iki kez olabildiğince yüksek kaba.
Bu basit görünebilir, ancak birçok etkilerinin akustik benzetim vardır:

* İki kez uzun olarak ince ayarlar ve bu nedenle voxels iki katı büyüklükte dalga boyu kaba için aynıdır.
* Benzetim zaman voxel boyutu yaklaşık 16 kat hızlı bir şekilde daha iyi bir hazırlama hazırlama bir kaba yapma, doğrudan ilgilidir.
* Portalları (örneğin, kapılar veya windows) voxel boyutundan daha küçük benzetimi yapılabilir. Kaba bir ayar değil benzetimi için daha küçük bu portallar bazılarını neden olabilir; Bu nedenle, bunlar çalışma zamanında ses üzerinden geçmez. Bu voxels görüntüleyerek olup olmadığını görebilirsiniz.
* Köşe ve kenarlar daha az diffraction alt benzetimi sıklığı sonuçlanır.
* Ses kaynakları geometri içeren voxels olan "dolu" voxels içinde bulunamıyor - bu ses yok sonuçlanır. İnce ayar kullandığından içinde kaba ' büyük voxels olmadıkları için ses kaynakları bulmak daha zordur.
* Daha büyük voxels daha aşağıda gösterildiği gibi portallarda oturum intrude. İlk görüntü kaba, iyi bir çözüm kullanarak aynı ortaklıklarına ortam hazırlayan ikinci olmakla birlikte kullanılarak oluşturuldu. Kırmızı işaretler tarafından belirtildiği gibi ince ayarını kullanarak ortaklıklarına ortam hazırlayan çok daha az giriş yok. Kırmızı çizgi voxel boyutu tarafından tanımlanan etkili akustik portalı olsa da, geometri tarafından tanımlanan aynıdır, mavi bir çizgi ortaklıklarına ortam hazırlayan kullanır. Bu yetkisiz erişim verilen bir durumda nasıl oynatılacağını tamamen geometri nesnelerinizi sahnedeki konumlarını ve boyutu tarafından belirlenir Portal ile nasıl voxels hizaya bağlıdır.

![Kaba kapısı](media/CoarseVoxelDoorway.png)

![İnce kapısı](media/FineVoxelDoorway.png)

## <a name="bake-tab"></a>Hazırlama sekmesi

Önizleme verileri memnun olduğunuzda kullanın **hazırlama** bulutta uygulamanızı hazırlama göndermek için sekmesinde.

### <a name="parts-of-the-bake-tab"></a>Hazırlama sekmesinin bölümleri

![Hazırlama sekmesi ayrıntısı](media/BakeTabDetails.png)

1. Bu sayfasını getirmek için kullanılan hazırlama için sekmesinde düğme.
2. Kısa bir açıklamasını bu sayfada yapmanız gerekenler.
3. Azure hesabınız oluşturulduktan sonra Azure kimlik bilgilerinizi girmek için alanlar. Daha fazla bilgi için [bir Azure Batch hesabı oluşturma](create-azure-account.md).
4. Akustik araç takımı için docker görüntü etiketi.
5. Aboneliklerinizi yönetin, kullanımını izlemek ve faturalandırma bilgileri vb. görüntülemek için Azure portalını başlatın. 
6. Azure batch işlem düğümü türü hesaplama için kullanılacak. Düğüm türü, Azure veri merkezi konumu tarafından desteklenmesi gerekir. Emin değilim, tutulacaksa **Standard_F8s_v2**.
7. Bu hesaplama için kullanmak için düğüm sayısı. Buraya girdiğiniz numara, Azure Batch çekirdek ayırma ile sınırlıdır ve hazırlama tamamlanma süresi etkiler. Varsayılan ayırma yalnızca iki 8 çekirdek düğümleri veya bir 16 çekirdek düğümü sağlar, ancak genişletilebilir. Çekirdek ayırma kısıtlamaları hakkında daha fazla bilgi için bkz. [bir Azure Batch hesabı oluşturma](create-azure-account.md).
8. Kullanılacak işlem havuzunuzu yapılandırmak için bu onay kutusunu seçin [düşük öncelikli düğümler](https://docs.microsoft.com/azure/batch/batch-low-pri-vms). Düşük öncelikli işlem düğümleri çok daha düşük bir maliyet vardır ancak bunlar her zaman kullanılabilir olmayabilir veya hiçbir zaman etkisiz hale getirilebilir.
9. Sahneniz hesaplanır olarak için yoklama sayısı **araştırmaları** sekmesi. Yoklama sayısını bulutta çalıştırılması gereken simülasyonlar sayısını belirler. Araştırmalar sayısından daha fazla düğüm belirtemezsiniz.
10. İşinizi bulutta çalıştırmak için için beklenen süre miktarı. Bu düğümü başlangıç süresini içermez. İş çalışmaya başladıktan sonra sonuçlar ulaşırsınız önce hakkında ne kadar süreyle olmalıdır budur. Bu yalnızca bir tahmin olduğunu unutmayın.
11. Simülasyonlar çalıştırılması için gereken bilgi işlem zamanı toplam miktarı. Toplam süre Azure'da kullanılacak düğümü işlem budur. Bkz: [Estimating hazırlama maliyet](#Estimating-bake-cost) aşağıda bu değeri kullanma hakkında daha fazla bilgi için.
12. Bu ileti, iş tamamlandığında hazırlama sonuçlarını kaydedileceği bildirir.
13. (Yalnızca Gelişmiş kullanım) Bir hazırlama hakkında unutmak çok Unity zorlamak için gereksinim duyduğunuz herhangi bir nedenle gönderilmesi durumunda (örneğin başka bir makineyi kullanmayı sonuçları yüklenir), tıklayın **Temizle durumu** gönderilen işiyle ilgili unutmak çok düğmesi. Bu sonuç dosyası, hazır olduğunuzda anlamına gelir, olur **değil** indirebilir, ve **bu işi iptal ediliyor dosyasındakiyle aynı olmaması**. İşi çalışıyorsa, bulutta çalışmaya devam eder.
14. Buluta hazırlama göndermek için hazırlama düğmesine tıklayın. Bir iş çalışırken bu gösterir **işi iptal et** yerine.
15. Yerel makine üzerinde akustik benzetim işlemek için hazırlar. Bkz: [yerel hazırlama](#Local-bake) daha fazla bilgi için.  
16. Bu alan hazırlama durumunu gösterir. Tamamlandığında, göstermelidir **yüklenen**.

Her zaman etkin işler, işlem havuzlarını ve depolama alanı ile ilgili eksiksiz bilgi alabileceğiniz [Azure portalı](https://portal.azure.com).

Bir iş çalışırken **hazırlama** düğmesi değişiklikleri **işi iptal et**. Devam eden işi iptal etmek için bu düğmeyi kullanın. İşi iptal edilmeden önce onaylayın istenir. Bir işi iptal ediliyor alınamaz, sonuç kullanıma sunulacak ve hala kullanılan herhangi bir Azure işlem süresi için ücretlendirilirsiniz.

Bir hazırlama başladıktan sonra Unity kapatabilirsiniz. Bir bulut hazırlama, proje, düğüm türü ve düğüm sayısına bağlı olarak birkaç saat sürebilir. Projeyi yeniden yükleyin ve akustik penceresini hazırlama iş durumu güncelleştirilir. İş tamamlandığında, çıkış dosyası indirilir.

Azure kimlik bilgileri yerel makinenize güvenli bir şekilde depolanır ve Unity editor'ınızdaki ile ilişkili. Bunlar, yalnızca Azure güvenli bir bağlantı kurmak için kullanılır.

### <a name="Estimating-bake-cost"></a> Azure hazırlama maliyet tahmini

Ne verilen hazırlama maliyetini tahmin etmek için gösterilen değer ele **işlem maliyeti tahmini**bir süresi olan ve birden çok kez tarafından saatlik maliyet yerel para **VM düğüm türü** seçtiniz. Düğümleri duruma getirmek için gereken ve çalışan düğümü zaman sonucu dahil edilmez. Örneğin, **Standard_F8s_v2** düğüm türünüz için olan 0.40 $/ SA maliyeti ve işlem tahmini maliyeti 3 saat 57 dakika, işi çalıştırmak için tahmini maliyeti $0.40 * ~ 4 saat olacak = ~ 1,60$. Gerçek maliyet büyük olasılıkla çalışmaya düğümleri almak için ek süreyi nedeniyle biraz daha yüksek olacaktır. Maliyeti saatlik düğümü bulabilirsiniz [Azure Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux) sayfası ("select işlem için iyileştirilmiş" veya "yüksek performanslı işlem" kategorisi için).

### <a name="reviewing-the-bake-results"></a>Hazırlama sonuçlarını gözden geçirme

Hazırlama işlemi tamamlandıktan sonra çalışma zamanı eklentisi çalıştırarak voxels ve araştırma noktaları beklenen konumlarına olduğunu kontrol edin. Daha fazla bilgi yer [tasarım işlemine genel bakış için akustik](design-process.md).

## <a name="Local-bake"></a>Yerel bir hazırlama
Yerel bir hazırlama akustik simülasyonu Azure Batch işlem kümesine boşaltma yerine kendi yerel bilgisayarda çalıştırır. Bu, bir Azure aboneliği ancak akustik benzetimi işlem bakımından yoğun ve simülasyon yapılandırma Sahne boyutuna bağlı olarak uzun zaman alabilir Not gerek kalmadan akustik ile denemek için iyi bir seçenek olabilir ve ham işleme makine bilgi işlem gücü.

### <a name="minimum-hardware-requirements"></a>En düşük donanım gereksinimleri
en az 8 çekirdek ve 32 GB RAM veya üzerinin 64 bit Intel işlemci.

Örneğin, 8 çekirdek makineye sahip Intel Xeon E5-1660 @ 3 GHz ve 32 GB bellek-
* 100 araştırmaları ile küçük Sahne yaklaşık 2 saat kaba bir hazırlama ve ince çözümleme hazırlama için yaklaşık 32 saat sürer.
* Daha büyük Sahne 1000 araştırmaları ile en fazla yaklaşık 20 saat kaba bir çözüm için ve bir ayrıntılı çözümleme hazırlama için ~ 21 gün sürebilir.

### <a name="setup-docker"></a>Docker Kurulumu
Yükleme ve Docker benzetim işleyen bilgisayarda yapılandırma-
1. Yükleme [Docker araç takımı](https://www.docker.com/products/docker-desktop).
2. Docker ayarları başlatın, "Gelişmiş" seçeneğine gidin ve kaynakları aşağıda gösterildiği gibi yapılandırın. ![Docker kaynakları](media/DockerSettings.png)
3. "Sürücüleri paylaşılan" seçeneğine gidin ve işleme için kullanılan sürücü için paylaşımı etkinleştirin.![DockerDriveSharing](media/DockerSharedDrives.png)

### <a name="run-local-bake"></a>Yerel çalışma hazırlama
1. Hazırlama sekmesinde "Hazırlama yerel Hazırlama" düğmesine tıklayın ve girdi dosyalarını ve yürütme komut dosyalarının kaydedileceği klasörü seçin. Ardından hazırlama en düşük donanım gereksinimlerini karşıladığından ve bu makineye klasörüne kopyalayarak Docker yüklü olduğu sürece herhangi bir makinede çalıştırabilirsiniz.
2. Proje akustik Docker görüntüsünü simülasyon işlemi için gerekli araç takımıyla getirmek ve benzetimi Başlat "runlocalbake.bat" komut dosyasını kullanarak simülasyon başlatın. 
3. Benzetim tamamlandıktan sonra elde edilen .ace dosyasını geri Unity projeniz bir Araştırmalarla sekmede belirtilen konuma kopyalayın. Hedef dosya adı için dosya uzantısı ".bytes" ekleyerek Unity'nın gereksinimlerine uyan emin olun. Ayrıntılı günlükler simülasyonu için "AcousticsLog.txt" dosyasında depolanır. Herhangi bir sorunla karşılaşırsanız çalıştırırsanız, tanı koymaya yardımcı olmak için bu dosyayı paylaşın.

## <a name="Data-Files"></a>Veri dosyaları

Bu eklenti çeşitli noktalarında tarafından oluşturulan dört veri dosyası vardır. Yalnızca bir tanesi gereklidir çalışma zamanında, bu nedenle projenize derlenmiş olmaz şekilde "Düzenleyicisi" adlı bir klasör içinde diğer üç altındadır.

* **Varlıklar/Düzenleyici / [SceneName]\_AcousticsParameters.asset**: Bu dosya akustik UI alanlarında girdiğiniz verileri depolar. Bu dosyanın adını ve konumunu değiştirilemez. Hazırlama etkileyen, bu dosyada depolanan diğer değerleri vardır, ancak bunlar Gelişmiş kullanıcılar için ve değiştirilmemesi gerekir.
* **Varlıklar/AcousticsData/akustik\_[SceneName].ace.bytes**: Bu dosyasıdır ne hazırlama benzetimi sırasında oluşturulur ve, sahnenin akustik işlemek için çalışma zamanı tarafından kullanılan arama verilerini içerir. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.
* **Assets/AcousticsData/Editor/Acoustics_[SceneName].vox**: voxelized akustik geometry ve malzeme özelliklerine bu dosyada depolanır. Kullanarak hesaplanan **hesapla...**  araştırmaları sekmesindeki düğmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.
* **Varlıklar/AcousticsData/Düzenleyicisi/akustik\_[SceneName]\_config.xml**: Bu dosya kullanılarak hesaplanır parametreleri depolayan **hesapla...**  düğmesini **araştırmaları** sekmesi. Bu dosyanın adını ve konumunu alanlara kullanarak değiştirilebilir **araştırmaları** sekmesi.

İlgileniriz silmemek *. ace.bytes dosya hazırlama indirilir. Bu dosya Sahne rebaking tarafından dışında kurtarılamaz.

## <a name="next-steps"></a>Sonraki adımlar
* Ses kaynaklarında hazırlama sonuçları uygulamak [tasarım işlemine genel bakış için akustik](design-process.md).

