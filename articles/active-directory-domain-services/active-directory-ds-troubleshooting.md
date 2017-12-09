---
title: "Azure Active Directory etki alanı Hizmetleri: Sorun giderme kılavuzu | Microsoft Docs"
description: "Azure AD etki alanı Hizmetleri için sorun giderme kılavuzu"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: maheshu
ms.openlocfilehash: 5b094ab27d9d11828b0818a6024ff9b108d6cddb
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD etki alanı Hizmetleri - sorun giderme kılavuzu
Bu makale, ayarlama veya Azure Active Directory (AD) etki alanı Hizmetleri yönetme karşılaşabileceğiniz sorunları için sorun giderme ipuçları sağlar.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure AD dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştirilemiyor
Bu bölümde, dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştirmeye çalıştığınızda hatalarında sorun giderme yardımcı olur.

Karşılaştığınız hata iletisine karşılık gelen sorun giderme adımları seçin.

| **Hata iletisi** | **Çözümleme** |
| --- |:--- |
| *Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.* |[Sanal ağda etki alanı adı çakışması](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.* |[Etki Alanı Hizmetleri, Azure AD etki alanı Hizmetleri eşitleme uygulama için yeterli izinlere sahip değil](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Etki Alanı Hizmetleri uygulaması kiracınızda düzgün yapılandırılmamış](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulama tanımlayıcısı 00000002-0000-0000-c000-000000000000 olan uygulamayı etkinleştirin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Microsoft Graph uygulaması, Azure AD kiracınızda devre dışı bırakıldı](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Etki alanı adı çakışması
**Hata iletisi:**

*Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.*

**Düzeltme:**

Bu sanal ağda kullanılabilir aynı etki alanı adına sahip mevcut bir etki alanına sahip değil emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Daha sonra bu sanal ağ üzerinde aynı etki alanı adı (diğer bir deyişle, "contoso.com") içeren bir Azure AD etki alanı Hizmetleri yönetilen etki alanı etkinleştirmeyi deneyin. Azure AD Etki Alanı Hizmetleri'ni etkinleştirmeye çalışırken bir hatayla karşılaşırsınız.

Bu sanal ağ üzerinde etki alanı adı için ad çakışmaları nedeniyle bu hatasıdır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

### <a name="inadequate-permissions"></a>Yetersiz izinler
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltme:**

Azure AD dizininizi ' Azure AD etki alanı Hizmetleri Sync' adlı bir uygulama olup olmadığını denetleyin. Bu uygulama zaten varsa, onu silin ve Azure AD etki alanı hizmetlerini yeniden etkinleştirin.

Uygulama zaten varsa uygulama olup olmadığını denetlemek için ve bunu silmek için aşağıdaki adımları gerçekleştirin:

1. Gidin **uygulamaları** Azure AD dizininizi bölümünü [Azure portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/).
2. Seçin **tüm uygulamaları** içinde **Göster** açılır. Seçin **herhangi** içinde **uygulamaları durum** açılır. Seçin **herhangi** içinde **uygulama görünürlük** açılır.
3. Tür **Azure AD etki alanı Hizmetleri eşitleme** arama kutusuna. Uygulama zaten varsa, tıklayın ve tıklayın **silmek** silmek için araç çubuğu düğmesi.
4. Uygulama sildikten sonra Azure AD etki alanı Hizmetleri kez yeniden etkinleştirmeyi deneyin.

### <a name="invalid-configuration"></a>Geçersiz yapılandırma
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltme:**

Azure AD dizininizi (ile bir d87dcbc6-a371-462e-88e3-28ad15ec4e64 uygulama tanıtıcısı) ' AzureActiveDirectoryDomainControllerServices' adlı bir uygulama olup olmadığını denetleyin. Bu uygulama zaten varsa, onu silin ve ardından Azure AD etki alanı hizmetleri yeniden etkinleştirmeniz gerekir.

Uygulamayı bulun ve silmek için aşağıdaki PowerShell betiğini kullanın.

> [!NOTE]
> Bu komut dosyası kullanan **Azure AD PowerShell sürüm 2** cmdlet'leri. Tam bir liste tüm kullanılabilir cmdlet'leri ve modül indirme için okuma [Azuread'i PowerShell başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt757189.aspx).
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

Etki Alanı Hizmetleri bu Azure AD kiracısında etkin değil. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulama tanımlayıcısı 00000002-0000-0000-c000-000000000000 uygulamayla etkinleştirin ve sonra etki alanı Hizmetleri, Azure AD kiracınızın etkinleştirmeyi deneyin.

**Düzeltme:**

Bir uygulama 00000002-0000-0000-c000-000000000000 tanımlayıcıyla devre dışı bıraktıysanız denetleyin. Bu uygulama Microsoft Azure AD uygulaması ve Azure AD kiracınıza grafik API'sine erişim sağlar. Azure AD etki alanı Hizmetleri, bu uygulama yönetilen etki alanınız için Azure AD kiracınıza eşitlemek için etkinleştirilmesi gerekir.

Bu hatayı gidermek için bu uygulamayı etkinleştir ve Azure AD kiracınız için etki alanı Hizmetleri'ni etkinleştirmeyi deneyin.

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kullanıcılar Azure AD Domain Services yönetilen etki alanında oturum açamıyor
Bir veya daha fazla kullanıcı Azure AD kiracınızda yeni oluşturulan yönetilen etki alanında oturum açmak erişemiyorsanız aşağıdaki sorun giderme adımları gerçekleştirin:

* **UPN biçimini kullanarak oturum açın:** UPN biçimini kullanarak oturum deneyin (örneğin, 'joeuser@contoso.com') yerine SAMAccountName biçimi ('CONTOSO\joeuser'). SAMAccountName, UPN önek aşırı uzun veya başka bir kullanıcı yönetilen etki alanı ile aynı olan kullanıcılar için otomatik olarak oluşturulabilir. UPN biçimini Azure AD kiracısı içinde benzersiz olması garanti edilmez.

> [!NOTE]
> Azure AD etki alanı Hizmetleri yönetilen etki alanına oturum açmak için UPN biçimini kullanmanızı öneririz.
>
>

* Başlarken kılavuzunda açıklanan adımlara uygun olarak [parola eşitlemesini etkinleştirdiğinizden](active-directory-ds-getting-started-password-sync.md) emin olun.
* **Dış hesaplar:** etkilenen kullanıcı hesabını ve Azure AD kiracısı içinde dış bir hesap olmadığından emin olun. Dış hesaplar örnekleri arasında Microsoft hesapları (örneğin, 'joe@live.com') veya bir dış kullanıcı hesapları Azure AD dizini. Bu tür kullanıcı hesapları için kimlik bilgileri Azure AD etki alanı Hizmetleri yok olduğundan, bu kullanıcılar yönetilen etki alanına oturum açamaz.
* **Eşitlenen hesaplar:** bir şirket içi Directory'den eşitlenen etkilenen kullanıcı hesapları, doğrulayın:

  * Dağıtılan veya güncelleştirildi [en son sürümü Azure AD Connect, önerilen](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * Azure AD Connect'e yapılandırdığınız [tam eşitleme gerçekleştirmek](active-directory-ds-getting-started-password-sync.md).
  * Dizininizin boyutuna bağlı olarak, bu kullanıcı hesapları için uzun sürebilir ve Azure AD Etki Alanı Hizmetleri'nde kullanılabilir olması için karma kimlik bilgisi. Kimlik doğrulama yetecek kadar uzun süre denemeden önce bekleyin emin olun.
  * Yukarıdaki adımları doğruladıktan sonra sorun devam ederse, Microsoft Azure AD eşitleme hizmetini yeniden başlatmayı deneyin. Eşitleme makinenizden bir komut istemi başlatın ve aşağıdaki komutları çalıştırın:

    1. net stop 'Microsoft Azure AD eşitleme'
    2. net start 'Microsoft Azure AD eşitleme'
* **Yalnızca bulut hesapları**: etkilenen kullanıcı hesabının bir yalnızca bulut kullanıcı hesabı, Azure AD etki alanı Hizmetleri etkin sonra kullanıcı parolalarını değiştirdi emin olun. Bu adım, Azure AD Domain Services için gereken kimlik bilgisi karmalarının oluşturulmasına neden olur.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Kullanıcı Azure AD kiracınıza kaldırıldı, yönetilen etki alanınızdan kaldırılmaz
Azure AD kullanıcı nesnelerinin yanlışlıkla silinmesine karşı sizi korur. Azure AD kiracınızdan bir kullanıcı hesabı sildiğinizde, buna karşılık gelen kullanıcı nesnesi Geri Dönüşüm Kutusu’na taşınır. Bu silme işlemi, yönetilen etki alanınıza eşitlendiğinde, karşılık gelen kullanıcı hesabı devre dışı olarak işaretlenmiş neden olur. Bu özellik, kurtarabilir veya daha sonra kullanıcı hesabını silmeyi geri al yardımcı olur.

Bir kullanıcı hesabıyla aynı UPN Azure AD dizininizi yeniden oluşturursanız bile, kullanıcı hesabının, yönetilen etki alanınız devre dışı durumda kalır. Kullanıcı hesabının, yönetilen etki alanından kaldırmak için zorla Azure AD kiracınıza silmeniz gerekir.

Kullanıcı, kullanıcı hesabına tam olarak yönetilen etki alanından kaldırmak için Azure AD kiracınıza kalıcı olarak sil. Kullanım `Remove-MsolUser` PowerShell cmdlet'iyle `-RemoveFromRecycleBin` seçeneği, bu konuda açıklandığı gibi [MSDN makalesine](https://msdn.microsoft.com/library/azure/dn194132.aspx).

## <a name="contact-us"></a>Bizimle İletişim Kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
