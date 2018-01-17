---
title: "Azure Active Directory Sertifika tabanlı kimlik doğrulaması - kullanmaya başlama | Microsoft Docs"
description: "Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırma konusunda bilgi edinin"
author: MarkusVi
documentationcenter: na
manager: mtillman
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5c96f33b8f678155dc4b7a84718e5eadc541f441
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Azure Active Directory'de sertifika tabanlı kimlik doğrulaması kullanmaya başlama

Sertifika tabanlı kimlik doğrulaması, Azure Active Directory tarafından Windows, Android veya iOS aygıtında bir istemci sertifikası ile Exchange online hesabınızı bağlanırken kimliğinin doğrulanmasını sağlar:

- Microsoft Outlook ve Microsoft Word gibi Microsoft mobil uygulamaları   

- Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandıran bir kullanıcı adı ve parola birleşimini belirli mail ve Microsoft Office uygulamaları mobil cihazınızdan girin gereğini ortadan kaldırır.

Bu konuda:

- Yapılandırma ve Office 365 Kurumsal, iş, eğitim ve ABD devlet kurumları planlarda sertifika tabanlı kimlik doğrulaması kullanıcılar için kiracıların kullanma adımları sağlar. Bu özellik, Office 365 Çin'de, US Government savunma ve US Government Federal planlarda Önizleme'de kullanılabilir.

- Zaten sahip olduğunuz varsayılmaktadır bir [ortak anahtar altyapısı (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) ve [AD FS](connect/active-directory-aadconnectfed-whatis.md) yapılandırılmış.    


## <a name="requirements"></a>Gereksinimler

Sertifika tabanlı kimlik doğrulamasını yapılandırmak için aşağıdakilerin doğru olması gerekir:  

- Sertifika tabanlı kimlik doğrulaması (CBA), yalnızca tarayıcı uygulamalarında veya modern kimlik doğrulaması (ADAL) kullanarak yerel istemciler için federe ortamlar için desteklenir. Exchange ActiveSync (EAS), Federal hem yönetilen hesapları için kullanılabilecek EXO için bir istisnadır.

- Kök sertifika yetkilisini ve ara sertifika yetkilileri için Azure Active Directory'de yapılandırılması gerekir.  

- Her sertifika yetkilisi URL Internet'e yönelik başvurulan bir sertifika iptal listesini (CRL) olması gerekir.  

- Azure Active Directory içinde yapılandırılan en az bir sertifika yetkilisine sahip olmanız gerekir. İlgili adımları bulabilirsiniz [sertifika yetkililerini](#step-2-configure-the-certificate-authorities) bölümü.  

- Exchange ActiveSync istemcileri için istemci sertifikası kullanıcının yönlendirilebilir e-posta adresi Exchange çevrimiçi asıl adı veya konu alternatif adı alanında RFC822 adı değeri olması gerekir. Azure Active Directory dizin Proxy adresi özniteliğinde RFC822 değeri eşler.  

- İstemci Cihazınızı istemci sertifikaları veren en az bir sertifika yetkilisi erişiminiz olması gerekir.  

- İstemci kimlik doğrulaması için bir istemci sertifikası, istemciye verilmiş olması gerekir.  




## <a name="step-1-select-your-device-platform"></a>1. adım: cihaz platformunuz seçin

İlk adım olarak, verdiğiniz ilgili cihaz platformu için aşağıdakileri gözden geçirin gerekir:

- Office mobil uygulamaları desteği
- Belirli uygulama gereksinimleri  

İlgili bilgiler için aşağıdaki cihaz platformlarını bulunmaktadır:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-the-certificate-authorities"></a>2. adım: sertifika yetkililerini yapılandırma

Her sertifika yetkilisi için Azure Active Directory'de sertifika yetkililerini yapılandırmak için aşağıdaki karşıya yükle:

* Sertifikanın ortak kısmını, *.cer* biçimi
* URL'leri sertifika iptal listeleri (CRL'ler) bulunduğu Internet'e

Bir sertifika yetkilisi için şemayı şu şekilde görünür:

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

Bu yapılandırma için kullandığınız [Azure Active Directory PowerShell sürüm 2](/powershell/azure/install-adv2?view=azureadps-2.0):  

1. Windows PowerShell'i yönetici ayrıcalıklarıyla başlatın.
2. Azure AD modülünü yükleyin. Sürümünü yüklemek gereken [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) ya da daha yüksek.  

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

İlk Yapılandırma adım olarak, Kiracı ile bir bağlantı oluşturmanız gerekir. Kiracı bir bağlantı var. hemen gözden geçirin, ekleyebilir, silme ve dizininizde tanımlanan güvenilen sertifika yetkilileri değiştirin.

### <a name="connect"></a>Bağlan

Kiracı ile bağlantı kurmak için kullanmak [Connect-Azuread'i](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:

    Connect-AzureAD


### <a name="retrieve"></a>Alma

Dizininizde tanımlanan güvenilen sertifika yetkilileri almak kullanın [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'i.

    Get-AzureADTrustedCertificateAuthority


### <a name="add"></a>Ekle

Güvenilen bir sertifika yetkilisi oluşturmak için kullanın [yeni AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet'i ve **crlDistributionPoint** özniteliği için doğru bir değer:

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca


### <a name="remove"></a>Kaldır

Güvenilen bir sertifika yetkilisi kaldırmak için kullanın [Kaldır AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]


### <a name="modfiy"></a>Modfiy

Güvenilen bir sertifika yetkilisi değiştirmek için [kümesi AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]


## <a name="step-3-configure-revocation"></a>3. adım: iptal yapılandırma

Bir istemci sertifikası iptal etmek için Azure Active Directory Sertifika iptal listesini (CRL) sertifika yetkilisi bilgilerinin bir parçası yüklenen URL'lerden getirir ve önbelleğe alır. Son zaman damgası yayımlama (**geçerlilik tarihi** özelliği) CRL'de CRL hala geçerli olduğundan emin olmak için kullanılır. CRL listesinin bir parçası olan sertifikalar erişimi iptal etmek için düzenli olarak başvurulur.

(Örneğin bir kullanıcının bir cihazı kaybederse) daha hızlı iptali gerekiyorsa, kullanıcının yetkilendirme belirteci geçersiz hale. Yetkilendirme belirteci geçersiz kılmak için ayarlanmış **StsRefreshTokenValidFrom** Windows PowerShell kullanarak belirli bir kullanıcı için alan. Güncelleştirmeniz gerekir **StsRefreshTokenValidFrom** için erişimi iptal etmek istediğiniz her bir kullanıcı için alan.

İptal işleminin devam ederse, ayarlamalısınız emin olmak için **geçerlilik tarihi** bir tarihten sonra tarafından belirlenen değer CRL'nin **StsRefreshTokenValidFrom** ve söz konusu sertifikanın CRL'de olduğundan emin olun.

Güncelleştirme ve ayarlayarak yetkilendirme belirteci geçersiz hale getiriliyor işlemi aşağıdaki adımları anahat **StsRefreshTokenValidFrom** alan.

**İptal yapılandırmak için:**

1. Yönetici kimlik MSOL hizmetine bağlanın:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. Bir kullanıcı için geçerli StsRefreshTokensValidFrom değerini alın:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Geçerli zaman damgası kullanıcı eşit için yeni bir StsRefreshTokensValidFrom değeri yapılandırın:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Ayarladığınız tarih gelecekte olmalıdır. Tarihi gelecekte değilse **StsRefreshTokensValidFrom** özelliği ayarlı değil. Tarih gelecekte ise **StsRefreshTokensValidFrom** geçerli saati (kümesi MsolUser komutu tarafından gösterilen tarihi değil) olarak ayarlanmış.


## <a name="step-4-test-your-configuration"></a>4. adım: Test yapılandırmanızı

### <a name="testing-your-certificate"></a>Sertifikanızı test etme

İlk Yapılandırma sınaması oturum açmak denemelisiniz [Outlook Web Access](https://outlook.office365.com) veya [SharePoint Online](https://microsoft.sharepoint.com) kullanarak, **-cihazdır**.

Oturum açma işleminiz başarılı olursa, ardından bunu biliyor:

- Kullanıcı sertifikası test Cihazınızı sağlandı
- AD FS düzgün yapılandırılmış  


### <a name="testing-office-mobile-applications"></a>Office mobil uygulamalarını test etme

**Mobil Office uygulamanızda sertifika tabanlı kimlik doğrulamasını sınamak için:**

1. Test aygıtınızda Office mobil uygulaması (örneğin, OneDrive) yükleyin.
3. Uygulamayı başlatın.
4. Kullanıcı adınızı girin ve ardından kullanmak istediğiniz sertifikayı seçin.

Başarıyla oturum açmanız.

### <a name="testing-exchange-activesync-client-applications"></a>Exchange ActiveSync istemci uygulamalarını test etme

Sertifika tabanlı kimlik doğrulaması Exchange ActiveSync (EAS) erişmek için istemci sertifikasını içeren bir EAS profili uygulamaya kullanılabilir olmalıdır.

EAS profili aşağıdaki bilgileri içermelidir:

- Kimlik doğrulaması için kullanılacak kullanıcı sertifikası

- EAS uç noktası (örneğin, outlook.office365.com)

EAS profili yapılandırılan ve mobil cihaz Yönetimi (MDM) Intune gibi veya el ile cihazda EAS profildeki sertifika yerleştirerek kullanımını üzerinden cihazın yerleştirilir.  

### <a name="testing-eas-client-applications-on-android"></a>Android'de test EAS istemci uygulamaları

**Sertifika kimlik doğrulamasını sınamak için:**  

1. EAS profili yukarıdaki gereksinimleri karşılayan yapılandırın.  
2. Uygulamayı açmak ve posta eşitliyor doğrulayın.
