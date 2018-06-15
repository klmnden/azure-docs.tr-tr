---
title: Twilio ses ve SMS (Java) için nasıl kullanılacağı | Microsoft Docs
description: Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. Java dilinde yazılan kod örnekleri.
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
ms.openlocfilehash: 5a1b2ffa160a31b639605242b651dc8d14e7a01b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866159"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Ses ve Java SMS yetenekler için Twilio kullanma
Bu kılavuz, Azure üzerinde Twilio API hizmeti genel programlama görevleri gerçekleştirmek gösterilmiştir. Kapsamdaki senaryolar bir telefon araması yapmadan ve kısa ileti hizmeti (SMS) ileti gönderme içerir. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz: [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses ve SMS uygulamaları oluşturmak için varolan web dilleri ve yetenekleri kullanmanıza olanak sağlayan bir telefon web hizmeti API'dir. Twilio, (olmayan bir Azure özelliğidir ve Microsoft Ürün) bir bir üçüncü taraf hizmetidir.

**Twilio sesli** yapmak ve telefon çağrılarını almak, uygulamalarınızın sağlar. **Twilio SMS** yapmak ve SMS iletileri almak, uygulamalarınızın sağlar. **Twilio istemci** mobil bağlantıları dahil olmak üzere var olan Internet bağlantıları kullanarak sesli iletişimi etkinleştirmek, uygulamalarınızın sağlar.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Twilio fiyatlandırma hakkında daha fazla bilgi şu adreste [Twilio fiyatlandırma][twilio_pricing]. Azure müşterilerine alma bir [özel teklif][special_offer]: boş bir kredi 1000 metinlerinin veya 1000 dakika sayısı. Bu teklif için kaydolun veya daha fazla bilgi edinmek için lütfen ziyaret [http://ahoy.twilio.com/azure][special_offer].

## <a id="Concepts"></a>Kavramları
Twilio API uygulamaları için ses ve SMS işlevselliği sağlayan bir RESTful API'dır. İstemci kitaplıkları, birden çok dilde kullanılabilir; bir listesi için bkz: [Twilio API kitaplıkları][twilio_libraries].

Twilio API anahtar yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) ' dir.

### <a id="Verbs"></a>Twilio fiiller
API Twilio yararlanır fiiller; Örneğin,  **&lt;Say&gt;**  fiili kullanımı bir çağrıda bir ileti teslim Twilio bildirir.

Twilio fiillerin listesi verilmiştir.

* **&lt;Arama&gt;**: başka bir telefon çağıran bağlanır.
* **&lt;Toplama&gt;**: telefon tuş takımında girilen Sayısal basamaklar toplar.
* **&lt;Kapat&gt;**: bir aramasını sonlandırır.
* **&lt;Yürüt&gt;**: bir ses dosyası çalar.
* **&lt;Sıra&gt;**: ekleme arayanlar sırasına.
* **&lt;Duraklatma&gt;**: sessizce belirtilen sayıda saniye bekler.
* **&lt;Kayıt&gt;**: arayanın sesli kayıtlar ve kayıt içeren bir dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;**: farklı bir URL'de TwiML çağrısı veya SMS denetim aktarır.
* **&lt;Reddetme&gt;**: faturalama olmadan Twilio numaranızı için bir gelen çağrıyı reddeder.
* **&lt;Söyleyin&gt;**: dönüştürür metin bir çağrıda yapılan okuma.
* **&lt;SMS&gt;**: SMS iletisi gönderir.

### <a id="TwiML"></a>TwiML
TwiML bir çağrı işlemek nasıl Twilio veya SMS bildiren Twilio fiilleri dayalı XML tabanlı yönergeleri kümesidir.

Örnek olarak, aşağıdaki TwiML metin dönüştürecektir **Merhaba Dünya!** Konuşma.

```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World!</Say>
    </Response>
```

Uygulamanız Twilio API çağırdığında API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amaçlı uygulamalarınız tarafından kullanılan TwiML yanıt sağlamanız için sağlanan Twilio URL'leri kullanabilirsiniz. TwiML yanıtları oluşturmak üzere kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır **TwiMLResponse** nesnesi.

Twilio fiiller, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio hesabı oluşturma
Twilio hesap almak hazır olduğunuzda, oturum açın [deneyin Twilio][try_twilio]. Ücretsiz bir hesap ile başlatın ve daha sonra hesabınızı yükseltin.

Twilio hesabı için kaydolduğunuzda, hesap Kimliğini ve kimlik doğrulama belirtecini alırsınız. Her ikisi de Twilio API çağrıları yapmanız gerekecektir. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirteci güvenli tutun. Hesap Kimliğini ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio konsol][twilio_console], etiketli alanları **HESABININ SID** ve **kimlik doğrulama BELİRTECİ**sırasıyla.

## <a id="create_app"></a>Bir Java uygulaması oluşturma
1. Twilio JAR edinin ve Java derleme yolu ve WAR dağıtım derleme ekleyin. Konumundaki [https://github.com/twilio/twilio-java][twilio_java], GitHub kaynakları indirmek ve kendi JAR oluşturabilir veya önceden oluşturulmuş bir JAR (veya ile bağımlılıklar olmadan) yükleyebilirsiniz.
2. JDK's olun **cacerts** anahtar deposu MD5 parmak izi 67:CB:9 D Equifax güvenli sertifika yetkilisi sertifikayla içerir: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (seri numarasını 35:DE:F4:CF ve SHA1 parmak izi D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Sertifika yetkilisi (CA) sertifikası budur [https://api.twilio.com] [ twilio_api_service] Twilio API'leri kullandığınızda çağrılan hizmet. JDK's sağlama hakkında bilgi için **cacerts** anahtar deposu doğru CA sertifikası varsa, bkz: [Java CA sertifika deposuna sertifika ekleme][add_ca_cert].

Java için Twilio istemci kitaplığı kullanılarak ayrıntılı yönergeler şurada bulunabilir [Azure üzerinde bir Java uygulamasında kullanarak bir telefon araması Twilio nasıl][howto_phonecall_java].

## <a id="configure_app"></a>Uygulamanızı Twilio kitaplıkları kullanacak şekilde yapılandırma
Kodunuzu içinde ekleyebileceğiniz **alma** deyimleri Twilio paketleri veya uygulamanızda kullanmak istediğiniz sınıfları için Kaynak dosyalarınız üstündeki.

Java kaynak dosyalar için:

```java
    import com.twilio.*;
    import com.twilio.rest.api.*;
    import com.twilio.type.*;
    import com.twilio.twiml.*;
```

Kaynak dosyaları Java sunucu sayfası (JSP):

```java
    import="com.twilio.*"
    import="com.twilio.rest.api.*"
    import="com.twilio.type.*"
    import="com.twilio.twiml.*"
 ```
 
Hangi Twilio paketleri veya sınıfları bağlı olarak, kullanmak istediğiniz, **alma** deyimleri farklı olabilir.

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Aşağıdaki çağrıda giden yapılacağını gösterir **çağrısı** sınıfı. Bu kod bir Twilio tarafından sağlanan site Twilio biçimlendirme dili (TwiML) yanıt döndürmek için de kullanır. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kod çalıştırılmadan önce Twilio hesabınız için telefon numarası.

```java
    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account_SID";
    String authToken = "your_twilio_authentication_token";

    // Initialize the Twilio client.
    Twilio.init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    URI uri = new URI("http://twimlets.com/message" +
            "?Message%5B0%5D=Hello%20World%21");

    // Declare To and From numbers
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

İçin geçirilen parametreler hakkında daha fazla bilgi için **Call.creator** yöntemi, bkz: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için bkz: [TwiML yanıtlarında sağlayan Azure üzerinde bir Java uygulamasının nasıl](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl yapılır: bir SMS iletisi gönderin
Aşağıdaki bir SMS kullanarak ileti gönder gösterilmektedir **ileti** sınıfı. **Gelen** numarası **4155992671**, SMS iletileri göndermek için tarafından deneme hesapları için Twilio sağlanır. **İçin** sayı doğrulandı, kod çalıştırılmadan önce Twilio hesabınız için.

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

İçin geçirilen parametreler hakkında daha fazla bilgi için **Message.creator** yöntemi, bkz: [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Nasıl yapılır: kendi Web sitesinden TwiML yanıtlarını sağlar
Olduğunda, uygulamanızın başlatır Twilio API çağrısı örneğin aracılığıyla **CallCreator.create** yöntemi, Twilio gönderecek isteğiniz TwiML yanıt döndürmek için beklenen bir URL. Yukarıdaki örnekte Twilio tarafından sağlanan URL'yi kullanır [http://twimlets.com/message][twimlet_message_url]. (TwiML Web Hizmetleri tarafından kullanılmak üzere tasarlandığından, TwiML tarayıcınızda görüntüleyebilirsiniz. For example, tıklatın [http://twimlets.com/message] [ twimlet_message_url] boş bir görmek için  **&lt;yanıt&gt;**  öğesi; başka bir örnek olarak, tıklatın [http://twimlets.com/message?Message%5B0%5D=Hello%20World%21] [ twimlet_message_url_hello_world] görmek için bir  **&lt;yanıt&gt;**  içeren öğe bir  **&lt;Say&gt;**  öğesi.)

Twilio tarafından sağlanan URL üzerinde güvenmek yerine, HTTP yanıtlarını döndürür kendi URL sitesi oluşturabilirsiniz. HTTP yanıt veren herhangi bir dilde site oluşturabilirsiniz; Bu konu, bir JSP sayfa URL'SİNDE barındırma varsayar.

Aşağıdaki JSP sayfasını sonuçlarını bildiren bir TwiML yanıtına **Merhaba Dünya!** arama üzerinde.

```xml
    <%@ page contentType="text/xml" %>
    <Response>
        <Say>Hello World!</Say>
    </Response>
```

Aşağıdaki JSP sayfası, bazı metinleri diyor, birkaç duraklatır olan ve Twilio API sürümü ve Azure rol adı hakkında bilgi belirten bir TwiML yanıt sonuçlanır.

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

**ApiVersion** parametredir Twilio sesli istekler (SMS istek değil) kullanılabilir. Twilio ses ve SMS istekleri kullanılabilir istek parametrelerini görmek için bkz: <https://www.twilio.com/docs/api/twiml/twilio_request> ve <https://www.twilio.com/docs/api/twiml/sms/twilio_request>sırasıyla. **RoleName** ortam değişkeni kullanılabilir Azure dağıtımın bir parçası olarak. (Bunlar gelen çekilmesi şekilde özel ortam değişkenleri eklemek istiyorsanız **System.getenv**, ortam değişkenleri bölümüne adresindeki bakın [çeşitli rol yapılandırma ayarları][misc_role_config_settings].)

TwiML yanıt sağlamanız için ayarlamanız JSP sayfanızı oluşturduktan sonra içine URL geçti olarak JSP sayfasının URL'sini kullanmak **Call.creator** yöntemi. Örneğin, adlandırılan bir Web uygulaması varsa bir Azure dağıtılan MyTwiML barındırılan hizmeti ve mytwiml.jsp JSP sayfanın adıdır, URL için geçirilebilir **Call.creator** aşağıda gösterildiği gibi:

```java
    // Declare To and From numbers and the URL of your JSP page
    PhoneNumber to = new PhoneNumber("NNNNNNNNNN");
    PhoneNumber from = new PhoneNumber("NNNNNNNNNN");
    URI uri = new URI("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    // Create a Call creator passing From, To and URL values
    // then make the call by executing the create() method
    Call.creator(to, from, uri).create();
```

Aracılığıyla TwiML ile yanıt için başka bir seçenek olan **VoiceResponse** kullanılabilir sınıfı **com.twilio.twiml** paket.

Java ile Azure Twilio kullanma hakkında ek bilgi için bkz: [Azure üzerinde bir Java uygulamasında kullanarak bir telefon araması Twilio nasıl][howto_phonecall_java].

## <a id="AdditionalServices"></a>Nasıl yapılır: ek Twilio Hizmetleri kullanın
Burada gösterilen örnekler yanı sıra, Azure uygulamanız ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler Twilio sunar. Ayrıntılar için bkz: [Twilio API belgelerine][twilio_api_documentation].

## <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmetinin öğrendiğinize göre daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl ın yapılır ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts]
* [Github'da Twilio][twilio_on_github]
* [Twilio desteklemek için konuşun][twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World%21
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/docs
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
