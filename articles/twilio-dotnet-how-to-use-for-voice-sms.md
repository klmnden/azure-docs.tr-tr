---
title: Twilio ses ve SMS (.NET) için nasıl kullanılacağı | Microsoft Docs
description: Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. .NET ile yazılan kod örnekleri.
services: ''
documentationcenter: .net
author: devinrader
manager: twilio
editor: ''
ms.assetid: 74d4f3c9-f1cb-4968-b744-36b32cd0e834
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/24/2015
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 1442e3af26ae87e645cf207228ed1197b2afdd4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23877030"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Twilio ses ve Azure SMS özelliklerini kullanma
Bu kılavuz, Azure üzerinde Twilio API hizmeti genel programlama görevleri gerçekleştirmek gösterilmiştir. Kapsamdaki senaryolar bir telefon araması yapmadan ve kısa ileti hizmeti (SMS) ileti gönderme içerir. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz: [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio iş iletişimleri, ses, VoIP ve uygulamalara Mesajlaşma geliştiricilerin etkinleştirme geleceği destekleyen. Bunlar Twilio iletişim API platformu gösterme bulut tabanlı, genel bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklendirilebilir. Kullandıkça Öde fiyatlandırma ile esnekliğinin ve bulut güvenilirlik ' yararlanabilirsiniz.

**Twilio sesli** yapmak ve telefon çağrılarını almak, uygulamalarınızın sağlar. **Twilio SMS** SMS iletileri göndermek ve almak için uygulamalarınızı sağlar. **Twilio istemci** VoIP çağrıları herhangi telefon, tablet ya da tarayıcı yapmanızı sağlar ve WebRTC destekler.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterilerin alacak bir [özel teklif](http://www.twilio.com/azure): 10 ücretsiz Twilio Twilio hesabınızı yükseltirken kredisi. Bu Twilio kredi varsa Twilio kullanımını ($10 alacak kadar 1.000 SMS iletileri göndermek ya da telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesli dakika alırken eşdeğerdir) uygulanabilir. Bu Twilio iade almak ve adresindeki başlama [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio Kullandıkça Ödeme tabanlı bir hizmettir. Hiçbir Kurulum ücretleri vardır ve herhangi bir zamanda hesabınızı kapatabilirsiniz. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma](http://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Kavramları
Twilio API uygulamaları için ses ve SMS işlevselliği sağlayan bir RESTful API'dır. İstemci kitaplıkları, birden çok dilde kullanılabilir; bir listesi için bkz: [Twilio API kitaplıkları][twilio_libraries].

Twilio API anahtar yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) ' dir.

### <a id="Verbs"></a>Twilio fiiller
API Twilio yararlanır fiiller; Örneğin,  **&lt;Say&gt;**  fiili kullanımı bir çağrıda bir ileti teslim Twilio bildirir.

Twilio fiillerin listesi verilmiştir.  Diğer fiilleri ve aracılığıyla özellikleri hakkında bilgi edinin [Twilio biçimlendirme dili belgeleri](http://www.twilio.com/docs/api/twiml).

* **&lt;Arama&gt;**: başka bir telefon çağıran bağlanır.
* **&lt;Toplama&gt;**: telefon tuş takımında girilen Sayısal basamaklar toplar.
* **&lt;Kapat&gt;**: bir aramasını sonlandırır.
* **&lt;Yürüt&gt;**: bir ses dosyası çalar.
* **&lt;Duraklatma&gt;**: sessizce belirtilen sayıda saniye bekler.
* **&lt;Kayıt&gt;**: arayanın sesli kayıtlar ve kayıt içeren bir dosyayı bir URL'sini döndürür.
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

## <a id="create_app"></a>Azure uygulama oluşturma
Etkin Twilio uygulamasını barındıran Azure uygulaması herhangi diğer Azure uygulamasından farklı değildir. Twilio .NET kitaplığı ekleyip Twilio .NET kitaplıklarına kullanmak için rol yapılandırabilirsiniz.
İlk Azure projesi oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio ile bir Azure projesi oluşturma][vs_project].

## <a id="configure_app"></a>Twilio kitaplıkları kullanmak için uygulamanızı yapılandırın
Twilio Twilio TwiML yanıtları oluşturmak Twilio REST API ve Twilio istemci ile etkileşim kurmak için basit ve kolay yollar sağlamak için çeşitli yönlerini sarmalamak .NET Yardımcısı kitaplıkları kümesi sağlar.

Twilio .NET geliştiricileri için beş kitaplıkları sağlar:
Kitaplık|Açıklama
---|---
Twilio.API|Twilio REST API kolay .NET Kitaplığı'nda sarmalar çekirdek Twilio kitaplığı. Bu kitaplık, .NET, Silverlight ve Windows Phone 7 için kullanılabilir.
Twilio.TwiML|TwiML biçimlendirme oluşturmak için bir .NET kolay yolunu sunar.
Twilio.MVC|ASP.NET MVC kullanan geliştiriciler için bu kitaplığı TwilioController, TwiML ActionResult ve istek doğrulama özniteliği içerir.
Twilio.WebMatrix|Microsoft'un ücretsiz WebMatrix geliştirme aracını kullanarak geliştiriciler için bu kitaplık çeşitli Twilio eylemler için Razor sözdizimi Yardımcıları içerir.
Twilio.Client.Capability|Twilio istemci JavaScript SDK'sı ile kullanılmak üzere yetenek belirteç Oluşturucu içerir.

Tüm kitaplıkları .NET 3.5, Silverlight 4 veya Windows Phone 7 veya üzeri gerektiğini unutmayın.

Bu kılavuzda sağlanan örnekleri Twilio.API kitaplığını kullanın.

Kitaplıkları olabilir [NuGet Paket Yöneticisi uzantısı kullanılarak yüklenen](http://www.twilio.com/docs/csharp/install) 2015 kadar Visual Studio 2010 için kullanılabilir.  Kaynak kodu barındırılan [GitHub][twilio_github_repo], kitaplıklarını kullanma için kapsamlı belgeler içeren bir Wiki içerir.

Varsayılan olarak, Microsoft Visual Studio 2010 NuGet 1.2 sürümünü yükler. Twilio kitaplıkları yükleme sürüm 1.6 NuGet veya üstü gerektirir. Yükleme veya NuGet güncelleştirme hakkında daha fazla bilgi için bkz: [http://nuget.org/][nuget].

> [!NOTE]
> NuGet'ın en son sürümünü yüklemek için önce Visual Studio Uzantı Yöneticisi'ni kullanarak yüklenen sürümü kaldırmanız gerekir. Bunu yapmak için Visual Studio'yu yönetici olarak çalıştırmanız gerekir. Aksi takdirde kaldırma düğmesi devre dışıdır.
>
>

### <a id="use_nuget"></a>Twilio kitaplıkları Visual Studio projenize eklemek için:
1. Çözümünüzü Visual Studio'da açın.
2. Sağ **başvurular**.
3. Tıklatın **NuGet paketlerini Yönet...**
4. Tıklatın **çevrimiçi**.
5. Arama çevrimiçi kutuya yazın *twilio*.
6. Tıklatın **yükleme** Twilio paketinizdeki.

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Aşağıdaki çağrıda giden yapılacağını gösterir **CallResource** sınıfı. Bu kod bir Twilio tarafından sağlanan site Twilio biçimlendirme dili (TwiML) yanıt döndürmek için de kullanır. Kendi değerlerinizi yerleştirin **için** ve **gelen** telefon numaraları ve doğrulamanız olun **gelen** telefon numarası Twilio hesabınız için kod çalıştırmadan önce.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    var url = "http://twimlets.com/message";
    url = $"{url}?Message%5B0%5D=Hello%20World";

    // Set the call From, To, and URL values to use for the call.
    // This sample uses the sandbox number provided by
    // Twilio to make the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri(url));
        }

İçin geçirilen parametreler hakkında daha fazla bilgi için **CallResource.Create** yöntemi, bkz: [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz. Daha fazla bilgi için bkz: [nasıl yapılır: sağlamak TwiML yanıtları kendi Web sitesinden](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl yapılır: bir SMS iletisi gönderin
Aşağıdaki ekran görüntüsü kullanarak bir SMS iletisi göndermek nasıl gösterir **MessageResource** sınıfı. **Gelen** numarası SMS iletileri göndermek için tarafından deneme hesapları için Twilio sağlanır. **İçin** numarası gerekir doğrulandı Twilio hesabınız için kod çalıştırmadan önce.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    const string accountSID = "your_twilio_account";
    const string authToken = "your_twilio_authentication_token";

    // Initialize the TwilioClient.
    TwilioClient.Init(accountSID, authToken);

    try
    {
        // Send an SMS message.
        var message = MessageResource.Create(
            to: new PhoneNumber("+12069419717"),
            from: new PhoneNumber("+14155992671"),
            body: "This is my SMS message.");
    }
    catch (TwilioException ex)
    {
        // An exception occurred making the REST call
        Console.WriteLine(ex.Message);
    }

## <a id="howto_provide_twiml_responses"></a>Nasıl yapılır: kendi Web sitesinden TwiML yanıtlarını sağlar
Olduğunda, uygulamanızın başlatır - Örneğin, Twilio API çağrısı aracılığıyla **CallResource.Create** yöntemi - Twilio gönderir isteğiniz TwiML yanıt döndürmek için beklenen bir URL. Örnekte [nasıl yapılır: giden bir çağrı yapmak](#howto_make_call) Twilio tarafından sağlanan URL'yi kullanır [http://twimlets.com/message] [ twimlet_message_url] yanıt dönün.

> [!NOTE]
> TwiML web hizmetleri tarafından kullanılmak üzere tasarlandığından, TwiML tarayıcınızda görüntüleyebilirsiniz. Örneğin, [http://twimlets.com/message] [ twimlet_message_url] boş bir görmek için &lt;yanıt&gt; öğesi; başka bir örnek olarak, tıklatın [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) görmek için bir &lt;yanıt&gt; içeren öğe bir &lt;Say&gt; öğesi.
>
>

Twilio tarafından sağlanan URL üzerinde güvenmek yerine, HTTP yanıtlarını döndürür kendi URL sitesi oluşturabilirsiniz. HTTP yanıt veren herhangi bir dilde sitesi oluşturabilirsiniz. Bu konu, bir ASP.NET genel işleyici URL'den barındırma varsayar.

Aşağıdaki ASP.NET işleyicisi bildiren TwiML yanıt işler **Hello World** çağrısında.

    using System.Text;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {
            public void ProcessRequest(HttpContext context)
            {
                const string twiMLResponse =
                    "<Response><Say>Hello World.</Say></Response>";
                
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = Encoding.UTF8;
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }
    
Yukarıdaki örnekte görüldüğü gibi TwiML yanıt basitçe bir XML dosyasıdır. Twilio.TwiML kitaplığı TwiML oluşturacaktır sınıfları içerir. Aşağıdaki örnek, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır **VoiceResponse** sınıfı.

    using System.Web;
    using Twilio.TwiML;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new VoiceResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

TwiML hakkında daha fazla bilgi için bkz: [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

TwiML yanıtları sağlamanın bir yolu ayarladıktan sonra bu URL'ye geçirebilirsiniz **CallResource.Create** yöntemi. Örneğin, bir Azure bulut hizmeti dağıtılmış MyTwiML adlı bir web uygulaması varsa ve ASP.NET işleyicinizi mytwiml.ashx adıdır, URL için geçirilebilir **CallResource.Create** aşağıdaki kod örneğinde gösterildiği gibi:

    // This sample uses the sandbox number provided by Twilio to make the call.
    // Place the call.
    var call = CallResource.Create(
        to: new PhoneNumber("+NNNNNNNNNN"),
        from: new PhoneNumber("NNNNNNNNNN"),
        url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
        }

ASP.NET ile azure'da Twilio kullanma hakkında ek bilgi için bkz: [Twilio Azure üzerinde bir web rolü kullanılarak bir telefon araması yapmak nasıl][howto_phonecall_dotnet].

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
