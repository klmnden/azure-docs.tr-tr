---
title: "Azure AD Connect: Otomatik yükseltme | Microsoft Docs"
description: "Bu konu, yerleşik Otomatik yükseltme Özelliği Azure AD Connect eşitleme açıklar."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 894e387b4b83ed859139b4aecb3d8bb5df9ab56f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Otomatik yükseltme
Bu özellik, (Şubat 2016 yayımlandı) yapı ile 1.1.105.0 sunulmuştur.

## <a name="overview"></a>Genel Bakış
Azure AD Connect yüklemenize her zaman güncel olmasını sağlama hiçbir zaman ile daha kolay **otomatik yükseltme** özelliği. Bu özellik express yüklemeleri için varsayılan olarak etkindir ve DirSync yükseltir. Yeni bir sürümü yayımlandığında, yükleme otomatik olarak yükseltilir.
Otomatik yükseltme, aşağıdakiler için varsayılan olarak etkindir:

* Hızlı ayarlar yüklemesi ve DirSync yükseltmeleri.
* SQL Express LocalDB kullanımıyla, ne zaman hızlı ayarları kullanın olduğu. SQL Express ile DirSync de LocalDB kullanın.
* Hızlı ayarları ve DirSync tarafından oluşturulan varsayılan MSOL_ hesap AD hesabıdır.
* 100. 000'den az nesneler meta veri deposunda sahip.

Otomatik yükseltmeyi geçerli durumunu PowerShell cmdlet'iyle görüntülenebilir `Get-ADSyncAutoUpgrade`. Aşağıdaki durumlara sahiptir:

| Durum | Açıklama |
| --- | --- |
| Etkin |Otomatik yükseltmeyi etkinleştirilir. |
| askıya alındı |Yalnızca sistem tarafından ayarlayın. Sistem artık otomatik güncelleştirmeleri almak uygun değil. |
| Devre dışı |Otomatik yükseltmeyi devre dışı bırakılır. |

Arasında değiştirebileceğiniz **etkin** ve **devre dışı** ile `Set-ADSyncAutoUpgrade`. Sistem durumu ayarlamalısınız yalnızca **askıya**.

Otomatik yükseltmeyi Azure AD Connect Health Yükseltme Altyapısı kullanıyor. İş, emin olmak otomatik yükseltme için proxy sunucusu URL'leri açtığınız **Azure AD Connect Health** açıklandığı gibi [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).


Varsa **Eşitleme Hizmeti Yöneticisi'ni** kullanıcı Arabirimi, sunucu üzerinde çalıştığından ve ardından yükseltme UI kapatılana kadar bekletilir.

## <a name="troubleshooting"></a>Sorun giderme
Connect yüklemenizi kendisini beklendiği gibi değil yükseltirseniz, nerede olabilir çıkışı bulmak için aşağıdaki adımları izleyin.

İlk olarak, yeni bir sürüm yayımlanan ilk günü denenmesi için otomatik yükseltmeyi beklememeniz gerekir. Bir yükseltme yüklemenizi hemen yükselttiyseniz değil uyarıda olmayan şekilde denenmeden önce kasıtlı rastgele yoktur.

Bir şey değil sağ düşünüyorsanız, önce Çalıştır `Get-ADSyncAutoUpgrade` otomatik yükseltme etkinse emin olmak için.

Ardından, gerekli URL'lerin proxy ya da güvenlik duvarı açtığınızdan emin olun. Otomatik güncelleştirme açıklandığı gibi Azure AD Connect Health kullanarak [genel bakış](#overview). Bir proxy kullanıyorsanız, sistem durumu, kullanacak şekilde yapılandırıldığından emin olun bir [proxy sunucusu](../connect-health/active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ayrıca test [sistem durumu bağlantı](../connect-health/active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) Azure ad.

Azure ad doğrulandı bağlanabilirlik, eventlogs aramak için saattir. Olay Görüntüleyicisi'ni başlatın ve konum **uygulama** eventlog. Kaynağı için bir olay günlüğüne filtre ekleme **Azure AD Connect Yükseltmesi** ve olay kimliği aralığını **300 399**.  
![Otomatik yükseltme için olay günlüğü filtresi](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Otomatik yükseltmeyi durumuyla ilişkili eventlogs şimdi görebilirsiniz.  
![Otomatik yükseltme için olay günlüğü filtresi](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Sonuç kodu durumu genel bir bakış önekiyle sahiptir.

| Sonuç kodu öneki | Açıklama |
| --- | --- |
| Başarılı |Yükleme başarıyla yükseltildi. |
| UpgradeAborted |Geçici bir durum yükseltme durduruldu. Yeniden denenecek ve daha sonra başarılı beklenir. |
| UpgradeNotSupported |Sistem otomatik olarak yükseltilecek sistemin engelleyen bir yapılandırmaya sahip. Durum değiştirme ancak sistem el ile yükseltilmesi gereken beklenir görmek için denenecek. |

Burada, bulduğunuz en yaygın iletilerinin bir listesi verilmiştir. Tüm listelenmez, ancak sonuç iletisi temizleyin hangi sorun mu var olmalıdır.

| Sonuç iletisi | Açıklama |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Kayıt defterine yazılamadı. |
| UpgradeAbortedInsufficientDatabasePermissions |Yerleşik Yöneticiler grubunun veritabanı izinleri yok. El ile bu sorunu gidermek için Azure AD Connect'in en son sürüme yükseltin. |
| UpgradeAbortedInsufficientDiskSpace |Bir yükseltme desteklemek için yeterli disk alanı yok. |
| UpgradeAbortedSecurityGroupsNotPresent |Sağlanamadı bulmak ve eşitleme altyapısı tarafından kullanılan tüm güvenlik gruplarını çözün. |
| UpgradeAbortedServiceCanNotBeStarted |NT hizmeti **Microsoft Azure AD eşitleme** başlatılamadı. |
| UpgradeAbortedServiceCanNotBeStopped |NT hizmeti **Microsoft Azure AD eşitleme** durdurulamadı. |
| UpgradeAbortedServiceIsNotRunning |NT hizmeti **Microsoft Azure AD eşitleme** çalışmıyor. |
| UpgradeAbortedSyncCycleDisabled |SyncCycle seçeneğinde [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md) devre dışı bırakıldı. |
| UpgradeAbortedSyncExeInUse |[Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi](active-directory-aadconnectsync-service-manager-ui.md) sunucuda açılır. |
| UpgradeAbortedSyncOrConfigurationInProgress |Yükleme Sihirbazı'nı çalıştıran veya eşitleme Zamanlayıcı dışında zamanlanabilir. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedAdfsSignInMethod | Adfs oturum açma yöntemi olarak seçtiniz. | 
| UpgradeNotSupportedCustomizedSyncRules |Yapılandırma için kendi özel kurallarınızı eklediniz. |
| UpgradeNotSupportedDeviceWritebackEnabled |Etkinleştirdiğiniz [cihaz geri yazma](active-directory-aadconnect-feature-device-writeback.md) özelliği. |
| UpgradeNotSupportedGroupWritebackEnabled |Etkinleştirdiğiniz [grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback) özelliği. |
| UpgradeNotSupportedInvalidPersistedState |Yükleme, bir hızlı ayarları veya DirSync yükseltme değil. |
| UpgradeNotSupportedMetaverseSizeExceeeded |Meta veri deposunda 100. 000'den fazla nesneler var. |
| UpgradeNotSupportedMultiForestSetup |Birden çok ormana bağlanırsınız. Hızlı Kurulum yalnızca bir ormana bağlanır. |
| UpgradeNotSupportedNonLocalDbInstall |Bir SQL Server Express LocalDB veritabanına kullanmıyorsunuz. |
| UpgradeNotSupportedNonMsolAccount |[AD Bağlayıcısı hesabı](active-directory-aadconnect-accounts-permissions.md#active-directory-account) varsayılan MSOL_ hesabı artık değil. |
| UpgradeNotSupportedNotConfiguredSignInMethod | AAD bağlanma ayarlarken, seçtiğiniz *yapılandırmazsanız* oturum açma yöntemi seçerken. | 
| UpgradeNotSupportedPtaSignInMethod | Doğrudan kimlik doğrulaması oturum açma yöntemi olarak seçtiniz. |
| UpgradeNotSupportedStagingModeEnabled |Sunucu içinde olacak şekilde ayarlanmıştır [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |Etkinleştirdiğiniz [kullanıcı geri yazma](active-directory-aadconnect-feature-preview.md#user-writeback) özelliği. |

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
