---
title: "Sertifika yenileme Office 365 ve Azure AD kullanıcıları için | Microsoft Docs"
description: "Bu makalede, Office 365 kullanıcıları için bir sertifika yenileme hakkında bildirim e-postalar ile ilgili sorunları gidermek nasıl açıklanmaktadır."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: billmath
ms.openlocfilehash: 675f5b31eb60a75e060a397f01777e427c068c64
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365 ve Azure Active Directory Federasyon sertifikalarını yenileme
## <a name="overview"></a>Genel Bakış
Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) arasındaki başarılı Federasyon için Azure AD içinde yapılandırılmış Azure AD'ye güvenlik belirteçleri imzalamak için AD FS tarafından kullanılan sertifikalar ile eşleşmesi gerekir. Öğelerdeki uyuşmazlık kaldırılmış güven yol açabilir. Azure AD, AD FS ve Web uygulama proxy'si (extranet erişimi için) dağıtırken bu bilgileri eşitlenmiş olarak saklanmasını sağlar.

Bu makalede, belirteç imzalama sertifikalarını yönetmek ve aşağıdaki durumlarda bunları Azure AD ile eşitlenmiş tutmak için ek bilgiler sağlanmaktadır:

* Web uygulaması Ara sunucusu dağıtma değil ve bu nedenle Federasyon meta verilerinin extranet kullanılabilir değildir.
* AD FS varsayılan yapılandırması için belirteç imzalama sertifikaları kullanarak değil.
* Bir üçüncü taraf kimlik sağlayıcısı kullanıyor.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>AD FS belirteç imzalama sertifikaları için varsayılan yapılandırma
Belirteç imzalama ve belirteç şifre çözme sertifikaları genellikle otomatik olarak imzalanan sertifikalar ve bir yıl için iyidir. Varsayılan olarak, AD FS adlı bir otomatik yenileme işlemi içerir **AutoCertificateRollover**. Otomatik olarak AD FS 2.0 veya üstü, Office 365 ve Azure AD kullanıyorsanız, sertifikanızın süresi dolmadan önce güncelleştirin.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Office 365 portalı ya da bir e-posta bildirim yenileme
> [!NOTE]
> Bir e-posta veya Office sertifikanızı yenilemek için bkz: isteyen bir portal bildirim alınırsa, [belirteç imzalama sertifikaları yapılan değişiklikleri yönetme](#managecerts) herhangi bir eylemde bulunmanız gerektiğinde denetlemek için. Microsoft, hiçbir eylem gerekli olduğunda bile, gönderilen sertifika yenileme için bildirimleri yol açabilir olası bir sorunu bilmektedir.
>
>

Federasyon meta verilerini izlemek ve bu meta verileri tarafından belirtildiği şekilde imzalama sertifikalarının belirtecinizi güncelleştirmek Azure AD çalışır. 30 gün önce sona erme tarihini belirteç imzalama sertifikalarını, Azure AD Federasyon meta verileri yoklayarak yeni sertifikalar kullanılabilir olup olmadığını denetler.

* Bu başarılı şekilde Federasyon meta verileri sorgulamak ve yeni sertifika almak, kullanıcıya e-posta bildirim veya Office 365 portalında uyarı verilir.
* Yeni belirteç imzalama sertifikaları alamıyorsa, ya da Federasyon meta verilerinin ulaşılabilir olmadığından veya otomatik sertifika aktarma etkin değil, Azure AD bir e-posta bildirimi ve Office 365 portalında bir uyarı verir.

![Office 365 portalı bildirim](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> İş sürekliliğini sağlamak üzere AD FS kullanıyorsanız, böylece bilinen sorunlara yönelik kimlik doğrulama hatalarının meydana gelmediğinden, sunucularınızı aşağıdaki güncelleştirmeleri olduğunu doğrulayın. Bu yenileme ve gelecekteki yenileme dönemleri için bilinen AD FS proxy sunucu sorunları azaltır:
>
> Server 2012 R2 - [Windows Server Mayıs 2014 paketi](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 ve 2012 - [proxy üzerinden kimlik doğrulaması başarısız Windows Server 2012 veya Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Sertifikaları güncelleştirilmesi gerekip gerekmediğini denetleyin<a name="managecerts"></a>
### <a name="step-1-check-the-autocertificaterollover-state"></a>1. adım: AutoCertificateRollover durumunu denetle
AD FS sunucunuzda, PowerShell'i açın. AutoCertificateRollover değeri True olarak ayarlanmış olduğundan emin olun.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>AD FS 2.0 kullanıyorsanız, önce Ekle-Pssnapın Microsoft.Adfs.Powershell çalıştırın.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>2. adım: AD FS ile Azure AD eşitleme olduğundan emin olun
AD FS sunucunuzun Azure AD PowerShell komut istemini açın ve Azure AD connect.

> [!NOTE]
> Azure AD PowerShell indirebilirsiniz [burada](https://technet.microsoft.com/library/jj151815.aspx).
>
>

    Connect-MsolService

AD FS ile Azure AD güven özellikleri belirtilen etki alanı için yapılandırılan sertifikaları denetleyin.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Her iki çıkışları parmak eşleşiyorsa sertifikalarınızı Azure AD ile eşitlenmiş.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>3. adım: sertifikanızı dolmak üzere olup olmadığını denetleyin
Get-MsolFederationProperty veya Get-AdfsCertificate çıktısında "Değil sonra" altında tarihi denetleyin Tarih 30 günden az hemen ise eylemi gerçekleştirmelisiniz.

| AutoCertificateRollover | Sertifikaları Azure AD ile eşitleme | Federasyon meta verileri genel olarak erişilebilir | Geçerlilik | Eylem |
|:---:|:---:|:---:|:---:|:---:|
| Evet |Evet |Evet |- |Eyleme gerek yok. Bkz: [yenileme belirteç imzalama sertifikası otomatik olarak](#autorenew). |
| Evet |Hayır |- |15 günden az |Hemen yenileyin. Bkz: [el ile yenileme belirteç imzalama sertifikası](#manualrenew). |
| Hayır |- |- |30 günden az |Hemen yenileyin. Bkz: [el ile yenileme belirteç imzalama sertifikası](#manualrenew). |

\[-] Önemli değildir

## Belirteç imzalama sertifikasının otomatik yenileme (önerilir)<a name="autorenew"></a>
Aşağıdakilerin her ikisi de doğruysa herhangi manuel adımları uygulamanız gerekmez:

* Erişim için Federasyon meta verilerinin extranet etkinleştirebilirsiniz Web uygulaması proxy'si dağıtmış.
* AD FS varsayılan yapılandırması (AutoCertificateRollover etkin) kullanıyor.

Sertifika otomatik olarak güncelleştirilebilir onaylamak için şunları denetleyin.

**1. AutoCertificateRollover AD FS özelliği True olarak ayarlanması gerekir.** Bu AD FS yeni belirteç imzalama ve belirteç şifre çözme sertifikaları otomatik olarak oluşturur, önce eski olanları sona gösterir.

**2. AD FS federasyon meta verileri genel olarak erişilebilir.** (Dışına şirket ağına) genel internet üzerindeki bir bilgisayardan aşağıdaki URL'sine giderek Federasyon meta verileri genel olarak erişilebilir olduğundan emin olun:

https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

Burada `(your_FS_name) `kuruluşunuzun kullandığı, fs.contoso.com gibi Federasyon Hizmeti ana bilgisayar adı ile değiştirilir.  Her ikisi de doğrulayamazsınız varsa bu ayarları başarıyla, başka bir şey yapmanız gerekmez.  

Örnek: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Belirteç imzalama sertifikasının el ile yenileme<a name="manualrenew"></a>
Belirteç imzalama sertifikalarının el ile yenilemek tercih edebilirsiniz. Örneğin, aşağıdaki senaryolar için el ile yenileme daha iyi çalışabilir:

* Belirteç imzalama sertifikalarının otomatik olarak imzalanan sertifikalar. Bunun en yaygın nedeni, kuruluşunuzun AD FS sertifikalarının bir kuruluş sertifika yetkilisinden kayıtlı yönetir ' dir.
* Ağ güvenliği genel olarak kullanılabilir olması Federasyon meta verilerinin izin vermiyor.

Belirteç imzalama sertifikalarını, güncelleştirdiğiniz her sefer bu senaryolarda da Office 365 etki alanınızın güncelleştirme MsolFederatedDomain PowerShell komutunu kullanarak güncelleştirmeniz gerekir.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>1. adım: AD FS yeni bir belirteç imzalama sertifikalarının sahip olduğundan emin olun.
**Varsayılan olmayan yapılandırma**

Varsayılan olmayan yapılandırma AD FS kullanıyorsanız, (burada **AutoCertificateRollover** ayarlanır **False**), büyük olasılıkla özel sertifikaları (kendinden imzalı) kullanıyor. AD FS belirteç imzalama sertifikalarını yenileme hakkında daha fazla bilgi için bkz: [yönergeler AD FS kullanmayan müşterileri için otomatik olarak imzalanan sertifikalar](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federasyon meta veri genel kullanıma açık değil**

Öte yandan, **AutoCertificateRollover** ayarlanır **doğru**, ancak, Federasyon meta verileri genel olarak erişilebilir değil, öncelikle yeni belirteç imzalama sertifikaları AD FS tarafından oluşturulan olduğundan emin olun. Yeni bir belirteç imzalama sertifikalarının aşağıdaki adımları uygulayarak sahip onaylayın:

1. Birincil AD FS sunucusunda oturum açan doğrulayın.
2. Bir PowerShell komut penceresi açıp aşağıdaki komutu çalıştırarak AD FS geçerli İmzalama sertifikaları denetleyin:

    PS C:\>Get-ADFSCertificate – Encryption belirteç imzalama

   > [!NOTE]
   > AD FS 2.0 kullanıyorsanız, Add-Pssnapın Microsoft.Adfs.Powershell ilk çalıştırmanız gerekir.
   >
   >
3. Listelenen herhangi bir sertifika komutu çıktıyı bakın. AD FS yeni bir sertifika oluşturmuşsa, iki sertifika çıktı görmeniz gerekir: biri için hangi **IsPrimary** değer **True** ve **NotAfter** tarihidir 5 gün ve biri için hangi içinde **IsPrimary** olan **False** ve **NotAfter** gelecekte bir yıl hakkında.
4. Yalnızca bir sertifika görüyorsanız ve **NotAfter** tarihi 5 gün içinde yeni bir sertifika oluşturmak gerekir.
5. Yeni bir sertifika oluşturmak için bir PowerShell komut isteminde aşağıdaki komutu yürütün: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Aşağıdaki komutu yeniden çalıştırarak güncelleştirme doğrulayın: PS C:\>Get-ADFSCertificate – Encryption belirteç imzalama

İki sertifika artık listelenir, bunlardan biri olan bir **NotAfter** tarih yaklaşık bir yıl sonra ve hangi **IsPrimary** değer **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>2. adım: yeni belirteç imzalama sertifikalarının Office 365 güven güncelleştirmek
Office 365 imzalama sertifikalarının gibi güven için kullanılacak olan yeni belirteci ile güncelleştirin.

1. Microsoft Azure Active Directory modülü için Windows PowerShell'i açın.
2. $Cred Çalıştır Get-Credential =. Bu cmdlet, kimlik bilgilerini ister, bulut hizmet yönetici hesabı kimlik bilgilerini yazın.
3. Connect MsolService Çalıştır – kimlik bilgisi $cred. Bu cmdlet bulut hizmetine bağlanır. Bulut hizmetine bağlanan bir içeriği oluşturma aracı tarafından yüklü ek cmdlet'lerinden herhangi birini çalıştırmadan önce gereklidir.
4. Bu komutlar, AD FS birincil Federasyon sunucusu olmayan bir bilgisayar üzerinde çalıştırıyorsanız, Set-MSOLAdfscontext Çalıştır-bilgisayar <AD FS primary server>, burada <AD FS primary server> iç FQDN birincil AD FS sunucusunun adıdır. Bu cmdlet için AD FS bağlanan bir içeriği oluşturur.
5. Güncelleştirme MSOLFederatedDomain – DomainName çalıştırmak <domain>. Bu cmdlet, AD FS ayarları bulut hizmete güncelleştirir ve ikisi arasında güven ilişkisi yapılandırır.

> [!NOTE]
> Contoso.com ve fabrikam.com, gibi birden çok üst düzey etki alanlarının desteklemeniz gerekiyorsa, kullanmalısınız **SupportMultipleDomain** geçiş herhangi cmdlet'lerle. Daha fazla bilgi için bkz: [birden çok üst düzey etki alanı için destek](active-directory-aadconnect-multiple-domains.md).
>


## Azure AD Connect kullanarak Azure AD güvenini Onar<a name="connectrenew"></a>
Azure AD Connect kullanarak AD FS grubu ve Azure AD güveni yapılandırdıysanız, belirteç imzalama sertifikaları için herhangi bir eylemde bulunmanız gerektiğinde algılamak için Azure AD Connect kullanabilirsiniz. Sertifikaları yenilemek gerekiyorsa, bunu yapmak için Azure AD Connect kullanabilirsiniz.

Daha fazla bilgi için bkz: [güven onarma](active-directory-aadconnect-federation-management.md).

## <a name="ad-fs-and-azure-ad-certificate-update-steps"></a>AD FS ile Azure AD güncelleştirme adımları sertifika
Standart X509 belirteç imzalama sertifikalarının federasyon sunucusunun verdiği tüm belirteçleri güvenle imzalamak için kullanılan sertifikaları. Belirteç şifre çözme sertifikaları olan standart X509 gelen belirteçlerin şifresini çözmek için kullanılan sertifikaları. 

Varsayılan olarak, AD FS belirteç imzalama ve belirteç şifre çözme sertifikaları başlangıç yapılandırmasını zaman hem sertifikaları kendi sona erme tarihi yaklaştığı olduğunda otomatik olarak oluşturmak için yapılandırılır.

Yeni bir sertifika 30 gün önce geçerli sertifika süre sonu, Federasyon Hizmeti meta verilerini almak Azure AD çalışır. Yeni bir sertifika o anda kullanılamaz durumda, Azure AD meta verilerin günlük düzenli aralıklarla izlemek devam eder. Yeni sertifika meta verilerde kullanılabilir etki alanı için Federasyon ayarlarını yeni sertifika bilgilerle güncellenir. Kullanabileceğiniz `Get-MsolDomainFederationSettings` NextSigningCertificate yeni sertifikayı görüyorsanız, doğrulamak için / SigningCertificate.

Belirteç imzalama hakkında daha fazla bilgi için bkz: AD FS'de Sertifikalar [elde edilir ve yapılandırma belirteç imzalama ve belirteç şifre çözme sertifikaları AD FS için](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ts-td-certs-ad-fs)