---
title: Azure eşlemeleri Search hizmetini kullanarak bir adres için arama yapma | Microsoft Docs
description: Azure eşlemeleri Search hizmetini kullanarak bir adres için arama öğrenin
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1acb95af7b62641c371627d6250067f9c2eac99c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600315"
---
# <a name="how-to-find-an-address-using-the-azure-maps-search-service"></a>Azure eşlemeleri arama hizmetini kullanarak bir adres bulma

Maps arama hizmeti adresleri, yerler, ilgi, iş listelerini ve diğer coğrafi bilgi noktaları için aranacak geliştiricileri için tasarlanmış API'leri RESTful kümesidir. Hizmet, belirli bir adresi, çapraz Sokak, coğrafi özelliği veya ilgi çekici (s) için enlem/boylam atar. Araması tarafından döndürülen enlem ve boylam değerleri, rota ve trafik akışı gibi diğer eşlemeleri hizmetlerini parametre olarak kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Maps hizmet API'leri yapılan her çağrı yapmak için Maps hesabı ve anahtarı gerekir. Bir hesap oluşturma ve bir anahtar alma hakkında daha fazla bilgi için bkz: [Azure eşlemeleri hesabınızın ve anahtarlarını yönetme](how-to-manage-account-keys.md).

Bu makalede kullanan [Postman uygulama](https://www.getpostman.com/apps) REST çağrılarını oluşturmak için. Tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz. 


## <a name="using-fuzzy-search"></a>Benzer arama kullanma

Arama hizmeti için varsayılan API [benzer arama](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), adres veya POI belirteçlerin herhangi bir bileşimini girişleri işler. Bu arama API kurallı ' tek satırlı arama ' ve bir arama sorgusu, hangi kullanıcı girişleri bilmiyorsanız durumlarda faydalıdır. Benzer arama API POI arama ve coğrafi kodlama birleşimidir. API bağlamsal bir konum (LAT/lon. Ayrıca ağırlıklı , tam olarak bir koordinat ve RADIUS, tarafından kısıtlı eşleştirin) veya daha genel bağlantı noktası biasing herhangi bir coğrafi çalıştırılabilir.

Varsayılan olarak çoğu arama sorguları ' maxFuzzyLevel performans elde etmek ve olağan dışı sonuçları azaltmak için = 1'. Her istek için sorgu parametresi geçirerek gerektiğinde bu varsayılanı geçersiz kılınabilir ' maxFuzzyLevel = 2' veya '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Benzer arama özelliğini kullanarak bir adres arayın

1. Postman uygulamasını açın, Yeni'ye tıklayın | Yeni oluşturun ve seçin **GET isteğini**. Bir istek adını girin **benzer arama**, koleksiyon veya kaydetmesi ve'klasörü seçin **kaydetmek**.

2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi ve istek URL'si, API uç noktası için girin.

    ![Benzer arama ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/fuzzy/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

    **Json** URL yolunu özniteliğinde yanıt biçimi belirler. Bu makale boyunca json kullanım kolaylığı ve Okunabilirlik için kullanıyorsunuz. Kullanılabilir yanıt biçimlerde bulabilirsiniz **arama belirsiz alma** [eşlemeleri işlevsel API başvuru] tanımını (https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:

    ![Benzer arama ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure eşlemeleri anahtarınızı\> |
    | sorgu | pizza |

4. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 

    "Pizza" belirsiz sorgu dizesi "pizza" ve "Restoran" dönmeden kategorilerle 10 nokta faiz (POI) sonuç döndürdü. Bir adresi enlem her sonucunu döndürür / boylam değerleri görünümü bağlantı noktası ve konumu için giriş noktası.
    
    Sonuçlar herhangi bir belirli başvuru konuma bağlanmayan bu sorgu için farklılık gösterir. Kullanabileceğiniz **countrySet** olası gereksiz sonuçları döndüren dünyaya aramak için varsayılan davranış olduğu gibi yalnızca uygulamanız gereken kapsamı, ülkelerin belirtmek için parametre.

5. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |------------------|-------------------------|
    | countrySet | ABD |
    
    Sonuçları şimdi ülke kodu tarafından ilişkisindeki ve sorgu pizza Restoran Amerika Birleşik Devletleri'nde döndürür.
    
    Belirli bir konuma yönelik sonuçları sağlamak için bir ilgi sorgular ve döndürülen enlem ve boylam değerleri belirsiz arama hizmeti için aramayı kullanın. Bu durumda, arama hizmeti Seattle alanı iğnenin konumunu döndürmek için kullanılan ve LAT kullanılan / lon. Arama yönlendirmek için değerler.
    
4. Params içinde aşağıdaki anahtarı girin / değer çiftleri ve tıklatın **Gönder**:

    ![Benzer arama ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Anahtar | Değer |
    |-----|------------|
    | LAT | 47.62039 |
    | lon | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Adres özellikleri ve koordinatları arayın 

Tam veya kısmi bir adres arama adresine API geçirebilir ve enlem ve boylam konumsal değerlerinin yanı sıra belediye veya alt bölümü gibi ayrıntılı adres özellikleri içeren bir yanıtı alırsınız.

1. Postman içinde tıklatın **yeni istek** | **GET isteğini** ve adlandırın **adresi arama**.
2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi, API uç noktası için istek URL'sini girin ve varsa bir yetkilendirme protokolünü seçin.

    ![Adresi arama ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

2. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    ![Adresi arama ](./media/how-to-search-for-address/address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure eşlemeleri anahtarınızı\> |
    | sorgu | 400 geniş St, Seattle, WA 98109 |
    
3. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 
    
    Bu durumda, belirtilen bir tam adresi sorgu ve yanıt gövdesi içinde tek bir sonuç almak. 
    
4. Parametreleri sorgu dizesi şu değere düzenleyin:
    ```
        400 Broad, Seattle
    ```

5. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | typeahead | true |

    **Typeahead** bayrağı sorgu kısmi bir girdi olarak kabul eder ve Tahmine dayalı değerler dizisi dönmek için adres arama API söyler.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Ters adresi arama özelliğini kullanarak bir adres arayın
1. Postman içinde tıklatın **yeni istek** | **GET isteğini** ve adlandırın **ters adresi arama**.

2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi ve istek URL'si, API uç noktası için girin.
    
    ![Geriye doğru adresi arama URL'si ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/reverse/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
2. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    ![Adres arama parametrelerini ters çevir ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure eşlemeleri anahtarınızı\> |
    | sorgu | 47.59093,-122.33263 |
    
3. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 
    
    Yanıt POI giriş Safeco alanı için "stadyum" ile bir poı kategorisi içerir. 
    
4. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | number | true |

    Varsa [numarası](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi istekle birlikte gönderilen, yanıt Sokak (sol/sağ) ve ayrıca bu sayıyı için uzaklık konumu tarafında içerebilir.
    
5. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | spatialKeys | true |

    Zaman [spatialKeys](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yanıt belirtilen konum için özel coğrafi uzamsal anahtar bilgileri içerir.

6. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | true |
    
    Zaman [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmış, yanıt dönüş gönderilen hız sınırı.

7. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | true |

    Zaman [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yanıt Sokak düzeyinde reversegeocodes için yol kullanım dizisi döndürür.

8. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | roadUse | true |

    Yol kullanarak bir özel tür için ters geocode sorgu kısıtlayabilirsiniz [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Çapraz sokak adresi arası Sokak arama ters kullanarak arayın

1. Postman içinde tıklatın **yeni istek** | **GET isteğini** ve adlandırın **Sokak arama arası adresi ters**.

2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi ve istek URL'si, API uç noktası için girin.
    
    ![Adres arası Sokak arama ters çevir ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
3. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure eşlemeleri anahtarınızı\> |
    | sorgu | 47.59093,-122.33263 |
    
4. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 

## <a name="next-steps"></a>Sonraki adımlar
- Araştır [Azure eşlemeleri search hizmeti](https://docs.microsoft.com/rest/api/maps/search) API belgeleri 
