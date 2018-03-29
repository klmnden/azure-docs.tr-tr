---
title: Azure AD Connect eşitleme hizmeti özelliklerini ve yapılandırma | Microsoft Docs
description: Azure AD Connect eşitleme hizmeti için hizmet tarafı özelliklerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 534e4e6d8b6ea2bfc059383e3e55c0352678ee04
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect eşitleme hizmeti özellikleri
Azure AD Connect eşitleme özelliği iki bileşenden oluşur:

* Şirket içi bileşeninizin **Azure AD Connect eşitleme**, olarak da bilinir **eşitleme altyapısı**.
* Azure AD'de olarak da bilinen bulunan hizmet **Azure AD Connect eşitleme hizmeti**

Bu konuda açıklanmaktadır nasıl özelliklerini aşağıdaki **Azure AD Connect eşitleme hizmeti** çalışma ve bunları Windows PowerShell kullanarak nasıl yapılandırabilirsiniz.

Bu ayarlar ile yapılandırılan [Azure Active Directory için Windows PowerShell Modülü](https://aka.ms/aadposh). İndirin ve Azure AD Connect'ten ayrı olarak yükleyin. Bu konudaki belgelenen cmdlet'leri de tanıtılan [2016 Mart sürümünden (yapı 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Bu konudaki belgelenen cmdlet'leri yok veya aynı sonucu verir değil, en son sürümü çalıştırdığınızdan emin olun.

Azure AD dizininizi yapılandırmasında görmek için çalıştırın `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures result](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Bu ayarların çoğu, yalnızca Azure AD Connect tarafından değiştirilebilir.

Aşağıdaki ayarları tarafından yapılandırılabilir `Set-MsolDirSyncFeature`:

| DirSyncFeature | Açıklama |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Birincil SMTP adresi yanı sıra userPrincipalName katılmak nesneleri sağlar. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |UserPrincipalName özniteliğinin (Federasyon olmayan) kullanıcıların yönetilen ve lisanslı güncelleştirmek eşitleme altyapısı sağlar. |

Bir özellik etkinleştirdikten sonra onu yeniden devre dışı bırakılamaz.

> [!NOTE]
> 24 Ağustos 2016'dan özelliği *yinelenen öznitelik dayanıklılık* yeni Azure için varsayılan olarak etkin AD dizinleri. Bu özellik ayrıca alındı ve olması bu tarihten önce oluşturulan dizinleri etkin. Bu özellik hakkında bilgi almak için dizininizin olduğunda, bir e-posta bildirimi alırsınız.
> 
> 

Aşağıdaki ayarları Azure AD Connect tarafından yapılandırılır ve tarafından değiştirilemez `Set-MsolDirSyncFeature`:

| DirSyncFeature | Açıklama |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: cihaz geri yazma özelliğini etkinleştirme](active-directory-aadconnect-feature-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect eşitleme: dizin uzantıları](active-directory-aadconnectsync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Dışa aktarma sırasında başarısız olan nesnenin tamamı yerine başka bir nesnenin yinelemesi karantinaya için bir öznitelik sağlar. |
| PasswordSync |[Parola Eşitleme ile Azure AD Connect eşitleme uygulama](active-directory-aadconnectsync-implement-password-hash-synchronization.md) |
| UnifiedGroupWriteback |[Önizleme: Grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback) |
| UserWriteback |Şu anda desteklenmiyor. |

## <a name="duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılık
Sağlama başarısız yerine ile yinelenen UPN nesneleri / proxyAddresses, yinelenen öznitelik "karantinaya" ve geçici bir değeri atanır. Çakışma giderildikten sonra geçici UPN doğru değer otomatik olarak değiştirilir. Daha fazla ayrıntı için bkz: [kimlik eşitleme ve yinelenen öznitelik dayanıklılık](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName yumuşak eşleşmiyor
Bu özellik etkinleştirildiğinde, soft-match için ek olarak UPN etkin [birincil SMTP adresi](https://support.microsoft.com/kb/2641663), her zaman etkin. Soft-match, şirket içi kullanıcıları Azure AD'de mevcut bulut kullanıcıları eşlemek için kullanılır.

Gerekirse eşleşme AD hesaplarıyla bulutta oluşturulan mevcut şirket içi ve Exchange Online kullanmıyorsanız, sonra bu özellik yararlı olur. Bu senaryoda, genel bulutta SMTP özniteliğini ayarlamak için bir neden yoktur.

Bu özellik için varsayılan olarak yeni Azure AD dizinlerinden oluşturulur. Bu özellik, çalıştırarak etkin olup olmadığını görebilirsiniz:  

```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Bu özellik, Azure AD dizini için etkin değilse, ardından onu çalıştırarak etkinleştirebilirsiniz:  

```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName güncelleştirmelerini eşitleme
Tarihsel olarak, şirket içi eşitleme hizmetini kullanmayı UserPrincipalName özniteliği güncelleştirmeleri engellendi, bu koşulların her ikisi de doğruysa sürece:

* Kullanıcı (Federasyon olmayan) yönetilir.
* Kullanıcı bir lisans atanmadı.

Daha fazla ayrıntı için bkz: [Office 365, Azure veya Intune kullanıcı adları şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor](https://support.microsoft.com/kb/2523192).

Bu özelliği etkinleştirmek, userPrincipalName içi değiştirilen olduğunda ve Parola Eşitleme'yi kullanmaya güncelleştirmek eşitleme altyapısı sağlar. Federasyon kullanırsanız, bu özellik desteklenmiyor.

Bu özellik için varsayılan olarak yeni Azure AD dizinlerinden oluşturulur. Bu özellik, çalıştırarak etkin olup olmadığını görebilirsiniz:  

```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Bu özellik, Azure AD dizini için etkin değilse, ardından onu çalıştırarak etkinleştirebilirsiniz:  

```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Bu özellik etkinleştirildikten sonra varolan userPrincipalName değerleri olarak kalır-değil. UserPrincipalName özniteliği içi sonraki değişiminde kullanıcıları normal delta eşitleme UPN güncelleştirir.  

## <a name="see-also"></a>Ayrıca bkz.
* [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

