---
title: Sertifika tabanlı kimlik doğrulaması ile - Azure Active Directory kullanmaya başlayın
description: Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırma hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: f57d4615fc80df6c5df9ba295288ad71ae12fa23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60359084"
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Azure Active Directory'de sertifika tabanlı kimlik doğrulamasını kullanmaya başlama

Sertifika tabanlı kimlik doğrulaması Azure Active Directory tarafından Windows, Android veya iOS aygıtında bir istemci sertifikası ile Exchange online hesabınıza bağlanırken kimlik doğrulaması yapmanızı sağlar:

- Microsoft Outlook ve Microsoft Word gibi Microsoft mobil uygulamaları
- Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandıran bir kullanıcı adı ve parola birleşimini belirli e-posta ve Microsoft Office uygulamaları ile mobil Cihazınızda girilmesi gereğini ortadan kaldırır.

Bu konuda:

- Sertifika tabanlı kimlik doğrulaması kullanıcılar için kiracının Office 365 Kurumsal, iş, eğitim ve ABD kamu planlarında yazılımınız olması ve yapılandırma adımları sağlar. Bu özellik, Office 365 Çin, ABD kamu savunma ve ABD kamu Federal planlarında önizlemede kullanılabilir.
- Zaten sahip olduğunuzu varsayar bir [ortak anahtar altyapısı (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) ve [AD FS](../hybrid/how-to-connect-fed-whatis.md) yapılandırılmış.

## <a name="requirements"></a>Gereksinimler

Sertifika tabanlı kimlik doğrulaması yapılandırmak için aşağıdaki deyimleri doğru olması gerekir:

- Sertifika tabanlı kimlik doğrulaması (CBA), yalnızca tarayıcı uygulamalarında veya modern kimlik doğrulaması (ADAL) kullanan yerel istemciler için federe ortamlar için desteklenir. Exchange Active Sync (EAS) için Exchange Online (Federasyon ve yönetilen hesapları için kullanılabilecek EXO), bir işlemdir.
- Kök sertifika yetkilisini ve ara sertifika yetkilileri herhangi bir Azure Active Directory'de yapılandırılması gerekir.
- Her sertifika yetkilisi, bir internet'e yönelik URL'si ile başvurulan bir sertifika iptal listesini (CRL) olmalıdır.
- Azure Active Directory içinde yapılandırılan en az bir sertifika yetkilisi olması gerekir. İlgili adımları bulabilirsiniz [sertifika yetkililerini Yapılandır](#step-2-configure-the-certificate-authorities) bölümü.
- Exchange ActiveSync istemcileri için istemci sertifikası kullanıcının yönlendirilebilir e-posta adresi Exchange online asıl adı veya konu alternatif adı alanında RFC822 adı değerini olmalıdır. Azure Active Directory dizin Proxy adresi özniteliğine RFC822 değeri eşler.
- İstemci Cihazınızı, istemci sertifikaları veren en az bir sertifika yetkilisi erişiminiz olması gerekir.
- İstemci kimlik doğrulaması için bir istemci sertifikası istemciniz için verilmiş olmalıdır.

## <a name="step-1-select-your-device-platform"></a>1\. adım: Cihaz platformunuzu seçin

Önem verdiğiniz cihaz platformu için bir ilk adım olarak aşağıdaki inceleyin yapmanız gerekir:

- Office mobil uygulamaları desteği
- Belirli uygulama gereksinimleri

İlgili bilgiler için aşağıdaki cihaz platformlarını bulunmaktadır:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)

## <a name="step-2-configure-the-certificate-authorities"></a>2\. adım: Sertifika yetkililerini Yapılandır

Sertifikanızı yapılandırmak için Azure Active Directory'de yetkilileri her sertifika yetkilisi için aşağıdakileri yükleyin:

* Sertifika ortak kısmını, *.cer* biçimi
* Sertifika iptal listelerini (CRL'ler) bulunduğu internet'e yönelik URL'ler

Bir sertifika yetkilisi için şema şu şekilde görünür:

    class TrustedCAsForPasswordlessAuth
    {
       CertificateAuthorityInformation[] certificateAuthorities;
    }

    class CertificateAuthorityInformation

    {
        CertAuthorityType authorityType;
        X509Certificate trustedCertificate;
        string crlDistributionPoint;
        string deltaCrlDistributionPoint;
        string trustedIssuer;
        string trustedIssuerSKI;
    }

    enum CertAuthorityType
    {
        RootAuthority = 0,
        IntermediateAuthority = 1
    }

Yapılandırma için kullandığınız [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0):

1. Windows PowerShell'i yönetici ayrıcalıklarıyla başlatın.
2. Azure AD modülü sürümü yükleme [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) veya üzeri.

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

İlk yapılandırma adımı olarak, kiracınız ile bir bağlantı oluşturmanız gerekir. Kiracınız için bir bağlantı var. hemen sonra inceleme, ekleme, silme ve dizininizde tanımlanan güvenilen sertifika yetkilileri değiştirin.

### <a name="connect"></a>Bağlan

Kiracınız ile bağlantı kurmak için kullanmanın [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:

    Connect-AzureAD

### <a name="retrieve"></a>Alma

Dizininizde tanımlanan güvenilen sertifika yetkilileri almak için kullanın [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'i.

    Get-AzureADTrustedCertificateAuthority

### <a name="add"></a>Ekle

Güvenilen bir sertifika yetkilisi oluşturmak için kullanın [yeni AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'i ve **crlDistributionPoint** doğru bir değer özniteliği:

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint="<CRL Distribution URL>"
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca

### <a name="remove"></a>Kaldır

Güvenilen bir sertifika yetkilisi kaldırmak için [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]

### <a name="modify"></a>Değiştir

Güvenilen bir sertifika yetkilisi değiştirmek için [kümesi AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]

## <a name="step-3-configure-revocation"></a>3\. adım: İptal yapılandırın

Bir istemci sertifikasını iptal etmek için Azure Active Directory Sertifika iptal listesini (CRL) sertifika yetkilisi bilgileri bir parçası olarak yüklenen URL'lerden getirir ve onu önbelleğe alır. Son zaman damgası yayımlama (**geçerlilik tarihi** özelliği) CRL'ye CRL hala geçerli olduğundan emin olmak için kullanılır. CRL listesinin bir parçası olan sertifikaları erişimini iptal etmek için düzenli olarak başvuruluyor.

(Örneğin bir kullanıcı cihazını kaybederse) daha hızlı iptali gerekiyorsa, kullanıcının yetkilendirme belirteci geçersiz kılınabilir. Yetkilendirme belirteci geçersiz kılmak üzere ayarla **StsRefreshTokenValidFrom** Windows PowerShell kullanarak belirli bir kullanıcı için alan. Güncelleştirmeniz gerekir **StsRefreshTokenValidFrom** alanındaki her kullanıcı erişimini iptal etmek istiyor.

İptal devam ederse, ayarlamalısınız emin olmak için **geçerlilik tarihi** bir tarihten sonra ayarlanan değer CRL'nin **StsRefreshTokenValidFrom** ve söz konusu sertifikanın CRL'de olduğundan emin olun.

Aşağıdaki adımlar, güncelleştiriliyor ve yetkilendirme belirteci geçersiz kılmalarını ayarlayarak sürecini özetlemektedir **StsRefreshTokenValidFrom** alan.

**İptal yapılandırmak için:**

1. Yönetici kimlik MSOL hizmetine bağlanın:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. Bir kullanıcı için geçerli StsRefreshTokensValidFrom değeri alın:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Kullanıcı eşit geçerli zaman damgası için yeni bir StsRefreshTokensValidFrom değeri yapılandırın:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Ayarladığınız tarihin gelecekte olması gerekir. Tarih gelecekte değilse **StsRefreshTokensValidFrom** özelliği ayarlı değil. Tarihin gelecekte olması halinde **StsRefreshTokensValidFrom** geçerli saatten (Set-MsolUser komutu tarafından belirtilen tarihi değil) ayarlayın.

## <a name="step-4-test-your-configuration"></a>4\. Adım: Yapılandırmanızı test etme

### <a name="testing-your-certificate"></a>Sertifikanızı test etme

İlk yapılandırması test olarak oturum açmak denemelisiniz [Outlook Web Access](https://outlook.office365.com) veya [SharePoint Online](https://microsoft.sharepoint.com) kullanarak, **cihazda tarayıcı**.

Oturum açma işleminiz başarılı olursa, daha sonra bildiğiniz:

- Kullanıcı sertifikası test cihazınıza sağlanmış
- AD FS düzgün yapılandırılıp yapılandırılmadığını

### <a name="testing-office-mobile-applications"></a>Office mobil uygulamalarını test etme

**Sertifika tabanlı kimlik doğrulaması mobil Office uygulamanızda test etmek için:**

1. Test Cihazınızda Office mobil uygulaması (örneğin OneDrive) yükleyin.
3. Uygulamayı başlatın.
4. Kullanıcı adınızı girin ve ardından kullanmak istediğiniz sertifikayı seçin.

Başarıyla oturum açmanız.

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync istemci uygulamalarını test etme

Sertifika tabanlı kimlik doğrulaması Exchange ActiveSync (EAS) erişmek için istemci sertifikasını içeren bir EAS profili bir uygulama için kullanılabilir olmalıdır.

EAS profili, aşağıdaki bilgileri içermelidir:

- Kimlik doğrulaması için kullanılacak kullanıcı sertifikası

- EAS uç noktası (örneğin, outlook.office365.com)

EAS profili yapılandırılan ve mobil cihaz Yönetimi (MDM) gibi Intune ya da el ile EAS profili cihazda sertifika verme kullanımı üzerinden cihazın yerleştirilir.

### <a name="testing-eas-client-applications-on-android"></a>EAS istemci uygulamaları android'de test etme

**Sertifika kimlik doğrulamasını test etmek için:**

1. Önceki bölümdeki gereksinimlerini karşılayan bir uygulamada bir EAS profili yapılandırın.
2. Uygulamayı açmak ve posta eşitleme olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

[Android cihazlara sertifika tabanlı kimlik doğrulaması hakkında ek bilgi almak için.](active-directory-certificate-based-authentication-android.md)

[İOS cihazları sertifika tabanlı kimlik doğrulaması hakkında ek bilgi almak için.](active-directory-certificate-based-authentication-ios.md)
