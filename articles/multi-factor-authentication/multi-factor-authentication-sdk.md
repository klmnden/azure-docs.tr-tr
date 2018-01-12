---
title: "MFA Yazılım Geliştirme Seti özel uygulamalar için | Microsoft Docs"
description: "Bu makalede karşıdan yükleme ve özel uygulamalarınız için iki aşamalı doğrulamayı etkinleştirmek için Azure MFA SDK'sını kullanma gösterilmektedir."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: joflore
ms.openlocfilehash: 7ae89241c67655fbcaa747c4cac224b898947f39
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Yapı multi-Factor Authentication özel uygulamalar (SDK)

> [!IMPORTANT]
> Azure Multi-Factor Authentication Yazılım Geliştirme Seti’nin (SDK) kullanımdan kaldırılacağı duyurulmuştur. Bu özellik artık yeni müşteriler için desteklenmez. Mevcut müşteriler 14 Kasım 2018’e kadar SDK'yı kullanmaya devam edebilir. O tarihten sonra SDK çağrıları başarısız olacaktır. 

Azure çok faktörlü kimlik doğrulaması Yazılım Geliştirme Seti (SDK) iki aşamalı doğrulama özelliklerini doğrudan oturum açma veya işlem işlemler Azure AD kiracınıza uygulamaların oluşturmanıza olanak sağlar.

Multi-Factor Authentication SDK'sı, C#, Visual Basic (.NET), Java, Perl, PHP ve Ruby için kullanılabilir. SDK, iki aşamalı doğrulamayı çevresinde ince bir sarmalayıcı sağlar. Açıklamalı kaynak kodu dosyaları, örneğin dosyaları ve ayrıntılı bir benioku dosyası da dahil olmak üzere, kodunuzu yazma için gereken her şeyi içerir. Her SDK ayrıca bir sertifika ve çok faktörlü kimlik doğrulama sağlayıcınızı benzersiz işlemleri şifreleme için özel anahtarı içerir. Bir sağlayıcı sahip olduğu sürece, gerektiği kadar çok dil ve biçimleri SDK'da indirebilirsiniz.

Multi-Factor Authentication SDK'sı API'lerinde yapısını basit bir işlemdir. Tek bir işlevi çok faktörlü seçeneği parametreleri (gibi doğrulama modu) ve kullanıcı verilerini (örneğin, çağırmak için telefon numarası veya doğrulamak için PIN numarası) ile bir API çağrısı yapın. API web hizmetleri istekleri içine işlev çağrısı için bulut tabanlı Azure çok faktörlü kimlik doğrulama hizmeti çevir. Tüm çağrılar her SDK'da bulunan özel sertifika için bir başvuru eklemeniz gerekir.

API'ler Azure Active Directory'de kayıtlı kullanıcılara erişimi olmadığı için bir dosya veya veritabanı kullanıcı bilgilerini sağlamanız gerekir. Ayrıca, bu işlemlerin uygulamanıza oluşturmak gereken şekilde API'leri kayıt ya da kullanıcı yönetimi özelliklerini sağlamaz.

> [!IMPORTANT]
> SDK’yı indirmek için Azure MFA, AAD Premium veya EMS lisanslarınız olsa bile bir Azure Multi-Factor Auth Sağlayıcısı oluşturmanız gerekir. Bu amaç için Azure multi-Factor Auth sağlayıcısı oluşturmak ve lisansları zaten varsa, sağlayıcı ile oluşturduğunuzdan emin olun **etkin kullanıcı başına** modeli. Ardından, Sağlayıcıyı Azure MFA, Azure AD Premium veya EMS lisansları içeren dizine bağlayın. Bu yapılandırma, sahip olduğunuz lisans sayısından SDK'sını kullanarak daha fazla benzersiz kullanıcı varsa, yalnızca faturalandırılır olmasını sağlar.


## <a name="download-the-sdk"></a>SDK'sını indirin
Azure çok faktörlü SDK'sını indirme gerektiren bir [Azure multi-Factor Auth sağlayıcısı](multi-factor-authentication-get-started-auth-provider.md).  Azure MFA, Azure AD Premium veya Enterprise Mobility Suite lisansları ait olsa bile bu tam Azure aboneliği gerektirir. SDK kullanım beri SDK indirilmesi genel yöntemler hizmetten. SDK'sını indirin gerekiyorsa, Microsoft ile bir destek servis talebi açmanız gerekir. SDK, SDK'i zaten kullanmakta olduğunuz müşterilerine sağlanmıştır. Yeni müşteriler edildi olmaz.

## <a name="whats-in-the-sdk"></a>SDK'ın nedir
SDK'yı aşağıdaki öğeleri içerir:

* **BENİOKU**. Yeni veya var olan bir uygulama içinde çok faktörlü kimlik doğrulaması API'leri kullanımı açıklanmaktadır.
* **Kaynak dosyaları** çok faktörlü kimlik doğrulaması
* **İstemci sertifikası** multi-Factor Authentication hizmetiyle iletişim kurmak için kullandıkları
* **Özel anahtarı** sertifika için
* **Çağrı sonuçları.** Arama sonuç kodları listesi. Bu dosyayı açmak için WordPad gibi biçimlendirme metin içeren bir uygulama kullanın. Test ve uygulamanızı multi-Factor Authentication uygulamasında sorun gidermek için arama sonuç kodları kullanın. Kimlik doğrulama durum kodları değiller.
* **Örnekler.** Temel çalışma uygulamasını çok faktörlü kimlik doğrulaması için örnek kod.

> [!WARNING]
> İstemci sertifikası, özellikle sizin için oluşturulan benzersiz bir özel sertifikasıdır. Paylaşım değil veya bu dosyayı kaybedersiniz. Bu multi-Factor Authentication hizmetiyle, iletişimin güvenliğini sağlamak için anahtardır.

## <a name="code-sample"></a>Kod örneği
Bu kod örneği API'leri Azure multi-Factor Authentication SDK'sı Standart mod sesli arama doğrulama uygulamanıza eklemek için nasıl kullanılacağını gösterir. Standart mod kullanıcı için # tuşuna basarak yanıt bir telefon çağrısı içindir.

Bu örnek, C# sunucu tarafı mantığı ile temel bir ASP.NET uygulamasında C# .NET 2.0 çok faktörlü kimlik doğrulaması SDK kullanır, ancak diğer dillerde benzer bir işlemdir. SDK kaynak dosyaları, değil yürütülebilir dosyaları, içerdiğinden dosyaları oluşturmak ve bunları başvuru veya doğrudan uygulamanızda içermiyor.

> [!NOTE]
> Çok faktörlü kimlik doğrulamasını yaparken, ek bir yöntem (telefon araması veya kısa mesaj) ikincil veya üçüncül doğrulama birincil kimlik doğrulama yöntemi (kullanıcı adı ve parola) desteklemek için kullanın. Bu yöntemler, birincil kimlik doğrulama yöntemi olarak tasarlanmamıştır.

### <a name="code-sample-overview"></a>Kod örnek genel bakış
Bu örnek kod basit bir web demo uygulaması için bir telefon çağrısı kullanıcının kimlik doğrulamasını doğrulamak için # anahtar yanıtta kullanır. Bu telefon görüşmesi faktörü çok faktörlü kimlik doğrulaması Standart modu olarak bilinir.

İstemci tarafı kodlar tüm çok faktörlü kimlik doğrulaması özgü öğeleri içermez. Ek kimlik doğrulama faktörleri birincil kimlik doğrulaması için bağımsız olduğundan, varolan oturum açma arabirimi değiştirmeden ekleyebilirsiniz. Çok faktörlü SDK API'leri, kullanıcı deneyimini özelleştirmesini sağlar, ancak hiç değişikliği gerekmeyebilir.

Sunucu tarafı kodu, adım 2'de Standart mod kimlik doğrulaması ekler. Standart mod doğrulama için gerekli olan parametrelere sahip bir PfAuthParams nesnesi oluşturur: kullanıcı adı, telefon numarası ve mod ve her çağrıda gerekli olan istemci sertifikası (CertFilePath) yolu. PfAuthParams tüm parametreleri tanıtımı için SDK'sındaki örnek dosyasına bakın.

Ardından, kod PfAuthParams nesnesi pf_authenticate() işleve iletir. Başarı veya başarısızlık kimlik doğrulamasının dönüş değerini gösterir. Parametreler, callStatus ve errorID ek arama sonucu bilgilerini içerir. Arama sonuç kodları SDK arama sonuçları dosyasında belgelenmiştir.

Bu en az uygulama birkaç satırda yazılabilir. Ancak, üretim kodunda, daha gelişmiş hata işleme, ek veritabanı kodu ve geliştirilmiş bir kullanıcı deneyimi içerir.

### <a name="web-client-code"></a>Web istemci kodu
Bir tanıtım sayfası için web istemci kodu verilmiştir.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
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
Aşağıdaki sunucu tarafı kodu, çok faktörlü kimlik doğrulaması yapılandırılan ve 2. adımda çalıştırın. Standart mod (MODE_STANDARD) kullanıcı # tuşuna basarak yanıtının bir telefon çağrısı içindir.

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
