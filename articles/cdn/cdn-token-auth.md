---
title: "Azure CDN varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme | Microsoft Docs"
description: "Azure CDN varlıklara erişimi güvenli hale getirmek için belirteci kimlik doğrulaması kullanmayı öğrenin."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/03/2017
ms.author: mezha
ms.openlocfilehash: 700f4c49bbcda1eccbcc7eafc703e625697fa2b4
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure içerik teslim ağı varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Belirteç kimlik doğrulama varlıklar yetkisiz istemcilerine hizmet veren Azure içerik teslim ağı (CDN) önlemek izin veren bir mekanizmadır. Belirteç kimlik doğrulama, genellikle ileti panosu genellikle, farklı bir Web varlıklarınızı izniniz olmadan kullanan içeriğinin "hotlinking" önlemek için yapılır. Hotlinking içerik teslim maliyetleriniz üzerinde bir etkisi olabilir. CDN içerik sunan önce CDN belirteci kimlik doğrulamasını etkinleştirerek, POP CDN kenarından istekleri doğrulanır. 

## <a name="how-it-works"></a>Nasıl çalışır?

Belirteç kimlik doğrulama istekleri istek sahibinin bu kodlanmış ayrı tutma bilgilerini belirteç değeri içeren isteklerine gerektirerek güvenilen bir site tarafından oluşturulan doğrular. Yalnızca kodlanmış bilgi gereksinimleri karşılıyorsa içerik için bir istek sunulan; Aksi takdirde, istek reddedilir. Bir veya daha fazla aşağıdaki parametreleri kullanarak gereksinimleri ayarlayabilirsiniz:

- Ülke: İzin ver veya tarafından belirtilen ülkelerde kaynaklanan istekleri reddetmesini kendi [ülke kodu](https://msdn.microsoft.com/library/mt761717.aspx).
- URL: Belirtilen varlık veya yol eşleşen istekleri izin verin.
- Ana bilgisayarı: İzin ver veya istek üstbilgisinde belirtilen konakların kullanan isteklerini reddedin.
- Başvuran: İzin ver veya belirtilen başvuran isteği reddedin.
- IP adresi: belirli bir IP adresi veya IP alt ağı kaynaklanan isteklere izin vermek.
- Protokol: İzin ver veya içeriği istemek için kullanılan protokolünü temel istekleri reddedin.
- Süre sonu: bağlantı yalnızca sınırlı bir süre boyunca geçerli olmaya devam ettiğinden emin olmak için bir tarih ve saat dönemi atayabilir.

Daha fazla bilgi için her parametre için ayrıntılı yapılandırma örneklerini görmek [belirteç kimlik doğrulamayı ayarlama](#setting-up-token-authentication).

Şifrelenmiş bir simge oluşturduktan sonra bu URL yolun sonuna bir sorgu dizesi olarak ekler. Örneğin, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.

## <a name="reference-architecture"></a>Başvuru mimarisi

Aşağıdaki iş akışı diyagramı belirteci kimlik doğrulaması CDN web uygulamanızın üzerinde çalışmak için nasıl kullandığını açıklar.

![CDN belirteç kimlik doğrulama iş akışı](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN uç noktasında belirteci Doğrulama mantığı
    
Aşağıdaki akış çizelgesi, nasıl Azure CDN belirteci kimlik doğrulaması CDN uç yapılandırıldığında, istemci isteği doğrular açıklar.

![CDN belirteci Doğrulama mantığı](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Belirteç kimlik doğrulamayı ayarlama

1. Gelen [Azure portal](https://portal.azure.com), CDN profilinize gidin ve ardından **Yönet** ek Portalı'nı başlatmak için.

    ![CDN profili Yönet düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Üzerine gelerek **HTTP büyük**ve ardından **belirteci Auth** çıkma içinde. Ardından şifreleme anahtarı ve şifreleme parametreleri aşağıdaki gibi ayarlayabilirsiniz:

    1. Bir benzersiz şifreleme anahtarı girin **birincil anahtar** kutusunda ve isteğe bağlı olarak bir yedek anahtarı girin **yedek anahtarı** kutusu.

        ![CDN belirteci auth kurulum anahtarı](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Şifreleme parametreleri şifrele aracıyla ayarlayın. Şifrele aracıyla izin verebilir veya sona erme zamanı, ülke, başvuran, protokol ve (herhangi bir birleşimini)'deki istemci IP göre istekleri reddetmesini. 

        ![CDN şifrelemek aracı](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Bir veya daha fazla aşağıdaki şifreleme parametreleri için değerler girin **şifrelemek aracı** alanı:  

       - **ec_expire**: geçmesi belirtecinin süresi dolmadan bir belirteç için süre sonu zamanı atar. Sona erme zamanı engellenir sonra gönderilen istek sayısı. Bu parametre standart dönem saniyesi sayısına dayalı bir UNIX zaman damgası kullanan `1/1/1970 00:00:00 GMT`. (Çevrimiçi araçlarını UNIX zaman Standart Saati arasında dönüştürmek için kullanabileceğiniz.) Örneğin, belirtecin anda süresi dolacak şekilde istiyorsanız `12/31/2016 12:00:00 GMT`, UNIX zaman damgası değeri kullanın `1483185600`aşağıdaki gibi. 
    
         ![CDN ec_expire örneği](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - **ec_url_allow**: belirli bir varlık veya yolu belirteçleri uyarlamak olanak tanır. URL'si Başlat belirli bir göreli yol isteklerine erişimi sınırlandırır. URL'leri büyük/küçük harfe duyarlıdır. Her yol virgül ile ayırarak birden fazla yol girin. Gereksinimlerinize bağlı olarak, farklı erişim düzeyini sağlamak için farklı değerlerini ayarlayabilirsiniz. 
        
         Örneğin, URL `http://www.mydomain.com/pictures/city/strasbourg.png`, bu istekleri için aşağıdaki giriş değerlerine izin verilir:

         - Giriş değeri `/`: tüm isteklerine izin verilir.
         - Giriş değeri `/pictures`, aşağıdaki isteklerine izin verilir:
            - `http://www.mydomain.com/pictures.png`
            - `http://www.mydomain.com/pictures/city/strasbourg.png`
            - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
         - Giriş değeri `/pictures/`: yalnızca istekleri içeren `/pictures/` yolu izin verilir. Örneğin, `http://www.mydomain.com/pictures/city/strasbourg.png`.
         - Giriş değeri `/pictures/city/strasbourg.png`: yalnızca bu belirli yol ve varlık izin ister.
    
       - **ec_country_allow**: yalnızca bir veya daha fazla belirtilen ülkelerden kaynaklanan isteklere izin verir. Diğer tüm ülkelerden kaynaklanan istekleri reddedilir. Ülke kodlarına kullanın ve her birini virgül ile ayırın. Yalnızca Amerika Birleşik Devletleri ve Fransa erişimine izin vermek istiyorsanız, örneğin, BİZE FR kutusuna aşağıdaki gibi girin.  
        
           ![CDN ec_country_allow örneği](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - **ec_country_deny**: bir veya daha fazla belirtilen ülkelerden kaynaklanan istekleri reddeder. Diğer tüm ülkelerden kaynaklanan isteklerine izin verilir. Ülke kodlarına kullanın ve her birini virgül ile ayırın. Örneğin, Amerika Birleşik Devletleri ve Fransa erişimini engellemek istiyorsanız, BİZE FR kutusuna girin.
    
       - **ec_ref_allow**: yalnızca belirtilen başvuran isteklere izin verir. Bir başvuran istenen kaynak bağlı web sayfasının URL'sini tanımlar. Protokol başvuran parametre değeri içermez. Giriş aşağıdaki türleri için parametre değeri verilir:
           - Bir ana bilgisayar adı veya bir ana bilgisayar adı ve yolu.
           - Birden çok başvuran. Birden çok başvuran eklemek için her başvuran virgül ile ayırın. Başvuran değeri belirtin, ancak tarayıcı yapılandırması nedeniyle isteği başvuran bilgi gönderilmez, bu istekleri varsayılan olarak reddedilir. 
           - Başvuran bilgileri eksik olan istek sayısı. Bu tür istekleri izin vermek için metin "eksik" girin veya boş bir değer girin. 
           - Alt etki alanları. Alt etki alanları izin vermek için bir yıldız işareti girin (\*). Örneğin, tüm alt etki alanlarına izin vermek için `consoto.com`, girin `*.consoto.com`. 
           
          Aşağıdaki örnek, gelen istekleri için erişime izin vermek için giriş gösterir `www.consoto.com`, altındaki tüm alt etki `consoto2.com`ve boş veya eksik başvuran istekleri.
        
          ![CDN ec_ref_allow örneği](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - **ec_ref_deny**: Belirtilen başvuran istekleri reddeder. Uygulama ec_ref_allow parametresi ile aynıdır.
         
       - **ec_proto_allow**: yalnızca belirtilen protokol gelen isteklere izin verir. Örneğin, HTTP veya HTTPS.
            
       - **ec_proto_deny**: Belirtilen protokolünden istekleri reddeder. Örneğin, HTTP veya HTTPS.
    
       - **ec_clientip**: Belirtilen sahibinin IP adresine erişimi kısıtlar. IPv4 ve IPv6 desteklenir. Bir tek istek IP adresi veya bir IP alt ağı belirtebilirsiniz.
            
         ![CDN ec_clientip örneği](./media/cdn-token-auth/cdn-token-auth-clientip.png)

    3. Şifreleme parametre değerlerini girme bitirdikten sonra (hem birincil hem de bir yedek anahtarı oluşturduysanız) şifrelemek için anahtar türü seçin **için anahtarı şifrelemek** listesinde, bir şifreleme sürümden  **Şifreleme sürümünü** listeleyin ve ardından **şifrele**.
        
    4. İsteğe bağlı olarak, belirteç şifre çözme aracıyla sınayın. Belirteç değeri yapıştırın **belirteç şifre çözme için** kutusu. Gelen şifresini çözmek için şifreleme anahtarı seçin **anahtarın şifresini çözmek** aşağı açılan listesinde ve ardından **şifresini**.

    5. İsteğe bağlı olarak, bir istek reddedildiğinde, döndürülen yanıt kodu türünü özelleştirin. Koddan seçin **yanıt kodu** aşağı açılan liste ve tıklatın **kaydetmek**. **403** yanıt kodu (Yasak), varsayılan olarak seçilidir. Belirli yanıt kodları, ayrıca, hata sayfasının URL'sini girebilirsiniz **üstbilgi değeri** kutusu. 

    6. Şifrelenmiş bir simge oluşturduktan sonra bu dosyanın sonuna bir sorgu dizesi olarak URL yolunuzda ekleyin. Örneğin, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.

3. Altında **HTTP büyük**, tıklatın **kurallar altyapısı**. Kurallar altyapısı özelliği geçerli, belirteç kimlik doğrulama özelliğini etkinleştirmek ve ek belirteç kimlik doğrulamayla ilgili özellikler etkinleştirmek için yollarını tanımlamak için kullanın. Daha fazla bilgi için bkz: [kurallar altyapısı başvuru](cdn-rules-engine-reference.md).

    1. Mevcut bir kuralı seçin veya belirteci kimlik doğrulaması uygulamak istediğiniz varlık veya yolunu tanımlamak için yeni bir kural oluşturun. 
    2. Bir kural belirteci kimlik doğrulamasını etkinleştirmek için seçin  **[belirteci Auth](cdn-rules-engine-reference-features.md#token-auth)**  gelen **özellikleri** aşağı açılan listesinde, ardından seçin **etkin**. Tıklatın **güncelleştirme** bir kural güncelleştiriyorsanız veya **Ekle** bir kural oluşturuluyorsa.
        
    ![CDN kurallar altyapısı belirteci kimlik doğrulamasını etkinleştir örnek](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Kuralları altyapısında ek belirteç kimlik doğrulamayla ilgili özellikler de etkinleştirebilirsiniz. Aşağıdaki özelliklerden herhangi birini etkinleştirmek için dosyayı seçin **özellikleri** aşağı açılan listesinde, ardından seçin **etkin**.
    
    - **[Belirteç kimlik doğrulama reddi kod](cdn-rules-engine-reference-features.md#token-auth-denial-code)**: bir istek reddedildiğinde kullanıcıya dönen yanıtının türünü belirler. Burada ayarlanan kurallarını geçersiz kılmasına kümesinde yanıt kodu **reddi özel işleme** belirteç tabanlı kimlik doğrulama sayfasındaki bölümü.
    - **[Belirteç kimlik doğrulama yoksay URL durumda](cdn-rules-engine-reference-features.md#token-auth-ignore-url-case)**: belirteci doğrulamak için kullanılan URL büyük küçük harfe duyarlı olup olmadığını belirler.
    - **[Auth parametre belirteci](cdn-rules-engine-reference-features.md#token-auth-parameter)**: İstenen URL'de görünür belirteci auth sorgu dizesi parametresi olarak yeniden adlandırır. 
        
    ![Belirteç kimlik doğrulama ayarları örnek CDN kurallar altyapısı](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Kaynak kodunda erişerek belirtecinizi özelleştirebilirsiniz [GitHub](https://github.com/VerizonDigital/ectoken).
Kullanılabilir diller şunlardır:
    
- C
- C#
- PHP
- Perl
- Java
- Python    

## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN özellikler ve fiyatlandırma sağlayıcısı

Bilgi için bkz: [CDN'ye genel bakış](cdn-overview.md).
