---
title: "Azure CDN varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme | Microsoft Docs"
description: "Azure CDN varlıklarınızı erişimin güvenliğini sağlamak için belirteci kimlik doğrulaması kullanarak."
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
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: b1bcaf14e9805afa82c765092b62201f58c7cbc4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Azure içerik teslim ağı varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Belirteç kimlik doğrulama varlıklar yetkisiz istemcilerine hizmet veren Azure içerik teslim ağı (CDN) önlemek izin veren bir mekanizmadır. Belirteç kimlik doğrulama, genellikle ileti panosu genellikle, farklı bir Web varlıklarınızı izniniz olmadan kullanan içeriğinin "hotlinking" önlemek için yapılır. Hotlinking içerik teslim maliyetleriniz üzerinde bir etkisi olabilir. Bu özelliği CDN etkinleştirerek, CDN içerik sunan önce istekleri POP CDN uç tarafından doğrulanır. 

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

Aşağıdaki iş akışı diyagramı belirteci kimlik doğrulaması CDN web uygulamanız ile çalışmak için nasıl kullandığını açıklar.

![CDN belirteç kimlik doğrulama iş akışı](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN uç noktasında belirteci Doğrulama mantığı
    
Aşağıdaki akış çizelgesi, nasıl Azure CDN belirteci kimlik doğrulaması CDN uç yapılandırıldığında, istemci isteği doğrular açıklar.

![CDN belirteci Doğrulama mantığı](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Belirteç kimlik doğrulamayı ayarlama

1. Gelen [Azure portal](https://portal.azure.com), CDN profilinize gidin ve ardından **Yönet** ek Portalı'nı başlatmak için.

    ![CDN profili Yönet düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Üzerine gelerek **HTTP büyük**ve ardından **belirteci Auth** çıkma içinde. Şifreleme anahtarı ve bu sekmedeki şifreleme parametreleri ayarlayın.

    1. Benzersiz bir şifreleme anahtarı için girin **birincil anahtar** ve için başka bir girin **yedekleme anahtar**.

        ![CDN belirteci auth kurulum anahtarı](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Şifreleme parametreleri şifrele aracıyla ayarlayın. Şifrele aracıyla izin verebilir veya sona erme zamanı, ülke, başvuran, protokol ve (herhangi bir birleşimini)'deki istemci IP göre istekleri reddetmesini.

        ![CDN şifrelemek aracı](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       - ec_expire: geçmesi belirtecinin süresi dolmadan bir belirteç için süre sonu zamanı atar. Sona erme zamanı engellenir sonra gönderilen istek sayısı. Bu parametre standart dönem saniyesi sayısına dayalı bir UNIX zaman damgası kullanan `1/1/1970 00:00:00 GMT`. (Çevrimiçi araçlarını UNIX zaman Standart Saati arasında dönüştürmek için kullanabileceğiniz.) Örneğin, belirtecin anda süresi dolacak şekilde ayarlamak istiyorsanız `12/31/2016 12:00:00 GMT`, UNIX zaman damgası değeri kullanmak `1483185600`aşağıdaki gibi. 
    
         ![CDN ec_expire örneği](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - ec_url_allow: belirli bir varlık veya yolu belirteçleri uyarlamak olanak tanır. URL'si Başlat belirli bir göreli yol isteklerine erişimi sınırlandırır. URL'leri büyük/küçük harfe duyarlıdır. Her yol virgül ile ayırarak birden fazla yol girin. Gereksinimlerinize bağlı olarak, farklı erişim düzeyini sağlamak için farklı değerlerini ayarlayabilirsiniz. 
        
          Örneğin, URL `http://www.mydomain.com/pictures/city/strasbourg.png`, giriş değeri ve erişim düzeyi aşağıdaki gibi ayarlayın:

           - Giriş değeri `"/"`: tüm isteklerine izin verilir
           - Giriş değeri `"/pictures`. Aşağıdaki isteklerine izin verilir:
              - `http://www.mydomain.com/pictures.png`
              - `http://www.mydomain.com/pictures/city/strasbourg.png`
              - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
            - Giriş değeri `/pictures/`: yalnızca için istekleri `/pictures/` izin verilir. Örneğin, `http://www.mydomain.com/pictures/city/strasbourg.png`.
            - Giriş değeri `/pictures/city/strasbourg.png`: yalnızca istekleri belirli bu varlık için izin verilir.
    
          ![CDN ec_url_allow örneği](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
       - ec_country_allow: yalnızca bir veya daha fazla belirtilen ülkelerden kaynaklanan isteklere izin verir. Diğer tüm ülkelerden kaynaklanan istekleri reddedilir. Ülke kodlarına kullanın ve her birini virgül ile ayırın. Yalnızca Amerika Birleşik Devletleri ve Fransa erişimine izin vermek istiyorsanız, örneğin, BİZE FR alanında aşağıdaki gibi girin.  
        
           ![CDN ec_country_allow örneği](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - ec_country_deny: bir veya daha fazla belirtilen ülkelerden kaynaklanan istekleri reddeder. Diğer tüm ülkelerden kaynaklanan isteklerine izin verilir. Ülke kodlarına kullanın ve her birini virgül ile ayırın. Örneğin, Amerika Birleşik Devletleri ve Fransa erişimini engellemek istiyorsanız, BİZE FR alanına girin.
    
       - ec_ref_allow: yalnızca belirtilen başvuran isteklere izin verir. Bir başvuran istenen kaynak bağlı web sayfasının URL'sini tanımlar. Protokol başvuran parametre değeri içermez. Giriş aşağıdaki türleri için parametre değeri verilir:
           - Bir ana bilgisayar adı veya bir ana bilgisayar adı ve yolu.
           - Birden çok başvuran. Birden çok başvuran eklemek için her başvuran virgül ile ayırın. Başvuran değeri belirtin, ancak tarayıcı yapılandırması nedeniyle isteği başvuran bilgi gönderilmez, bu istekleri varsayılan olarak reddedilir. 
           - Başvuran bilgileri eksik olan istek sayısı. Bu tür istekleri izin vermek için metin "eksik" girin veya boş bir değer girin. 
           - Alt etki alanları. Alt etki alanları izin vermek için bir yıldız işareti (*) girin. Örneğin, tüm alt etki alanlarına izin vermek için `consoto.com` girin `*.consoto.com`. 
           
          Aşağıdaki örnek, gelen istekleri için erişime izin vermek için giriş gösterir `www.consoto.com`, altındaki tüm alt etki `consoto2.com`ve boş veya eksik başvuran istekleri.
        
          ![CDN ec_ref_allow örneği](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - ec_ref_deny: Belirtilen başvuran istekleri reddeder. Uygulama ec_ref_allow parametresi ile aynıdır.
         
       - ec_proto_allow: yalnızca belirtilen protokol gelen isteklere izin verir. Örneğin, HTTP veya HTTPS.
        
         ![CDN ec_proto_allow örneği](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
       - ec_proto_deny: Belirtilen protokolünden istekleri reddeder. Örneğin, HTTP veya HTTPS.
    
       - ec_clientip: Belirtilen sahibinin IP adresine erişimi kısıtlar. IPv4 ve IPv6 desteklenir. Bir tek istek IP adresi veya bir IP alt ağı belirtebilirsiniz.
            
         ![CDN ec_clientip örneği](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. İsteğe bağlı olarak açıklama aracıyla belirtecinizi sınayın.

    4. İsteğe bağlı olarak, bir istek reddedildiğinde, döndürülen yanıt türünü özelleştirin. Varsayılan olarak, yanıt kodu 403 kullanılır.

3. Altında **HTTP büyük**, tıklatın **kurallar altyapısı**. Kurallar altyapısı özelliği geçerli, belirteç kimlik doğrulama özelliğini etkinleştirmek ve ek belirteç kimlik doğrulamayla ilgili özellikler etkinleştirmek için yollarını tanımlamak için kullanın.

    1. Kullanım **IF** belirteci kimlik doğrulaması uygulamak istediğiniz varlık veya yolunu tanımlamak için sütun. 
    2. Belirteç kimlik doğrulamasını etkinleştirmek için seçin **belirteci Auth** gelen **özellikleri** açılır.
        
    ![CDN kurallar altyapısı belirteci kimlik doğrulamasını etkinleştir örnek](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Kuralları altyapısında etkinleştirebilirsiniz birkaç ek özellikler vardır:
    
    - **Belirteç kimlik doğrulama reddi kod**: bir istek reddedildiğinde kullanıcıya dönen yanıtının türünü belirler. Burada ayarlanan kurallarını geçersiz kılmasına reddi kod ayarında **belirteci Auth** sekmesi.
    - **Belirteç kimlik doğrulama yoksay URL durumda**: belirteci doğrulamak için kullanılan URL büyük küçük harfe duyarlı olup olmadığını belirler.
    - **Auth parametre belirteci**: İstenen URL'de görünür belirteci auth sorgu dizesi parametresi olarak yeniden adlandırır. 
        
    ![Belirteç kimlik doğrulama ayarları örnek CDN kurallar altyapısı](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Belirteç tabanlı kimlik doğrulama özellikleri için belirteci oluşturan bir uygulamadır belirtecinizi özelleştirebilirsiniz. Kaynak kodu erişilebilir [GitHub](https://github.com/VerizonDigital/ectoken).
Kullanılabilir diller şunlardır:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN özellikler ve fiyatlandırma sağlayıcısı

Bilgi için bkz: [CDN'ye genel bakış](cdn-overview.md).
