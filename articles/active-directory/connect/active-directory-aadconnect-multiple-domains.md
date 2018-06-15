---
title: Azure AD Connect birden çok etki alanı
description: Bu belgede ayarlama ve O365 ve Azure AD ile birden çok üst düzey etki alanlarını yapılandırma açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2f596f64041b3d429b99db482cd635ab441bf468
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801516"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Azure AD ile Federasyon için Çoklu Etki Alanı Desteği
Aşağıdaki belgeler Office 365 veya Azure AD etki alanları ile federasyonunu zaman birden çok üst düzey etki alanları ve alt etki alanlarını kullanma hakkında yönergeler sağlar.

## <a name="multiple-top-level-domain-support"></a>Birden çok üst düzey etki alanı desteği
Birden çok federasyonunu, üst düzey etki alanlarının Azure AD ile bir üst düzey etki alanı ile federasyonunu kullanılırken gerekli değildir bazı ek yapılandırma gerektirir.

Bir etki alanı Azure AD ile Federasyon olduğunda, çeşitli özellikleri etki alanı Azure ayarlanır.  Bir önemli IssuerUri adrestir.  Bu özellik, belirtecin ilişkili olduğu etki alanını tanımlamak için Azure AD tarafından kullanılan bir URI kullanılır.  URI, herhangi bir şey ancak geçerli bir URI olmalıdır çözümlemek gerekmez.  Varsayılan olarak, Azure AD URI Federasyon Hizmeti tanımlayıcısı değeri için şirket içi AD FS ayarlar yapılandırma.

> [!NOTE]
> Federasyon Hizmeti tanımlayıcısı bir Federasyon Hizmeti benzersiz olarak tanımlayan bir URI değil.  Federasyon Hizmeti AD FS bu işlevler güvenlik belirteci hizmeti örneğidir.
>
>

PowerShell komutunu kullanarak IssuerUri görüntüleyebilirsiniz `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Birden çok üst düzey etki alanı eklediğinizde, bir sorun ortaya çıkar.  Örneğin, Azure AD arasındaki Federasyon kurulu düşünelim ve şirket içi ortamınıza.  Bu belge için etki alanı bmcontoso.com kullanılıyor.  Şimdi bir ikinci, üst düzey etki alanı, bmfabrikam.com eklendi.

![Etki Alanları](./media/active-directory-multiple-domains/domains.png)

Birleştirilecek bmfabrikam.com etki alanı dönüştürmeye çalıştığınızda hata oluşur.  Bir nedeni, Azure AD'de birden fazla etki alanı için aynı değeri sağlamak IssuerUri özelliğe izin vermeyen bir kısıtlaması vardır.  

![Federasyon hata](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parametresi
Bu sınırlama olarak çözmek için kullanılarak yapılabilir farklı bir IssuerUri eklemeniz gerekir `-SupportMultipleDomain` parametresi.  Bu parametre, aşağıdaki cmdlet ile birlikte kullanılır:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Bu parametre, Azure AD etki alanı adını temel alarak IssuerUri yapılandırın yapar.  IssuerUri dizinlerde Azure AD içinde benzersiz olacaktır.  Parametresini kullanarak başarıyla tamamlamak için PowerShell komutunu sağlar.

![Federasyon hata](./media/active-directory-multiple-domains/convert.png)

Bmfabrikam.com etki alanı için ayarları, bakarak aşağıdakileri görebilirsiniz:

![Federasyon hata](./media/active-directory-multiple-domains/settings.png)

`-SupportMultipleDomain` adfs.bmcontoso.com Federasyon hizmetine işaret edecek şekilde yapılandırılmış olan diğer bütün uç değiştirmez.

Başka bir şey, `-SupportMultipleDomain` yapar, uygun veren değeriyle AD FS sistem için Azure AD yayınlanan belirteçleri içerir güvence altına alır. Bu değer, kullanıcıların UPN etki alanı kısmı alma ve etki alanında IssuerUri, yani https://{upn soneki olarak ayarlayarak ayarlanır} / adfs/services/güven.

Bu nedenle Azure ad kimlik doğrulaması sırasında veya kullanıcı belirteci IssuerUri öğesinde, Office 365, Azure AD etki alanını bulmak için kullanılır.  Bir eşleşme bulunamazsa, kimlik doğrulaması başarısız olur.

Örneğin, bir kullanıcının UPN ise bsimon@bmcontoso.com, AD FS sorunları, belirtecin IssuerUri öğe ayarlanacak http://bmcontoso.com/adfs/services/trust. Bu öğe Azure AD yapılandırma ile eşleşir ve kimlik doğrulama başarılı olur.

Bu mantığı uygular özelleştirilmiş talep kuralı verilmiştir:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Yeni Ekle veya zaten mevcut olan etki alanları dönüştürmek için çalışırken - SupportMultipleDomain anahtar kullanabilmek için federasyon güven zaten desteklemek için ayarlanan gerekiyor.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ile Azure AD arasında güven güncelleştirmek nasıl
AD FS ile Azure AD Örneğinize arasında federe güven yukarı ayarlamadıysanız bu güven yeniden oluşturmanız gerekebilir.  İlk olarak olmadan ayarlandığında, nedeni `-SupportMultipleDomain` parametresi IssuerUri varsayılan değer olan ayarlanır.  Aşağıdaki ekran görüntüsünde IssuerUri ayarlanmış görebilirsiniz https://adfs.bmcontoso.com/adfs/services/trust.

Azure AD portalında yeni bir etki alanı başarıyla eklediniz ve kullanarak dönüştürmeyi denemeden `Convert-MsolDomaintoFederated -DomainName <your domain>`, aşağıdaki hatayı alırsınız.

![Federasyon hata](./media/active-directory-multiple-domains/trust1.png)

Eklemeyi denediğinizde `-SupportMultipleDomain` geçin, aşağıdaki hatayı alırsınız:

![Federasyon hata](./media/active-directory-multiple-domains/trust2.png)

Çalıştırmayı denerseniz `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` özgün etki alanında da bir hatayla sonuçlanır.

![Federasyon hata](./media/active-directory-multiple-domains/trust3.png)

Ek bir üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.  Bir etki alanı eklemiş ve kullanmaz `-SupportMultipleDomain` parametresi, kaldırma ve özgün etki alanınızın güncelleştirme adımlarını ile başlar.  Üst düzey etki alanı henüz eklemediyseniz, PowerShell Azure AD Connect kullanarak bir etki alanı ekleme adımları başlatabilirsiniz.

Microsoft Online güven kaldırın ve özgün etki alanınızın güncelleştirmek için aşağıdaki adımları kullanın.

1. AD FS federasyon sunucunuzda açık **AD FS yönetimi.**
2. Sol bölmede, genişletin **güven ilişkileri** ve **bağlı olan taraf güvenleri**
3. Sağ tarafta silme **Microsoft Office 365 kimlik Platformu'na** girişi.
   ![Microsoft çevrimiçi kaldırma](./media/active-directory-multiple-domains/trust4.png)
4. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
5. Kullanıcı adı ve parola genel yöneticinin ile federasyonunu Azure AD etki alanını girin.
6. PowerShell'de girin `Connect-MsolService -Credential $cred`
7. PowerShell'de girin `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Bu güncelleştirme için özgün etki alanıdır.  Bu nedenle bu olacaktır yukarıdaki etki alanlarını kullanma:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

PowerShell kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın

1. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
2. Kullanıcı adı ve parola genel yöneticinin ile federasyonunu Azure AD etki alanını girin
3. PowerShell'de girin `Connect-MsolService -Credential $cred`
4. PowerShell'de girin `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Azure AD Connect'i kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.

1. Azure AD Connect masaüstünden başlatmak veya Başlat menüsü
2. "Ek Azure AD etki alanı Ekle" seçin ![ek bir ekleme Azure AD etki alanı](./media/active-directory-multiple-domains/add1.png)
3. Azure AD girin ve Active Directory kimlik bilgileri
4. Federasyon için yapılandırmak istediğiniz ikinci etki alanını seçin.
   ![Ek bir ekleme Azure AD etki alanı](./media/active-directory-multiple-domains/add2.png)
5. Yükle'ye tıklayın

### <a name="verify-the-new-top-level-domain"></a>Yeni üst düzey etki alanını doğrulayın
PowerShell komutunu kullanarak `Get-MsolDomainFederationSettings -DomainName <your domain>`güncelleştirilmiş IssuerUri görüntüleyebilirsiniz.  Aşağıdaki ekran görüntüsünde özgün etki alanı ayarları güncelleştirildi Federasyon gösterir. http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ve yeni etki alanında IssuerUri ayarlamak https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Alt etki alanları desteği
İşlenen şekilde Azure AD etki alanları nedeniyle bir alt etki alanı eklediğinizde, üst öğesinin ayarlarını devralır.  Bu nedenle, IssuerUri gereken üst eşleşecek şekilde.

Bunu sağlar, örneğin, t bmcontoso.com varsa ve corp.bmcontoso.com eklemek söyleyin.  Bir kullanıcıdan corp.bmcontoso.com IssuerUri olması gerekecektir  **http://bmcontoso.com/adfs/services/trust.**  Yukarıda Azure AD için uygulanan standart kural oluşturacağını ancak bir veren belirteciyle  **http://corp.bmcontoso.com/adfs/services/trust.** değeri gerekli etki alanı eşleşmez ve kimlik doğrulaması başarısız olur.

### <a name="how-to-enable-support-for-subdomains"></a>Alt etki alanları için desteği etkinleştirme
Bu davranışı geçici olarak çözmek için bağlı olan taraf güveni Microsoft Online için AD FS güncelleştirilmesi gerekiyor.  Bunu yapmak için kullanıcının UPN soneki gelen tüm alt etki alanları devre dışı özel veren değeriyle oluşturulurken şeritler böylece özel talep kuralı yapılandırmanız gerekir.

Aşağıdaki talep bunu yapın:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Normal ifade kümesindeki son numarası kaç üst etki kök etki alanınızdaki'dır. İki üst etki alanı gerekli; bu nedenle burada bmcontoso.com, kullanılır. Etki alanları üç ana durumunda tutulması gerçekleştirilen (örn: corp.bmcontoso.com), sayı üç olabilirdi sonra. Sonuçta bir aralık gösterilen, eşleşme her zaman maksimum etki alanlarının eşleşmesi için yapılır. "{2,3}" iki ila üç etki alanları ile eşleşir (örn: bmfabrikam.com ve corp.bmcontoso.com).

Alt etki alanlarını desteklemek için bir özel talep eklemek için aşağıdaki adımları kullanın.

1. Açık AD FS Yönetimi
2. Microsoft çevrimiçi RP güven sağ tıklayın ve düzenlemek talep kurallarını seçin
3. Üçüncü talep kuralı seçin ve Değiştir ![düzenleme talep](./media/active-directory-multiple-domains/sub1.png)
4. Geçerli talep değiştirin:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Talep değiştirin](./media/active-directory-multiple-domains/sub2.png)

5. Tamam'ı tıklatın.  Uygula'yı tıklatın.  Tamam'ı tıklatın.  AD FS Yönetimi'ni kapatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](active-directory-aadconnect-whats-next.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](active-directory-aadconnect-feature-automatic-upgrade.md), [Yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](active-directory-aadconnectsync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.