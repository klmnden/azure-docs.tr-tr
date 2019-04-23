---
title: Azure haritalar Arama Hizmeti'ni kullanarak bir adres için arama yapma | Microsoft Docs
description: Azure haritalar Arama Hizmeti'ni kullanarak bir adres arama hakkında bilgi edinin
author: walsehgal
ms.author: v-musehg
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8ab2c73030c0860fc709a774b9fd84d20a6d7c99
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59785590"
---
# <a name="find-an-address-using-the-azure-maps-search-service"></a>Azure haritalar arama hizmetini kullanarak bir adres bulma

Haritalar arama hizmeti, adresler, yerler, ilgi alanı, iş dökümleri ve diğer coğrafi bilgileri noktaları için aranacak geliştiriciler için tasarlanmış bir RESTful API'ler kümesidir. Hizmet belirli bir adresi, çapraz olan Sokak, coğrafi özellik veya ilgi çekici (POI) için enlem/boylam atar. Arama sonucunda döndürülen enlem ve boylam değerleri, rota ve trafik akışı gibi diğer haritalar Hizmetleri parametreler olarak kullanılabilir.

Bu makalede, öğreneceksiniz, nasıl yapılır:

* Bir adresi kullanarak arama [belirsiz arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* Özellikler ve koordinatları yanı sıra adres arama
* Olun bir [ters adresi arama](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) aramak için bir sokak adresi
* Çapraz Sokak kullanarak arama [arama adresi ters arası Sokak API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet)

## <a name="prerequisites"></a>Önkoşullar

Haritalar hizmetini API çağrıları yapmak için bir haritalar hesabı ve anahtarı gereklidir. Hesap oluşturma ve anahtar alma hakkında daha fazla bilgi için bkz: [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md).

Bu makalede [Postman uygulamasını](https://www.getpostman.com/apps) REST çağrılarını oluşturulacak. Tercih ettiğiniz herhangi bir API geliştirme ortamında kullanabilirsiniz.

## <a name="using-fuzzy-search"></a>Belirsiz aramayı kullanma

Arama hizmeti için varsayılan API [belirsiz arama](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ve bir arama sorgusu için kullanıcı girişleri nelerdir bilmiyorsanız olduğunda yararlıdır. API, POI arama ve coğrafi kodlama canonical 'tek satır arama' halinde birleştirir. Örneğin, API, herhangi bir adres veya POI belirteci birleşimi girişleri başa çıkabilir. Bu ayrıca bağlamsal bir konum (LAT/lon. ağırlıklı çifti), tam olarak bir koordinat ve RADIUS tarafından kısıtlanmış veya daha genel bağlantı noktası biasing herhangi bir coğrafi yürütüldü.

Varsayılan olarak çoğu arama sorguları `maxFuzzyLevel=1` performans elde edin ve olağan dışı sonuçları azaltmak için. Bu varsayılan sorgu parametresini geçirerek her istek için gerektiği gibi geçersiz kılınabilir `maxFuzzyLevel=2` veya `3`.

### <a name="search-for-an-address-using-fuzzy-search"></a>Belirsiz arama kullanarak adres arama

1. Postman uygulamasını açın, yeni | Yeni oluşturun ve seçin **GET isteği**. Bir istek adı girin **belirsiz arama**, bir koleksiyon ya da kaydedin ve klasör seç **Kaydet**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.

    ![Belirsiz arama](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

    **Json** özniteliği URL yolunda yanıt biçimi belirler. Kullanım kolaylığı ve Okunabilirlik için bu makalenin tamamında json kullanıyor. Kullanılabilir yanıt biçimlerde bulabilirsiniz **arama belirsiz alma** tanımını [haritalar işlevsel API Başvurusu](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:

    ![Belirsiz arama](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | pizza |

4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin.

    10 "pizza" belirsiz sorgu dizesi döndürdü [ilgi sonucunun noktası](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (POI) sonuçları kategorileri "pizza" ve "Restoran" dönülüyor. Her sonuç döndüren bir adresin enlem / boylam değerleri, bağlantı noktasını ve giriş noktası konumu için görünümü.
  
    Sonuçları herhangi bir belirli başvuru konuma bağlı değildir, bu sorgu için farklılık gösterir. Kullanabileceğiniz **countrySet** potansiyel olarak gereksiz sonuçları döndüren dünyaya aramak için varsayılan davranış olduğu gibi yalnızca uygulamanızın gereken kapsama, ülkelerin belirtmek için parametre.

5. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |------------------|-------------------------|
    | countrySet | ABD |
  
    Sonuçları artık ülke kodu tarafından sınırlanan ve sorgu Amerika Birleşik Devletleri'nde pizza restoranlar döndürür.
  
    İlgi noktası sorgu sonuçları için bir konum sağlayacak ve, belirsiz arama hizmeti çağrısı döndürülen enlem ve boylam değerleri kullanın. Bu durumda, arama hizmeti Seattle alanı iğne konumu döndürmek için kullanılan ve LAT kullanılan / lon. Arama yönlendirmek için değerler.
  
6. Params içinde aşağıdaki anahtarını girin / değer çiftlerini ve tıklayın **Gönder**:

    ![Belirsiz arama](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Anahtar | Değer |
    |-----|------------|
    | LAT | 47.620525 |
    | lon | -122.349274 |

## <a name="search-for-address-properties-and-coordinates"></a>Adres özelliklerini ve koordinatları Ara

API arama adresine tam veya kısmi bir adres geçirin ve enlem ve boylam belediye veya alt yanı sıra, konumsal değerleri gibi ayrıntılı adres özelliklerini içeren bir yanıt alırsınız.

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **adresi arama**.
2. Oluşturucu sekmesinde **alma** HTTP yöntemi, API uç noktanız için istek URL'sini girin ve varsa bir Yetkilendirme Protokolü seçin.

    ![Adres Arama](./media/how-to-search-for-address/address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) |
    | Yetkilendirme | Hiçbir kimlik doğrulama |

3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
  
    ![Adres Arama](./media/how-to-search-for-address/address_search_params.png)
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 400 geniş St, Seattle, WA 98109 |
  
4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin.
  
    Bu durumda, belirtilen tam adresi sorgu ve yanıt gövdesi içinde tek bir sonuç alırsınız.
  
5. Params içinde aşağıdaki değerle sorgu dizesini düzenleyin:
    ```plaintext
        400 Broad, Seattle
    ```

6. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | typeahead | true |

    **Typeahead** bayrağı sorgu kısmi bir girdi olarak kabul et ve Tahmine dayalı bir değerler dizisini döndürmek için adres arama API'si söyler.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Adres Arama ters kullanarak bir adres arayın

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **ters adresi arama**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.
  
    ![Ters adresi arama URL'si](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
  
3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
  
    ![Adres arama parametrelerini ters çevir](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 47.591180,-122.332700 |
  
4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin.

    Yanıt Safeco Field'daki sayı anahtar adresi bilgilerini içerir.
  
5. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | number | true |

    Varsa [numarası](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi istekle birlikte gönderilen, yanıt Sokak (sol/sağ) ve ayrıca kaydırmak için bu sayıyı tarafında içerebilir.
  
6. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnSpeedLimit | true |
  
    Zaman [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) yanıtı döndürür gönderilen Hız sınırını sorgu parametresi ayarlanır.

7. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | returnRoadUse | true |

    Zaman [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi olarak ayarlanmışsa, ters geocodes Sokak düzeyinde için yol kullanmak dizi yanıtı döndürür.

8. Aşağıdaki anahtar eklemek / değer çiftine **Params** 'ye tıklayın **Gönder**:

    | Anahtar | Değer |
    |-----|------------|
    | roadUse | true |

    Yol kullanarak, belirli bir türe ters geocode sorgu kısıtlayabilirsiniz [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgu parametresi.
  
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Çapraz sokak adresi arası Sokak arama ters kullanarak arayın

1. Postman içinde tıklayın **yeni istek** | **GET isteği** ve adlandırın **Sokak arama çapraz adresi ters**.

2. Oluşturucu sekmesinde **alma** HTTP yöntemi ve API uç noktanız için istek URL'sini girin.
  
    ![Ters adresi arası Sokak arama](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parametre | Önerilen değer |
    |---------------|------------------------------------------------|
    | HTTP yöntemi | GET |
    | İstek URL'si | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Yetkilendirme | Hiçbir kimlik doğrulama |
  
3. Tıklayın **Params**ve aşağıdaki anahtarı girin / değer çiftleri olarak istek URL'si içinde sorgu veya yol parametrelerini kullanmak için:
  
    | Anahtar | Değer |
    |------------------|-------------------------|
    | API sürümü | 1.0 |
    | Abonelik anahtarı | \<Azure haritalar anahtarınız\> |
    | sorgu | 47.591180,-122.332700 |
  
4. Tıklayın **Gönder** ve yanıt gövdesinin gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

- Keşfedin [Azure haritalar, arama hizmetinizi](https://docs.microsoft.com/rest/api/maps/search) API belgeleri.
