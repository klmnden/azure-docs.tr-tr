---
title: Twilio ses ve SMS (PHP) için nasıl kullanılacağı | Microsoft Docs
description: Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. PHP ile yazılan kod örnekleri.
documentationcenter: php
services: ''
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: bd50eac7390e8639f77894689388e6926cdb619c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866180"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Ses ve PHP SMS özelliklerini için Twilio kullanma
Bu kılavuz, Azure üzerinde Twilio API hizmeti genel programlama görevleri gerçekleştirmek gösterilmiştir. Kapsamdaki senaryolar bir telefon araması yapmadan ve kısa ileti hizmeti (SMS) ileti gönderme içerir. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz: [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio iş iletişimleri, ses, VoIP ve uygulamalara Mesajlaşma geliştiricilerin etkinleştirme geleceği destekleyen. Bunlar Twilio iletişim API platformu gösterme bulut tabanlı, genel bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklendirilebilir. Sizinle ödeme-olarak esnekliğinin fiyatlandırma gidin ve bulut güvenilirlik ' yararlanır.

**Twilio sesli** yapmak ve telefon çağrılarını almak, uygulamalarınızın sağlar. **Twilio SMS** metin ileti gönderme ve alma için uygulamanızı sağlar. **Twilio istemci** VoIP çağrıları herhangi telefon, tablet ya da tarayıcı yapmanızı sağlar ve WebRTC destekler.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterilerin alacak bir [özel teklif](http://www.twilio.com/azure): 10 ücretsiz Twilio Twilio hesabınızı yükseltirken kredisi. Bu Twilio kredi varsa Twilio kullanımını ($10 alacak kadar 1.000 SMS iletileri göndermek ya da telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesli dakika alırken eşdeğerdir) uygulanabilir. Bu Twilio iade almak ve adresindeki kullanmaya başlama: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio Kullandıkça Ödeme tabanlı bir hizmettir. Hiçbir Kurulum ücretleri vardır ve herhangi bir zamanda hesabınızı kapatabilirsiniz. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma][twilio_pricing].

## <a id="Concepts"></a>Kavramları
Twilio API uygulamaları için ses ve SMS işlevselliği sağlayan bir RESTful API'dır. İstemci kitaplıkları, birden çok dilde kullanılabilir; bir listesi için bkz: [Twilio API kitaplıkları][twilio_libraries].

Twilio API anahtar yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) ' dir.

### <a id="Verbs"></a>Twilio fiiller
API Twilio yararlanır fiiller; Örneğin,  **&lt;Say&gt;**  fiili kullanımı bir çağrıda bir ileti teslim Twilio bildirir.

Twilio fiillerin listesi verilmiştir. Diğer fiilleri ve aracılığıyla özellikleri hakkında bilgi edinin [Twilio biçimlendirme dili belgeleri](http://www.twilio.com/docs/api/twiml).

* **&lt;Arama&gt;**: başka bir telefon çağıran bağlanır.
* **&lt;Toplama&gt;**: telefon tuş takımında girilen Sayısal basamaklar toplar.
* **&lt;Kapat&gt;**: bir aramasını sonlandırır.
* **&lt;Yürüt&gt;**: bir ses dosyası çalar.
* **&lt;Duraklatma&gt;**: sessizce belirtilen sayıda saniye bekler.
* **&lt;Kayıt&gt;**: arayanın sesli kayıtlar ve kayıt içeren bir dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;**: farklı bir URL'de TwiML çağrısı veya SMS denetim aktarır.
* **&lt;Reddetme&gt;**: faturalama olmadan Twilio numaranızı için bir gelen çağrıyı reddeder
* **&lt;Söyleyin&gt;**: dönüştürür metin bir çağrıda yapılan okuma.
* **&lt;SMS&gt;**: SMS iletisi gönderir.

### <a id="TwiML"></a>TwiML
TwiML bir çağrı işlemek nasıl Twilio veya SMS bildiren Twilio fiilleri dayalı XML tabanlı yönergeleri kümesidir.

Örnek olarak, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşma için.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Uygulamanız Twilio API çağırdığında API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amaçlı uygulamalarınız tarafından kullanılan TwiML yanıt sağlamanız için sağlanan Twilio URL'leri kullanabilirsiniz. TwiML yanıtları oluşturmak üzere kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır **TwiMLResponse** nesnesi.

Twilio fiiller, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio hesabı oluşturma
Twilio hesap almak hazır olduğunuzda, oturum açın [deneyin Twilio][try_twilio]. Ücretsiz bir hesap ile başlatın ve daha sonra hesabınızı yükseltin.

Twilio hesabı için kaydolduğunuzda, hesap Kimliğini ve kimlik doğrulama belirtecini alırsınız. Her ikisi de Twilio API çağrıları yapmanız gerekecektir. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirteci güvenli tutun. Hesap Kimliğini ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio hesap sayfası][twilio_account], etiketli alanları **HESABININ SID** ve **kimlik doğrulama BELİRTECİ**sırasıyla.

## <a id="create_app"></a>PHP uygulaması oluşturma
Twilio hizmeti kullanır ve Azure üzerinde çalışan bir PHP uygulaması Twilio hizmetini kullanan tüm diğer PHP uygulaması farklı değildir. Twilio Hizmetleri REST tabanlı ve PHP'nin çeşitli yollarla çağrılabilir olsa da, bu makalede Twilio Hizmetleri ile kullanmak üzere nasıl odaklanacaktır [GitHub PHP'nin Twilio kitaplığının][twilio_php]. PHP için Twilio kitaplığını kullanma hakkında daha fazla bilgi için bkz: [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Oluşturma ve azure'a Twilio/PHP uygulaması dağıtma hakkında ayrıntılı yönergeler şurada bulunabilir [Azure üzerinde bir PHP uygulamasının içinde kullanarak bir telefon araması Twilio nasıl][howto_phonecall_php].

## <a id="configure_app"></a>Uygulamanızı Twilio kitaplıkları kullanacak şekilde yapılandırma
Uygulamanızı Twilio kitaplığı PHP için iki yolla kullanacak şekilde yapılandırabilirsiniz:

1. Twilio kitaplığı PHP github'dan indirin ([https://github.com/twilio/twilio-php][twilio_php]) ve ekleme **Hizmetleri** uygulamanıza dizin.
   
    -VEYA-
2. Twilio kitaplığı için PHP ARMUTLU paket olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

PHP için Twilio kitaplığı yükledikten sonra daha sonra ekleyebilirsiniz bir **require_once** kitaplığı başvurmak için PHP dosyalarınızı üstündeki deyimi:

        require_once 'Services/Twilio.php';

Daha fazla bilgi için bkz: [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Aşağıdaki çağrıda giden yapılacağını gösterir **Services_Twilio** sınıfı. Bu kod bir Twilio tarafından sağlanan site Twilio biçimlendirme dili (TwiML) yanıt döndürmek için de kullanır. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kod çalıştırılmadan önce Twilio hesabınız için telefon numarası.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için bkz: [nasıl TwiML yanıtlar sağlayan bilgisayarınızı kendi Web sitesinden](#howto_provide_twiml_responses).

* **Not**: SSL sertifikası doğrulama hatalarını gidermek için bkz: [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Nasıl yapılır: bir SMS iletisi gönderin
Aşağıdaki bir SMS kullanarak ileti gönder gösterilmektedir **Services_Twilio** sınıfı. **Gelen** numarası SMS iletileri göndermek için tarafından deneme hesapları için Twilio sağlanır. **İçin** sayı doğrulandı, kod çalıştırılmadan önce Twilio hesabınız için.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Nasıl yapılır: kendi Web sitesinden TwiML yanıtlarını sağlar
Uygulamanızı Twilio API çağrısı başlattığında Twilio TwiML yanıt döndürmek için beklenen bir URL isteğinizi gönderin. Yukarıdaki örnekte Twilio tarafından sağlanan URL'yi kullanır [http://twimlets.com/message][twimlet_message_url]. (TwiML Twilio tarafından kullanılmak üzere tasarlandığından, BT tarayıcınızda görüntüleyebilirsiniz. For example, tıklatın [http://twimlets.com/message] [ twimlet_message_url] boş bir görmek için `<Response>` öğesi; başka bir örnek olarak, tıklatın [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] görmek için bir `<Response>` içeren öğe bir `<Say>` öğesi.)

Twilio tarafından sağlanan URL üzerinde güvenmek yerine, HTTP yanıtlarını döndürür, kendi site oluşturabilirsiniz. XML yanıtlarını döndürür herhangi bir dilde site oluşturabilirsiniz; Bu konu, PHP TwiML oluşturmak için kullanmaya başlayacağınız varsayar.

Aşağıdaki PHP sayfasını sonuçlarını bildiren bir TwiML yanıtına **Hello World** çağrısında.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Yukarıdaki örnekte görüldüğü gibi TwiML yanıt basitçe bir XML dosyasıdır. PHP için Twilio kitaplığı TwiML oluşturacaktır sınıfları içerir. Aşağıdaki örnek, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır **Hizmetleri\_Twilio\_Twiml** sınıfı PHP Twilio Kitaplığı'nda:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML hakkında daha fazla bilgi için bkz: [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

TwiML yanıt sağlamanız için ayarlamanız PHP sayfanızı oluşturduktan sonra içine URL geçti olarak PHP sayfasının URL'sini kullanmak `Services_Twilio->account->calls->create` yöntemi. Adlı bir Web uygulaması varsa, örneğin, **MyTwiML** dağıtılan Azure barındırılan hizmeti ve PHP sayfanın adıdır **mytwiml.php**, URL için geçirilebilir **Services_Twilio -> Hesap çağrıları -> -> oluşturma** aşağıdaki örnekte gösterildiği gibi:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

PHP ile azure'da Twilio kullanma hakkında ek bilgi için bkz: [Azure üzerinde bir PHP uygulamasının içinde kullanarak bir telefon araması Twilio nasıl][howto_phonecall_php].

## <a id="AdditionalServices"></a>Nasıl yapılır: ek Twilio Hizmetleri kullanın
Burada gösterilen örnekler yanı sıra, Azure uygulamanız ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler Twilio sunar. Ayrıntılar için bkz: [Twilio API belgelerine][twilio_api_documentation].

## <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmetinin öğrendiğinize göre daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl ın yapılır ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts] 
* [Github'da Twilio][twilio_on_github]
* [Twilio desteklemek için konuşun][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
