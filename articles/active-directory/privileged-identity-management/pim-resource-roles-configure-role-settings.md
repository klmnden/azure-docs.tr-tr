---
title: PIM - Azure Active Directory Azure kaynak rol ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rol ayarlarını yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73d42c693fae6b538136d1e8c93094a0ea9e2077
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59494876"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>PIM'de Azure kaynak rol ayarlarını yapılandırma

Azure kaynak rol ayarlarını yapılandırdığınızda, Azure kaynak rol atamaları Azure Active Directory (Azure AD) Privileged Identity Management (PIM) içinde uygulanan varsayılan ayarları tanımlar. Onay iş akışını yapılandırın ve kimler onaylayabilir veya istekleri reddetme belirtmek için aşağıdaki yordamları kullanın.

## <a name="open-role-settings"></a>Rol ayarlarını Aç

Bir Azure Kaynak rolü ayarlarını açmak için şu adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Bir abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağa tıklayın.

    ![Azure kaynaklarını yönetmek için listesi](./media/pim-resource-roles-configure-role-settings/resources-list.png)

1. Tıklayın **rol ayarları**.

    ![Rol ayarları](./media/pim-resource-roles-configure-role-settings/resources-role-settings.png)

1. Ayarları yapılandırmak istediğiniz rolüne tıklayın.

    ![Rol ayarı ayrıntıları](./media/pim-resource-roles-configure-role-settings/resources-role-setting-details.png)

1. Tıklayın **Düzenle** rol ayarlar bölmesini açın.

    ![Rol ayarlarını Düzenle](./media/pim-resource-roles-configure-role-settings/resources-role-settings-edit.png)

    Rol ayarı bölmesinde her rol için yapılandırabileceğiniz birkaç ayar vardır.

## <a name="assignment-duration"></a>Atama süresi

Rol ayarlarını yapılandırırken (uygun ve etkin) her bir atama türü için iki atama süresi seçenekleri arasından seçim yapabilirsiniz. Bu seçenekler, PIM rolüne bir üye atandığında varsayılan en uzun süresi olur.

Bunlardan birini seçebileceğiniz **uygun** atama süresi seçenekleri:

| | |
| --- | --- |
| **Kalıcı uygun atamaya izin ver** | Kalıcı uygun üyelik kaynak yöneticileri atayabilirsiniz. |
| **Uygun atamayı sonra süresi dolacak** | Kaynak yöneticileri, tüm uygun atamalar belirtilen başlangıç ve bitiş tarihi olmasını isteyebilir. |

Ve bunlardan birini seçebileceğiniz **etkin** atama süresi seçenekleri:

| | |
| --- | --- |
| **Kalıcı etkin atamaya izin ver** | Kalıcı etkin üyeliğini kaynak yöneticileri atayabilirsiniz. |
| **Etkin atamada sonra süresi dolacak** | Kaynak yöneticileri, tüm etkin atamalar belirtilen başlangıç ve bitiş tarihi olmasını isteyebilir. |

> [!NOTE] 
> Kaynak yöneticileri tarafından belirtilen son tarihe sahip tüm atamaları yenilenebilir. Ayrıca, üyeleri Self Servis isteklerine başlatabilirsiniz [genişletin veya rol atamalarını yenileme](pim-resource-roles-renew-extend.md).

## <a name="require-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması gerektir

PIM iki farklı senaryolar için isteğe bağlı zorlama Azure multi-Factor Authentication (MFA) sağlar.

### <a name="require-multi-factor-authentication-on-active-assignment"></a>Etkin atamada Multi-Factor Authentication iste

Bazı durumlarda (örneğin bir gün) kısa bir süre için bir role üye atamak isteyebilirsiniz. Bu durumda, etkinleştirme isteği atanan üyelerini gerek yoktur. Üye kendi rol ataması kullandığında zaten atanmış olan şu rolden etkin olduğu Bu senaryoda, MFA PIM zorunlu kılamaz.

Atama yerine getirmesini Kaynak Yöneticisi kimin söyledikleri olduğundan emin olmak için MFA etkin atamada denetleyerek zorunlu kılabilir **etkin atamada multi-Factor Authentication iste** kutusu.

### <a name="require-multi-factor-authentication-on-activation"></a>Etkinleştirme yapılırken Multi-Factor Authentication'ı gerekli kılın

MFA etkinleştirilebilmesi çalıştırmak uygun rolü üyelerinin gerektirebilir. Bu işlem, etkinleştirme kim makul kesin olarak söyledikleri isteyen kullanıcı sağlar. Kullanıcı hesabının tehlikede, bu seçenek zorlamayı durumlarda kritik kaynaklara korur.

Mfa'yı etkinleştirme tamamlanmadan önce çalıştırmak uygun bir üye gerektirecek şekilde kontrol **etkinleştirme multi-Factor Authentication iste** kutusu.

Daha fazla bilgi için [multi factor authentication (MFA) ve PIM](pim-how-to-require-mfa.md).

## <a name="activation-maximum-duration"></a>Maksimum etkinlik süresi

Kullanım **maksimum etkinleştirme süresi** en uzun süreyi saat cinsinden süresi dolmadan önce rol etkin kaldığından ayarlamak için kaydırıcıyı. Bu değer 1 ile 24 saat arasında olabilir.

## <a name="require-justification"></a>Gerekçelendirme iste

Üyeleri bir gerekçe etkin atamada veya ne zaman etkinleştirdikleri girin gerektirebilir. Gerekçelendirme iste denetleyin **etkin atamada gerekçelendirme iste** kutusu veya **etkinleştirmede gerekçelendirme iste** kutusu.

## <a name="require-approval-to-activate"></a>Etkinleştirmek için onay gerektir

Bir rolü etkinleştirmek için onay gerektir istiyorsanız, aşağıdaki adımları izleyin.

1. Denetleme **etkinleştirmek için onay gerektir** onay kutusu.

1. Tıklayın **onaylayanları seçin** Seç üyelerinin veya grubun bir bölme açmak için.

    ![Üye veya grup seçin](./media/pim-resource-roles-configure-role-settings/resources-role-settings-select-approvers.png)

1. En az bir üye veya grup seçin ve ardından **seçin**. Herhangi bir birleşimini üyeleri ve grupları ekleyebilirsiniz. En az bir onaylayan seçmeniz gerekir. Hiçbir varsayılan onaylayanlar vardır.

    Seçimlerinizi seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirledikten sonra tıklayın **güncelleştirme** yaptığınız değişiklikleri kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)
