---
title: 'Python eğitmen: Bilişsel hizmetler - Azure Search gibi dizin oluşturma bir ardışık düzeninde çağırın'
description: Veri ayıklama, doğal dil ve görüntü AI örneği bir Jupyter Python Not Defteri kullanarak Azure Search'te işlem adımı. Ayıklanan veriler dizini ve sorgu ile kolayca erişilebilir.
manager: cgronlun
author: LisaLeib
services: search
ms.service: search
ms.devlang: python
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: v-lilei
ms.openlocfilehash: b1166e0acdbc9371b1c7ca2361fc6ebb7479b6a7
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672078"
---
# <a name="python-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>Python eğitmen: Bir Azure Search dizini oluşturma ardışık düzen Bilişsel hizmetler API'lerini çağırma

Bu öğreticide, *bilişsel becerileri* kullanarak Azure Search’te programlama veri zenginleştirmesi mekanizmasını öğrenirsiniz. Beceriler, doğal dil işlemeyi (NLP) ve Bilişsel hizmetler görüntü analizi özelliklerini tarafından desteklenir. Beceri kümesi oluşturma ve yapılandırma yoluyla, metin ve resim veya taranan dosyanın metin temsilleridir ayıklayabilirsiniz. Ayrıca, dil, varlıklar, anahtar ifadeleri ve daha da algılayabilir. Zengin ek içerik dizinleme bir işlem hattındaki AI zenginleştirmelerinin ile oluşturulmuş bir Azure Search dizini içinde sonucudur. 

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için Python'ı kullanacaksınız:

> [!div class="checklist"]
> * Bir dizine giden yolda örnek verileri zenginleştiren dizin oluşturma işlem hattı oluşturma
> * Yerleşik becerileri uygulama: varlık tanıma, dil algılama, metin işleme, anahtar tümcecik ayıklama
> * Bir beceri kümesindeki çıktılara girişleri eşleyerek becerilerin nasıl birbirine zincirleneceğini öğrenme
> * İstekleri yürütme ve sonuçları gözden geçirme
> * Daha fazla geliştirme için dizini ve dizin oluşturucuları sıfırlama

Çıktı, Azure Search üzerinde tam metin araması yapılabilir bir dizindir. [Eş anlamlılar](search-synonyms.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [çözümleyiciler](search-analyzers.md) ve [filtreler](search-filters.md) gibi diğer standart özelliklerle dizini geliştirebilirsiniz. 

Bu öğreticide ücretsiz hizmet üzerinde çalışır, ancak ücretsiz işlem sayısı günde 20 belgeleri sınırlıdır. Bu öğreticide, aynı gün içinde birden çok kez çalıştırmak istiyorsanız, daha fazla çalıştırma sığacak şekilde ayarlamak daha küçük bir dosya kullanın.

> [!NOTE]
> Kapsam işleme sıklığını artırarak daha fazla belgelerin eklenmesi genişletmeniz veya daha fazla yapay ZEKA algoritmalarının eklenmesi gerekir [Faturalanabilir bir Bilişsel hizmetler kaynağı ekleme](cognitive-search-attach-cognitive-services.md). API'leri, Bilişsel hizmetler ve Azure Search'te belge çözme aşamasının bir parçası olarak görüntü ayıklama çağırırken ücretler tahakkuk. Metin ayıklama belgelerden için ücretlendirme yoktur.
>
> Yerleşik yetenek yürütülmesi sırasında mevcut ücretlendirilir [Bilişsel hizmetler ödeme-olarak-, Git fiyat](https://azure.microsoft.com/pricing/details/cognitive-services/). Görüntü ayıklama fiyatlandırma üzerinde açıklanmıştır [Azure fiyatlandırma sayfasını arama](https://go.microsoft.com/fwlink/?linkid=2042400).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki hizmetleri, araçları ve verileri kullanılır. 

+ [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) örnek verileri depolamak için. Azure Search ile aynı bölgede depolama hesabı olduğundan emin olun.

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), sağlama Python 3.x ve Jupyter not defterleri.

+ [Örnek verileri](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) farklı türlerde küçük bir dosya kümesinden oluşur. 

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="get-a-key-and-url"></a>Bir anahtarı ve URL alma

Azure Search hizmet ile etkileşimde bulunmak üzere hizmet URL'si ve erişim anahtarı gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinizdeki **genel bakış** sayfa olduğunda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. İçinde **ayarları** > **anahtarları**, hizmette tam haklarına yönelik bir yönetici anahtarını alın. Bir gece yarısında gerektiği durumlarda iş sürekliliği için sağlanan iki birbirinin yerine yönetici anahtarı mevcuttur. Ekleme, değiştirme ve silme nesneler için istekleri birincil veya ikincil anahtar kullanabilirsiniz.

![Bir HTTP uç noktası ve erişim anahtarını alma](media/search-get-started-postman/get-url-key.png "bir HTTP uç noktası ve erişim anahtarını alma")

Tüm istekleri hizmete gönderilen her istekte bir API anahtarı gerektirir. Geçerli bir anahtar, istek başına temelinde, uygulama gönderme isteği ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Zenginleştirme işlem hattı, Azure veri kaynaklarından çekme işlemi yapar. Kaynak veriler, [Azure Search dizin oluşturucunun](search-indexer-overview.md) desteklenen bir veri kaynağı türünden gelmelidir. Azure tablo depolama için bilişsel arama desteklenmiyor. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolama kullanırız.

1. [Azure portalında oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, tıklayın **Blobları**ve ardından **+ kapsayıcı**.

1. [Bir Blob kapsayıcısı oluşturursunuz](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) örnek verileri içerecek şekilde. Genel erişim düzeyi geçerli değerleri birini ayarlayabilirsiniz.

1. Kapsayıcıyı oluşturduktan sonra dosyayı açın ve seçin **karşıya** önceki bir adımda indirdiğiniz örnek dosyalarını karşıya yüklemek için komut çubuğunda.

   ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Örnek dosyalar yüklendikten sonra Blob depolamanız için bir bağlantı dizesi ve kapsayıcı adını alın. Azure portalda depolama hesabınıza giderek bunu yapabilirsiniz. Tıklayın **erişim anahtarları**ve ardından kopyalama **bağlantı dizesi** alan.

Bağlantı dizesi bu biçimde olacaktır: `DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT-NAME>;AccountKey=<YOUR-STORAGE-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

Bağlantı dizesi elinizin altında tutun. Gelecekteki bir adımda gerekir.

Paylaşılan erişim imzası sağlama gibi, bağlantı dizesini belirtmenin başka birçok yolu vardır. Veri kaynağı kimlik bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Blob Depolama Alanı dizinini oluşturma](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

> [!Note]
> Bu makalede, bir veri kaynağı, dizin, dizin oluşturucu ve bir dizi Python betiklerini kullanarak beceri kümesi oluşturma işlemini göstermektedir. Eksiksiz bir not defteri örneği indirmek için Git [Azure-Search-python-samples deposuna](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment-Jupyter-Notebook).

Anaconda Gezgin Jupyter not defteri başlatın ve yeni bir Python 3 not defteri oluşturmak için kullanın.

## <a name="connect-to-azure-search"></a>Azure Search'e Bağlan

Dizüstü bilgisayarınız için JSON ile çalışma ve HTTP isteklerini formulating kullanılan kitaplıklar yüklemek için bu betiği çalıştırın.

```python
import json
import requests
from pprint import pprint
```

Ardından, veri kaynağı, dizin, dizin oluşturucu ve beceri kümesi adlarını tanımlar. Bu öğretici için adlarını ayarlamak için bu betiği çalıştırın.

```python
#Define the names for the data source, skillset, index and indexer
datasource_name="cogsrch-py-datasource"
skillset_name="cogsrch-py-skillset"
index_name="cogsrch-py-index"
indexer_name="cogsrch-py-indexer"
```

> [!Tip]
> Ücretsiz bir hizmet üç dizin, dizin oluşturucular ve veri kaynakları için sınırlı olursunuz. Bu öğreticide hepsinden birer tane oluşturulur. Daha fazla işlenmesini önce yeni nesneler oluşturmak için yer olduğundan emin olun.

Aşağıdaki komut, arama hizmetinizin (YOUR-SEARCH-hizmet-adı) yer tutucularını değiştirin ve yönetici API'si (YOUR-ADMIN-API-KEY) anahtar ve ardından uygulamayı Arama Hizmeti uç nokta ayarlamayı.

```python
#Setup the endpoint
endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
headers = {'Content-Type': 'application/json',
        'api-key': '<YOUR-ADMIN-API-KEY>' }
params = {
    'api-version': '2019-05-06'
}
```

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

Hizmetleriniz ve kaynak dosyalarınız hazırlandığına göre şimdi dizin oluşturma işlem hattınızın bileşenlerini derlemeye başlayabilirsiniz. Dış kaynak verileri almak nasıl Azure Search bildiren bir veri kaynağı nesnesi ile başlar.

Aşağıdaki betikte yer tutucu YOUR-BLOB-RESOURCE-CONNECTION-STRING, önceki adımda oluşturduğunuz blob için bağlantı dizesiyle değiştirin. Ardından, adlı bir veri kaynağı oluşturmak için betiği çalıştırın `cogsrch-py-datasource`.

```python
#Create a data source
datasourceConnectionString = "<YOUR-BLOB-RESOURCE-CONNECTION-STRING>"
datasource_payload = {
    "name": datasource_name,
    "description": "Demo files to demonstrate cognitive search capabilities.",
    "type": "azureblob",
    "credentials": {
    "connectionString": datasourceConnectionString
   },
    "container": {
     "name": "basic-demo-data-pr"
   }
}
r = requests.put( endpoint + "/datasources/" + datasource_name, data=json.dumps(datasource_payload), headers=headers, params=params )
print (r.status_code)
```

İstek başarılı onaylayan 201 durum kodu döndürmelidir.

Azure portalında arama hizmeti Pano sayfasında cogsrch-py-datasource göründüğünü doğrulayın **veri kaynakları** listesi. Tıklayın **Yenile** sayfası.

![Portaldaki veri kaynakları kutucuğu](./media/cognitive-search-tutorial-blob-python/py-data-source-tile.png "Portaldaki veri kaynakları kutucuğu")

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bu adımda, verilere uygulanacak zenginleştirme adımları tanımlayacaksınız. Her zenginleştirme adımını *beceri* olarak ve zenginleştirme adımları kümesini de *beceri kümesi* olarak adlandırabilirsiniz. Bu öğreticide [yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) becerilerine için:

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar tümcecik ayıklama becerisini çağırmadan önce büyük içeriği daha küçük öbeklere ayırmak için [Metni Böl](cognitive-search-skill-textsplit.md). Anahtar tümcecik ayıklama, 50.000 veya daha az karakterden oluşan girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ [Varlık tanıma](cognitive-search-skill-entity-recognition.md) blob kapsayıcısındaki içeriğinden kuruluşların adlarını ayıklanacağı.

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md). 

### <a name="python-script"></a>Python betiği
Adlı bir beceri kümesi oluşturmak için aşağıdaki betiği çalıştırın `cogsrch-py-skillset`.

```python
#Create a skillset
skillset_payload = {
  "name": skillset_name,
  "description":
  "Extract entities, detect language and extract key-phrases",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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

r = requests.put(endpoint + "/skillsets/" + skillset_name, data=json.dumps(skillset_payload), headers=headers, params=params)
print(r.status_code)
```

İstek başarılı onaylayan 201 durum kodu döndürmelidir.

Anahtar ifade ayıklama beceri her sayfa için uygulanır. Bağlam ayarlayarak `"document/pages/*"`, bu enricher (için belgenin her sayfasında) belge/sayfaları dizinin her üyesi için çalıştırın.

Her beceri, belge içeriğinde yürütülür. İşleme sırasında Azure Search, farklı dosya biçimlerinden içerik okumak için her bir belgeyi çözer. Kaynak dosyada bulunan metin içine yerleştirilir bir `content` alan, her belge için bir tane. Bu nedenle, giriş olarak kümesi `"/document/content"`.

Beceri kümesinin grafiksel gösterimi aşağıda gösterilmektedir.

![Beceri kümesini anlama](media/cognitive-search-tutorial-blob/skillset.png "Beceri kümesini anlama")

Dil kodu ile olduğu gibi bir aşağı akış becerisi ya da her ikisini girdi olarak kullanılan bir dizine çıkışları eşlenebilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

## <a name="create-an-index"></a>Dizin oluşturma

Bu bölümde, arama yapılabilir bir dizin eklemek için kullanılan alanları belirtme ve her bir alan için arama özniteliklerini ayarlayarak dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | id         | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |

Adlı bir dizin oluşturmak için bu betiği çalıştırın `cogsrch-py-index`.

```python
#Create an index
index_payload = {
    "name": index_name,
    "fields": [
      {
        "name": "id",
        "type": "Edm.String",
        "key": "true",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false",
        "sortable": "true"
      },
      {
        "name": "content",
        "type": "Edm.String",
        "sortable": "false",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "languageCode",
        "type": "Edm.String",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "keyPhrases",
        "type": "Collection(Edm.String)",
        "searchable": "true",
        "filterable": "false",
        "facetable": "false"
      },
      {
        "name": "organizations",
        "type": "Collection(Edm.String)",
        "searchable": "true",
        "sortable": "false",
        "filterable": "false",
        "facetable": "false"
      }
   ]
}

r = requests.put(endpoint + "/indexes/" + index_name, data=json.dumps(index_payload), headers=headers, params=params)
print(r.status_code)
```

İstek başarılı onaylayan 201 durum kodu döndürmelidir.

Dizin tanımlama hakkında daha fazla bilgi için bkz. [Dizin Oluşturma (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Dizin oluşturucu oluşturma, alanları eşleme ve dönüştürmeler yürütme

Şu ana kadar bir veri kaynağı, bir beceri kümesi ve bir dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bu nesneler birbirine bir dizin oluşturucu için alan eşlemelerini tanımlamanız gerekir.

+ FieldMappings veri kaynağından kaynak alanları dizindeki hedef alan için eşleme becerilerine önce işlenir. Alan adları ve türleri aynı anda iki ucu varsa, hiçbir eşleme gereklidir.

+ OutputFieldMappings belge çözme kadar mevcut olmayan sourceFieldNames başvuran becerilerine sonra işlenir veya bunları zenginleştirme oluşturur. TargetFieldName dizinde bir alandır.

Çıkış girdileri olaylara yanı sıra alan eşlemelerini veri yapılarını düzleştirmek için de kullanabilirsiniz. Daha fazla bilgi için [zenginleştirilmiş alanları arama yapılabilir bir dizin eşlemeyle ilgili bilgi](cognitive-search-output-field-mapping.md).

Adlı bir dizin oluşturucu oluşturmak için bu betiği çalıştırın `cogsrch-py-indexer`.

```python
# Create an indexer
indexer_payload = {
    "name": indexer_name,
    "dataSourceName": datasource_name,
    "targetIndexName": index_name,
    "skillsetName": skillset_name,
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

r = requests.put(endpoint + "/indexers/" + indexer_name, data=json.dumps(indexer_payload), headers=headers, params=params)
print(r.status_code)
```

İstek hızla 201 durum kodu döndürmelidir, ancak işlem tamamlanması birkaç dakika sürebilir. Veri kümesini küçük olsa da, görüntü analizi gibi analitik yetenekler yoğun işlem gücü gerektiren ve zaman alabilir.

Kullanım [dizin oluşturucu durumunu denetleme](#check-indexer-status) dizin oluşturucu işlemin tamamlandığını belirlemek için sonraki bölümde betiği.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Girişler ve çıkışlar, eşleme verileri erişilirken bir sorun varsa veya işlemlerin sırası ile bu aşamada görünür. Kod veya betik değişiklikler ile işlem hattını yeniden çalıştırmak için nesneleri önce silmeniz gerekebilir. Daha fazla bilgi için bkz. [Sıfırlama ve yeniden çalıştırma](#reset).

#### <a name="explore-the-request-body"></a>İstek gövdesini keşfetme

Betik, `"maxFailedItems"` değerini -1 olarak ayarlayarak dizin oluşturma motoruna, veri içeri aktarma sırasında hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca yapılandırma parametrelerinde `"dataToExtract":"contentAndMetadata"` deyimine de dikkat edin. Bu ifade içeriği farklı dosya biçimlerini ve her dosyayla ilgili meta verilerini ayıklamak için dizin oluşturucu bildirir.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için `imageAction` değerini ayarlayabilirsiniz. `"imageAction":"generateNormalizedImages"` Yapılandırma, OCR beceri ve metin yetenek, birleştirme ile birlikte kullanıldığında metin (örneğin, "Durdur" trafiğinden durdurma oturum word) görüntülerden ayıklayın ve içerik alanı bir parçası olarak eklemek için dizin oluşturucuyu söyler. Bu davranış (PDF içinde görüntü düşünün) belgelerinde katıştırılmış görüntüler ve JPG dosyası örneği için bir veri kaynağında bulunan görüntüleri için geçerlidir.

<a name="check-indexer-status"></a>

## <a name="check-indexer-status"></a>Dizin oluşturucu durumunu denetleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin Oluşturucu işleme tam olup olmadığını öğrenmek için aşağıdaki betiği çalıştırın.

```python
#Get indexer status
r = requests.get(endpoint + "/indexers/" + indexer_name + "/status", headers=headers,params=params)
pprint(json.dumps(r.json(), indent=1))
```

Yanıtta "lastResult", "Durum" ve "endTime" değerlerini izleyin. Düzenli aralıklarla durumu denetlemek için betiği çalıştırın. Dizin Oluşturucu tamamlandığında, durum "başarılı" durumuna ayarlanır, "endTime" belirtilen ve yanıt hataları ve zenginleştirme gerçekleşen uyarıları içerir.

![Dizin Oluşturucu oluşturulur](./media/cognitive-search-tutorial-blob-python/py-indexer-is-created.png "dizin oluşturucu oluşturuldu")

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır. Örneğin, bir metin yok. JPEG dosyaları uyarı bu ekran görüntüsünde gösterilir.

![Örnek dizin oluşturucu Uyarısı](./media/cognitive-search-tutorial-blob-python/py-indexer-warning-example.png "örnek dizin oluşturucu Uyarısı")

## <a name="query-your-index"></a>Dizininizi sorgulama

Dizin oluşturma işlemi tamamlandıktan sonra tek tek alanların içeriklerini döndüren sorgular çalıştırın. Azure Search varsayılan olarak ilk 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Yönergeler için bkz. [Azure Search’te sonuçların sayfasını oluşturma](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

```python
#Query the index for all fields
r = requests.get(endpoint + "/indexes/" + index_name, headers=headers,params=params)
pprint(json.dumps(r.json(), indent=1))
```

Sonuçları şu örneğe benzemelidir. Ekran yalnızca yanıtın bir parçası gösterir.

![Tüm alanlar için sorgu dizini](./media/cognitive-search-tutorial-blob-python/py-query-index-for-fields.png "tüm alanlar için dizini sorgulama")

Çıktı, her bir alanın adını, türünü ve özniteliklerini içeren dizin şemasıdır.

`organizations` gibi tek bir alanın tüm içeriklerini döndürmek için ikinci bir `"*"` sorgusu gönderin.

```python
#Query the index to return the contents of organizations
r = requests.get(endpoint + "/indexes/" + index_name + "/docs?&search=*&$select=organizations", headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Sonuçları şu örneğe benzemelidir. Ekran yalnızca yanıtın bir parçası gösterir.

![Sorgu dizini kuruluşların içeriklerinin](./media/cognitive-search-tutorial-blob-python/py-query-index-for-organizations.png "kuruluşlar içeriğini döndürmek için dizini sorgulama")

Ek alanlar için işlemi tekrarlayın: içeriği, languageCode, keyPhrases ve bu alıştırmada kuruluşlar. Virgülle ayrılmış bir liste kullanarak `$select` aracılığıyla birden fazla alan döndürebilirsiniz.

Sorgu dizesinin karmaşıklık ve uzunluğuna bağlı olarak GET veya POST dizesini kullanabilirsiniz. Daha fazla bilgi için bkz. [REST API kullanarak sorgu](https://docs.microsoft.com/rest/api/searchservice/search-documents).
Bunu <a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

İşlem hattı geliştirmenin ilk deneme aşamalarında, tasarım yinelemeleri için en pratik yaklaşım, Azure Search’ten nesneleri silmek ve kodunuzun bunları yeniden oluşturmasını sağlamaktır. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Yeni tanımlarla belgelerinizin yeniden dizinini oluşturmak için:

1. Dizini silerek kalıcı verileri kaldırın. Dizin oluşturucuyu silerek hizmetinizde yeniden oluşturun.
2. Yetenek ve dizin tanımlarını değiştirin.
3. İşlem hattını çalıştırmak için hizmet üzerinde yeniden bir dizin ve dizin oluşturucu oluşturun.

Dizin, dizin oluşturucular ve becerilerini silmek için portalı kullanabilirsiniz. Dizin Oluşturucu sildiğinizde, dizin, beceri kümesi ve veri kaynağı isteğe bağlı olarak, seçmeli olarak aynı anda silebilirsiniz.

![Arama nesneleri silmek](./media/cognitive-search-tutorial-blob-python/py-delete-indexer-delete-all.png "nesneleri portalda aramayı Sil")

Bir betik kullanarak silebilirsiniz. Aşağıdaki komut dosyasında oluşturduğumuz becerilerine silinmesine neden olur. Dizin, dizin oluşturucu ve veri kaynağını silme isteği, bir kolayca değiştirebilirsiniz.

```python
#delete the skillset
r = requests.delete(endpoint + "/skillsets/" + skillset_name, headers=headers, params=params)
pprint(json.dumps(r.json(), indent=1))
```

Kodunuz geliştikçe bir yeniden derleme stratejisini iyileştirmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

## <a name="takeaways"></a>Paketler

Bu öğreticide, veri kaynağı, beceri kümesi, dizin ve dizin oluşturucu gibi bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmektedir.

[Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) beceri kümesi tanımları ve becerileri zinciri girdileri ve çıktıları ile birlikte bir yol ile birlikte sunulan. İşlem hattındaki zenginleştirilmiş değerleri bir Azure Search hizmetinde aranabilir bir dizine yönlendirmek için dizin oluşturucu tanımında `outputFieldMappings` gerektiğini öğrendiniz.

Son olarak, test sonuçları ve ek yinelemeler için sistemi hakkında bilgi edindiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Bu yayında, iç yapıları (sistem tarafından oluşturulan zenginleştirilmiş belgeler) görüntüleme mekanizması vardır. Ayrıca dizin oluşturucu durumu denetlemek üzere nasıl öğrendiniz ve hangi nesneleri bir işlem hattı yeniden çalıştırmadan önce silinmesi gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmektir. Her iki hizmet de aynı grupta yerleştirdiğiniz varsayarsak, her şeyi, hizmetleri ve Bu öğretici için oluşturduğunuz depolanan tüm içeriğin dahil kalıcı olarak silmek için kaynak grubunu silin. Portalda kaynak grubu adı, her bir hizmetin Genel Bakış sayfasındadır.

## <a name="next-steps"></a>Sonraki adımlar

Özel becerilerle işlem hattını özelleştirin veya genişletin. Özel bir beceri oluşturup bir beceri kümesine eklemeniz, kendi yazdığınız metin veya görüntü analizini eklemenize olanak sağlar.

> [!div class="nextstepaction"]
> [Örnek: Bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
