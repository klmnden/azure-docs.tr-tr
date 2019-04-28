---
title: Twilio ses ve SMS (Python) için kullanma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. Python'da yazılan kod örneklerini.
services: ''
documentationcenter: python
author: devinrader
manager: twilio
editor: ''
ms.assetid: 561bc75b-4ac4-40ba-bcba-48e901f27cc3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: e6cfd9e72dc1a38e4ed0c11320336ccc4b44a2c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61457677"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-python"></a>Ses ve SMS özellikleri python'da için Twilio kullanma
Bu kılavuzda, Azure üzerinde Twilio API'si hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Telefon görüşmesi yapma ve kısa mesaj servisi (SMS) ileti gönderme senaryoları ele alınmaktadır. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz. [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses, VoIP ve mesajlaşma uygulamaları gömmek geliştiricilerin iş iletişimleri, geleceğin güçlendiren. Bunlar, Twilio iletişimleri API platformu ile gösterme genel, bulut tabanlı bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklenebilir. Sizinle ödeme-olarak esnekliğinin fiyatlandırma gidin ve bulut güvenilirlik ' yararlanabilir.

**Twilio ses** yapıp telefon çağrılarını almak, uygulamaların sağlar.
**Twilio SMS** uygulamanızın gönderin ve metin iletileri almasına olanak tanır.
**Twilio istemci** WebRTC destekler ve herhangi bir telefon, tablet veya tarayıcı VoIP çağrı yapmak sağlar.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterileri alır bir [özel teklif] [ special_offer] Twilio, Twilio hesabınızın yükselttiğinizde kredi 10 ABD Doları. Twilio kredi herhangi bir Twilio kullanım (10 ABD Doları kredi 1.000 adede kadar SMS mesajları göndermek veya telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesi dakika alma eşdeğerdir) uygulanabilir. Bu ürünü kullananlar [Twilio kredi] [ special_offer] ve çalışmaya başlayın.

Twilio, bir Kullandıkça Öde hizmetidir. Kurulum ücret yoktur ve hesabınızı dilediğiniz zaman kapatabilirsiniz. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma][twilio_pricing].

## <a id="Concepts"></a>Kavramları
Twilio ses ve SMS işlevselliğini uygulamaları için sağlayan bir RESTful API API'dir. Birden fazla dilde istemci kitaplıkları vardır; bir liste için bkz. [Twilio API kitaplıkları][twilio_libraries].

Twilio API'si önemli yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) var.

### <a id="Verbs"></a>Twilio Verbs
Twilio'yu kullanarak API yapar; fiiller Örneğin, **&lt;Say&gt;** fiil kullanımı bir çağrıda bir iletiyi teslim etmek için Twilio bildirir.

Twilio fiillerin listesi verilmiştir. Diğer fiilleri ve aracılığıyla özellikler hakkında bilgi edinin [Twilio işaretleme dili belge][twiml].

* **&lt;Arama&gt;**: Çağıran, başka bir telefonu bağlanır.
* **&lt;Gather&gt;**: Telefon tuş takımında girilen sayı toplar.
* **&lt;Kapat&gt;**: Bir çağrı sona erer.
* **&lt;Duraklatma&gt;**: Sessiz bir şekilde belirtilen sayıda saniye bekler.
* **&lt;Play&gt;**: Ses dosyası yürütülür.
* **&lt;Kuyruk&gt;**: Ekleme çağıranlar kuyruğuna.
* **&lt;Kayıt&gt;**: Arayanın ses kayıtlarını ve kayıt içeren dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;**: Arama veya SMS için farklı bir URL'de TwiML aktarımları denetim.
* **&lt;Reddetme&gt;**: Faturalama olmadan Twilio numaranızı için gelen bir çağrıyı reddeder.
* **&lt;Söyleyin&gt;**: Metin, üzerinde bir çağrı yapılır okuma dönüştürür.
* **&lt;SMS&gt;**: Bir SMS mesajı gönderir.

### <a id="TwiML"></a>TwiML
TwiML çağrı işlemek nasıl Twilio veya SMS konusunda bilgilendiren Twilio fiilleri XML tabanlı yönergeleri kümesidir.

Örneğin, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşmaya.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
      <Say>Hello World</Say>
    </Response>

Uygulamanız için Twilio API'sini çağırdığında, API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amacıyla, Twilio tarafından sağlanan URL uygulamalarınız tarafından kullanılan TwiML yanıtlar sağlamak için kullanabilirsiniz. TwiML yanıtları üretmek için kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır `TwiMLResponse` nesne.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API'si][twilio_api].

## <a id="CreateAccount"></a>Twilio hesap oluşturma
Twilio hesap almak hazır olduğunuzda, adresinde kaydolun [deneyin Twilio][try_twilio]. Ücretsiz bir hesapla yeniden başlayın ve daha sonra hesabınızı yükseltin.

Twilio hesabınız için kaydolduğunuzda, bir hesap SID'si ve bir kimlik doğrulama belirteci alırsınız. Hem Twilio API çağrıları gerçekleştirmek için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvenli tutun. Hesap SID'si ve kimlik doğrulama belirteci içinde görüntülenebilir [Twilio konsol][twilio_console], etiketli alanları **hesap SID'si** ve **AUTH TOKEN**sırasıyla.

## <a id="create_app"></a>Bir Python uygulaması oluşturma
Twilio hizmeti kullanan ve Azure'da çalışan bir Python uygulaması, Twilio hizmeti kullanan tüm diğer Python uygulamaları farklı değildir. Twilio Hizmetleri REST tabanlı ve çeşitli yollarla Python'dan çağrılabilir olsa da bu makalede Twilio Hizmetleri ile kullanma hakkında odaklanacaktır [github'dan Python için Twilio Kitaplığı][twilio_python]. Python için Twilio kitaplığını kullanma hakkında daha fazla bilgi için bkz. [ https://www.twilio.com/docs/libraries/python ] [ twilio_lib_docs].

İlk olarak, [Kurulum yeni bir Azure Linux VM], yeni bir Python web uygulaması için bir konak görevi görecek şekilde azure_vm_setup. Sanal makine çalışmaya başladığında, aşağıda açıklandığı gibi bir genel bağlantı noktasını uygulama kullanıma sunmak ihtiyacınız olacak.

### <a name="add-an-incoming-rule"></a>Bir gelen kuralı ekleyin
  1. [Ağ güvenlik grubu] [azure_nsg] sayfasına gidin.
  2. Karşılık gelen ağ güvenlik grubu ile sanal makinenizi seçin.
  3. Ekleme ve **giden kuralı** için **bağlantı noktası 80**. Herhangi bir adresten gelen izin verdiğinizden emin olun.

### <a name="set-the-dns-name-label"></a>DNS ad etiketi ayarlama
  1. [Genel IP adresleri] [azure_ips] sayfasına gidin.
  2. Genel karşılık gelen IP ile sanal makinenizi seçin.
  3. Ayarlama **DNS ad etiketi** içinde **yapılandırma** bölümü. Bu örnekte aşağıdakine benzer görünecektir *etki alanı etiketi your*. centralus.cloudapp.azure.com

Sanal makineye SSH aracılığıyla bağlanmak, mümkün olduğunda kendi tercih ettiğiniz bir Web çerçevesi yükleyebilirsiniz (en iyi Python olduğu bilinen iki [Flask](http://flask.pocoo.org/) ve [Django](https://www.djangoproject.com)). Bunlardan birini hemen çalıştırarak yükleyin `pip install` komutu.

Sanal makine yalnızca 80 numaralı bağlantı noktasında trafiğe izin verecek şekilde yapılandırılmış olduğunu aklınızda bulundurun. Bu nedenle uygulamanın bu bağlantı noktasını kullanacak şekilde yapılandırdığınızdan emin olun.

## <a id="configure_app"></a>Twilio kitaplıkları kullanmak için uygulamanızı yapılandırma
Uygulamanızı Twilio kitaplığı iki yolla için Python kullanacak şekilde yapılandırabilirsiniz:

* Twilio kitaplığı için Python Pip paket olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pip install twilio

    -VEYA-

* Twilio kitaplığı Python github'dan indirin ([https://github.com/twilio/twilio-python][twilio_python]) ve şu şekilde yükleyin:

        $ python setup.py install

Python için Twilio Kitaplığı'nı yükledikten sonra böylece `import` Python dosyalarınızı içinde:

        import twilio

Daha fazla bilgi için [twilio_github_readme](https://github.com/twilio/twilio-python/blob/master/README.rst).

## <a id="howto_make_call"></a>Nasıl Yapılır: Giden bir çağrı yapın
Giden bir çağrı yapmak nasıl gösterir. Bu kod, Twilio tarafından sağlanan bir site ayrıca Twilio biçimlendirme dili (TwiML) yanıt için kullanır. Kendi değerlerinizi yerleştirin **from_number** ve **to_number** telefon numaraları ve doğruladıktan emin olun **from_number** Twilio hesabınız için telefon numarası kod çalıştırmadan önce.

    from urllib.parse import urlencode

    # Import the Twilio Python Client.
    from twilio.rest import TwilioRestClient

    # Set your account ID and authentication token.
    account_sid = "your_twilio_account_sid"
    auth_token = "your_twilio_authentication_token"

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from_number = "NNNNNNNNNNN"

    # The number of the phone receiving call.
    to_number = "NNNNNNNNNNN"

    # Use the Twilio-provided site for the TwiML response.
    url = "https://twimlets.com/message?"

    # The phone message text.
    message = "Hello world."

    # Initialize the Twilio client.
    client = TwilioRestClient(account_sid, auth_token)

    # Make the call.
    call = client.calls.create(to=to_number,
                               from_=from_number,
                               url=url + urlencode({'Message': message}))
    print(call.sid)

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için [nasıl bilgisayarınızı kendi Web sitesinden sağlamak TwiML yanıtları](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl Yapılır: Bir SMS iletisi gönderin
Aşağıdakileri kullanarak bir SMS iletisi göndermek nasıl gösterir `TwilioRestClient` sınıfı. **From_number** numarası, SMS mesajları gönderebilir tarafından deneme hesapları için Twilio sağlanır. **To_number** numarası gerekir doğrulanabilir Twilio hesabınız için kodu çalıştırmadan önce.

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

## <a id="howto_provide_twiml_responses"></a>Nasıl Yapılır: Kendi Web sitesinden TwiML yanıtları sağlayın
Uygulamanızı Twilio API'sine çağrıda başlattığında, Twilio isteğiniz TwiML yanıt beklenen bir URL'ye gönderirsiniz. Yukarıdaki örnekte, Twilio tarafından sağlanan URL'yi kullanır [ https://twimlets.com/message ] [ twimlet_message_url]. (TwiML Twilio tarafından kullanılmak üzere tasarlandığından, tarayıcınızda görüntüleyebilirsiniz. Örneğin, [ https://twimlets.com/message ] [ twimlet_message_url] boş görmek için `<Response>` öğesi; başka bir örnek olarak, tıklayın [ https://twimlets.com/message?Message%5B0%5D=Hello%20World ] [ twimlet_message_url_hello_world]görmek için bir `<Response>` öğesini içeren bir `<Say>` öğesi.)

Twilio tarafından sağlanan URL üzerinde işlemine güvenmek yerine, HTTP yanıtlarını döndürür, kendi site oluşturabilirsiniz. XML yanıtlar döndüren herhangi bir dilde site oluşturabilirsiniz; Bu konuda, Python TwiML oluşturmak için kullanacağınız varsayılır.

Aşağıdaki örnekler bildiren TwiML yanıt çıkarır **Hello World** çağrısında.

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

Yukarıdaki örnekte görebileceğiniz gibi yalnızca bir XML belgesi TwiML yanıt. Python için Twilio kitaplığı TwiML oluşturacaktır sınıflar içerir. Aşağıdaki örnekte, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır `twiml` modülü Python Twilio Kitaplığı'nda:

    from twilio import twiml

    response = twiml.Response()
    response.say("Hello world.")
    print(str(response))

TwiML hakkında daha fazla bilgi için bkz: [ https://www.twilio.com/docs/api/twiml ] [ twiml_reference].

URL yöntemlere geçirilen gibi TwiML yanıt vermek üzere kurulan Python uygulamanızı oluşturduktan sonra uygulama URL'sini kullanın `client.calls.create` yöntemi. Örneğin, adlı bir Web uygulaması varsa **MyTwiML** barındırılan bir Azure'a dağıtılan hizmeti olarak aşağıdaki örnekte gösterildiği gibi Web kancası URL'sini kullanabilirsiniz:

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

## <a id="AdditionalServices"></a>Nasıl Yapılır: Ek Twilio hizmetlerini kullanma
Burada gösterilen örneklerden yanı sıra, Twilio, Azure uygulamanızı ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [Twilio API'si belgeleri][twilio_api].

## <a id="NextSteps"></a>Sonraki Adımlar
Twilio hizmeti temel bilgileri öğrendiniz, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl yapılır kılavuzları ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts]
* [Twilio github'da][twilio_on_github]
* [Twilio desteği için konuşma][twilio_support]

[special_offer]: https://ahoy.twilio.com/azure
[twilio_python]: https://github.com/twilio/twilio-python
[twilio_lib_docs]: https://www.twilio.com/docs/libraries/python
[twilio_github_readme]: https://github.com/twilio/twilio-python/blob/master/README.md

[twimlet_message_url]: https://twimlets.com/message
[twimlet_message_url_hello_world]: https://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: https://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_console]:  https://www.twilio.com/console
[twilio_security_guidelines]: https://www.twilio.com/docs/security
[twilio_howtos]: https://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: https://www.twilio.com/help/contact
[twilio_quickstarts]: https://www.twilio.com/docs/quickstart
