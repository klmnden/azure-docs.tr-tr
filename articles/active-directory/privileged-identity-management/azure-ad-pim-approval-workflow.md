---
title: Onaylayın veya reddedin istekleri için Azure AD dizin rollerini PIM | Microsoft Docs
description: Onaylayın veya reddedin Azure AD Privileged Identity Management (PIM), Azure AD Dizin rolleri için istekleri öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
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
ms.openlocfilehash: 68b0dba0280ce7875e797634a5dc2254cb889ad7
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244990"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Onaylayın veya Azure AD dizin rollerini PIM isteklerini reddedin

Azure AD Privileged Identity Management ile (PIM), rollerini etkinleştirme için onay gerektirecek şekilde yapılandırın ve bir veya birden çok kullanıcı veya grup onaylayanlara temsilci olarak seçin. Temsilci onaylayanlar, istekleri onaylamak için 24 saat vardır. Bir isteği 24 saat içinde onaylanmazsa, ardından uygun kullanıcı yeni bir isteği yeniden göndermeniz gerekir. Onay 24 saatlik zaman penceresi yapılandırılabilir değildir.

Onaylayın veya reddedin istekleri için Azure AD Dizin rolleri için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Onayınızı bekleyen bir Azure AD dizini rol istek olduğunda, bir temsilci onaylayan olarak bir e-posta bildirimi alırsınız. Bu bekleyen istekler PIM'de görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD Dizin rolleri**.

1. Tıklayın **istekleri onaylama**.

    ![PIM Azure AD Dizin rolleri - roller](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Onayınızı bekleyen isteklerinin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. İstekleri onaylama ve ardından istediğiniz seçin **Onayla** Onayla açmak için istekleri bölmesinde seçili.

    ![PIM'i Onayla istekleri listesi](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. İçinde **onay nedeni** bir neden yazın.

    ![İstekler seçilmiş PIM'i Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Tıklayın **onaylama**.

    Durum simgesinde onayınızı ile güncelleştirilecektir.

    ![İstekler seçilmiş PIM'i Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>İstekleri Reddet

1. İstekleri Reddet ve ardından istediğiniz seçin **Reddet** Reddet açmak için istekleri bölmesinde seçili.

    ![PIM'i Onayla istekleri listesi](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. İçinde **reddetme nedeni** bir neden yazın.

    ![Seçili PIM Reddet istekleri](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Tıklayın **Reddet**.

    Durum simgesinde, engelleme ile güncelleştirilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM e-posta bildirimleri](pim-email-notifications.md)
- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
