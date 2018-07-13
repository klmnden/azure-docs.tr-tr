---
title: Azure haritalar Arama Hizmeti'ni kullanarak bir adres için arama yapma | Microsoft Docs
description: Azure haritalar Arama Hizmeti'ni kullanarak bir adres arama hakkında bilgi edinin
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8b7d2119e1eef8532c30b0a45ae2684493462277
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990022"
---
# <a name="how-to-find-an-address-using-the-azure-maps-search-service"></a>Azure haritalar arama hizmetini kullanarak bir adres bulma

Hizmet eşlemeleri arama API'leri geliştiriciler, adresler, yerler, ilgi alanı, iş dökümleri ve diğer coğrafi bilgileri noktaları için arama yapmak için tasarlanmış bir RESTful kümesi özelliğidir. Hizmet belirli bir adresi, çapraz olan Sokak, coğrafi özellik veya ilgi çekici (POI) için enlem/boylam atar. Arama sonucunda döndürülen enlem ve boylam değerleri, rota ve trafik akışı gibi diğer haritalar Hizmetleri parametreler olarak kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Haritalar hizmetini API çağrıları yapmak için bir haritalar hesabı ve anahtarı gereklidir. Hesap oluşturma ve anahtar alma hakkında daha fazla bilgi için bkz: [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md).

Bu makalede [Postman uygulamasını](https://www.getpostman.com/apps) REST çağrılarını oluşturulacak. Tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz. 


## <a name="using-fuzzy-search"></a>Belirsiz aramayı kullanma

Arama hizmeti için varsayılan API [belirsiz arama](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), adres veya POI belirteçlerin herhangi bir birleşimini girişleri işler. Bu arama API'si, canonical ' tek satır arama ' ve bir arama sorgusu, hangi kullanıcı girişleri biliyor musunuz gerektiğinde faydalıdır. Belirsiz arama API'si, POI arama ve coğrafi kodlama birleşimidir. API bağlamsal bir konum (LAT/lon. Ayrıca ağırlıklı , tam olarak bir koordinatları ve yarıçap tarafından kısıtlanmış pair) veya daha genel bağlantı noktası biasing herhangi bir coğrafi yürütülmek üzere.

Çoğu arama sorguları için varsayılan ' maxFuzzyLevel performans elde edin ve olağan dışı sonuçları azaltmak için = 1'. Bu varsayılan sorgu parametresini geçirerek her istek için gerektiği gibi geçersiz kılınabilir ' maxFuzzyLevel = 2' veya '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Belirsiz arama kullanarak adres arama

1. Postman uygulamasını açın, yeni | Yeni oluşturun ve seçin **GET isteği**. Bir istek adı girin **belirsiz arama**, bir koleksiyon ya da kaydedin ve klasör seç **Kaydet**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.

    ![Belirsiz arama ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/fuzzy/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

    **Json** özniteliği URL yolunda yanıt biçimi belirler. Kullanım kolaylığı ve Okunabilirlik için bu makalenin tamamında json kullanıyor. Kullanılabilir yanıt biçimlerde bulabilirsiniz **arama belirsiz alma** tanımı [haritalar işlevsel API Başvurusu] (https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:

    ![Belirsiz arama ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | pizza |

4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin. 

    "Pizza" belirsiz sorgu dizesi "pizza" ve "Restoran" dönülüyor kategorileri 10 noktası (POI) ilgilendiğiniz sonuçları döndürdü. Her sonuç döndüren bir adresin enlem / boylam değerleri, bağlantı noktasını ve giriş noktası konumu için görünümü.
    
    Sonuçları herhangi bir belirli başvuru konuma bağlı değildir, bu sorgu için farklılık gösterir. Kullanabileceğiniz **countrySet** potansiyel olarak gereksiz sonuçları döndüren dünyaya aramak için varsayılan davranış olduğu gibi yalnızca uygulamanızın gereken kapsama, ülkelerin belirtmek için parametre.

5. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |------------------|-------------------------|
    | countrySet | ABD |
    
    Sonuçları artık ülke kodu tarafından sınırlanan ve sorgu Amerika Birleşik Devletleri'nde pizza restoranlar döndürür.
    
    Belirli bir konuma yönelik sonuçları sağlamak için ilgi noktası sorgulamak ve, belirsiz arama hizmeti çağrısı döndürülen enlem ve boylam değerleri kullanın. Bu durumda, arama hizmeti Seattle alanı iğne konumu döndürmek için kullanılan ve LAT kullanılan / lon. Arama yönlendirmek için değerler.
    
4. Params içinde aşağıdaki anahtarını girin / değer çiftlerini ve tıklayın **Gönder**:

    ![Belirsiz arama ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Anahtar | Değer |
    |-----|------------|
    | LAT | 47.62039 |
    | lon | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Adres özelliklerini ve koordinatları Ara 

API arama adresine tam veya kısmi bir adres geçirin ve enlem ve boylam belediye veya alt yanı sıra, konumsal değerleri gibi ayrıntılı adres özelliklerini içeren bir yanıt alırsınız.

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **adresi arama**.
2. Oluşturucu sekmesinde **alma** HTTP yöntemi, API uç noktanız için istek URL'sini girin ve varsa bir Yetkilendirme Protokolü seçin.

    ![Adres Arama ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

2. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
    
    ![Adres Arama ](./media/how-to-search-for-address/address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 400 geniş St, Seattle, WA 98109 |
    
3. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin. 
    
    Bu durumda, belirtilen tam adresi sorgu ve yanıt gövdesi içinde tek bir sonuç alırsınız. 
    
4. Params içinde aşağıdaki değerle sorgu dizesini düzenleyin:
    ```
        400 Broad, Seattle
    ```

5. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | typeahead | true |

    **Typeahead** bayrağı sorgu kısmi bir girdi olarak kabul et ve Tahmine dayalı bir değerler dizisini döndürmek için adres arama API'si söyler.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Adres Arama ters kullanarak bir adres arayın
1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **ters adresi arama**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.
    
    ![Ters adresi arama URL'si ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/reverse/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
2. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
    
    ![Adres arama parametrelerini ters çevir ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 47.59093,-122.33263 |
    
3. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin. 
    
    Yanıt, bir "stadyum" poı kategorisiyle Safeco Field'daki sayı için POI giriş içeriyor. 
    
4. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | number | true |

    Varsa [numarası](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi istekle birlikte gönderilen, yanıt Sokak (sol/sağ) ve ayrıca kaydırmak için bu sayıyı tarafında içerebilir.
    
5. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | spatialKeys | true |

    Zaman [spatialKeys](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yanıtta belirtilen bir konuma için özel Jeo-uzamsal anahtar bilgilerini içerir.

6. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | true |
    
    Zaman [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) yanıtı döndürür gönderilen Hız sınırını sorgu parametresi ayarlanır.

7. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | true |

    Zaman [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yol kullanmak dizi reversegeocodes Sokak düzeyinde için yanıtı döndürür.

8. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | roadUse | true |

    Yol kullanarak, belirli bir türe ters geocode sorgu kısıtlayabilirsiniz [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Çapraz sokak adresi arası Sokak arama ters kullanarak arayın

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **Sokak arama çapraz adresi ters**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.
    
    ![Ters adresi arası Sokak arama ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 47.59093,-122.33263 |
    
4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin. 

## <a name="next-steps"></a>Sonraki adımlar
- Keşfedin [Azure haritalar, arama hizmetinizi](https://docs.microsoft.com/rest/api/maps/search) API belgeleri 
