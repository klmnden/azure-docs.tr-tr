---
title: Öğretici - Azure Search gibi dizin oluşturma bir ardışık düzeninde Bilişsel hizmetler API'lerini çağırma
description: Bu öğreticide, Azure Search veri ayıklama için dizin oluşturma ve dönüştürme işlemindeki veri ayıklama, doğal dil ve görüntü AI işleme örneği adımlarını izleyin.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f60b9002f939cbf4c3a0ecfb78b358598713ea1c
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881639"
---
# <a name="tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline-preview"></a>Öğretici: Bilişsel hizmetler API'leri çağırmak bir Azure Search dizini oluşturma ardışık düzen (Önizleme)

Bu öğreticide, *bilişsel becerileri* kullanarak Azure Search’te programlama veri zenginleştirmesi mekanizmasını öğrenirsiniz. Beceriler, doğal dil işlemeyi (NLP) ve Bilişsel hizmetler görüntü analizi özelliklerini tarafından desteklenir. Beceri kümesi oluşturma ve yapılandırma yoluyla, metin ve resim veya taranan dosyanın metin temsilleridir ayıklayabilirsiniz. Ayrıca, dil, varlıklar, anahtar ifadeleri ve daha da algılayabilir. Son ek zengin bir yapay ZEKA destekli dizini oluşturma ardışık düzeni tarafından oluşturulan bir Azure Search dizini içeriğinde sonucudur. 

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için REST API çağrıları yaparsınız:

> [!div class="checklist"]
> * Bir dizine giden yolda örnek verileri zenginleştiren dizin oluşturma işlem hattı oluşturma
> * Yerleşik becerileri uygulama: varlık tanıma, dil algılama, metin işleme, anahtar tümcecik ayıklama
> * Bir beceri kümesindeki çıktılara girişleri eşleyerek becerilerin nasıl birbirine zincirleneceğini öğrenme
> * İstekleri yürütme ve sonuçları gözden geçirme
> * Daha fazla geliştirme için dizini ve dizin oluşturucuları sıfırlama

Çıktı, Azure Search üzerinde tam metin araması yapılabilir bir dizindir. [Eş anlamlılar](search-synonyms.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [çözümleyiciler](search-analyzers.md) ve [filtreler](search-filters.md) gibi diğer standart özelliklerle dizini geliştirebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> 21 aralık 2018 tarihinden itibaren Bilişsel hizmetler kaynağı bir Azure Search beceri kümesi ile ilişkilendirmek mümkün olmayacak. Bu beceri yürütmesi için ücretlendirme başlatmak için bize izin verir. Bu tarihte, biz de belge çözme aşamasının bir parçası olarak görüntü ayıklama için başlayacağız. Belgelerden metin ayıklama işlemi ek masraf olmadan sağlanmaya devam edecektir.
>
> Var olan konumunda yerleşik yetenek yürütülmesini ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/) . Görüntü ayıklama fiyatlandırma Önizleme fiyatıyla ücretlendirilirsiniz ve üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400). [Daha fazla bilgi](cognitive-search-attach-cognitive-services.md) edinin.

## <a name="prerequisites"></a>Önkoşullar

Bilişsel aramada yeni misiniz? Öğrenmek için ["Bilişsel arama nedir?"](cognitive-search-concept-intro.md) bölümünü okuyun veya önemli kavramlara pratik bir giriş için [portal hızlı başlangıcını](cognitive-search-quickstart-blob.md) deneyin.

Azure Search’e REST çağrıları yapmak için PowerShell veya Telerik Fiddler ya da Postman gibi bir web testi aracı kullanarak HTTP isteklerini formüle edin. Bu araçlar sizin için yeniyse, [Fiddler veya Postman kullanarak Azure Search REST API’lerini keşfetme](search-fiddler.md) bölümüne bakın.

[Azure portalını](https://portal.azure.com/) kullanarak uçtan uca bir iş akışında kullanılan hizmetler oluşturun. 

### <a name="set-up-azure-search"></a>Azure Search ayarlama

İlk olarak, Azure Search hizmetine kaydolun. 

1. [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızı kullanarak oturum açın.

1. **Kaynak oluştur**’a tıklayın, Azure Search’ü arayın ve **Oluştur**’a tıklayın. İlk kez bir arama hizmeti ayarlıyorsanız [Portalda Azure Search hizmeti oluşturma](search-create-service-portal.md) bölümüne bakın.

   ![Pano portalı](./media/cognitive-search-tutorial-blob/create-search-service-full-portal.png "Portalda Azure Search hizmeti oluşturma")

1. Kaynak grubu için, bu öğreticide oluşturduğunuz tüm kaynakları içerecek bir kaynak grubu oluşturun. Böylece, öğreticiyi tamamladıktan sonra kaynakları temizlemeniz kolaylaşır.

1. Konum için verilerinizi ve diğer bulut uygulamaları yakın bir bölge seçin.

1. Fiyatlandırma katmanı için, **Ücretsiz** bir hizmet oluşturarak öğreticileri ve hızlı başlangıçları tamamlayabilirsiniz. Kendi verilerinizi kullanarak daha ayrıntılı araştırma yapmak için **Temel** veya **Standart** gibi bir [ücretli hizmet](https://azure.microsoft.com/pricing/details/search/) oluşturun. 

   Ücretsiz hizmet; 3 dizin, 16 MB maksimum blob boyutu ve 2 dizinleme dakikası ile sınırlıdır ve bu da bilişsel aramanın tüm yeteneklerini uygulamak için yeterli değildir. Farklı katmanlara ilişkin sınırları gözden geçirmek için bkz. [Hizmet Sınırları](search-limits-quotas-capacity.md).

   ![Portalda Hizmet tanım sayfası](./media/cognitive-search-tutorial-blob/create-search-service1.png "portalında hizmet tanımı sayfası")
   ![portalında hizmet tanımı sayfası](./media/cognitive-search-tutorial-blob/create-search-service2.png "hizmet tanımı sayfasında portal")

 
1. Hizmet bilgilerine hızlı erişim için hizmeti panoya sabitleyin.

   ![Portaldaki hizmet tanımı sayfası](./media/cognitive-search-tutorial-blob/create-search-service3.png "Portaldaki hizmet tanımı sayfası")

1. Hizmet oluşturulduktan sonra aşağıdaki bilgileri toplayın: **URL** genel bakış sayfasından ve **api anahtarını** (birincil veya ikincil) anahtarlar sayfasındaki.

   ![Portaldaki uç nokta ve anahtar bilgileri](./media/cognitive-search-tutorial-blob/create-search-collect-info.png "Portaldaki uç nokta ve anahtar bilgileri")

### <a name="set-up-azure-blob-service-and-load-sample-data"></a>Azure Blob hizmetini ayarlama ve örnek veriler yükleme

Zenginleştirme işlem hattı, Azure veri kaynaklarından çekme işlemi yapar. Kaynak veriler, [Azure Search dizin oluşturucunun](search-indexer-overview.md) desteklenen bir veri kaynağı türünden gelmelidir. Azure tablo depolaması için bilişsel arama desteklenmediğini unutmayın. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolama kullanırız.

1. [Örnek veri indirin](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4). Örnek veriler, farklı türlerde küçük bir dosya kümesinden oluşur. 

1. Azure Blob depolamaya kaydolun, depolama hesabı oluşturun, Depolama Gezgini’nde oturum açın ve `basicdemo` adlı bir kapsayıcı oluşturun. Tüm adımlardaki yönergeler için bkz. [Azure Depolama Gezgini Hızlı Başlangıcı](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

1. Azure Depolama Gezgini’ni kullanarak, oluşturduğunuz `basicdemo` kapsayıcısında **Karşıya Yükle**’ye tıklayarak örnek dosyaları karşıya yükleyin.

1. Örnek dosyalar yüklendikten sonra Blob depolamanız için bir bağlantı dizesi ve kapsayıcı adını alın. Azure portalda depolama hesabınıza giderek bunu yapabilirsiniz. **Erişim anahtarları** bölümünde **Bağlantı Dizesi** alanını kopyalayın.

   Bağlantı dizesi, aşağıdaki örneğe benzer bir URL olmalıdır:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Paylaşılan erişim imzası sağlama gibi, bağlantı dizesini belirtmenin başka birçok yolu vardır. Veri kaynağı kimlik bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Blob Depolama Alanı dizinini oluşturma](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="create-a-data-source"></a>Veri kaynağı oluşturma

Hizmetleriniz ve kaynak dosyalarınız hazırlandığına göre şimdi dizin oluşturma işlem hattınızın bileşenlerini derlemeye başlayabilirsiniz. Azure Search’e, dış kaynak verilerinin nasıl alınacağını bildiren bir [veri kaynağı nesnesi](https://docs.microsoft.com/rest/api/searchservice/create-data-source) ile başlayın.

Bu öğretici için, PowerShell, Postman veya Fiddler gibi HTTP isteklerini formüle edip gönderebilen bir araç ve REST API kullanın. İstek üst bilgisinde, Azure Search hizmetini oluştururken kullandığınız hizmet adını ve arama hizmetiniz için oluşturulan api-anahtarını sağlayın. İstek gövdesinde, blob kapsayıcı adını ve bağlantı dizesini belirtin.

### <a name="sample-request"></a>Örnek İstek
```http
POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
Content-Type: application/json
api-key: [admin key]
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "name" : "demodata",
  "description" : "Demo files to demonstrate cognitive search capabilities.",
  "type" : "azureblob",
  "credentials" :
  { "connectionString" :
    "DefaultEndpointsProtocol=https;AccountName=<your account name>;AccountKey=<your account key>;"
  },
  "container" : { "name" : "<your blob container name>" }
}
```
İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Bu ilk isteğiniz olduğundan, veri kaynağının Azure Search’te oluşturulduğunu onaylamak için Azure portalını denetleyin. Arama hizmeti panosu sayfasında, Veri Kaynakları kutucuğunun yeni bir öğe içerdiğini doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir. 

  ![Portaldaki veri kaynakları kutucuğu](./media/cognitive-search-tutorial-blob/data-source-tile.png "Portaldaki veri kaynakları kutucuğu")

Bir 403 veya 404 hatası aldıysanız, istek yapısını denetleyin: `api-version=2017-11-11-Preview`, uç nokta üzerinde olmalıdır, `api-key`, Üst bilgide `Content-Type` öğesinden sonra gelmelidir ve değeri bir arama hizmeti için geçerli olmalıdır. Bu öğreticide kalan adımlar için üst bilgiyi kullanabilirsiniz.

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bu adımda, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesini tanımlarsınız. Her zenginleştirme adımını *beceri* olarak ve zenginleştirme adımları kümesini de *beceri kümesi* olarak adlandırabilirsiniz. Bu öğreticide, beceri kümesi için [önceden tanımlanmış bilişsel beceriler](cognitive-search-predefined-skills.md) kullanılmaktadır:

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar tümcecik ayıklama becerisini çağırmadan önce büyük içeriği daha küçük öbeklere ayırmak için [Metni Böl](cognitive-search-skill-textsplit.md). Anahtar tümcecik ayıklama, 50.000 veya daha az karakterden oluşan girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Blob kapsayıcıdaki içerikten kuruluşların adlarını ayıklamak için [Adlandırılmış Varlık Tanıma](cognitive-search-skill-named-entity-recognition.md).

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md). 

### <a name="sample-request"></a>Örnek İstek
Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Bu istek bir beceri kümesi oluşturur. Bu öğreticinin geri kalanında beceri kümesi adı ```demoskillset``` olarak ifade edilir.

```http
PUT https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "description":
  "Extract entities, detect language and extract key-phrases",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations", "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
      "inputs": [
        {
          "name": "text", "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "languageCode",
          "targetName": "languageCode"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "textSplitMode" : "pages",
      "maximumPageLength": 4000,
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        },
        {
          "name": "languageCode",
          "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "textItems",
          "targetName": "pages"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
      "context": "/document/pages/*",
      "inputs": [
        {
          "name": "text", "source": "/document/pages/*"
        },
        {
          "name":"languageCode", "source": "/document/languageCode"
        }
      ],
      "outputs": [
        {
          "name": "keyPhrases",
          "targetName": "keyPhrases"
        }
      ]
    }
  ]
}
```

İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

#### <a name="about-the-request"></a>İstek hakkında

Her bir sayfa için anahtar tümcecik ayıklama becerisinin nasıl uygulandığına dikkat edin. Bağlamı ```"document/pages/*"``` olarak ayarlayarak, belge/sayfa dizisinin her üyesi (belgedeki her sayfa) için bu zenginleştiriciyi çalıştırırsınız.

Her beceri, belge içeriğinde yürütülür. İşleme sırasında Azure Search, farklı dosya biçimlerinden içerik okumak için her bir belgeyi çözer. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, girişi ```"/document/content"``` olarak ayarlayın.

Beceri kümesinin grafiksel gösterimi aşağıda gösterilmektedir. 

![Beceri kümesini anlama](media/cognitive-search-tutorial-blob/skillset.png "Beceri kümesini anlama")

Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

## <a name="create-an-index"></a>Dizin oluşturma

Bu bölümde, aranabilir dizine dahil edilecek alanları ve her bir alana ilişkin arama özniteliklerini belirterek dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="sample-request"></a>Örnek İstek
Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Bu istek bir dizin oluşturur. Bu öğreticinin geri kalanında ```demoindex``` dizin adını kullanın.

```http
PUT https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi

```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Dizin tanımlama hakkında daha fazla bilgi için bkz. [Dizin Oluşturma (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).


## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Dizin oluşturucu oluşturma, alanları eşleme ve dönüştürmeler yürütme

Şu ana kadar bir veri kaynağı, beceri kümesi ve dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bunları bir dizin oluşturucu birbirine bağlamak için alan eşlemeleri tanımlamanız gerekir. Alan eşlemeleri, dizin oluşturucu tanımının bir parçası olup isteği gönderdiğinizde dönüştürmeler yürütür.

Zenginleştirilmemiş dizin oluşturma için, alan adları veya veri türleri tam olarak eşleşmiyorsa ya da bir işlev kullanmak istiyorsanız dizin oluşturucu tanımı, isteğe bağlı bir *fieldMappings* bölümü sağlar.

Zenginleştirme işlem hattı içeren bilişsel arama iş yükleri için dizin oluşturucu, *outputFieldMappings* gerektirir. Bir iç işlem (zenginleştirme işlem hattı), alan değerlerinin kaynağı olduğunda bu eşlemeler kullanılır. *outputFieldMappings* için benzersiz olan davranışlar, zenginleştirmenin parçası olarak oluşturulan karmaşık türleri işleme yeteneğini içerir (şekillendirici becerisi aracılığıyla). Ayrıca belge başına birçok öğe olabilir (örneğin, bir belgedeki birden çok kuruluş). *outputFieldMappings* yapısı, sistemi, öğe koleksiyonunu tek bir kayıtta "düzleştirmesi" için yönlendirir.

### <a name="sample-request"></a>Örnek İstek

Bu REST çağrısını yapmadan önce, aracınız çağrılar arasında istek üst bilgisini korumuyorsa aşağıdaki istekte yer alan hizmet adını ve yönetici anahtarını değiştirmeyi unutmayın. 

Ayrıca, dizin oluşturucunuzun adını sağlayın. Bu öğreticinin geri kalanında bunu ```demoindexer``` olarak ifade edebilirsiniz.

```http
PUT https://[servicename].search.windows.net/indexers/demoindexer?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```
#### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi

```json
{
  "name":"demoindexer", 
  "dataSourceName" : "demodata",
  "targetIndexName" : "demoindex",
  "skillsetName" : "demoskillset",
  "fieldMappings" : [
    {
      "sourceFieldName" : "metadata_storage_path",
      "targetFieldName" : "id",
      "mappingFunction" :
        { "name" : "base64Encode" }
    },
    {
      "sourceFieldName" : "content",
      "targetFieldName" : "content"
    }
  ],
  "outputFieldMappings" :
  [
    {
      "sourceFieldName" : "/document/organizations",
      "targetFieldName" : "organizations"
    },
    {
      "sourceFieldName" : "/document/pages/*/keyPhrases/*",
      "targetFieldName" : "keyPhrases"
    },
    {
      "sourceFieldName": "/document/languageCode",
      "targetFieldName": "languageCode"
    }
  ],
  "parameters":
  {
    "maxFailedItems":-1,
    "maxFailedItemsPerBatch":-1,
    "configuration":
    {
      "dataToExtract": "contentAndMetadata",
      "imageAction": "generateNormalizedImages"
    }
  }
}
```

İsteği gönderin. Web testi aracı, 201 durum kodunu döndürerek işlemin başarılı olduğunu onaylamalıdır. 

Bu adımın tamamlanması birkaç dakika sürebilir. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. Görüntü analizi gibi bazı beceriler uzun sürer.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir. İşlem hattını, kod veya betik değişiklikleriyle yeniden çalıştırmak için önce nesneleri bırakmanız gerekebilir. Daha fazla bilgi için bkz. [Sıfırlama ve yeniden çalıştırma](#reset).

### <a name="explore-the-request-body"></a>İstek gövdesini keşfetme

Betik, ```"maxFailedItems"``` değerini -1 olarak ayarlayarak dizin oluşturma motoruna, veri içeri aktarma sırasında hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca yapılandırma parametrelerinde ```"dataToExtract":"contentAndMetadata"``` deyimine de dikkat edin. Bu deyim, dizin oluşturucuya, farklı dosya biçimlerinden içeriği ve her bir dosyayla ilgili meta verileri otomatik olarak ayıklamasını bildirir. 

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için ```imageAction``` değerini ayarlayabilirsiniz. ```"imageAction":"generateNormalizedImages"``` Yapılandırma, OCR beceri ve metin yetenek, birleştirme ile birlikte kullanıldığında metin (örneğin, "Durdur" trafiğinden durdurma oturum word) görüntülerden ayıklayın ve içerik alanı bir parçası olarak eklemek için dizin oluşturucuyu söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

## <a name="check-indexer-status"></a>Dizin oluşturucu durumunu denetleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin oluşturucunun halen çalışıp çalışmadığını öğrenmek için aşağıdaki isteği göndererek dizin oluşturucu durumunu denetleyin.

```http
GET https://[servicename].search.windows.net/indexers/demoindexer/status?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Yanıt, dizin oluşturucunun çalışıp çalışmadığını size bildirir. Dizin oluşturma işlemi tamamlandıktan sonra, STATUS uç noktasına yönelik başka bir HTTP GET kullanarak, zenginleştirme sırasında oluşan hata ve uyarıların raporlarını görün.  

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır (örneğin, JPEG dosyalarında bir metin girişi yok). Dizin oluşturma sırasında oluşan uyarılar hakkında ayrıntılı bilgi için durum yanıtını gözden geçirebilirsiniz.
 
## <a name="verify-content"></a>İçeriği doğrulama

Dizin oluşturma işlemi tamamlandıktan sonra tek tek alanların içeriklerini döndüren sorgular çalıştırın. Azure Search varsayılan olarak ilk 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Yönergeler için bkz. [Azure Search’te sonuçların sayfasını oluşturma](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

```http
GET https://[servicename].search.windows.net/indexes/demoindex?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Çıktı, her bir alanın adını, türünü ve özniteliklerini içeren dizin şemasıdır.

`organizations` gibi tek bir alanın tüm içeriklerini döndürmek için ikinci bir `"*"` sorgusu gönderin.

```http
GET https://[servicename].search.windows.net/indexes/demoindex/docs?search=*&$select=organizations&api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Bu çalışmadaki diğer içerik, dil, anahtar tümceler ve kuruluşlar alanları için yineleyin. Virgülle ayrılmış bir liste kullanarak `$select` aracılığıyla birden fazla alan döndürebilirsiniz.

Sorgu dizesinin karmaşıklık ve uzunluğuna bağlı olarak GET veya POST dizesini kullanabilirsiniz. Daha fazla bilgi için bkz. [REST API kullanarak sorgu](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="access-enriched-document"></a>

## <a name="accessing-the-enriched-document"></a>Zenginleştirilmiş belgeye erişme

Bilişsel arama, zenginleştirilmiş belgenin yapısını görmenizi sağlar. Zenginleştirilmiş belgeler, zenginleştirme sırasında oluşturulan geçici yapılardır ve işlem tamamlandığında silinir.

Dizin oluşturma sırasında oluşturulan zenginleştirilmiş belgenin anlık görüntüsünü yakalamak için dizininize ```enriched``` adlı bir alan ekleyin. Dizin oluşturucu, otomatik olarak alana, o belgenin tüm zenginleştirmelerinin dize gösteriminin dökümünü alır.

```enriched``` alanı, JSON’da bellek içi zenginleştirilmiş belgenin mantıksal gösterimi olan bir dize içerir.  Ancak alan değeri geçerli bir JSON belgesidir. Tırnak işaretlerine kaçış karakteri eklenir, böylece belgeyi biçimlendirilmiş JSON olarak görüntülemek için `\"` öğesini `"` ile değiştirmeniz gerekir.  

İfadelerin değerlendirildiği içeriğin mantıksal şeklini anlamanıza yardımcı olması için ```enriched``` alanı yalnızca hata ayıklama için tasarlanmıştır. Beceri kümenizi anlamak ve hatasını ayıklamak için bu faydalı bir araç olabilir.

Zenginleştirilmiş belgenin içeriklerini yakalamak için bir `enriched` alanını ekleyerek önceki çalışmayı yineleyin:

### <a name="request-body-syntax"></a>İstek Gövdesi Sözdizimi
```json
{
  "fields": [
    {
      "name": "id",
      "type": "Edm.String",
      "key": true,
      "searchable": true,
      "filterable": false,
      "facetable": false,
      "sortable": true
    },
    {
      "name": "content",
      "type": "Edm.String",
      "sortable": false,
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "languageCode",
      "type": "Edm.String",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "keyPhrases",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "organizations",
      "type": "Collection(Edm.String)",
      "searchable": true,
      "sortable": false,
      "filterable": false,
      "facetable": false
    },
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```
<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

İşlem hattı geliştirmenin ilk deneme aşamalarında, tasarım yinelemeleri için en pratik yaklaşım, Azure Search’ten nesneleri silmek ve kodunuzun bunları yeniden oluşturmasını sağlamaktır. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Yeni tanımlarla belgelerinizin yeniden dizinini oluşturmak için:

1. Dizini silerek kalıcı verileri kaldırın. Dizin oluşturucuyu silerek hizmetinizde yeniden oluşturun.
2. Bir beceri kümesi ve dizin tanımını değiştirin.
3. İşlem hattını çalıştırmak için hizmet üzerinde yeniden bir dizin ve dizin oluşturucu oluşturun. 

Dizin, dizin oluşturucular ve becerilerini silmek için portalı kullanabilirsiniz.

```http
DELETE https://[servicename].search.windows.net/skillsets/demoskillset?api-version=2017-11-11-Preview
api-key: [api-key]
Content-Type: application/json
```

Silme işlemi başarılı olduğunda durum kodu 204 döndürülür.

Kodunuz geliştikçe bir yeniden derleme stratejisini iyileştirmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

## <a name="takeaways"></a>Paketler

Bu öğreticide, veri kaynağı, beceri kümesi, dizin ve dizin oluşturucu gibi bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmektedir.

Girişler ve çıktılar yoluyla becerileri zincirleme mekanizmalarının ve beceri kümesi tanımının yanı sıra [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) sunulmuştur. İşlem hattındaki zenginleştirilmiş değerleri bir Azure Search hizmetinde aranabilir bir dizine yönlendirmek için dizin oluşturucu tanımında `outputFieldMappings` gerektiğini öğrendiniz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Bu yayında, iç yapıları (sistem tarafından oluşturulan zenginleştirilmiş belgeler) görüntüleme mekanizması vardır. Ayrıca dizin oluşturucu durumunun nasıl denetleneceğini ve işlem hattı yeniden çalıştırılmadan önce hangi nesnelerin silineceğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmektir. Her iki hizmeti de aynı gruba koyduğunuz varsayılarak, şimdi bu öğreticide oluşturduğunuz depolanan içerikler ve hizmetler de dahil olmak üzere, kaynak grubunun içindeki her şeyi silmek için kaynak grubunu silin. Portalda kaynak grubu adı, her bir hizmetin Genel Bakış sayfasındadır.

## <a name="next-steps"></a>Sonraki adımlar

Özel becerilerle işlem hattını özelleştirin veya genişletin. Özel bir beceri oluşturup bir beceri kümesine eklemeniz, kendi yazdığınız metin veya görüntü analizini eklemenize olanak sağlar. 

> [!div class="nextstepaction"]
> [Örnek: özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
