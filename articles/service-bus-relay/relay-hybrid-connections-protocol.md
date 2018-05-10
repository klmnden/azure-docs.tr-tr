---
title: Azure geçişi karma bağlantılar protokol Kılavuzu | Microsoft Docs
description: Azure geçişi karma bağlantılar Protokolü Kılavuzu.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: ''
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 306a21add76261dce99c954a2ba373e4b5047a75
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure geçişi karma bağlantılar Protokolü

Azure geçişi Azure Service Bus platformunun anahtar özelliği ayaklar biridir. Yeni _karma bağlantılar_ yetenektir geçişinin HTTP ve WebSockets dayalı bir güvenli, açık Protokolü evrimi. Eşit oranda adlı eski yerini _BizTalk Services_ özel Protokolü foundation üzerinde oluşturulmuş özellik. Azure App Services karma bağlantılar tümleştirilmesi olarak çalışmaya devam edecek-değil.

Karma bağlantılar, çift yönlü, ikili akış iletişimi ve ağa bağlı iki uygulama arasındaki basit veri birimi akışını sağlar. Biri veya her iki taraf NAT veya güvenlik duvarı arkasında bulunabilir.

Bu makalede bağlanan istemciler dinleyicisi ve gönderen rolleri için karma bağlantılar geçiş ile istemci tarafı etkileşimler açıklanmaktadır. Ayrıca, nasıl dinleyicileri yeni bağlantıları ve istekleri kabul anlatır.

## <a name="interaction-model"></a>Etkileşim modeli

Karma bağlantılar geçişi, bir randevu noktası Azure bulutta tarafların bulmak ve kendi ağın açısından bağlanmak sağlayarak iki taraf bağlanır. Randevu noktası "karma" Bu ve diğer belgelerin API'leri hem de Azure portalında bağlantısıdır. Karma bağlantılar Hizmeti uç noktası bu makalenin geri kalanı için "hizmet" olarak adlandırılır.

Web yuvasını bağlantıları ve HTTP (S) istekleri ve yanıtları geçişi için hizmet sağlar.

Diğer ağ birçok API'ler tarafından kurulan terminolojisi üzerinde etkileşim modeli leans. İlk gelen bağlantıları işlemek için hazırlık gösterir ve daha sonra bunları kabul geldikçe dinleyici yoktur. Diğer tarafta bir istemci, bir çift yönlü iletişim yolu kurmak için kabul edilmesi için bu bağlantıyı bekleniyor dinleyicisi doğru bağlanır. "Bağlan" "Dinleme" ve "Kabul" çoğu yuva API'leri Bul aynı şartları şunlardır.

Herhangi bir geçişli iletişim modelini hizmet uç noktası doğru giden bağlantı her iki taraf sahiptir. Bu "dinleyicisi", "istemci" de cümlecik kullanımda hale getirir. ve diğer terminolojisi aşırı da neden olabilir. Bu nedenle karma bağlantıları için kullanılan kesin terminolojisi aşağıdaki gibidir:

Hizmet istemcilere olduğundan bağlantısının her iki tarafında programlar "istemcileri," adı verilir. Bekler ve bağlantıları kabul istemci "dinleyicisi" veya "dinleyicisi rol." olarak kabul edilir Hizmeti üzerinden bir dinleyici doğru yeni bir bağlantı başlatır istemci "gönderen" adlı ya da "gönderen rol."

### <a name="listener-interactions"></a>Dinleyici etkileşimleri

Dinleyici hizmetiyle beş etkileşimleri; yine de sahip istiyor musunuz? Tüm kablo ayrıntıları başvuru bölümünde bu makalenin sonraki bölümlerinde açıklanmıştır.

Dinleme, kabul etme ve istek iletileri hizmetinden alınır. Yenileme ve Ping işlemlerini dinleyicisi tarafından gönderilir.

#### <a name="listen-message"></a>Dinleme iletisi

Bir dinleyicisi hizmetine hazırlık belirtmek için bağlantıları kabul etmeye hazır bir giden WebSocket bağlantısı oluşturur. Bağlantı el sıkışması geçiş ad alanı ve bu ad "Dinleme" hakkı confers bir güvenlik belirteci yapılandırılmış karma bağlantı adını taşır.

WebSocket hizmeti tarafından kabul edildiğinde kayıt tamamlandıktan ve kurulan WebSocket "etkinleştirilmesine yönelik tüm sonraki etkileşimler denetim kanalı olarak" Canlı tutulur. Hizmetin en fazla 25 eşzamanlı dinleyicileri bir karma bağlantı sağlar. AppHooks kotasının belirlenemediğinden sağlamaktır.

İki veya daha fazla etkin dinleyiciler varsa karma bağlantılar için gelen bağlantıları bunları rastgele sırayla dengeli; Orta dağıtım ile en iyi çaba denenir.

#### <a name="accept-message"></a>İleti kabul et

Bir gönderici service üzerinde yeni bir bağlantı oturum açtığında, hizmet seçer ve karma bağlantı etkin dinleyicileri birini bildirir. Bu bildirim için dinleyici JSON iletisi olarak açık denetim kanalı üzerinden gönderilir. İleti dinleyici bağlantı kabul etmek için bağlanmalısınız WebSocket uç nokta URL'sini içerir.

URL olabilir ve doğrudan ek iş olmadan dinleyicisi tarafından kullanılan gerekir.
Kodlanmış bilgiler yalnızca zaman, aslında göndereni bağlantı kurulan baştan sona olmasını bekleyin konusunda istekli mi en çok kısa bir süre için geçerlidir. Varsaymak en fazla 30 saniyedir. URL, yalnızca bir başarılı bağlantı denemesinde için kullanılabilir. Randevu URL WebSocket bağlantı kuran hemen tüm başka etkinlik bu WebSocket ilk ve son gönderen geçirilen. Bu, hizmet tarafından herhangi bir araya veya yorumlama gerçekleşir.

### <a name="request-message"></a>İstek iletisi

Bu yetenek karma bağlantısında açıkça etkinse WebSocket bağlantı yanı sıra, dinleyicisi Ayrıca HTTP isteği çerçeveleri bir gönderenden alabilir.

Karma bağlantılar için HTTP desteği ekleme dinleyicileri gerekir işlemek `request` hareketi. İşlemiyor bir dinleyici `request` ve bu nedenle neden yinelenen zaman aşımı hataları sırasında bağlı hizmet tarafından gelecekte kara listede.

HTTP üstbilgisi ayrıştırma kitaplıkları JSON ayrıştırıcıları nadir olduğundan HTTP çerçeve üstbilgi meta verileri JSON içinde daha basit işleme için dinleyici çerçevesi tarafından da çevrilir. Yalnızca gönderenin ve yetkilendirme bilgileri de dahil olmak üzere geçiş HTTP ağ geçidi arasındaki ilişki için uygun olan HTTP meta verileri iletilmez. HTTP istek gövdelerine saydam ikili WebSocket çerçeveleri olarak aktarılır.

Dinleyici eşdeğer yanıt özelliğini kullanarak HTTP isteklerine yanıt verebilir.

İstek/yanıt akış denetim kanalı varsayılan olarak kullanılır, ancak "ayrı bir randevu için gerekli olduğunda WebSocket yükseltilebilir". Ayrı WebSocket bağlantıları her istemci konuşma verimliliğini artırmak, ancak bunlar ele alınması gereken daha fazla bağlantıya sahip dinleyici yük basit istemciler için desire mümkün olmayabilir.

Denetim kanalı, istek ve yanıt gövdesi boyutu en fazla 64 kB sınırlıdır. HTTP üstbilgisi meta verileri için 32 kB toplam sınırlıdır. İstek veya yanıtı bu eşiği aşarsa, randevu işleme için eşdeğer bir özelliğini kullanarak WebSocket için dinleyici yükseltmelisiniz [kabul](#accept-message).

İstekler için hizmet istekleri denetim kanalı üzerinden yönlendirmek kullanılıp karar verir. Bu içerir, ancak bir istek 64 kB (üst bilgiler ve gövde) depolayabileceği aşıyor burada ya da istekle birlikte gönderilen çalışmalarını sınırlı olmayabilir ["transfer-encoding öbekli"](https://tools.ietf.org/html/rfc7230#section-4.1) ve hizmet isteği 64 kB aşan beklenir neden olan veya İstek okunurken anlık değil. Hizmet randevu isteği göndermeyi seçerse, yalnızca bir dinleyici randevu adresini geçirir.
Dinleyici sonra WebSocket randevu oluşturmanız gerekir ve hizmet derhal gövdeleri randevu WebSocket de dahil olmak üzere tam istek sunar. Yanıt WebSocket randevu de kullanmanız gerekir.

Denetim kanalı üzerinden gelen istekler için dinleyici denetim kanalı üzerinden veya randevu aracılığıyla yanıt verilip karar verir. Hizmeti denetim kanalı üzerinden yönlendirilmiş her istek ile bir randevu adresine eklemeniz gerekir. Bu adres yalnızca geçerli istekten yükseltmek için geçerli olur.

Dinleyici yükseltme seçerse bağlanır ve derhal yanıt kurulan randevu yuva sunar.

Bir kez WebSocket kurulan randevu dinleyicisi, isteklerin ve yanıtların aynı istemciden daha fazla işleme korumanız gerekir. Hizmet için WebSocket gönderici ile bağlantı HTTPS yuva sürece devam ederse ve tüm istekler bu gönderenden işlenen WebSocket üzerinden yönlendirecek korur. Dinleyici WebSocket randevu kendi taraftan bırakma seçerse hizmet ayrıca bağlantı olup olmadığını Taleplerde devam ediyor zaten olabilir yedeklemiş gönderene bırakın.

#### <a name="renew-operation"></a>Yenileme işlemi

Dinleyici etkinken dinleyicisi kaydetme ve denetim kanalı korumak için kullanılması gereken güvenlik belirteci dolabilir. Belirteç süre sonu devam eden bağlantılarını etkilemez, ancak denetim kanalı düzeyinde veya sona erme anda hemen sonra hizmeti tarafından kesilmesine neden olmaz. "Yenile" işlemi dinleyicisi denetim kanalı için uzun süreler sürdürülebilir denetim kanalı ile ilişkili belirteci değiştirdiğinizden gönderebileceğiniz bir JSON iletisidir.

#### <a name="ping-operation"></a>Ping işlemi

Denetim kanalı aracılar şekilde, uzun bir süre için boşta kalırsa gibi yük Dengeleyiciler ya da NAT TCP bağlantısı kaybolmasına neden olabilir. "Ping" işlemi, çok küçük miktarda veri herkes bağlantı etkin olması için tasarlanmıştır ve ayrıca için dinleyici "canlı" test hizmet ağdaki yönlendiricilerin anımsatır kanalda göndererek önler. Ping başarısız olursa, denetim kanalı kullanılamaz olarak düşünülmelidir ve dinleyiciyi yeniden bağlanmanız.

### <a name="sender-interaction"></a>Gönderen etkileşimi

Hizmet ile iki etkileşimleri göndericisi vardır: bir Web yuva bağlandığında veya HTTPS üzerinden istekleri gönderir. İstekleri gönderen rolünden Web yuvası üzerinden gönderilemiyor.

#### <a name="connect-operation"></a>İşlemi Bağlan

"Bağlan" işlemi hizmet üzerinde WebSocket açar karma bağlantı adını ve (isteğe bağlı olarak, ancak gerekli varsayılan olarak) sağlayan bir güvenlik belirteci sorgu dizesinde conferring "Gönderme" izni. Hizmeti daha önce açıklandığı şekilde dinleyicisi etkileşim ve bu WebSocket ile birleştirilmiş bir randevu bağlantı dinleyicisi oluşturur. WebSocket kabul ettikten sonra bu WebSocket üzerinde tüm diğer etkileşimler ile bağlantılı bir dinleyici ' dir.

#### <a name="request-operation"></a>İstek işlemi

Karma özelliği etkin bağlantılar için gönderen dinleyicileri için büyük ölçüde Kısıtlanmamış HTTP istekleri gönderebilirsiniz.

Ya da sorgu dizesi veya isteğin HTTP üstbilgisinin katıştırılmış bir geçiş erişim belirtecini dışında geçişi dinleyicisi tr denetiminde tam olarak bırakarak tüm HTTP işlemlerini geçiş adresinde ve geçiş adres yolunun tüm son ekleri için tamamen saydam d uca yetkilendirme ve hatta HTTP uzantısı özellikleri gibi [CORS](https://www.w3.org/TR/cors/).

Gönderen yetkilendirme geçiş uç noktası ile varsayılan olarak açık, ancak isteğe bağlı değildir. Karma bağlantı sahibi anonim göndericiler izin vermeyi seçebilirsiniz. Hizmetin müdahale, inceleyin ve yetkilendirme bilgileri gibi çıkar:

1. Sorgu dizesi içeriyorsa bir `sb-hc-token` ifadesi ifade her zaman Sorgu dizesinden atılması. Geçiş yetkilendirme açıksa değerlendirilir.
2. İstek üstbilgilerini içeriyorsa bir `ServiceBusAuthorization` üstbilgi, üstbilgi ifade üstbilgi koleksiyonundan her zaman çıkarılır.
   Geçiş yetkilendirme açıksa değerlendirilir.
3. Yalnızca geçiş yetkilendirme açıksa ve istek üstbilgileri içeriyorsa bir `Authorization` üstbilgi ve önceki ifadelerden hiçbiri varsa, üstbilgi değerlendirilir ve kesilmiş olabilir. Aksi takdirde, `Authorization`her zaman olarak geçirildiğinde-değil.

Hiçbir etkin olan dinleyici varsa, hizmet bir 502 "Bozuk ağ geçidi" hata kodu döndürür. Hizmet isteği işlemek üzere görünmüyorsa, hizmet 60 saniye sonra 504 "ağ geçidi zaman aşımı" döndürür.

### <a name="interaction-summary"></a>Etkileşim özeti

Bu etkileşimi model gönderen istemci için bir dinleyici bağlı olduğu ve herhangi bir ek preambles veya hazırlık gerekir "temiz" bir WebSocket ile el sıkışması dışında geldiğini sonucudur. Bu model taşımalarına karma bağlantılar hizmeti kendi WebSocket istemci katmana doğru şekilde oluşturulmuş bir URL sağlayarak yararlanmak neredeyse her mevcut WebSocket istemci uygulamanızı sağlar.

Randevu bağlantı kabul etkileşiminin dinleyicisi edinir WebSocket de temiz ve tüm mevcut WebSocket sunucu uygulamasına kendi framework'ün yerel ağ dinleyicileri "kabul et" işlemleri ve karma bağlantılar uzak "kabul et" işlemleri arasında ayırt bazı en az bir ek soyutlama ile karmalayan.

HTTP istek/yanıt modeli gönderen bir büyük ölçüde Kısıtlanmamış HTTP protokolü yüzey alanını bir isteğe bağlı yetkilendirme katman sağlar. Dinleyici geri bir aşağı akış HTTP isteği açık veya ikili çerçeveler ile HTTP gövdeleri taşımasına gibi ele önceden ayrıştırılmış HTTP isteği üstbilgi bölümü alır. Yanıtlar aynı biçimi kullanır. 64 KB'den az istek ve yanıt gövdesi ile etkileşim için tüm gönderenlerin paylaşılan tek bir Web Yuva üzerinden işlenebilir. Daha büyük isteklerinin ve yanıtlarının randevu modelini kullanarak işlenebilir.

## <a name="protocol-reference"></a>Protokolü başvurusu

Bu bölümde daha önce açıklanan Protokolü etkileşimleri ayrıntılarını açıklanmaktadır.

Tüm WebSocket bağlantılar bağlantı noktası 443 üzerinde bazı WebSocket framework veya API tarafından yaygın olarak soyutlanır HTTPS 1.1'den yükseltme olarak yapılır. Açıklamayı buraya uygulama belirli bir framework öneren olmadan nötr, tutulur.

### <a name="listener-protocol"></a>Dinleyici Protokolü

Dinleyici Protokolü iki bağlantı hareketleri ve üç ileti işlemlerini oluşur.

#### <a name="listener-control-channel-connection"></a>Dinleyici denetim kanalı bağlantısı

Denetim kanalı WebSocket bağlantı oluşturma konusunda açıldığında:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

`namespace-address` Genellikle formunun karma bağlantı barındıran Azure geçiş ad alanı tam etki alanı adı `{myname}.servicebus.windows.net`.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir.

| Parametre        | Gerekli | Açıklama
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Evet      | Dinleyici rolü için parametre olmalıdır **sb hc eylem dinleme =**
| `{path}`         | Evet      | Üzerinde bu dinleyici kaydetmek için önceden yapılandırılmış karma bağlantı URL kodlanmış ad alanı yolu. Bu deyim eklenir sabit `$hc/` yol bölümü.
| `sb-hc-token`    | Evet\*    | Dinleyici bir geçerli, URL kodlanmış Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **dinleme** doğru.
| `sb-hc-id`       | Hayır       | Bu istemci tarafından sağlanan isteğe bağlı kimliği uçtan uca Tanılama izleme sağlar.

WebSocket bağlantı kaydedilmemiş, karma bağlantı yolu veya eksik veya geçersiz bir belirteç veya başka bir hata nedeniyle başarısız olursa hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması bir hata izleme-Azure destek personeli için iletilen kimliğini içerir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma bağlantı yolu geçersiz veya temel URL biçimi yanlış.
| 401  | Yetkilendirilmemiş   | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz olur.
| 403  | Yasak      | Güvenlik belirteci, bu eylem için bu yol için geçerli değil.
| 500  | İç Hata | Bir hizmette sorun oluştu.

Başlangıçta yukarı, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun WebSocket protokolü hata kodu kullanarak iletişim böylece nedeni ayarlandı sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa Hizmet denetim kanalı bir hata koşulu karşılaşılmadan kapanır değil. Temiz bir kapatma denetlenen istemcidir.

| WS durumu | Açıklama
| --------- | -------------------------------------------------------------------------------
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti.
| 1011      | Bir hizmette sorun oluştu.

#### <a name="accept-handshake"></a>Anlaşma kabul et

"Kabul" bildirim hizmeti tarafından dinleyiciye daha önce oluşturulmuş denetim kanalı üzerinden WebSocket metin çerçevesinde JSON ileti olarak gönderilir. Bu iletiyi yok Yanıtla yoktur.

İleti şu anda aşağıdaki özellikleri tanımlayan "kabul" adlı bir JSON nesnesi içerir:

* **Adres** – hizmet WebSocket kurmak için bir gelen bağlantı kabul etmek için kullanılacak URL dizesi.
* **Kimliği** – Bu bağlantı için benzersiz tanımlayıcı. Kimliği gönderen istemci tarafından sağlanan, sağlanan gönderen değerdir, aksi takdirde bir sistem tarafından oluşturulan değer.
* **connectHeaders** – sn WebSocket protokolü ve sn WebSocket uzantıları üstbilgileri de içeren gönderenin geçiş uç noktasına tarafından sağlanan tüm HTTP üstbilgileri.

```json
{
    "accept" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?..."
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

##### <a name="accepting-the-socket"></a>Yuva kabul etme

Kabul etmek için dinleyici sağlanan adresi WebSocket bağlantı kurar.

"Kabul" iletisi taşıyorsa bir `Sec-WebSocket-Protocol` üstbilgisi, bu protokolü destekliyorsa, dinleyiciyi yalnızca WebSocket kabul beklenir. Ayrıca, WebSocket belirlenen üstbilgisini ayarlar.

Aynı durum geçerlidir `Sec-WebSocket-Extensions` üstbilgi. Framework uzantı destekliyorsa, gerekli olan sunucu tarafı yanıt üstbilgisi ayarlamalısınız `Sec-WebSocket-Extensions` el sıkışma uzantısı.

URL olarak kullanılması gereken-kabul yuva kurmak için olsa da, aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir yuva kabul etmek için parametre olmalıdır `sb-hc-action=accept`
| `{path}`       | Evet      | (aşağıdaki paragraf bakın)
| `sb-hc-id`     | Hayır       | Önceki açıklamasına bakın **kimliği**.

`{path}` URL kodlanmış ad alanı, bu dinleyiciyi kaydetmek önceden yapılandırılmış karma bağlantısı yoludur. Bu deyim eklenir sabit `$hc/` yol bölümü.

`path` İfade genişletilmiş bir sonek ve kayıtlı adı ayıran eğik sonra izleyen bir sorgu dizesi ifadesi.
Bu, HTTP üstbilgilerini eklemek mümkün değilse, gönderme bağımsız değişkenleri kabul dinleyicisi geçirmek gönderen istemci sağlar. Dinleyici framework sabit yol bölümü ve yolun kayıtlı adından ayrıştırır ve önüne hiçbir sorgu dizesi bağımsız değişkenler olmadan geri kalanı, büyük olasılıkla hale getirir beklenir `sb-`, bağlantı kabul etmeye karar verme için uygulama tarafından kullanılabilir.

Daha fazla bilgi için aşağıdaki "Gönderen Protokolü" bölümüne bakın.

Varsa bir hata, hizmet gibi yanıtlayabilir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -----------------------------------
| 403  | Yasak      | URL geçerli değil.
| 500  | İç Hata | Bir hizmette sorun oluştu

 Bağlantı kurulduktan sonra sunucu WebSocket göndereni aşağı ya da aşağıdaki durumundaki kapattığında WebSocket kapatır:

| WS durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.                        |
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |
| 1011      | Bir hizmette sorun oluştu.                                            |

##### <a name="rejecting-the-socket"></a>Yuva reddetme

 Yuva denetledikten sonra reddetme `accept` ileti, böylece durum kodu ve durum açıklaması reddetme akmasını sağlamak için neden iletişim gönderene geri benzer bir el sıkışması gerektirir.

 (Yani bir tanımlanan hata durumunda sonlandırmak için tasarlanmıştır) WebSocket el sıkışma Protokolü tasarım seçiminin kullanılmasıdır dinleyicisi istemci uygulamaları WebSocket istemcide yararlanmaya devam edebilirsiniz ve gerekmez fazladan kullandığınızda, HTTP istemci tam.

 Yuva reddetmek için URI adresi istemci geçen gelen `accept` iletisi ve iki sorgu dizesi parametreleri için aşağıdaki gibi ekler:

| Param                   | Gerekli | Açıklama                              |
| ----------------------- | -------- | ---------------------------------------- |
| SB hc statusCode        | Evet      | Sayısal HTTP durum kodu.                |
| SB hc statusDescription | Evet      | Reddetme İnsan okunabilir açıklaması. |

Sonuçta elde edilen URI sonra WebSocket bağlantı kurmak için kullanılır.

Hiçbir WebSocket kurulduktan sonra doğru tamamlarken, bu el sıkışma bilerek 410, HTTP hata koduyla başarısız olur. Bir sorun yaşanırsa, aşağıdaki kodları hata açıklanmaktadır:

| Kod | Hata          | Açıklama                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Yasak      | URL geçerli değil.                |
| 500  | İç Hata | Bir hizmette sorun oluştu. |

#### <a name="request-message"></a>İstek iletisi

`request` İletinin gönderildiği hizmeti tarafından dinleyiciye denetim kanalı üzerinden. Aynı iletiyi de randevu sonra WebSocket gönderilir.

`request` İki bölümden oluşur: üstbilgi ve ikili gövde çerçevede.
Hiçbir gövdesi varsa, gövde çerçeveler göz ardı edilir. Gövde bulunup bulunmadığını için boolean göstergesidir `body` istek iletisindeki özelliği.

Bir istek gövdesi bir talebi yapısı şu şekilde görünebilir:

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : true,
        ...
    }
}
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame -FIN
FEFEFEFEFEFEFEFEFEFEF...
----------------------------------
```

Dinleyici birden çok ikili çerçevelere istek gövdesi alma işlemesi gerekir (bkz [WebSocket parçaları](https://tools.ietf.org/html/rfc6455#section-5.4)).
İkili bir çerçeve FIN bayrağı ayarlanmış alındığında istek sona erer.

Body olmadan bir istek için yalnızca bir metin çerçevesi yoktur.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : false,
        ...
    }
}
----------------------------------
```

JSON içeriği için `request` aşağıdaki gibidir:

* **Adres** -URI dize. Bu istek için kullanılacak randevu adresini budur. Gelen istek 64 KB'den büyük ise, bu iletiyi geri kalan boş bırakılır ve istemci, randevu el sıkışma eşdeğer başlatmalıdır `accept` aşağıda açıklanan işlemi. Hizmet ardından tam sokar `request` yerleşik Web yuvadaki. 64 kB aşmayı yanıtı bekleniyor, dinleyicisi gerekir ayrıca bir randevu el sıkışması başlatın ve ardından kurulan Web yuvası yanıtı aktarın.
* **Kimliği** – dize. Bu istek için benzersiz tanımlayıcı.
* **requestHeaders** – bu nesne uç noktasına yetkilendirme bilgilerinin durumla gönderici tarafından açıklandığı gibi sağlanmadı tüm HTTP üstbilgilerin içerir [yukarıda](#request-operation)ve kesinlikle ilişkili üstbilgileri ağ geçidi ile bağlantı. Tüm üstbilgileri özellikle tanımlanan veya içinde ayrılmış [RFC7230](https://tools.ietf.org/html/rfc7230), dışında `Via`, atılmış ve iletilen değil:

  * `Connection` (RFC7230, Bölüm 6.1)
  * `Content-Length`  (RFC7230, Bölüm 3.3.2)
  * `Host`  (RFC7230, bölüm 5.4)
  * `TE`  (RFC7230, Bölüm 4.3)
  * `Trailer`  (RFC7230, Bölüm 4.4)
  * `Transfer-Encoding`  (RFC7230, Bölüm 3.3.1)
  * `Upgrade` (RFC7230, Bölüm 6.7)
  * `Close`  (RFC7230, Bölüm 8.1)

* **requestTarget** – dize. Bu özellik tutan [(RFC7230, bölüm 5.3) "istek hedef"](https://tools.ietf.org/html/rfc7230#section-5.3) istek. Bu, tüm yapılandırıldıktan sorgu dizesi bölümü içerir `sb-hc-` parametreleri öneki.
* **yöntem** -dize. Bu istek başına yöntemidir [RFC7231, Bölüm 4](https://tools.ietf.org/html/rfc7231#section-4). `CONNECT` Yöntemi olmayan kullanılmalıdır.
* **Gövde** – boolean. Bir daha ikili gövde çerçeve izleyip izlemediğini gösterir.

``` JSON
{
    "request" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?...",
        "id" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "requestTarget" : "/abc/def?myarg=value&otherarg=...",
        "method" : "GET",
        "requestHeaders" : {
            "Host" : "...",
            "Content-Type" : "...",
            "User-Agent" : "..."
        },
        "body" : true
     }
}
```

##### <a name="responding-to-requests"></a>İsteklerine yanıt

Alıcı yanıtlaması gerekir. Bağlantı korurken isteklerini yanıtlamak için yinelenen hatası kara listede dinleyiciyi neden olabilir.

Yanıtları herhangi bir sırada gönderilebilir, ancak her istek için 60 saniye içinde yanıt gerekir veya teslim başarısız olarak raporlanır. 60 saniye son tarihe kadar sayılan `response` çerçeve hizmeti tarafından alınmış. Birden çok ikili çerçeveler ile devam eden bir yanıt 60 saniyeden fazla bir süre için boş olamaz veya sonlandırılır.

İstek denetim kanalı üzerinden alınmazsa, yanıt ya da denetim kanalı isteği burada alındı gönderilmelidir veya bir randevu kanal üzerinden gönderilen gerekir.

Yanıt "yanıt" adlı bir JSON nesnesidir. Gövde içerik işlemeye yönelik kurallar ile tam olarak gibi olan `request` iletisi ve temel `body` özelliği.

* **RequestId** – dize. GEREKLİ. `id` Özellik değerinin `request` için yanıt iletisi.
* **statusCode** – sayı. GEREKLİ. bildirim sonucunu gösteren bir sayısal HTTP durum kodu. Tüm durum kodlarını [RFC7231, Bölüm 6](https://tools.ietf.org/html/rfc7231#section-6) izin verilen dışında [502 "bozuk ağ geçidi"](https://tools.ietf.org/html/rfc7231#section-6.6.3) ve [504 "ağ geçidi zaman aşımı"](https://tools.ietf.org/html/rfc7231#section-6.6.5).
* **statusDescription** -dize. İSTEĞE BAĞLI. HTTP durum kodunu neden ifadesini başına [RFC7230, Bölüm 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)
* **responseHeaders** – bir dış HTTP yanıtına ayarlanacak HTTP üstbilgileri.
  İle `request`, RFC7230 tanımlanan üstbilgi değil kullanılmalıdır.
* **Gövde** – boolean. İkili Gövde follow(s) çerçevede olup olmadığını gösterir.

``` text
----- Web Socket text frame ----
{
    "response" : {
        "requestId" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "statusCode" : "200",
        "responseHeaders" : {
            "Content-Type" : "application/json",
            "Content-Encoding" : "gzip"
        }
         "body" : true
     }
}
----- Web Socket binary frame -FIN
{ "hey" : "mydata" }
----------------------------------
```

##### <a name="responding-via-rendezvous"></a>Randevu yanıt

64 kB aşan yanıtlar için yanıt bir randevu yuva teslim edilmelidir. Ayrıca, istek 64 kB aşarsa ve `request` yalnızca adres alanı içeren bir randevu yuva elde etmek için oluşturulmuş olmalıdır `request`. Bir randevu yuva kurulduktan sonra devam ederken ilgili istemci ve ilgili istemci ve sonraki istek için yanıtlarının randevu yuvası üzerinden teslim edilmelidir.

`address` URL'de `request` olarak kullanılması gereken-randevu yuva kurmak için olsa da, aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir yuva kabul etmek için parametre olmalıdır `sb-hc-action=request`

Varsa bir hata, hizmet gibi yanıtlayabilir:

| Kod | Hata           | Açıklama
| ---- | --------------- | -----------------------------------
| 400  | Geçersiz istek | Tanınmayan bir eylem veya URL geçerli değil.
| 403  | Yasak       | URL süresi doldu.
| 500  | İç Hata  | Bir hizmette sorun oluştu

 Bağlantı kurulduktan sonra sunucu istemcinin HTTP yuva kapatıldığında veya aşağıdaki durumundaki WebSocket kapatır:

| WS durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.                        |
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |
| 1011      | Bir hizmette sorun oluştu.                                            |


#### <a name="listener-token-renewal"></a>Dinleyici belirteci yenileme

Dinleyici belirteci dolmak üzere olduğunda, bu yerleşik denetim kanalı aracılığıyla hizmetine çerçeve mesaj göndererek değiştirebilirsiniz. İleti adlı bir JSON nesnesi içerir `renewToken`, şu anda aşağıdaki özelliği tanımlar:

* **belirteç** – ad alanı veya confers karma bağlantı için geçerli, URL kodlanmış bir hizmet veri yolu paylaşılan erişim belirteci **dinleme** doğru.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

Belirteç doğrulama başarısız olursa, erişim reddedildi ve bulut hizmeti bir hata ile denetim kanalı WebSocket kapatır. Aksi durumda yanıt yoktur.

| WS durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti. |

### <a name="web-socket-connect-protocol"></a>Web yuvasına bağlanma Protokolü

Gönderen Protokolü dinleyici kurulan şekilde etkili bir şekilde aynıdır.
Uçtan uca WebSocket için maksimum saydamlığı hedeftir. Bağlanmak için adres dinleyicisi ile aynıdır, ancak "eylem" farklıdır ve farklı izin belirteci gerekiyor:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

_Ad alanı adresi_ genellikle formunun karma bağlantı barındıran Azure geçiş ad alanı tam etki alanı adı `{myname}.servicebus.windows.net`.

İstek, uygulama tanımlı olanlar da dahil olmak üzere, rasgele ek HTTP üstbilgileri içerebilir. Tüm sağlanan üstbilgileri akış dinleyiciye ve bulunabilir `connectHeader` nesnesinin **kabul** denetim iletisi.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir:

| Param          | Gerekli mi? | Açıklama
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Evet       | Gönderen rolü için parametre olmalıdır `sb-hc-action=connect`.
| `{path}`       | Evet       | (aşağıdaki paragraf bakın)
| `sb-hc-token`  | Evet\*     | Dinleyici bir geçerli, URL kodlanmış Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **Gönder** doğru.
| `sb-hc-id`     | Hayır        | Uçtan uca Tanılama izleme sağlar ve bir dinleyici kabul anlaşması sırasında kullanılabilir duruma isteğe bağlı bir kimliği.

 `{path}` , Bu dinleyiciyi kaydetmek önceden yapılandırılmış karma bağlantısı URL kodlanmış ad alanı yolu. `path` İfadesi uzatabilirsiniz sonek ve daha fazla iletişim kurmak için bir sorgu dizesi ifadesi. Karma bağlantı yolu altında kaydedilmişse `hyco`, `path` ifade olabilir `hyco/suffix?param=value&...` burada tanımlanan sorgu dizesi parametreleri tarafından izlenen. Tam bir deyim aşağıdaki gibi olabilir:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

`path` İfade geçirilir aracılığıyla dinleyicisi "kabul" denetimi iletisinde bulunan URI adresi.

WebSocket bağlantı kayıtlı karma bağlantı yolu, eksik veya geçersiz bir belirteç veya başka bir hata nedeniyle başarısız olursa hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması izleme bildirilmesi kimliği Azure destek personeli için bir hata içeriyor:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma bağlantı yolu geçersiz veya temel URL biçimi yanlış.
| 401  | Yetkilendirilmemiş   | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz olur.
| 403  | Yasak      | Güvenlik belirteci bu yol için ve bu eylem için geçerli değil.
| 500  | İç Hata | Bir hizmette sorun oluştu.

Yukarı, izleme kimliği de içeren açıklayıcı bir hata iletisi ile birlikte uygun WebSocket protokolü hata kodu kullanarak iletişim böylece nedeni başlangıçta ayarlandıktan sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa

| WS durumu | Açıklama
| --------- | ------------------------------------------------------------------------------- 
| 1000      | Dinleyici yuva kapatın.
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesini ihlal etti.
| 1011      | Bir hizmette sorun oluştu.

### <a name="http-request-protocol"></a>HTTP istek Protokolü

HTTP isteği protokol Protokolü yükseltmeler dışında rasgele HTTP isteklerini sağlar.
HTTP istekleri, karma bağlantılar WebSocket istemcileri için kullanılan $hc iç olmadan varlığın normal çalışma zamanı adresine işaret.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

_Ad alanı adresi_ genellikle formunun karma bağlantı barındıran Azure geçiş ad alanı tam etki alanı adı `{myname}.servicebus.windows.net`.

İstek, uygulama tanımlı olanlar da dahil olmak üzere, rasgele ek HTTP üstbilgileri içerebilir. Üst bilgiler, doğrudan RFC7230 içinde tanımlanan dışındaki tüm sağlanan (bkz [istek iletisi](#Request message)) akış dinleyiciye ve bulunabilir `requestHeader` nesnesinin **isteği** ileti.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir:

| Param          | Gerekli mi? | Açıklama
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Evet\*     | Dinleyici bir geçerli, URL kodlanmış Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **Gönder** doğru.

Belirteç ayrıca ya da aktarılabilen `ServiceBusAuthorization` veya `Authorization` HTTP üstbilgisi. Karma bağlantı anonim isteklere izin vermek için yapılandırılmışsa, belirteç atlanabilir.

Ya da olmayan bir true HTTP proxy ekler olsa bile hizmet etkili bir şekilde bir proxy sunucu olarak davrandığı için bir `Via` üstbilgisi veya varolan açıklama ekler `Via` üstbilgi uyumlu [RFC7230, bölüm 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1).
Geçiş ad alanı ana bilgisayar adı için hizmet ekler `Via`.

| Kod | İleti  | Açıklama                    |
| ---- | -------- | ------------------------------ |
| 200  | Tamam       | İstek en az bir dinleyici tarafından işlendi.  |
| 202  | Kabul Edildi | İstek en az bir dinleyici tarafından kabul edildi. |

Varsa bir hata, hizmet gibi yanıtlayabilir. Yanıt hizmetinden veya dinleyiciyi olup gelmektedir varlığını tanımlanabilir `Via` üstbilgi. Üstbilgi varsa, yanıt dinleyicisi ' dir.

| Kod | Hata           | Açıklama
| ---- | --------------- |--------- |
| 404  | Bulunamadı       | Karma bağlantı yolu geçersiz veya temel URL biçimi yanlış.
| 401  | Yetkilendirilmemiş    | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz olur.
| 403  | Yasak       | Güvenlik belirteci bu yol için ve bu eylem için geçerli değil.
| 500  | İç Hata  | Bir hizmette sorun oluştu.
| 503  | Hatalı Ağ Geçidi     | İstek herhangi bir dinleyici yönlendirilmiş değil.
| 504  | Ağ geçidi zaman aşımı | İstek için bir dinleyici yönlendirilen ancak dinleyicisi giriş gerekli sürede kabul etmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)
