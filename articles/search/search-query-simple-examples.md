---
title: Azure Search için basit bir sorgu örnekleri | Microsoft Docs
description: Tam metin araması, filtre arama, coğrafi arama, çok yönlü arama ve bir Azure Search dizinini sorgulama için kullanılan diğer sorgu dizeleri için basit bir sorgu örnekleri.
author: HeidiSteen
manager: cgronlun
tags: Simple query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: heidist
ms.openlocfilehash: e4bb72eb8ad6f15b0e5bc14e0e07556e76d0477b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369124"
---
# <a name="simple-syntax-query-examples-for-building-queries-in-azure-search"></a>Azure Search'te sorgular oluşturmaya yönelik basit sözdizimi sorgu örnekleri

[Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) Azure Search dizini tam metin arama sorguları yürütmek varsayılan sorgu ayrıştırıcı çağırır. Basit Sorgu Çözümleyicisi, hızlı ve Azure Search, tam metin araması, filtrelenmiş ve çok yönlü arama ve coğrafi arama gibi yaygın senaryoları ele alır. Bu makalede, basit söz dizimi kullanırken kullanılabilir sorgu işlemleri gösteren örnekler adım.

Diğer sorgu söz dizimi [tam Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), daha karmaşık sorgu yapıları gibi destekleyici belirsiz ve joker karakter araması, işlemek için ek zaman alabilir. Daha fazla bilgi ve tam söz dizimi gösteren örnekler için bkz. [Lucene sözdizimi sorgu örnekleri](search-query-lucene-examples.md).

## <a name="formulate-requests-in-postman"></a>Postman'deki istekleri düzenleme

Aşağıdaki örnekler tarafından sağlanan bir veri kümesini temel alan işleri kullanılabilir oluşan NYC işleri arama dizini yararlanarak [New York City OpenData](https://nycopendata.socrata.com/) girişim. Bu veriler, geçerli ya da tam düşünülmemelidir. Diğer bir deyişle, bir Azure aboneliği veya Azure Search'ün bu sorguları deneyin gerekmez Microsoft tarafından sağlanan bir korumalı alan hizmeti dizinidir.

Ne gerekiyor, Postman veya HTTP GET isteği verme eşdeğer bir aracı değil. Daha fazla bilgi için [REST istemcileri ile Test](search-fiddler.md).

### <a name="set-the-request-header"></a>İstek üst bilgisini ayarlayın

1. İstek üstbilgisinde ayarlanan **Content-Type** için `application/json`.

2. Ekleme bir **api anahtarını**ve bu dize olarak ayarlayın: `252044BE3886FE4A8E3BAA4F595114BB`. NYC işleri dizini barındırma korumalı alan arama hizmeti için bir sorgu anahtarı budur.

İstek üstbilgisi belirttikten sonra onu tüm sorgular bu makalede, yalnızca takas tekrar kullanabilirsiniz **arama =** dize. 

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>İstek URL'sini ayarlayın

Azure arama uç noktası ve arama dizesini içeren URL'yi ile eşleştirilmiş bir GET komutu isteğidir.

  ![Postman isteği üst bilgisi](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

URL'si birleşimi aşağıdaki öğeleri içerir:

+ **`https://azs-playground.search.windows.net/`** bir korumalı alan arama hizmeti, Azure Search geliştirme ekibi tarafından korunur. 
+ **`indexes/nycjobs/`** Bu hizmetin dizinlerini koleksiyonunda NYC işleri dizinidir. İstekte hizmet adını ve dizin gereklidir.
+ **`docs`** documents koleksiyonunu içeren tüm aranabilir içeriği. İstek üst bilgisinde sağlanan sorgunuzun api anahtarını, yalnızca documents koleksiyonunu hedefleyen okuma işlemleri üzerinde çalışır.
+ **`api-version=2017-11-11`** her istekte gerekli bir parametre olan api-version, ayarlar.
+ **`search=*`** ilk sorgu null sorgu dizesi, ilk 50 sonuçları döndüren (varsayılan).

## <a name="send-your-first-query"></a>İlk sorgunuzu Gönder

Bir doğrulama adımı aşağıdaki isteği GET yapıştırın ve tıklayın **Gönder**. Sonuçları ayrıntılı JSON belgeleri olarak döndürülür. Kopyalama bu URL'yi aşağıdaki ilk örnekte yapıştırma.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

Sorgu dizesi **`search=*`**, belirtilmeyen bir arama null veya boş aramaya eşdeğerdir. Özellikle kullanışlı değildir, ancak yapabileceğiniz basit arama olur.

İsteğe bağlı olarak ekleyebileceğiniz **`$count=true`** arama ölçütleriyle eşleşen belgelerin sayısını döndürmek için URL. Üzerinde bir boş bir arama dizesi (2802 NYC işleri söz konusu olduğunda) dizindeki tüm belgelerin budur.

## <a name="how-to-invoke-simple-query-parsing"></a>Basit Sorgu ayrıştırma çağırmak nasıl

Etkileşimli sorgular için herhangi bir şey belirtmeniz gerekmez: basit bir varsayılandır. Kodda, daha önce çağırdıysanız **queryType = full** tam sorgu sözdizimi ile varsayılana geri sıfırlayabiliyor **queryType = basit**.

## <a name="example-1-field-scoped-query"></a>Örnek 1: Alan kapsamlı sorgu

İlk sorgu değil (sorgusu için hem basit hem de tam sözdizimi çalışır) söz dizimi özgü ancak makul okunabilir bir JSON yanıtı üreten bir temel sorgu kavramı tanıtmak için bu örneği sağlama. Konuyu uzatmamak amacıyla, sorgu hedefleyen yalnızca *business_title* alan ve yalnızca iş başlıkları döndürülür belirtir. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&search=*
```

**SearchFields** parametresi yalnızca iş başlık alanı için arama kısıtlar. **Seçin** parametre, sonuç kümesinde hangi alanların ekleneceğini belirler.

Bu sorgu için yanıt, aşağıdaki ekran görüntüsüne benzer görünmelidir.

  ![Postman örnek yanıt](media/search-query-lucene-examples/postman-sample-results.png)

Arama puanı belirtilmemiş olsa bile arama puanı her belge için de döndürülen fark etmiş olabilirsiniz. Arama puanı sonuçları sıralama düzenini belirten bir değer ile meta verileri olmasıdır. 1 Tekdüzen puanları olduğunda hiçbir sıralama veya arama değil, tam metin araması olduğundan ya da uygulamak için ölçüt olduğundan oluşur. Null arama için ölçüt yoktur ve geri dönmeyi satırlar rastgele sırayla şöyledir. Arama ölçütleri hakkında daha fazla tanım yararlanırken, anlamlı değerlere puanları gelişmek arama görürsünüz.

## <a name="example-2-look-up-by-id"></a>Örnek 2: Arama kimliği

Bu örnek bir bit alışılmadık olduğu, ancak arama davranışlarını değerlendirirken, tüm neden dahil veya Eşleşmeyenler sonuçlara dahil anlamak üzere bir belge içeriğini incelemek isteyebilirsiniz. Tüm belgeyi döndürmek için bir [arama işlemi](https://docs.microsoft.com/rest/api/searchservice/lookup-document) belge kimliği geçirmek için

Tüm belgeleri, benzersiz bir tanımlayıcıya sahip. Bir arama sorgusu söz diziminin denemek için kullanmak üzere bulabilmek belge kimlikleri listesini başta döndürür. NYC işleri için tanımlayıcılar içinde depolanan `id` alan.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=id&$select=id&search=*
 ```

Sonraki örnek bir arama sorgusu dayalı belirli bir belge döndürme olan `id` önceki yanıtta ilk göründüğü "9E1E3AF9-0660-4E00-AF51-9B654925A2D5". Aşağıdaki sorgu, yalnızca seçili alanları, tüm belgeyi döndürür. 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs/9E1E3AF9-0660-4E00-AF51-9B654925A2D5?api-version=2017-11-11&$count=true&search=*
 ```

## <a name="example-3-search-precision"></a>Örnek 3: Arama duyarlık

Terim, bağımsız olarak değerlendirilen tek terimleri, bunları, muhtemelen çoğunu sorgulardır. Tümcecik sorguları tırnak işareti içine alınmış ve bir verbatim dizesi değerlendirilir. Duyarlık eşleşmenin işleçler ve searchMode tarafından denetlenir.

Örnek 1: **`&search=fire`** burada tüm sözcük yangın belgedeki yere eşleşmeleri 150 sonuçları döndürür.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire
```

Örnek 2: **`&search=fire department`** 2002 sonuçlarını döndürür. Eşleşme yangın veya bölüm içeren belgeleri döndürülür.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search=fire department
```

Örnek 3: **`&search="fire department"`** 82 sonuçlarını döndürür. Her iki terim verbatim bir arama dizesini tırnak işaretleri içine kapsayan olduğu ve eşleşme parçalanmış koşulları birleşik koşullarını oluşan dizininde bulunur. Bu gibi bir arama neden açıklıyor **`search=+fire +department`** eşdeğer değildir. Her iki terim gerekiyor, ancak için bağımsız olarak taranır. 

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&search="fire department"
```

## <a name="example-4-booleans-with-searchmode"></a>Örnek 4: Boole değerlerini searchMode ile

Basit söz dizimi karakter biçiminde Boole işleçleri destekler (`+, -, |`). İle kesinlik ve geri çağırırsanız, bir denge searchMode parametresi bildiren `searchMode=any` geri çağırma öncelik tanıdığından (herhangi bir ölçüte uyan niteleyen sonuç kümesi için bir belge), ve `searchMode=all` öncelik tanıdığından duyarlık (tüm ölçütleri eşleştirilmelidir). Varsayılan `searchMode=any`, hangi kullanılabilir birden çok işleç bir sorgu yığın, kafa karıştırıcı ve daha dar sonuçları yerine daha geniş alınıyor. Burada sonuçlarında "içeren değil" tüm belgeleri NOT ile özellikle, böyle bir terim.

(Any) varsayılan searchMode kullanarak 2800 belgeler döndürülür: birden çok parça içeren "Metrotech Center" terimi olmayan tüm belgelerin yanı sıra "fire departmanı" terimi.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=any&search="fire department"  -"Metrotech Center"
```
Değiştirme için searchMode `all` bir toplu etkisi ölçütlere zorunlu kılan ve tüm "fire departman", bu işleri Metrotech merkezi adresten eksi ifadesini içeren belgeleri oluşan daha küçük sonuç kümesi - 21 belgeleri - döndürür.

```
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchMode=all&search="fire department"  -"Metrotech Center"
```


## <a name="example-5-structuring-results"></a>Örnek 5: Yapılandırma sonuçları

Her batch ve sıralama düzeni döndürülen belgelerin sayısını alanlar aramaya olan birkaç parametre denetimi sonuçlanır. Bu örnek, önceki örneklerde, birkaçını sonuçları belirli alanlara kullanarak sınırlama resurfaces **$select** deyimi ve 82 eşleşme dönmeden verbatim arama ölçütü 

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"
```
Önceki örnekte eklenen, başlığa göre sıralayabilirsiniz. Bu sıralama civil_service_title olduğundan çalışır *sıralanabilir* dizinde.

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title
```

Disk belleği sonuçları kullanılarak gerçekleştirilir **$top** parametresi, bu durumda ilk 5 belgeleri döndüren:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=0
```

Sonraki 5 almak için ilk batch atla:

```http
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&$select=job_id,agency,business_title,civil_service_title,work_location,job_description&search="fire department"&$orderby=civil_service_title&$top=5&$skip=5
```

## <a name="next-steps"></a>Sonraki adımlar
Kodunuzda sorguları belirtmeyi deneyin. Aşağıdaki bağlantıları arama sorguları hem .NET hem de varsayılan basit söz dizimi kullanarak REST API için nasıl yapılacağını açıklar.

* [Azure Search .NET SDK kullanarak dizininizi sorgulama](search-query-dotnet.md)
* [Azure Search REST API kullanarak dizininizi sorgulama](search-query-rest-api.md)

Ek söz dizimi başvurusu, sorgu mimarisi ve örnekleri aşağıdaki bağlantılarda bulunabilir:

+ [Lucene sözdizimi sorgu örnekleri Gelişmiş sorgular oluşturmak için](search-query-lucene-examples.md)
+ [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
+ [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Tam Lucene sorgu](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)
