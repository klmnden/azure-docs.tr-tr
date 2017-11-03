---
title: "Twilio ses ve SMS (Python) için nasıl kullanılacağı | Microsoft Docs"
description: "Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. Kod örnekleri Python içinde yazılmış."
services: 
documentationcenter: python
author: devinrader
manager: twilio
editor: 
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: f4a02bb7a7c46e7a0e3c75b870c522eae8294339
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a>Ses ve Python SMS özelliklerini için Twilio kullanma
Bu kılavuz, Azure üzerinde Twilio API hizmeti genel programlama görevleri gerçekleştirmek gösterilmiştir. Kapsamdaki senaryolar bir telefon araması yapmadan ve kısa ileti hizmeti (SMS) ileti gönderme içerir. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz: [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio iş iletişimleri, ses, VoIP ve uygulamalara Mesajlaşma geliştiricilerin etkinleştirme geleceği destekleyen. Bunlar Twilio iletişim API platformu gösterme bulut tabanlı, genel bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklendirilebilir. Sizinle ödeme-olarak esnekliğinin fiyatlandırma gidin ve bulut güvenilirlik ' yararlanır.

**Twilio sesli** yapmak ve telefon çağrılarını almak, uygulamalarınızın sağlar.
**Twilio SMS** metin ileti gönderme ve alma için uygulamanızı sağlar.
**Twilio istemci** VoIP çağrıları herhangi telefon, tablet ya da tarayıcı yapmanızı sağlar ve WebRTC destekler.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterilerin alacak bir [özel teklif] [ special_offer] 10 Twilio Twilio hesabınızı yükseltirken kredisi. Bu Twilio kredi varsa Twilio kullanımını ($10 alacak kadar 1.000 SMS iletileri göndermek ya da telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesli dakika alırken eşdeğerdir) uygulanabilir. Bu kullanmak [Twilio kredi] [ special_offer] ve çalışmaya başlama.

Twilio Kullandıkça Ödeme tabanlı bir hizmettir. Hiçbir Kurulum ücretleri vardır ve herhangi bir zamanda hesabınızı kapatabilirsiniz. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma][twilio_pricing].

## <a id="Concepts"></a>Kavramları
Twilio API uygulamaları için ses ve SMS işlevselliği sağlayan bir RESTful API'dır. İstemci kitaplıkları, birden çok dilde kullanılabilir; bir listesi için bkz: [Twilio API kitaplıkları][twilio_libraries].

Twilio API anahtar yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) ' dir.

### <a id="Verbs"></a>Twilio fiiller
API Twilio yararlanır fiiller; Örneğin,  **&lt;Say&gt;**  fiili kullanımı bir çağrıda bir ileti teslim Twilio bildirir.

Twilio fiillerin listesi verilmiştir. Diğer fiilleri ve aracılığıyla özellikleri hakkında bilgi edinin [Twilio biçimlendirme dili belgeleri][twiml].

* **&lt;Arama&gt;**: başka bir telefon çağıran bağlanır.
* **&lt;Toplama&gt;**: telefon tuş takımında girilen Sayısal basamaklar toplar.
* **&lt;Kapat&gt;**: bir aramasını sonlandırır.
* **&lt;Duraklatma&gt;**: sessizce belirtilen sayıda saniye bekler.
* **&lt;Yürüt&gt;**: bir ses dosyası çalar.
* **&lt;Sıra&gt;**: ekleme arayanlar sırasına.
* **&lt;Kayıt&gt;**: arayan sesli kayıtlar ve kayıt içeren bir dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;**: farklı bir URL'de TwiML çağrısı veya SMS denetim aktarır.
* **&lt;Reddetme&gt;**: faturalama olmadan Twilio numaranızı için bir gelen çağrıyı reddeder.
* **&lt;Söyleyin&gt;**: dönüştürür metin bir çağrıda yapılan okuma.
* **&lt;SMS&gt;**: SMS iletisi gönderir.

### <a id="TwiML"></a>TwiML
TwiML bir çağrı işlemek nasıl Twilio veya SMS bildiren Twilio fiilleri dayalı XML tabanlı yönergeleri kümesidir.

Örnek olarak, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşma için.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Uygulamanız Twilio API çağırdığında API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amaçlı uygulamalarınız tarafından kullanılan TwiML yanıt sağlamanız için sağlanan Twilio URL'leri kullanabilirsiniz. TwiML yanıtları oluşturmak üzere kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır `TwiMLResponse` nesnesi.

Twilio fiiller, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API][twilio_api].

## <a id="CreateAccount"></a>Twilio hesabı oluşturma
Twilio hesap almak hazır olduğunuzda, oturum açın [deneyin Twilio][try_twilio]. Ücretsiz bir hesap ile başlatın ve daha sonra hesabınızı yükseltin.

Twilio hesabı için kaydolduğunuzda, SID bir hesap ve kimlik doğrulama belirtecini alır. Her ikisi de Twilio API çağrıları yapmanız gerekecektir. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirteci güvenli tutun. Hesabınızın SID ve kimlik doğrulama belirteci içinde görüntülenebilir [Twilio konsol][twilio_console], etiketli alanları **HESABININ SID** ve **kimlik doğrulama BELİRTECİ**sırasıyla.

## <a id="create_app"></a>Python uygulaması oluşturma
Twilio hizmeti kullanır ve Azure üzerinde çalışan bir Python uygulaması Twilio hizmetini kullanan tüm diğer Python uygulamadan daha farklı değildir. Twilio Hizmetleri REST tabanlı ve Python çeşitli yollarla çağrılabilir olsa da, bu makalede Twilio Hizmetleri ile kullanmak üzere nasıl odaklanacaktır [github'dan Python için Twilio Kitaplığı][twilio_python]. Python için Twilio kitaplığını kullanma hakkında daha fazla bilgi için bkz: [http://readthedocs.org/docs/twilio-python/en/latest/index.html][twilio_lib_docs].

İlk olarak, [Kurulum yeni bir Azure Linux VM], yeni bir Python web uygulaması için bir konak olarak davranacak şekilde azure_vm_setup. Sanal makine çalışmaya başladıktan sonra aşağıda açıklandığı gibi genel bir bağlantı noktası, uygulamanızda açmanız gerekir.

### <a name="add-an-incoming-rule"></a>Bir gelen kuralı ekleyin
  1. [Ağ güvenlik grubu] [azure_nsg] sayfasına gidin.
  2. Ağ güvenlik grubu, sanal makineyle karşılık gelen seçin.
  3. Ekleme ve **giden kuralı** için **bağlantı noktası 80**. Herhangi bir adresinden gelen izin vermek emin olun.

### <a name="set-the-dns-name-label"></a>DNS ad etiketi ayarlayın
  1. [Genel IP adresi] [azure_ips] sayfasına gidin.
  2. Sanal makineniz ile bu correspends ortak IP adresi seçin.
  3. Ayarlama **DNS ad etiketi** içinde **yapılandırma** bölümü. Bu örnekte aşağıdakine benzer görünecektir *etki alanı etiketi bilgisayarınızı*. centralus.cloudapp.azure.com

Sanal makineye SSH üzerinden bağlanabilmek için bir kez tercih ettiğiniz Web çerçevesi yükleyebilirsiniz (en iyi Python olan bilinen iki [Flask](http://flask.pocoo.org/) ve [Django](https://www.djangoproject.com)). Bunlardan birini yalnızca çalıştırarak yükleyebilirsiniz `pip install` komutu.

Sanal makine yalnızca bağlantı noktası 80 üzerinde trafiğe izin verecek şekilde yapılandırılmış olduğunu aklınızda bulundurun. Bu nedenle bu bağlantı noktasını kullanmak üzere uygulamayı yapılandırdığınızdan emin olun.

## <a id="configure_app"></a>Uygulamanızı Twilio kitaplıkları kullanacak şekilde yapılandırma
Uygulamanızı Twilio kitaplığı Python için iki yolla kullanacak şekilde yapılandırabilirsiniz:

* Twilio kitaplığı Python için PIP paket olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pip install twilio

    -VEYA-

* Twilio kitaplığı Python github'dan indirin ([https://github.com/twilio/twilio-python][twilio_python]) ve aşağıdaki gibi yükleyin:

        $ python setup.py install

Python için Twilio kitaplığı yükledikten sonra böylece `import` Python dosyalarınızı içinde:

        import twilio

Daha fazla bilgi için bkz: [https://github.com/twilio/twilio-python/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Giden bir çağrı yapma gösterir. Bu kod bir Twilio tarafından sağlanan site Twilio biçimlendirme dili (TwiML) yanıt döndürmek için de kullanır. Kendi değerlerinizi yerleştirin **from_number** ve **to_number** telefon numaraları ve doğrulandıktan emin olun **from_number** telefon numarası Twilio hesabınız için kod çalıştırmadan önce.

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "http://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için bkz: [nasıl TwiML yanıtlar sağlayan bilgisayarınızı kendi Web sitesinden](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl yapılır: bir SMS iletisi gönderin
Aşağıdaki bir SMS kullanarak ileti gönder gösterilmektedir `TwilioRestClient` sınıfı. **From_number** numarası SMS iletileri göndermek için tarafından deneme hesapları için Twilio sağlanır. **To_number** numarası gerekir doğrulandı Twilio hesabınız için kod çalıştırmadan önce.

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    from_number = "NNNNNNNNNNN"  # With trial account, texts can only be sent from your Twilio number.
    to_number = "NNNNNNNNNNN"
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Send the SMS message.
    message = client.messages.create(to=to_number,
                                     from_=from_number,
                                     body=message)

## <a id="howto_provide_twiml_responses"></a>Nasıl yapılır: kendi Web sitesinden TwiML yanıtlarını sağlar
Uygulamanızı Twilio API çağrısı başlattığında Twilio TwiML yanıt döndürmek için beklenen bir URL isteğinizi gönderin. Yukarıdaki örnekte Twilio tarafından sağlanan URL'yi kullanır [http://twimlets.com/message][twimlet_message_url]. (TwiML Twilio tarafından kullanılmak üzere tasarlandığından, tarayıcınızda görüntüleyebilirsiniz. For example, tıklatın [http://twimlets.com/message] [ twimlet_message_url] boş bir görmek için `<Response>` öğesi; başka bir örnek olarak, tıklatın [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] görmek için bir `<Response>` içeren öğe bir `<Say>` öğesi.)

Twilio tarafından sağlanan URL üzerinde güvenmek yerine, HTTP yanıtlarını döndürür, kendi site oluşturabilirsiniz. XML yanıtlarını döndürür herhangi bir dilde site oluşturabilirsiniz; Bu konu Python TwiML oluşturmak için kullanacağınız varsayar.

Aşağıdaki örnekler bildiren TwiML yanıt çıkış **Hello World** çağrısında.

Flask ile:

    from flask import Response
    @app.route("/")
    def hello():
        xml = '<Response><Say>Hello world.</Say></Response>'
        return Response(xml, mimetype='text/xml')

Django ile:

    from django.http import HttpResponse
    def hello(request):
        xml = '<Response><Say>Hello world.</Say></Response>'
        return HttpResponse(xml, content_type='text/xml')

Yukarıdaki örnekte görüldüğü gibi TwiML yanıt basitçe bir XML dosyasıdır. Python için Twilio kitaplığı TwiML oluşturacaktır sınıfları içerir. Aşağıdaki örnek, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır `twiml` modülü Python Twilio Kitaplığı'nda:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

TwiML hakkında daha fazla bilgi için bkz: [https://www.twilio.com/docs/api/twiml][twiml_reference].

TwiML yanıt sağlamanız için ayarlamanız Python uygulamanızı sahip olduktan sonra içine URL geçti olarak uygulama URL'sini kullanın `client.calls.create` yöntemi. Adlı bir Web uygulaması varsa, örneğin, **MyTwiML** barındırılan bir Azure dağıtılan hizmet, aşağıdaki örnekte gösterildiği gibi Web kancası olarak URL'sini kullanabilirsiniz:

    from twilio.rest import TwilioRestClient

    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"
    from_number = "NNNNNNNNNNN"
    to_number = "NNNNNNNNNNN"
    url = "http://your-domain-label.centralus.cloudapp.azure.com/MyTwiML/"

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url)
    print(call.sid)

## <a id="AdditionalServices"></a>Nasıl yapılır: ek Twilio Hizmetleri kullanın
Burada gösterilen örnekler yanı sıra, Azure uygulamanız ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler Twilio sunar. Ayrıntılar için bkz: [Twilio API belgelerine][twilio_api].

## <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmeti temel bilgileri öğrendiniz, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl yapılır kılavuzları ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts]
* [Github'da Twilio][twilio_on_github]
* [Twilio desteklemek için konuşun][twilio_support]

[special_offer]: http://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-python/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
