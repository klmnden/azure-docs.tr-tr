---
title: "Azure konum tabanlı Hizmetleri (Önizleme) arama hizmetini kullanarak bir adres için arama yapma | Microsoft Docs"
description: "Azure konum tabanlı Hizmetleri (Önizleme) arama hizmeti kullanmak için bir adres arama öğrenin"
services: location-based-services
keywords: "Verme ekleyin veya SEO uzmanınıza danışmanlık olmadan anahtar sözcükleri düzenleyin."
author: philmea
ms.author: philmea
ms.date: 11/29/2017
ms.topic: how-to
ms.service: location-based-services
ms.openlocfilehash: d928e4ff7c6e35291bcc1e6a1359d54542968278
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-find-an-address-using-the-azure-location-based-services-preview-search-service"></a>Azure konum tabanlı Hizmetleri (Önizleme) arama hizmeti kullanarak adresi bulma
Arama hizmeti adresleri, yerler, ilgi, iş listelerini ve diğer coğrafi bilgi noktaları için aranacak geliştiricileri için tasarlanmış API'leri RESTful kümesidir. Arama hizmeti, belirli bir adresi, çapraz Sokak, coğrafi özelliği veya ilgi çekici (s) için enlem/boylam atar. Enlem ve boylam değerleri arama hizmeti API tarafından döndürülen Azure konum tabanlı Hizmetleri rota ve trafik akışını API'leri gibi diğer parametre olarak kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
Yükleme [Postman uygulama](https://www.getpostman.com/apps).

Bir Azure konum tabanlı Hizmetleri hesabınızı ve aboneliğinizi anahtarı. Bir hesap oluşturma ve abonelik anahtarı alma hakkında daha fazla bilgi için bkz: [Azure konum tabanlı hizmetleri hesabı ve anahtarlarını yönetme](how-to-manage-account-keys.md). 

## <a name="using-fuzzy-search"></a>Benzer arama kullanma

Arama hizmeti için varsayılan adres veya POI belirteçlerin herhangi bir bileşimini girdileri işleme benzer arama API'dir. Bu arama API kurallı ' tek satırlı arama ' ve bir arama sorgusu, hangi kullanıcı girişleri bilmiyorsanız durumlarda faydalıdır. Benzer arama API POI arama ve coğrafi kodlama birleşimidir. API bağlamsal bir konum (LAT/lon. Ayrıca ağırlıklı , tam olarak bir koordinat ve RADIUS, tarafından kısıtlı eşleştirin) veya daha genel bağlantı noktası biasing herhangi bir coğrafi çalıştırılabilir.

Varsayılan olarak çoğu arama sorguları ' maxFuzzyLevel performans elde etmek ve olağan dışı sonuçları azaltmak için = 1'. Her istek için sorgu parametresi geçirerek gerektiğinde bu varsayılanı geçersiz kılınabilir ' maxFuzzyLevel = 2' veya '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Benzer arama özelliğini kullanarak bir adres arayın

1. Postman uygulamasını açın, Yeni'ye tıklayın | Yeni oluşturun ve seçin **GET isteğini**. Bir istek adını girin **benzer arama**, koleksiyon veya kaydetmesi ve'klasörü seçin **kaydetmek**.

    Daha fazla bilgi için Postman'ın istekleri belgelerine bakın.

2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi ve istek URL'si, API uç noktası için girin.

    ![Benzer arama ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://Atlas.microsoft.com/search/Fuzzy/JSON? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

    **Json** URL yolunu özniteliğinde yanıt biçimi belirler. Bu makale boyunca json kullanım kolaylığı ve Okunabilirlik için kullanıyorsunuz. Kullanılabilir yanıt biçimlerde bulabilirsiniz **arama belirsiz almak** [konum tabanlı Hizmetleri işlevsel API başvuru] tanımını (https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchfuzzy).

3. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:

    ![Benzer arama ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | *Abonelik anahtarı* |
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

Tam veya kısmi bir adres arama adresi API'sine geçirin ve enlem ve boylam konumsal değerlerinin yanı sıra belediye veya alt bölümü gibi ayrıntılı adres özellikleri içeren bir yanıtı alırsınız.

1. Postman içinde tıklatın **yeni istek** | **GET isteğini** ve adlandırın **adresi arama**.
2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi, API uç noktası için istek URL'sini girin ve varsa bir yetkilendirme protokolünü seçin.

    ![Adresi arama ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://Atlas.microsoft.com/search/Address/JSON? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

2. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    ![Adresi arama ](./media/how-to-search-for-address/address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | *Abonelik anahtarı* |
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
    | İstek URL'si | https://Atlas.microsoft.com/search/Address/reverse/JSON? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
2. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    ![Adres arama parametrelerini ters çevir ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | *Abonelik anahtarı* |
    | sorgu | 47.59093,-122.33263 |
    
3. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 
    
    Yanıt POI giriş Safeco alanı için "stadyum" ile bir poı kategorisi içerir. 
    
4. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | numarası | true |

    Varsa [numarası](https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi istekle birlikte gönderilen, yanıt Sokak (sol/sağ) ve ayrıca bu sayıyı için uzaklık konumu tarafında içerebilir.
    
5. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | spatialKeys | true |

    Zaman [spatialKeys](https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yanıt belirtilen konum için özel coğrafi uzamsal anahtar bilgileri içerir.

6. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | true |
    
    Zaman [returnSpeedLimit](https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmış, yanıt dönüş gönderilen hız sınırı.

7. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | true |

    Zaman [returnRoadUse](https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi olarak ayarlanmışsa, yanıt Sokak düzeyinde reversegeocodes için yol kullanım dizisi döndürür.

8. Aşağıdaki anahtarı ekleyin / değer çifti **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | roadUse | true |

    Yol kullanarak bir özel tür için ters geocode sorgu kısıtlayabilirsiniz [roadUse](https://docs.microsoft.com/en-us/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) sorgu parametresi.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Çapraz sokak adresi arası Sokak arama ters kullanarak arayın

1. Postman içinde tıklatın **yeni istek** | **GET isteğini** ve adlandırın **Sokak arama arası adresi ters**.

2. Oluşturucu sekmesinde seçin **almak** HTTP yöntemi ve istek URL'si, API uç noktası için girin.
    
    ![Adres arası Sokak arama ters çevir ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | https://Atlas.microsoft.com/search/Address/reverse/crossstreet/JSON? |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
    
3. Tıklatın **Params**ve aşağıdaki anahtarı girin / değer çiftleri sorgu veya yol parametreleri istek URL'sindeki olarak kullanın:
    
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | *Abonelik anahtarı* |
    | sorgu | 47.59093,-122.33263 |
    
4. Tıklatın **Gönder** ve yanıt gövdesi gözden geçirin. 

## <a name="next-steps"></a>Sonraki adımlar
- Araştır [Azure konum tabanlı Serices arama hizmeti](https://docs.microsoft.com/en-us/rest/api/location-based-services/search) API belgeleri 
