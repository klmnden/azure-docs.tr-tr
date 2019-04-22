---
title: Azure AD Connect eşitleme hizmeti özelliklerini ve yapılandırma | Microsoft Docs
description: Azure AD Connect eşitleme hizmeti için hizmet tarafı özelliklerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 213aab20-0a61-434a-9545-c4637628da81
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: be67a6f287e2d6e77070928cbe12542857696011
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680290"
---
# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect eşitleme hizmeti özellikleri

Azure AD Connect eşitleme özelliği iki bileşenden oluşur:

* Şirket içi bileşeninizin **Azure AD Connect eşitleme**ayrıca adlı **eşitleme altyapısı**.
* Hizmet olarak da bilinen Azure AD'de bulunan **Azure AD Connect eşitleme hizmeti**

Bu konu açıklar nasıl özelliklerini aşağıdaki **Azure AD Connect eşitleme hizmeti** iş ve nasıl Windows PowerShell kullanarak yapılandırmak için.

Bu ayarları tarafından yapılandırılan [Azure Active Directory için Windows PowerShell Modülü](https://aka.ms/aadposh). İndirin ve ayrı ayrı Azure AD Connect'ten yükleyin. Bu konudaki belgelenen cmdlet'leri sunulan [2016 Mart sürüm (derleme 9031.1)](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Bu konudaki belgelenen cmdlet'leri izniniz yok veya aynı sonucu üretir değil, en son sürümü çalıştırdığınızdan emin olun.

Azure AD dizininizi yapılandırmada görmek için şunu çalıştırın `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures sonucu](./media/how-to-connect-syncservice-features/getmsoldirsyncfeatures.png)

Bu ayarların çoğu, yalnızca Azure AD Connect tarafından değiştirilebilir.

Aşağıdaki ayarları tarafından yapılandırılabilir `Set-MsolDirSyncFeature`:

| DirSyncFeature | Açıklama |
| --- | --- |
| [EnableSoftMatchOnUpn](#userprincipalname-soft-match) |Birincil SMTP adresi yanı sıra userPrincipalName katılın nesnelerin sağlar. |
| [SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) |UserPrincipalName özniteliği (Federasyon olmayan) kullanıcıların yönetilen ve lisanslı güncelleştirmek eşitleme altyapısı sağlar. |

Bir özelliği etkinleştirdikten sonra onu tekrar devre dışı bırakılamaz.

> [!NOTE]
> 24 Ağustos 2016'dan özellik *yinelenen öznitelik dayanıklılığı* varsayılan olarak etkin için yeni Azure AD dizinleri. Bu özellik ayrıca piyasaya sunuluyor ve bu tarihten önce oluşturulan dizinleri etkin. Bu özellik hakkında bilgi almak için dizininiz olduğunda bir e-posta bildirimi alırsınız.
> 
> 

Aşağıdaki ayarlar, Azure AD Connect tarafından yapılandırılır ve tarafından değiştirilemez `Set-MsolDirSyncFeature`:

| DirSyncFeature | Açıklama |
| --- | --- |
| DeviceWriteback |[Azure AD Connect: Cihaz geri yazma özelliğini etkinleştirme](how-to-connect-device-writeback.md) |
| DirectoryExtensions |[Azure AD Connect eşitlemesi: Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) |
| [DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) |Bir öznitelik bir kopyasını başka bir nesne yerine, tüm nesne dışarı aktarma sırasında başarısız olduğunda karantinaya sağlar. |
| Parola Karması Eşitleme |[Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](how-to-connect-password-hash-synchronization.md) |
|Doğrudan Kimlik Doğrulama|[Azure Active Directory Geçişli Kimlik Doğrulaması ile kullanıcı oturumu açma](how-to-connect-pta.md)|
| UnifiedGroupWriteback |[Önizleme: Grup geri yazma](how-to-connect-preview.md#group-writeback) |
| UserWriteback |Şu anda desteklenmiyor. |

## <a name="duplicate-attribute-resiliency"></a>Yinelenen öznitelik dayanıklılığı

Sağlama başarısız olmak yerine yinelenen UPN ile nesneler / proxyAddresses, "Yinelenen öznitelik karantinaya alındı" ve geçici bir değer atanır. Geçici UPN çakışma giderildikten sonra uygun değere otomatik olarak değiştirilir. Daha fazla ayrıntı için [kimlik eşitleme ve yinelenen öznitelik dayanıklılığı](how-to-connect-syncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName yazılım eşleştir

Bu özellik etkinleştirildiğinde, geçici eşleştirme için ek olarak UPN etkin [birincil SMTP adresi](https://support.microsoft.com/kb/2641663), her zaman etkin olan. Geçici eşleştirme, var olan bulut kullanıcıları, şirket içi kullanıcıları Azure AD'de eşlemek için kullanılır.

Gerekirse eşleşme AD hesaplarıyla bulutta oluşturulan mevcut şirket ve Exchange Online kullanmıyorsanız, ardından bu özellik yararlıdır. Bu senaryoda, genel bulutta SMTP öznitelik ayarlamak için bir neden yoktur.

Bu özellik için varsayılan olarak yeni Azure AD dizini oluşturulur. Bu özellik, çalıştırarak etkin olup olmadığını görebilirsiniz:  

```powershell
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Ardından Azure AD dizininiz için bu özellik etkin değilse çalıştırarak etkinleştirebilirsiniz:  

```powershell
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName güncelleştirmelerini eşitleme

Tarihsel olarak, güncelleştirmeler için eşitleme hizmeti aracılığıyla şirket içi UserPrincipalName özniteliği engellendi, bu koşulların her ikisinin de doğru olduğu sürece:

* Kullanıcı (Federasyon olmayan) yönetilir.
* Kullanıcı bir lisans atanmadı.

Daha fazla ayrıntı için [Office 365, Azure veya Intune'daki kullanıcı adları, şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor](https://support.microsoft.com/kb/2523192).

Bu özelliği etkinleştirmek, şirket değiştirilmiş olduğundan ve parola karması eşitleme veya doğrudan kimlik doğrulaması kullandığınızda userPrincipalName güncelleştirmek eşitleme altyapısı sağlar. Federasyon kullanıyorsanız, bu özellik desteklenmiyor.

Bu özellik için varsayılan olarak yeni Azure AD dizini oluşturulur. Bu özellik, çalıştırarak etkin olup olmadığını görebilirsiniz:  

```powershell
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Ardından Azure AD dizininiz için bu özellik etkin değilse çalıştırarak etkinleştirebilirsiniz:  

```powershell
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Bu özellik etkinleştirildikten sonra mevcut userPrincipalName değerleri olarak kalacak-olduğu. UserPrincipalName özniteliği şirket sonraki değişiminde normal delta eşitleme kullanıcıların UPN güncelleştirir.  

## <a name="see-also"></a>Ayrıca bkz.

* [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
