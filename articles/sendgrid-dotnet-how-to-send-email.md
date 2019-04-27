---
title: SendGrid e-posta hizmeti (.NET) kullanmaya nasıl | Microsoft Docs
description: Bilgi e-posta SendGrid e-posta hizmeti ile Azure üzerinde nasıl gönderin. C# dilinde yazılan kod örnekleri ve .NET API kullanma.
services: ''
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: ''
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: 91d28802b4af23da5b8060fa7c8f9a7e843a7dab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444915"
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Azure ile SendGrid kullanarak e-posta gönderme
## <a name="overview"></a>Genel Bakış
Bu kılavuzda, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Örnekler C dilinde yazılan\# ve .NET Standard 1.3 destekler. E-posta oluşturma, e-posta gönderen, ekleri ekleme ve çeşitli posta etkinleştirme ve izleme ayarları senaryoları ele alınmaktadır. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz. [sonraki adımlar] [ Next steps] bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olduğu bir [bulut tabanlı e-posta hizmeti] sağlayan güvenilir [İşlem tabanlı e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz, özel tümleştirmeyi kolaylaştıran esnek API'ler ile birlikte. Ortak SendGrid kullanım örnekleri şunlardır:

* Giriş veya satın alma onayı otomatik olarak müşterilere gönderiliyor.
* Dağıtım yönetme için müşterileri aylık ilanlara ve promosyonlar göndermek için listeler.
* Engellenen e-posta ve müşteri etkileşimi gibi şeyler için gerçek zamanlı ölçümler toplama.
* İletme müşteri sorgular.
* Gelen e-postaları işleniyor.

Daha fazla bilgi için ziyaret [ https://sendgrid.com ](https://sendgrid.com) veya SendGrid [C# Kitaplığı] [ sendgrid-csharp] GitHub deposu.

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>SendGrid .NET sınıf kitaplığı başvurusu
[SendGrid NuGet paketini](https://www.nuget.org/packages/Sendgrid) SendGrid API'sini almanın ve uygulamanızı tüm bağımlılıklarıyla yapılandırmanın en kolay yolu. NuGet ile Microsoft Visual Studio 2015 dahil Visual Studio uzantısıdır ve bunun üstünde, yükleme ve kitaplıkları ve araçları güncelleştirme kolaylaştırır.

> [!NOTE]
> Visual Studio'nun Visual Studio 2015'den önceki bir sürümünü çalıştırıyorsanız NuGet yüklemek için ziyaret [ https://www.nuget.org ](https://www.nuget.org), tıklatıp **Nuget'i Yükle** düğmesi.
>
>

Uygulamanızda SendGrid NuGet paketini yüklemek için aşağıdakileri yapın:

1. Tıklayarak **yeni proje** seçip bir **şablon**.

   ![Yeni bir proje oluşturma][create-new-project]
2. İçinde **Çözüm Gezgini**, sağ **başvuruları**, ardından **NuGet paketlerini Yönet**.

   ![SendGrid NuGet paketi][SendGrid-NuGet-package]
3. Arama **SendGrid** seçip **SendGrid** sonuç listesinde öğesi.
4. Nuget paketinin en son kararlı sürüm nesne modeli ile çalışmak için sürüm açılır listeden seçin ve API'leri bu makalede gösterilmiştir.

   ![SendGrid paket][sendgrid-package]
5. Tıklayın **yükleme** yüklemeyi tamamlayın ve ardından bu iletişim kutusunu kapatın.

SendGrid .NET sınıf kitaplığı çağrılır **SendGrid**. Aşağıdaki ad alanlarını içerir:

* **SendGrid** SendGrid API ile iletişim kurmak için.
* **SendGrid.Helpers.Mail** kolayca e-postaları göndermek nasıl belirtin SendGridMessage nesneleri oluşturmak yardımcı yöntemler için.

Aşağıdaki kod ad alanı bildirimi, SendGrid e-posta hizmetini programlı olarak erişmek istediğiniz tüm C# dosyasının en üstüne ekleyin.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Nasıl yapılır: E-posta oluşturma
Kullanım **SendGridMessage** bir e-posta iletisi oluşturmak için nesne. İleti nesnesi oluşturulduktan sonra özellikleri ve yöntemleri, e-posta gönderenin e-posta alıcısının ve konu ve e-postanın gövdesi de dahil olmak üzere ayarlayabilirsiniz.

Aşağıdaki örnek, tam olarak girilmiş bir e-posta nesnesinin nasıl oluşturulacağını gösterir:

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing the SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

Tüm özellikleri ve yöntemleri tarafından desteklenen daha fazla bilgi için **SendGrid** yazın, bkz: [sendgrid-csharp] [ sendgrid-csharp] GitHub üzerinde.

## <a name="how-to-send-an-email"></a>Nasıl yapılır: E-posta Gönder
Bir e-posta iletisi oluşturduktan sonra SendGrid API kullanarak gönderebilirsiniz. Alternatif olarak, kullanabilir [. NET Kitaplığı'nda oluşturulan][NET-library].

E-posta gönderme, SendGrid API anahtarınızı sağlamanız gerekir. SendGrid API anahtarlarını API anahtarları yapılandırma hakkında daha fazla ayrıntı gerekiyorsa, lütfen [belgeleri][documentation].

Uygulama Ayarları'nı ve uygulama ayarları altında anahtar/değer çiftleri ekleyerek bu kimlik bilgileri, Azure portalı üzerinden depolayabilir.

 ![Azure uygulama ayarları][azure_app_settings]

 Ardından, bunları şu şekilde erişebilir:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

Aşağıdaki örnekler, SendGrid Web API'sini kullanarak bir konsol uygulaması ile bir e-posta iletisi göndermek nasıl gösterir.

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from the SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }
    
## <a name="how-to-send-email-from-asp-net-core-api-using-mailhelper-class"></a>Nasıl yapılır: ASP .NET Core MailHelper sınıfını kullanarak API e-posta Gönder

Aşağıdaki örnekte, ASP .NET Core API kullanarak birden fazla kişi için tek bir e-posta göndermek için kullanılabilir `MailHelper` sınıfının `SendGrid.Helpers.Mail` ad alanı. Bu örnekte ASP .NET Core 1.0 kullanıyoruz. 

Bu örnekte, API anahtarı içinde daha fazladır saklanan `appsettings.json` Yukarıdaki örneklerde gösterildiği gibi Azure portalından kılınabilir dosyası.

İçeriğini `appsettings.json` dosya benzer şekilde görünmelidir:

    {
       "Logging": {
       "IncludeScopes": false,
       "LogLevel": {
       "Default": "Debug",
       "System": "Information",
       "Microsoft": "Information"
         }
       },
     "SENDGRID_API_KEY": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }

İlk olarak eklemek ihtiyacımız aşağıda kodda `Startup.cs` .NET Core API proje dosyası. Biz erişebilmesi için bu gereklidir `SENDGRID_API_KEY` gelen `appsettings.json` API denetleyicisi bağımlılık ekleme kullanılarak dosya. `IConfiguration` Arabirimi eklenen denetleyicinin Oluşturucusu sırasında ekledikten sonra `ConfigureServices` aşağıdaki yöntemi. İçeriği `Startup.cs` gerekli kodu ekledikten sonra aşağıdaki dosya şöyle:

        public IConfigurationRoot Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            // Add mvc here
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }

Ekleme sonra denetleyicisinde `IConfiguration` kullanabiliriz arabirimi `CreateSingleEmailToMultipleRecipients` yöntemi `MailHelper` birden çok alıcıya tek bir e-posta göndermek için sınıf. Adlı bir ilave bir Boole parametresi yöntemi kabul `showAllRecipients`. E-posta alıcılarını görmeye olup olmadığını denetlemek için bu parametreyi kullanılabilir birbirlerinin e-posta üst bilgisi için bölümünde e-posta adresi. Denetleyici için örnek kod aşağıdaki gibi olmalıdır 

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using SendGrid;
    using SendGrid.Helpers.Mail;
    using Microsoft.Extensions.Configuration;

    namespace SendgridMailApp.Controllers
    {
        [Route("api/[controller]")]
        public class NotificationController : Controller
        {
           private readonly IConfiguration _configuration;

           public NotificationController(IConfiguration configuration)
           {
             _configuration = configuration;
           }      
        
           [Route("SendNotification")]
           public async Task PostMessage()
           {
              var apiKey = _configuration.GetSection("SENDGRID_API_KEY").Value;
              var client = new SendGridClient(apiKey);
              var from = new EmailAddress("test1@example.com", "Example User 1");
              List<EmailAddress> tos = new List<EmailAddress>
              {
                  new EmailAddress("test2@example.com", "Example User 2"),
                  new EmailAddress("test3@example.com", "Example User 3"),
                  new EmailAddress("test4@example.com","Example User 4")
              };
            
              var subject = "Hello world email from Sendgrid ";
              var htmlContent = "<strong>Hello world with HTML content</strong>";
              var displayRecipients = false; // set this to true if you want recipients to see each others mail id 
              var msg = MailHelper.CreateSingleEmailToMultipleRecipients(from, tos, subject, "", htmlContent, false);
              var response = await client.SendEmailAsync(msg);
          }
       }
    }
    
## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: Bir ek ekleyin
Ekleri eklenebilir bir ileti çağırarak **AddAttachment** yöntemi ve dosya adını ve Base64 ile kodlanmış içeriği en az belirleme eklemek istiyor. Birden çok eki eklemek istediğiniz her dosya için bir kez bu yöntemi çağırarak veya kullanarak içerebilir **AddAttachments** yöntemi. Aşağıdaki örnek, bir iletiye ek ekleme gösterir:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: Posta ayarları altbilgilerinde, izleme ve analiz etkinleştirmek için kullanın
SendGrid e-posta ve izleme ayarlarını kullanarak ek e-posta işlevselliğini sağlar. Bu ayarlar, izleme'ye tıklayın, Google analytics, izleme aboneliği ve benzeri gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilir. Uygulamaların tam listesi için bkz. [ayarları belgeleri][settings-documentation].

Uygulamalar uygulanabilir **SendGrid** e-posta iletileri bir parçası olarak uygulanmış olan yöntemlerle kullanarak **SendGridMessage** sınıfı. Aşağıdaki örnekler, alt bilgi göstermek ve filtreleri İzleme'yi tıklatın:

Aşağıdaki örnekler, alt bilgi göstermek ve filtreleri İzleme'yi tıklatın:

### <a name="footer-settings"></a>Alt bilgi ayarları
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>İzleme'ye tıklayın.
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: Ek SendGrid hizmetlerini kullanma
SendGrid çeşitli API'ler ve ek işlevselliğinden Azure uygulamanızda kullandığınız Web kancaları da sunar. Daha fazla ayrıntı için [SendGrid API Başvurusu][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki adımlar
SendGrid e-posta hizmeti ile ilgili temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* SendGrid C\# kitaplığı depo: [sendgrid-csharp][sendgrid-csharp]
* SendGrid API belgeleri: <https://sendgrid.com/docs>

[Next steps]: #next-steps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/solutions
[İşlem tabanlı e-posta teslimi]: https://sendgrid.com/use-cases/transactional-email

