---
title: Ses, VoIP ve Azure'da iletileri SMS için Twilio kullanma
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. Node.js'de yazılmış kod örnekleri.
services: ''
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: ''
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: d9f419c48f64ba697e031dfc680bc9cb12bba5c4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60422929"
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Ses, VoIP ve Azure'da iletileri SMS için Twilio kullanma
Bu kılavuz, Twilio ve azure'da node.js ile iletişim kuran uygulamaları nasıl oluşturacağınızı gösterir.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Twilio nedir?
Twilio, geliştiricilerin olun ve telefon çağrılarını almak, göndermek ve metin iletileri almasına ve tarayıcı tabanlı ve yerel mobil uygulamalara VoIP çağrısı katıştırmak için kolay bir API'si platformudur. Şimdi bunun nasıl girmeden önce çalıştığı kısaca gidin.

### <a name="receiving-calls-and-text-messages"></a>Aramaları ve Sms'ler alma
Twilio geliştiricilerinin sağlayan [programlanabilir telefon numaralarını satın] [ purchase_phone] hem gönderdikleri hem de aramaları ve sms'ler almak için kullanılabilir. Twilio numarası bir gelen çağrı veya kısa mesaj aldığında, Twilio, web uygulamanızın bir HTTP POST veya GET isteği, nasıl çağrı veya kısa mesaj yönetileceğine ilişkin yönergeler için soran gönderir. Sunucunuz Twilio'nın HTTP isteği ile yanıt verecektir [TwiML][twiml], bir çağrı veya kısa mesaj işlemek yönergeler içeren bir XML etiketleri basit bir dizi. Bir dakika içinde TwiML örnekleri göreceğiz.

### <a name="making-calls-and-sending-text-messages"></a>Çağrıları yapma ve kısa mesaj gönderme
Geliştiriciler, Twilio web hizmeti API'sine HTTP isteği yaparak, kısa mesaj gönderin veya giden telefon görüşmeleri başlatın. Giden çağrıları için geliştirici TwiML yönergeleri için bu bağlantı kurulduktan sonra giden çağrı işlemek nasıl döndüren bir URL de belirtmeniz gerekir.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Ekleme VoIP özelliklerini UI kod (JavaScript, iOS veya Android)
Twilio, herhangi bir masaüstü web tarayıcısı, iOS uygulaması veya Android uygulamanızı VoIP telefonunuza kapatabilirsiniz bir istemci tarafı SDK sunar. Bu makalede, tarayıcıda çağırma VoIP kullanmayı odaklanacağız. Ek olarak *Twilio JavaScript SDK'sı* tarayıcıda çalışan, sunucu tarafı application (node.js uygulamamızı) "özelliği belirteci" JavaScript istemciye vermek için kullanılmalıdır. Daha fazla bilgi edinebilirsiniz VoIP node.js ile kullanma hakkında [Twilio geliştirme blogunda][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Twilio (Microsoft indirim) için kaydolun
Twilio hizmetlerini kullanmadan önce önce [bir hesap için kaydolun][signup]. Microsoft Azure müşterileri, bir özel indirim - alma [buradan kaydolun mutlaka][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Node.js Azure Web sitesi oluşturup dağıtın
Ardından, Azure üzerinde çalışan bir node.js Web sitesi oluşturmanız gerekecektir. [Bunu yapmak için resmi belgeleri şuradan ulaşabilirsiniz][azure_new_site]. Yüksek bir düzeyde, aşağıdaki gerçekleştirecekseniz:

* Zaten yoksa, bir Azure hesabı için kaydolma
* Yeni bir Web sitesi oluşturmak için Azure yönetici konsolunu kullanarak
* (Git kullanılan varsayacağız) kaynak denetimi desteği ekleme
* Bir dosya oluşturarak `server.js` basit node.js web uygulaması ile
* Bu basit bir uygulamayı azure'a dağıtma

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a>Twilio modülünü Yapılandır
Ardından, Twilio API'si kullanan sağlayan bir basit bir node.js uygulaması yazma başlayacağız. Başlamadan önce size sunduğumuz Twilio hesap kimlik bilgilerini yapılandırmanız gerekir.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Sistem ortam değişkenlerini, Twilio kimlik bilgilerini yapılandırma
Twilio arka uca kimliği doğrulanmış istekler yapmak için hesap SID'si ve kimlik doğrulama belirteci, kullanıcı adı ve parolası Twilio hesabımız için Ata hangi işlevi gerekiyor. Bu kullanım için azure'da düğüm modülü ile yapılandırmak için en güvenli aracılığıyla doğrudan Azure yönetici konsolundan ayarlayabilirsiniz sistem ortam değişkenlerini yoludur.

Node.js Web sitenizi seçin ve "Yapılandır" bağlantısına tıklayın.  Bir bit kaydırırsanız, uygulamanız için yapılandırma özelliklerini ayarlayabileceğiniz bir alan görürsünüz.  Twilio hesap kimlik bilgilerinizi girin ([, Twilio konsolunda bulunan][twilio_console]) gösterildiği gibi - bunları adlandırdığınızdan emin olun `TWILIO_ACCOUNT_SID` ve `TWILIO_AUTH_TOKEN`sırasıyla:

![Azure Yönetim Konsolu][azure-admin-console]

Bu değişkenler yapılandırdıktan sonra uygulamanızı Azure konsolunda yeniden başlatın.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Package.json Twilio modülünde bildirme
Ardından, bizim düğüm modülü bağımlılıklarını aracılığıyla yönetmek için bir package.json oluşturmak ihtiyacımız [npm]. Aynı düzeyde `server.js` oluşturduğunuz dosya *Azure/node.js* öğreticide adlı bir dosya oluşturun `package.json`.  Bu dosyanın aşağıdakileri yerleştirin:

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

Bu bağımlılık, olarak popüler twilio modülü bildirir [Express web çerçevesini] [ express] ve EJS şablon altyapısı.  Tamam, şimdi biz her şey Tamam-biraz kod yazalım!

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Giden bir çağrı yapmak
Burada birkaç çağrısı yerleştireceğiniz basit bir form oluşturalım. Açık yukarı `server.js`ve aşağıdaki kodu girin. "Azure, Web sitesinin adını buraya koymanız CHANGE_ME" - Burada yazacaktır dikkat edin:

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

Ardından, adında bir dizin oluşturun `views` - bu dizin içinde adlı bir dosya oluşturun `index.ejs` aşağıdaki içeriklerle:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

Şimdi, Web sitenizi Azure'a dağıtın ve ev açın. Metin alanına telefon numaranızı girin ve Twilio numaranızı bir çağrı almak olmalıdır!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>Bir SMS iletisi gönderin
Şimdi bir kısa mesaj göndermek için bir kullanıcı arabirimi ve işleme mantığı formunu şimdi ayarlayın. Açık yukarı `server.js`ve son çağrısından sonra aşağıdaki kodu ekleyin `app.post`:

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

İçinde `views/index.ejs`, bir sayı ve kısa mesaj göndermek için birinci altında başka bir form ekleyin:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

Uygulamanızı azure'a yeniden dağıtmak ve artık, form ve kendinizi (veya en yakın arkadaşlarınızın birini) SMS mesajı gönderecek gönderebilmesi olmalıdır!

<a id="nextsteps"/>

## <a name="next-steps"></a>Sonraki Adımlar
Artık, node.js ve Twilio, iletişim kuran uygulamaları oluşturmak için kullanmanın temel adımlarını öğrendiniz. Ancak bu örnekler şekilleri, Twilio ve node.js ile olasılıklara bölümüdür. Node.js ile Twilio kullanarak daha fazla bilgi için aşağıdaki kaynaklara göz atın:

* [Resmi Modülü][docs]
* [VoIP öğretici ile node.js uygulamalarını][voipnode]
* [Votr - oylama uygulaması node.js ve CouchDB (üç parça) ile gerçek zamanlı bir SMS][votr]
* [Node.js ile tarayıcıda çifti programlama][pair]

Azure'da node.js ve Twilio deşifre etme sevdiğiniz umuyoruz!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: https://ahoy.twilio.com/azure
[azure_new_site]: app-service/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: https://npmjs.org
[express]: https://expressjs.com
[voipnode]: https://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: https://www.twilio.com/docs/libraries/reference/twilio-node/
[votr]: https://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: https://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
