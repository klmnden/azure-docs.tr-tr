---
title: Yönetilmeyen bir dizinin - Azure Active Directory yönetici devralma | Microsoft Docs
description: Bir DNS etki alanı adı (gölge Kiracı) Azure Active Directory'de bir yönetilmeyen dizinde öncelikli yapma.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: b32ef37c6d61c88a18acd5ddc80cc6154369ca29
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65780532"
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Azure Active Directory'de yönetici olarak yönetilmeyen bir dizini devralma

Bu makalede, Azure Active Directory (Azure AD) bir yönetilmeyen dizinde bir DNS etki alanı adı ele iki yolu açıklanır. Bir self servis kullanıcısı, Azure AD kullanan bir bulut hizmetine kaydolduğunda bu kullanıcı, e-posta etki alanına göre yönetilmeyen bir Azure AD dizinine eklenir. Self Servis veya "viral" hizmeti için kayıt hakkında daha fazla bilgi için bkz. [Azure Active Directory için Self Servis kaydolma nedir?](directory-self-service-signup.md)

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Nasıl yönetilmeyen bir dizini devralma istediğinize karar verin
Yönetici devralma işlemi sırasında, [Azure AD’ye özel etki alanı adı ekleme](../fundamentals/add-custom-domain.md) bölümünde açıklandığı gibi sahipliği kanıtlayabilirsiniz. Sonraki bölümlerde, yönetici deneyimi daha ayrıntılı şekilde açıklanmaktadır, ancak bir özeti aşağıda verilmiştir:

* Yönetilmeyen bir Azure dizininin ["iç" yönetici devralma işlemini](#internal-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizinin genel yöneticisi olarak eklendiniz. Herhangi bir kullanıcı, etki alanı veya hizmet planı, yönettiğiniz diğer dizine geçirilmiyor.

* Yönetilmeyen bir Azure dizininin ["dış" yönetici devralma işlemini](#external-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizininizin DNS etki alanı adını, yönetilen Azure dizininize eklersiniz. Etki alanı adını eklediğinizde, kullanıcıların kesinti olmadan hizmetlere erişmeye devam edebilmesi için yönetilen Azure dizininizde, kullanıcıların kaynaklara bir eşlemesi oluşturulur. 

## <a name="internal-admin-takeover"></a>İç yönetici devralma işlemini

SharePoint ve OneDrive, Office 365 gibi bazı ürünler, dış devralma desteklemez. Senaryonuz olan veya bir Yöneticiyseniz ve yönetilmeyen almak istiyorsanız veya "gölge" Kiracı Self Servis kayıt kullanan kullanıcılar tarafından oluşturursanız, bir iç yönetici devralma işlemini ile bunu yapabilirsiniz.

1. Power BI'a kaydolma aracılığıyla yönetilmeyen bir Kiracı Kullanıcı bağlamı oluşturun. Örneğin kolaylık olması için aşağıdaki adımları bu yolu varsayılır.

2. Açık [Power BI sitenizde](https://powerbi.com) seçip **ücretsiz Başlat**. Kuruluş etki alanı adını kullanan bir kullanıcı hesabı girin; Örneğin, `admin@fourthcoffee.xyz`. Doğrulama kodu girdikten sonra onay kodu için e-postanızı kontrol edin.

3. Onay e-postadaki Power bı'dan seçin **Evet, bu benim**.

4. Oturum [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com) Power BI kullanıcı hesabıyla. Yönlendiren bir ileti alırsınız **yönetici olun** etki alanı adının yönetilmeyen kiracıda zaten doğrulandı. seçin **Evet, yönetici olmak istiyorum**.
  
   ![Yönetici olun ilk ekran görüntüsü](./media/domains-admin-takeover/become-admin-first.png)
  
5. Etki alanı adının ait olduğunu kanıtlamak için TXT kaydı eklemek **fourthcoffee.xyz** adı kayıt şirketinde, etki alanı. Bu örnekte, buna GoDaddy.com var.
  
   ![Bir txt kaydı etki alanı adı ekleme](./media/domains-admin-takeover/become-admin-txt-record.png)

DNS TXT kayıtlarının, etki alanı adı kayıt şirketinize belirlediğinizde, Azure AD kiracısını yönetebilirsiniz.

Yukarıdaki adımları tamamladıktan sonra artık Office 365'te Fourth Coffee kiracının genel Yöneticisi olursunuz. Etki alanı adı, diğer Azure Hizmetleri ile tümleştirme için Office 365'ten kaldırın ve azure'da yönetilen farklı bir kiracıya ekleyin.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>Etki alanı adı, Azure AD'de yönetilen bir kiracıya ekleme

1. Açık [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com).
2. Seçin **kullanıcılar** sekmesini tıklatıp gibi yeni bir kullanıcı hesabı oluşturmanız *kullanıcı\@fourthcoffeexyz.onmicrosoft.com* özel etki alanı adını kullanmaz. 
3. Yeni kullanıcı hesabının Azure AD kiracınız için genel yönetici ayrıcalıkları olduğundan emin olun.
4. Açık **etki alanları** sekmesinde Microsoft 365 Yönetim merkezinde, etki alanı adını seçip seçin **Kaldır**. 
  
   ![etki alanı adını Office 365'ten Kaldır](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Kullanıcıları veya grupları Office 365'te başvuran Kaldırılan etki alanı adı varsa, bunlar için kaydedilmelidir. onmicrosoft.com etki alanı. Zorlarsanız, etki alanı adını silmek, tüm kullanıcılar otomatik olarak, bu örnekte adlandırılır *kullanıcı\@fourthcoffeexyz.onmicrosoft.com*.
  
6. Oturum [Azure AD yönetim merkezini](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) Azure AD kiracınız için genel yönetici olan bir hesapla.
  
7. Seçin **özel etki alanı adları**, sonra etki alanı adı ekleyin. Etki alanı sahipliğini doğrulamak için DNS TXT kayıtlarının girmek zorunda kalırsınız. 
  
   ![Azure AD'ye eklenen doğrulanmış etki alanı](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Office 365 kiracıya atanan lisanslara sahip tüm kullanıcılar Power BI veya Azure Rights Management hizmeti, etki alanı adı kaldırılırsa, panoları kaydetmeniz gerekir. Gibi bir kullanıcı adı ile oturum açmaları gerektiğini *kullanıcı\@fourthcoffeexyz.onmicrosoft.com* yerine *kullanıcı\@fourthcoffee.xyz*.

## <a name="external-admin-takeover"></a>Dış yönetici devralma

Zaten bir kiracı Azure hizmetlerine veya Office 365 ile yönetiyorsanız, zaten başka bir Azure AD kiracısında doğrulanırsa, özel etki alanı ekleyemezsiniz. Ancak, Azure AD'de yönetilen kiracınızdan yönetilmeyen bir kiracı bir dış yönetici devralma işlemini alabilir. Genel yordam aşağıdaki makalede [Azure AD'ye özel etki alanı ekleme](../fundamentals/add-custom-domain.md).

Etki alanı sahipliğini doğrulayın, Azure AD etki alanı adı yönetilmeyen kiracıdan kaldırır ve mevcut kiracınıza taşır. Dış yönetici devralma işlemini bir yönetilmeyen dizinin iç yönetici devralma işlemini aynı DNS TXT doğrulama işlemine gerektirir. Aşağıdakileri de etki alanı adıyla taşınması fark vardır:

- Kullanıcılar
- Subscriptions
- Lisans ataması

### <a name="support-for-external-admin-takeover"></a>Dış yönetici devralma işlemini desteği
Dış yönetici devralma işlemini aşağıdaki online services tarafından desteklenir:

- Power BI
- Azure Rights Management
- Exchange Online

Desteklenen hizmet planı içerir:

- Power BI ücretsiz
- Power BI Pro
- Ücretsiz bir PowerApps
- PowerFlow ücretsiz
- Kişiler için RMS
- Microsoft Stream
- Dynamics 365 ücretsiz deneme

Hizmet planları, SharePoint, OneDrive veya iş için Skype Kurumsal dahil olan herhangi bir hizmeti için dış yönetici devralma işlemini desteklenmiyor; Örneğin, bir Office ücretsiz abonelik veya Office temel SKU. İsteğe bağlı olarak kullanabileceğiniz [ **zorla devralma** seçeneği](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) yönetilmeyen kiracıdan etki alanı adı kaldırılıyor ve istenen kiracıda doğrulanıyor. Bu zorla devralma seçeneği değil kullanıcıları taşımak veya aboneliğe erişimi korur. Bunun yerine, bu seçenek, yalnızca etki alanı adını taşır. 

#### <a name="more-information-about-rms-for-individuals"></a>Kişiler için RMS hakkında daha fazla bilgi

İçin [kişiler için RMS](/azure/information-protection/rms-for-individuals), yönetilmeyen bir kiracı Kiracı ile aynı bölgede size ait olduğunu, otomatik olarak oluşturulan olduğunda [Azure Information Protection Kiracı anahtarınızı](/azure/information-protection/plan-implement-tenant-key) ve [varsayılan koruma şablonları](/azure/information-protection/configure-usage-rights#rights-included-in-the-default-templates) etki alanı adıyla ayrıca taşındığını. 

Yönetilmeyen Kiracı farklı bir bölgede olduğunda anahtar ve şablonları taşınmaz. Örneğin, Avrupa ve Kuzey Amerika içinde olan sahip kiracısı yönetilmeyen Kiracı olur. 

Kişiler için RMS korumalı içeriği açmak için Azure AD kimlik doğrulamasını desteklemek için tasarlanmış olsa da, kullanıcıların içeriği korumaktan engellemez. Kullanıcıların kişiler için RMS aboneliği ile içerik koruma ve anahtar ve şablonları taşınmadı, içeriğin sonra etki alanı devralma erişilemez.

#### <a name="more-information-about-power-bi"></a>Power BI hakkında daha fazla bilgi

Devralma yerleştirilir önce oluşturduğunuz bir dış devralma, Power BI içeriğini gerçekleştirirken bir [Power BI arşivlenmiş çalışma](/power-bi/service-admin-power-bi-archived-workspace). El ile yeni kiracıda kullanmak istediğiniz herhangi bir içeriği geçirmeniz gerekir.

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>Zorla devralma seçeneği için Azure AD PowerShell cmdlet'leri
Bu cmdlet'ler içinde kullanılan görebilirsiniz [PowerShell örneği](#powershell-example).


Cmdlet'i | Kullanım 
------- | -------
`connect-msolservice` | İstendiğinde, yönetilen bir kiracı için oturum açın.
`get-msoldomain` | Etki alanı adlarınızla geçerli Kiracı ile ilişkilendirilen gösterir.
`new-msoldomain –name <domainname>` | (Hiçbir DNS doğrulaması henüz gerçekleştirildikten) doğrulanmamış Kiracı etki alanı adını ekler.
`get-msoldomain` | Etki alanı adı artık, yönetilen bir kiracı ile ilişkilendirilen etki alanı adları listesi dahil, ancak olarak listelenen **doğrulanmamış**.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Etki alanı için yeni bir DNS TXT kayıt yerleştirmenin bilgileri sağlar (MS xxxxx =). Doğrulama değil sorun hemen yayılması, TXT kaydı için biraz zaman alır çünkü böylece olduğunu düşünmeden önce birkaç dakika bekleyip **- zorla devralma** seçeneği. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Etki alanı adınızı hala doğrulanamazsa, devam edebilirsiniz **- zorla devralma** seçeneği. TXT kaydı oluşturuldu ve kapatma devralma işlemini başlatıyor doğrular.<li>**- Zorla devralma** seçeneği, yalnızca yönetilmeyen Kiracı olduğunda Office 365 hizmetlerine devralma engelleme gibi bir dış yönetici devralma işlemini zorlama cmdlet'e eklenmelidir.
`get-msoldomain` | Etki alanı adı olarak etki alanı listesi gösterdiğini **doğrulandı**.

### <a name="powershell-example"></a>PowerShell örneği

1. Self Servis teklife yanıt vermek için kullanılan kimlik bilgilerini kullanarak Azure AD'ye bağlanın:
   ```powershell
    Install-Module -Name MSOnline
    $msolcred = get-credential
    
    connect-msolservice -credential $msolcred
   ```
2. Etki alanlarının bir listesini alın:
  
   ```powershell
    Get-MsolDomain
   ```
3. Bir challenge oluşturmak için Get-MsolDomainVerificationDns cmdlet'ini çalıştırın:
   ```powershell
    Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
  
    For example:
  
    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
   ```

4. Bu komuttan döndürülen değer (sınama) kopyalayın. Örneğin:
   ```powershell
    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
   ```
5. Genel DNS ad alanınız içinde önceki adımda kopyaladığınız değeri içeren bir DNS txt kaydı oluşturun. Bu kayıt için bir ad üst etki alanı adını, DNS rolü Windows Server'ı kullanarak bu kaynak kaydı oluşturun, böylece kayıt adı boş ve yalnızca Yapıştır değeri metin kutusuna bırakın.
6. Kimlik doğrulamak için Onayla-MsolDomain cmdlet'i çalıştırın:
  
   ```powershell
    Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*
   ```
  
   Örneğin:
  
   ```powershell
    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com
   ```

Başarılı bir challenge, istem olmadan bir hata döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel etki alanı adını Azure AD'ye ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
