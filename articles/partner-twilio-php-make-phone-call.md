---
title: Twilio (PHP'den) bir telefon görüşmesi yapma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. PHP uygulaması örnekleri içindir.
documentationcenter: php
services: ''
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
ms.openlocfilehash: 03b74f5a931e1cfbf09433af76c250607b7fc80c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60422330"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Azure'da bir PHP uygulaması içinde Twilio kullanarak telefon görüşmesi yapma
Aşağıdaki örnek, Azure'da barındırılan bir PHP web sayfasından çağrı yapmak için Twilio nasıl kullanabileceğinizi gösterir. Elde edilen uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi kullanıcıdan telefon araması değerlerini ister.

![Twilio ve PHP kullanarak azure çağrı formu][twilio_php]

Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesap ve kimlik doğrulama Al belirtecine, [Twilio konsol][twilio_console]. Fiyatlandırma, Twilio ile çalışmaya başlamak için değerlendirmek [ https://www.twilio.com/pricing ] [ twilio_pricing]. Bir deneme hesabı için kaydolabilirsiniz [ https://www.twilio.com/try-twilio ] [ try_twilio].
2. Elde [PHP için Twilio Kitaplığı](https://github.com/twilio/twilio-php) veya ARMUTLU paket olarak yükleyin. Daha fazla bilgi için [Benioku dosyası](https://github.com/twilio/twilio-php/blob/master/README.md).
3. PHP için Azure SDK'sını yükleyin. 
<!-- For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md) -->

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki HTML kodu bir web sayfası oluşturmak nasıl gösterir (**callform.html**) arama yapmak için kullanıcı verilerini alır:

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

## <a name="create-the-code-to-make-the-call"></a>Çağrı yapmak için kod oluşturun
Aşağıdaki kod nasıl oluşturulacağını gösterir **makecall.php**, kullanıcı tarafından görüntülenen formu gönderdiğinde çağırılır **callform.html**. Aşağıda gösterilen kod çağrı ileti oluşturur ve bir çağrı oluşturur. Ayrıca kimlik doğrulaması ve Twilio hesabı kullandığınızdan emin olun belirtecine [Twilio konsol] [ twilio_console] atanan yer tutucu değerleri yerine **$sid** ve  **$token** aşağıdaki kodda.

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
            array('url' => https://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Çağrıyı yapan yanı sıra **makecall.php** aşağıdaki resimde gösterildiği gibi bazı çağrı meta verilerini görüntüler. Çağrı meta veriler hakkında daha fazla bilgi için bkz. [ https://www.twilio.com/docs/api/rest/call#instance-properties ] [ twilio_call_properties].

![Twilio ve PHP kullanarak azure çağrı yanıtı][twilio_php_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Sonraki adım [uygulamanızı Azure Web Apps, Git ile dağıtmaya](app-service/app-service-web-get-started-php.md) (tüm bilgiler var. ilgili rağmen). 

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Twilio azure'da PHP kullanarak temel işlevselliğini göstermek için sağlanmıştır. Üretimde Azure'a dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Web formu kullanmak yerine, Azure depolama blobları veya SQL veritabanı telefon numaraları depolamak ve metin çağırmak için kullanabilirsiniz. PHP'de Azure depolama BLOB'ları kullanma hakkında daha fazla bilgi için bkz: [kullanarak Azure depolama ile PHP uygulamaları][howto_blob_storage_php]. PHP'de SQL veritabanını kullanma hakkında daha fazla bilgi için bkz: [kullanarak SQL veritabanı ile PHP uygulamaları][howto_sql_azure_php].
* **Makecall.php** kod Twilio tarafından sağlanan URL kullanır ([https://twimlets.com/message][twimlet_message_url]) nasıl devam etmek Twilio bildiren bir Twilio biçimlendirme dili (TwiML) yanıt sağlamak için çağrısı. Örneğin, döndürülen TwiML içerebilir bir `<Say>` çağrı alıcıya konuşulan metnin sonuçlanır fiil. Twilio tarafından sağlanan URL kullanmak yerine, Twilio'nın isteğine yanıt vermek için kendi hizmet oluşturabilirsiniz; Daha fazla bilgi için [kullanım Twilio ses ve SMS özellikleri php'de için nasıl][howto_twilio_voice_sms_php]. TwiML hakkında daha fazla bilgi şu adreste bulunabilir: [ https://www.twilio.com/docs/api/twiml ] [ twiml]ve daha fazla bilgi `<Say>` ve diğer Twilio fiilleri şu yolda bulunabilir: [ https://www.twilio.com/docs/api/twiml/say ][twilio_say].
* Twilio güvenlik yönergeleri okuyun [ https://www.twilio.com/docs/security ] [ twilio_docs_security].

Twilio hakkında ek bilgi için bkz: [ https://www.twilio.com/docs ] [ twilio_docs].

## <a name="see-also"></a>Ayrıca Bkz.
* [Ses ve SMS özellikleri php'de için Twilio kullanma](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: https://twimlets.com/message
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api_service]: https://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: https://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: https://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[twilio_php_github]: https://github.com/twilio/twilio-php
