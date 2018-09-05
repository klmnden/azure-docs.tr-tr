---
title: Azure kaynağı rollerim PIM etkinleştirme | Microsoft Docs
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
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59bce2c61db5838bb21a29757d4e354311ecffd5
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666256"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Azure kaynağı rollerim PIM etkinleştir

Azure AD Privileged Identity Management (PIM) kullanarak Azure kaynakları için uygun rolü üyeleri etkinleştirme bir gelecek tarih ve saat için zamanlayabilirsiniz. Bunlar, belirli etkinleştirme süresi üst sınırı (yönetici tarafından yapılandırılır) içinde de seçebilirsiniz.

Bu makalede, Azure Kaynak rolü PIM etkinleştirmek için gereken üyeleri içindir.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi

Bir Azure Kaynak rolü gerektiğinde kullanarak etkinleştirme isteyebilir **rollerim** PIM gezinme seçeneği.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**. PIM kutucuk, panonuza ekleme hakkında daha fazla bilgi için bkz: [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **rollerim** , uygun bir listesini görmek için Azure AD Dizin rolleri ve Azure kaynağı rolleri.

    ![Azure AD Dizin rolleri ve Azure kaynak rolleri - rollerim](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

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

    Rolü onay gerektirmez, artık etkinleştirilir ve rol etkin rollerin listesinde görünür. Varsa [rolünü onay gerektiren](pim-resource-roles-approval-workflow.md) etkinleştirmek için bir bildirim isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![Bildirim istek](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için Bekleyen isteklerinizi durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim** Azure AD dizin rolü ve Azure Kaynak rolü listesini görmek için ister.

    ![Azure AD Dizin rolleri ve Azure kaynak rolleri - isteklerim](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Görüntülemek için sağa kaydırma **istek durumu** sütun.

## <a name="use-a-role-immediately-after-activation"></a>Hemen etkinleştirildikten sonra bir rol kullanın

Önbelleğe alma nedeniyle etkinleştirmeleri hemen yenilemeye olmadan Azure portalında gerçekleşmez. Bir rol etkinleştirdikten sonra gecikmeler olasılığını azaltmak gerekiyorsa, kullanabileceğiniz **uygulama erişimi** portalında sayfası. Bu sayfadan erişilen uygulamalar için yeni rol atamaları hemen denetleyin.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **uygulama erişimi** sayfası.

    ![PIM uygulama erişimi - ekran görüntüsü](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Tıklayın **Azure kaynaklarını** Şirket portalı yeniden **tüm kaynakları** sayfası.

    Bu bağlantıya tıkladığınızda, daha önce yenilemeye zorlamak ve yeni Azure kaynak rol atamaları denetimi yoktur.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Onay gerektiren bir rolü etkinleştirmesi gerekmiyorsa, bekleyen istek dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim**.

1. İptal etmek istediğiniz role tıklayın **iptal** bağlantı.

    İptal'i tıklattığınızda, isteği iptal edilecek. Rolü etkinleştirmek için yeniden, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Bekleyen isteği iptal et](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri genişletmek veya yenileme](pim-resource-roles-renew-extend.md)
- [Azure AD dizin rollerim PIM etkinleştir](pim-how-to-activate-role.md)