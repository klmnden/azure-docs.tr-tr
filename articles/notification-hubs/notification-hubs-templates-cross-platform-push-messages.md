---
title: Şablonlar
description: Bu konu, şablonları Azure bildirim hub'ları için açıklar.
services: notification-hubs
documentationcenter: .net
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 3e587bdf0efc7c5b416183640abb19286a5cff31
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33776660"
---
# <a name="templates"></a>Şablonlar
## <a name="overview"></a>Genel Bakış
Şablonları almak istediği bildirim tam biçimini belirtmek bir istemci uygulaması etkinleştirin. Şablonları kullanarak bir uygulama aşağıdakiler de dahil olmak üzere birkaç farklı faydaları hayata geçirmek:

* Platform belirsiz arka uç
* Kişiselleştirilmiş bildirimler
* İstemci sürümü bağımsızlığı
* Kolay yerelleştirme

Bu bölümde platformlar genelinde tüm aygıtlarınızın hedefleme platform belirsiz bildirimleri göndermek için ve her aygıt için yayın bildirim kişiselleştirmek için şablonları kullanma iki ayrıntılı örnekler sağlar.

## <a name="using-templates-cross-platform"></a>Şablonları platformlar arası kullanma
Anında iletme bildirimleri göndermek için standart gönderilmesi için belirli bir yükü platform Bildirim Hizmetleri (WNS, APNS) için her bir bildirim göndermek için yoludur. Örneğin, APNS için bir uyarı göndermek için aşağıdaki biçimde bir Json nesnesi yükü şöyledir:

    {"aps": {"alert" : "Hello!" }}

Bir Windows mağazası uygulaması benzer bir bildirim iletisi göndermek için bir XML yükü aşağıdaki gibidir:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

MPNS (Windows Phone) ve GCM (Android) platformlar için benzer yükü oluşturabilirsiniz.

Bu gereksinim, her platform için farklı yüklerini üretmek için uygulama arka ucu zorlar ve etkili bir şekilde arka uç uygulamasının sunu katmanı parçası sorumlu yapar. Bazı sorunları yerelleştirme ve grafik düzenleri (döşeme çeşitli türleri için bildirimleri içeren özellikle Windows mağazası uygulamaları için) içerir.

Bildirim hub'ları şablon özelliği, bir şablon etiketleri kümesi yanı sıra içerir şablon kayıtlar adlı özel kayıtları oluşturmak için bir istemci uygulaması sağlar. Bildirim hub'ları şablon özelliği (tercih edilen) yüklemeleri veya kayıtlar ile çalışıyorsanız olup olmadığını aygıtları şablonları ile ilişkilendirmek için bir istemci uygulaması sağlar. Önceki bir yükü örnekler verildiğinde, platformdan bağımsız olarak yalnızca gerçek uyarı iletisi (Merhaba!) bilgilerdir. Bir şablonu, bu özel istemci uygulaması kaydını platformdan bağımsız iletiyi biçimlendirmek nasıl bildirim hub'ına yönelik yönergeler kümesidir. Önceki örnekte, tek bir özellik platformdan bağımsız iletisidir: **ileti Hello =!**.

Aşağıdaki resimde işlem gösterilmektedir:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

İOS istemci uygulamasını kaydı için şablonu aşağıdaki gibidir:

    {"aps": {"alert": "$(message)"}}

Windows mağazası istemci uygulaması için karşılık gelen şablon şöyledir:

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

Gerçek ileti geçmesidir olduğunu ifade $(ileti) dikkat edin. Bu ifade ve ortak değeri anahtarları izleyen bir ileti oluşturmak için bu belirli kayıt bir ileti gönderir her bildirim hub'ı bildirir.

Yükleme modeliyle çalışıyorsanız, birden çok şablonlarının JSON yükleme "Şablon" anahtarı saklar. Kayıt modeliyle çalışıyorsanız, istemci uygulaması birden fazla şablon kullanmak için birden çok kayıt oluşturabilir; Örneğin, uyarı iletileri için bir şablon ve bölme için bir şablon güncelleştirir. İstemci uygulamaları, yerel kayıtları (kayıtlar herhangi bir şablon ile) ve şablon kayıtlar de karıştırabilirsiniz.

Bildirim hub'ı aynı istemci uygulamasına ait düşünmeden her şablon için bir bildirim gönderir. Bu davranış, daha fazla bildirimleri platformdan bağımsız bildirimlerini çevirmek için kullanılabilir. Örneğin, aynı platformdan bağımsız iletinin bildirim hub'ına sorunsuz bir bildirim uyarı ve döşeme Güncelleştirmesi'nde bu durumdan haberdar olmasını arka uç gerek kalmadan çevrilebilir. Kısa bir süre içinde gönderirse bazı platformlar (örneğin, iOS) aynı cihaza birden fazla bildirim daraltın.

## <a name="using-templates-for-personalization"></a>Kişiselleştirme için şablonları kullanma
Şablonları kullanarak başka bir avantajı kayıt başına kişiselleştirme bildirimler gerçekleştirmek için Notification Hubs'ı kullanma becerisini ' dir. Örneğin, belirli bir konumda hava koşulları içeren bir kutucuk görüntüleyen bir hava durumu uygulama göz önünde bulundurun. Bir kullanıcı derece veya Fahrenhayt derece ve tek veya beş günlük bir tahmini arasında seçim yapabilirsiniz. Şablonları kullanarak her bir istemci uygulama yükleme için gereken biçimi kaydedebilirsiniz (1 gün Celsius, 1 gün Fahrenhayt, 5 gün derece, 5 gün Fahrenhayt), ve bu şablonları doldurmak için gerekli tüm bilgileri içeren tek bir ileti gönderme arka uç (örneğin, bir beş derece ve Fahrenhayt derece ile tahmin gün).

Şablon için bir günlük tahmin etme ile derece şu şekildedir:

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

Bildirim Hub'ına gönderilen ileti aşağıdaki özellikleri içerir:

<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>

Bu yöntemi kullanarak, arka uç yalnızca tek bir ileti uygulama kullanıcılar için belirli kişiselleştirme seçenekleri depolamak zorunda kalmadan gönderir. Aşağıdaki resimde bu senaryo gösterilmektedir:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Şablonları kaydetme
(Tercih edilen) yükleme modeli veya kayıt modelini kullanarak şablonları ile kaydetmek için bkz: [kayıt yönetimi](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Şablon ifade dili
Şablonlar, XML veya JSON belge biçimlerine sınırlıdır. Ayrıca, belirli yerlerde ifadeleri yalnızca koyabilirsiniz; örnek, düğüm öznitelikleri veya XML değerleri için özellik değerleri için JSON dizesi.

Aşağıdaki tabloda şablonlarında izin dil gösterilmektedir:

| İfade | Açıklama |
| --- | --- |
| $(prop) |Verilen ada sahip bir olay özellik referansı. Özellik adları büyük küçük harfe duyarlı değildir. Özellik mevcut değilse bu ifade özelliğin metin değeri veya boş bir dize çözümler. |
| $(prop, n) |Yukarıdaki gibi ancak açıkça metindir n karakter kırpılmış, örneğin $(başlık, 20) title özelliğini içeriğini 20 karakter kırpar. |
| . (prop, n) |Yukarıdaki gibi ancak bunu kırpılmış gibi metin ile üç noktaya sonekine. Kırpılmış dize ve sonek toplam boyutu n karakteri aşmayan. . (başlık, 20) "Bu başlık satırıdır" sonuçlarında bir giriş özelliğiyle **başlığı budur...** |
| %(prop) |Benzer şekilde $(name) çıkış URI kodlanır dışında. |
| #(prop) |JSON şablonları (örneğin, iOS ve Android şablonları için) kullanılır.<br><br>Bu işlev tam olarak aynı daha önce JSON şablonları (örneğin, Apple Şablonları) kullanılması dışında belirtilen $(prop) çalışır. Bu işlev tarafından çevrelenen değil, bu durumda, "{','}" (örneğin, 'myJsonProperty': '#(ad)'), ve onu, Javascript biçiminde bir sayı Örneğin, regexp değerlendirir: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*)) (\. &#91;0-9&#93;+)? ((e&#124;E) (+&#124;-)? &#91;0-9&#93;+)?, sonra da çıktıyı JSON bir sayıdır.<br><br>Örneğin, ' rozet: '#(ad)' hale 'göstergeye': 40 (ve değil '40'). |
| 'text' veya "metin" |Bir hazır değer. Değişmez değerler tek veya çift tırnak içine alınmış rastgele metin içerir. |
| Expr1 + expr2 |Tek bir dize iki ifadelere birleştirme birleştirme işleci. |

Deyimler önceki biçimlerden birini olabilir.

İle birleştirme kullanırken, tüm deyimin alınmalıdır {}. Örneğin, {$(prop) + '-' + $(prop2)}. |

Örneğin, aşağıdaki şablonu geçerli bir XML şablon değil:

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


Birleştirme kullanırken, daha önce açıklandığı gibi ifadeler süslü parantez içine alınmalıdır. Örneğin:

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

