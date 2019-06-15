---
title: Twilio ses ve SMS (Java) için kullanma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. Java dilinde yazılan kod örneklerini.
services: ''
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: f3508965-5527-4255-9d51-5d5f926f4d43
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 386b4b8440c74f6599e7147996b5843ea0f67e68
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60623961"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Ses ve SMS özellikleri Java için Twilio kullanma
Bu kılavuzda, Azure üzerinde Twilio API'si hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Telefon görüşmesi yapma ve kısa mesaj servisi (SMS) ileti gönderme senaryoları ele alınmaktadır. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz. [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses ve SMS uygulamaları oluşturmak için mevcut web dilleri ve becerilerinizi kullanmanıza olanak sağlayan bir telefon web hizmeti API'dir. Twilio, (bir Azure özelliğidir ve Microsoft Ürün değil) bir üçüncü taraf hizmetidir.

**Twilio ses** yapıp telefon çağrılarını almak, uygulamaların sağlar. **Twilio SMS** yapmak ve SMS mesajları uygulamalarınızı sağlar. **Twilio istemci** uygulamalarınızı mobil bağlantılar da dahil olmak üzere var olan Internet bağlantılarını kullanarak ses iletişimine olanak tanır.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Twilio fiyatlandırması hakkında bilgi şu adreste [Twilio fiyatlandırma][twilio_pricing]. Azure müşterileri alır bir [özel teklif][special_offer]: ücretsiz kredi 1000 metinlerinin veya 1000 dakika sayısı. Bu teklif için kaydolun veya daha fazla bilgi edinmek için lütfen [ https://ahoy.twilio.com/azure ] [ special_offer].

## <a id="Concepts"></a>Kavramları
Twilio ses ve SMS işlevselliğini uygulamaları için sağlayan bir RESTful API API'dir. Birden fazla dilde istemci kitaplıkları vardır; bir liste için bkz. [Twilio API kitaplıkları][twilio_libraries].

Twilio API'si önemli yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) var.

### <a id="Verbs"></a>Twilio Verbs
Twilio'yu kullanarak API yapar; fiiller Örneğin, **&lt;Say&gt;** fiil kullanımı bir çağrıda bir iletiyi teslim etmek için Twilio bildirir.

Twilio fiillerin listesi verilmiştir.

* **&lt;Arama&gt;** : Çağıran, başka bir telefonu bağlanır.
* **&lt;Gather&gt;** : Telefon tuş takımında girilen sayı toplar.
* **&lt;Kapat&gt;** : Bir çağrı sona erer.
* **&lt;Play&gt;** : Ses dosyası yürütülür.
* **&lt;Kuyruk&gt;** : Ekleme çağıranlar kuyruğuna.
* **&lt;Duraklatma&gt;** : Sessiz bir şekilde belirtilen sayıda saniye bekler.
* **&lt;Kayıt&gt;** : Arayanın ses kayıtlarını ve kayıt içeren dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;** : Arama veya SMS için farklı bir URL'de TwiML aktarımları denetim.
* **&lt;Reddetme&gt;** : Faturalama olmadan Twilio numaranızı için gelen bir çağrıyı reddeder.
* **&lt;Söyleyin&gt;** : Metin, üzerinde bir çağrı yapılır okuma dönüştürür.
* **&lt;SMS&gt;** : Bir SMS mesajı gönderir.

### <a id="TwiML"></a>TwiML
TwiML çağrı işlemek nasıl Twilio veya SMS konusunda bilgilendiren Twilio fiilleri XML tabanlı yönergeleri kümesidir.

Örneğin, aşağıdaki TwiML metin dönüştürecektir **Merhaba Dünya!** Konuşma.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Uygulamanız için Twilio API'sini çağırdığında, API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amacıyla, Twilio tarafından sağlanan URL uygulamalarınız tarafından kullanılan TwiML yanıtlar sağlamak için kullanabilirsiniz. TwiML yanıtları üretmek için kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır **TwiMLResponse** nesne.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API'si][twilio_api].

## <a id="CreateAccount"></a>Twilio hesap oluşturma
Twilio hesap almak hazır olduğunuzda, adresinde kaydolun [deneyin Twilio][try_twilio]. Ücretsiz bir hesapla yeniden başlayın ve daha sonra hesabınızı yükseltin.

Twilio hesabınız için kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Hem Twilio API çağrıları gerçekleştirmek için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvenli tutun. Hesap Kimliği ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio konsol][twilio_console], etiketli alanları **hesap SID'si** ve **AUTH TOKEN**sırasıyla.

## <a id="create_app"></a>Bir Java uygulaması oluşturma
1. Twilio JAR edinin ve, Java derleme yolu ve WAR dağıtım derlemenize ekleyin. Konumunda [ https://github.com/twilio/twilio-java ] [ twilio_java], GitHub kaynakları indirmek ve kendi JAR oluşturmak veya önceden oluşturulmuş bir JAR (ile veya bağımlılıkları olmadan) indirin.
2. JDK'ın olun **cacerts** keystore MD5 parmak izi 67:CB:9 D ile Equifax güvenli sertifika yetkilisi sertifika içeriyor: (seri numarası, 35:DE:F4:CF ve SHA1 C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 parmak izi olan D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Bu sertifika yetkilisi (CA) sertifikası, [ https://api.twilio.com ] [ twilio_api_service] tanımak için Twilio API'lerini kullanırken çağrılan hizmet. JDK'ın sağlama hakkında bilgi için **cacerts** keystore doğru CA sertifikası varsa, bkz: [bir sertifika eklemek için Java CA sertifika Store][add_ca_cert].

Java için Twilio istemci kitaplığını kullanmaya yönelik ayrıntılı yönergeleri [nasıl bir telefon araması Twilio kullanarak azure'da bir Java uygulaması görünebileceğini][howto_phonecall_java].

## <a id="configure_app"></a>Twilio kitaplıkları kullanmak için uygulamanızı yapılandırma
Kodunuzun içinde eklediğiniz **alma** Twilio paketleri veya uygulamanızda kullanmak istediğiniz sınıfları için kaynak dosyalarını üst kısmındaki deyimleri.

Java kaynak dosyaları için:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Java sunucu sayfası (JSP) için kaynak dosyaları:

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Hangi Twilio paketleri veya sınıflar bağlı olarak kullanmak istediğiniz, **alma** deyimleri farklı olabilir.

## <a id="howto_make_call"></a>Nasıl Yapılır: Giden bir çağrı yapın
Aşağıdaki çağrıda giden hale getirmeyi açıklayan **çağrı** sınıfı. Bu kod, Twilio tarafından sağlanan bir site ayrıca Twilio biçimlendirme dili (TwiML) yanıt için kullanır. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kodu çalıştırmadan önce Twilio hesabı için telefon numarası.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("https://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

İçin geçirilen parametreler hakkında daha fazla bilgi için **Call.creator** yöntemi bkz [ https://www.twilio.com/docs/api/rest/making-calls ] [ twilio_rest_making_calls].

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için [TwiML yanıtlarını azure'da bir Java uygulaması sağlamak için nasıl](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl Yapılır: Bir SMS iletisi gönderin
Aşağıdakileri kullanarak bir SMS iletisi göndermek nasıl gösterir **ileti** sınıfı. **Gelen** numarası **4155992671**, SMS mesajları gönderebilir tarafından deneme hesapları için Twilio sağlanır. **İçin** numarası doğrulandı, kodu çalıştırmadan önce Twilio hesabınız için.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Declare To and From numbers and the Body of the SMS message
    PhoneNumber to = new PhoneNumber("+14159352345"); // Replace with a valid phone number for your account.
    PhoneNumber from = new PhoneNumber("+14158141829"); // Replace with a valid phone number for your account.
    String body = "Where's Wallace?";

    // Create a Message creator passing From, To and Body values
    // then send the SMS message by calling the create() method
    Message sms = Message.creator(to, from, body).create();
```

İçin geçirilen parametreler hakkında daha fazla bilgi için **Message.creator** yöntemi bkz [ https://www.twilio.com/docs/api/rest/sending-sms ] [ twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Nasıl Yapılır: Kendi Web sitesinden TwiML yanıtları sağlayın
Ne zaman uygulamanızı başlatan Twilio API'sine çağrıda örneğin aracılığıyla **CallCreator.create** yöntemi, Twilio gönderecek isteğiniz TwiML yanıt dönmesi beklenen bir URL. Yukarıdaki örnekte, Twilio tarafından sağlanan URL'yi kullanır [ https://twimlets.com/message ] [ twimlet_message_url]. (TwiML Web Hizmetleri tarafından kullanılmak üzere tasarlandığından, TwiML tarayıcınızda görüntüleyebilirsiniz. Örneğin, [ https://twimlets.com/message ] [ twimlet_message_url] boş görmek için **&lt; yanıt&gt;** öğesi; başka bir örnek olarak, tıklayın [ https://twimlets.com/message?Message%5B0%5D=Hello%20World%21 ] [ twimlet_message_url_hello_world] görmek için bir **&lt; yanıt&gt;** öğesini içeren bir **&lt; Say&gt;** öğesi.)

Twilio tarafından sağlanan URL üzerinde işlemine güvenmek yerine, HTTP yanıtlarını döndürür kendi URL site oluşturabilirsiniz. Sitenin HTTP yanıtlarını döndüren herhangi bir dilde oluşturabilirsiniz; Bu konuda, bir JSP sayfası URL'de barındırma varsayılır.

Aşağıdaki JSP sayfası sonuçları bildiren bir TwiML yanıtına **Merhaba Dünya!** arama üzerinde.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

Metin diyor, birkaç duraklatır ve Twilio API sürümü ve Azure rol adı hakkında bilgi belirten bir TwiML yanıt aşağıdaki JSP sayfası sonuçlanır.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello from Azure!</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>
```

**ApiVersion** parametresi, Twilio ses istekler (SMS istek değil) kullanılabilir. Kullanılabilir İstek parametreleri için Twilio ses ve SMS isteklerini görmek için bkz: <https://www.twilio.com/docs/api/twiml/twilio_request> ve <https://www.twilio.com/docs/api/twiml/sms/twilio_request>sırasıyla. **RoleName** ortam değişkeni kullanılabilir Azure dağıtımın bir parçası olarak. (Bunlar gelen çekilmesi böylece özel ortam değişkenleri eklemek istiyorsanız **System.getenv**, ortam değişkenleri bölümüne bakın [çeşitli rol yapılandırma ayarları] [ misc_role_config_settings].)

TwiML yanıt vermek şekilde ayarlamanız, JSP sayfası oluşturduktan sonra URL'yi içine geçirilen olarak JSP sayfası URL'sini kullanmak **Call.creator** yöntemi. Örneğin, adlı bir Web uygulaması varsa bir Azure projesi MyTwiML barındırılan hizmeti ve mytwiml.jsp JSP sayfası adıdır, URL geçirilebilir **Call.creator** aşağıda gösterildiği gibi:

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Aracılığıyla TwiML ile yanıt için başka bir seçenek, **VoiceResponse** kullanılabilir sınıfı **com.twilio.twiml** paket.

Twilio Azure'u Java ile kullanma hakkında ek bilgi için bkz: [nasıl bir telefon araması Twilio kullanarak azure'da bir Java uygulaması görünebileceğini][howto_phonecall_java].

## <a id="AdditionalServices"></a>Nasıl Yapılır: Ek Twilio hizmetlerini kullanma
Burada gösterilen örneklerden yanı sıra, Twilio, Azure uygulamanızı ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [Twilio API'si belgeleri][twilio_api_documentation].

## <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmeti temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl yapılır kılavuzları ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts]
* [Twilio github'da][twilio_on_github]
* [Twilio desteği için konuşma][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: https://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: https://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: https://www.twilio.com/pricing
[special_offer]: https://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: https://www.twilio.com/docs
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
