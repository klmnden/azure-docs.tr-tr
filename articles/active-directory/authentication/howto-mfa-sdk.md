---
title: Özel uygulamalar - Azure Active Directory'de Azure MFA Yazılım Geliştirme Seti
description: Bu makalede, özel uygulamalar için iki aşamalı doğrulamayı etkinleştirmek için Azure MFA SDK'sını indirip gösterilmektedir.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b6f5def70dcb2564e92c04e53b5d2ef5f0631fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414896"
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Yapı multi Factor Authentication'ı özel uygulamalarda (SDK)

> [!IMPORTANT]
> Azure Multi-Factor Authentication Yazılım Geliştirme Seti’nin (SDK) kullanımdan kaldırılacağı duyurulmuştur. Bu özellik, yeni müşteriler için artık desteklenmeyecektir. Mevcut müşteriler 14 Kasım 2018’e kadar SDK'yı kullanmaya devam edebilir. O tarihten sonra SDK çağrıları başarısız olacaktır. 

Azure multi-Factor Authentication Yazılım Geliştirme Seti (SDK) uygulamalarının Azure AD kiracınızda oturum açma veya işlem işlemleri doğrudan iki aşamalı doğrulama oluşturmanıza olanak sağlar.

Multi-Factor Authentication SDK'sı, C#, Visual Basic (.NET), Java, Perl, PHP ve Ruby için kullanılabilir. SDK, iki aşamalı doğrulama çevresinde ince bir sarmalayıcı sağlar. Açıklamalı kaynak kodu dosyaları, örneğin dosyaları ve ayrıntılı bir benioku dosyası gibi kodunuzu yazmanız gereken her şeyi içerir. Her bir SDK, bir sertifika ve özel anahtar, multi-Factor Authentication sağlayıcısı için benzersiz olan işlemler şifrelemek için de içerir. Bir sağlayıcının sahip olduğu sürece, gereksinim duyduğunuz kadar çok dil ve biçimlerinde SDK'sını indirebilirsiniz.

Multi-Factor Authentication SDK'sı API'leri yapısını, basit bir işlemdir. Tek bir işlevi çok faktörlü seçeneğin parametrelerinin (gibi doğrulama modu) ve kullanıcı verilerini (örneğin, çağırmak için telefon numarası veya doğrulamak için PIN numarası) ile bir API çağrısı yapın. API, web hizmeti istekleri için bulut tabanlı Azure multi-Factor Authentication hizmetinin işlev çağrısını çevir. Tüm çağrıları her SDK'da bulunan sertifikanın özel bir başvuru eklemeniz gerekir.

API'leri, Azure Active Directory'de kayıtlı kullanıcılara erişimi olmadığı için bir dosyadan veya veritabanından kullanıcı bilgilerini sağlamanız gerekir. Ayrıca, bu işlemlerin uygulamanıza uygulama oluşturmanızı sağlayacak şekilde API'leri kayıt veya kullanıcı yönetimi özelliklerini sağlamaz.

> [!IMPORTANT]
> SDK’yı indirmek için Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturmanız gerekir. Bu amaçla Azure multi-Factor Auth sağlayıcısı oluşturun ve zaten lisanslarınız varsa, sağlayıcıyla oluşturduğunuzdan emin olun **etkin kullanıcı başına** modeli. Ardından, Sağlayıcıyı Azure MFA, Azure AD Premium veya EMS lisansları içeren dizine bağlayın. Bu yapılandırma, olduğunuz lisans sayısından SDK'sını kullanarak daha fazla benzersiz kullanıcılarınız varsa, yalnızca faturalandırıldığını sağlar.


## <a name="download-the-sdk"></a>SDK'sını indirin
Azure multi-Factor Authentication SDK'ın yüklenmesini gerektirir bir [Azure multi-Factor Auth sağlayıcısı](concept-mfa-authprovider.md).  Azure MFA, Azure AD Premium veya Enterprise Mobility Suite lisansları ait olsa bile bu tam Azure aboneliği gerektirir. SDK'sı kullanımdan bu yana SDK indirme için genel yöntemler kullanımdan. SDK'yı indirmeniz gerekiyorsa, Microsoft ile destek talebinde bulunun. SDK'sı, SDK'sı zaten kullanmakta olduğunuz müşterilerine sağlanmıştır. Yeni müşteriler eklenen olmayacaktır.

## <a name="whats-in-the-sdk"></a>SDK'yı nedir
SDK'sı, aşağıdaki öğeleri içerir:

* **BENİOKU**. Yeni veya mevcut bir uygulama içinde çok faktörlü kimlik doğrulaması API'leri kullanmayı açıklar.
* **Kaynak dosyaları** çok faktörlü kimlik doğrulaması
* **İstemci sertifikası** multi-Factor Authentication hizmetiyle iletişim kurmak için kullandığınız
* **Özel anahtar** sertifikası
* **Çağrı sonuçları.** Arama sonuç kodları listesi. Bu dosyayı açmak için metin biçimlendirme, WordPad gibi bir uygulama kullanın. Arama sonuç kodları, test ve multi-Factor Authentication uygulamasını uygulamanızdaki sorunları gidermek için kullanın. Kimlik doğrulaması durum kodları değiller.
* **Örnekler.** Multi-Factor Authentication'ın temel çalışma uygulamaları için örnek kod.

> [!WARNING]
> İstemci sertifikası, özellikle sizin için oluşturulan benzersiz bir özel sertifikadır. Paylaşım yok veya bu dosyayı kaybetmek. Multi-Factor Authentication hizmetiyle, iletişimin güvenliğini sağlamak için anahtarınızı var.

## <a name="code-sample"></a>Kod örneği
Bu kod örneği API'leri Azure multi-Factor Authentication SDK'sı Standart mod sesli arama doğrulama uygulamanıza eklemek için nasıl kullanılacağını gösterir. Kullanıcı için # tuşuna basarak yanıt veren bir telefon görüşmesi standart modudur.

Bu örnek, C# sunucu tarafı mantık ile temel bir ASP.NET uygulamasında C# .NET 2.0 multi-Factor Authentication SDK'sı kullanır, ancak diğer dillerde benzer bir işlemdir. SDK'sı olmayan yürütülebilir dosyaları, kaynak dosyalar içerdiğinden derleme dosyalarını ve bunlara başvurmak veya bunları doğrudan uygulamanıza dahil.

> [!NOTE]
> Çok faktörlü kimlik doğrulaması yaparken, ek bir yöntem (telefon görüşmesi veya SMS mesajı) ikincil veya üçüncül doğrulama birincil kimlik doğrulama yönteminizi (kullanıcı adı ve parola) desteklemek için kullanın. Bu yöntemler, birincil kimlik doğrulama yöntemi olarak tasarlanmamıştır.

### <a name="code-sample-overview"></a>Kod örneğine genel bakış
Bu Tanıtım basit bir web uygulaması için örnek kod kullanıcının kimlik doğrulamasını doğrulamak için # anahtar yanıtıyla telefon görüşmesi kullanır. Bu telefon görüşmesi faktörü multi-Factor Authentication işleminde Standart modu olarak bilinir.

İstemci tarafı kod, çok faktörlü kimlik doğrulaması özgü öğeleri içermez. Ek kimlik doğrulama faktörleri birincil kimlik doğrulaması bağımsız olduğundan, var olan oturum açma arabirimi değiştirmeden ekleyebilirsiniz. Multi-Factor Authentication SDK'sı API'leri, kullanıcı deneyimini özelleştirmenize olanak sağlar, ancak hiçbir değişikliği gerekmeyebilir.

Sunucu tarafı kod, 2. adım Standart mod kimlik doğrulaması ekler. Standart mod doğrulama için gerekli parametreler ile PfAuthParams nesnesi oluşturur: kullanıcı adı, telefon numarası ve modu ve her çağrının içinde gerekli olan istemci sertifikası (CertFilePath) yolu. Tüm parametrelerin PfAuthParams gösterimi için SDK'sı örnek dosyasına bakın.

Ardından, kod PfAuthParams nesne pf_authenticate() işleve geçirir. Başarılı veya başarısız kimlik doğrulamasının dönüş değeri gösterir. Parametreler, callStatus ve errorID ek arama sonucu bilgiler içerir. Arama sonuç kodları, SDK'sı çağrı sonuçları dosyasında belirtilmiştir.

Bu en az bir uygulama içinde birkaç satır kod yazılabilir. Ancak, üretim kodunda daha karmaşık hata işleme, ek veritabanı kod ve geliştirilmiş kullanıcı deneyimi verilebilir.

### <a name="web-client-code"></a>Web istemci kodu
Bir tanıtım sayfası için web istemci kodu verilmiştir.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="https://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Sunucu Tarafında Çalışan Kod
Aşağıdaki sunucu tarafı kodu, multi-Factor Authentication yapılandırılır ve 2. adımda çalıştırın. Standart mod (MODE_STANDARD) için # tuşuna basarak kullanıcı yanıt telefon bir çağrıdır.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
