---
title: "Twilio (PHP'nin) bir telefon araması yapma | Microsoft Docs"
description: "Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. PHP uygulaması için örneklerdir."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9866a196b3be10548d7a431430e570b41c190fc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Azure üzerinde bir PHP uygulamasının Twilio kullanarak bir telefon araması yapma
Aşağıdaki örnek, bir PHP web sayfasından Azure üzerinde barındırılan bir arama yapmak için Twilio nasıl kullanabileceğinizi gösterir. Sonuçta elde edilen uygulama telefon araması değerleri için aşağıdaki ekran görüntüsünde gösterildiği gibi ister.

![Twilio ve PHP kullanarak azure çağrı formu][twilio_php]

Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesabı ve kimlik doğrulaması almak belirtecine, [Twilio konsol][twilio_console]. Twilio ile çalışmaya başlamak için fiyatlandırma değerlendirmek [http://www.twilio.com/pricing][twilio_pricing]. Bir deneme hesabı için kaydolabilirsiniz [https://www.twilio.com/try-twilio][try_twilio].
2. Elde [Twilio kitaplığı için PHP](https://github.com/twilio/twilio-php) veya ARMUTLU paket olarak yükleyin. Daha fazla bilgi için bkz: [Benioku dosyasını](https://github.com/twilio/twilio-php/blob/master/README.md).
3. PHP için Azure SDK'yı yükleyin. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki HTML kod bir web sayfası oluşturmak nasıl gösterir (**callform.html**) arama yapmak için kullanıcı verilerini alır:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a>Çağrı yapmak için kod oluşturma
Aşağıdaki kod nasıl oluşturulacağını gösterir **makecall.php**, kullanıcı tarafından görüntülenen formu gönderdiğinde adlandırılan **callform.html**. Aşağıda gösterilen kodu çağrı iletisi oluşturur ve çağrı oluşturur. Ayrıca, Twilio hesabı ve kimlik doğrulaması kullandığınızdan emin olun belirtecine [Twilio konsol] [ twilio_console] atanan yer tutucu değerlerini yerine **$sid** ve **$token** kod.

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => http://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Çağrı yapma yanı sıra **makecall.php** aşağıdaki resimde gösterildiği gibi bazı çağrı meta verilerini görüntüler. Çağrı meta veriler hakkında daha fazla bilgi için bkz: [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Twilio ve PHP kullanarak azure çağrı yanıtı][twilio_php_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Sonraki adım [Azure Web Apps Git ile uygulamanızı dağıtmak](app-service/app-service-web-get-started-php.md) (tüm bilgileri var. ilgili olmasına rağmen). 

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Azure üzerinde php'de Twilio kullanarak temel işlevselliğini göstermek için sağlanmıştır. Azure'a üretimde dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Bir web formu kullanmak yerine, Azure storage bloblarında veya SQL veritabanı telefon numaralarını depolamak ve metin çağırmak için kullanabilirsiniz. Azure storage bloblarında PHP ile kullanma hakkında daha fazla bilgi için bkz: [PHP uygulamaları kullanarak Azure depolamaya][howto_blob_storage_php]. SQL veritabanı PHP ile kullanma hakkında daha fazla bilgi için bkz: [SQL veritabanıyla kullanarak PHP uygulamaları][howto_sql_azure_php].
* **Makecall.php** kod Twilio tarafından sağlanan URL kullanır ([http://twimlets.com/message][twimlet_message_url]) çağrısı ile devam etmek nasıl Twilio bildiren bir Twilio biçimlendirme dili (TwiML) yanıt sağlamak için. Örneğin, döndürülen TwiML içerebilir bir `<Say>` çağrısı alıcıya konuşulan metin sonuçlanır fiil. Twilio tarafından sağlanan URL kullanmak yerine, kendi hizmet Twilio'nın isteğine yanıt vermek için derleme; Daha fazla bilgi için bkz: [ses ve PHP SMS özelliklerini için kullanım Twilio nasıl][howto_twilio_voice_sms_php]. TwiML hakkında daha fazla bilgi bulunabilir [http://www.twilio.com/docs/api/twiml][twiml]ve hakkında daha fazla bilgi `<Say>` ve diğer Twilio fiiller bulunabilir [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Twilio güvenlik yönergeleri okuyun [https://www.twilio.com/docs/security][twilio_docs_security].

Twilio hakkında ek bilgi için bkz: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Ayrıca Bkz.
* [Ses ve PHP SMS özelliklerini için Twilio kullanma](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[twilio_php_github]: https://github.com/twilio/twilio-php
