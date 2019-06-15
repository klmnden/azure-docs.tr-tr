---
title: SendGrid e-posta hizmetini (PHP) kullanma | Microsoft Docs
description: Bilgi e-posta SendGrid e-posta hizmeti ile Azure üzerinde nasıl gönderin. PHP'de yazılan kod örneklerini.
documentationcenter: php
services: ''
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: db3333aa52782ceb949ef3f46a903b618f6e3f2f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60931237"
---
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Php'den SendGrid e-posta hizmetini kullanma

Bu kılavuzda, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Örnekler, PHP'de yazılır.
Senaryoları ele alınmaktadır **e-posta oluşturma**, **e-posta gönderme**, ve **ekler eklenirken**. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olduğu bir [bulut tabanlı e-posta hizmeti] sağlayan güvenilir [İşlem tabanlı e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz, özel tümleştirmeyi kolaylaştıran esnek API'ler ile birlikte. SendGrid kullanım senaryoları şunları içerir:

* Otomatik olarak okundu bilgilerini müşterilere gönderme
* Müşteriler aylık e-ilanlara ve özel teklifler göndermek için dağıtım yönetme listeler
* Engellenen e-posta ve müşteri yanıt hızı gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemenize yardımcı olması için rapor oluşturma
* İletme müşteri sorguları
* Uygulamanızdan e-posta bildirimleri

Daha fazla bilgi için [ https://sendgrid.com ] [ https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma

[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>PHP uygulamanızı Sendgrid'den kullanma

SendGrid kullanarak bir Azure PHP uygulaması içinde gerektiren özel yapılandırma veya kodlama. SendGrid bir hizmet olduğundan, bir şirket içi uygulamasından mümkün olduğunca bunun bir bulut uygulamasında tam olarak aynı şekilde erişilebilir.

## <a name="how-to-send-an-email"></a>Nasıl yapılır: E-posta Gönder

E-posta SMTP veya SendGrid tarafından sağlanan Web API'sini kullanarak gönderebilirsiniz.

### <a name="smtp-api"></a>SMTP API

SendGrid SMTP API'sini kullanarak e-posta göndermek için *Swift kullanılmasının*, PHP uygulamalarından e-posta göndermek için bileşen tabanlı bir kitaplık. İndirebileceğiniz [Swift kullanılmasının Kitaplığı](https://swiftmailer.symfony.com/) v5.3.0 (kullanın [Oluşturucu] Swift kullanılmasının yüklemek için). Kitaplığı ile e-posta göndermeyi içerir örneklerini oluşturmaya `Swift\_SmtpTransport`, `Swift\_Mailer`, ve `Swift\_Message` sınıfları, ilgili özellikleri ayarlarken ve çağırma `Swift\_Mailer::send` yöntemi.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
 $html = "<html>
       <head></head>
       <body>
           <p>Hi!<br>
               How are you?<br>
           </p>
       </body>
       </html>";
 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');
 // Email recipients
 $to = array(
       'john@contoso.com'=>'Destination 1 Name',
       'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
     // This will let us know how many users received this message
     echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
     echo "Something went wrong - ";
     print_r($failures);
 }
```

### <a name="web-api"></a>Web API
PHP'ın kullanma [curl işlevi] [ curl function] Web API'si SendGrid kullanarak e-posta göndermek için.

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $params = array(
      'api_user' => $user,
      'api_key' => $pass,
      'to' => 'john@contoso.com',
      'subject' => 'testing from curl',
      'html' => 'testing body',
      'text' => 'testing body',
      'from' => 'anna@contoso.com',
   );

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

Bu gerçek anlamda bir RESTful API'si çoğu çağrılarındaki her ikisini de almak ve sonrası fiilleri birbirlerinin yerine kullanılabilir olmasa da SendGrid Web API'sini bir REST API'si için çok benzer.

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: Bir ek ekleyin

### <a name="smtp-api"></a>SMTP API

SMTP API'sini kullanarak bir eki göndererek, ek bir Swift kullanılmasının içeren bir e-posta göndermek için örnek kod için kod satırı gerektirir.

```php
<?php
 include_once "vendor/autoload.php";
 /*
  * Create the body of the message (a plain-text and an HTML version).
  * $text is your plain-text email
  * $html is your html version of the email
  * If the receiver is able to view html emails then only the html
  * email will be displayed
  */
 $text = "Hi!\nHow are you?\n";
  $html = "<html>
      <head></head>
      <body>
         <p>Hi!<br>
            How are you?<br>
         </p>
      </body>
      </html>";

 // This is your From email address
 $from = array('someone@example.com' => 'Name To Appear');

 // Email recipients
 $to = array(
      'john@contoso.com'=>'Destination 1 Name',
      'anna@contoso.com'=>'Destination 2 Name'
 );
 // Email subject
 $subject = 'Example PHP Email';

 // Login credentials
 $username = 'yoursendgridusername';
 $password = 'yourpassword';

 // Setup Swift mailer parameters
 $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
 $transport->setUsername($username);
 $transport->setPassword($password);
 $swift = Swift_Mailer::newInstance($transport);

 // Create a message (subject)
 $message = new Swift_Message($subject);

 // attach the body of the email
 $message->setFrom($from);
 $message->setBody($html, 'text/html');
 $message->setTo($to);
 $message->addPart($text, 'text/plain');
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

 // send message
 if ($recipients = $swift->send($message, $failures))
 {
      // This will let us know how many users received this message
      echo 'Message sent out to '.$recipients.' users';
 }
 // something went wrong =(
 else
 {
      echo "Something went wrong - ";
      print_r($failures);
 }
```

Ek kod satırı aşağıdaki gibidir:

```php
 $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));
```

Bu kod satırı Ekle yöntemi çağırır `Swift\_Message` nesne ve statik yöntemi kullanan `fromPath` üzerinde `Swift\_Attachment` almak ve bir dosya eklemek için sınıfı.

### <a name="web-api"></a>Web API

Web API'si kullanarak ek gönderme, Web API'si kullanarak bir e-posta göndermeyi çok benzer. Ancak, aşağıdaki örnekte, parametre dizisi bu öğe içermesi gerektiğini unutmayın:

```php
    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
```

#### <a name="example"></a>Örnek

```php
<?php

 $url = 'https://api.sendgrid.com/';
 $user = 'USERNAME';
 $pass = 'PASSWORD';

 $fileName = 'myfile';
 $filePath = dirname(__FILE__);

 $params = array(
     'api_user' => $user,
     'api_key' => $pass,
     'to' =>'john@contoso.com',
     'subject' => 'test of file sends',
     'html' => '<p> the HTML </p>',
     'text' => 'the plain text',
     'from' => 'anna@contoso.com',
     'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
 );

 print_r($params);

 $request = $url.'api/mail.send.json';

 // Generate curl request
 $session = curl_init($request);

 // Tell curl to use HTTP POST
 curl_setopt ($session, CURLOPT_POST, true);

 // Tell curl that this is the body of the POST
 curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

 // Tell curl not to return headers, but do return the response
 curl_setopt($session, CURLOPT_HEADER, false);
 curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

 // obtain response
 $response = curl_exec($session);
 curl_close($session);

 // print everything out
 print_r($response);
```

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: Altbilgi, izleme ve analiz etkinleştirmek için filtreleri kullanın

SendGrid kullanarak ek e-posta işlevselliği sağlar *filtreleri*. Bu izleme'ye tıklayın, Google analytics, izleme aboneliği ve benzeri etkinleştirme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisi eklenebilir ayarlarıdır.

İleti filtreleri filtreleri özelliğini kullanarak uygulanabilir. Her filtre filtre özgü ayarları içeren bir karma olarak belirtilir. Aşağıdaki örnek alt bilgi filtre sağlar ve e-posta iletisinin alt kısmına eklenir bir kısa mesaj belirtir. Bu örnekte kullanacağız [sendgrid php Kitaplığı].

Kullanım [Oluşturucu] kitaplığını yüklemek için:

```bash
php composer.phar require sendgrid/sendgrid 2.1.1
```

### <a name="example"></a>Örnek  

```php
<?php
 /*
  * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
  */
 include "vendor/autoload.php";

 $email = new SendGrid\Email();
 // The list of addresses this message will be sent to
 // [This list is used for sending multiple emails using just ONE request to SendGrid]
 $toList = array('john@contoso.com', 'anna@contoso.com');

 // Specify the names of the recipients
 $nameList = array('Name 1', 'Name 2');

 // Used as an example of variable substitution
 $timeList = array('4 PM', '5 PM');

 // Set all of the above variables
 $email->setTos($toList);
 $email->addSubstitution('-name-', $nameList);
 $email->addSubstitution('-time-', $timeList);

 // Specify that this is an initial contact message
 $email->addCategory("initial");

 // You can optionally setup individual filters here, in this example, we have
 // enabled the footer filter
 $email->addFilter('footer', 'enable', 1);
 $email->addFilter('footer', "text/plain", "Thank you for your business");
 $email->addFilter('footer', "text/html", "Thank you for your business");

 // The subject of your email
 $subject = 'Example SendGrid Email';

 // Where is this message coming from. For example, this message can be from
 // support@yourcompany.com, info@yourcompany.com
 $from = 'someone@example.com';

 // If you do not specify a sender list above, you can specify the user here. If
 // a sender list IS specified above, this email address becomes irrelevant.
 $to = 'john@contoso.com';

 # Create the body of the message (a plain-text and an HTML version).
 # text is your plain-text email
 # html is your html version of the email
 # if the receiver is able to view html emails then only the html
 # email will be displayed

 /*
  * Note the variable substitution here =)
  */
 $text = "
 Hello -name-,
 Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
 Regards,
 Fred";

 $html = "
 <html>
 <head></head>
 <body>
 <p>Hello -name-,<br>
 Thank you for your interest in our products. We have set up an appointment
 to call you at -time- EST to discuss your needs in more detail.

 Regards,

 Fred<br>
 </p>
 </body>
 </html>";

 // set subject
 $email->setSubject($subject);

 // attach the body of the email
 $email->setFrom($from);
 $email->setHtml($html);
 $email->addTo($to);
 $email->setText($text);

 // Your SendGrid account credentials
 $username = 'sendgridusername@yourdomain.com';
 $password = 'example';

 // Create SendGrid object
 $sendgrid = new SendGrid($username, $password);

 // send message
 $response = $sendgrid->send($email);

 print_r($response);
 ```

## <a name="next-steps"></a>Sonraki Adımlar

SendGrid e-posta hizmeti ile ilgili temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* SendGrid belgeler: <https://sendgrid.com/docs>
* SendGrid PHP kitaplığı: <https://github.com/sendgrid/sendgrid-php>
* Azure müşterileri için SendGrid özel tekliftir: <https://sendgrid.com/windowsazure.html>

Daha fazla bilgi için Ayrıca bkz: [PHP Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: https://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: https://php.net/curl
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[İşlem tabanlı e-posta teslimi]: https://sendgrid.com/transactional-email
[sendgrid php kitaplığı]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[Oluşturucu]: https://getcomposer.org/download/
