---
title: Azure kaynağı rollerim PIM - Azure Active Directory etkinleştirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM), Azure kaynak rolleri etkinleştirmeyi öğrenin.
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
ms.openlocfilehash: 04e8615cc5534255308c35fa1f675ef3a85aa84e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60438532"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Azure kaynağı rollerim PIM etkinleştir

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullanarak Azure kaynakları için uygun rolü üyeleri etkinleştirme bir gelecek tarih ve saat için zamanlayabilirsiniz. Bunlar, belirli etkinleştirme süresi üst sınırı (yönetici tarafından yapılandırılır) içinde de seçebilirsiniz.

Bu makalede, Azure Kaynak rolü PIM etkinleştirmek için gereken üyeleri içindir.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi

Bir Azure Kaynak rolü gerektiğinde kullanarak etkinleştirme isteyebilir **rollerim** PIM gezinme seçeneği.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**. PIM kutucuk, panonuza ekleme hakkında daha fazla bilgi için bkz: [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **rollerim**.

    ![Azure AD rolleri ve Azure kaynak rolleri - rollerim](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Tıklayın **Azure kaynağı rolleri** , uygun bir Azure kaynağı rolleri listesini görmek için.

   ![Azure kaynağı rolleri](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. İçinde **Azure kaynağı rolleri** listesinde, etkinleştirmek istediğiniz rol bulur.

    ![Azure kaynağı rolleri - roller listem](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Tıklayın **etkinleştirme** etkinleştirme bölmesini açmak için.

1. Rolünüz multi factor authentication (MFA) gerektiriyorsa, tıklayın **devam etmeden önce kimliğinizi Doğrulamamız**. Yalnızca bir kez oturum başına kimlik doğrulaması gerekir.

    ![MFA ile rol etkinleştirme tamamlanmadan önce doğrulayın.](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Tıklayın **Kimliğimi doğrula** ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![Ek güvenlik doğrulaması](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Sınırlı kapsam belirtmek istiyorsanız, tıklayın **kapsam** kaynak filtre bölmesini açın.

    Yalnızca ihtiyacınız olan kaynakları için erişim istemek için iyi bir uygulamadır. Kaynak Filtresi bölmesinde erişmesi gereken kaynakları ve kaynak gruplarını belirtebilirsiniz.

    ![Etkinleştir - kaynak filtresi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Gerekirse, bir özel etkinleştirme başlangıç saatini belirtin. Üye, seçilen bir zaman sonra etkinleştirilir.

1. İçinde **neden** kutusuna, etkinleştirme isteği nedeni girin.

    ![Tamamlanan etkinleştirme bölmesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Tıklayın **etkinleştirme**.

    Rolü onay gerektirmez, etkin ve etkin rollerin listesine eklenir. Rol kullanmak istiyorsanız, sonraki bölümde yer alan adımları izleyin.

    Varsa [rolünü onay gerektiren](pim-resource-roles-approval-workflow.md) etkinleştirmek için bir bildirim isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![Bildirim istek](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Hemen etkinleştirildikten sonra bir rol kullanın

Azure kaynak rollerinizi hemen kullanmak için etkinleştirdikten sonra etkinleştirme işleminden sonra herhangi bir gecikme olması durumunda bu adımları izleyin.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **rollerim** , uygun bir listesini görmek için Azure AD rolleri ve Azure kaynağı rolleri.

1. Tıklayın **Azure kaynağı rolleri**.

1. Tıklayın **etkin rollerin** sekmesi.

1. Rol etkin hale geldikten sonra portalı oturumunu kapat ve yeniden oturum açın.

    Rol artık kullanmak için kullanılabilir olmalıdır.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için Bekleyen isteklerinizi durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim** Azure AD rolüne ve Azure Kaynak rolü listesini görmek için ister.

    ![Azure AD rolleri ve Azure kaynak rolleri - isteklerim](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Görüntülemek için sağa kaydırma **istek durumu** sütun.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Onay gerektiren bir rolü etkinleştirmesi gerekmiyorsa, bekleyen istek dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim**.

1. İptal etmek istediğiniz role tıklayın **iptal** bağlantı.

    İptal'i tıklattığınızda, isteği iptal edilecek. Rolü etkinleştirmek için yeniden, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Bekleyen isteği iptal et](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-not-granted-after-activating-a-role"></a>Bir rol etkinleştirdikten sonra değil izinler

Bir rolü PIM etkinleştirdiğinizde, istenen yönetim portalına erişmek veya belirli bir yönetim iş yükü içinde işlevleri gerçekleştirmek için önce en az 10 dakika sürer. Etkinleştirme tamamlandıktan sonra Azure portalında oturum açın ve yeni etkinleştirilen rol kullanmaya başlamak için yeniden oturum açın.

Ek sorun giderme adımları için bkz. [sorun giderme yükseltilmiş izinler](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx).

### <a name="cannot-activate-a-role-due-to-a-resource-lock"></a>Rol kaynak kilidi nedeniyle etkinleştirilemiyor

Bir ileti alırsanız kapsamında bir rol ataması olan bir kaynak bir kaynak kilidi olduğundan rol etkinleştirmeyi denediğinizde bir Azure kaynağı kilitli olduğunu, bunun olması olabilir. Kilitleri kaynakları yanlışlıkla silme veya beklenmeyen değişikliklerden koruyun. Bir kilit de PIM etkinleştirme süresi sonunda kaynakta bir rol ataması çıkarmasını engeller. PIM bir kilidi uygulandığında düzgün olduğundan PIM rollerine kaynak etkinleştirmesini kullanıcıları engeller. Bu sorunu çözmek iki yolu vardır:

- Bölümünde anlatıldığı gibi silme [beklenmeyen değişiklikleri önlemek için kaynakları kilitleme](../../azure-resource-manager/resource-group-lock-resources.md).
- Kilit tutmak istiyorsanız, rol ataması kalıcı hale getirmek veya bir kesme cam hesabı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri genişletmek veya yenileme](pim-resource-roles-renew-extend.md)
- [PIM Azure AD'ye rollerimi etkinleştir](pim-how-to-activate-role.md)
