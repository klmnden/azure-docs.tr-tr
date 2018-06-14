---
title: Twilio (.NET) bir telefon araması yapma | Microsoft Docs
description: Bir telefon araması yapın ve Azure üzerinde Twilio API hizmetiyle SMS mesajı göndermek öğrenin. .NET ile yazılan kod örnekleri.
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
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: cd9792881182fbe90d9c210130ae8a34b12da363
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
ms.locfileid: "26366013"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Azure üzerinde bir web rolü Twilio kullanarak bir telefon araması yapma
Bu kılavuz Twilio Azure üzerinde barındırılan bir web sayfasından bir arama yapmak için nasıl kullanılacağını gösterir. Sonuçta elde edilen uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi verilen sayıda ve ileti ile arama yapmak için kullanıcıya sorar.

![Twilio ve ASP.NET kullanarak azure çağrı formu][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Önkoşullar
Bu konudaki kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio hesabı ve kimlik doğrulaması almak belirtecine [Twilio konsol][twilio_console]. Twilio ile çalışmaya başlamak için oturum açın [https://www.twilio.com/try-twilio][try_twilio]. Konumundaki fiyatlandırma değerlendirebilir [http://www.twilio.com/pricing][twilio_pricing]. Twilio tarafından sağlanan API'si hakkında daha fazla bilgi için bkz: [http://www.twilio.com/voice/api][twilio_api].
2. Ekleme *Twilio .NET kitaplığı* web rolünüz. Bkz: **Twilio kitaplıkları web rolü projenize eklemek için**, bu konunun devamındaki.

Temel bir oluşturma konusunda bilgi sahibi olmanız gerekir [Azure Web rolü][azure_webroles_get_started].

## <a name="howtocreateform"></a>Nasıl yapılır: arama yapmak için web formu oluşturma
<a id="use_nuget"></a>Twilio kitaplıkları web rolü projenize eklemek için:

1. Çözümünüzü Visual Studio'da açın.
2. Sağ **başvurular**.
3. Tıklatın **NuGet paketlerini Yönet**.
4. Tıklatın **çevrimiçi**.
5. Arama çevrimiçi kutuya yazın *twilio*.
6. Tıklatın **yükleme** Twilio paketinizdeki.

Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için web formu oluşturulacağını gösterir. Bu örnekte, bir ASP.NET Web rolü adlı **TwilioCloud** oluşturulur.

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

## <a id="howtocreatecode"></a>Nasıl yapılır: arama yapmak için kod oluşturma
Kullanıcı formu tamamlandığında olarak adlandırılır, aşağıdaki kod, arama iletisi oluşturur ve çağrı oluşturur. Bu örnekte, formdaki düğmenin onclick olay işleyicisini kod çalıştırılır. (Kullanım Twilio hesabı ve kimlik doğrulama belirteci atanan yer tutucu değerlerini yerine `accountSID` ve `authToken` aşağıdaki kodda.)

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
            // Call porcessing happens here.

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
                var url = $"http://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
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

Çağrı yapılır ve Twilio uç noktası, API sürümü ve arama durumu görüntülenir. Aşağıdaki ekran görüntüsünde bir örnek çalışma çıktısını gösterir.

![Twilio ve ASP.NET kullanılarak azure çağrı yanıtı][twilio_dotnet_basic_form_output]

TwiML hakkında daha fazla bilgi bulunabilir [http://www.twilio.com/docs/api/twiml][twiml]. Hakkında daha fazla bilgi &lt;Say&gt; ve diğer Twilio fiiller bulunabilir [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Sonraki adımlar
Bu kod, bir ASP.NET web rolünde Azure ile ilgili Twilio kullanarak temel işlevselliğini göstermek için sağlanmıştır. Azure'a üretimde dağıtmadan önce daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örneğin:

* Bir web formu kullanmak yerine, Azure Blob storage veya Azure SQL veritabanı örneği telefon numaralarını depolamak ve metin çağırmak için kullanabilirsiniz. Azure BLOB'ları kullanma hakkında daha fazla bilgi için bkz: [.NET ile Azure Blob Depolama hizmetini kullanmayı][howto_blob_storage_dotnet]. SQL veritabanı kullanma hakkında daha fazla bilgi için bkz: [.NET uygulamalarında Azure SQL Database kullanmayı][howto_sql_azure_dotnet].
* Kullanabileceğinizi `RoleEnvironment.getConfigurationSettings` hesap Kimliğini ve kimlik doğrulama belirteci form değerleri sabit kodlama yerine dağıtımınızın yapılandırma ayarları Twilio alınamadı. Hakkında bilgi için `RoleEnvironment` sınıfı için bkz: [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Twilio güvenlik yönergeleri okuyun [https://www.twilio.com/docs/security][twilio_docs_security].
* Twilio hakkında daha fazla bilgi [https://www.twilio.com/docs][twilio_docs].

## <a name="seealso"></a>Ayrıca bkz.
* [Azure'dan ses ve SMS özelliklerine yönelik Twilio kullanma](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-get-started
