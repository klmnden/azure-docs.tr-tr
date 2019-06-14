---
title: (.NET) Twilio telefon görüşmesi yapma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. .NET ile yazılan kod örneklerini içerir.
services: ''
documentationcenter: .net
author: devinrader
manager: timlt
editor: ''
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: jeconnoc
ms.openlocfilehash: c41057203da949e371f62332e938feb92e84534f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60422823"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Azure'da bir web rolünde Twilio kullanarak telefon görüşmesi yapma
Bu kılavuz, Azure'da barındırılan bir web sayfasından çağrı yapmak için Twilio kullanma gösterilmektedir. Aşağıdaki ekran görüntüsünde gösterildiği gibi sonuç uygulamayı verilen sayının ve ileti ile arama yapmak için kullanıcıya sorar.

![Twilio ve ASP.NET kullanarak azure çağrı formu][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Önkoşullar
Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesap ve kimlik doğrulama Al belirtecine [Twilio konsol][twilio_console]. Twilio ile çalışmaya başlamak için oturum açın [ https://www.twilio.com/try-twilio ] [ try_twilio]. Bölümünde fiyatlar değerlendirebilirsiniz [ https://www.twilio.com/pricing ] [ twilio_pricing]. Twilio tarafından sağlanan API hakkında daha fazla bilgi için bkz: [ https://www.twilio.com/voice/api ] [ twilio_api].
2. Ekleme *Twilio .NET kitaplığı* web rolünüz. Bkz: **Twilio kitaplıkları web rolü projenize eklemek için**, bu konunun devamındaki.

Temel oluşturma konusunda bilgi sahibi olmanız gerekir [Azure Web rolünde][azure_webroles_get_started].

## <a name="howtocreateform"></a>Nasıl Yapılır: Arama yapmak için web formu oluşturma
<a id="use_nuget"></a>Twilio kitaplıkları web rolü projenize eklemek için:

1. Çözümünüzü Visual Studio'da açın.
2. Sağ **başvuruları**.
3. Tıklayın **NuGet paketlerini Yönet**.
4. Tıklayın **çevrimiçi**.
5. Arama çevrimiçi kutusuna *twilio*.
6. Tıklayın **yükleme** Twilio paketteki.

Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için bir web formu oluşturma işlemini gösterir. Bu örnekte, bir ASP.NET Web rolü adlı **TwilioCloud** oluşturulur.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a id="howtocreatecode"></a>Nasıl Yapılır: Çağrı yapmak için kod oluşturun
Kullanıcı formu tamamladığında çağrılır, aşağıdaki kod, çağrı ileti oluşturur ve çağrı oluşturur. Bu örnekte, form üzerinde düğmesinin tıklatıldığında olay işleyicisi kod çalıştırılır. (Kullanım Twilio hesabınız ve kimlik doğrulama belirteci atanmış yer tutucu değerlerini yerine `accountSID` ve `authToken` aşağıdaki kodda.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call processing happens here.

            // Use your account SID and authentication token instead of
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"https://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

Çağrı yapılır ve Twilio uç noktası, API sürümü ve arama durumu görüntülenir. Örnek çalıştırmanın çıktısı aşağıdaki ekran gösterilir.

![Twilio ve ASP.NET kullanarak azure çağrı yanıtı][twilio_dotnet_basic_form_output]

TwiML hakkında daha fazla bilgi şu adreste bulunabilir: [ https://www.twilio.com/docs/api/twiml ] [ twiml]. Hakkında daha fazla bilgi &lt;Say&gt; ve diğer Twilio fiilleri şu yolda bulunabilir: [ https://www.twilio.com/docs/api/twiml/say ] [ twilio_say].

## <a id="nextsteps"></a>Sonraki adımlar
Bu kod, Azure üzerinde ASP.NET web rolünde Twilio kullanarak temel işlevselliğini göstermek için sağlanmıştır. Üretimde Azure'a dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Web formu kullanmak yerine, Azure Blob Depolama veya Azure SQL veritabanı örneği telefon numaraları depolamak ve metin çağırmak için kullanabilirsiniz. Azure'da BLOB'ları kullanma hakkında daha fazla bilgi için bkz: [. NET'te Azure Blob Depolama hizmetinin kullanmayı][howto_blob_storage_dotnet]. SQL veritabanı'nı kullanma hakkında daha fazla bilgi için bkz: [.NET uygulamalarında Azure SQL veritabanı kullanmayı][howto_sql_azure_dotnet].
* Kullanabileceğinizi `RoleEnvironment.getConfigurationSettings` hesap kimliği ve kimlik doğrulaması belirteci, formdaki değerler sabit kodlama yerine dağıtımınızın yapılandırma ayarlarından Twilio alınacak. Hakkında bilgi için `RoleEnvironment` sınıfı [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Twilio güvenlik yönergeleri okuyun [ https://www.twilio.com/docs/security ] [ twilio_docs_security].
* Twilio hakkında daha fazla bilgi [ https://www.twilio.com/docs ] [ twilio_docs].

## <a name="seealso"></a>Ayrıca bkz.
* [Azure'dan ses ve SMS özellikleri için Twilio kullanma](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: https://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-get-started
