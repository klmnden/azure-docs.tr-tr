---
title: Azure AD Connect birden çok etki alanı
description: Bu belgede, ayarlama ve O365 ve Azure AD ile birden çok üst düzey etki alanı yapılandırma açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e822906a072ec8244c7108e98289482adebb5a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60245116"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Azure AD ile Federasyon için Çoklu Etki Alanı Desteği
Aşağıdaki belgeler, Office 365 veya Azure AD etki alanları ile Federasyon olduğunda birden çok en üst düzey etki alanları ve alt etki alanlarını kullanma hakkında yönergeler sağlar.

## <a name="multiple-top-level-domain-support"></a>Birden çok üst düzey etki alanı desteği
Birden çok Federasyon, Azure AD ile en üst düzey etki alanları ile bir üst düzey etki alanını federasyona ekleme, gerekli olmayan bazı ek yapılandırma gerektirir.

Bir etki alanının Azure AD ile Federasyon olduğunda, azure'da bir etki alanında bulunan çeşitli özellikleri ayarlarsınız.  Bir önemli IssuerUri olandır.  Bu özellik Azure AD tarafından belirteç ilişkili olduğu etki alanını tanımlamak için kullanılan bir URI'dir.  URI, ancak hepsi geçerli bir URI olmalıdır çözümlenmesi gerekmez.  Varsayılan olarak, Azure AD URI değeri Federasyon Hizmeti tanımlayıcısı olarak şirket içinde AD FS ayarlar yapılandırma.

> [!NOTE]
> Federasyon Hizmeti tanımlayıcısı bir Federasyon Hizmeti benzersiz olarak tanımlayan bir URI'dir.  Federasyon Hizmeti AD FS bu işlevler güvenlik belirteci hizmeti örneğidir.
>
>

PowerShell komutunu kullanarak IssuerUri görüntüleyebilirsiniz `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Birden fazla üst düzey etki alanı eklediğinizde, bir sorun ortaya çıkar.  Örneğin, Azure AD arasında federasyon kurulu varsayalım ve şirket içi ortamınız.  Bu belgede, etki alanı bmcontoso.com kullanılır.  Artık bir ikinci, üst düzey etki alanı, bmfabrikam.com eklendi.

![Etki Alanları](./media/how-to-connect-install-multiple-domains/domains.png)

Birleştirilecek bmfabrikam.com etki alanına dönüştürülecek denediğinizde hata oluşur.  Neden Azure AD IssuerUri özelliğini birden fazla etki alanı için aynı değere sahip izin vermeyen bir kısıtlamaya sahip olduğu.  

![Federasyon hata](./media/how-to-connect-install-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parametresi
Bu kısıtlama geçici olarak çözmek için kullanarak yapılabilir bir farklı IssuerUri eklemeniz gerekir `-SupportMultipleDomain` parametresi.  Bu parametre ile aşağıdaki cmdlet'ler kullanılır:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Bu parametre, Azure AD etki alanı adını temel alan IssuerUri yapılandırın yapar.  IssuerUri dizinler Azure AD'de benzersiz olur.  Parametresini kullanarak başarıyla tamamlamak için PowerShell komutunu sağlar.

![Federasyon hata](./media/how-to-connect-install-multiple-domains/convert.png)

Bmfabrikam.com etki alanı ayarlarını, bakarak aşağıdakileri görebilirsiniz:

![Federasyon hata](./media/how-to-connect-install-multiple-domains/settings.png)

`-SupportMultipleDomain` adfs.bmcontoso.com Federasyon Hizmeti'ne işaret edecek şekilde yapılandırılmış olan diğer bütün uç değiştirmez.

Başka bir şey, `-SupportMultipleDomain` mu, AD FS sistem Azure AD için verilen belirteçlere uygun veren değerini içeren güvence altına alır. Bu değer, kullanıcıların UPN etki alanı bölümü alma ve bu etki alanında IssuerUri, yani https://{upn sonek olarak ayarlayarak ayarlanır} / adfs/services/güven.

Bu nedenle Azure AD'ye kimlik doğrulaması sırasında veya kullanıcı belirteci IssuerUri öğesinde, Office 365, Azure AD'de etki alanını bulmak için kullanılır.  Bir eşleşme bulunamıyor, kimlik doğrulaması başarısız olur.

Örneğin, bir kullanıcının UPN ise bsimon@bmcontoso.com, belirteç, AD FS sorunları IssuerUri öğesinde ayarlanacak <http://bmcontoso.com/adfs/services/trust>. Bu öğe, Azure AD yapılandırmasının eşleşir ve kimlik doğrulaması başarılı olur.

Bu mantık uygulayan özel talep kuralı verilmiştir:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Yeni Ekle veya zaten var olan etki alanları dönüştürmek için çalışırken - SupportMultipleDomain anahtarı kullanmak için Federasyon güveninizi zaten bunları desteklemek için ayarlanmış gerekir.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ile Azure AD arasında güven güncelleştirme
AD FS ile Azure ad Örneğinize arasında federasyon güveni ayarlamadıysanız, bu güven yeniden oluşturmanız gerekebilir.  Neden olan, başlangıçta olmadan ayarlandığında `-SupportMultipleDomain` parametresi IssuerUri ile varsayılan değer ayarlanır.  Aşağıdaki ekran görüntüsünde IssuerUri ayarlandığında görebilirsiniz https://adfs.bmcontoso.com/adfs/services/trust.

Azure AD portalında yeni bir etki alanı başarıyla eklediniz ve sonra onu kullanarak dönüştürmeyi denemeden `Convert-MsolDomaintoFederated -DomainName <your domain>`, aşağıdaki hatayı alırsınız.

![Federasyon hata](./media/how-to-connect-install-multiple-domains/trust1.png)

Eklemeyi denerseniz `-SupportMultipleDomain` geçiş, aşağıdaki hatayı alırsınız:

![Federasyon hata](./media/how-to-connect-install-multiple-domains/trust2.png)

Çalıştırmayı denerken `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` özgün etki alanında da bir hataya neden olur.

![Federasyon hata](./media/how-to-connect-install-multiple-domains/trust3.png)

Ek bir üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.  Bir etki alanı zaten eklenmiş ve değil kullanmadı `-SupportMultipleDomain` parametresi, kaldırma ve özgün etki alanınızı güncelleştirmek için adımları ile başlayın.  Henüz bir üst düzey etki alanı eklemediyseniz, PowerShell, Azure AD Connect kullanarak bir etki alanı ekleme adımlarını ile başlayabilirsiniz.

Microsoft Online güven kaldırın ve özgün etki alanınızı güncelleştirmek için aşağıdaki adımları kullanın.

1. AD FS federasyon sunucunuzda açık **AD FS yönetimi.**
2. Sol tarafta, genişletme **güven ilişkileri** ve **bağlı olan taraf güvenleri**
3. Sağ tarafta Sil **Microsoft Office 365 kimlik platformu** girişi.
   ![Microsoft çevrimiçi Kaldır](./media/how-to-connect-install-multiple-domains/trust4.png)
4. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
5. Azure AD etki alanı ile Federasyon için kullanıcı adı ve bir genel yönetici parolasını girin.
6. PowerShell'de girin `Connect-MsolService -Credential $cred`
7. PowerShell'de girin `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Bu güncelleştirme için özgün etki alanıdır.  Bu nedenle bu olacaktır yukarıdaki etki alanları kullanarak:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

PowerShell kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.

1. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
2. Azure AD etki alanı ile Federasyon için genel yönetici parolası ve kullanıcı adı girin
3. PowerShell'de girin `Connect-MsolService -Credential $cred`
4. PowerShell'de girin `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Azure AD Connect kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.

1. Azure AD Connect masaüstünden başlatmak veya Başlat menüsü
2. "Ek bir Azure AD etki alanı Ekle" öğesini seçin ![ek bir ekleme Azure AD etki alanı](./media/how-to-connect-install-multiple-domains/add1.png)
3. Azure AD girin ve Active Directory kimlik bilgileri
4. Federasyon için yapılandırmak istediğiniz ikinci etki alanını seçin.
   ![Ek bir ekleme Azure AD etki alanı](./media/how-to-connect-install-multiple-domains/add2.png)
5. Yükle'ye tıklayın

### <a name="verify-the-new-top-level-domain"></a>Yeni üst düzey etki alanını doğrulama
PowerShell komutunu kullanarak `Get-MsolDomainFederationSettings -DomainName <your domain>`güncelleştirilmiş IssuerUri görüntüleyebilirsiniz.  Aşağıdaki ekran görüntüsünde özgün etki alanı ayarları güncelleştirildi Federasyon gösterir. http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Ve yeni etki alanında IssuerUri ayarlamak https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Alt etki alanları desteği
İşlenen şekilde Azure AD etki alanlarına nedeniyle bir alt etki alanı eklediğinizde, bu üst ayarlarını devralır.  Bu nedenle, IssuerUri gereken üst eşleştirilecek.

Bunu sağlar, örneğin, ı bmcontoso.com varsa ve corp.bmcontoso.com eklersiniz varsayalım.  Bir kullanıcının corp.bmcontoso.com IssuerUri olması gerekecektir  **http://bmcontoso.com/adfs/services/trust.**  Azure AD için yukarıdaki uygulanan standart kural üretir ancak bir veren belirteciyle  **http://corp.bmcontoso.com/adfs/services/trust.** değer gerekli etki alanı eşleşmez ve kimlik doğrulaması başarısız olur.

### <a name="how-to-enable-support-for-subdomains"></a>Alt etki alanları için desteği etkinleştirme
Bu davranışa geçici bir çözüm için AD FS bağlı olan taraf güveni için Microsoft Online güncelleştirilmesi gerekiyor.  Bunu yapmak için kullanıcının UPN soneki öğesinden herhangi bir alt etki alanlarını devre dışı özel veren değerini oluştururken kaldırır, böylece bir özel talep kuralı yapılandırmanız gerekir.

Aşağıdaki talep, bunu yapabilirsiniz:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Son normal ifade kümesi kaç üst etki, kök etki alanındadır sayısıdır. İki üst etki alanı gerekli olacak şekilde burada bmcontoso.com kullanılır. Üç ana varsa tutulması için olan etki alanları (örn: corp.bmcontoso.com), sayı üç olabilirdi sonra. Sonunda bir aralık gösterilen, eşleşen her zaman en yüksek etki alanlarının eşleştirilecek yapılacaktır. "{2,3}" iki ila üç etki alanı eşleşir (örn: bmfabrikam.com ve corp.bmcontoso.com).

Alt etki alanlarını desteklemek için bir özel talep eklemek için aşağıdaki adımları kullanın.

1. Açık AD FS Yönetimi
2. Microsoft Online RP güvene sağ tıklayın ve düzenlemek için talep kurallarını seçin
3. Üçüncü talep kuralı seçin ve Değiştir ![talep Düzenle](./media/how-to-connect-install-multiple-domains/sub1.png)
4. Geçerli talep değiştirin:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Talep değiştirin](./media/how-to-connect-install-multiple-domains/sub2.png)

5. Tamam'a tıklayın.  Uygula düğmesini tıklatın.  Tamam'a tıklayın.  AD FS Yönetimi'ni kapatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md), [yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md), ve [Azure AD Connect Health](how-to-connect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
