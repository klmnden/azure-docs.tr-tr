---
title: Twilio ses ve SMS (.NET) için kullanma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. .NET ile yazılan kod örneklerini içerir.
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
ms.openlocfilehash: 3b8b21de9664a969e8b1ce5699034aa9ab41d0f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60329500"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Ses ve azure'dan SMS özellikleri için Twilio kullanma
Bu kılavuzda, Azure üzerinde Twilio API'si hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Telefon görüşmesi yapma ve kısa mesaj servisi (SMS) ileti gönderme senaryoları ele alınmaktadır. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz. [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses, VoIP ve mesajlaşma uygulamaları gömmek geliştiricilerin iş iletişimleri, geleceğin güçlendiren. Bunlar, Twilio iletişimleri API platformu ile gösterme genel, bulut tabanlı bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklenebilir. Kullandıkça Öde fiyatlandırması ile esnekliğinin ve bulut güvenilirlik ' yararlanabilir.

**Twilio ses** yapıp telefon çağrılarını almak, uygulamaların sağlar. **Twilio SMS** SMS iletileri gönderip almak için uygulamalarınızı sağlar. **Twilio istemci** WebRTC destekler ve herhangi bir telefon, tablet veya tarayıcı VoIP çağrı yapmak sağlar.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterileri alır bir [özel teklif](https://www.twilio.com/azure): 10 ücretsiz Twilio Twilio hesabınızın yükselttiğinizde kredi. Twilio kredi herhangi bir Twilio kullanım (10 ABD Doları kredi 1.000 adede kadar SMS mesajları göndermek veya telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesi dakika alma eşdeğerdir) uygulanabilir. Twilio kredi almak ve başlamak [twilio.com/azure](https://twilio.com/azure).

Twilio, bir Kullandıkça Öde hizmetidir. Hesabınızı dilediğiniz zaman kapatabilirsiniz ve Kurulum ücret vardır. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma](https://www.twilio.com/voice/pricing).

## <a id="Concepts"></a>Kavramları
Twilio ses ve SMS işlevselliğini uygulamaları için sağlayan bir RESTful API API'dir. Birden fazla dilde istemci kitaplıkları vardır; bir liste için bkz. [Twilio API kitaplıkları][twilio_libraries].

Twilio API'si önemli yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) var.

### <a id="Verbs"></a>Twilio fiiller
Twilio'yu kullanarak API yapar; fiiller Örneğin, **&lt;Say&gt;** fiil kullanımı bir çağrıda bir iletiyi teslim etmek için Twilio bildirir.

Twilio fiillerin listesi verilmiştir.  Diğer fiilleri ve aracılığıyla özellikler hakkında bilgi edinin [Twilio işaretleme dili belge](https://www.twilio.com/docs/api/twiml).

* `<Dial>`: Çağıran, başka bir telefonu bağlanır.
* `<Gather>`: Telefon tuş takımında girilen sayı toplar.
* `<Hangup>`: Bir çağrı sona erer.
* `<Play>`: Ses dosyası yürütülür.
* `<Pause>`: Sessiz bir şekilde belirtilen sayıda saniye bekler.
* `<Record>`: Arayanın ses kayıtlarını ve kayıt içeren bir dosyanın bir URL döndürür.
* `<Redirect>`: Arama veya SMS için farklı bir URL'de TwiML aktarımları denetim.
* `<Reject>`: Twilio numaranızı gelen bir arama faturalama olmadan reddeder.
* `<Say>`: Metin, üzerinde bir çağrı yapılır okuma dönüştürür.
* `<Sms>`: Bir SMS mesajı gönderir.

### <a name="twiml"></a>TwiML
TwiML çağrı işlemek nasıl Twilio veya SMS konusunda bilgilendiren Twilio fiilleri XML tabanlı yönergeleri kümesidir.

Örneğin, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşmaya.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Response>
  <Say>Hello World</Say>
</Response>
```

Uygulamanız için Twilio API'sini çağırdığında, API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amacıyla, Twilio tarafından sağlanan URL uygulamalarınız tarafından kullanılan TwiML yanıtlar sağlamak için kullanabilirsiniz. TwiML yanıtları üretmek için kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır **TwiMLResponse** nesne.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API'si][twilio_api].

## <a id="CreateAccount"></a>Twilio hesap oluşturma
Twilio hesap almak hazır olduğunuzda, adresinde kaydolun [deneyin Twilio][try_twilio]. Ücretsiz bir hesapla yeniden başlayın ve daha sonra hesabınızı yükseltin.

Twilio hesabınız için kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Hem Twilio API çağrıları gerçekleştirmek için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvenli tutun. Hesap Kimliği ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio hesap sayfası][twilio_account], etiketli alanları **hesap SID'si** ve **AUTH TOKEN**sırasıyla.

## <a id="create_app"></a>Azure uygulama oluşturma
Etkin olan Twilio uygulamasını barındıran bir Azure uygulaması herhangi diğer Azure uygulamasından farklı değildir. Twilio .NET kitaplığı ekleyip Twilio .NET kitaplıklarını kullanmak için rol yapılandırabilirsiniz.
İlk bir Azure projesi oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio ile bir Azure projesi oluşturma][vs_project].

## <a id="configure_app"></a>Twilio kitaplıkları kullanmak için uygulamanızı yapılandırın
Twilio TwiML yanıtları oluşturmak için Twilio REST API ve Twilio istemci ile etkileşim kurmak için basit ve kolay şekilde sağlamak için Twilio çeşitli yönlerini kaydırma .NET yardımcı kitaplıklar kümesi sağlar.

Twilio, .NET geliştiricileri için beş kitaplıkları sağlar:

| Kitaplık | Açıklama |
| --- | --- |
| Twilio.API | Kolay bir .NET kitaplığı Twilio REST API'sini sarmalayan çekirdek Twilio kitaplığı. Bu kitaplık, .NET, Silverlight ve Windows Phone 7 için kullanılabilir. |
| Twilio.TwiML | TwiML biçimlendirme oluşturmak için bir .NET kolay yolunu sağlar. |
| Twilio.MVC | ASP.NET MVC kullanan geliştiriciler için bu kitaplık TwilioController, TwiML ActionResult ve istek doğrulama özniteliği içerir. |
| Twilio.WebMatrix | Geliştiriciler için Microsoft'un ücretsiz WebMatrix geliştirme aracını kullanarak bu kitaplığı çeşitli Twilio eylemler için Razor sözdizimi Yardımcıları içerir. |
| Twilio.Client.Capability | Twilio istemci JavaScript SDK'sı ile kullanılmak üzere yetenek belirteci Oluşturucu içerir. |

> [!Important]
> Tüm kitaplıkları, .NET 3.5, Silverlight 4'ü veya Windows Phone 7 veya üzerini gerektirir.

Bu kılavuzda sağlanan örnekleri Twilio.API kitaplığını kullanın.

Kitaplıkları olabilir [NuGet Paket Yöneticisi uzantısını kullanarak yüklü](https://www.twilio.com/docs/csharp/install) 2015 kadar Visual Studio 2010 için kullanılabilir.  Kaynak kodu barındırılan [GitHub][twilio_github_repo], kitaplıkları kullanmaya yönelik kapsamlı belgeler içeren bir Wiki içerir.

Varsayılan olarak, Microsoft Visual Studio 2010 NuGet 1.2 sürümünü yükler. Twilio kitaplıkları yükleme sürüm 1.6 NuGet veya üzerini gerektirir. Yükleme veya güncelleştirme NuGet hakkında daha fazla bilgi için bkz: [ https://nuget.org/ ] [ nuget].

> [!NOTE]
> NuGet'ın en son sürümünü yüklemek için önce Visual Studio Uzantı Yöneticisi'ni kullanarak yüklenen sürümü kaldırmanız gerekir. Bunu yapmak için Visual Studio'yu yönetici olarak çalıştırmalısınız. Aksi takdirde Kaldır düğmesini devre dışı bırakıldı.
>
>

### <a id="use_nuget"></a>Twilio kitaplıkları Visual Studio projenize eklemek için:
1. Çözümünüzü Visual Studio'da açın.
2. Sağ **başvuruları**.
3. Tıklayın **NuGet paketlerini Yönet...**
4. Tıklayın **çevrimiçi**.
5. Arama çevrimiçi kutusuna *twilio*.
6. Tıklayın **yükleme** Twilio paketteki.

## <a id="howto_make_call"></a>Nasıl Yapılır: Giden bir çağrı yapın
Aşağıdaki çağrıda giden hale getirmeyi açıklayan **CallResource** sınıfı. Bu kod, Twilio tarafından sağlanan bir site ayrıca Twilio biçimlendirme dili (TwiML) yanıt için kullanır. Kendi değerlerinizi yerleştirin **için** ve **gelen** telefon numaraları ve doğrulamanız olun **gelen** telefon numarası Twilio hesabınız için kodu çalıştırmadan önce.

```csharp
// Use your account SID and authentication token instead
// of the placeholders shown here.
const string accountSID = "your_twilio_account";
const string authToken = "your_twilio_authentication_token";

// Initialize the TwilioClient.
TwilioClient.Init(accountSID, authToken);

// Use the Twilio-provided site for the TwiML response.
var url = "https://twimlets.com/message";
url = $"{url}?Message%5B0%5D=Hello%20World";

// Set the call From, To, and URL values to use for the call.
// This sample uses the sandbox number provided by
// Twilio to make the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri(url));
    }
```

İçin geçirilen parametreler hakkında daha fazla bilgi için **CallResource.Create** yöntemi bkz [ https://www.twilio.com/docs/api/rest/making-calls ] [ twilio_rest_making_calls].

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz. Daha fazla bilgi için [nasıl yapılır: Kendi Web sitesinden TwiML yanıt vermek](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Nasıl Yapılır: Bir SMS iletisi gönderin
Aşağıdaki ekran görüntüsünde, bir SMS kullanarak ileti göndermek gösterilmektedir **MessageResource** sınıfı. **Gelen** numarası, SMS mesajları gönderebilir tarafından deneme hesapları için Twilio sağlanır. **İçin** numarası gerekir doğrulanabilir Twilio hesabınız için kodu çalıştırmadan önce.

```csharp
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
```

## <a id="howto_provide_twiml_responses"></a>Nasıl Yapılır: Kendi Web sitesinden TwiML yanıtları sağlayın
Ne zaman uygulamanızı başlatan Twilio API'si - Örneğin, bir çağrı aracılığıyla **CallResource.Create** yöntem - Twilio gönderir isteğiniz TwiML yanıt dönmesi beklenen bir URL. Örnekte [nasıl yapılır: Giden bir çağrı yapmak](#howto_make_call) Twilio tarafından sağlanan URL'yi kullanır [ https://twimlets.com/message ] [ twimlet_message_url] yanıtta döndürülecek.

> [!NOTE]
> TwiML web hizmetleri tarafından kullanılmak üzere tasarlandığından, tarayıcınızda TwiML görüntüleyebilirsiniz. Örneğin, [ https://twimlets.com/message ] [ twimlet_message_url] boş görmek için `<Response>` öğesi; başka bir örnek olarak, tıklayın [ https://twimlets.com/message?Message%5B0%5D=Hello%20World ](https://twimlets.com/message?Message%5B0%5D=Hello%20World) görmek için bir `<Response>` öğesini içeren bir &lt; Say&gt; öğesi.
>

Twilio tarafından sağlanan URL üzerinde işlemine güvenmek yerine, HTTP yanıtlarını döndürür kendi URL site oluşturabilirsiniz. HTTP yanıtlarını döndüren herhangi bir dilde site oluşturabilirsiniz. Bu konuda, bir ASP.NET genel işleyicisi URL'den barındırma varsayılır.

Aşağıdaki ASP.NET işleyicisi bildiren TwiML yanıt öğelerini **Hello World** çağrısında.

```csharp
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
```
    
Yukarıdaki örnekte görebileceğiniz gibi yalnızca bir XML belgesi TwiML yanıt. Twilio.TwiML kitaplık TwiML oluşturacaktır sınıfları içerir. Aşağıdaki örnekte, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır **VoiceResponse** sınıfı.

```csharp
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
```

TwiML hakkında daha fazla bilgi için bkz: [ https://www.twilio.com/docs/api/twiml ](https://www.twilio.com/docs/api/twiml).

TwiML yanıtlar sağlamak için bir şekilde ayarladıktan sonra bu URL'ye geçirebilirsiniz **CallResource.Create** yöntemi. Örneğin, bir Azure bulut hizmeti için dağıtılan MyTwiML adlı bir web Uygulamam var ve mytwiml.ashx ASP.NET işleyicinizi adıdır, URL geçirilebilir **CallResource.Create** aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
// This sample uses the sandbox number provided by Twilio to make the call.
// Place the call.
var call = CallResource.Create(
    to: new PhoneNumber("+NNNNNNNNNN"),
    from: new PhoneNumber("NNNNNNNNNN"),
    url: new Uri("http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx"));
    }
```

ASP.NET ile azure'da Twilio kullanma hakkında ek bilgi için bkz: [azure'da bir web rolünde Twilio kullanarak telefon görüşmesi yapma][howto_phonecall_dotnet].

[!INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]

[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md

[twimlet_message_url]: https://twimlets.com/message

[twilio_rest_making_calls]: https://www.twilio.com/docs/api/rest/making-calls

[vs_project]:https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-azure-project-create
[nuget]:https://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api]: https://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/console
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
