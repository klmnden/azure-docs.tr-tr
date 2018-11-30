---
title: Azure AD dizin rollerim PIM etkinleştirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM), Azure AD Dizin rolleri etkinleştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 534714accb504e4ce487950fef028ab675c46a87
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496400"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Azure AD dizin rollerim PIM etkinleştir

Azure Active Directory (Azure AD) Privileged Identity Management (PIM), kuruluşlara ayrıcalıklı Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetme sürecini basitleştirir.  

Bir yönetici rolü için uygun yapılmadıysa, ayrıcalıklı eylemler gerçekleştirmek, ihtiyacınız olduğunda bu role aktive edebileceğiniz anlamına gelir. Office 365 özellikleri bazen yönetiyorsanız, bu rol diğer hizmetleri çok etkiler beri Örneğin, kuruluşunuzun ayrıcalıklı rol yöneticileri, kalıcı bir genel yönetici sunamazsınız. Bunun yerine, bunlar, Exchange Online yönetici gibi Azure AD rolleri için uygun yapın. Kendi ayrıcalıklarına sahip olmanız ve ardından önceden belirlenmiş bir süre için yönetici denetim gerekir. Bu rolü etkinleştirmek için istekte bulunabilir.

Bu makale, Azure AD dizin rollerine PIM etkinleştirmek isteyen yöneticiler için yazılmıştır.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi

Bir Azure AD dizin rolü gerektiğinde kullanarak etkinleştirme isteyebilir **rollerim** PIM gezinme seçeneği.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**. PIM kutucuk, panonuza ekleme hakkında daha fazla bilgi için bkz: [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **rollerim** , uygun bir listesini görmek için Azure AD Dizin rolleri.

    ![Azure AD Dizin rolleri - rollerim](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Etkinleştirmek istediğiniz bir rol bulur.

    ![Azure AD Dizin rolleri - roller listem](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

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

    Rolü onay gerektirmez, etkin ve etkin rollerin listesine eklenir. Rol hemen kullanmak istiyorsanız, sonraki bölümde yer alan adımları izleyin.

    Varsa [rolünü onay gerektiren](./azure-ad-pim-approval-workflow.md) etkinleştirmek için bir bildirim isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![Bildirim istek](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Hemen etkinleştirildikten sonra bir rol kullanın

Bir rolü PIM etkinleştirdiğinizde, istenen yönetim portalına erişmek veya belirli bir yönetim iş yükü içinde işlevleri gerçekleştirmek için önce en az 10 dakika sürer. İzinlerinizi güncelleştirmesini zorlamak için kullanan **uygulama erişimi** sayfasında aşağıdaki adımlarda anlatıldığı gibi.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **uygulama erişimi** sayfası.

    ![PIM uygulama erişimi](./media/pim-how-to-activate-role/pim-application-access.png)

1. Tıklayın **Azure Active Directory** Şirket portalı yeniden bağlantı **tüm kullanıcılar** sayfası.

    Bu bağlantıya tıkladığınızda, geçerli belirtecinizi geçersiz kılmak ve güncelleştirilmiş izinlerinizi içermesi gereken yeni bir belirteç almak için Azure portalında zorla.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için Bekleyen isteklerinizi durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **isteklerim** isteklerinizin listesini görmek için.

    ![Azure AD Dizin rolleri - isteklerim](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Rol devre dışı bırak

Bir rol etkinleştirildikten sonra süre sınırı (uygunluk süresi) ulaşıldığında otomatik olarak devre dışı bırakır.

Erken yönetici görevleri tamamlanırsa, el ile Azure AD Privileged Identity Management içinde rol devre dışı bırakabilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **rollerim**.

1. Tıklayın **etkin rollerin** etkin rollerin listesini görmek için.

1. Bitti rol Bul kullanarak ve ardından **devre dışı bırak**.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Onay gerektiren bir rolü etkinleştirmesi gerekmiyorsa, bekleyen istek dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **isteklerim**.

1. İptal etmek istediğiniz role tıklayın **iptal** düğmesi.

    İptal'i tıklattığınızda, isteği iptal edilecek. Rolü etkinleştirmek için yeniden, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Bekleyen isteği iptal et](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-not-granted-after-activating-a-role"></a>Bir rol etkinleştirdikten sonra değil izinler

Bir rolü PIM etkinleştirdiğinizde, istenen yönetim portalına erişmek veya belirli bir yönetim iş yükü içinde işlevleri gerçekleştirmek için önce en az 10 dakika sürer. İzinlerinizi güncelleştirmesini zorlamak için kullanan **uygulama erişimi** sayfasında daha önce açıklandığı [hemen etkinleştirildikten sonra bir rol kullanın](#use-a-role-immediately-after-activation).

Ek sorun giderme adımları için bkz. [sorun giderme yükseltilmiş izinler](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynağı rollerim PIM etkinleştir](pim-resource-roles-activate-your-roles.md)
