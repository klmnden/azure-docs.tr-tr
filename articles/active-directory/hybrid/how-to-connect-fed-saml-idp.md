---
title: 'Azure AD Connect: Çoklu oturum açma için SAML 2.0 kimlik sağlayıcısı kullanın | Microsoft Docs'
description: Bu belgede, çoklu oturum açma için SAML 2.0 uyumlu IDP kullanma açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1870137505b3d00ee6ed31595050908c970c444
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878103"
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Çoklu oturum açma için SAML 2.0 kimlik sağlayıcısı (IDP) kullanın

Bu belgede bir SAML 2.0 uyumlu SP Lite profili tabanlı kimlik sağlayıcısı, tercih edilen güvenlik belirteci hizmeti (STS) kullanma hakkında bilgi içeren / kimlik sağlayıcısı. Bu senaryo, bir kullanıcı dizini ve SAML 2.0 kullanarak erişilen şirket içinde depolamak parola zaten yararlıdır. Bu varolan bir kullanıcı dizinle oturum açmayı Office 365 ve diğer Azure AD güvenlikli kaynaklara için kullanılabilir. SAML 2.0 SP-Lite profili, bir oturum açma ve öznitelik exchange çerçeve sağlamak için yaygın olarak kullanılan güvenlik onaylama işlemi biçimlendirme dili (SAML) federe kimlik standardını temel alır.

>[!NOTE]
>3. taraf Azure AD ile kullanılmak üzere test edilmiştir IDP listesi için bkz: [Azure AD Federasyonu uyumluluk listesi](how-to-connect-fed-compatibility.md)

Microsoft tümleştirmesini düzgün bir şekilde yapılandırılmış, SAML 2.0 Idp'yi profili tabanlı ile Office 365 gibi bir Microsoft bulut hizmeti olarak bu oturum açma deneyimini destekler. SAML 2.0 kimlik sağlayıcısı olan üçüncü taraf ürünler ve bu nedenle Microsoft destek dağıtımı için yapılandırma, bunları ilgili en iyi sorun giderme sağlamaz. Bir kez düzgün bir şekilde yapılandırılmış, SAML 2.0 kimlik sağlayıcısı için uygun yapılandırma aşağıda daha ayrıntılı olarak açıklandığı Microsoft bağlantı Çözümleyicisi aracını kullanarak test edilebilir ile tümleştirme. SAML 2.0 SP-Lite profili tabanlı kimlik sağlayıcınız hakkında daha fazla bilgi için sağlanan kuruluş isteyin.

> [!IMPORTANT]
> Sınırlı sayıda istemciyle yalnızca bu senaryoda oturum açma SAML 2.0 kimlik sağlayıcılarıyla birlikte kullanılabilir, bu içerir:
> 
> - Outlook Web Access ve SharePoint Online gibi Web tabanlı istemciler
> - E-posta zengin istemciler, temel kimlik doğrulaması ve IMAP, POP, Active Sync, MAPI, (dağıtılması için Gelişmiş istemci protokol uç noktası gereklidir) vb. gibi desteklenen bir Exchange erişim yöntemi de dahil olmak üzere kullanır:
>     - Outlook 2013/Microsoft Outlook 2010/Outlook 2016'ın, Apple iPhone (iOS sürümleri çeşitli)
>     - Çeşitli Google Android cihazlar
>     - Windows Phone 7, Windows Phone 7.8 ve Windows Phone 8.0
>     - Posta istemcisi Windows 8 ve Windows 8.1 posta istemcisi
>     - Windows 10 posta istemcisi

Diğer tüm istemciler, bu senaryoda oturum açma, SAML 2.0 kimlik sağlayıcısı ile kullanılamaz. Örneğin, Lync 2010 masaüstü istemcisi ile çoklu oturum açma için yapılandırılmış, SAML 2.0 kimlik sağlayıcısı hizmette oturum açamaz değil.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Azure AD SAML 2.0 protokolü gereksinimleri
Bu belge, SAML 2.0 kimlik sağlayıcısı oturum açma bir veya daha fazla Microsoft bulut hizmetlerine (örneğin, Office 365) etkinleştirmek için Azure AD ile federasyona eklemek için uygulamalıdır biçimlendirme ileti ve protokolü ayrıntılı gereksinimler içermektedir. Bu senaryoda kullanılan bir Microsoft bulut hizmeti için SAML 2.0 bağlı olan taraf (SP-STS), Azure AD.

SAML 2.0 kimlik sağlayıcınız emin olmanız önerilir çıkış iletileri için sağlanan örnek izlemeleri gibi benzer mümkün olduğunca. Sağlanan özel öznitelik değerlerini de Azure AD meta verileri mümkün olduğunda. Çıkış iletilerinizi memnun olduğunuzda, aşağıda açıklandığı gibi Microsoft bağlantı Çözümleyicisi ile test edebilirsiniz.

Azure AD meta verileri bu URL'den indirilebilir: [ https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml ](https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Office 365 Çin özgü örneğini kullanan Çin'de müşteriler için aşağıdaki Federasyon uç noktası kullanılmalıdır: [ https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml ](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>SAML protokolü gereksinimleri
İstek ve yanıt iletisi çiftleri birlikte put nasıl ayrıntılarını sipariş iletilerinizi doğru şekilde biçimlendirmek için yardımcı olması için bu bölümü.

Azure AD, aşağıda listelenen bazı belirli gereksinimlerine SAML 2.0 SP Lite profilini kullanan kimlik sağlayıcıları ile çalışacak şekilde yapılandırılabilir. Otomatik ve el ile test yanı sıra örnek SAML isteği ve yanıt iletileri kullanarak, Azure AD ile birlikte çalışabilirlik elde etmek için çalışabilir.

## <a name="signature-block-requirements"></a>İmza bloğu gereksinimleri
SAML yanıtını iletisi içindeki imza düğüm iletinin dijital imza hakkında bilgi içerir. İmza bloğunu aşağıdaki gereksinimlere sahiptir:

1. Onaylama işlemi düğümü oturum açmış olmanız gerekir
2.  RSA sha1 algoritması DigestMethod kullanılmalıdır. Diğer dijital imza algoritmaları kabul edilmez.
   `<ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  Ayrıca, XML belgesi oturum açabilirsiniz. 
4.  Dönüştürme algoritması, aşağıdaki örnekte değerlerin eşleşmesi gerekir:    `<ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  Aşağıdaki örnek SignatureMethod'a algoritması eşleşmesi gerekir:   `<ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Desteklenen bağlamaları
Bağlamaları gerekli olan aktarım ilgili bilgiler parametreleridir. Bağlamaları aşağıdaki gereksinimler geçerlidir

1. Gerekli aktarım https'dir.
2.  Azure AD oturum açma sırasında token gönderimi için HTTP POST gerektirir
3.  Azure AD kimlik doğrulama isteğini yeniden yönlendirme ve kimlik sağlayıcısı için kimlik sağlayıcısı oturum kapatma iletiye için HTTP POST kullanın.

## <a name="required-attributes"></a>Gerekli öznitelikleri
Bu tabloda belirli öznitelikler için gereksinimleri SAML 2.0 iletisinde gösterilir.
 
|Öznitelik|Açıklama|
| ----- | ----- |
|Nameıd|Bu onay değerini Azure AD kullanıcı Immutableıd ile aynı olmalıdır. Bu, en fazla 64 alfasayısal karakter olabilir. Güvenli olmayan html karakterler kodlanmış olması gerekir, örneğin, "+" karakter ".2B" gösterilir.|
|IDPEmail|Kullanıcı asıl adı (UPN) SAML yanıtta IDPEmail ada sahip bir öğe olarak listelenen Azure AD/Office 365'te kullanıcının UserPrincipalName (UPN). E-posta adresi biçiminde UPN'dir. Office 365'te Windows (Azure Active Directory) UPN değeri.|
|Veren|Kimlik sağlayıcısının bir URI olması gerekir. Örnek ileti verenden yeniden kullanmayın. Birden çok en üst düzey etki alanı içinde Azure AD kiracılarıyla varsa verici etki alanı başına yapılandırılmış belirtilen URI ayarı eşleşmelidir.|

>[!IMPORTANT]
>Şu anda Azure AD için SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid aşağıdaki Nameıd biçimi URI destekler-biçimi: kalıcı.

## <a name="sample-saml-request-and-response-messages"></a>Örnek SAML isteği ve yanıt iletileri
Bir istek ve yanıt iletisi çift oturum açma ileti değişimi için gösterilir.
Azure AD'den bir örnek SAML 2.0 kimlik sağlayıcısına gönderilen örnek bir istek iletisi aşağıda verilmiştir. Active Directory Federasyon Hizmetleri (AD FS) SAML-P protokolünü kullanmak üzere yapılandırılmış örnek SAML 2.0 kimlik sağlayıcısı var. Birlikte çalışabilirliği test diğer SAML 2.0 kimlik sağlayıcıları ile tamamlandı.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Örnek SAML 2.0 uyumlu bir kimlik sağlayıcısından Azure AD'ye gönderilen örnek bir yanıt iletisi aşağıda verilmiştir / Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>SAML 2.0 uyumlu kimlik sağlayıcınızı yapılandırın
Bu bölüm, SAML 2.0 kimlik sağlayıcısı SAML 2.0 protokolünü kullanarak bir veya daha fazla Microsoft bulut hizmetlerine (örneğin, Office 365) çoklu oturum açma erişimi etkinleştirmek için Azure AD ile federasyona eklemek için yapılandırma hakkında yönergeler içerir. Azure AD bağlı olan taraf Bu senaryoda kullanılan bir Microsoft bulut hizmeti için SAML 2.0.

## <a name="add-azure-ad-metadata"></a>Azure AD meta verileri ekleme
SAML 2.0 kimlik sağlayıcınız, Azure AD bağlı olan taraf hakkındaki bilgilere bağlı olması gerekiyor. Azure AD, meta veriler en yayımlar https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

Her zaman en son Azure AD'ye meta veriler, SAML 2.0 kimlik sağlayıcısı yapılandırırken içe aktardığınız emin önerilir.

>[!NOTE]
>Azure AD kimlik sağlayıcısından gelen meta veri okumaz.

## <a name="add-azure-ad-as-a-relying-party"></a>Azure AD bağlı olan taraf Ekle
SAML 2.0 kimlik sağlayıcısı ve Azure AD arasındaki iletişimi etkinleştirmeniz gerekir. Bu yapılandırma, belirli bir kimlik sağlayıcısını bağımlı olacaktır ve bu belgelerine başvurmanız gerekir. Genellikle bağlı olan taraf kimliği Entityıd aynı Azure AD meta verilerine ayarlarsınız.

>[!NOTE]
>SAML 2.0 kimlik sağlayıcısı sunucunuzdaki saati bir doğru zaman kaynağı eşitlendiğini doğrulayın. Yanlış bir saatin, Federasyon oturumu başarısız olmasına neden olabilir.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>SAML 2.0 kimlik sağlayıcısı ile oturum açma için Windows PowerShell'i yükleme
Azure AD ile oturum açma, SAML 2.0 kimlik sağlayıcısı kullanmak için yapılandırdıktan sonra sonraki adımda Azure Active Directory için Windows PowerShell modülü yükleyip sağlamaktır. Yüklendikten sonra Azure AD etki alanlarınızı Federasyon etki alanları yapılandırmak için bu cmdlet'leri kullanır.

Azure Active Directory için Windows PowerShell modülü, Azure AD'de Kuruluş verilerinizde yönetmeye yönelik bir yüklemedir. Bu modül için Windows PowerShell cmdlet'leri kümesini yükler; Azure ad çoklu oturum açma erişimi ayarlamak için bu cmdlet'leri çalıştırın ve ardından tüm bulut hizmetlerine abone olduğunuz. İndirin ve cmdlet'lerini yükleme hakkında yönergeler için bkz: [https://technet.microsoft.com/library/jj151815.aspx](https://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>SAML kimlik sağlayıcısı ve Azure AD arasında güven oluşturma
Bir Azure AD etki alanında Federasyon yapılandırmadan önce yapılandırılmış özel bir etki alanının olması gerekir. Microsoft tarafından sağlanan varsayılan etki alanı federasyona eklenemez. Microsoft gelen varsayılan etki alanı "onmicrosoft.com" ile biter.
Bir dizi cmdlet eklemek veya dönüştürmek için çoklu oturum açma etki alanları için Windows PowerShell komut satırı arabirimi çalıştırır.

SAML 2.0 kimlik sağlayıcısı kullanarak birleştirmek istediğiniz her bir Azure Active Directory etki alanı bir çoklu oturum açma etki olarak eklenmeli veya çoklu oturum açma etki alanından standart bir etki alanına dönüştürülmelidir. SAML 2.0 kimlik sağlayıcısı ve Azure AD arasında güven ekleme veya bir etki alanı dönüştürme ayarlar.

Aşağıdaki yordam, SAML 2.0 SP-Lite'ı kullanarak bir Federasyon etki alanına varolan standart bir etki alanını dönüştürme işlemlerinde size yol gösterir. 

>[!NOTE]
>Etki alanınız için 2 saat sonra bu adımı yukarı kullanıcıları etkileyen kesinti yaşayabilirsiniz.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Federasyon için Azure AD dizininizde bir etki alanını yapılandırma


1. Azure AD dizininiz için Kiracı Yöneticisi olarak bağlanın: Connect-MsolService.
2.  SAML 2.0 ile Federasyon kullanmak için istenen Office 365 etki alanınızı yapılandırın: `$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  İmzalama sertifikasını base64 ile kodlanmış dize IDP meta verileri dosyanızdan elde edebilirsiniz. Bu konum örneği sağlanmadı ancak uygulamanız üzerinde biraz göre farklılık gösterebilir.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

"Set-MsolDomainAuthentication" hakkında daha fazla bilgi için bkz: [ https://technet.microsoft.com/library/dn194112.aspx ](https://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Kullanım çalıştırmalısınız `$ecpUrl = "https://WS2012R2-0.contoso.com/PAOS"` kimlik sağlayıcınız ECP uzantı ayarlarsanız. Outlook Web uygulaması (OWA) hariç olmak üzere, Exchange Online istemcilerin güvendiği bir GÖNDERİ etkin uç noktasına bağlı. Shibboleth'ın ECP uygulamasına yönelik etkin bir uç noktayı benzer bir etkin uç noktası, SAML 2.0 STS uyguluyorsa Exchange Online hizmetiyle etkileşim kurmak bu zengin istemciler mümkün olabilir.

Federasyon yapılandırıldıktan sonra geri "Federasyon olmayan" (veya "yönetilen"), ancak bu değişikliğin tamamlanması iki saat sürer geçebilir ve her kullanıcı için bulut tabanlı oturum açma için yeni rastgele parolalar atama gerektirir. Geri "yönetilen" geçiş hatayla ayarlarınızı sıfırlamak için bazı senaryolarda gerekebilir. Etki alanı dönüştürme hakkında daha fazla bilgi için bkz: [ https://msdn.microsoft.com/library/windowsazure/dn194122.aspx ](https://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-to-azure-ad--office-365"></a>Azure ad kullanıcı asıl adları sağlama / Office 365
Kullanıcılarınız Office 365 için kimlik doğrulama gerçekleştirmeden önce SAML 2.0 talep onaylama karşılık gelen kullanıcı asıl adları ile Azure AD hazırlamanız gerekir. Bu kullanıcı ilkeleri önceden Azure AD'ye bilinmiyorsa, ardından bunlar Federasyon oturum açmak için kullanılamaz. Azure AD Connect ya da Windows PowerShell, kullanıcı asıl adları sağlamak için kullanılabilir.

Azure AD Connect, şirket içi Active Directory'den etki alanlarınızı Azure AD dizininiz için ilkeleri sağlamak için kullanılabilir. Daha ayrıntılı bilgi için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).

Windows PowerShell, Azure AD'ye yeni kullanıcı ekleme otomatikleştirme ve şirket içi dizinden değişiklikleri eşitlemek için kullanılabilir. Windows PowerShell cmdlet'lerini kullanmak için yüklemelidir [Azure Active Directory modülleri](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Bu yordam, tek bir kullanıcı Azure AD'ye ekleme gösterilmektedir.


1. Azure AD dizininiz için Kiracı Yöneticisi olarak bağlanın: Connect-MsolService.
2.  Yeni bir kullanıcı asıl oluşturun:
    ```powershell
    New-MsolUser
      -UserPrincipalName elwoodf1@contoso.com
      -ImmutableId ABCDEFG1234567890
      -DisplayName "Elwood Folk"
      -FirstName Elwood 
      -LastName Folk 
      -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
      -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
      -UsageLocation "US" 
    ```

"New-MsolUser" kullanıma alma hakkında daha fazla bilgi için [https://technet.microsoft.com/library/dn194096.aspx](https://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>"UserPrinciplName" değer "IDPEmail için", SAML 2.0 talebi gönderecek bir değerle eşleşmelidir ve "Immutableıd" değer "Nameıd" değerinizi gönderilen değerle eşleşmelidir.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Çoklu oturum açma, SAML 2.0 IDP'yi ile doğrulayın.
Yönetici olarak doğrulayın ve çoklu oturum açma (Ayrıca çağrılan Kimlik Federasyonu) yönetmek için önce bilgileri gözden geçirin ve çoklu oturum açma, SAML 2.0 SP-Lite tabanlı kimlik sağlayıcısı ile ayarlamak için aşağıdaki makalelere göz atın adımları uygulayın:


1.  Azure AD SAML 2.0 protokolü gereksinimleri gözden geçirmeniz gerekir
2.  SAML 2.0 kimlik sağlayıcısı yapılandırmış olmanız
3.  SAML 2.0 kimlik sağlayıcısı ile çoklu oturum açma için Windows PowerShell'i yükleme
4.  SAML 2.0 kimlik sağlayıcısı ve Azure AD arasında güven oluşturma
5.  Azure Active Directory (Office 365) için bir bilinen test kullanıcı asıl, Windows PowerShell veya Azure AD Connect ile sağlandı.
6.  Dizin eşitleme kullanarak yapılandırma [Azure AD Connect](whatis-hybrid-identity.md).

Çoklu oturum açma, SAML 2.0 SP-tabanlı Lite kimlik sağlayıcısı ile yedekleme ayarladıktan sonra doğru şekilde çalıştığını doğrulamanız gerekir.

>[!NOTE]
>Bir eklemek yerine bir etki alanı dönüştürülürse, çoklu oturum açmayı ayarlamak için 24 saat sürebilir.
Çoklu oturum açma doğrulamadan önce Active Directory eşitlemesini ayarlama işlemini sonlandırmak, sonra dizinlerinizin eşitlenmesi ve, eşitlenmiş kullanıcıları etkinleştirin.

### <a name="use-the-tool-to-verify-that-single-sign-on-has-been-set-up-correctly"></a>Bu çoklu oturum açma doğru şekilde ayarlandığını gösterdiğinde doğrulamak için Aracı'nı kullanın
Bu çoklu oturum açma doğru şekilde ayarlandığını gösterdiğinde doğrulamak için bulut hizmetine şirket kimlik bilgilerinizle oturum açmanız mümkün olduğunu onaylamak için aşağıdaki yordamı gerçekleştirebilirsiniz.

SAML 2.0 tabanlı kimlik sağlayıcınız test etmek için kullanabileceğiniz bir aracı Microsoft sağlamıştır. Test aracı çalıştırmadan önce kimlik sağlayıcınız ile federasyona eklemek için Azure AD kiracısı yapılandırmış olmanız gerekir.

>[!NOTE]
>Bağlantı Çözümleyicisi'ni Internet Explorer 10 veya üstünü gerektirir.



1. Bağlantı Çözümleyicisi'ni indirin [ https://testconnectivity.microsoft.com/?tabid=Client ](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Başlamak için Şimdi Yükle'ye tıklayın. aracı yükleyip.
3.  "Office 365, Azure veya Azure Active Directory kullanan diğer hizmetlerde ile Federasyon oluşturamıyorum" seçin.
4.  Araç, indirilen ve çalışır durumdaysa, bağlantı tanılama penceresinde görürsünüz. Aracı'nı Federasyon bağlantınızı test sürecinde adım adım anlatır.
5.  Bağlantı Çözümleyicisi'ni, oturum açma, sınamakta olduğunuz kullanıcı asıl için kimlik bilgilerini girin, SAML 2.0 IDP'yi açılır: ![SAML](./media/how-to-connect-fed-saml-idp/saml1.png)
6.  Federasyon test oturum açma penceresinde, SAML 2.0 kimlik sağlayıcınız ile federasyona eklenmesi için yapılandırılmış Azure AD kiracısı için bir hesap adı ve parola girmelisiniz. Aracı bu kimlik bilgilerini kullanarak oturum açın dener ve oturum açma denemesi sırasında gerçekleştirilen testin ayrıntılı sonuçları çıktı olarak sağlanacaktır.
![SAML](./media/how-to-connect-fed-saml-idp/saml2.png)
7. Bu pencere, başarısız bir test sonucunu gösterir. Tıklayarak gözden geçirme ayrıntılı sonuçları gerçekleştirildiği her test sonuçlarıyla ilgili bilgileri gösterir. Ayrıca, bunları paylaşmak için disk sonuçlarını da kaydedebilirsiniz.
 
>[!NOTE]
>Bağlantı Çözümleyicisi'ni kullanarak WS * aktif Federasyonu da test-tabanlı ve ECP/PAOS protokoller. Bunları kullanmıyorsanız şu hatayı alırsanız göz ardı edebilirsiniz: Kimlik sağlayıcınızın aktif Federasyonu uç noktayı kullanarak etkin oturum açma akışını test etme.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Bu çoklu oturum açma doğru şekilde ayarlandığını gösterdiğinde el ile doğrulama
El ile doğrulama, SAML 2.0 kimlik sağlayıcısı pek çok senaryoda düzgün çalıştığından emin olmak için uygulayabileceğiniz ek adımları sağlar.
Bu çoklu oturum açma doğru şekilde ayarlandığını gösterdiğinde doğrulamak için aşağıdaki adımları tamamlayın:


1. Etki alanına katılmış bir bilgisayarda oturum için Kurumsal kimlik bilgilerinizi aynı oturum açma adını kullanarak bulut hizmetinizi açın.
2.  Parola kutusunun içine tıklayın. Çoklu oturum açma ayarlanıp ayarlanmadığını parola kutusu gölgeli olur ve aşağıdaki iletiyi görürsünüz: "Artık, oturum açmanız gereklidir &lt;şirketinizin&gt;."
3.  Oturum açmada tıklayın &lt;şirketinizin&gt; bağlantı. Mümkünse oturum açma, tek oturum açma ayarlandığını gösterdiğinde.

## <a name="next-steps"></a>Sonraki Adımlar


- [Active Directory Federasyon Hizmetleri Yönetimi ve Azure AD Connect ile özelleştirme](how-to-connect-fed-management.md)
- [Azure AD federasyonu uyumluluk listesi](how-to-connect-fed-compatibility.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
