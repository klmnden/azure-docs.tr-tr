---
title: Azure CDN varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme | Microsoft Docs
description: Azure CDN varlıklara erişimi güvenli hale getirmek için belirteci kimlik doğrulaması kullanmayı öğrenin.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/17/2017
ms.author: mezha
ms.openlocfilehash: aaec713a7680aeda8317f5af41b9b99bcbdca4b7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Azure CDN varlıklar belirteci kimlik doğrulaması ile güvenli hale getirme

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Belirteç kimlik doğrulama varlıklar yetkisiz istemcilerine hizmet veren Azure içerik teslim ağı (CDN) önlemek izin veren bir mekanizmadır. Belirteç kimlik doğrulama önlemek için genellikle yapıldığı *hotlinking* içerik, ileti panosu gibi farklı bir Web sitesi varlıklarınızı izni olmaksızın kullanır. Hotlinking içerik teslim maliyetleriniz üzerinde bir etkisi olabilir. CDN içerik sunan önce CDN belirteci kimlik doğrulamasını etkinleştirerek, CDN uç sunucusu tarafından istekler doğrulanır. 

## <a name="how-it-works"></a>Nasıl çalışır?

Belirteç kimlik doğrulama isteklerini ayrı tutma istek sahibi hakkında bilgi kodlanmış bir belirteç değeri içeren gerektirerek istekleri güvenilen bir site tarafından oluşturulan doğrular. Yalnızca kodlanmış bilgi gereksinimleri karşılıyorsa içerik için bir istek sunulan; Aksi takdirde, istek reddedilir. Bir veya daha fazla aşağıdaki parametreleri kullanarak gereksinimleri ayarlayabilirsiniz:

- Ülke: İzin ver veya tarafından belirtilen ülkelerde kaynaklanan istekleri reddetmesini kendi [ülke kodu](https://msdn.microsoft.com/library/mt761717.aspx).
- URL: Belirtilen varlık veya yol eşleşen istekleri izin verin.
- Ana bilgisayarı: İzin ver veya istek üstbilgisinde belirtilen konakların kullanan isteklerini reddedin.
- Başvuran: İzin ver veya belirtilen başvuran isteği reddedin.
- IP adresi: belirli bir IP adresi veya IP alt ağı kaynaklanan isteklere izin vermek.
- Protokol: İzin ver veya içeriği istemek için kullanılan protokolünü temel istekleri reddedin.
- Süre sonu: bağlantı yalnızca sınırlı bir süre boyunca geçerli olmaya devam ettiğinden emin olmak için bir tarih ve saat dönemi atayabilir.

Daha fazla bilgi için her parametre için ayrıntılı yapılandırma örneklerini görmek [belirteç kimlik doğrulamayı ayarlama](#setting-up-token-authentication).

>[!IMPORTANT] 
> Bu hesaptaki herhangi bir yol için belirteci yetkilendirme etkinleştirilirse, standart Önbellek modu sorgu dizesini önbelleğe alma işlemi için kullanılan tek modudur. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string-premium.md).

## <a name="reference-architecture"></a>Başvuru mimarisi

Aşağıdaki iş akışı diyagramı belirteci kimlik doğrulaması CDN web uygulamanızın üzerinde çalışmak için nasıl kullandığını açıklar.

![CDN belirteç kimlik doğrulama iş akışı](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN uç noktasında belirteci Doğrulama mantığı
    
Aşağıdaki akış çizelgesi, nasıl Azure CDN belirteci kimlik doğrulaması CDN uç yapılandırıldığında, bir istemci isteği doğrular açıklar.

![CDN belirteci Doğrulama mantığı](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Belirteç kimlik doğrulamayı ayarlama

1. Gelen [Azure portal](https://portal.azure.com), CDN profilinize gidin ve ardından **Yönet** ek Portalı'nı başlatmak için.

    ![CDN profili Yönet düğmesi](./media/cdn-token-auth/cdn-manage-btn.png)

2. Üzerine gelerek **HTTP büyük**seçeneğini belirleyip **belirteci Auth** çıkma içinde. Ardından şifreleme anahtarı ve şifreleme parametreleri aşağıdaki gibi ayarlayabilirsiniz:

    1. Bir veya daha fazla şifreleme anahtarları oluşturun. Bir şifreleme anahtarı duyarlıdır ve herhangi bir bileşimini alfasayısal karakterler içerebilir. Karakterler, boşluklar dahil, başka türlerde izin verilmiyor. Uzunluk en fazla 250 karakterdir. Şifreleme anahtarlarınızı rastgele, kullanarak bunları oluşturmanız önerilir emin olmak için [OpenSSL aracı](https://www.openssl.org/). 

       OpenSSL aracın sözdizimi aşağıdaki gibidir:

       ```rand -hex <key length>```

       Örneğin:

       ```OpenSSL> rand -hex 32``` 

       Kesinti süresini önlemek için hem birincil hem de bir yedek anahtarı oluşturun. Birincil anahtarınızı güncelleştirildiğinde bir yedek anahtarı içeriğinizi kesintisiz erişim sağlar.
    
    2. Bir benzersiz şifreleme anahtarı girin **birincil anahtar** kutusunda ve isteğe bağlı olarak bir yedek anahtarı girin **yedek anahtarı** kutusu.

    3. Her anahtarından için en düşük şifreleme sürümünü seçin, **Minimum şifreleme sürümünü** listeleyin ve ardından **güncelleştirme**:
       - **V2**: anahtarı sürüm 2.0 ve 3.0 belirteçleri oluşturmak için kullanılabilir gösterir. Yalnızca sürüm 3.0 anahtarına bir eski sürüm 2.0 şifreleme anahtarı geçiş, bu seçeneği kullanın.
       - **V3**: (önerilen) anahtarı yalnızca sürüm 3.0 belirteçleri oluşturmak için kullanılabilir olduğunu gösterir.

    ![CDN belirteci auth kurulum anahtarı](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    4. Şifreleme parametreleri ayarlama ve bir belirteç oluşturmak için şifrele aracını kullanın. Şifrele aracıyla izin verebilir veya sona erme zamanı, ülke, başvuran, protokol ve (herhangi bir birleşimini)'deki istemci IP göre istekleri reddetmesini. Sınır sayısı ve bir belirteç oluşturmak için birleştirilebilir parametreler birleşimi olsa da, bir belirteç toplam uzunluğu 512 karakterle sınırlıdır. 

       ![CDN şifrelemek aracı](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       Bir veya daha fazla aşağıdaki şifreleme parametreleri için değerler girin **şifrelemek aracı** bölümü: 

       > [!div class="mx-tdCol2BreakAll"] 
       > <table>
       > <tr>
       >   <th>Parametre adı</th> 
       >   <th>Açıklama</th>
       > </tr>
       > <tr>
       >    <td><b>ec_expire</b></td>
       >    <td>Bir süre sonra belirtecinin süresi dolmadan bir belirteç için atar. Sona erme zamanı engellenir sonra gönderilen istek sayısı. Bu parametre standart UNIX dönem saniyesi sayısına dayalı bir UNIX zaman damgası kullanan `1/1/1970 00:00:00 GMT`. (Çevrimiçi araçlarını UNIX zaman Standart Saati arasında dönüştürmek için kullanabileceğiniz.)> 
       >    Örneğin, belirtecin anda süresi dolacak şekilde istiyorsanız `12/31/2016 12:00:00 GMT`, UNIX zaman damgası değeri girin `1483185600`. 
       > </tr>
       > <tr>
       >    <td><b>ec_url_allow</b></td> 
       >    <td>Belirli bir varlık veya yolu belirteçleri uyarlamak olanak tanır. URL'si Başlat belirli bir göreli yol isteklerine erişimi sınırlandırır. URL'leri büyük/küçük harfe duyarlıdır. Her yol virgül ile ayırarak birden fazla yol giriş; boşluk eklemeyin. Gereksinimlerinize bağlı olarak, farklı erişim düzeyini sağlamak için farklı değerlerini ayarlayabilirsiniz.> 
       >    Örneğin, URL `http://www.mydomain.com/pictures/city/strasbourg.png`, bu istekleri için aşağıdaki giriş değerlerine izin verilir: 
       >    <ul>
       >       <li>Giriş değeri `/`: tüm isteklerine izin verilir.</li>
       >       <li>Giriş değeri `/pictures`, aşağıdaki isteklerine izin verilir: <ul>
       >          <li>`http://www.mydomain.com/pictures.png`</li>
       >          <li>`http://www.mydomain.com/pictures/city/strasbourg.png`</li>
       >          <li>`http://www.mydomain.com/picturesnew/city/strasbourgh.png`</li>
       >       </ul></li>
       >       <li>Giriş değeri `/pictures/`: yalnızca istekleri içeren `/pictures/` yolu izin verilir. Örneğin, `http://www.mydomain.com/pictures/city/strasbourg.png`.</li>
       >       <li>Giriş değeri `/pictures/city/strasbourg.png`: yalnızca bu belirli yol ve varlık izin ister.</li>
       >    </ul>
       > </tr>
       > <tr>
       >    <td><b>ec_country_allow</b></td> 
       >    <td>Yalnızca bir veya daha fazla belirtilen ülkelerden kaynaklanan isteklere izin verir. Diğer tüm ülkelerden kaynaklanan istekleri reddedilir. İki harfli kullanmak [ISO 3166 ülke kodu](https://msdn.microsoft.com/library/mt761717.aspx) her ülke ve her birini virgül ile ayırın; boşluk eklemeyin. Örneğin, yalnızca Amerika Birleşik Devletleri ve Fransa erişime izin verecek şekilde istiyorsanız girin `US,FR`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_country_deny</b></td> 
       >    <td>Bir veya daha fazla belirtilen ülkelerden kaynaklanan isteklerini reddeder. Diğer tüm ülkelerden kaynaklanan isteklerine izin verilir. Aynı uygulamasıdır <b>ec_country_allow</b> parametresi. Bir ülke kodu hem de varsa, <b>ec_country_allow</b> ve <b>ec_country_deny</b> parametreleri <b>ec_country_allow</b> parametresi önceliklidir.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_ref_allow</b></td>
       >    <td>Yalnızca belirtilen başvuran isteklere izin verir. Bir başvuran istenen kaynak bağlı web sayfasının URL'sini tanımlar. Protokol parametre değeri içermez.>    
       >    Giriş aşağıdaki türleri kullanılabilir:
       >    <ul>
       >       <li>Bir ana bilgisayar adı veya bir ana bilgisayar adı ve yolu.</li>
       >       <li>Birden çok başvuran. Birden çok başvuran eklemek için her başvuran virgülle ayırın; bir alanı eklemeyin. Başvuran değeri belirtin, ancak tarayıcı yapılandırması nedeniyle isteği başvuran bilgi gönderilmez, istek varsayılan olarak reddedilir.</li> 
       >       <li>Eksik veya boş başvuran bilgilerle istek sayısı. Varsayılan olarak, <b>ec_ref_allow</b> parametresi bu tür istekleri engeller. Bu isteklere izin vermek için "eksik" ya da bir metin girin veya (Sondaki virgül kullanarak) boş bir değer girin.</li> 
       >       <li>Alt etki alanları. Alt etki alanları izin vermek için bir yıldız işareti girin (\*). Örneğin, tüm alt etki alanlarına izin vermek için `contoso.com`, girin `*.contoso.com`.</li>
       >    </ul>     
       >    Örneğin, gelen istekleri için erişime izin verecek şekilde `www.contoso.com`, altındaki tüm alt etki `contoso2.com`, boş veya eksik başvuran istekleriyle girin `www.contoso.com,*.contoso.com,missing`.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_ref_deny</b></td>
       >    <td>Belirtilen başvuran istekleri reddeder. Aynı uygulamasıdır <b>ec_ref_allow</b> parametresi. Bir başvuran ikisi de varsa, <b>ec_ref_allow</b> ve <b>ec_ref_deny</b> parametreleri <b>ec_ref_allow</b> parametresi önceliklidir.</td>
       > </tr>
       > <tr> 
       >    <td><b>ec_proto_allow</b></td> 
       >    <td>Yalnızca belirtilen protokol gelen isteklere izin verir. Geçerli değerler `http`, `https`, veya `http,https`.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_proto_deny</b></td>
       >    <td>Belirtilen protokolünden isteklerini reddeder. Aynı uygulamasıdır <b>ec_proto_allow</b> parametresi. Bir protokol hem de varsa, <b>ec_proto_allow</b> ve <b>ec_proto_deny</b> parametreleri <b>ec_proto_allow</b> parametresi önceliklidir.</td>
       > </tr>
       > <tr>
       >    <td><b>ec_clientip</b></td>
       >    <td>Belirtilen sahibinin IP adresine olan erişimi sınırlandırır. IPv4 ve IPv6 desteklenir. Bir tek istek IP adresi ya da belirli bir alt ağ ile ilişkili IP adreslerini belirtebilirsiniz. Örneğin, `11.22.33.0/22` 11.22.32.1 için 11.22.35.254 IP adreslerinden isteklere izin verir.</td>
       > </tr>
       > </table>

    5. Şifreleme parametre değerlerini girme bitirdikten sonra (hem birincil hem de bir yedek anahtarı oluşturduysanız) şifrelemek için bir anahtar seçin **için anahtarı şifrelemek** listesi.
    
    6. Bir şifreleme sürümden seçin **şifreleme sürümünü** listesi: **V2** sürüm 2 için veya **V3** için sürüm 3 (önerilen). 

    7. Seçin **şifrele** belirteci üretmek için.

    Belirteç oluşturulduktan sonra görüntülenir **oluşturulan belirteç** kutusu. Belirteç kullanmak için URL yolu dosyasında sonuna bir sorgu dizesi olarak ekleyin. Örneğin, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
    8. İsteğe bağlı olarak, belirtecin parametreleri görüntüleyebilmesi için belirtecinizi şifresini çözme aracıyla test edin. Belirteç değeri yapıştırın **belirteç şifre çözme için** kutusu. Kullanmak için şifreleme anahtarını seçin **anahtarın şifresini çözüp** listeleyin ve ardından **şifresini**.

    Belirteç şifresi sonra parametrelerini görüntülenen **özgün parametreleri** kutusu.

    9. İsteğe bağlı olarak, bir istek reddedildiğinde, döndürülen yanıt kodu türünü özelleştirin. Seçin **etkin**, yanıt kodu ile seçip **yanıt kodu** listesi. **Üstbilgi adı** ayarlandığında otomatik olarak **konumu**. Seçin **kaydetmek** yeni yanıt kodu uygulamak için. Belirli yanıt kodları, ayrıca, hata sayfasının URL'sini girmelisiniz **üstbilgi değeri** kutusu. **403** yanıt kodu (Yasak), varsayılan olarak seçilidir. 

3. Altında **HTTP büyük**seçin **kurallar altyapısı**. Kurallar altyapısı özelliği geçerli, belirteç kimlik doğrulama özelliğini etkinleştirmek ve ek belirteç kimlik doğrulamayla ilgili özellikler etkinleştirmek için yollarını tanımlamak için kullanın. Daha fazla bilgi için bkz: [kurallar altyapısı başvuru](cdn-rules-engine-reference.md).

    1. Mevcut bir kuralı seçin veya belirteci kimlik doğrulaması uygulamak istediğiniz varlık veya yolunu tanımlamak için yeni bir kural oluşturun. 
    2. Bir kural belirteci kimlik doğrulamasını etkinleştirmek için seçin **[belirteci Auth](cdn-rules-engine-reference-features.md#token-auth)** gelen **özellikleri** listeleyin ve ardından **etkin**. Seçin **güncelleştirme** bir kural güncelleştiriyorsanız veya **Ekle** bir kural oluşturuluyorsa.
        
    ![CDN kurallar altyapısı belirteci kimlik doğrulamasını etkinleştir örnek](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Kuralları altyapısında ek belirteç kimlik doğrulamayla ilgili özellikler de etkinleştirebilirsiniz. Aşağıdaki özelliklerden herhangi birini etkinleştirmek için dosyayı seçin **özellikleri** listeleyin ve ardından **etkin**.
    
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

Özellikleri hakkında daha fazla bilgi için bkz: [Azure CDN ürün özellikleri](cdn-features.md). Fiyatlandırma hakkında daha fazla bilgi için bkz: [içerik teslim ağı fiyatlandırma](https://azure.microsoft.com/pricing/details/cdn/).
