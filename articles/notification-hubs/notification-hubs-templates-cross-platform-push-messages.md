---
title: Şablonlar
description: Bu konuda Azure bildirim hub'ları için şablonları açıklanmaktadır.
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 02473eb5649c7d201b6a54fd57faea997c1a21cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60872114"
---
# <a name="templates"></a>Şablonlar

Şablonları almak istediği bildirim tam biçimini belirtmek için bir istemci uygulaması sağlar. Şablonları kullanarak bir uygulama aşağıdakiler dahil olmak üzere birçok farklı avantajı hayata geçirebilirsiniz:

- Platformdan bağımsız arka uç
- Kişiselleştirilmiş bildirimler
- İstemci sürümü bağımsızlığı
- Kolay yerelleştirme

Bu bölüm, tüm cihazlarınızı platformlar arasında hedefleyen platformu belirsiz bildirimler gönderin ve her cihaz için yayın bildirim kişiselleştirmek için şablonları kullanma iki ayrıntılı örnekler sağlar.

## <a name="using-templates-cross-platform"></a>Şablonları platformlar arası kullanma

Anında iletme bildirimleri göndermek için standart gönderilmesi için platform bildirim hizmetlerine (WNS, APNS) için belirli bir yükü her bir bildirim göndermek için yoludur. Örneğin, APNS için bir uyarı göndermek için aşağıdaki biçimde bir JSON nesnesi yükü şöyledir:

```json
{"aps": {"alert" : "Hello!" }}
```

Bir Windows Store uygulaması üzerinde benzer bir bildirim iletisi göndermek için XML yükünü aşağıdaki gibidir:

```xml
<toast>
  <visual>
    <binding template=\"ToastText01\">
      <text id=\"1\">Hello!</text>
    </binding>
  </visual>
</toast>
```

MPNS (Windows Phone) ve FCM (Android) platformlar için benzer yükü oluşturabilirsiniz.

Bu gereksinim, her platform için farklı yüklerini üretmeye uygulama arka ucu zorlar ve etkili bir şekilde arka uç sunu katmanı uygulama bölümü için sorumlu hale getirir. Bazı sorunlar, yerelleştirme ve grafik düzenleri (kutucuk çeşitli türleri için bildirimleri içeren özellikle Windows Store uygulamaları için) içerir.

Bildirim hub'ları şablon özelliği, bir şablon yanı sıra etiketler kümesini içeren şablon kayıtları olarak adlandırılan özel kayıtları oluşturmak bir istemci uygulaması sağlar. Bildirim hub'ları şablon özelliği (tercih edilir) yüklemeleri veya kayıtları ile çalışıyor olmanızdan cihazları şablonları ile ilişkilendirmek bir istemci uygulaması sağlar. Önceki yük örnekler verildiğinde, platformdan bağımsız olarak yalnızca gerçek uyarı iletisi (Merhaba!) bilgilerdir. Yönergeler için belirli bir istemci uygulaması kaydını bir platformdan bağımsız ileti biçimine bildirim hub'ı için bir dizi şablonudur. Önceki örnekte, tek bir özellik platformdan bağımsız iletidir: `message = Hello!`.

Aşağıdaki resimde, işlem gösterilmektedir:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

İOS istemci uygulamasını kaydı için şablon aşağıdaki gibidir:

```json
{"aps": {"alert": "$(message)"}}
```

Windows Store istemci uygulaması için karşılık gelen şablon şöyledir:

```xml
<toast>
    <visual>
        <binding template=\"ToastText01\">
            <text id=\"1\">$(message)</text>
        </binding>
    </visual>
</toast>
```

Gerçek ileti konur, ifade $(mesaj) dikkat edin. Bu ifade ve ortak değeri anahtarları izleyen bir ileti oluşturmak için bu belirli kayıt için bir ileti gönderdiğinde bildirim hub'ı bildirir.

Yükleme modeli ile çalışıyorsanız, birden fazla şablon bir JSON yüklemesi "Şablon" anahtar tutar. Kayıt modeliyle çalışıyorsanız, istemci uygulama birden fazla şablon kullanmak için birden çok kayıtları oluşturabilirsiniz; Örneğin, uyarı iletileri için bir şablon ve kutucuk için bir şablon güncelleştirir. İstemci uygulamaları, yerel kayıtları (kayıtları hiçbir şablon ile) ve şablon kayıtları da karıştırabilirsiniz.

Bildirim hub'ı aynı istemci uygulamasına ait olup olmadığını düşünmeden her şablon için bir bildirim gönderir. Bu davranış, daha fazla bildirim içine platformdan bağımsız bildirimler çevirmek için kullanılabilir. Örneğin, aynı platformdan bağımsız ileti bildirim Hub'ına sorunsuz bir şekilde bir bildirim uyarı ve bir kutucuk güncelleştirmesi, arka uç, bu durumun farkında olmasını gerektirmeden çevrilebilir. Kısa bir süre içinde gönderiliyorsa bazı platformlarda (örneğin, iOS) aynı cihaza birden çok bildirim Daralt.

## <a name="using-templates-for-personalization"></a>Kişiselleştirme için şablonları kullanma

Şablonları kullanarak başka bir avantajı bildirimler kayıt başına kişiselleştirme gerçekleştirmek için Notification Hubs'ı kullanma yeteneğidir. Örneğin, belirli bir konumda hava koşulları içeren bir kutucuk görüntüleyen bir hava durumu uygulama göz önünde bulundurun. Bir kullanıcı Santigrat veya Fahrenhayt derece ve tek veya beş günlük bir tahmin arasında seçim yapabilirsiniz. Şablonları kullanarak, her istemci uygulaması yüklemesi için gereken biçimde kaydedebilirsiniz (1 gün Celsius, 1 gün Fahrenhayt, 5 gün Santigrat, 5 gün Fahrenhayt), ve bu şablonları doldurmak için gerekli tüm bilgileri içeren tek bir ileti gönderme arka uç (örneğin, Santigrat ve Fahrenhayt derece ile tahmini bir beş gün).

Şablon için bir günlük hava durumu tahminini Santigrat Sıcaklıkların aşağıdaki gibidir:

```xml
<tile>
  <visual>
    <binding template="TileWideSmallImageAndText04">
      <image id="1" src="$(day1_image)" alt="alt text"/>
      <text id="1">Seattle, WA</text>
      <text id="2">$(day1_tempC)</text>
    </binding>  
  </visual>
</tile>
```

Bildirim Hub'ına gönderilen ileti, aşağıdaki özellikleri içerir:

```html
<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>
```

Bu düzeni kullanarak, arka uç yalnızca tek bir ileti uygulama kullanıcılarının belirli kişiselleştirme seçeneklerini depolamak zorunda kalmadan gönderir. Aşağıdaki resimde, bu senaryo gösterilmektedir:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Şablonları kaydetme

Yükleme modeli (tercih edilen) veya kayıt modelini kullanarak şablonları ile kaydetmek için bkz: [kayıt yönetimi](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Şablon ifade dili

Şablonlar, XML veya JSON belge biçimlerine sınırlıdır. Ayrıca, belirli yerlerde ifadeleri yalnızca yerleştirebilirsiniz; örnek, düğüm öznitelikleri veya XML değerleri, özellik değerleri için JSON dizesi.

Aşağıdaki tabloda şablonlarında izin dil gösterilmektedir:

| İfade       | Açıklama |
| ---------------- | --- |
| $(prop)          | Belirtilen ada sahip bir olay özelliğine başvuru. Özellik adlarını büyük küçük harfe duyarlı değildir. Özellik mevcut değilse bu ifade özelliğin metin değeri veya boş bir dize olarak çözümleniyor. |
| $(prop, n)       | Yukarıdaki gibi ancak açıkça metindir n karakter kırpılarak, örneğin $(başlık, 20) title özelliğini içeriğini 20 karakter kırpar. |
| . (prop, n)       | Yukarıdaki gibi ancak kırpılmış olsun gibi metin ile üç noktaya soneki. Sabitlenmiş dize ve soneki toplam boyutu n karakterden uzun olmamalıdır. . (title, 20) bir giriş özelliğiyle "Bu bir başlık satırı" sonuçlarında **bu başlıktır...** |
| %(prop)          | Benzer şekilde gt;$(Name) çıkış URI ile kodlanmış olması dışında. |
| #(prop)          | JSON şablonları (örneğin, iOS ve Android şablonları için) kullanılır.<br><br>Bu işlev, aynı $(prop) daha önce JSON şablonları (örneğin, Apple şablonlar) kullanılması dışında belirtilen şekilde çalışır. Bu işlev tarafından çevrelenen değil, bu durumda, "{','}" (örneğin, 'myJsonProperty': '(ad) #'), ve onu Javascript biçiminde bir sayı gibi regexp değerlendirir: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*)) (\.&#91;0-9&#93;+)? ((e&#124;E) (+&#124;-)? &#91;0-9&#93;+)?, sonra da JSON çıkışındaki bir sayıdır.<br><br>Örneğin, ' rozet: '#(ad)', 'rozet' olur: 40 (ve '40' değil). |
| 'text' ya da "text" | Bir sabit değer. Değişmez değerler, tek veya çift tırnak işaretleri içine rastgele metin içerir. |
| Deyim1 + Deyim2    | İki ifadeleri tek bir dizede birleştirmek birleştirme işleci. |

İfadeler, önceki biçimlerden birini olabilir.

İle birleştirme kullanırken, tüm ifade alınmalıdır `{}`. Örneğin, `{$(prop) + ‘ - ’ + $(prop2)}`.

Örneğin, aşağıdaki şablonu geçerli bir XML şablon değil:

```xml
<tile>
  <visual>
    <binding $(property)>
      <text id="1">Seattle, WA</text>
    </binding>  
  </visual>
</tile>
```

Birleştirme kullanırken daha önce açıklandığı gibi ifadeler kaşlı ayraçlar içine alınmalıdır. Örneğin:

```xml
<tile>
  <visual>
    <binding template="ToastText01">
      <text id="1">{'Hi, ' + $(name)}</text>
    </binding>  
  </visual>
</tile>
```
