---
title: Yapay ZEKA destekli Azure portalı - Azure Search dizini oluşturmak için bilişsel arama işlem hattı oluşturma
description: Örnek veriler kullanılarak Azure portalında veri ayıklama, doğal dil ve görüntü işleme becerileri örneği.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 50b2973f2b245cfb42ed7212e443fec1c66217cf
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015281"
---
# <a name="quickstart-create-a-cognitive-search-pipeline-using-skills-and-sample-data"></a>Hızlı Başlangıç: Bilişsel arama yetenekleri ve örnek verileri kullanarak işlem hattı oluşturma

Bilişsel arama (önizleme), Azure Search dizin oluşturma işlem hattına veri ayıklama, doğal dil işleme (NLP) ve görüntü işleme becerileri ekleyerek aranamayan veya yapılandırılmamış içeriği daha aranabilir hale getirir. 

Bilişsel arama işlem hattı tümleşir [Bilişsel hizmetler kaynakları](https://azure.microsoft.com/services/cognitive-services/) - gibi [OCR](cognitive-search-skill-ocr.md), [dil algılama](cognitive-search-skill-language-detection.md), [varlık tanıma](cognitive-search-skill-entity-recognition.md)- bir dizin oluşturma işlemine. Bilişsel hizmetler, yapay ZEKA algoritması, temel Azure Search'te tam metin araması çözümlerinde, yapılar ve kullanılabilir metinsel içeriği döndüren kaynak verilerdeki desenleri, özellikleri ve özelliklerini bulmak için kullanılır.

Bu hızlı başlangıçta, ilk zenginleştirme hattınızı oluşturma [Azure portalında](https://portal.azure.com) tek satırlık bir kod yazmadan önce:

> [!div class="checklist"]
> * Azure Blob depolamada örnek verileri kullanmaya başlama
> * Yapılandırma [ **verileri içeri aktarma** ](search-import-data-portal.md) bilişsel dizin oluşturma ve zenginleştirme Sihirbazı 
> * Sihirbazı çalıştırma (bir varlık becerisi, kişileri, konumu ve kuruluşları algılar)
> * Kullanım [ **arama Gezgini** ](search-explorer.md) zenginleştirilmiş verileri sorgulamak için

## <a name="supported-regions"></a> Desteklenen bölgeler

Aşağıdaki bölgelerde oluşturulan bir Azure Search hizmetinde bilişsel aramayı deneyebilirsiniz:

* Batı Orta ABD
* Orta Güney ABD
* Doğu ABD
* Doğu ABD 2
* Batı ABD 2
* Orta Kanada
* Batı Avrupa
* Birleşik Krallık Güney
* Kuzey Avrupa
* Güney Brezilya
* Güneydoğu Asya
* Orta Hindistan
* Avustralya Doğu

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.
>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400). Bilgi [daha fazla](cognitive-search-attach-cognitive-services.md).

## <a name="prerequisites"></a>Önkoşullar

[“Bilişsel arama nedir?”](cognitive-search-concept-intro.md) zenginleştirme mimarisi ve bileşenleri sunar. 

Bu senaryoda özel olarak Azure hizmetleri kullanılır. İhtiyaç duyduğunuz hizmetleri oluşturma, hazırlığın bir parçasını oluşturur.

+ [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) kaynak verileri sağlar
+ [Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/) (oluşturabileceğiniz bu kaynakları satır içi, işlem hattı belirtirken) yapay ZEKA sağlar
+ [Azure Search'ü](https://azure.microsoft.com/services/search/) özel apps'te kullanılmak üzere zenginleştirilmiş dizinleme işlem hattına ve zengin serbest biçimli metin arama deneyimi sağlar.

### <a name="set-up-azure-search"></a>Azure Search ayarlama

İlk olarak, Azure Search hizmetine kaydolun. 

1. [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızı kullanarak oturum açın.

1. **Kaynak oluştur**’a tıklayın, Azure Search’ü arayın ve **Oluştur**’a tıklayın. İlk kez bir arama hizmeti ayarlıyorsanız ve daha fazla yardıma ihtiyacınız varsa bkz. [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md).

  ![Pano portalı](./media/cognitive-search-tutorial-blob/create-search-service-full-portal.png "Portalda Azure Search hizmeti oluşturma")

1. Kaynak grubu için bu hızlı başlangıçta oluşturduğunuz tüm kaynakları içeren yeni bir kaynak grubu oluşturun. Böylece, hızlı başlangıcı tamamladıktan sonra kaynakları temizlemeniz kolaylaşır.

1. Konum için aşağıdakilerden birini seçin [desteklenen bölgeler](#supported-regions) bilişsel arama için.

1. Fiyatlandırma katmanı için, **Ücretsiz** bir hizmet oluşturarak öğreticileri ve hızlı başlangıçları tamamlayabilirsiniz. Kendi verilerinizi kullanarak daha ayrıntılı araştırma yapmak için **Temel** veya **Standart** gibi bir [ücretli hizmet](https://azure.microsoft.com/pricing/details/search/) oluşturun. 

  Ücretsiz hizmet; 3 dizin, 16 MB maksimum blob boyutu ve 2 dizinleme dakikası ile sınırlıdır ve bu da bilişsel aramanın tüm yeteneklerini uygulamak için yeterli değildir. Farklı katmanlara ilişkin sınırları gözden geçirmek için bkz. [Hizmet Sınırları](search-limits-quotas-capacity.md).

  ![Portaldaki hizmet tanımı sayfası](./media/cognitive-search-tutorial-blob/create-search-service2.png "Portaldaki hizmet tanımı sayfası")

  > [!NOTE]
  > Bilişsel arama genel önizleme aşamasındadır. Beceri kümesi yürütme şu anda ücretsiz katman da dahil olmak üzere tüm katmanlarda kullanılabilir. Ücretli bir Bilişsel hizmetler kaynağı ilişkilendirmeden zenginleştirmelerinin sınırlı sayıda gerçekleştirmek mümkün olacaktır. Bilgi [daha fazla](cognitive-search-attach-cognitive-services.md).

1. Hizmet bilgilerine hızlı erişim için hizmeti panoya sabitleyin.

  ![Portaldaki hizmet tanımı sayfası](./media/cognitive-search-tutorial-blob/create-search-service3.png "Portaldaki hizmet tanımı sayfası")

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Azure Blob hizmetini ayarlama ve örnek veriler yükleme

Zenginleştirme işlem hattı, [Azure Search dizin oluşturucuları](search-indexer-overview.md) tarafından desteklenen Azure veri kaynaklarından çekme işlemi yapar. Azure tablo depolaması için bilişsel arama desteklenmediğini unutmayın. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolama kullanırız.

1. Farklı türlerden oluşan küçük bir dosya kümesini içeren [örnek verileri indirin](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). 

1. Azure Blob Depolama için kaydolun, depolama hesabı oluşturma, Blob Hizmetleri sayfalarını açın ve bir kapsayıcı oluşturun. Genel erişim düzeyini ayarlayın, kapsayıcıdaki **kapsayıcı**. Daha fazla bilgi için ["bir kapsayıcı oluşturma" bölümünde](../storage/blobs/storage-unstructured-search.md#create-a-container) içinde *yapılandırılmamış verileri arama* öğretici.

1. Oluşturduğunuz kapsayıcıya tıklayın **karşıya** önceki bir adımda indirdiğiniz örnek dosyalarını karşıya yüklemek için.

  ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

## <a name="create-the-enrichment-pipeline"></a>Zenginleştirme işlem hattı oluşturma

Azure Search Hizmeti Pano sayfasına dönün ve **verileri içeri aktarma** dört adımda bilişsel zenginleştirme ayarlamak için komut çubuğunda.

  ![Verileri içeri aktar komutu](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

İçinde **verilerinize bağlanın**, seçin **Azure Blob Depolama**, kapsayıcı oluşturduğunuz ve hesabı seçin. Veri kaynağına bir ad verin ve geri kalanı için varsayılan değerleri kullanın. 

  ![Azure blob yapılandırması](./media/cognitive-search-quickstart-blob/blob-datasource.png)

Bir sonraki sayfasına devam edin.

  ![Bilişsel arama için sonraki sayfa düğmesi](media/cognitive-search-quickstart-blob/next-button-add-cog-search.png)

### <a name="step-2-add-cognitive-skills"></a>2. Adım: Bilişsel yetenekler Ekle

Daha sonra dizin oluşturma işlem hattına zenginleştirme adımları ekleyin. Bilişsel hizmetler kaynağı yoksa 20 işlem günlük sağlayan ücretsiz bir sürümü için kaydolabilirsiniz. Bu sihirbazı çalıştırdıktan sonra günlük ayrılan çoğunlukla kullanılacak böylece örnek verileri 14 dosyasından oluşur.

1. Genişletin **ekleme Bilişsel Hizmetler** kaynaklama Bilişsel hizmetler API'leri için seçenekleri görmek için. Bu öğreticinin amaçları doğrultusunda, kullandığınız **ücretsiz** kaynak.

  ![Bilişsel Hizmetleri Ekleme](media/cognitive-search-quickstart-blob/cog-search-attach.png)

2. Genişletin **ekleme Zenginleştirmelerinin** ve doğal dil işleme gerçekleştirme yetenekleri seçin. Bu hızlı başlangıç için, kişiler, kuruluşlar ve konumlar için varlık tanımayı seçin.

  ![Bilişsel Hizmetleri Ekleme](media/cognitive-search-quickstart-blob/skillset.png)

  Portal, OCR işleme ve metin analizi için yerleşik yetenekler sunar. Portalda beceri kümesi, tek bir kaynak alanının üzerinde çalışır. Bu küçük bir hedef gibi görünebilir, ancak Azure blobları için `content` alanı, blob belgesinin çoğunu içerir (örneğin, Word belgesi veya PowerPoint destesi). Aynı şekilde bir blobun tüm içeriği de bu alanda bulunduğundan bu alan ideal bir giriştir.

3. Bir sonraki sayfasına devam edin.

  ![Sonraki sayfa dizini özelleştirme](media/cognitive-search-quickstart-blob/next-button-customize-index.png)

> [!NOTE]
> Doğal dil işleme becerileri, örnek veri kümesindeki metin içeriği üzerinde çalışır. Biz OCR seçeneğini seçmediyseniz olduğundan, bu hızlı başlangıçta örnek veri kümesinde bulunan JPEG ve PNG dosyaları işlenmez. 

### <a name="step-3-configure-the-index"></a>3. Adım: Dizini yapılandırma

Sihirbaz, genellikle varsayılan bir dizin çıkarabilir. Bu adımda oluşturulan dizin şemasını görüntülemek ve potansiyel olarak tüm ayarları gözden geçirin. Aşağıda varsayılan dizini için tanıtım Blob veri kümesi oluşturulur.

Bu hızlı başlangıç, makul varsayılanlar ayarlanması konusunda iyi bir iş çıkarır: 

+ Varsayılan ad *azureblob dizin*.
+ Varsayılan anahtar *metadata_storage_path* (Bu alan, benzersiz değerler içeren).
+ Varsayılan veri türleri ve öznitelikleri, tam metin arama senaryoları için geçerlidir.

Temizleme göz önünde bulundurun **alınabilir** gelen `content` alan. Bloblar, bu alan binlerce satır çalıştırabilirsiniz. Bu içerik ağır dosyaları Word belgeleri veya PowerPoint Desteleri gibi JSON olarak bir arama sonuçları listesinde görüntülemek üzere olacaktır ne kadar zor hayal edebilirsiniz. 

Bir beceri kümesi tanımlanmadığından, sihirbaz, özgün kaynak veri alanı yanı sıra bilişsel işlem hattı tarafından oluşturulan çıkış alanları istediğinizi varsayar. Bu nedenle portal, `content`, `people`, `organizations` ve `locations` için dizin alanları ekler. Sihirbaz otomatik olarak etkinleştirdiğini unutmayın **alınabilir** ve **aranabilir** bu alanlar için. **Aranabilir** bir alan aranabilir gösterir. **Alınabilir** bunu döndürülmesi sonuçlarında anlamına gelir. 

  ![Dizin alanları](media/cognitive-search-quickstart-blob/index-fields.png)
  
Bir sonraki sayfasına devam edin.

  ![Sonraki sayfaya dizin oluşturucu oluşturma](media/cognitive-search-quickstart-blob/next-button-create-indexer.png)

### <a name="step-4-configure-the-indexer"></a>4. adım: Dizin oluşturucuyu yapılandırma

Dizin oluşturucu, dizin oluşturma işlemini destekleyen, yüksek düzeyli bir kaynaktır. Bu veri kaynağı adı, hedef dizin ve yürütme sıklığı belirtir. **Verileri içeri aktar** sihirbazının nihai sonucunda her zaman art arda çalıştırabileceğiniz bir dizin oluşturucu elde edilir.

İçinde **dizin oluşturucu** sayfasında, varsayılan adı kabul edin ve kullanın **bir kez çalıştır** zamanlama hemen çalıştırmak için seçenek. 

  ![Dizin oluşturucu tanımı](media/cognitive-search-quickstart-blob/indexer-def.png)

Tıklayın **Gönder** oluşturmak ve aynı anda dizin oluşturucuyu çalıştırmak için.

## <a name="monitor-indexing"></a>Dizin oluşturma İzleyicisi

Zenginleştirme adımları metin tabanlı tipik dizin daha tamamlanması daha uzun sürer. İlerleme durumunu izleyebilmeniz için sihirbazın genel bakış sayfasında dizin oluşturucu listesini açın. Kendi kendine gezintiye genel bakış sayfasında ve tıklayın Git **dizin oluşturucular**.

Uyarı JPG ve PNG dosyaları görüntü dosyaları ve biz bu işlem hattından OCR beceri atlanmış oluşur. Kesme bildirimleri da bulabilirsiniz. Azure arama, ücretsiz katmanda 32.000 karakter ayıklama sınırlar.

  ![Azure Search bildirimi](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Dizin oluşturma ve zenginleştirme zaman alabilir; ilk keşif için daha küçük veri kümelerinin önerilmesinin nedeni budur. 

## <a name="query-in-search-explorer"></a>Arama gezgininde sorgulama

Bir dizin oluşturulduktan sonra, dizinden belgeleri döndürmek için sorgular gönderebilirsiniz. Portalda **Arama gezgini**’ni kullanarak sorgular çalıştırın ve sonuçları görüntüleyin. 

1. Arama hizmeti panosu sayfasında, komut çubuğunda **Arama gezgini**’ne tıklayın.

1. Oluşturduğunuz dizini seçmek için üst kısımdaki **Dizini değiştir**'e tıklayın.

1. Gibi dizini sorgulamak için bir arama dizesi girin `search=Microsoft&searchFields=organizations`.

Sonuçlar JSON olarak döndürülür, bu nedenle özellikle de Azure bloblardan gelen büyük belgelerde ayrıntılı ve okuması zor olabilir. Sonuçları kolayca tarayamazsanız, belgeler içinde arama yapmak için CTRL-F tuşlarını kullanın. Bu sorgu için özel terimleri JSON içinde arama yapabilirsiniz. 

CTRL-F tuş birleşimi, belirli bir sonuç kümesinde kaç tane belgenin bulunduğunu belirlemenize de yardımcı olabilir. Her değer belge için benzersiz olduğundan Azure blobları için portal, anahtar olarak "metadata_storage_path" öğesini seçer. CTRL-F kullanarak, "metadata_storage_path" araması yapıp belgelerin sayısını alın. 

  ![Arama gezgini örneği](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Paketler

Artık, ilk bilişsel zenginleştirilmiş dizinleme alıştırma tamamladınız. Bu hızlı başlangıcın amacı, kendi verilerinizi kullanarak hızlı şekilde bir bilişsel arama çözümünün prototipini oluşturabilmeniz için sihirbazda size yol göstermek ve önemli kavramları tanıtmaktır.

Topladığınız bazı temel kavramlar, Azure veri kaynaklarına bağımlılık içerir. Bilişsel arama zenginleştirmesi, dizin oluşturuculara bağlıdır ve dizin oluşturucular, Azure’a ve kaynağa özgüdür. Bu hızlı başlangıçta Azure Blob depolama kullanılsa da, başka Azure veri kaynakları da mümkündür. Daha fazla bilgi için bkz. [Azure Search’te dizin oluşturucular](search-indexer-overview.md).

Başka bir önemli kavram da, becerilerin giriş alanları üzerinde çalışmasıdır. Portalda, tüm yetenekler için tek kaynak alan seçmeniz gerekir. Kodda girişler, başka alanlar olabilir veya bir yukarı akış becerisinin çıktısı olabilir.

 Bir beceriye yönelik girişler, bir dizindeki çıktı alanına eşlenir. Dahili olarak portal, [ek açıklamalar](cognitive-search-concept-annotations-syntax.md) ayarlar ve işlemlerin sırasını ve genel akışı oluşturan bir [beceri kümesini](cognitive-search-defining-skillset.md) tanımlar. Bu adımlar portalda gizlenir, ancak kod yazmaya başladığınızda bu kavramlar önemli hale gelir.

Son olarak, görüntüleme sonuçlarının dizinin sorgulanmasıyla elde edildiğini öğrendiniz. Sonunda Azure Search, [basit](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) veya [tamamen genişletilmiş sorgu sözdizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) kullanarak sorgulayabileceğiniz, aranabilir bir dizin sağlar. Zenginleştirilmiş alanlar içeren bir dizin de diğerlerine benzer. Standart veya [özel çözümleyiciler](search-analyzers.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [eş anlamlılar](search-synonyms.md), [ayrıntılı filtreler](search-filters-facets.md), coğrafi arama veya başka bir Azure Search özelliğine yer vermek istiyorsanız bunu yapabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Keşfiniz sonlandığında temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmektir.  

Her iki hizmeti de aynı gruba koyduğunuz varsayılarak, şimdi bu çalışmada oluşturduğunuz depolanan içerikler ve hizmetler de dahil olmak üzere, kaynak grubunun içindeki her şeyi silmek için kaynak grubunu silin. Portalda kaynak grubu adı, her bir hizmetin Genel Bakış sayfasındadır.

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler kaynağı nasıl sağladığınız bağlı olarak, dizin oluşturma ve zenginleştirme ile farklı becerileri ve kaynak veri alanları ile sihirbazını yeniden çalıştırarak denemeler yapabilirsiniz. Adımları yinelemek için, dizini ve dizin oluşturucuyu silin, ardından dizin oluşturucuyu yeni seçimlerle yeniden oluşturun.

+ **Genel Bakış** > **Dizinler** bölümünde, oluşturduğunuz dizini seçin ve sonra **Sil**’e tıklayın.

+ **Genel Bakış**’ta **Dizin Oluşturucular** kutucuğuna çift tıklayın. Oluşturduğunuz dizin oluşturucuyu bulup silin.

Alternatif olarak, oluşturduğunuz örnek veri ve hizmetleri yeniden kullanın ve sonraki öğreticide aynı görevleri programlama yoluyla nasıl gerçekleştireceğinizi öğrenin. 

> [!div class="nextstepaction"]
> [Öğretici: Bilişsel arama REST API'leri öğrenin](cognitive-search-tutorial-blob.md)
