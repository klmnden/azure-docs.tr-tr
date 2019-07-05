---
title: Onaylayın veya reddedin istekleri için Azure AD PIM - Azure Active Directory rolleri | Microsoft Docs
description: Onaylayın veya reddedin istekleri için Azure AD Privileged Identity Management (PIM) Azure AD rolleri öğrenin.
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
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f83cb38567feb51ba7959ada7730d66ded677bf9
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476534"
---
# <a name="approve-or-deny-requests-for-azure-ad-roles-in-pim"></a>Onaylayın veya reddedin istekleri için Azure AD PIM rolleri

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) ile rollerini etkinleştirme için onay gerektirecek şekilde yapılandırın ve bir veya birden çok kullanıcı veya grup onaylayanlara temsilci olarak seçin. Temsilci onaylayanlar, istekleri onaylamak için 24 saat vardır. Bir isteği 24 saat içinde onaylanmazsa, ardından uygun kullanıcı yeni bir isteği yeniden göndermeniz gerekir. Onay 24 saatlik zaman penceresi yapılandırılabilir değildir.

Onaylamak veya reddetmek için Azure AD rolleri istekleri için bu makaledeki adımları izleyin.

## <a name="view-pending-requests"></a>Bekleyen istekleri görüntüleme

Onayınızı bekleyen bir Azure AD rol istek olduğunda, bir temsilci onaylayan olarak bir e-posta bildirimi alırsınız. Bu bekleyen istekler PIM'de görüntüleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **istekleri onaylama**.

    ![Azure AD rolleri - istekleri Onayla](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Onayınızı bekleyen isteklerinin bir listesini görürsünüz.

## <a name="approve-requests"></a>İstekleri onaylama

1. İstekleri onaylama ve ardından istediğiniz seçin **Onayla** Onayla açmak için istekleri bölmesinde seçili.

    ![İstek listesi Onayla seçeneğinin vurgulandığı Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. İçinde **onay nedeni** bir neden yazın.

    ![Seçili istekleri Onayla neden bölmesiyle Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Tıklayın **onaylama**.

    Durum simgesinde onayınızı ile güncelleştirilecektir.

    ![Onayla düğmesine tıklanana sonra bölmesinde seçili istekleri Onayla](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>İstekleri Reddet

1. İstekleri Reddet ve ardından istediğiniz seçin **Reddet** Reddet açmak için istekleri bölmesinde seçili.

    ![İzin verme seçeneği vurgulanmış istekleri listesi Onayla](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. İçinde **reddetme nedeni** bir neden yazın.

    ![Reddetme nedeni bölmesiyle seçili istekleri Reddet](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Tıklayın **Reddet**.

    Durum simgesinde, engelleme ile güncelleştirilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM e-posta bildirimleri](pim-email-notifications.md)
- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
