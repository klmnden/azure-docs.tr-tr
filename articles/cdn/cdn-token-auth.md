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
ms.openlocfilehash: 42b182c314795b1ebf69639ec7ac5583208dc7c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Azure CDN varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Genel Bakış

Belirteç kimlik doğrulama varlıklar yetkisiz istemcilerine hizmet veren Azure CDN önlemek izin veren bir mekanizmadır.  Bu genellikle, içeriğin "hotlinking" önlemek için burada ileti panosu genellikle, farklı bir Web varlıklarınızı izniniz olmadan kullanan gerçekleştirilir.  Bu içerik teslim maliyetleriniz üzerinde etkisi olabilir. Bu özelliği CDN etkinleştirerek, istekleri CDN kenarı içerik teslim etmeden POP tarafından doğrulanır. 

## <a name="how-it-works"></a>Nasıl çalışır?

Belirteç kimlik doğrulama istekleri istek sahibinin kodlanmış bilgilerini içeren bir belirteç değeri içeren isteklerine gerektirerek güvenilen bir site tarafından oluşturulan doğrular. Kodlanmış bilgi gereksinimleri karşıladığında içeriği yalnızca istemciye sunulacak, aksi takdirde isteği reddedilir. Aşağıdaki bir veya birden çok parametre kullanarak gereksinimini ayarlayabilirsiniz.

- Ülke: izin ver veya belirtilen ülkelerden kaynaklanan istekleri reddedin.  [Geçerli ülke kodlarının listesi.](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL: yalnızca belirtilen varlık veya yolu istemek izin verir.  
- Ana bilgisayarı: izin ver veya istek üstbilgisinde belirtilen ana kullanan isteklerinin reddedin.
- Başvuran: izin ver veya istemek için belirtilen başvuran reddedin.
- IP adresi: yalnızca belirli bir IP adresi veya IP alt ağı kaynaklanan istekleri izin verin.
- Protokol: izin verin veya içeriği istemek için kullanılan protokolünü temel isteklerini engellemek.
- Süre sonu: bağlantı yalnızca sınırlı bir süre için geçerli olmaya devam ettiğinden emin olmak için bir tarih ve saat dönemi atayabilir.

Her bir parametreyi ayrıntılı yapılandırma örneği bakın.

## <a name="reference-architecture"></a>Başvuru mimarisi

Web uygulamanızın üzerinde çalışmak için CDN belirteci kimlik doğrulamasını ayarlama referans mimarisi aşağıya bakın.

![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN uç noktasında belirteci Doğrulama mantığı
    
Bu grafik, nasıl Azure CDN belirteci kimlik doğrulaması CDN uç yapılandırıldığında, istemci isteği doğrular açıklar.

![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Belirteç kimlik doğrulamayı ayarlama

1. Gelen [Azure portal](https://portal.azure.com), CDN profilinize gidin ve ardından **Yönet** ek Portalı'nı başlatmak için düğmesi.

    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Üzerine gelerek **HTTP büyük**ve ardından **belirteci Auth** çıkma içinde. Şifreleme anahtarı ve bu sekmedeki şifreleme parametreleri ayarlayacaksınız.

    1. Benzersiz bir şifreleme anahtarı için girin **birincil anahtar**.  İçin başka bir girin **yedekleme anahtarı**

        ![CDN belirteci auth kurulum anahtarı](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Şifreleme parametreleri şifreleme aracıyla ayarlayın (izin ver veya Reddet istekleri göre süre sonu, ülke, başvuran, protokol, istemci IP. "Herhangi bir birleşimini kullanabilirsiniz.)

        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - EC-sona: bir belirteç sona erme süresi belirli bir süre sonra atar. Sona erme zamanı reddedilir sonra gönderilen istek sayısı. Bu parametre UNIX zaman damgası kullanır (standart dönemi: 1/1/1970'ten beri geçen saniye göre 00:00:00 GMT. Çevrimiçi araçları Standart Saati ve UNIX saat arasında dönüştürme sağlamak için kullanabilirsiniz.)  Örneğin, 31/12/2016 süresinin belirtecin ayarlamak istiyorsanız, 12:00:00 GMT, UNIX saat: 1483185600 aşağıdaki gibi kullanın:
    
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - EC url izin: belirli bir varlık veya yolu belirteçleri uyarlamak olanak tanır. URL'si Başlat belirli bir göreli yol isteklerine erişimi sınırlandırır. Her yol virgül ile ayırarak birden fazla yol girebilirsiniz. URL'leri büyük/küçük harfe duyarlıdır. Gereksinim bağlı olarak farklı erişim düzeyini sağlamak için farklı değeri ayarlayabilirsiniz. Aşağıda birkaç senaryo vardır:
        
            Bir URL varsa: http://www.mydomain.com/pictures/city/strasbourg.png. Giriş değeri bkz: "" ve buna göre erişim düzeyi

            1. Giriş değeri "/": tüm istekleri izin verilir
            2. Giriş değeri "/ resim": aşağıdaki isteklerini izin olacaktır
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Giriş değeri "/ resimler /": /pictures/ izin verilmesi yalnızca istekleri
            4. Giriş değeri "/ pictures/city/strasbourg.png": yalnızca bu varlık için istek sağlar
    
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - AB Ülke izin: yalnızca bir veya daha fazla belirtilen ülkelerden kaynaklanan isteklere izin verir. Diğer tüm ülkelerden kaynaklı istekler reddedilir. Her ülke kodu virgül ile ayırarak ve ülke kodu parametrelerini ayarlamak için kullanın. Örneğin, Amerika Birleşik Devletleri ve Fransa erişimine izin vermek istiyorsanız, BİZE FR sütununda girin.  
        
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - AB Ülke reddetme: bir veya daha fazla belirtilen ülkelerden kaynaklanan istekleri reddeder. Diğer tüm ülkelerden kökenli isteklerine izin verilir. Her ülke kodu virgül ile ayırarak ve ülke kodu parametrelerini ayarlamak için kullanın. Örneğin, Amerika Birleşik Devletleri ve Fransa erişimini engellemek istiyorsanız, BİZE sütun FR girin.
    
        - EC ref izin: yalnızca belirtilen başvuran isteklere izin verir. Bir başvuran istenen kaynak bağlı web sayfasının URL'sini tanımlar. Başvuran parametre değeri Protokolü içermemelidir. Bir ana bilgisayar adı ve/veya bu ana bilgisayar üzerinde belirli bir yol girebilirsiniz. Ayrıca, her biri virgül ile ayırarak tek bir parametre içinde birden çok başvuran ekleyebilirsiniz. Bir başvuran değer belirttiniz, ancak bazı tarayıcı yapılandırması nedeniyle isteği başvuran bilgi gönderilmez, bu istekleri varsayılan olarak reddedilir. Başvuran bilgileri eksik olan bu isteklere izin vermek için "Eksik" veya parametresinde boş bir değer atayabilirsiniz. Aynı zamanda "*. consoto.com" consoto.com tüm alt etki alanlarına izin vermek için.  Örneğin, www.consoto.com, consoto2.com ve boş veya eksik reffers ile erquests altındaki tüm alt etki gelen istekleri erişime izin vermek istiyorsanız, aşağıda değeri giriş:
        
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - EC ref reddetme: Belirtilen başvuran istekleri reddeder. Ayrıntılar ve "AB-ref-izin ver" parametresi örnekte bakın.
         
        - EC proto izin: yalnızca belirtilen protokol gelen isteklere izin verir. Örneğin, http veya https.
        
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - EC proto reddetme: Belirtilen protokolünden istekleri reddeder. Örneğin, http veya https.
    
        - EC clientip: Belirtilen sahibinin IP adresine erişimi kısıtlar. IPv4 ve IPv6 desteklenir. Tek istek IP adresi veya IP alt ağı belirtebilirsiniz.
            
        ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Belirtecinizi tanımı aracıyla test edebilirsiniz.

    4. İstek reddedildiğinde kullanıcıya döndürülecek yanıtının türünü özelleştirebilirsiniz. Varsayılan olarak 403 kullanırız.

3. Şimdi **kurallar altyapısı** altında sekmesinde **HTTP büyük**. Bu sekme özelliği geçerli, belirteç kimlik doğrulama özelliğini etkinleştirmek ve etkinleştirmek için yollarını tanımlamak için kullanacağınız ek belirteci kimlik doğrulaması ile ilgili özellikler.

    - Varlık veya belirteci kimlik doğrulaması uygulamak istediğiniz yolu tanımlamak için "IF" sütunu kullanın. 
    - "Belirteç Auth" belirteci kimlik doğrulamasını etkinleştirmek için özellik aşağı açılır listeden eklemek için tıklatın.
        
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. İçinde **kurallar altyapısı** sekmesinde, etkinleştirebilirsiniz birkaç ek özellikler vardır.
    
    - Belirteç kimlik doğrulama reddi kod: bir istek reddedildiğinde kullanıcıya döndürülecek yanıt türünü belirler. Burada ayarlanan kuralları belirteci auth sekmesinde reddi kod ayarını geçersiz kılar.
    - Belirteç kimlik doğrulama yoksay: belirteci doğrulamak için kullanılan URL büyük küçük harfe duyarlı olup olmayacağını belirler.
    - Belirteç kimlik doğrulama parametresi: İstenen URL'de gösteren belirteci auth sorgu dizesi parametresi yeniden adlandırın. 
        
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Belirteç tabanlı kimlik doğrulama özellikleri için belirteci oluşturan bir uygulama olan belirtecinizi özelleştirebilirsiniz. Kaynak kodu erişilebilir burada içinde [GitHub](https://github.com/VerizonDigital/ectoken).
Kullanılabilir diller şunlardır:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN özellikler ve fiyatlandırma sağlayıcısı

Bkz: [CDN'ye genel bakış](cdn-overview.md) konu.
