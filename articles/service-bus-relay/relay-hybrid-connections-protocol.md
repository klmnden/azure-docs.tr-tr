---
title: Azure geçiş karma bağlantıları protokol Kılavuzu | Microsoft Docs
description: Azure geçiş karma bağlantıları protokol Kılavuzu.
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
ms.openlocfilehash: e96d0103a03e841f39e8adb88215f6d6e24a305a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64706085"
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Azure geçiş karma bağlantılar Protokolü

Azure geçişi Azure Service Bus platformunun önemli özellik yapı taşları biridir. Yeni _karma bağlantılar_ özelliktir geçişinin HTTP ve WebSockets dayalı bir güvenli, açık protokol evrimi. Eşit adlı önceki sürümlereni _BizTalk Hizmetleri_ bir özel Protokolü foundation üzerinde oluşturulan özellik. Azure App Services karma bağlantılar tümleştirilmesi olarak çalışmaya devam edecek-olduğu.

Karma bağlantı, çift yönlü, ikili akış iletişimi ve ağa bağlı iki uygulama arasındaki basit veri birimi akışını sağlar. Veya her ikisi de NAT veya güvenlik duvar arkasında bulunabilir.

Bu makalede, dinleyici ve gönderen rollerindeki istemcilerin bağlanmasında karma bağlantılar geçişi ile istemci tarafı etkileşimler açıklanmaktadır. Ayrıca, nasıl dinleyicileri yeni bağlantıları ve istekleri kabul açıklar.

## <a name="interaction-model"></a>Etkileşim modeli

Karma bağlantılar geçişi bir buluşma noktası taraflar keşfedin ve kendi ağın açısından bağlanmak Azure bulutunda sağlayarak iki tarafın bağlanır. Buluşma noktası API'leri ve aynı zamanda Azure portalında bu ve diğer belgeler, "hibrit bağlantı" adı verilir. Karma bağlantılar hizmet uç noktası için bu makalenin geri kalanında "hizmet" olarak adlandırılır.

Web yuvası bağlantı ve HTTP (S) istekleri ve yanıtları geçişi için hizmet verir.

Diğer ağ çok sayıda API tarafından kurulan adlandırma üzerinde etkileşim modeli leans. Önce gelen bağlantıları işlemek için hazır olma durumu gösteren ve daha sonra bunları kabul geldikçe bir dinleyici yok. Diğer tarafta bir istemci, çift yönlü iletişim yolunun kurmak için kabul edilmesi için bu bağlantıyı bekleniyor dinleyiciyi doğrultusunda bağlanır. "Bağlan" "Listen" ve "Kabul" çoğu yuva API'leri bulma aynı terimlerdir.

Bir hizmet uç noktasının doğru giden bağlantı taraflardan herhangi bir geçişli iletişim modelini sahiptir. Bu, "dinleyici", "istemci" de cümlecik kullanımda olmasını sağlar. ve diğer terminolojisi aşırı da neden olabilir. Bu nedenle karma bağlantıları için kullanılan kesin terminoloji aşağıdaki gibidir:

İstemcilere hizmet çünkü program bir bağlantının her iki tarafındaki "istemcileri," adı verilir. Bekler ve bağlantılarını kabul istemci "dinleyici" veya "dinleyici rol." olarak kabul edilir Hizmeti aracılığıyla bir dinleyici doğrultusunda yeni bir bağlantı başlatır istemci "sender" olarak adlandırılır ve "gönderen rol."

### <a name="listener-interactions"></a>Dinleyici etkileşimleri

Dinleyici hizmetiyle beş etkileşimleri; yine de sahip istiyor musunuz? Tüm kablo ayrıntıları başvuru bölümünde bu makalenin sonraki bölümlerinde açıklanmıştır.

Dinleme, kabul etme ve istek iletileri hizmetinden alınır. Yenileme ve Ping işlemlerini dinleyici tarafından gönderilir.

#### <a name="listen-message"></a>İleti dinleme

Dinleyici hizmeti için hazırlık belirtmek için bağlantı kabul etmeye hazır giden bir WebSocket bağlantısı oluşturur. Bağlantı el sıkışması geçiş ad alanı ve bu ad, "Dinleme" sağa confers bir güvenlik belirteci yapılandırılmış bir karma bağlantı adını taşır.

WebSocket hizmeti tarafından kabul edildiğinde, kayıt tamamlandıktan ve kurulan WebSocket canlı olarak "sonraki tüm etkileşimleri etkinleştirmek için denetim kanalı" tutulur. Hizmet, en fazla 25 eş zamanlı dinleyicileri bir karma bağlantı sağlar. AppHooks kotasının belirlenecek sağlamaktır.

İki veya daha fazla etkin dinleyiciler varsa karma bağlantılar için gelen bağlantıları arasında rastgele sırayla dengelenir; Orta dağıtım ile en iyi çaba denenir.

#### <a name="accept-message"></a>İletiyi kabul

Bir gönderici service üzerinde yeni bir bağlantı oturum açtığında, hizmet seçer ve bir karma bağlantı etkin dinleyiciler bildirir. Bu bildirim dinleyicisi için bir JSON ileti denetim kanalı üzerinden gönderilir. İleti dinleyici bağlantı kabul etmek için bağlanmalısınız WebSocket uç nokta URL'sini içerir.

URL olabilir ve herhangi bir ek çalışma yapmadan dinleyici tarafından doğrudan kullanılması gerekir.
Kodlanmış bilgiler yalnızca kısa bir süre sonu, temelde göndereni bağlantı kurulan uçtan uca olmasını bekleyin iradeye sahip olması için geçerlidir. Varsaymak en fazla 30 saniyedir. URL, yalnızca bir başarılı bağlantı denemesinde için kullanılabilir. WebSocket bağlantısı buluşma URL'si ile kurulan hemen sonra bu WebSocket tüm daha fazla etkinlik başlangıç ve bitiş gönderen iletilir. Bu, hizmet tarafından herhangi bir araya veya yorumu olmadan gerçekleşir.

### <a name="request-message"></a>İstek iletisi

Bu özellik, karma bağlantıyı Ekle'ye açıkça etkinse, WebSocket bağlantılarını yanı sıra, dinleyici Ayrıca HTTP isteği çerçeve bir gönderenden alabilir.

Karma bağlantılar HTTP desteği ekleme dinleyicileri gerekir işlemek `request` hareketi. İşlemiyor bir dinleyici `request` ve bu nedenle neden yinelenen zaman aşımı hataları sırasında bağlı hizmet tarafından gelecekte kara listede.

HTTP üstbilgisi ayrıştırma kitaplıkları JSON Çözümleyicileri nadir olduğu için HTTP çerçeve üstbilgi meta verileri JSON'a daha basit işleme için dinleyici framework tarafından da çevrilir. Yalnızca ilgili gönderen ve yetkilendirme bilgileri dahil olmak üzere geçiş HTTP ağ geçidi arasındaki ilişki için olan HTTP meta verileri iletilmez. HTTP istek gövdesi ikili WebSocket çerçeveler şeffaf bir şekilde aktarılır.

Dinleyici, eşdeğer yanıt hareketini kullanarak HTTP isteklerine yanıt verebilir.

İstek/yanıt akışı varsayılan olarak denetim kanalı kullanır, ancak "farklı bir randevu gerekli olduğunda WebSocket yükseltilebilir". Her istemci konuşma için aktarım hızı ayrı WebSocket bağlantılarını iyileştirin, ancak bunlar ele alınması gereken daha fazla bağlantı dinleyicisiyle yük basit istemciler için arzusu mümkün olmayabilir.

Denetim kanalı, istek ve yanıt gövdeleri en fazla 64 kB boyutunda sınırlıdır. HTTP üstbilgisi meta verileri 32 kB'ın toplam sınırlıdır. İstek veya yanıtı bu eşiği aşarsa, dinleyici bir randevu işleme için eşdeğer bir hareket kullanarak WebSocket yükseltmelisiniz [kabul](#accept-message).

İstekleri için hizmet istekleri denetim kanalı üzerinden yönlendirmek etkinleştirilip etkinleştirilmeyeceğini belirler. Bu içerir, ancak bir istek 64 kB (üst bilgiler ve gövdesi) yükseltebilir aşıyor veya ile istek gönderirse çalışmalarına sınırlı olmayabilir ["öbekli aktarım kodlamasını"](https://tools.ietf.org/html/rfc7230#section-4.1) ve hizmet isteği için 64 kB'ı aşan beklenir neden veya İstek okunurken anlık bir işlem değildir. Hizmet isteği randevu teslim seçerse dinleyiciye yalnızca randevu adresine geçer.
Dinleyici sonra WebSocket randevu oluşturmanız gerekir ve hizmetin en kısa sürede randevu WebSocket organları dahil olmak üzere tam istek sunar. Yanıt, ayrıca WebSocket randevu kullanmanız gerekir.

Denetim kanalı gelen istekleri için dinleyici olup denetim kanalı üzerinden veya randevu aracılığıyla yanıt karar verir. Hizmet, bir randevu adresine denetim kanalı üzerinden yönlendirilmesini her istekle içermelidir. Bu adres, yalnızca geçerli istekten yükseltmek için geçerlidir.

Dinleyici yükseltme seçerse bağlanır ve en kısa sürede yanıt kurulan randevu yuva sunar.

Bir kez WebSocket kurulduktan randevu dinleyicisi bu isteklerin ve yanıtların aynı istemciden daha ayrıntılı işleme için korumanız gerekir. Hizmet için WebSocket HTTPS bağlantısı gönderen ile yuva olduğu sürece devam ederse ve gönderenden gelen tüm istekler tutulan WebSocket üzerinden yönlendirecek tutacaktır. Dinleyici randevu WebSocket alt tarafında açılan seçerse hizmet ayrıca olup Taleplerde sürüyor zaten olabilir aldıklarına bağlı olarak, gönderen bağlantısı bırakın.

#### <a name="renew-operation"></a>Yenileme işlemi

Dinleyici etkin durumdayken dinleyici kaydetme ve denetim kanalı korumak için kullanılması gereken güvenlik belirteci dolabilir. Belirteç süre sonu devam eden bağlantılar etkilemez, ancak denetim kanalı, sırasında veya hemen sonra süre sonu anındaki hizmeti tarafından kesilmesine neden olmaz. "Yenile" denetim kanalı için uzun süreler sürdürülebilir denetim kanalı ile ilişkilendirilmiş belirteç değiştirilecek dinleyici gönderebileceğiniz bir JSON ileti işlemdir.

#### <a name="ping-operation"></a>Ping işlemi

Denetim kanalı aracılar üzerinde yöntemi, uzun bir süredir boşta kalırsa TCP bağlantısı gibi yük Dengeleyiciler ya da NAT bırakabilir. "Ping" işlemi, az miktarda veriniz herkesin bağlantı etkin tutulan bağlantıyı destekliyorsa olacak şekilde tasarlanmış ve dinleyici için de "canlı" bir test olarak kullanılır ağdaki yönlendiricilerin ulaşabileceğini belirten kanalındaki göndererek önler. Ping başarısız olursa, denetim kanalı kullanılamaz olarak düşünülmelidir ve dinleyiciyi yeniden bağlanacaktır.

### <a name="sender-interaction"></a>Gönderen etkileşimi

Hizmet iki etkileşim göndericisi vardır: bir Web yuvası bağlanan veya HTTPS aracılığıyla istekler gönderir. İstekleri gönderen rolünden bir Web yuvası gönderilemez.

#### <a name="connect-operation"></a>İşlem bağlama

"Bağlan" işlemi hizmet üzerinde bir WebSocket açılır adı (isteğe bağlı olarak, ancak varsayılan olarak gerekli) ve karma bağlantı sağlayan bir güvenlik belirteci sorgu dizesinde conferring "Gönderme" izni. Hizmeti daha önce açıklanan şekilde dinleyicinin etkileşim ve bu WebSocket ile birleştirilmiş bir randevu bağlantı dinleyicisi oluşturur. WebSocket kabul ettikten sonra diğer tüm etkileşimler, WebSocket için bağlı bir dinleyici ' dir.

#### <a name="request-operation"></a>İstek işlemi

Karma özellik etkin bağlantılar için gönderen dinleyicilere büyük ölçüde Kısıtlanmamış HTTP istekleri gönderebilirsiniz.

Ya da sorgu dizesi veya bir isteğin HTTP üstbilgisindeki gömülü olan bir geçiş erişim belirteci dışında geçiş dinleyici tr denetiminde tam olarak bırakarak tüm HTTP işlemleri bir geçiş adresine ve geçiş adres yoluna tüm sonekleri için tamamen saydam. d uçtan uca yetkilendirme ve hatta HTTP uzantı özellikleri gibi [CORS](https://www.w3.org/TR/cors/).

Gönderen yetkilendirme geçiş uç noktası ile varsayılan olarak açıktır, ancak isteğe bağlı değildir. Karma bağlantı sahibi, anonim gönderenlerin izin vermeyi seçebilirsiniz. Hizmet ıntercept, inceleyin ve yetkilendirme bilgileri gibi Şerit:

1. Sorgu dizesini içeriyorsa bir `sb-hc-token` deyim, ifade her zaman Sorgu dizesinden kaldırılır. Geçiş yetkilendirme açıksa değerlendirilir.
2. İstek üst bilgilerini içermesi durumunda bir `ServiceBusAuthorization` üst bilgi, üst bilgi ifade üstbilgi koleksiyondan her zaman çıkarılır.
   Geçiş yetkilendirme açıksa değerlendirilir.
3. Geçiş yetkilendirme yalnızca açıktır ve istek üst bilgilerini içeren bir `Authorization` üst bilgi ve önceki ifadelerden hiçbiri varsa, üst bilgi değerlendirilir ve kesilmiş. Aksi takdirde, `Authorization`her zaman olarak geçirilen-olduğu.

Etkin dinleyici yok ise, hizmet bir 502 "Hatalı ağ geçidi" hata kodu döndürür. Hizmet isteği işlemek için görünmüyorsa, hizmet bir 504 "ağ geçidi zaman aşımı" 60 saniye sonra döndürür.

### <a name="interaction-summary"></a>Etkileşim özeti

Bu etkileşimi model gönderen istemci için bir dinleyici bağlı olduğu ve daha fazla preambles ya da hazırlık gerekir "temiz" bir WebSocket sıkışmaya dışında geldiğini sonucudur. Bu model, kolayca karma bağlantılar hizmet doğru şekilde oluşturulmuş bir URL, WebSocket istemcisi katmana sağlanarak yararlanmak neredeyse tüm mevcut websocket istemcisi sağlar.

Randevu bağlantı kabul etkileşiminin dinleyici alır WebSocket de temiz ve tüm mevcut WebSocket sunucu uygulamalarına "kabul et" işlemleri ayırt bazı fazladan en az soyutlama ile teslim framework'ün yerel ağ dinleyicileri ve karma bağlantılar uzak "işlemleri kabul".

HTTP istek/yanıt modeli gönderen bir büyük ölçüde Kısıtlanmamış HTTP protokolü yüzey alanıyla bir isteğe bağlı yetkilendirme katmanı sağlar. Dinleyici aşağı akış bir HTTP isteği geri açık veya ikili çerçevelerle HTTP gövdesi taşıyan olduğu gibi ele bir önceden ayrıştırılmış HTTP isteği üst bilgisi bölümü alır. Yanıtlar aynı biçimi kullanır. Tüm Gönderenler için paylaşılan tek bir Web yuvası üzerinden istek ve yanıt gövdesinin 64 KB'den az etkileşim işlenebilir. Büyük istekleri ve yanıtları randevu modeli kullanılarak işlenebilir.

## <a name="protocol-reference"></a>Protokolü başvurusu

Bu bölümde daha önce açıklanan Protokolü etkileşimleri ayrıntılarını açıklar.

Tüm WebSocket bağlantılarını bağlantı noktası 443 üzerinde genellikle bazı WebSocket framework veya API tarafından soyutlanır HTTPS 1.1'den yükseltme olarak yapılır. Açıklamayı buraya uygulama belirli bir framework önerme olmadan nötr, tutulur.

### <a name="listener-protocol"></a>Dinleyici Protokolü

Dinleyici protokolü, iki bağlantı hareketlerini ve üç ileti işlemleri oluşur.

#### <a name="listener-control-channel-connection"></a>Dinleyici denetim kanalı bağlantısı

Denetim kanalı WebSocket bağlantı oluşturma ile açılır:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

`namespace-address` Karma bağlantı, genellikle formun barındıran Azure geçiş ad alanı tam etki alanı adıdır `{myname}.servicebus.windows.net`.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir.

| Parametre        | Gerekli | Açıklama
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Evet      | Dinleyici rolü için bir parametresi olmalıdır **sb hc eylem dinleme =**
| `{path}`         | Evet      | Şirket bu dinleyici kaydetmek için önceden yapılandırılmış karma bağlantıyı URL olarak kodlanmış bir ad alanı yolu. Bu ifade eklenir sabit `$hc/` yol bölümü.
| `sb-hc-token`    | Evet\*    | Dinleyici bir geçerli URL olarak kodlanmış, Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **dinleme** doğru.
| `sb-hc-id`       | Hayır       | Bu istemci tarafından sağlanan isteğe bağlı kimliği uçtan uca tanılama izlemesi sağlar.

WebSocket bağlantısı kaydedilmemiş, karma bağlantı yolu veya eksik veya geçersiz bir belirteç veya başka bir hata nedeniyle başarısız olursa, hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması bir hata izleme-Azure destek personelinin bildirilebilmesi kimliğini içerir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma bağlantı yolu geçersiz veya temel URL yanlış biçimlendirilmiş.
| 401  | Yetkilendirilmemiş   | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz.
| 403  | Yasak      | Güvenlik belirteci, bu yol için bu eylem için geçerli değil.
| 500  | İç hata | Hizmette bir sorun oluştu.

Bu ilk yedekleme izleme kimliğini de içeren bir açıklayıcı hata iletisi ile birlikte uygun bir WebSocket protokolü hatası kodu kullanarak bildiriliyor böylece nedeni ayarlandıktan sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa Hizmet denetim kanalı bir hata koşulu karşılaşılmadan kapatacak değil. Herhangi bir temiz kapatma denetlenen istemcisidir.

| WS durumu | Açıklama
| --------- | -------------------------------------------------------------------------------
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi.
| 1011      | Hizmette bir sorun oluştu.

#### <a name="accept-handshake"></a>Anlaşma kabul edin

"Kabul" bildirim hizmeti tarafından dinleyici için önceden kurulmuş bir denetim kanalı üzerinden WebSocket metin çerçevesinde bir JSON ileti olarak gönderilir. Bu iletiye yanıt yoktur.

İleti şu anda aşağıdaki özellikleri tanımlayan "kabul" adlı bir JSON nesnesi içerir:

* **Adres** – hizmetine WebSocket kurmak için bir gelen bağlantı kabul etmesi için kullanılan URL dizesi.
* **Kimliği** – Bu bağlantı için benzersiz tanımlayıcı. Kimlik gönderen istemci tarafından sağlandıysa, sağlanan gönderen değeri, aksi takdirde sistem tarafından oluşturulan değeri.
* **connectHeaders** – sn WebSocket protokolü ve sn WebSocket uzantıları üst bilgileri de içeren gönderenin geçiş uç noktaya tarafından sağlanan tüm HTTP üstbilgileri.

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

JSON iletisinde sağlanan adresi URL'si, kabul etme veya reddetme gönderen yuva için WebSocket oluşturmak üzere dinleyici tarafından kullanılır.

##### <a name="accepting-the-socket"></a>Yuva kabul etme

Kabul etmek için dinleyiciyi sağlanan adres WebSocket bağlantısı kurar.

"Kabul" iletisi taşıyorsa bir `Sec-WebSocket-Protocol` üst bilgisi, bu protokolü destekliyorsa, dinleyiciyi yalnızca WebSocket kabul beklenir. Ayrıca, WebSocket belirlenen üstbilgisini ayarlar.

Aynı geçerlidir `Sec-WebSocket-Extensions` başlığı. Uzantı framework destekliyorsa, gerekli'nın sunucu tarafı yanıt üst bilgisi ayarlamalısınız `Sec-WebSocket-Extensions` el sıkışması uzantısı.

URL olarak kullanılması gerekir-kabul yuva oluşturma için olan, ancak aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir yuva kabul etmek için parametre olmalıdır `sb-hc-action=accept`
| `{path}`       | Evet      | (aşağıdaki paragrafı bakın)
| `sb-hc-id`     | Hayır       | Önceki açıklamasına bakın **kimliği**.

`{path}` önceden yapılandırılmış bir karma bağlantı, bu dinleyici kaydetmek ad alanı URL kodlamalı yoludur. Bu ifade eklenir sabit `$hc/` yol bölümü.

`path` İfade genişletilmiş bir sonek ve kayıtlı adı ayıran eğik sonra izleyen bir sorgu dizesi ifadesi.
Bu, HTTP üst bilgilerini eklemek mümkün değilse, gönderme bağımsız değişkenleri kabul dinleyiciye geçirmek gönderen istemci sağlar. Dinleyici framework sabit yol bölümü ve hizmetin kayıtlı adı yolundan ayrıştırır ve önek bir sorgu dizesi bağımsız değişkenler olmadan büyük olasılıkla kalanını yapar beklenir `sb-`, karar için uygulama tarafından kullanılabilir kabul edilip edilmeyeceğini bağlantı.

Daha fazla bilgi için aşağıdaki "Gönderen Protokolü" bölümüne bakın.

Varsa bir hata, bir hizmet gibi yanıtlayabilir:

| Kod | Hata          | Açıklama
| ---- | -------------- | -----------------------------------
| 403  | Yasak      | URL geçerli değil.
| 500  | İç hata | Hizmette bir sorun oluştu

 Bağlantı kurulduktan sonra sunucu WebSocket göndereni aşağı veya aşağıdaki durum kapattığında WebSocket kapatan:

| WS durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.                        |
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |
| 1011      | Hizmette bir sorun oluştu.                                            |

##### <a name="rejecting-the-socket"></a>Yuva reddetme

 Yuva inceleyerek sonra reddetme `accept` gönderene durum kodu ve durum açıklaması ret akış için bir nedenle iletişim geri yönlendirebilmesi benzer bir el sıkışması iletisi gerektirir.

 Protokol tasarım seçiminin (yani tanımlanan hata durumunda sonlanan şekilde tasarlanmıştır) bir WebSocket anlaşması kullanmaktır dinleyicisi istemci uygulamaları bir WebSocket istemcide yararlanmayı devam edebilir ve gerekmeyen fazladan kullanmak istemiyorsunuz, çıplak HTTP istemcisi.

 Yuva reddetmek için istemci, URI adresini alır gelen `accept` iletisi ve iki sorgu dizesi parametreleri gibi ekler:

| param                   | Gerekli | Açıklama                              |
| ----------------------- | -------- | ---------------------------------------- |
| SB hc statusCode        | Evet      | Sayısal HTTP durum kodu.                |
| SB hc Durumaçıklaması | Evet      | İnsan tarafından okunabilir reddedilme. |

Sonuçta elde edilen URI'nin sonra WebSocket bağlantısı kurmak için kullanılır.

Hiçbir WebSocket kurulduktan sonra doğru tamamlarken, bu el sıkışması kasıtlı olarak bir 410, HTTP hata koduyla başarısız oluyor. Bir sorun yaşanırsa, hatayı aşağıdaki kodları açıklanmaktadır:

| Kod | Hata          | Açıklama                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Yasak      | URL geçerli değil.                |
| 500  | İç hata | Hizmette bir sorun oluştu. |

#### <a name="request-message"></a>İstek iletisi

`request` İleti gönderilir hizmet tarafından dinleyici için denetim kanalı üzerinden. Aynı iletiyi de randevu sonra WebSocket gönderilir.

`request` İki bölümden oluşur: bir başlık ve ikili gövde çerçeve(leri).
Hiçbir gövdesi varsa, gövde çerçeveleri çıkarılır. Gövde bulunup bulunmadığını için boolean göstergesidir `body` özelliğinde istek iletisi.

İstek gövdesi ile bir istek için yapı şöyle görünebilir:

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
İkili bir çerçeve FIN bayrağı ayarlanmış alındığında isteği sona erer.

Gövdesiz bir istek için yalnızca bir metin çerçeve yoktur.

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

JSON içeriğinin `request` aşağıdaki gibidir:

* **Adres** -URI dizesi. Bu istek için kullanılacak randevu adresini budur. Gelen istek 64 KB'den büyükse, geri kalanında bu iletiyi boş bırakılır ve istemci, randevu el sıkışması eşdeğer başlatmalıdır `accept` aşağıda açıklanan işlemi. Hizmet daha sonra tam sokar `request` üzerinde kurulu bir Web yuvası. Yanıt 64 kB aşmayı beklenebilir, dinleyici gerekir ayrıca bir randevu anlaşması başlatın ve ardından kurulan Web yuvası yanıtı aktarın.
* **Kimliği** – dize. Bu istek için benzersiz tanımlayıcı.
* **requestHeaders** – bu nesne, uç nokta için yetkilendirme bilgileri bu durumun gönderen tarafından açıklandığı gibi sağlanmadı tüm HTTP üstbilgilerini içeren [yukarıda](#request-operation), kesin olarak ilişkili üstbilgileri ağ geçidi bağlantısı. Tüm üstbilgileri özellikle tanımlanan veya içinde ayrılmış [RFC7230](https://tools.ietf.org/html/rfc7230), dışında `Via`, çıkartılır ve değil iletilen:

  * `Connection` (RFC7230, Bölüm 6.1)
  * `Content-Length`  (RFC7230 3.3.2 bölüm)
  * `Host`  (RFC7230, bölüm 5.4)
  * `TE`  (RFC7230, Bölüm 4.3)
  * `Trailer`  (RFC7230, Bölüm 4.4)
  * `Transfer-Encoding`  (RFC7230 3.3.1 bölümünde)
  * `Upgrade` (RFC7230, Bölüm 6.7)
  * `Close`  (RFC7230, Bölüm 8.1)

* **requestTarget** – dize. Bu özellik tutan [(RFC7230, bölüm 5.3) "istek Target"](https://tools.ietf.org/html/rfc7230#section-5.3) istek. Bu, tüm yapılandırıldıktan sorgu dizesi bölümü içerir `sb-hc-` parametreleri öneki.
* **yöntem** -dize. Bu, istek başına yöntemidir [RFC7231, 4. bölüm](https://tools.ietf.org/html/rfc7231#section-4). `CONNECT` Yöntemi değil kullanılmalıdır.
* **Gövde** : boolean. Bir veya daha fazla ikili gövde çerçeve izlemeyeceğini gösterir.

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

##### <a name="responding-to-requests"></a>İsteklerini yanıtlama

Alıcı yanıt vermesi gerekir. Bağlantı korurken isteklerine yanıt vermek için yinelenen kara listede dinleyicisi neden olabilir.

Herhangi bir sırada yanıtları gönderilebilir, ancak her istek için 60 saniye içinde yanıt gerekir veya teslim başarısız bildirilir. 60 saniye son tarihe kadar sayılır `response` çerçeve hizmet tarafından alınmış. Birden çok ikili çerçeveleri ile devam eden bir yanıtı 60 saniyeden uzun bir süre boş olamaz veya sonlandırılır.

İstek denetim kanalı üzerinden alınan yanıt ya da denetim kanalı isteği burada alındı gönderilmelidir veya randevu kanalı üzerinden gönderilmesi gerekir.

Yanıt "yanıt" adlı bir JSON nesnesidir. Gövde içeriği işleme kuralları ile tıpkı olan `request` iletisi ve temel `body` özelliği.

* **RequestId** – dize. GEREKLİ. `id` Özelliği değerinin `request` için yanıt iletisi.
* **statusCode** – sayı. GEREKLİ. bildirim sonucunu gösteren bir sayısal HTTP durum kodu. Tüm durum kodlarını [RFC7231, 6. bölüm](https://tools.ietf.org/html/rfc7231#section-6) izin verilen dışında [502 "hatalı ağ geçidi"](https://tools.ietf.org/html/rfc7231#section-6.6.3) ve [504 "ağ geçidi zaman aşımı"](https://tools.ietf.org/html/rfc7231#section-6.6.5).
* **Durumaçıklaması** -dize. İSTEĞE BAĞLI. HTTP durum kodu neden ifadesini başına [RFC7230, Bölüm 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)
* **responseHeaders** – bir dış HTTP yanıtında ayarlamak için HTTP üstbilgileri.
  Olduğu gibi `request`, RFC7230 adet tanımlı üst bilgiyle değil kullanılmalıdır.
* **Gövde** : boolean. İkili Gövde follow(s) çerçeve(leri) olup olmadığını gösterir.

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

64 kB'ı aşan yanıtlar için yanıta bir randevu yuva teslim EDİLMELİDİR. Ayrıca, istek 64 kB'ı aşarsa ve `request` yalnızca adres alanı içeren bir randevu yuva edinme kurulması gerekir `request`. Bir randevu yuva kurulduktan sonra devam ettiği sürece ilgili istemciden gelen sonraki istekleri ve ilgili istemci yanıtlarını randevu yuva teslim EDİLMELİDİR.

`address` URL'de `request` olarak kullanılması gerekir-randevu yuva oluşturma için olan, ancak aşağıdaki parametreleri içerir:

| Parametre      | Gerekli | Açıklama
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Evet      | Bir yuva kabul etmek için parametre olmalıdır `sb-hc-action=request`

Varsa bir hata, bir hizmet gibi yanıtlayabilir:

| Kod | Hata           | Açıklama
| ---- | --------------- | -----------------------------------
| 400  | Geçersiz istek | Tanınmayan bir eylem veya URL geçerli değil.
| 403  | Yasak       | URL'nin süresi doldu.
| 500  | İç hata  | Hizmette bir sorun oluştu

 Bağlantı kurulduktan sonra sunucu istemcinin HTTP yuva kapatıldığında veya aşağıdaki durum WebSocket kapatan:

| WS durumu | Açıklama                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | Gönderen istemci bağlantıyı kapatır.                                    |
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.                        |
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |
| 1011      | Hizmette bir sorun oluştu.                                            |


#### <a name="listener-token-renewal"></a>Dinleyici belirteci yenileme

Dinleyici belirtecin süresi dolmak üzere olduğunda, bu yerleşik denetim kanalı aracılığıyla hizmet çerçeve mesaj göndererek değiştirebilirsiniz. İleti adlı bir JSON nesnesi içeren `renewToken`, şu anda aşağıdaki özelliği tanımlar:

* **belirteç** – ad alanı veya confers karma bağlantı için geçerli, URL kodlamalı bir Service Bus paylaşılan erişim belirteci **dinleme** doğru.

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
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi. |

### <a name="web-socket-connect-protocol"></a>Web yuvası bağlama Protokolü

Gönderen protokolü bir dinleyici kurulur şekilde etkili bir şekilde aynıdır.
En fazla saydamlık için uçtan uca WebSocket hedeftir. Bağlanmak için adresini dinleyici ile aynıdır, ancak "action" farklıdır ve belirteç farklı bir izni gerekiyor:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

_Ad alanı adresi_ karma bağlantı, genellikle formun barındıran Azure geçiş ad alanı tam etki alanı adıdır `{myname}.servicebus.windows.net`.

İstek, uygulama tanımlı olanlar da dahil olmak üzere, rastgele ek HTTP üstbilgileri içerebilir. Tüm sağlanan üstbilgileri akış dinleyiciye ve bulunabilir `connectHeader` nesnesinin **kabul** denetim iletisi.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir:

| param          | Gerekli mi? | Açıklama
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Evet       | Gönderen rolü için bir parametresi olmalıdır `sb-hc-action=connect`.
| `{path}`       | Evet       | (aşağıdaki paragrafı bakın)
| `sb-hc-token`  | Evet\*     | Dinleyici bir geçerli URL olarak kodlanmış, Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **Gönder** doğru.
| `sb-hc-id`     | Hayır        | Uçtan uca tanılama izlemesi sağlar ve dinleyiciye kabul anlaşması sırasında sunulan isteğe bağlı bir kimliği.

 `{path}` Önceden yapılandırılmış bir karma bağlantı, bu dinleyici kaydetmek ad alanı URL kodlamalı yoludur. `path` İfadesi uzatabilirsiniz bir son eki ile daha fazla iletişim kurmak için bir sorgu dizesi ifadesi. Karma bağlantı yolu altında kayıtlı değilse `hyco`, `path` ifade olabilir `hyco/suffix?param=value&...` burada tanımlanan sorgu dizesi parametreleri tarafından izlenen. Tam bir ifadeyi aşağıdaki gibi olabilir:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

`path` İfade geçirildiğinde aracılığıyla "kabul" denetimi iletisinde bulunan URI adresini dinleyici.

WebSocket bağlantısı kayıtlı karma bağlantı yolu, geçersiz veya eksik bir belirteç veya başka bir hata nedeniyle başarısız olursa, hata geri bildirim normal HTTP 1.1 durumu geri bildirim modeli kullanılarak sağlanır. Durum açıklaması, Azure destek personelinin izleme bildirilebilmesi kimliği bir hata içeriyor:

| Kod | Hata          | Açıklama
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Bulunamadı      | Karma bağlantı yolu geçersiz veya temel URL yanlış biçimlendirilmiş.
| 401  | Yetkilendirilmemiş   | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz.
| 403  | Yasak      | Güvenlik belirteci, bu yol için ve bu eylem için geçerli değil.
| 500  | İç hata | Hizmette bir sorun oluştu.

Kurulum, izleme kimliği de içeren bir açıklayıcı hata iletisi ile birlikte uygun bir WebSocket protokolü hatası kodu kullanarak bildiriliyor böylece nedeni başlangıçta ayarlandıktan sonra WebSocket bağlantısı kasıtlı olarak hizmet tarafından kapatılırsa .

| WS durumu | Açıklama
| --------- | ------------------------------------------------------------------------------- 
| 1000      | Dinleyici yuva kapatıldı.
| 1001      | Karma bağlantı yolu silinmiş veya devre dışı.
| 1008      | Güvenlik belirtecinin süresi doldu, bu nedenle yetkilendirme ilkesi ihlal edildi.
| 1011      | Hizmette bir sorun oluştu.

### <a name="http-request-protocol"></a>HTTP isteği Protokolü

HTTP isteği protokol Protokolü yükseltmeleri dışında rastgele HTTP istekleri sağlar.
HTTP isteklerini, karma bağlantılar WebSocket istemcileri için kullanılan $hc içtakı olmadan varlığın normal çalışma zamanı adresine işaret.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

_Ad alanı adresi_ karma bağlantı, genellikle formun barındıran Azure geçiş ad alanı tam etki alanı adıdır `{myname}.servicebus.windows.net`.

İstek, uygulama tanımlı olanlar da dahil olmak üzere, rastgele ek HTTP üstbilgileri içerebilir. Üst bilgiler, doğrudan RFC7230 içinde tanımlanmış olanlar dışında tüm sağlanan (bkz [istek iletisi](#Request message)) akış dinleyiciye ve bulunabilir `requestHeader` nesnesinin **isteği** ileti.

Sorgu dizesi parametresi seçenekleri aşağıdaki gibidir:

| param          | Gerekli mi? | Açıklama
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Evet\*     | Dinleyici bir geçerli URL olarak kodlanmış, Service Bus paylaşılan erişim belirteci ad alanı veya confers karma bağlantı için sağlamalısınız **Gönder** doğru.

Belirteç de ya da aktarılabilen `ServiceBusAuthorization` veya `Authorization` HTTP üstbilgisi. Karma bağlantı anonim isteklere izin vermek üzere yapılandırılmışsa, belirteç atlanabilir.

Bile ya da ekler değil gerçek HTTP proxy olarak hizmet etkili bir şekilde bir proxy olarak çalışır çünkü bir `Via` üst bilgi veya var olan açıklama ekler `Via` üst bilgisi ile uyumlu [RFC7230, bölüm 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1).
Geçiş ad alanı ana bilgisayar adı için bir hizmet ekler `Via`.

| Kod | `Message`  | Açıklama                    |
| ---- | -------- | ------------------------------ |
| 200  | Tamam       | İstek, en az bir dinleyici tarafından işlendi.  |
| 202  | Kabul edildi | En az bir dinleyici tarafından istek kabul edildi. |

Varsa bir hata, hizmet gibi yanıtlayabilir. Yanıt hizmetinden veya dinleyici olup gelmektedir varlığını tanımlanabilir `Via` başlığı. Yanıt üstbilgisi mevcutsa, dinleyicisinden.

| Kod | Hata           | Açıklama
| ---- | --------------- |--------- |
| 404  | Bulunamadı       | Karma bağlantı yolu geçersiz veya temel URL yanlış biçimlendirilmiş.
| 401  | Yetkilendirilmemiş    | Güvenlik belirteci eksik veya hatalı biçimlendirilmiş veya geçersiz.
| 403  | Yasak       | Güvenlik belirteci, bu yol için ve bu eylem için geçerli değil.
| 500  | İç hata  | Hizmette bir sorun oluştu.
| 503  | Hatalı Ağ Geçidi     | İstek için tüm dinleyici yönlendirilmesini değil.
| 504  | Ağ geçidi zaman aşımı | İstek için bir dinleyici yönlendirildi ancak dinleyicisi giriş gerekli zamanında kabul etmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Geçiş hakkında SSS](relay-faq.md)
* [Ad alanı oluşturma](relay-create-namespace-portal.md)
* [.NET kullanmaya başlama](relay-hybrid-connections-dotnet-get-started.md)
* [Node kullanmaya başlama](relay-hybrid-connections-node-get-started.md)
