---
title: "Ses, VoIP ve Azure'da SMS Mesajlaşma Twilio kullanma"
description: "Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. Node.js içinde yazılan kod örnekleri."
services: 
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: 
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: f347540c78be712fc46d1d36b47ca4e23a62e28a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Ses, VoIP ve Azure'da SMS Mesajlaşma Twilio kullanma
Bu kılavuz, iletişim Twilio ve azure'da node.js uygulamaları geliştirmek gösterilmiştir.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Twilio nedir?
Twilio yapmak ve telefon çağrılarını almak, Gönder ve metin iletileri almasına ve tarayıcı tabanlı ve yerel mobil uygulamalara VoIP arama katıştırmak geliştiriciler için kolaylaştıran bir API platformudur. Şimdi bunun girmeden önce işleyişi kısaca gidin.

### <a name="receiving-calls-and-text-messages"></a>Aramaları ve metin iletileri alma
Twilio geliştiricilerine verir [programlanabilir telefon numaralarını satın] [ purchase_phone] hem göndermek ve çağrıları ve metin iletileri almak için kullanılabilir. Twilio sayıyı bir gelen arama veya metin aldığında, Twilio web uygulamanızı bir HTTP POST veya GET isteğini nasıl arama veya kısa mesaj yönetileceğine ilişkin yönergeler için isteyen gönderir. Sunucunuz Twilio'nın HTTP isteğiyle yanıtlar [TwiML][twiml], arama veya kısa mesaj işlemek yönergeler içeren XML etiketleri basit bir dizi. Yalnızca bir dakika içinde TwiML örneklerde göreceğiz.

### <a name="making-calls-and-sending-text-messages"></a>Çağrıları yapma ve metin iletileri gönderme
Geliştiriciler, Twilio web hizmeti API'sine HTTP isteklerini yaparak, kısa mesaj göndermek veya giden telefon aramaları başlatın. Giden çağrıları için Geliştirici de onu bağlandıktan sonra giden çağrısını işlemek nasıl TwiML yönergeler döndüren bir URL belirtmeniz gerekir.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>UI kodunda (JavaScript, iOS veya Android) VoIP yetenekleri katıştırma
Twilio tüm masaüstü web tarayıcısı, iOS uygulaması ya da Android uygulaması VoIP telefon kapatabilirsiniz bir istemci-tarafı SDK sağlar. Bu makalede, biz tarayıcıda çağırma VoIP kullanma hakkında odaklanır. Ek olarak *Twilio JavaScript SDK'sı* tarayıcıda çalışan, bir sunucu-tarafı uygulaması (node.js uygulamamız) bir "özelliği" JavaScript istemciye belirteç için kullanılması gerekir. Daha fazla bilgiyi VoIP node.js ile kullanma hakkında [Twilio geliştirme blogunda][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Twilio (Microsoft iskonto) için kaydolun
Twilio hizmetlerini kullanmadan önce öncelikle [bir hesap için kaydolabilirsiniz][signup]. Microsoft Azure müşterilerin alma özel bir indirim - [burada oturum özen][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Oluşturma ve bir node.js Azure Web sitesi dağıtma
Ardından, Azure üzerinde çalışan bir node.js Web sitesi oluşturmanız gerekir. [Bunu yapmak için resmi belge burada bulunduğu][azure_new_site]. Yüksek bir düzeyde, aşağıdakileri yaparak:

* Zaten yoksa, bir Azure hesabı için kaydolma
* Yeni bir Web sitesi oluşturmak için Azure yönetim konsolunu kullanarak
* Kaynak denetimi desteği (git kullanılan varsayacağız) ekleme
* Bir dosya oluşturulurken `server.js` basit bir node.js web uygulaması ile
* Bu basit uygulamayı Azure'a dağıtma

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a>Twilio modülü Yapılandır
Ardından, biz Twilio API'si kullanmak olmasını sağlayan bir basit bir node.js uygulaması yazma başlar. Başlamadan önce bizim Twilio hesap kimlik bilgilerini yapılandırmanız gerekir.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Sistem ortam değişkenleri Twilio kimlik bilgilerini yapılandırma
Kimliği doğrulanmış istekler Twilio arka uç yapmak için hesap SID'si ve kimlik doğrulama belirteci, kullanıcı adı ve parola bizim Twilio hesabı için ayarlanan hangi işlevi ihtiyacımız var. Bunlar kullanmak için Azure düğümü modülünde ile yapılandırmak için en güvenli aracılığıyla doğrudan Azure Yönetici konsolunda ayarlayabilirsiniz sistem ortam değişkenlerini yoludur.

Node.js Web sitenizi seçin ve "Yapılandırma" bağlantısını tıklatın.  Bir bit kaydırın, uygulamanız için yapılandırma özelliklerini ayarlayabileceğiniz bir alan görürsünüz.  Twilio hesabı kimlik bilgilerinizi girin ([Twilio konsolunuzdan bulunan][twilio_console]) gösterildiği gibi - bunları ad verdiğinizden emin olun `TWILIO_ACCOUNT_SID` ve `TWILIO_AUTH_TOKEN`sırasıyla:

![Azure Yönetim Konsolu][azure-admin-console]

Bu değişkenler yapılandırdıktan sonra uygulamanızı Azure konsolunda yeniden başlatın.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Package.json Twilio modülünde bildirme
Ardından, bizim düğümü modülü bağımlılıkları aracılığıyla yönetmek için bir package.json oluşturmak ihtiyacımız [npm]. Aynı düzeyde `server.js` içinde oluşturduğunuz dosya *Azure/node.js* öğretici adlı bir dosya oluşturun `package.json`.  Bu dosya içinde aşağıdaki koyun:

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

Bu bağımlılık yanı sıra popüler twilio modülü bildirir [Express web çerçevesi] [ express] ve EJS şablon motoru.  Tamam, biz başlamaya artık hazırsınız - biraz kod yazalım!

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Giden bir çağrı yapın
Burada birkaç çağrı yerleştirir basit bir form oluşturalım. Açık `server.js`ve aşağıdaki kodu girin. Burada "CHANGE_ME" azure Web sitenizin adı var. put - diyor dikkat edin:

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

Ardından, adlı bir dizin oluşturun `views` - bu dizin içinde adlı bir dosya oluşturun `index.ejs` aşağıdaki içeriğe sahip:

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

Şimdi, Azure için Web sitenizi dağıtmak ve ev açın. Metin alanına telefon numaranızı girin ve bir çağrı Twilio numarasından almak mümkün olmalıdır!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>SMS iletisi gönder
Şimdi, şimdi kısa mesaj göndermek için bir kullanıcı arabirimi ve mantığı işleme formunu ayarlayın. Açık `server.js`ve son çağrısından sonra aşağıdaki kodu ekleyin `app.post`:

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

İçinde `views/index.ejs`, başka bir formu bir sayı ve kısa mesaj göndermek için birinci altında ekleyin:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

Azure uygulamanızı yeniden dağıtmanıza ve şimdi, form ve kendiniz (veya en yakın arkadaşlarınızın hiçbirini) SMS mesajı gönderecek gönderemeyecek olmalıdır!

<a id="nextsteps"/>

## <a name="next-steps"></a>Sonraki Adımlar
Artık, node.js ve Twilio iletişim uygulamalar oluşturmak için kullanma temelleri öğrendiniz. Ancak bu örnekler Twilio ve node.js ile olası nedir yüzeyine neredeyse hiç boş. Node.js ile Twilio kullanarak daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Resmi modülü belgeleri][docs]
* [Node.js uygulamaları ile VoIP öğretici][voipnode]
* [Votr - uygulama node.js ve CouchDB (üç bölümden) ile oylama gerçek zamanlı bir SMS][votr]
* [Node.js ile tarayıcıda çifti programlama][pair]

Azure'da node.js ve Twilio korsan memnuniyet umuyoruz!

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
