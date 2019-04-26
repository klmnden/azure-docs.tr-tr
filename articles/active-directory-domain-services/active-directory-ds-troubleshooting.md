---
title: 'Azure Active Directory etki alanı Hizmetleri: Sorun giderme kılavuzu | Microsoft Docs'
description: Azure AD Domain Services için sorun giderme kılavuzu
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: ergreenl
ms.openlocfilehash: 48831767f72dd1b978fad5b0a9a8f2c7a11ec89d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60416390"
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD etki alanı Hizmetleri - sorun giderme kılavuzu
Bu makalede, ayarlama veya Azure Active Directory (AD) etki alanı Hizmetleri yönetme karşılaşabileceğiniz sorunları için sorun giderme ipuçları sağlar.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure AD dizininiz için Azure AD Domain Services etkinleştirilemiyor
Bu bölümde Azure AD Domain Services dizininiz için etkinleştirmeye çalıştığınızda hata gidermenize yardımcı olur.

Karşılaştığınız hata iletisine karşılık gelen sorun giderme adımları seçin.

| **Hata iletisi** | **Çözümleme** |
| --- |:--- |
| *Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.* |[Sanal ağdaki etki alanı adı çakışması](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.* |[Etki Alanı Hizmetleri, Azure AD Domain Services Sync uygulama için yeterli izinlere sahip değil](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Kiracınızda Domain Services uygulamasının düzgün şekilde yapılandırılmamış](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulama tanımlayıcısı 00000002-0000-0000-c000-000000000000 olan uygulamayı etkinleştirin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Azure AD kiracınızda Microsoft Graph uygulaması devre dışı bırakıldı](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Etki alanı adı çakışması
**Hata iletisi:**

*Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.*

**Düzeltme:**

Bu sanal ağda bulunan aynı etki alanı adına sahip mevcut bir etki alanına sahip değil emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Daha sonra bu sanal ağ üzerinde aynı etki alanı adı (diğer bir deyişle, "contoso.com") ile bir Azure AD Domain Services yönetilen etki alanında etkinleştirmeyi deneyin. Azure AD Domain Services'ı etkinleştirmeyi denediğinizde bir hatayla karşılaşırsınız.

Bu hata, bu sanal ağ üzerinde etki alanı adı için ad çakışmalarını kaynaklanır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

### <a name="inadequate-permissions"></a>Yetersiz izinler
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltme:**

' % S'adı 'Azure AD Domain Services Sync' Azure AD dizininizde sahip bir uygulama olup olmadığını denetleyin. Bu uygulama varsa silin ve Azure AD Domain Services'ı yeniden etkinleştirin.

Uygulama varsa uygulamanın varlığını denetlemek ve bunu silmek için aşağıdaki adımları gerçekleştirin:

1. Gidin **uygulamaları** Azure AD dizininizde bölümünü [Azure portalında](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/).
2. Seçin **tüm uygulamaları** içinde **Göster** açılır. Seçin **herhangi** içinde **uygulama durumu** açılır. Seçin **herhangi** içinde **uygulama görünürlüğü** açılır.
3. Tür **Azure AD Domain Services Sync** arama kutusuna. Uygulama zaten varsa üzerine tıklayarak ve tıklayın **Sil** silmek için araç çubuğu düğmesi.
4. Uygulama sildikten sonra Azure AD Domain Services yeniden etkinleştirmeyi deneyin.

### <a name="invalid-configuration"></a>Geçersiz yapılandırma
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltme:**

Azure AD dizininizde (ile bir uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan) ' AzureActiveDirectoryDomainControllerServices' adı ile bir uygulama olup olmadığını denetleyin. Bu uygulama varsa silin ve Azure AD Domain Services'ı yeniden etkinleştirmeniz gerekir.

Uygulama bulma ve silmek için aşağıdaki PowerShell betiğini kullanın.

> [!NOTE]
> Bu betiği kullanan **Azure AD PowerShell sürüm 2** cmdlet'leri. Tam bir liste tüm kullanılabilir cmdlet'lerin ve modül indirme için okuma [AzureAD PowerShell başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph devre dışı
**Hata iletisi:**

Etki Alanı Hizmetleri, bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulama tanımlayıcısı 00000002-0000-0000-c000-000000000000 olan uygulamayı etkinleştirin ve ardından Azure AD kiracınızda Domain Services'ı etkinleştirmeyi deneyin.

**Düzeltme:**

' % S'tanımlayıcısı 00000002-0000-0000-c000-000000000000 uygulamayla devre dışı bıraktıysanız denetleyin. Bu uygulama Microsoft Azure AD uygulaması ve Azure AD kiracınız ile Graph API'sine erişim sağlar. Azure AD etki alanı Hizmetleri, bu uygulama, yönetilen etki alanınızı Azure AD kiracınıza eşitlemek için etkinleştirilmesi gerekir.

Bu hatayı gidermek için bu uygulamayı etkinleştirin ve ardından Azure AD kiracınızda Domain Services'ı etkinleştirmeyi deneyin.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kullanıcılar Azure AD Domain Services yönetilen etki alanında oturum açamıyor
Azure AD kiracınızdaki kullanıcıların bir veya daha fazla yeni oluşturulan yönetilen etki alanında oturum açamıyor olması durumunda, aşağıdaki sorun giderme adımları uygulayın:

* **UPN biçimini kullanarak oturum açın:** SAMAccountName biçimi ('CONTOSO\joeuser') yerine UPN biçimini kullanarak oturum açmayı deneyin (örneğin, 'joeuser@contoso.com'). SAMAccountName, UPN önek aşırı uzun veya başka bir kullanıcı yönetilen etki alanında aynı kullanıcılar için otomatik olarak oluşturulabilir. UPN biçimini Azure AD kiracısı içinde benzersiz olması garanti edilir.

> [!NOTE]
> Azure AD Domain Services yönetilen etki alanında oturum açmak için UPN biçimini kullanmanızı öneririz.
>
>

* Başlarken kılavuzunda açıklanan adımlara uygun olarak [parola eşitlemesini etkinleştirdiğinizden](active-directory-ds-getting-started-password-sync.md) emin olun.
* **Dış hesapları:** Etkilenen kullanıcı hesabının, Azure AD kiracısında bir dış hesap olmadığından emin olun. Örnekler dış hesaplar Microsoft hesapları (örneğin, 'joe@live.com') veya bir dış kullanıcı hesapları Azure AD dizini. Azure AD Domain Services yok olduğundan bu tür kullanıcı hesapları için kimlik bilgilerini, bu kullanıcılar yönetilen etki alanında oturum açamaz.
* **Eşitlenen hesaplar:** Bir şirket içi dizinden etkilenen kullanıcı hesapları eşitlenmişse, aşağıdakileri doğrulayın:

  * Dağıtılan veya güncelleştirildi [en son sürüm, Azure AD Connect'in önerilen](https://www.microsoft.com/download/details.aspx?id=47594).
  * Azure AD Connect'e yapılandırdığınız [tam eşitleme gerçekleştir](active-directory-ds-getting-started-password-sync.md).
  * Dizininizin boyutuna bağlı olarak, bir kullanıcı hesapları için birkaç dakika sürebilir ve kimlik bilgisi karmalarının Azure AD Etki Alanı Hizmetleri'nde kullanılabilir olması. Yeterince uzun bir kimlik doğrulama yeniden denemeden önce beklenmesini emin olun.
  * Önceki adımlarda doğrulandıktan sonra sorun devam ederse, Microsoft Azure AD eşitleme hizmeti yeniden başlatmayı deneyin. Eşitleme makinenizden bir komut istemi başlatın ve aşağıdaki komutları yürütün:

    1. 'Microsoft Azure AD eşitleme' net stop
    2. 'Microsoft Azure AD eşitleme' net start
* **Yalnızca bulutta yer alan hesapları**: Etkilenen kullanıcı hesabının bir yalnızca bulut kullanıcı hesabıysa, Azure AD Domain Services'ı etkinleştirdikten sonra kullanıcının parolasını değiştirdiğinden emin olun. Bu adım, Azure AD Domain Services için gereken kimlik bilgisi karmalarının oluşturulmasına neden olur.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Yönetilen etki alanınızda bir veya daha fazla uyarı yok

Yönetilen etki alanınızda uyarılar ederek gidermek nasıl [sorun giderme uyarıları](active-directory-ds-troubleshoot-alerts.md) makalesi.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Azure AD kiracınızdan kaldırılan kullanıcılar yönetilen etki alanınızdan kaldırılmaz
Azure AD kullanıcı nesnelerinin yanlışlıkla silinmesine karşı sizi korur. Azure AD kiracınızdan bir kullanıcı hesabı sildiğinizde, buna karşılık gelen kullanıcı nesnesi Geri Dönüşüm Kutusu’na taşınır. Bu silme işlemi yönetilen Etki Alanınızla eşitlendiğinde, ilgili kullanıcı hesabının devre dışı olarak işaretlenmesine neden olur. Bu özellik, kurtarmak veya daha sonra kullanıcı hesabını silmeyi geri alma yardımcı olur.

Kullanıcı hesabının bir kullanıcı hesabı aynı UPN ile Azure AD dizininizde yeniden oluşturmanız bile yönetilen etki alanınıza, devre dışı durumda kalır. Kullanıcı hesabının yönetilen etki alanınızdan kaldırmak için Azure AD kiracınızdan onu zorla silinecek gerekir.

Kullanıcı, kullanıcı hesabının tam olarak yönetilen etki alanınızdan kaldırmak için Azure AD kiracınızdan kalıcı olarak sil. Kullanım `Remove-MsolUser` PowerShell cmdlet'iyle `-RemoveFromRecycleBin` seçeneği, bu konuda açıklandığı gibi [MSDN makalesi](/previous-versions/azure/dn194132(v=azure.100)).


## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
