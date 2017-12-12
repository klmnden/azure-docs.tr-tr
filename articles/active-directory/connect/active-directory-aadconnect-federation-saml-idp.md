---
title: "Azure AD Connect: SAML 2.0 kimlik sağlayıcısı için çoklu oturum açma kullanın | Microsoft Docs"
description: "Bu konu, çoklu oturum açma için SAML 2.0 uyumlu IDP kullanarak açıklar."
services: active-directory
author: billmath
manager: mtillman
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 46c65e0efdc91b70c5d0d2afdf83d7205efc8057
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Çoklu oturum açma için SAML 2.0 kimlik sağlayıcısı (IDP) kullanın

Bu konudaki bilgileri içeren bir SAML 2.0 kullanımına uyumlu SP Lite profili kimlik sağlayıcısı olarak tercih edilen güvenlik belirteci hizmeti (STS) dayalı / kimlik sağlayıcısı. Zaten bir kullanıcı dizini ve SAML 2.0 kullanarak erişilen şirket içi depolama parola olduğu bu yararlı olur. Bu olan bir kullanıcı dizini, oturum açma Office 365 ve Azure AD güvenli kaynaklar için kullanılabilir. SAML 2.0 SP-Lite profili, bir oturum açma ve öznitelik exchange çerçeve sağlamak üzere yaygın olarak kullanılan güvenlik onaylama işlemi biçimlendirme dili (SAML) federe kimlik standardını temel temel alır.

>[!NOTE]
>3. taraf Azure AD ile kullanmak için test Idps listesi için bkz: [Azure AD Federasyonu uyumluluk listesi](active-directory-aadconnect-federation-compatibility.md)

Microsoft Office 365 gibi bir Microsoft bulut Hizmeti Tümleştirme olarak bu oturum açma deneyimini destekler, düzgün şekilde yapılandırılmış, SAML 2.0 ile IDP profiline dayalı. SAML 2.0 kimlik sağlayıcısı üçüncü taraf ürünleri olan ve bu nedenle Microsoft destek dağıtımı için yapılandırma, bunları ilgili en iyi uygulamaları sorunlarını giderme sağlamaz. Bir kez düzgün bir şekilde yapılandırılmış, SAML kimlik sağlayıcısı için uygun yapılandırma aşağıdaki daha ayrıntılı olarak açıklanan Microsoft bağlantı Çözümleyicisi aracını kullanarak test edilebilir 2.0 ile tümleştirme. SAML 2.0 SP-Lite profili tabanlı kimlik sağlayıcınızı hakkında daha fazla bilgi için onu sağlanan kuruluş isteyin.

>[!IMPORTANT]
>Bu, sınırlı sayıda istemciyle yalnızca bu senaryoda oturum açma SAML 2.0 kimlik sağlayıcısı ile kullanılabilir içerir:

>- Outlook Web Access ve SharePoint Online gibi Web tabanlı istemciler
- E-posta zengin istemciler, temel kimlik doğrulaması ve IMAP, POP, Active Sync, MAPI, (Gelişmiş istemci protokol uç noktası dağıtılması için gerekli değildir) vb. gibi desteklenen bir Exchange erişim yöntemi de dahil olmak üzere kullanır:
    - Outlook 2013/Microsoft Outlook 2010/Outlook 2016, Apple iPhone (çeşitli iOS sürümleri)
    - Çeşitli Google Android cihazları
    - Windows Phone 7, Windows Phone 7,8 ve Windows Phone 8.0
    - Windows 8 posta istemcisi ve Windows 8.1 posta istemcisi
    - Windows 10 posta istemcisi

Diğer tüm istemcileri, bu senaryoda oturum açma, SAML 2.0 kimlik sağlayıcısı ile kullanılamaz. Örneğin, Lync 2010 masaüstü istemcisi için çoklu oturum açmayı yapılandırılmış, SAML 2.0 kimlik sağlayıcısı ile hizmette oturum açabilmeniz değil.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Azure AD SAML 2.0 protokolü gereksinimleri
Bu konu, ayrıntılı gereksinimler protokolü ve oturum açma bir veya daha fazla Microsoft bulut hizmetlerine (örneğin, Office 365) etkinleştirmek için Azure AD ile birleştirmek için SAML 2.0 kimlik sağlayıcısı uygulamalıdır biçimlendirme iletiyi içerir. Bu senaryoda kullanılan bir Microsoft bulut hizmeti için SAML 2.0 bağlı olan taraf (SP-STS) Azure AD ' dir.

SAML 2.0 kimlik sağlayıcısı sağlamanız önerilir çıktı iletileri için sağlanan örnek izlemeleri kadar benzer mümkün olduğunca. Ayrıca, sağlanan belirli öznitelik değerlerinden kullanın Azure AD meta verileri, mümkün olduğunda. Çıktı iletilerinizi Mutluluk duyuyoruz sonra ile Microsoft bağlantı Çözümleyicisi'ni aşağıda açıklandığı gibi test edebilirsiniz.

Azure AD meta verileri bu URL'den indirilebilir: [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](http://https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Office 365 Çin özgü örneğini kullanan Çin'de müşteriler için aşağıdaki Federasyon bitiş noktasını kullanılmalıdır: [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>SAML protokolü gereksinimleri
İstek ve yanıt iletisi çiftleri birlikte put nasıl ayrıntıları sipariş iletilerinizi doğru biçimlendirmek için yardımcı olması için bu bölümü.

Azure AD, aşağıda listelenen bazı belirli gereksinimleri ile SAML 2.0 SP Lite profilini kullanan kimlik sağlayıcıları çalışmak üzere yapılandırılabilir. Otomatik ve el ile test birlikte örnek SAML istek ve yanıt iletileri kullanarak, Azure AD ile birlikte çalışabilirlik elde etmek için çalışabilir.

## <a name="signature-block-requirements"></a>İmza bloğu gereksinimleri
SAML yanıt iletisi içinde imza düğüm iletinin dijital imza hakkında bilgi içerir. İmza bloğunu aşağıdaki gereksinimlere sahiptir:

1. Onaylama işlemi düğüm imzalanmalıdır.
2.  RSA sha1 algoritması DigestMethod kullanılmalıdır. Diğer dijital imza algoritması kabul edilmedi.
   `<ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  XML belgesi de kaydolabilirsiniz. 
4.  Dönüştürme algoritmasını aşağıdaki örneği değerleri aynı olması gerekir.`<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  SignatureMethod'a algoritması aşağıdaki örnekle eşleşmesi gerekir:`<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Desteklenen bağlamaları
Bağlamaları değil taşıma ilgili gerekli iletişimleri parametreleri. Aşağıdaki gereksinimleri bağlamaları için geçerlidir

1. HTTPS gerekli Aktarım ' dir.
2.  Azure AD oturum açma sırasında belirteci gönderimi için HTTP POST gerektirir
3.  Azure AD kimlik doğrulama isteğini yeniden yönlendirme ve kimlik sağlayıcısı kimlik sağlayıcısı için oturum kapatma iletisi için HTTP POST kullanır.

## <a name="required-attributes"></a>Gerekli öznitelikler
Bu tablo belirli öznitelikler için gereksinimleri SAML 2.0 iletisinde gösterir.
 
|Öznitelik|Açıklama|
| ----- | ----- |
|NameID|Bu onay değeri Azure AD kullanıcının İmmutableıd ile aynı olması gerekir. En fazla 64 alfasayısal karakter olabilir. Güvenli olmayan HTML karakterler kodlanmış olmalıdır, örneğin "+" karakter ".2B" gösterilir.|
|IDPEmail|Kullanıcı asıl adı (UPN) listelenen Azure AD/Office 365'te kullanıcının UserPrincipalName (UPN) budur IDPEmail ada sahip bir öğe olarak SAML yanıt. UPN, e-posta adresi biçime sahip. Windows Office 365 (Azure Active Directory) UPN değeri.|
|Veren|Bu, kimlik sağlayıcısı, bir URI olması gereklidir. Örnek iletileri verenden yeniden kullanmamalısınız. Veren, Azure AD kiracılar birden çok üst düzey etki alanı varsa, etki alanı başına yapılandırılmış belirtilen URI ayarı eşleşmesi gerekir.|

>[!IMPORTANT]
>Şu anda Azure AD için SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid aşağıdaki NameID biçimi URI destekler-biçimi: kalıcı.

## <a name="sample-saml-request-and-response-messages"></a>Örnek SAML istek ve yanıt iletileri
Bir istek ve yanıt iletisi çift oturum açma ileti değişimi için gösterilir.
Azure AD'den bir örnek SAML 2.0 kimlik sağlayıcısı için gönderilen bir örnek istek iletisi budur. Örnek SAML 2.0 kimlik sağlayıcısı Active Directory Federasyon Hizmetleri (AD FS) SAML-P protokolünü kullanmak üzere yapılandırılmış ' dir. Birlikte çalışabilirlik sınaması diğer SAML 2.0 kimlik sağlayıcıları ile tamamlandı.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Bu örnek SAML 2.0 uyumlu kimlik sağlayıcısından Azure AD ile gönderilen örnek yanıt iletisidir / Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
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

## <a name="configure-your-saml-20-compliant-identity-provider"></a>SAML 2.0 uyumlu kimlik sağlayıcısı yapılandırma
Bu konu, tek oturum açma SAML 2.0 protokolünü kullanarak bir veya daha fazla Microsoft bulut hizmetlerine erişim (örneğin, Office 365) etkinleştirmek için Azure AD ile birleştirmek için SAML 2.0 kimlik sağlayıcısı yapılandırma hakkında yönergeler içerir. Azure AD bağlı olan taraf Bu senaryoda kullanılan bir Microsoft bulut hizmeti için SAML 2.0 olur.

## <a name="add-azure-ad-metadata"></a>Azure AD meta verileri ekleme
SAML 2.0 kimlik sağlayıcısı, bağlı olan taraf Azure AD ile ilgili bilgilere bağlı olması gerekiyor. Azure AD https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml konumundaki meta verilerini yayımlar.

Her zaman en son Azure AD meta SAML 2.0 kimlik sağlayıcısı yapılandırırken içe olduğunu önerilir. Azure AD kimlik sağlayıcısı'ndan meta verileri okumaz unutmayın.

## <a name="add-azure-ad-as-a-relying-party"></a>Azure AD bağlı olan taraf Ekle
SAML 2.0 kimlik sağlayıcısı ve Azure AD arasındaki iletişimi etkinleştirmeniz gerekir. Bu yapılandırma, belirli bir kimlik sağlayıcısı bağımlı olur ve bu belgelere başvurmalıdır. Genellikle bağlı olan taraf kimliği Entityıd aynı Azure AD meta verilerini ayarlamanız.

>[!NOTE]
>SAML 2.0 kimlik sağlayıcısı sunucunuzda saat için doğru zaman kaynağı eşitlendiğini doğrulayın. Yanlış bir saatin, federe oturum açma başarısız olmasına neden olabilir.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>SAML 2.0 kimlik sağlayıcısı ile oturum açma için Windows PowerShell yükleme
Azure AD oturum açma ile kullanım için SAML 2.0 kimlik sağlayıcınız yapılandırdıktan sonra sonraki adım Azure Active Directory için Windows PowerShell modülü yükleyip olmaktır. Yüklendikten sonra Azure AD etki alanlarınızı Federasyon etki alanı yapılandırmak için bu cmdlet'leri kullanır.

Azure Active Directory için Windows PowerShell modülü, Azure AD'de Kuruluş verilerinizde yönetmeye yönelik bir yüklemedir. Bu modül için Windows PowerShell cmdlet'leri kümesini yükler; Azure ad çoklu oturum açma erişimini ayarlamak için bu cmdlet'leri çalıştırın ve ardından tüm bulut hizmetlerine abone olduğunuz. Cmdlet'leri yükleyip hakkında yönergeler için bkz: [http://technet.microsoft.com/library/jj151815.aspx](http://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>SAML kimlik sağlayıcısı ve Azure AD arasında güven oluşturma
Azure AD etki alanı üzerinde Federasyon yapılandırmadan önce özel bir etki alanı yapılandırılmış olması gerekir. Microsoft tarafından sağlanan varsayılan etki alanını birleştiremeyiz. Varsayılan etki alanı Microsoft "onmicrosoft.com" ile sona erer.
Cmdlet'leri bir dizi eklemek veya etki alanları için çoklu oturum açma dönüştürmek için Windows PowerShell komut satırı arabirimi çalıştırır.

SAML 2.0 kimlik sağlayıcısı kullanarak birleştirmek istediğiniz her bir Azure Active Directory etki alanı bir tek oturum açma etki olarak eklenmeli veya bir tek oturum açma etki alanından standart bir etki alanına dönüştürülmelidir. SAML 2.0 kimlik sağlayıcısı ve Azure AD arasında güven ekleme veya bir etki alanı dönüştürme ayarlar.

Aşağıdaki yordamda var olan bir standart etki SAML 2.0 SP-Lite kullanarak bir Federasyon etki alanına dönüştürme konusunda size yol gösterir. Etki alanınızı kullanıcılar için bu adımı sonraki 2 saat etkiler kesinti yaşayabilirsiniz unutmayın.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Federasyon için Azure AD dizininizdeki bir etki alanını yapılandırma


1. Azure AD dizininizi bir kiracı yönetici olarak bağlanın: bağlanmak MsolService.
2.  SAML 2.0 ile Federasyon kullanmak için istenen Office 365 etki alanınızı yapılandırın:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  İmzalama sertifikası base64 kodlu dize IDP meta veri dosyasından elde edebilirsiniz. Bu konum örneği sağlanmış olan ancak biraz uygulamanız göre farklılık gösterebilir.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

"Set-MsolDomainAuthentication" hakkında daha fazla bilgi için bkz: [http://technet.microsoft.com/library/dn194112.aspx](http://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Kullanım çalıştırmanız gerekir "$ecpUrl"https://WS2012R2-0.contoso.com/PAOS"=" Kimlik sağlayıcısı için ECP uzantı ayarlarsanız. Exchange Online istemciler, Outlook Web uygulaması (OWA) hariç Bel etkin uç noktası bir POST tabanlı. SAML 2.0 STS Shibboleth'ın ECP uyarlamasını bir etkin uç noktası için benzer bir etkin uç noktası uyguluyorsa Exchange Online hizmetiyle etkileşim kurmak bu zengin istemcileri için mümkün olabilir.

Federasyon yapılandırıldıktan sonra geri "Federasyon olmayan" (veya "yönetilen"), ancak bu değişikliği tamamlamak için iki saat sürer geçiş yapabilirsiniz ve her kullanıcı için bulut tabanlı oturum açma için yeni rastgele parolaları atama gerektirir. Geri "yönetilen" geçiş bazı senaryolarda ayarlarınızda hata sıfırlamak için gerekebilir. Etki alanı dönüştürme hakkında daha fazla bilgi için bkz: [http://msdn.microsoft.com/library/windowsazure/dn194122.aspx](http://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-to-azure-ad--office-365"></a>Azure ad kullanıcı ilkeleri sağlamak / Office 365
Office 365 kullanıcılarınızın kimliğini doğrulamadan önce SAML 2.0 talep onaylama karşılık gelen kullanıcı ilkeleri ile Azure AD hazırlamanız gerekir. Bu kullanıcı ilkeleri önceden Azure AD ile bilinmiyorsa sonra bunlar federe oturum açma için kullanılamaz. Azure AD Connect veya Windows PowerShell, kullanıcı ilkeleri sağlamak için kullanılabilir.

Azure AD Connect, şirket içi Active Directory'den Azure AD dizininizdeki etki alanlarınızı için asıl adlar sağlamak için kullanılabilir. Daha ayrıntılı bilgi için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

Windows PowerShell için Azure AD yeni kullanıcı ekleme otomatikleştirmek ve şirket içi dizin değişikliklerden eşitlemek için de kullanılabilir. İndirmeniz gerekir Windows PowerShell cmdlet'lerini kullanmak için [Azure Active Directory modülleri](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Bu yordamda, Azure AD ile tek bir kullanıcı eklemek gösterilmiştir.


1. Azure AD dizininizi bir kiracı yönetici olarak bağlanın: bağlanmak MsolService.
2.  Yeni bir kullanıcı asıl oluşturun:` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

"Yeni-MsolUser" kullanıma, hakkında daha fazla bilgi için [http://technet.microsoft.com/library/dn194096.aspx](http://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>"UserPrinciplName" değeri "IDPEmail için" içinde SAML 2.0 talebi gönderecek değerle eşleşmelidir ve "İmmutableıd" değeri "NameID" değerinizi gönderilen değerle eşleşmelidir.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Çoklu oturum açma ile SAML 2.0 IDP doğrulayın
Yönetici olarak doğrulayın ve çoklu oturum açma (Ayrıca çağrılan Kimlik Federasyonu) yönetmek için önce bilgileri gözden geçirin ve çoklu oturum açma SAML 2.0 SP-Lite tabanlı kimlik sağlayıcınız ile ayarlamak için aşağıdaki makalelerde adımları uygulayın:


1.  Azure AD SAML 2.0 protokolü gereksinimleri gözden geçirdikten
2.  SAML 2.0 kimlik sağlayıcısı yapılandırmış olduğunuz
3.  SAML 2.0 kimlik sağlayıcısı ile çoklu oturum açma için Windows PowerShell yükleme
4.  SAML 2.0 kimlik sağlayıcısı ve Azure AD arasında güven oluşturma
5.  Bir bilinen test kullanıcı asıl Azure Active Directory (Office 365) için Windows PowerShell veya Azure AD Connect ile sağlandı.
6.  Dizin eşitleme kullanarak yapılandırma [Azure AD Connect](active-directory-aadconnect.md).

Çoklu oturum açma SAML 2.0 SP-tabanlı Lite kimliğinizi sağlayıcısı ile kurduktan sonra düzgün çalıştığını doğrulamanız gerekir.

>[!NOTE]
>Eklenirken bir yerine bir etki alanı dönüştürülürse, çoklu oturum açmayı kurduğunuzda 24 saate kadar sürebilir.
Çoklu oturum açmayı doğrulamak için önce Active Directory eşitleme kurulumunun tamamlanması, dizinlerinizi eşitleyin ve eşitlenmiş kullanıcıları etkinleştirme gerekir.

### <a name="use-the-tool-to-verify-that-single-sign-on-has-been-set-up-correctly"></a>Bu çoklu oturum açma doğru olarak ayarlanmış olan doğrulamak için Aracı'nı kullanın
Bu çoklu oturum açma doğru olarak ayarlanmış olan doğrulamak için bulut hizmeti şirket kimlik bilgilerinizle oturum açmak sorunsuz yaptığınızı doğrulamak için aşağıdaki yordamı gerçekleştirebilirsiniz.

Microsoft, SAML 2.0 tabanlı kimlik sağlayıcınızı test etmek için kullanabileceğiniz bir aracı sağlamıştır. Test aracı çalıştırmadan önce kimlik sağlayıcınız ile birleştirmek için Azure AD kiracısı yapılandırmış olmanız gerekir.

>[!NOTE]
>Bağlantı Çözümleyicisi'ni Internet Explorer 10 veya üstünü gerektirir.



1. Bağlantı Çözümleyicisi'ni indirin [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Başlamak için Şimdi Yükle'yi tıklatın indiriliyor ve aracı yükleniyor.
3.  "I Office 365, Azure veya Azure Active Directory kullanan diğer hizmetler ile Federasyon ayarlayamazsınız" seçin.
4.  Sonra aracı yüklenir ve çalışan, bağlantı tanılama penceresinde görürsünüz. Aracı Federasyon bağlantınızı test aracılığıyla adım.
5.  Bağlantı Çözümleyicisi'ni, oturum açma, test ettiğiniz kullanıcı asıl adı için kimlik bilgilerini girmek, SAML 2.0 IDP açılır: ![SAML](media/active-directory-aadconnect-federation-saml-idp/saml1.png)
6.  Federasyon test oturum açma penceresine SAML 2.0 kimlik sağlayıcınız ile birleştirilecek yapılandırılmış Azure AD kiracısı için bir hesap adı ve parola girmelisiniz. Aracı bu kimlik bilgilerini kullanarak oturum açın dener ve oturum açma denemesi sırasında gerçekleştirilen testleri ayrıntılı sonuçlarını çıkış olarak sağlanacaktır.
![SAML](media/active-directory-aadconnect-federation-saml-idp/saml2.png)
7. Bu pencere sınama başarısız sonucunu gösterir. Ayrıntılı sonuçları gerçekleştirilen her testi sonuçlarıyla ilgili bilgileri gösterir İnceleme tıklayarak. Onları paylaşmak için diske sonuçları kaydedebilirsiniz.
 
>[!NOTE]
>Bağlantı Çözümleyicisi'ni kullanarak WS * Active Federasyon da test-tabanlı ve ECP/PAOS protokoller. Bunlar kullanmıyorsanız şu hatayı göz ardı edebilirsiniz: kullanan kimlik sağlayıcısının etkin Federasyon uç noktasını etkin oturum açma akışını test etme.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Bu çoklu oturum açma doğru olarak ayarlanmış olan el ile doğrulayın
El ile doğrulama SAML 2.0 kimlik sağlayıcısı birçok senaryoda düzgün çalıştığından emin olmak için yapabileceğiniz ek adımları sağlar.
Bu çoklu oturum açma doğru olarak ayarlanmış olan doğrulamak için aşağıdaki adımları tamamlayın:


1. Bulut hizmetiniz için şirket kimlik bilgilerinizi kullanın aynı oturum açma adını kullanarak etki alanına katılmış bir bilgisayarda oturum açın.
2.  Parola kutusunun içine tıklayın. Çoklu oturum açma ayarlandığından, parola kutusu gölgeli olur ve aşağıdaki iletiyi görürsünüz: "sırasında oturum açmak için artık gerekli <your company>."
3.  Oturum açma tıklatın <your company> bağlantı. Oturum açabilir, ardından çoklu oturum açma ayarlanmış olması.

## <a name="next-steps"></a>Sonraki Adımlar


- [Active Directory Federasyon Hizmetleri Yönetimi ve Azure AD Connect ile özelleştirme](active-directory-aadconnect-federation-management.md)
- [Azure AD federasyonu uyumluluk listesi](active-directory-aadconnect-federation-compatibility.md)
- [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md)
