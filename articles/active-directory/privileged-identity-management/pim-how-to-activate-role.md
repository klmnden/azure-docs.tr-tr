---
title: PIM - Azure Active Directory Azure AD'ye rollerimi etkinleştir | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure AD rolleri etkinleştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa820d6c140251fce6b09110e65b45005b53afcc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60289659"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>PIM Azure AD'ye rollerimi etkinleştir

Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kuruluşlara ayrıcalıklı Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetme sürecini basitleştirir.  

Bir yönetici rolü için uygun yapılmadıysa, ayrıcalıklı eylemler gerçekleştirmek, ihtiyacınız olduğunda bu role aktive edebileceğiniz anlamına gelir. Office 365 özellikleri bazen yönetiyorsanız, bu rol diğer hizmetleri çok etkiler beri Örneğin, kuruluşunuzun ayrıcalıklı rol yöneticileri, kalıcı bir genel yönetici sunamazsınız. Bunun yerine, bunlar, Exchange Online yönetici gibi Azure AD rolleri için uygun yapın. Kendi ayrıcalıklarına sahip olmanız ve ardından önceden belirlenmiş bir süre için yönetici denetim gerekir. Bu rolü etkinleştirmek için istekte bulunabilir.

Bu makale, Azure AD rollerine PIM etkinleştirmek isteyen yöneticiler için yazılmıştır.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi

Bir Azure AD rolü gerektiğinde kullanarak etkinleştirme isteyebilir **rollerim** PIM gezinme seçeneği.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**. PIM kutucuk, panonuza ekleme hakkında daha fazla bilgi için bkz: [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **rollerim** , uygun bir listesini görmek için Azure AD rolleri.

    ![Azure AD rolleri - rollerim](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Etkinleştirmek istediğiniz bir rol bulur.

    ![Azure AD rolleri - roller listem](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Tıklayın **etkinleştirme** rol etkinleştirme ayrıntıları bölmesini açmak için.

1. Rolünüz multi factor authentication (MFA) gerektiriyorsa, tıklayın **devam etmeden önce kimliğinizi Doğrulamamız**. Yalnızca bir kez oturum başına kimlik doğrulaması gerekir.

    ![MFA ile rol etkinleştirme tamamlanmadan önce doğrulayın.](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Tıklayın **Kimliğimi doğrula** ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![Ek güvenlik doğrulaması](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Tıklayın **etkinleştirme** etkinleştirme bölmesini açmak için.

    ![Etkinleştirme bölmesi](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Gerekirse, bir özel etkinleştirme başlangıç saatini belirtin.

1. Etkinleştirme süresi belirtin.

1. İçinde **etkinleştirme nedeni** kutusuna, etkinleştirme isteği nedeni girin. Bazı roller, sorun bileti numarası girmesini gerektirir.

    ![Tamamlanan etkinleştirme bölmesi](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Tıklayın **etkinleştirme**.

    Rol onay gerektirmiyorsa bir **etkinleştirme durumu** etkinleştirme durumunu görüntüleyen bölmesi görünür.

    ![Etkinleştirme durumu](./media/pim-how-to-activate-role/activation-status.png)

    Tüm aşamaları tamamlandıktan sonra tıklayın **oturumunuzu** dışında Azure portalında oturum için bağlantı. Ayrıca, portalda yeniden oturum açın, artık rol kullanabilirsiniz.

    Varsa [rolünü onay gerektiren](./azure-ad-pim-approval-workflow.md) etkinleştirmek için bir bildirim isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![Bildirim istek](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için Bekleyen isteklerinizi durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **isteklerim** isteklerinizin listesini görmek için.

    ![Azure AD rolleri - isteklerim](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Rol devre dışı bırak

Bir rol etkinleştirildikten sonra süre sınırı (uygunluk süresi) ulaşıldığında otomatik olarak devre dışı bırakır.

Erken yönetici görevleri tamamlanırsa, el ile Azure AD Privileged Identity Management içinde rol devre dışı bırakabilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **rollerim**.

1. Tıklayın **etkin rollerin** etkin rollerin listesini görmek için.

1. Bitti rol Bul kullanarak ve ardından **devre dışı bırak**.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Onay gerektiren bir rolü etkinleştirmesi gerekmiyorsa, bekleyen istek dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **isteklerim**.

1. İptal etmek istediğiniz role tıklayın **iptal** düğmesi.

    İptal'i tıklattığınızda, isteği iptal edilecek. Rolü etkinleştirmek için yeniden, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Bekleyen isteği iptal et](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-not-granted-after-activating-a-role"></a>Bir rol etkinleştirdikten sonra değil izinler

Bir rolü PIM etkinleştirdiğinizde, istenen yönetim portalına erişmek veya belirli bir yönetim iş yükü içinde işlevleri gerçekleştirmek için önce en az 10 dakika sürer. Etkinleştirme tamamlandıktan sonra Azure portalında oturum açın ve yeni etkinleştirilen rol kullanmaya başlamak için yeniden oturum açın.

Ek sorun giderme adımları için bkz. [sorun giderme yükseltilmiş izinler](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynağı rollerim PIM etkinleştir](pim-resource-roles-activate-your-roles.md)
