---
title: Sertifika yenileme için Office 365 ve Azure AD kullanıcıları | Microsoft Docs
description: Bu makalede, Office 365 kullanıcıları için sertifika yenileme hakkında bildiren bir e-postaları ile ilgili sorunları gidermek nasıl açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d98a1aabef2de505e66b2127226b9e89cd791e20
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60244813"
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365 ve Azure Active Directory için Federasyon sertifikalarını yenileme
## <a name="overview"></a>Genel Bakış
Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) arasında başarılı Federasyon için Azure AD'de yapılandırılmış Azure AD'ye güvenlik belirteçleri imzalamak için AD FS tarafından kullanılan sertifikaları eşleşmesi gerekir. Herhangi bir uyuşmazlık güvenin Bozulması neden olabilir. Azure AD, AD FS ve Web uygulaması Ara sunucusu (extranet erişimi için) dağıtırken bu bilgileri eşitlenmiş tutulduğundan emin sağlar.

Bu makalede, belirteç imzalama sertifikalarını yönetmek ve aşağıdaki durumlarda Azure AD ile eşitlenmiş halde tutun için ek bilgiler sağlanmaktadır:

* Web uygulaması Ara sunucusu dağıtıyorsanız değil ve bu nedenle Federasyon meta verilerine extranet kullanılabilir değil.
* Varsayılan AD FS yapılandırması için belirteç imzalama sertifikaları kullanarak değil.
* Bir üçüncü taraf kimlik sağlayıcısı kullanıyor.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>AD FS belirteç imzalama sertifikaları için varsayılan yapılandırma
Belirteç imzalama ve belirteç şifre çözme sertifikaları genellikle otomatik olarak imzalanan sertifikaları ve bir yıl için uygundur. Varsayılan olarak, AD FS adlı bir otomatik yenileme işlemi içerir. **AutoCertificateRollover**. Otomatik olarak AD FS 2.0 veya sonraki bir sürümü, Office 365 ve Azure AD kullanıyorsanız, sertifikanın süresi dolmadan önce güncelleştirin.

### <a name="renewal-notification-from-the-microsoft-365-admin-center-or-an-email"></a>Microsoft 365 Yönetici merkezinden veya bir e-posta yenileme bildirimi
> [!NOTE]
> E-posta veya Office için sertifikanızı yenilemek için bkz: isteyen bir portal bildirimi aldıysanız [belirteç imzalama sertifikaları yapılan değişiklikleri yönetme](#managecerts) herhangi bir eylemde bulunmanız gerekiyorsa denetlemek için. Microsoft sertifika yenileme herhangi bir işlem gerekli olsa bile, gönderilen bildirimler için yol açabilecek olası bir sorunu farkındadır.
>
>

Azure AD Federasyon meta verilerini izleme ve belirteç imzalama sertifikaları bu meta veriler tarafından belirtildiği şekilde güncelleştirmek çalışır. 30 gün önce sona erme tarihini belirteç imzalama sertifikaları, Azure AD Federasyon meta verileri yoklayarak yeni sertifikalar kullanılabilir olup olmadığını denetler.

* Başarıyla Federasyon meta verileri yoklamak ve yeni sertifikalar almak, kullanıcıya e-posta bildirimi ya da Microsoft 365 Yönetim merkezinde uyarı verilir.
* Yeni belirteç imzalama sertifikaları alınamıyor, ya da Federasyon meta verilerine erişilemiyor veya otomatik sertifika aktarma etkin değil, çünkü Azure AD bir e-posta bildirimi ve Microsoft 365 Yönetim merkezinde bir uyarı verir.

![Office 365 portal bildirimi](./media/how-to-connect-fed-o365-certs/notification.png)

> [!IMPORTANT]
> İş sürekliliğinin sağlanması için AD FS kullanıyorsanız, lütfen bilinen sorunlara yönelik kimlik doğrulama hatalarının meydana gelmediğinden, sunucularınızı aşağıdaki güncelleştirmeleri sahip olduğunuzu doğrulayın. Bu, bu yenileme ve gelecekteki yenileme dönemleri için bilinen AD FS proxy sunucusu sorunlarını hafifletir:
>
> Server 2012 R2 - [Windows Server Mayıs 2014 paketi](https://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 ve 2012 - [proxy üzerinden kimlik doğrulaması başarısız Windows Server 2012 veya Windows 2008 R2 SP1](https://support.microsoft.com/kb/3094446)
>
>

## Sertifikaları güncelleştirilmesi gerekip gerekmediğini denetleyin <a name="managecerts"></a>
### <a name="step-1-check-the-autocertificaterollover-state"></a>1. Adım: AutoCertificateRollover durumunu denetleyin
AD FS sunucunuza, PowerShell'i açın. AutoCertificateRollover değeri True olarak ayarlandığından emin olun.

    Get-Adfsproperties

![AutoCertificateRollover](./media/how-to-connect-fed-o365-certs/autocertrollover.png)

>[!NOTE] 
>AD FS 2.0 kullanıyorsanız, Add-Pssnapin Microsoft.Adfs.Powershell çalıştırın.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>2. Adım: AD FS ile Azure AD eşitleme olduğundan emin olun
AD FS sunucunuzun MSOnline PowerShell istemi açın ve Azure AD'ye bağlanma.

> [!NOTE]
> MSOL cmdlet'lerinden MSOnline PowerShell modülünde bir parçasıdır.
> MSOnline PowerShell modülünde doğrudan PowerShell Galerisi'nden indirebilirsiniz.
> 
>

    Install-Module MSOnline

MSOnline PowerShell modülünü kullanarak Azure AD'ye bağlanın.

    Import-Module MSOnline
    Connect-MsolService

AD FS ile Azure AD özellikleri için belirtilen etki alanı güven yapılandırılan sertifikaları kontrol edin.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/how-to-connect-fed-o365-certs/certsync.png)

Her iki çıkışları parmak izlerinin eşleşiyorsa sertifikalarınızı Azure AD ile eşitlenmiş halde değil.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>3. Adım: Sertifikanızın süresi dolmak üzere olup olmadığını denetleyin
Get-MsolFederationProperty veya Get-AdfsCertificate çıktısındaki tarih "Değil sonra" altında olup olmadığını denetleyin 30 günden az hemen tarihse eylemde bulunmanız gerekir.

| AutoCertificateRollover | Azure AD ile eşitlenmiş sertifikaları | Federasyon meta verileri genel olarak erişilebilir | Geçerlilik | Eylem |
|:---:|:---:|:---:|:---:|:---:|
| Evet |Evet |Evet |- |Eyleme gerek yok. Bkz: [yenileme belirteç imzalama sertifikası otomatik olarak](#autorenew). |
| Evet |Hayır |- |15 günden az |Hemen yenileyin. Bkz: [el ile yenileme belirteç imzalama sertifikası](#manualrenew). |
| Hayır |- |- |30 günden az |Hemen yenileyin. Bkz: [el ile yenileme belirteç imzalama sertifikası](#manualrenew). |

\[-] Önemli değildir

## Belirteç imzalama sertifikası otomatik olarak yenileme (önerilir) <a name="autorenew"></a>
Aşağıdakilerin her ikisi de doğruysa el ile herhangi bir adım gerçekleştirmeniz gerekmez:

* Erişim için Federasyon meta verilerine extranet kaynaklı etkinleştirebilirsiniz Web uygulama proxy'si dağıttım.
* AD FS varsayılan yapılandırması (AutoCertificateRollover etkin) kullanıyor.

Sertifika otomatik olarak güncelleştirilebilir onaylamak için aşağıdakini işaretleyin.

**1. AutoCertificateRollover AD FS özelliği True olarak ayarlanmalıdır.** Bu AD FS yeni belirteç imzalama ve belirteç şifre çözme sertifikaları otomatik olarak oluşturmak, önce eski olanları sona gösterir.

**2. AD FS federasyon meta verileri genel olarak erişilebilir.** Genel İnternet'e (dışına, kurumsal ağ) üzerindeki bir bilgisayardan aşağıdaki URL'ye giderek Federasyon meta verilerinizi genel olarak erişilebilir olduğundan emin olun:

https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

Burada `(your_FS_name)` kuruluşunuzun kullandığı, fs.contoso.com gibi Federasyon Hizmeti konak adı ile değiştirilir.  Her ikisi de doğrulayamadı varsa bu ayarları başarıyla, başka bir şey yapmanız gerekmez.  

Örnek: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml
## Belirteç imzalama sertifikasının el ile yenileme <a name="manualrenew"></a>
Belirteç imzalama sertifikalarının el ile yenilemek tercih edebilirsiniz. Örneğin, aşağıdaki senaryolar için el ile yenileme daha iyi çalışabilir:

* Kendinden imzalı sertifikaların belirteç imzalama sertifikaları. Bunun en yaygın nedeni, kuruluşunuzun AD FS sertifikalarının bir kuruluş sertifika yetkilisi tarafından kaydedilen yönetmesidir.
* Ağ güvenliği gelen kullanıma açık Federasyon meta verileri izin vermez.

Belirteç imzalama sertifikalarını her güncelleştirdiğinizde bu senaryolarda, ayrıca Office 365 etki alanınızı: Update-MsolFederatedDomain PowerShell komutunu kullanarak güncelleştirmeniz gerekir.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>1. Adım: AD FS yeni belirteç imzalama sertifikalarının olduğundan emin olun
**Varsayılan olmayan yapılandırma**

Bir varsayılan olmayan yapılandırma AD FS kullanıyorsanız (burada **AutoCertificateRollover** ayarlanır **False**), büyük olasılıkla özel sertifikaları (otomatik imzalı değil) kullanıyorsunuz. AD FS belirteç imzalama sertifikaları yenileme hakkında daha fazla bilgi için bkz. [yönergeler AD FS kullanarak olmayan müşteriler için otomatik imzalı sertifikalar](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federasyon meta verileri, genel kullanıma açık değil.**

Öte yandan, varsa **AutoCertificateRollover** ayarlanır **True**, ancak Federasyon meta verilerinizi genel olarak erişilebilir değil, ilk olarak AD FS tarafından oluşturulmuş olan yeni bir belirteç imzalama sertifikaları emin olun. Yeni belirteç imzalama sertifikaları aşağıdaki adımları izleyerek sahip olmadığını onaylayın:

1. Birincil AD FS sunucusunda oturum açan doğrulayın.
2. Bir PowerShell komut penceresi açıp ve aşağıdaki komutu çalıştırarak AD FS geçerli İmzalama sertifikaları denetleyin:

    PS C:\>Get-ADFSCertificate – CertificateType belirteç imzalama

   > [!NOTE]
   > AD FS 2.0 kullanıyorsanız, Add-Pssnapin Microsoft.Adfs.Powershell çalıştırmalısınız.
   >
   >
3. Komut çıktısı, listelenen herhangi bir sertifika bakın. AD FS yeni bir sertifika oluşturdu, iki sertifika çıktı görmeniz gerekir: biri olan **Isprimary** değer **True** ve **NotAfter** tarihtir 5 gün içinde , diğeri hangi **Isprimary** olduğu **False** ve **NotAfter** gelecekte bir yıl hakkında.
4. Yalnızca bir sertifika görüyorsanız ve **NotAfter** tarihi 5 gün içinde yeni bir sertifika oluşturmanız gerekiyor.
5. Yeni bir sertifika oluşturmak için bir PowerShell komut isteminde aşağıdaki komutu yürütün: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Güncelleştirme, aşağıdaki komutu çalıştırarak yeniden doğrulayın: PS C:\>Get-ADFSCertificate – CertificateType belirteç imzalama

İki sertifika listelenir artık, biri olan bir **NotAfter** tarih yaklaşık bir yıl sonra ve hangi **Isprimary** değer **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>2. Adım: Yeni belirteç imzalama sertifikaları için Office 365 güven güncelleştir
Office 365, yeni belirteç imzalama sertifikaları gibi güven için kullanılacak ile güncelleştirin.

1. Microsoft Azure Active Directory modülü için Windows PowerShell'i açın.
2. $Cred Çalıştır = Get-Credential. Bu cmdlet için kimlik bilgilerini sorduğunda, bulut hizmeti yönetici hesabı kimlik bilgilerini yazın.
3. Connect-MsolService çalıştırma – $cred kimlik bilgisi. Bu cmdlet bulut hizmetine bağlanır. Bulut hizmetine bağlanan bir bağlam oluşturma aracı tarafından yüklenen ek cmdlet'lerinden herhangi birini çalıştırmadan önce gereklidir.
4. Bu komutlar, AD FS birincil Federasyon sunucusu olmayan bir bilgisayar üzerinde çalıştırıyorsanız, Set-MSOLAdfscontext Çalıştır-bilgisayar &lt;AD FS birincil sunucusunu&gt;burada &lt;AD FS birincil sunucusunu&gt; iç FQDN: Birincil AD FS sunucusunun adı. Bu cmdlet, AD FS ile bağlanan bir bağlam oluşturur.
5. : Update-MSOLFederatedDomain-DomainName çalıştırma &lt;etki alanı&gt;. Bu cmdlet, bulut hizmeti AD fs'den ayarlarını güncelleştirir ve ikisi arasındaki güven ilişkisi yapılandırır.

> [!NOTE]
> Contoso.com ve fabrikam.com, gibi birden çok en üst düzey etki alanlarını destekleyecek şekilde gerekiyorsa kullanmalısınız **SupportMultipleDomain** herhangi bir cmdlet ile geçiş yapın. Daha fazla bilgi için [birden çok üst düzey etki alanları desteği](how-to-connect-install-multiple-domains.md).
>


## Azure AD Connect kullanarak Azure AD güvenini Onar <a name="connectrenew"></a>
Azure AD Connect kullanarak AD FS grubu ve Azure AD güvenini yapılandırdıysanız, belirteç imzalama sertifikaları için herhangi bir eylemde bulunmanız gerekiyorsa algılamak için Azure AD Connect kullanabilirsiniz. Sertifikaları yenilemek gerekiyorsa, bunu yapmak için Azure AD Connect kullanabilirsiniz.

Daha fazla bilgi için [güveni onarma](how-to-connect-fed-management.md).

## <a name="ad-fs-and-azure-ad-certificate-update-steps"></a>AD FS ile Azure AD güncelleştirme adımları sertifika
Standart X509 belirteç imzalama sertifikaları federasyon sunucusunun verdiği tüm belirteçleri güvenle imzalamak için kullanılan sertifika. Belirteç şifre çözme sertifikaları olan standart X509 gelen belirteçlerin şifresini çözmek için kullanılan sertifikaları. 

Varsayılan olarak, AD FS belirteç imzalama ve belirteç şifre çözme sertifikaları hem başlangıç yapılandırmasını zaman hem de sertifika, sona erme tarihi bölümüyle iletişime geçerken otomatik olarak oluşturmak için yapılandırılır.

Azure AD, Federasyon hizmet meta verilerinden 30 gün önce geçerli sertifikanın süre sonu yeni bir sertifika almaya çalışır. Yeni bir sertifika o anda kullanılabilir değil durumunda, Azure AD meta veriler günlük düzenli aralıklarla izlemek devam eder. Yeni sertifika meta verilerde mevcut etki alanında Federasyon ayarlarını yeni sertifika bilgileriyle güncellenir. Kullanabileceğiniz `Get-MsolDomainFederationSettings` NextSigningCertificate yeni sertifikayı görürseniz doğrulamak için / SigningCertificate.

Belirteç imzalama hakkında daha fazla bilgi için bkz: AD FS sertifikaları [elde edilir ve yapılandırma, belirteç imzalama ve AD FS belirteç şifre çözme sertifikaları](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ts-td-certs-ad-fs)
