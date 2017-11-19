---
title: "Azure geçişi karma bağlantılar protokol Kılavuzu | Microsoft Docs"
description: "Azure geçişi karma bağlantılar Protokolü Kılavuzu."
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 9d015678dbd99b8d978c2c8200b36bf51cac8893
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure geçişi karma bağlantılar Protokolü
Azure geçişi Azure Service Bus platformunun anahtar özelliği ayaklar biridir. Yeni *karma bağlantılar* yetenektir geçişinin HTTP ve WebSockets dayalı bir güvenli, açık Protokolü evrimi. Eşit oranda adlı eski yerini *BizTalk Services* özel Protokolü foundation üzerinde oluşturulmuş özellik. Azure App Services karma bağlantılar tümleştirilmesi olarak çalışmaya devam edecek-değil.

Karma bağlantılar sırasında birini veya her ikisini tarafların NAT veya güvenlik duvarı arkasında bulunduğu iki ağa bağlı uygulamalar arasında çift yönlü, ikili akış iletişim sağlar. Bu makalede bağlanan istemciler dinleyicisi ve gönderen rol ve nasıl dinleyicileri yeni bağlantıları kabul etmek için karma bağlantılar geçiş ile istemci tarafı etkileşimler açıklanmaktadır.

## <a name="interaction-model"></a>Etkileşim modeli
Karma bağlantılar geçişi, bir randevu noktası Azure bulutta, hem tarafların bulmak ve kendi ağın açısından bağlanmak sağlayarak iki taraf bağlanır. Bu randevu noktası "karma" Bu ve diğer belgelerin API'leri hem de Azure portalında bağlantısıdır. Karma bağlantılar Hizmeti uç noktası bu makalenin geri kalanı için "hizmet" olarak adlandırılır. Diğer ağ birçok API'ler tarafından kurulan terminolojisi üzerinde etkileşim modeli leans.

İlk gelen bağlantıları işlemek için hazırlık gösterir ve daha sonra bunları kabul geldikçe dinleyici yoktur. Diğer tarafta bir çift yönlü iletişim yolu kurmak için kabul edilmesi için bu bağlantıyı bekleniyor dinleyicisi doğru bağlayan bağlanan bir istemcinin yoktur.
"Bağlan" "Dinleme" ve "Kabul" çoğu yuva API'leri Bul aynı şartları şunlardır.

Herhangi bir geçişli iletişim modelini "dinleyicisi", "istemci" de cümlecik kullanımda hale getirir. ve diğer terminolojisi aşırı da neden olabilir bir hizmet uç noktası doğru giden bağlantı her iki taraf sahiptir. Bu nedenle karma bağlantılar için kullandığımız kesin terminolojisi aşağıdaki gibidir:

Hizmet istemcilere olduğundan bağlantısının her iki tarafında programlar "istemcileri," adı verilir. Bekler ve bağlantıları kabul istemci "dinleyicisi" veya "dinleyicisi rol." olarak kabul edilir Hizmeti üzerinden bir dinleyici doğru yeni bir bağlantı başlatır istemci "gönderen" adlı ya da "gönderen rol."

### <a name="listener-interactions"></a>Dinleyici etkileşimleri
Dinleyici hizmeti ile dört etkileşimleri; yine de sahip istiyor musunuz? Tüm kablo ayrıntıları başvuru bölümünde bu makalenin sonraki bölümlerinde açıklanmıştır.

#### <a name="listen"></a>Dinleme
Bir dinleyicisi hizmetine hazırlık belirtmek için bağlantıları kabul etmeye hazır bir giden WebSocket bağlantısı oluşturur. Bağlantı el sıkışması geçiş ad alanı ve bu ad "Dinleme" hakkı confers bir güvenlik belirteci yapılandırılmış karma bağlantı adını taşır.
WebSocket hizmeti tarafından kabul edildiğinde kayıt tamamlandıktan ve yerleşik web WebSocket "etkinleştirilmesine yönelik tüm sonraki etkileşimler denetim kanalı olarak" Canlı tutulur. Hizmetin en fazla 25 eşzamanlı dinleyicileri karma bir bağlantı sağlar. İki veya daha fazla etkin dinleyiciler varsa, gelen bağlantıları bunları rastgele sırayla dengeli; Orta dağıtım garanti edilmez.

#### <a name="accept"></a>Kabul et
Bir gönderici service üzerinde yeni bir bağlantı oturum açtığında, hizmet seçer ve karma bağlantı etkin dinleyicileri birini bildirir. Bu bildirim için dinleyici dinleyici bağlantı kabul etmek için bağlanmalısınız WebSocket uç noktasının URL'sini içeren bir JSON ileti olarak açık denetim kanalı üzerinden gönderilir.

URL olabilir ve doğrudan ek iş olmadan dinleyicisi tarafından kullanılan gerekir.
Kodlanmış bilgiler yalnızca temelde için gönderen bağlantının kurulan baştan sona olması, ancak en çok 30 saniye bekleyin konusunda istekli mi kadar uzun süre kısa bir süre için geçerlidir. URL, yalnızca bir başarılı bağlantı denemesinde için kullanılabilir. Randevu URL WebSocket bağlantı kuran hemen tüm başka etkinlik bu WebSocket ilk ve son gönderen herhangi bir araya veya hizmeti tarafından tercüme olmadan geçirilen.

#### <a name="renew"></a>Yenile
Dinleyici etkinken dinleyicisi kaydetme ve denetim kanalı korumak için kullanılması gereken güvenlik belirteci dolabilir. Belirteç süre sonu devam eden bağlantılarını etkilemez, ancak denetim kanalı düzeyinde veya sona erme anda hemen sonra hizmeti tarafından kesilmesine neden olmaz. "Yenile" işlemi dinleyicisi denetim kanalı için uzun süreler sürdürülebilir denetim kanalı ile ilişkili belirteci değiştirdiğinizden gönderebileceğiniz bir JSON iletisidir.

#### <a name="ping"></a>Ping
Denetim kanalı aracılar şekilde, uzun bir süre için boşta kalırsa gibi yük Dengeleyiciler ya da NAT TCP bağlantısı kaybolmasına neden olabilir. "Ping" işlemi, çok küçük miktarda veri herkes bağlantı etkin olması için tasarlanmıştır ve ayrıca için dinleyici "canlı" test hizmet ağdaki yönlendiricilerin anımsatır kanalda göndererek önler. Ping başarısız olursa, denetim kanalı kullanılamaz olarak düşünülmelidir ve dinleyiciyi yeniden bağlanmanız.

### <a name="sender-interaction"></a>Gönderen etkileşimi
Gönderen yalnızca tek bir etkileşim hizmeti ile vardır: bağladığı.

#### <a name="connect"></a>Bağlan
"Bağlan" işlemi hizmet üzerinde WebSocket açar karma bağlantı adını ve (isteğe bağlı olarak, ancak gerekli varsayılan olarak) sağlayan bir güvenlik belirteci sorgu dizesinde conferring "Gönderme" izni. Hizmeti daha önce açıklandığı şekilde dinleyicisi etkileşim ve bu WebSocket ile birleştirilmiş bir randevu bağlantı dinleyicisi oluşturur. WebSocket kabul ettikten sonra bu WebSocket üzerinde tüm diğer etkileşimler ile bağlantılı bir dinleyici ' dir.

### <a name="interaction-summary"></a>Etkileşim özeti
Bu etkileşimi model gönderen istemci için bir dinleyici bağlı olduğu ve herhangi bir ek preambles veya hazırlık gerekir "temiz" bir WebSocket ile el sıkışması dışında geldiğini sonucudur. Bu model taşımalarına karma bağlantılar hizmeti kendi WebSocket istemci katmana doğru şekilde oluşturulmuş bir URL sağlayarak yararlanmak neredeyse her mevcut WebSocket istemci uygulamanızı sağlar.

Randevu bağlantı kabul etkileşiminin dinleyicisi edinir WebSocket de temiz ve tüm mevcut WebSocket sunucu uygulamasına kendi framework'ün yerel ağ dinleyicileri "kabul et" işlemleri ve karma bağlantılar uzak "kabul et" işlemleri arasında ayırt bazı en az bir ek soyutlama ile karmalayan.

## <a name="protocol-reference"></a>Protokolü başvurusu

Bu bölümde daha önce açıklanan Protokolü etkileşimleri ayrıntılarını açıklanmaktadır.

Tüm WebSocket bağlantılar bağlantı noktası 443 üzerinde bazı WebSocket framework veya API tarafından yaygın olarak soyutlanır HTTPS 1.1'den yükseltme olarak yapılır. Açıklamayı buraya uygulama belirli bir framework öneren olmadan nötr, tutulur.

### <a name="listener-protocol"></a>Dinleyici Protokolü
Dinleyici Protokolü iki bağlantı hareketleri ve üç ileti işlemlerini oluşur.

#### <a name="listener-control-channel-connection"></a>Dinleyici denetim kanalı bağlantısı
Denetim kanalı WebSocket bağlantı oluşturma konusunda açıldığında:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

`namespace-address` Genellikle formunun karma bağlantı barındıran Azure geçiş ad alanı tam etki alanı adı `{myname}.servicebus.windows.net`.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir.

| Parametre | Gerekli | Açıklama |
| --- | --- | --- |
| `sb-hc-action` |Evet |Dinleyici rolü için parametre olmalıdır **sb hc eylem dinleme =** |
| `{path}` |Evet |Üzerinde bu dinleyici kaydetmek için önceden yapılandırılmış karma bağlantı URL kodlanmış ad alanı yolu. Bu deyim eklenir sabit `$hc/` yol bölümü. |
| `sb-hc-token` |Evet\* |Dinleyici bir geçerli, URL kodlanmış Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **dinleme** doğru. |
| `sb-hc-id` |Hayır |Bu istemci tarafından sağlanan isteğe bağlı kimliği uçtan uca Tanılama izleme sağlar. |

WebSocket bağlantı kaydedilmemiş, karma bağlantı yolu veya eksik veya geçersiz bir belirteç veya başka bir hata nedeniyle başarısız olursa hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması bir hata izleme-Azure destek personeli için iletilen kimliğini içerir:

| Kod | Hata | Açıklama |
| --- | --- | --- |
| 404 |Bulunamadı |Karma bağlantı yolu geçersiz veya temel URL biçimi yanlış. |
| 401 |Yetkilendirilmemiş |Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz olur. |
| 403 |Yasak |Güvenlik belirteci, bu eylem için bu yol için geçerli değil. |
| 500 |İç hata |Bir hizmette sorun oluştu. |

Başlangıçta yukarı, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun WebSocket protokolü hata kodu kullanarak iletişim böylece nedeni ayarlandı sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa Hizmet denetim kanalı bir hata koşulu karşılaşılmadan kapanır değil. Temiz bir kapatma denetlenen istemcidir.

| WS durumu | Açıklama |
| --- | --- |
| 1001 |Karma bağlantı yolu silinmiş veya devre dışı. |
| 1008 |Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |
| 1011 |Bir hizmette sorun oluştu. |

### <a name="accept-handshake"></a>Anlaşma kabul et
"Kabul" bildirim hizmeti tarafından dinleyiciye daha önce oluşturulmuş denetim kanalı üzerinden WebSocket metin çerçevesinde JSON ileti olarak gönderilir. Bu iletiyi yok Yanıtla yoktur.

İleti şu anda aşağıdaki özellikleri tanımlayan "kabul et" adlı bir JSON nesnesi içerir:

* **Adres** – hizmet WebSocket kurmak için bir gelen bağlantı kabul etmek için kullanılacak URL dizesi.
* **Kimliği** – Bu bağlantı için benzersiz tanımlayıcı. Kimliği gönderen istemci tarafından sağlanan, sağlanan gönderen değerdir, aksi takdirde bir sistem tarafından oluşturulan değer.
* **connectHeaders** – sn WebSocket protokolü ve sn WebSocket uzantıları üstbilgileri de içeren gönderenin geçiş uç noktasına tarafından sağlanan tüm HTTP üstbilgileri.

#### <a name="accept-message"></a>İleti kabul et

```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",  
        "connectHeaders" : {                                         
            "Host" : "...",                                                
            "Sec-WebSocket-Protocol" : "...",                              
            "Sec-WebSocket-Extensions" : "..."                             
        }                                                            
     }                                                         
}
```

JSON iletisinde sağlanan adres URL dinleyicisi tarafından kabul etme veya reddetme gönderen yuva için WebSocket kurmak için kullanılır.

#### <a name="accepting-the-socket"></a>Yuva kabul etme
Kabul etmek için dinleyici sağlanan adresi WebSocket bağlantı kurar.

"Kabul" iletisi taşıyorsa bir `Sec-WebSocket-Protocol` üstbilgisi, bu protokolü destekliyorsa, dinleyiciyi yalnızca WebSocket kabul beklenir. Ayrıca, WebSocket belirlenen üstbilgisini ayarlar.

Aynı durum geçerlidir `Sec-WebSocket-Extensions` üstbilgi. Framework uzantı destekliyorsa, gerekli olan sunucu tarafı yanıt üstbilgisi ayarlamalısınız `Sec-WebSocket-Extensions` el sıkışma uzantısı.

URL olarak kullanılması gereken-kabul yuva kurmak için olsa da, aşağıdaki parametreleri içerir:

| Parametre | Gerekli | Açıklama |
| --- | --- | --- |
| `sb-hc-action` |Evet |Bir yuva kabul etmek için parametre olmalıdır`sb-hc-action=accept` |
| `{path}` |Evet |(aşağıdaki paragraf bakın) |
| `sb-hc-id` |Hayır |Önceki açıklamasına bakın **kimliği**. |

`{path}`URL kodlanmış ad alanı, bu dinleyiciyi kaydetmek önceden yapılandırılmış karma bağlantısı yoludur. Bu deyim eklenir sabit `$hc/` yol bölümü. 

`path` İfade genişletilmiş bir sonek ve kayıtlı adı ayıran eğik sonra izleyen bir sorgu dizesi ifadesi. Bu, HTTP üstbilgilerini eklemek mümkün değilse, gönderme bağımsız değişkenleri kabul dinleyicisi geçirmek gönderen istemci sağlar. Dinleyici framework sabit yol bölümü ve yolun kayıtlı adından ayrıştırır ve önüne hiçbir sorgu dizesi bağımsız değişkenler olmadan geri kalanı, büyük olasılıkla hale getirir beklenir `sb-`, bağlantı kabul etmeye karar verme için uygulama tarafından kullanılabilir.

Daha fazla bilgi için aşağıdaki "Gönderen Protokolü" bölümüne bakın.

Varsa bir hata, hizmet gibi yanıtlayabilir:

| Kod | Hata | Açıklama |
| --- | --- | --- |
| 403 |Yasak |URL geçerli değil. |
| 500 |İç hata |Bir hizmette sorun oluştu |

Bağlantı kurulduktan sonra sunucu WebSocket göndereni aşağı ya da aşağıdaki durumundaki kapattığında WebSocket kapatır:

| WS durumu | Açıklama |
| --- | --- |
| 1001 |Gönderen istemci bağlantıyı kapatır. |
| 1001 |Karma bağlantı yolu silinmiş veya devre dışı. |
| 1008 |Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |
| 1011 |Bir hizmette sorun oluştu. |

#### <a name="rejecting-the-socket"></a>Yuva reddetme
Durum kodu ve durum açıklaması reddetme nedeni iletişim gönderene akış yapabileceği "kabul" mesajını inceleyerek sonra yuva reddetme benzer bir el sıkışması gerektirir.

(Yani bir tanımlanan hata durumunda sonlandırmak için tasarlanmıştır) WebSocket el sıkışma Protokolü tasarım seçiminin kullanılmasıdır dinleyicisi istemci uygulamaları WebSocket istemcide yararlanmaya devam edebilirsiniz ve gerekmez fazladan kullandığınızda, HTTP istemci tam.

Yuva reddetmek için istemci "kabul" iletisi URI adresi alır ve aşağıdaki gibi iki sorgu dizesi parametreleri ekler:

| Param | Gerekli | Açıklama |
| --- | --- | --- |
| statusCode |Evet |Sayısal HTTP durum kodu. |
| StatusDescription |Evet |Reddetme İnsan okunabilir açıklaması. |

Sonuçta elde edilen URI sonra WebSocket bağlantı kurmak için kullanılır.

Hiçbir WebSocket kurulduktan sonra doğru tamamlarken, bu el sıkışma bilerek 410, HTTP hata koduyla başarısız olur. Bir sorun yaşanırsa, aşağıdaki kodları hata açıklanmaktadır:

| Kod | Hata | Açıklama |
| --- | --- | --- |
| 403 |Yasak |URL geçerli değil. |
| 500 |İç hata |Bir hizmette sorun oluştu. |

### <a name="listener-token-renewal"></a>Dinleyici belirteci yenileme
Dinleyici belirteci dolmak üzere olduğunda, bu yerleşik denetim kanalı aracılığıyla hizmetine çerçeve mesaj göndererek değiştirebilirsiniz. İleti adlı bir JSON nesnesi içerir `renewToken`, şu anda aşağıdaki özelliği tanımlar:

* **belirteç** – ad alanı veya confers karma bağlantı için geçerli, URL kodlanmış bir hizmet veri yolu paylaşılan erişim belirteci **dinleme** doğru.

#### <a name="renewtoken-message"></a>renewToken iletisi

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Belirteç doğrulama başarısız olursa, erişim reddedildi ve bulut hizmeti bir hata ile denetim kanalı WebSocket kapatır. Aksi durumda yanıt yoktur.

| WS durumu | Açıklama |
| --- | --- |
| 1008 |Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |

## <a name="sender-protocol"></a>Gönderen Protokolü
Gönderen Protokolü dinleyici kurulan şekilde etkili bir şekilde aynıdır.
Uçtan uca WebSocket için maksimum saydamlığı hedeftir. Bağlanmak için adres dinleyicisi ile aynıdır, ancak "eylem" farklıdır ve farklı izin belirteci gerekiyor:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

*Ad alanı adresi* genellikle formunun karma bağlantı barındıran Azure geçiş ad alanı tam etki alanı adı `{myname}.servicebus.windows.net`.

İstek, uygulama tanımlı olanlar da dahil olmak üzere, rasgele ek HTTP üstbilgileri içerebilir. Tüm sağlanan üstbilgileri akış dinleyiciye ve bulunabilir `connectHeader` nesnesinin **kabul** denetim iletisi.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir:

| Param | Gerekli mi? | Açıklama |
| --- | --- | --- |
| `sb-hc-action` |Evet |Gönderen rolü için parametre olmalıdır `action=connect`. |
| `{path}` |Evet |(aşağıdaki paragraf bakın) |
| `sb-hc-token` |Evet\* |Dinleyici bir geçerli, URL kodlanmış Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **Gönder** doğru. |
| `sb-hc-id` |Hayır |Uçtan uca Tanılama izleme sağlar ve bir dinleyici kabul anlaşması sırasında kullanılabilir duruma isteğe bağlı bir kimliği. |

`{path}` , Bu dinleyiciyi kaydetmek önceden yapılandırılmış karma bağlantısı URL kodlanmış ad alanı yolu. `path` İfadesi uzatabilirsiniz sonek ve daha fazla iletişim kurmak için bir sorgu dizesi ifadesi. Karma bağlantı yolu altında kaydedilmişse `hyco`, `path` ifade olabilir `hyco/suffix?param=value&...` burada tanımlanan sorgu dizesi parametreleri tarafından izlenen. Tam bir deyim aşağıdaki gibi olabilir:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

`path` İfade geçirilir aracılığıyla dinleyicisi "kabul" denetimi iletisinde bulunan URI adresi.

WebSocket bağlantı kayıtlı karma bağlantı yolu, eksik veya geçersiz bir belirteç veya başka bir hata nedeniyle başarısız olursa hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması izleme bildirilmesi kimliği Azure destek personeli için bir hata içeriyor:

| Kod | Hata | Açıklama |
| --- | --- | --- |
| 404 |Bulunamadı |Karma bağlantı yolu geçersiz veya temel URL biçimi yanlış. |
| 401 |Yetkilendirilmemiş |Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz olur. |
| 403 |Yasak |Güvenlik belirteci bu yol için ve bu eylem için geçerli değil. |
| 500 |İç hata |Bir hizmette sorun oluştu. |

Yukarı, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun WebSocket protokolü hata kodu kullanarak iletişim böylece nedeni başlangıçta ayarlandıktan sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa

| WS durumu | Açıklama |
| --- | --- |
| 1000 |Dinleyici yuva kapatın. |
| 1001 |Karma bağlantı yolu silinmiş veya devre dışı. |
| 1008 |Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |
| 1011 |Bir hizmette sorun oluştu. |

## <a name="next-steps"></a>Sonraki adımlar
* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)

