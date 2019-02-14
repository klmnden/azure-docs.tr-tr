---
title: Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri | Microsoft Docs
description: Onaylayın veya reddedin Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için istekleri öğrenin.
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
ms.date: 02/08/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: a458f62ec38503e401b3fb810dc7edc8059a7715
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245584"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri

Azure AD Privileged Identity Management ile (PIM), rollerini etkinleştirme için onay gerektirecek şekilde yapılandırın ve bir veya birden çok kullanıcı veya grup onaylayanlara temsilci olarak seçin. Temsilci onaylayanlar, istekleri onaylamak için 24 saat vardır. Bir isteği 24 saat içinde onaylanmazsa, ardından uygun kullanıcı yeni bir isteği yeniden göndermeniz gerekir. Onay 24 saatlik zaman penceresi yapılandırılabilir değildir.

Onaylayın veya reddedin Azure kaynak rolleri için istekleri için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Onayınızı bekleyen bir Azure Kaynak rolü istek olduğunda, bir temsilci onaylayan olarak bir e-posta bildirimi alırsınız. Bu bekleyen istekler PIM'de görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **istekleri onaylama**.

    ![Azure kaynakları - istekleri Onayla](./media/pim-resource-roles-approval-workflow/resources-approve-requests.png)

    İçinde **rol etkinleştirmesi için istekler** bölümünde onayınızı bekleyen isteklerinin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. Bulup onaylamak istediğiniz istekte tıklayın. Bir onay bölmesi görünür.

    ![İstekleri bölmesinde Onayla](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. İçinde **gerekçe** bir neden yazın.

1. Tıklayın **onaylama**.

    Onayınızı ile bir bildirim görüntülenir.

    ![Bildirimi onaylayın](./media/pim-resource-roles-approval-workflow/resources-approve-notification.png)

## <a name="deny-requests"></a>İstekleri Reddet

1. Bulun ve reddetmek istediğiniz isteği'ni tıklatın. Bir onay bölmesi görünür.

    ![İstekleri bölmesinde Onayla](./media/pim-resource-roles-approval-workflow/resources-approve-pane.png)

1. İçinde **gerekçe** bir neden yazın.

1. Tıklayın **Reddet**.

    Bir bildirim, engelleme ile görüntülenir.

## <a name="workflow-notifications"></a>İş akışı bildirimlerini

İş akışı bildirimlerini hakkında bazı bilgiler aşağıda verilmiştir:

- Bir rol için bir isteği, gözden geçirme olduğunda tüm üyelerinin onaylayan listesini e-posta ile bildirilir. E-posta bildirimleri onaylayan burada onaylama veya reddetme isteğini doğrudan bir bağlantı içerir.
- İstekleri onaylar veya reddeder listenin ilk üyesi tarafından çözümlenir.
- Bir onaylayan isteği yanıtladığında onaylayan listenin tüm üyelerini eylemi bildirilir.
- Onaylanan bir üyenin içindeki rollerine etkin olduğunda, kaynak yöneticileri bildirilir.

>[!Note]
>Onaylanan bir üyenin etkin olmamalıdır düşündüğü bir kaynak yöneticisi PIM'de etkin bir rol atamasını kaldırabilir. Onaylayan listesini üyesi olduğu sürece kaynak yöneticileri, bekleyen istek bildirilmez olsa da, görüntüleyebilir ve bekleyen istekler PIM görüntüleyerek bekleyen tüm kullanıcıların istekleri iptal et. 

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri genişletmek veya yenileme](pim-resource-roles-renew-extend.md)
- [PIM e-posta bildirimleri](pim-email-notifications.md)
- [Onaylayın veya Azure AD dizin rollerini PIM isteklerini reddedin](azure-ad-pim-approval-workflow.md)
