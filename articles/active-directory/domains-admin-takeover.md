---
title: Bir yönetilmeyen dizinin veya Azure Active Directory'de gölge Kiracı yönetici devralma | Microsoft Docs
description: Bir yönetilmeyen dizininde (gölge Kiracı) Azure Active Directory DNS etki alanı adı öncelikli yapma.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 04/06/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: b1185fef53797a88ae929e35be56d2bc79067b49
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Azure Active Directory'de yönetici olarak bir yönetilmeyen dizin üzerinde gerçekleştirin
Bu makalede, Azure Active Directory (Azure AD) yönetilmeyen bir dizinde bir DNS etki alanı adı öncelikli iki yolu açıklanmaktadır. Bir self servis kullanıcısı, Azure AD kullanan bir bulut hizmetine kaydolduğunda bu kullanıcı, e-posta etki alanına göre yönetilmeyen bir Azure AD dizinine eklenir. Self Servis ya da "viral" için bir hizmet hakkında daha fazla bilgi için bkz: [Azure Active Directory için Self Servis kaydolma nedir?]()

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Yönetilmeyen bir dizin üzerinde almak istediğiniz nasıl karar verin
Yönetici devralma işlemi sırasında, [Azure AD’ye özel etki alanı adı ekleme](add-custom-domain.md) bölümünde açıklandığı gibi sahipliği kanıtlayabilirsiniz. Sonraki bölümlerde, yönetici deneyimi daha ayrıntılı şekilde açıklanmaktadır, ancak bir özeti aşağıda verilmiştir:

* Yönetilmeyen bir Azure dizininin ["iç" yönetici devralma işlemini](#internal-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizinin genel yöneticisi olarak eklendiniz. Herhangi bir kullanıcı, etki alanı veya hizmet planı, yönettiğiniz diğer dizine geçirilmiyor.

* Yönetilmeyen bir Azure dizininin ["dış" yönetici devralma işlemini](#external-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizininizin DNS etki alanı adını, yönetilen Azure dizininize eklersiniz. Etki alanı adını eklediğinizde, kullanıcıların kesinti olmadan hizmetlere erişmeye devam edebilmesi için yönetilen Azure dizininizde, kullanıcıların kaynaklara bir eşlemesi oluşturulur. 

## <a name="internal-admin-takeover"></a>İç yönetim devralma

SharePoint ve OneDrive, Office 365 gibi dahil bazı ürünler dış devralma desteklemez. Senaryonuz olan ya da bir yönetici olan ve yönetilmeyen almak istediğiniz veya "gölge" Kiracı Self Servis kaydolma kullanan kullanıcılar tarafından oluşturursanız, bu bir iç yönetici devralma ile yapabilirsiniz.

1. Bir kullanıcı bağlamı gibi Power BI ile kaydolan yoluyla yönetilmeyen Kiracı oluşturun. Örneğin kolaylık olması için aşağıdaki adımları yol varsayalım.

2. Açık [Power BI sitesini](https://powerbi.com) seçip **Başlat boş**. Kuruluş etki alanı adını kullanan bir kullanıcı hesabı girin; Örneğin, `admin@fourthcoffee.xyz`. Doğrulama kodu girdikten sonra onay kodu için e-postanızı kontrol edin.

3. Doğrulama e-içinde Power BI, seçin **bana Evet,**.

4. Oturum [Office 365 Yönetim Merkezi](https://portal.office.com/adminportal/Home) Power BI kullanıcı hesabı ile. Size yönlendiren bir ileti alırsınız **yönetici hale** yönetilmeyen kiracınızda zaten doğrulandı etki alanı adı. seçin **Evet, yönetici olmasını istediğiniz**.
  
  ![Become yönetici için ilk ekran görüntüsü](./media/domains-admin-takeover/become-admin-first.png)
  
5. Etki alanı adını kendi kanıtlamak için TXT kaydı ekleme **fourthcoffee.xyz** kayıt şirketinizdeki etki alanı adı. Bu örnekte, GoDaddy.com değil.
  
  ![Etki alanı adı için bir txt kaydı ekleme](./media/domains-admin-takeover/become-admin-txt-record.png)

DNS TXT kayıtları etki alanı adı kayıt şirketinizde belirlediğinizde, Azure AD kiracısı yönetebilirsiniz.

Yukarıdaki adımları tamamladıktan sonra artık Office 365'te Fourth Coffee Kiracı genel yönetici demektir. Etki alanı adı, diğer Azure hizmetleriyle tümleştirmeye yönelik, Office 365'ten kaldırın ve farklı bir yönetilen Kiracı Azure ekleyin.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>Azure AD'de yönetilen bir kiracı için etki alanı adı ekleme 

1. Açık [Office 365 Yönetim Merkezi](https://portal.office.com/adminportal/Home).
2. Seçin **kullanıcılar** sekmesini tıklatın ve gibi bir adla yeni bir kullanıcı hesabı oluşturun *user@fourthcoffeexyz.onmicrosoft.com* özel etki alanı adını kullanmaz. 
3. Yeni kullanıcı hesabı için Azure AD kiracısı genel yönetici ayrıcalıklarına sahip olduğundan emin olun.
4. Açık **etki alanları** sekmesinde Office 365 Yönetim Merkezi'nde, etki alanı adı seçin ve Seç **kaldırmak**. 
  
  ![etki alanı adını Office 365'ten Kaldır](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Tüm kullanıcılar veya Office 365'te Kaldırılan etki alanı adını başvuru grupları varsa, bunlar için kaydedilmelidir. onmicrosoft.com etki alanı. Zorlayabilir etki alanı adı silerseniz, tüm kullanıcıların otomatik olarak, bu örnekte adlandırılır *user@fourthcoffeexyz.onmicrosoft.com*.
  
6. Oturum [Azure AD Yönetim Merkezi](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) Azure AD Kiracı için genel yönetici olan bir hesapla.
  
7. Seçin **özel etki alanı adları**, etki alanı adını ekleyin. Etki alanı adını sahipliği doğrulamak için DNS TXT kayıt girmeniz gerekecek. 
  
  ![Azure AD ile eklenen etki alanı](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Etki alanı adı kaldırılırsa Office 365 Kiracı atanan lisanslara sahip tüm kullanıcılar Power BI veya Azure Rights Management hizmetinin kendi panolar kaydetmeniz gerekir. Bir kullanıcı adı gibi oturum imzalamalısınız *user@fourthcoffeexyz.onmicrosoft.com* yerine *user@fourthcoffee.xyz*.

## <a name="external-admin-takeover"></a>Dış yönetim devralma

Bir kiracı Azure hizmetlerine veya Office 365 ile yönetiyorsanız, başka bir Azure AD kiracınızda zaten doğrulandı, özel etki alanı ekleyemezsiniz. Ancak, yönetilen Azure AD kiracınızda gelen bir dış yönetici devralma yönetilmeyen bir kiracı alabilir. Makaleyi genel yordam aşağıdaki [Azure AD ile özel bir etki alanı ekleme](add-custom-domain.md).

Etki alanı adı sahipliğini doğruladığınızda Azure AD etki alanı adı yönetilmeyen kiracıdan kaldırır ve varolan kiracınız taşır. Yönetilmeyen bir dizinin dış yönetici devralma iç yönetim devralma aynı DNS TXT doğrulama süreci gerektirir. Aşağıdakileri de etki alanı adıyla taşınması fark vardır:

- Kullanıcılar
- Abonelikler
- Lisans atama

### <a name="support-for-external-admin-takeover"></a>Dış yönetim devralma desteği
Dış yönetim devralma aşağıdaki çevrimiçi hizmetler tarafından desteklenir:

- Power BI
- Azure Rights Management
- Exchange Online

Desteklenen hizmet planları şunları içerir:

- Ücretsiz Power BI
- Power BI Pro
- PowerApps boş
- PowerFlow boş
- Kişiler için RMS
- Microsoft Stream
- Dynamics 365 ücretsiz deneme sürümü

Dış yönetim devralma hizmet planları, SharePoint, OneDrive veya Skype için iş dahil olan tüm hizmetler için desteklenmiyor; Örneğin, bir Office ücretsiz abonelik veya Office temel SKU. İsteğe bağlı olarak kullanabileceğiniz [ **ForceTakeover** seçeneği](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) etki alanı adı yönetilmeyen kiracıdan kaldırma ve istenen Kiracı'doğrulanıyor. Bu ForceTakeover seçeneği değil içindeki kullanıcı taşımak veya aboneliğe erişimi korur. Bunun yerine, bu seçenek, yalnızca etki alanı adını taşır. 

#### <a name="more-information-about-rms-for-individuals"></a>Kişiler için RMS hakkında daha fazla bilgi

İçin [kişiler için RMS](/information-protection/understand-explore/rms-for-individuals), yönetilmeyen Kiracı Kiracı ile aynı bölgede size ait olduğunu, otomatik olarak oluşturulan olduğunda [Azure Information Protection Kiracı anahtarını](/information-protection/plan-design/plan-implement-tenant-key) ve [varsayılan koruma şablonları](/information-protection/deploy-use/configure-usage-rights#rights-included-in-the-default-templates) etki alanı adıyla ayrıca taşındığını. 

Yönetilmeyen Kiracı farklı bir bölgede olduğunda anahtar ve şablonları taşınmaz. Örneğin, Avrupa ve Kuzey Amerika olan kendi Kiracı yönetilmeyen Kiracı kullanılıyor. 

Kişiler için RMS korumalı içeriği açmak için Azure AD kimlik doğrulamayı destekleyecek şekilde tasarlanmış olsa da, kullanıcıların içeriği de koruma engellemez. Kullanıcıların kişiler için RMS aboneliği içerikle koruma sağlamaz ve anahtar ve şablonları taşınmadı, o içeriği sonra etki alanı devralma erişilemeyecek.    

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>ForceTakeover seçeneği için Azure AD PowerShell cmdlet'leri
Kullanılan Bu cmdlet görebilirsiniz [PowerShell örnek](#powershell-example).


cmdlet'i | Kullanım 
------- | -------
`connect-msolservice` | İstendiğinde, yönetilen kiracınız için oturum açın.
`get-msoldomain` | Geçerli Kiracı ile ilişkilendirilen, etki alanı adlarını gösterir.
`new-msoldomain –name <domainname>` | Etki alanı adı (DNS doğrulama henüz gerçekleştirilmedi) Unverified Kiracı ekler.
`get-msoldomain` | Etki alanı adı, yönetilen bir kiracı ile ilişkili etki alanı adlarının listesini artık dahil, ancak olarak listelenen **Unverified**.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Etki alanı için yeni bir DNS TXT kayıt yerleştirin için bilgiler sağlar (MS xxxxx =). Doğrulama durum hemen yaymak TXT kaydı için biraz zaman alır çünkü böylece beklememek dikkate önce birkaç dakika **- ForceTakeover** seçeneği. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Etki alanı adınızı hala doğrulanamazsa, devam edebilmeniz **- ForceTakeover** seçeneği. TXT kaydı oluşturuldu ve devralma işlem başlatır doğrular.<li>**- ForceTakeover** seçeneği eklenmesi cmdlet'e yalnızca yönetilmeyen Kiracı devralma engelleme Office 365 hizmetlerine olduğunda gibi bir dış yönetici devralma zorlama olduğunda.
`get-msoldomain` | Etki alanı listesi şimdi etki alanı adı olarak görünür **doğrulandı**.

### <a name="powershell-example"></a>PowerShell örneği

1. Self Servis teklifine yanıt vermek için kullanılan kimlik bilgilerini kullanarak Azure ad connect:
  ````
    import-module MSOnline
    $msolcred = get-credential
    
    connect-msolservice -credential $msolcred
  ````
2. Etki alanlarının bir listesini alın:
  
  ````
    Get-MsolDomain
  ````
3. Bir challenge oluşturmak için Get-MsolDomainVerificationDns cmdlet'ini çalıştırın:
  ````
    Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
  
    For example:
  
    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
  ````

4. Bu komuttan döndürülen değeri (sınama) kopyalayın. Örneğin:
  ````
    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
  ````
5. Genel DNS ad alanınız içinde önceki adımda kopyaladığınız değeri içeren bir DNS txt kaydı oluşturun. Bu kayıt için ad üst etki alanı adı, Windows Server DNS rolünden kullanarak bu kaynak kaydı oluşturun, böylece kayıt adı boş ve hemen Yapıştır değeri metin kutusuna bırakın.
6. Sınama doğrulamak için Onayla MsolDomain cmdlet'ini çalıştırın:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*
  ````
  
  Örneğin:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com
  ````

Başarılı bir challenge, bir hata olmadan istemi döndürür.

## <a name="next-steps"></a>Sonraki adımlar
* [Özel etki alanı adını Azure AD'ye ekleme](add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
