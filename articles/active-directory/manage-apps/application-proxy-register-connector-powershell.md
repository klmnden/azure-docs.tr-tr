---
title: Azure AD uygulama ara sunucusu Bağlayıcısı sessiz yükleme | Microsoft Docs
description: Şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD uygulama ara sunucusu Bağlayıcısı katılımsız yüklemesini gerçekleştirmek nasıl etkinleştireceğinizi de açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb193119186c2cf9e758f8c74f99f18c5fb389b8
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58792527"
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Azure AD uygulama ara sunucusu Bağlayıcısı için Katılımsız yükleme betiği oluşturma

Bu konuda, katılımsız yükleme ve, Azure AD uygulama ara sunucusu Bağlayıcısı için kayıt sağlayan bir Windows PowerShell Betiği oluşturmanıza yardımcı olur.

Bu özellik, istediğinizde yararlıdır:

* Bağlayıcı, etkin kullanıcı arabirimi yoksa veya Uzak Masaüstü ile erişemiyor Windows sunucularını yükleyin.
* Yükleyin ve tek seferde çok sayıda bağlayıcı kaydedin.
* Bağlayıcıyı yükleme ve kayıt başka bir yordamının parçası olarak tümleştirin.
* Bağlayıcı BITS içerir ancak kayıtlı değil standart sunucu görüntüsünü oluşturun.

İçin [uygulama ara sunucusu bağlayıcısını](application-proxy-connectors.md) çalışmak için bir uygulama yönetici ve parolayı kullanarak, Azure AD dizini ile kayıtlı olması gerekir. Normalde bu bilgileri bir açılır iletişim kutusunda Bağlayıcısı yüklemesi sırasında girilir ancak bunun yerine bu işlemi otomatikleştirmek için PowerShell kullanabilirsiniz.

Katılımsız yükleme için iki adımı vardır. İlk olarak Bağlayıcısı'nı yükleyin. İkinci olarak, bağlayıcıyı Azure AD'ye kaydedin. 

## <a name="install-the-connector"></a>Bağlayıcısını yükleme
Kayıt olmadan Bağlayıcısı'nı yüklemek için aşağıdaki adımları kullanın:

1. Bir komut istemi açın.
2. /Q Sessiz yükleme anlamına gelir, aşağıdaki komutu çalıştırın. Sessiz yükleme son kullanıcı lisans sözleşmesini kabul ister değil.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Bağlayıcı Azure AD'ye kaydetme
Bağlayıcıyı kaydetmek için kullanabileceğiniz iki yöntem vardır:

* Bir Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcıyı kaydetme
* Çevrimdışı oluşturan bir belirteç kullanarak bağlayıcıyı kaydetme

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Bir Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcıyı kaydetme
1. Bir Windows PowerShell kimlik bilgileri nesnesi oluşturma `$cred` bir yönetici kullanıcı adı ve parola dizininiz için içerir. Aşağıdaki komutu çalıştırın değiştirerek *\<kullanıcıadı\>* ve  *\<parola\>*:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Git **C:\Program Files\Microsoft AAD uygulama Proxy Bağlayıcısı** ve kullanarak aşağıdaki betiği çalıştırın `$cred` oluşturduğunuz nesnesi:
   
        .\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature ApplicationProxy

### <a name="register-the-connector-using-a-token-created-offline"></a>Çevrimdışı oluşturan bir belirteç kullanarak bağlayıcıyı kaydetme
1. Bu kod parçacığı veya PowerShell cmdlet'lerini aşağıdaki değerleri kullanarak Authenticationcontext'i sınıfını kullanarak çevrimdışı bir belirteç oluşturun:

    **C# kullanarak:**

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

    **PowerShell kullanarak:**

        # Locate AzureAD PowerShell Module
        # Change Name of Module to AzureAD after what you have installed
        $AADPoshPath = (Get-InstalledModule -Name AzureAD).InstalledLocation
        # Set Location for ADAL Helper Library
        $ADALPath = $(Get-ChildItem -Path $($AADPoshPath) -Filter Microsoft.IdentityModel.Clients.ActiveDirectory.dll -Recurse ).FullName | Select-Object -Last 1
        
        # Add ADAL Helper Library
        Add-Type -Path $ADALPath
        
        #region constants
        
        # The AAD authentication endpoint uri
        [uri]$AadAuthenticationEndpoint = "https://login.microsoftonline.com/common/oauth2/token?api-version=1.0/" 
        
        # The application ID of the connector in AAD
        [string]$ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489"
        
        # The reply address of the connector application in AAD
        [uri]$ConnectorRedirectAddress = "urn:ietf:wg:oauth:2.0:oob" 
        
        # The AppIdUri of the registration service in AAD
        [uri]$RegistrationServiceAppIdUri = "https://proxy.cloudwebappproxy.net/registerapp"
        
        #endregion
        
        #region GetAuthenticationToken
        
        # Set AuthN context
        $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $AadAuthenticationEndpoint
        
        # Build platform parameters
        $promptBehavior = [Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Always
        $platformParam = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList $promptBehavior
        
        # Do AuthN and get token
        $authResult = $authContext.AcquireTokenAsync($RegistrationServiceAppIdUri.AbsoluteUri, $ConnectorAppId, $ConnectorRedirectAddress, $platformParam).Result
        
        # Check AuthN result
        If (($authResult) -and ($authResult.AccessToken) -and ($authResult.TenantId) ) {
        $token = $authResult.AccessToken
        $tenantId = $authResult.TenantId
        }
        Else {
        Write-Output "Authentication result, token or tenant id returned are null"
        }
        
        #endregion

2. Belirteci aldıktan sonra belirteci kullanarak bir SecureString oluşturun:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Aşağıdaki Windows PowerShell komutunu çalıştırın değiştirerek \<GUID Kiracı\> , dizin kimliği:

   `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID> -Feature ApplicationProxy`

## <a name="next-steps"></a>Sonraki adımlar 
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](application-proxy-configure-custom-domain.md)
* [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
* [Uygulama Ara sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)


