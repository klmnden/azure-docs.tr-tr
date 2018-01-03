---
title: SendGrid e-posta hizmetine (.NET) kullanma | Microsoft Docs
description: "Bilgi nasıl Azure üzerinde SendGrid e-posta hizmeti ile e-posta gönderin. C# dilinde yazılan kod örnekleri ve .NET API kullanır."
services: 
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: 
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: a5f07d02bfe4032d77a17e5972b88f6530125f28
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2017
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Azure ile SendGrid kullanarak e-posta gönderme
## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevleri gerçekleştirmek gösterilmiştir. Örnekler C yazılır\# ve .NET standart 1.3 destekler. Kapsamdaki senaryolar, e-posta oluşturma, e-posta gönderen, ekleri ekleme ve çeşitli posta etkinleştirme ve izleme ayarları içerir. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz: [sonraki adımlar] [ Next steps] bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olan bir [bulut tabanlı e-posta hizmeti] güvenilir sağlayan [işleme uygun e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz özel tümleştirme kolaylaştırmak esnek API'leri yanı sıra. Ortak SendGrid kullanım örnekleri şunlardır:

* Otomatik olarak giriş veya satın alma onayı müşterilere gönderme.
* Dağıtım yönetme aylık ilanları ve promosyonlar müşteriler göndermek için listeler.
* Engellenen e-posta ve müşteri katılım gibi için gerçek zamanlı ölçümleri toplama.
* İletme müşteri sorgular.
* Gelen e-postaları işleniyor.

Daha fazla bilgi için ziyaret [https://sendgrid.com](https://sendgrid.com) veya SendGrid'ın [C# Kitaplığı] [ sendgrid-csharp] GitHub depo.

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>SendGrid .NET sınıf kitaplığı başvurusu
[SendGrid NuGet paketi](https://www.nuget.org/packages/Sendgrid) SendGrid API'sini almanın ve uygulamanızı tüm bağımlılıkları ile yapılandırmak için çok kolay yoludur. NuGet Microsoft Visual Studio 2015 ile birlikte bulunan bir Visual Studio uzantısı gerekir ve bu düzeyin yüklemek ve kitaplıklar ve Araçlar güncelleştirmek kolaylaştırır.

> [!NOTE]
> Visual Studio Visual Studio 2015'ten önceki bir sürümünü çalıştırıyorsanız, NuGet yüklemek için ziyaret edin [http://www.nuget.org](http://www.nuget.org), tıklatıp **Nuget'i Yükle** düğmesi.
>
>

Uygulamanızda SendGrid NuGet paketini yüklemek için aşağıdakileri yapın:

1. Tıklayın **yeni proje** seçip bir **şablon**.

   ![Yeni bir proje oluşturma][create-new-project]
2. İçinde **Çözüm Gezgini**, sağ **başvuruları**, ardından **NuGet paketlerini Yönet**.

   ![SendGrid NuGet paketi][SendGrid-NuGet-package]
3. Arama **SendGrid** seçip **SendGrid** sonuçlar listesinde öğesi.
4. Nesne modeli ile çalışmak için sürüm açılır Nuget paketi en son kararlı sürümü seçin ve bu makalede API'leri gösterilmektedir.

   ![SendGrid paketi][sendgrid-package]
5. Tıklatın **yükleme** yüklemeyi tamamlamak ve sonra bu iletişim kutusunu kapatın.

SendGrid'ın .NET sınıf kitaplığı çağrılır **SendGrid**. Şu ad alanlarından içerir:

* **SendGrid** SendGrid'ın API'si ile iletişim kurmak için.
* **SendGrid.Helpers.Mail** kolayca e-postalar gönderme belirtin SendGridMessage nesneleri oluşturmak yardımcı yöntemler için.

Aşağıdaki kod ad alanı bildirimleri SendGrid e-posta hizmeti programlı olarak erişmek istediğiniz tüm C# dosyasının üstüne ekleyin.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Nasıl yapılır: bir e-posta oluşturma
Kullanım **SendGridMessage** bir e-posta iletisi oluşturmak için nesne. İleti nesnesi oluşturulduktan sonra özellikleri ve yöntemleri, e-posta gönderen, e-posta alıcısının ve konu ve gövde e-posta dahil olmak üzere ayarlayabilirsiniz.

Aşağıdaki örnek, bir tam olarak doldurulan bir e-posta nesnesinin nasıl oluşturulacağını gösterir:

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

Tüm özellikleri ve yöntemleri tarafından desteklenen hakkında daha fazla bilgi için **SendGrid** yazın, bkz: [sendgrid csharp] [ sendgrid-csharp] github'da.

## <a name="how-to-send-an-email"></a>Nasıl yapılır: bir e-posta Gönder
Bir e-posta iletisi oluşturduktan sonra SendGrid'ın API kullanarak gönderebilirsiniz. Alternatif olarak, kullanabilir [. NET'in yerleşik Kitaplığı'nda][NET-library].

E-posta gönderilmesi SendGrid API anahtarınızı sağlamanız gerekir. API anahtarları yapılandırma hakkında ayrıntılar gerekiyorsa, lütfen SendGrid'ın API anahtarları adresini ziyaret edin [belgelerine][documentation].

Uygulama Ayarları'nı ve uygulama ayarları altından anahtar/değer çiftleri ekleyerek, Azure Portal bu kimlik bilgileri depolayabilir.

 ![Azure uygulama ayarları][azure_app_settings]

 Daha sonra bunları aşağıdaki gibi erişebilirsiniz:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

Aşağıdaki örnekler bir konsol uygulaması ile SendGrid Web API kullanarak e-posta iletisi göndermek nasıl gösterir.

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

Aşağıdaki örnekte, ASP .NET Core API kullanmaları için birden çok kişinin tek bir e-posta göndermek için kullanılabilir `MailHelper` sınıfının `SendGrid.Helpers.Mail` ad alanı. ASP .NET Core 1.0 Bu örnek için kullanıyoruz. 

Bu örnekte, API anahtarını içinde zamandır saklanan `appsettings.json` Azure portalından, Yukarıdaki örneklerde gösterildiği gibi geçersiz kılınabilir dosya.

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

İlk olarak eklemek ihtiyacımız kodda aşağıda `Startup.cs` .NET Core API projesinin dosya. Biz erişebilmesi için bu gereklidir `SENDGRID_API_KEY` gelen `appsettings.json` API denetleyicisi bağımlılık ekleme kullanılarak dosya. `IConfiguration` Arabirimi eklenen denetleyicisi Oluşturucusu ekledikten sonra `ConfigureServices` yöntemi aşağıdaki. İçeriği `Startup.cs` dosya gerekli kodu ekledikten sonra aşağıdaki gibi görünür:

        public IConfigurationRoot Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            // Add mvc here
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }

İnjecting sonra denetleyicisinde `IConfiguration` arabirimi, kullanabileceğiniz `CreateSingleEmailToMultipleRecipients` yöntemi `MailHelper` birden çok alıcıya tek bir e-posta göndermek için sınıf. Adlı bir ek Boole parametresi yöntemi kabul `showAllRecipients`. E-posta alıcılarını görmeye olup olmadığını denetlemek için bu parametreyi kullanılabilir birbirlerinin e-posta üstbilgisinin Kime bölümdeki e-posta adresi. Denetleyici için örnek kod aşağıdaki gibi olmalıdır 

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
    
## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: bir eki ekleyin
Ekleri eklenebilir bir iletiye çağırarak **AddAttachment** yöntemi ve dosya adını ve Base64 ile kodlanmış içeriği en düşük düzeyde belirten eklemek istediğiniz. Eklemek istediğiniz her dosya için bir kez bu yöntemi çağırmadan veya kullanarak birden çok eki içerebilir **AddAttachments** yöntemi. Aşağıdaki örnek, bir iletinin eki ekleme gösterir:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: altbilgi, izleme ve analiz etkinleştirmek için posta ayarları kullanın
SendGrid posta ve izleme ayarlarını kullanarak ek e-posta işlevselliği sağlar. Bu ayarlar, izleme'ye tıklayın, Google analytics, izleme abonelik vb. gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilir. Uygulamaların tam listesi için bkz: [ayarları belgelerine][settings-documentation].

Uygulamalar uygulanabilir **SendGrid** e-posta iletileri bir parçası olarak uygulanan yöntemleri kullanarak **SendGridMessage** sınıfı. Aşağıdaki örnekler altbilgi göstermek ve filtreleri İzleme'yi tıklatın:

Aşağıdaki örnekler altbilgi göstermek ve filtreleri İzleme'yi tıklatın:

### <a name="footer-settings"></a>Altbilgi ayarları
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>İzleme'yi tıklatın
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: ek SendGrid Hizmetleri kullanın
SendGrid çeşitli API'ler ve Azure uygulamanız içinde ek işlevsellik yararlanmak için kullanabileceğiniz Web kancalarını sunar. Daha fazla ayrıntı için bkz: [SendGrid API Başvurusu][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki adımlar
SendGrid e-posta hizmeti temel bilgileri öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* SendGrid C\# kitaplığı deposu: [sendgrid csharp][sendgrid-csharp]
* SendGrid API belgelerine: <https://sendgrid.com/docs>

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
[işleme uygun e-posta teslimi]: https://sendgrid.com/use-cases/transactional-email

