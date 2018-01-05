---
title: "Sessiz yükleme Azure AD uygulama ara sunucusu Bağlayıcısı | Microsoft Docs"
description: "Şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı katılımsız yüklemesini gerçekleştirmek nasıl ele alınmaktadır."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 3aa1c7f2-fb2a-4693-abd5-95bb53700cbb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a89a64499fba5ad0adf55863d809857f00edb2d5
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Azure AD uygulama ara sunucusu Bağlayıcısı için bir katılımsız yükleme komut dosyası oluşturma

Bu konu, katılımsız yükleme ve Azure AD uygulama ara sunucusu Bağlayıcısı için kayıt etkinleştiren bir Windows PowerShell betik oluşturmanıza yardımcı olur.

Bu özellik, aşağıdakileri yapmak istediğinizde yararlıdır:

* Etkin kullanıcı arabirimi yoksa veya Uzak Masaüstü'nü erişemeyen Windows sunucularında Bağlayıcısı'nı yükleyin.
* Yükleme ve aynı anda birçok bağlayıcılar kaydedin.
* Bağlayıcısını yükleme ve başka bir yordamının parçası olarak kayıt tümleştirin.
* Bağlayıcı BITS içerir ancak kaydedilmemiş standart sunucu görüntüsünü oluşturun.

İçin [uygulama ara sunucusu Bağlayıcısı](application-proxy-understand-connectors.md) çalışması için bir genel yönetici ve parola kullanarak Azure AD dizininizi ile kayıtlı olması gerekir. Normalde bu bilgileri açılan iletişim kutusunda Bağlayıcısı yüklemesi sırasında girilir ancak bunun yerine bu işlemi otomatikleştirmek için PowerShell kullanın.

Katılımsız yükleme için iki adımı vardır. İlk olarak, bağlayıcı yükleyin. İkinci olarak, bağlayıcıyı Azure AD ile kaydedin. 

## <a name="install-the-connector"></a>Bağlayıcısı'nı yüklemek
Kaydetme olmadan Bağlayıcısı'nı yüklemek için aşağıdaki adımları kullanın:

1. Bir komut istemi açın.
2. /Q Sessiz yükleme yani aşağıdaki komutu çalıştırın. Sessiz yükleme Son Kullanıcı Lisans Sözleşmesi'ni kabul ister değil.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Bağlayıcı Azure AD ile kaydetme
Bağlayıcı kaydetmek için kullanabileceğiniz iki yöntem vardır:

* Bir Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcı kaydetme
* Çevrimdışı oluşturan bir belirteç kullanarak bağlayıcı kaydetme

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Bir Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcı kaydetme
1. Bir Windows PowerShell kimlik bilgilerini nesnesi oluşturmak `$cred` bir yönetici kullanıcı adı ve parola dizininiz için içerir. Aşağıdaki komutu çalıştırın değiştirme  *\<kullanıcıadı\>*  ve  *\<parola\>*:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Git **C:\Program Files\Microsoft AAD uygulama Proxy Bağlayıcısı** ve aşağıdaki komut dosyasını kullanarak çalıştırma `$cred` oluşturduğunuz nesnesi:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred

### <a name="register-the-connector-using-a-token-created-offline"></a>Çevrimdışı oluşturan bir belirteç kullanarak bağlayıcı kaydetme
1. Bu kod parçacığında değerleri kullanarak Authenticationcontext'i sınıfını kullanarak çevrimdışı bir belirteç oluşturur:

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }


2. Belirteç olduktan sonra belirteci kullanarak bir SecureString oluşturun:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Aşağıdaki Windows PowerShell komutunu çalıştırın değiştirme \<GUID Kiracı\> , dizin kimliği:

   `RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID>`

## <a name="next-steps"></a>Sonraki adımlar 
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)
* [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
* [Uygulama Ara sunucusu ile ilgili sorunları giderme](active-directory-application-proxy-troubleshoot.md)


