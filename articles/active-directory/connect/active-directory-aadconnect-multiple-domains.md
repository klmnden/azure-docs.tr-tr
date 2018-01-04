---
title: "Azure AD Connect birden çok etki alanı"
description: "Bu belgede ayarlama ve O365 ve Azure AD ile birden çok üst düzey etki alanlarını yapılandırma açıklanmaktadır."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 597ea863275a5603e093307ce4334ae68e5ea5cf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Azure AD ile Federasyon için Çoklu Etki Alanı Desteği
Aşağıdaki belgeler Office 365 veya Azure AD etki alanları ile federasyonunu zaman birden çok üst düzey etki alanları ve alt etki alanlarını kullanma hakkında yönergeler sağlar.

## <a name="multiple-top-level-domain-support"></a>Birden çok üst düzey etki alanı desteği
Birden çok federasyonunu, üst düzey etki alanlarının Azure AD ile bir üst düzey etki alanı ile federasyonunu kullanılırken gerekli değildir bazı ek yapılandırma gerektirir.

Bir etki alanı Azure AD ile Federasyon olduğunda, çeşitli özellikleri etki alanı Azure ayarlanır.  Bir önemli IssuerUri adrestir.  Azure AD tarafından belirteci ile ilişkili etki alanını tanımlamak için kullanılan bir URI budur.  URI, herhangi bir şey ancak geçerli bir URI olmalıdır çözümlemek gerekmez.  Varsayılan olarak, Azure AD bu Federasyon Hizmeti tanımlayıcısı değerine, şirket içi AD FS ayarlar yapılandırma.

> [!NOTE]
> Federasyon Hizmeti tanımlayıcısı bir Federasyon Hizmeti benzersiz olarak tanımlayan bir URI değil.  Federasyon Hizmeti AD FS bu işlevler güvenlik belirteci hizmeti örneğidir. 
> 
> 

PowerShell komutunu kullanarak görünümü IssuerUri yapabilecekleriniz `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Birden çok üst düzey etki alanı eklemek istediğinizde bir sorun ortaya çıkar.  Örneğin, Kurulum Federasyon Azure AD arasında sahip düşünelim ve şirket içi ortamınıza.  Bu belge için bmcontoso.com kullanıyorum.  İkinci, üst düzey etki alanı eklemiş olduğunuz artık bmfabrikam.com.

![Etki Alanları](./media/active-directory-multiple-domains/domains.png)

Biz birleştirilecek bizim bmfabrikam.com etki alanı dönüştürmeye çalıştığınızda, biz hata alırsınız.  Bunun nedeni, Azure AD'de birden fazla etki alanı için aynı değeri sağlamak IssuerUri özelliğe izin vermeyen bir kısıtlaması vardır.  

![Federasyon hata](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parametresi
Geçici çözüm için bu, hangi kullanılarak yapılabilir farklı bir IssuerUri eklemek ihtiyacımız `-SupportMultipleDomain` parametresi.  Bu parametre, aşağıdaki cmdlet ile birlikte kullanılır:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Bu parametre, Azure AD etki alanı adını temel alarak IssuerUri yapılandırın yapar.  Bu dizinlerde Azure AD içinde benzersiz olacaktır.  Parametresini kullanarak başarıyla tamamlamak için PowerShell komutunu sağlar.

![Federasyon hata](./media/active-directory-multiple-domains/convert.png)

Bizim yeni bmfabrikam.com etki alanı ayarında aramanız aşağıdakileri görebilirsiniz:

![Federasyon hata](./media/active-directory-multiple-domains/settings.png)

Unutmayın `-SupportMultipleDomain` hangi adfs.bmcontoso.com bizim Federasyon Hizmeti'ne işaret edecek şekilde yapılandırılmış olan diğer uç noktalardan değiştirmez.

Başka bir şey, `-SupportMultipleDomain` yapar, uygun veren değeriyle AD FS sistem için Azure AD yayınlanan belirteçleri içerir güvence altına alır. Bunu kullanıcıların UPN etki alanı kısmı alma ve bu etki alanında IssuerUri, yani https://{upn soneki olarak ayarlayarak yapar} / adfs/services/güven. 

Bu nedenle Azure ad kimlik doğrulaması sırasında veya kullanıcı belirteci IssuerUri öğesinde, Office 365, Azure AD etki alanını bulmak için kullanılır.  Kimlik doğrulaması başarısız olur bir eşleşme bulunamazsa. 

Örneğin, bir kullanıcının UPN ise bsimon@bmcontoso.com, belirteç AD FS sorunları IssuerUri öğesinde http://bmcontoso.com/adfs/services/trust için ayarlanacak. Bu Azure AD yapılandırma ile eşleşir ve kimlik doğrulama başarılı olur.

Bu mantığı uygular özelleştirilmiş talep kuralı verilmiştir:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Yeni Ekle veya zaten Dönüştür girişimi etki alanları eklendiğinde - SupportMultipleDomain anahtar kullanabilmek için Kurulum başlangıçta desteklemek için federasyon güven olması gerekir.  
> 
> 

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ile Azure AD arasında güven güncelleştirmek nasıl
AD FS ile Azure AD Örneğinize arasında federe güven Kurulum belirtilmedi, bu güven yeniden oluşturmanız gerekebilir.  Kurulum olmadan başlangıçta olduğu zaman, bunun nedeni `-SupportMultipleDomain` parametresi IssuerUri varsayılan değer olan ayarlanır.  Aşağıdaki ekran görüntüsünde IssuerUri https://adfs.bmcontoso.com/adfs/services/trust için ayarlandığını görebilirsiniz.

Bunu şimdi, Azure AD portalında yeni bir etki alanı başarıyla eklediniz ve kullanarak dönüştürmeyi denemeden `Convert-MsolDomaintoFederated -DomainName <your domain>`, biz aşağıdaki hatayı alıyorsunuz.

![Federasyon hata](./media/active-directory-multiple-domains/trust1.png)

Eklemeyi denediğinizde `-SupportMultipleDomain` anahtar biz aşağıdaki hatayı alırsınız:

![Federasyon hata](./media/active-directory-multiple-domains/trust2.png)

Çalıştırmayı denerseniz `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` özgün etki alanında da bir hatayla sonuçlanır.

![Federasyon hata](./media/active-directory-multiple-domains/trust3.png)

Ek bir üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.  Bir etki alanı eklemiş ve kullanmaz `-SupportMultipleDomain` kaldırma ve özgün etki alanınızın güncelleştirme için adımlara parametre Başlat.  Henüz bir üst düzey etki alanı eklemediyseniz PowerShell Azure AD Connect kullanarak bir etki alanı ekleme adımları başlatabilirsiniz.

Microsoft Online güven kaldırın ve özgün etki alanınızın güncelleştirmek için aşağıdaki adımları kullanın.

1. AD FS federasyon sunucunuzda açık **AD FS yönetimi.** 
2. Sol bölmede, genişletin **güven ilişkileri** ve **bağlı olan taraf güvenleri**
3. Sağ tarafta silme **Microsoft Office 365 kimlik Platformu'na** girişi.
   ![Microsoft çevrimiçi kaldırma](./media/active-directory-multiple-domains/trust4.png)
4. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
5. Kullanıcı adı ve parola genel yöneticinin ile federasyonunu Azure AD etki alanını girin
6. PowerShell'de girin`Connect-MsolService -Credential $cred`
7. PowerShell'de girin `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Özgün etki alanı için budur.  Bu nedenle bu olacaktır yukarıdaki etki alanlarını kullanma:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

PowerShell kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın

1. Sahip bir makinede [Azure Active Directory için Windows PowerShell Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü aşağıdaki komutu çalıştırın: `$cred=Get-Credential`.  
2. Kullanıcı adı ve parola genel yöneticinin ile federasyonunu Azure AD etki alanını girin
3. PowerShell'de girin`Connect-MsolService -Credential $cred`
4. PowerShell'de girin`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Azure AD Connect'i kullanarak yeni üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.

1. Azure AD Connect masaüstünden başlatmak veya Başlat menüsü
2. "Ek Azure AD etki alanı Ekle" seçin ![ek bir ekleme Azure AD etki alanı](./media/active-directory-multiple-domains/add1.png)
3. Azure AD girin ve Active Directory kimlik bilgileri
4. Federasyon için yapılandırmak istediğiniz ikinci etki alanını seçin.
   ![Ek bir ekleme Azure AD etki alanı](./media/active-directory-multiple-domains/add2.png)
5. Yükle'yi tıklatın

### <a name="verify-the-new-top-level-domain"></a>Yeni üst düzey etki alanını doğrulayın
PowerShell komutunu kullanarak `Get-MsolDomainFederationSettings -DomainName <your domain>`güncelleştirilmiş IssuerUri görüntüleyebilirsiniz.  Aşağıdaki ekran görüntüsünde ayarlar bizim özgün etki alanı http://bmcontoso.com/adfs/services/trust üzerinde güncelleştirildi Federasyon gösterir.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Bizim yeni bir etki alanı üzerinde IssuerUri https://bmfabrikam.com/adfs/services/trust için ayarlayın

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Alt etki alanları desteği
İşlenen şekilde Azure AD etki alanları nedeniyle bir alt etki alanı eklediğinizde, üst öğesinin ayarlarını devralır.  Bu, IssuerUri Üst eşleşmesi gerektiği anlamına gelir.

Bu nedenle deyin ı bmcontoso.com varsa ve corp.bmcontoso.com eklemek örneğin olanak sağlar.  Bir kullanıcıdan corp.bmcontoso.com IssuerUri olması gerekir yani **http://bmcontoso.com/adfs/services/trust.**  Yukarıda Azure AD için uygulanan standart kural oluşturacağını ancak bir veren belirteciyle **http://corp.bmcontoso.com/adfs/services/trust.** değeri gerekli etki alanı eşleşmez ve kimlik doğrulaması başarısız olur.

### <a name="how-to-enable-support-for-sub-domains"></a>Alt etki alanları desteği etkinleştirme
AD FS bu sorunu çözmek için Microsoft Online bağlı olan taraf güveni güncelleştirilmesi gerekir.  Bunu yapmak için tüm alt etki alanlarından kullanıcı UPN soneki kapalı özel veren değeriyle oluşturulurken şeritler böylece özel talep kuralı yapılandırmanız gerekir. 

Aşağıdaki talep bunu yapın:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Normal ifade son sayısı, kök etki alanında yok kaç üst etki alanları ayarlayın. Burada i sahip bmcontoso.com şekilde iki üst etki alanı gerekli. Etki alanları üç ana durumunda tutulması gerçekleştirilen (örn: corp.bmcontoso.com), sayı üç olabilirdi sonra. Eventualy bir aralık gösterilen, eşleşme her zaman maksimum etki alanlarının eşleşmesi için yapılır. "{2,3}" iki ila üç etki alanları eşleşir (örn: bmfabrikam.com ve corp.bmcontoso.com).

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

