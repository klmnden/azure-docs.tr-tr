---
title: Twilio ses ve SMS (Ruby) için nasıl kullanılacağı | Microsoft Docs
description: Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. Ruby içinde yazılan kod örnekleri.
services: ''
documentationcenter: ruby
author: devinrader
manager: twilio
editor: ''
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 69e50e7fe0e1f302e96c309878b8dea6869dff4a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866082"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Ses ve SMS özelliklerini Söyleniş için Twilio kullanma
Bu kılavuz, Azure üzerinde Twilio API hizmeti genel programlama görevleri gerçekleştirmek gösterilmiştir. Kapsamdaki senaryolar bir telefon araması yapmadan ve kısa ileti hizmeti (SMS) ileti gönderme içerir. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz: [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses ve SMS uygulamaları oluşturmak için varolan web dilleri ve yetenekleri kullanmanıza olanak sağlayan bir telefon web hizmeti API'dir. Twilio, (olmayan bir Azure özelliğidir ve Microsoft Ürün) bir bir üçüncü taraf hizmetidir.

**Twilio sesli** yapmak ve telefon çağrılarını almak, uygulamalarınızın sağlar. **Twilio SMS** yapmak ve SMS iletileri almak, uygulamalarınızın sağlar. **Twilio istemci** mobil bağlantıları dahil olmak üzere var olan Internet bağlantıları kullanarak sesli iletişimi etkinleştirmek, uygulamalarınızın sağlar.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Twilio fiyatlandırma hakkında daha fazla bilgi şu adreste [Twilio fiyatlandırma][twilio_pricing]. Azure müşterilerine alma bir [özel teklif][special_offer]: boş bir kredi 1000 metinlerinin veya 1000 dakika sayısı. Bu teklif için kaydolun veya daha fazla bilgi edinmek için lütfen ziyaret [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Kavramları
Twilio API uygulamaları için ses ve SMS işlevselliği sağlayan bir RESTful API'dır. İstemci kitaplıkları, birden çok dilde kullanılabilir; bir listesi için bkz: [Twilio API kitaplıkları][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML bir çağrı işlemek nasıl Twilio veya SMS bildiren XML tabanlı yönergeleri kümesidir.

Örnek olarak, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşma için.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Tüm TwiML dosyalarınız `<Response>` kendi kök öğesi olarak. Burada, uygulamanızın davranışını tanımlamak için Twilio fiilleri kullanın.

### <a id="Verbs"></a>TwiML fiiller
Twilio fiiller olan Twilio ne söylemek XML etiketleri **yapmak**. Örneğin,  **&lt;Say&gt;**  fiili kullanımı bir çağrıda bir ileti teslim Twilio bildirir. 

Twilio fiillerin listesi verilmiştir.

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

Twilio fiiller, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio hesabı oluşturma
Twilio hesap almak hazır olduğunuzda, oturum açın [deneyin Twilio][try_twilio]. Ücretsiz bir hesap ile başlatın ve daha sonra hesabınızı yükseltin.

Twilio hesabı için kaydolduğunuzda, uygulamanız için bir ücretsiz telefon numarası elde edersiniz. Ayrıca, bir hesap SID'si ve bir kimlik doğrulama belirteci alırsınız. Her ikisi de Twilio API çağrıları yapmanız gerekecektir. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirteci güvenli tutun. Hesabınızın SID ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio hesap sayfası][twilio_account], etiketli alanları **HESABININ SID** ve **kimlik doğrulama BELİRTECİ**, sırasıyla.

### <a id="VerifyPhoneNumbers"></a>Telefon numaralarını doğrulayın
Yanı sıra numarası Twilio tarafından verilen de doğrulayabilirsiniz uygulamalarınızda kullanım için kontrol ettiğiniz (yani, cep telefonu veya ev telefon numarası) numaralandırır. 

Bir telefon numarası doğrulama hakkında daha fazla bilgi için bkz: [yönetmek numaraları][verify_phone].

## <a id="create_app"></a>Ruby uygulaması oluşturma
Twilio hizmeti kullanır ve Azure üzerinde çalışan bir Ruby uygulaması Twilio hizmetini kullanan tüm diğer Söyleniş uygulamadan daha farklı değildir. Twilio Hizmetleri RESTful ve Ruby çeşitli yollarla çağrılabilir olsa da, bu makalede Twilio Hizmetleri ile kullanmak üzere nasıl odaklanacaktır [Twilio yardımcı kitaplık Ruby için][twilio_ruby].

İlk olarak, [Kurulum yeni bir Azure Linux VM] [ azure_vm_setup] yeni Söyleniş web uygulamanız için bir konak olarak hareket edecek. Rayları uygulama, yalnızca Kurulum VM oluşturulmasını içeren adımları yoksayın. Bir dış bağlantı noktası 80 ve 5000 iç bir bağlantı noktası ile bir uç nokta oluşturduğunuzdan emin olun.

Aşağıdaki örneklerde, biz kullanacağınız [Sinatra][sinatra], Ruby için çok basit web çerçevesi. Ancak kesinlikle Twilio yardımcı kitaplık için Ruby Ruby rayları üzerinde de dahil olmak üzere tüm diğer web framework ile kullanabilirsiniz.

Yeni VM'nin içine SSH ve yeni uygulamanız için bir dizin oluşturun. Bu dizin içinde Gemfile adlı bir dosya oluşturun ve içine aşağıdaki kodu kopyalayın:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Komut satırını Çalıştır üzerinde `bundle install`. Bu, yukarıdaki bağımlılıkları yükler. Sonraki adlı bir dosya oluşturun `web.rb`. Bu, web uygulamanız için kod nerede yaşıyor olacaktır. Aşağıdaki kodu yapıştırın:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Bu noktada görüyor olmalısınız komutunu çalıştırın `ruby web.rb -p 5000`. Bu dönüş-bağlantı noktası 5000 küçük web sunucusunda yukarı. Tarayıcınız bu uygulama için URL'yi ziyaret ederek Kurulum Azure VM için göz olması gerekir. Web uygulamanızı tarayıcıda ulaştıktan sonra bir Twilio uygulaması oluşturmaya başlamak hazırsınız.

## <a id="configure_app"></a>Uygulamanızı Twilio kullanacak şekilde yapılandırma
Web uygulamanızı güncelleştirerek Twilio kitaplığını kullanacak şekilde yapılandırabilirsiniz, `Gemfile` bu satır eklemek için:

    gem 'twilio-ruby'

Komut satırında çalıştırmak `bundle install`. Şimdi açmak `web.rb` ve bu satırı üst dahil olmak üzere:

    require 'twilio-ruby'

Şimdi tüm web uygulamanız için Ruby Twilio yardımcı kitaplık kullanmaya hazırsınız.

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Giden bir çağrı yapma gösterir. Temel kavramları Twilio yardımcı kitaplık Ruby için REST API çağrıları yapma kullanmayı ve TwiML işleme içerir. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kod çalıştırılmadan önce Twilio hesabınız için telefon numarası.

Bu işlev eklemek `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

Açık yukarı ise `http://yourdomain.cloudapp.net/make_call` bir tarayıcıda, tetiklemek telefon araması yapmak için Twilio API çağrısı. İlk iki parametre `client.account.calls.create` oldukça kendinden açıklamalıdır: sayı çağrıdır `from` ve sayı çağrı `to`. 

Üçüncü parametre (`url`) Twilio çağrı bağlandıktan sonra yapmanız gerekenler hakkında yönergeler almak için ister URL'dir. Bu durumda biz Kurulum bir URL (`http://yourdomain.cloudapp.net`), basit bir TwiML belge döndürür ve kullanır `<Say>` fiil bazı okuma yapmak ve çağrı alan kişiye "Merhaba Monkey" söyleyin.

## <a id="howto_recieve_sms"></a>Nasıl yapılır: alma SMS iletisi
Önceki örnekte biz başlatılan bir **giden** telefon araması. Bu kez, bize sırasında Twilio verdiği telefon numarasını kullanalım kaydolma işlemi için bir **gelen** SMS iletisi.

Birincisi, oturum açma için sizin [Twilio Pano][twilio_account]. "Numaralarında" içinde üst gezinme ve size sağlanan Twilio sayısına tıklayın. Yapılandırabileceğiniz iki URL'leri görürsünüz. Sesli istek URL'si ve bir SMS istek URL'si. Twilio numaranızı gönderilen bir SMS veya telefon görüşmesi yapılan her çağrı URL'leri bunlar. URL'leri "web kancaları" da bilinir.

İsteriz gelen SMS iletileri işlemek için URL'ye sağlandığından güncelleştirme `http://yourdomain.cloudapp.net/sms_url`. Bir tane sayfanın sonundaki değişiklikleri Kaydet'i tıklatın. Şimdi, geri `web.rb` şimdi uygulamamız bu durumu çözmek için program:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Değişikliği yaptıktan sonra web uygulamanızı yeniden başlatmayı emin olun. Şimdi telefonunuz alın ve Twilio numaranızı bir SMS gönder. Derhal "Hey, ping işlemi için teşekkür ederiz! bildiren bir SMS yanıt almanız gerekir Twilio ve Azure rock! ".

## <a id="additional_services"></a>Nasıl yapılır: ek Twilio Hizmetleri kullanın
Burada gösterilen örnekler yanı sıra, Azure uygulamanız ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler Twilio sunar. Ayrıntılar için bkz: [Twilio API belgelerine][twilio_api_documentation].

### <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmetinin öğrendiğinize göre daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio HowTos ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts] 
* [Github'da Twilio][twilio_on_github]
* [Twilio desteklemek için konuşun][twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
