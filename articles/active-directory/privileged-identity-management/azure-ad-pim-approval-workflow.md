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
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7bf1e437e97fdb4d929af23bd7b2a9abb49268df
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189167"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Onaylayın veya Azure AD dizin rollerini PIM isteklerini reddedin

Privileged Identity Management ile rollerini etkinleştirme için onay gerektirecek şekilde yapılandırın ve bir veya birden çok kullanıcı veya grup onaylayanlara temsilci olarak seçin.

## <a name="view-pending-approvals-requests"></a>Bekleyen onaylar (istek) görüntüleme

Onayınızı bekleyen istek olduğunda, bir temsilci onaylayan olarak e-posta bildirimleri alırsınız. PIM Portalı'nda bu istekleri görüntülemek için Panoda (Yeni Gezinti) "Bekleyen onay isteklerini" sekmesi sol gezinti çubuğunda seçin.

![](media/azure-ad-pim-approval-workflow/image023.png)

Burada, onay bekleyen istek listesi görürsünüz:

![](media/azure-ad-pim-approval-workflow/image024.png)

## <a name="approve-or-deny-requests-for-role-elevation-single-andor-bulk"></a>Onaylayın veya reddedin rol yükseltme (tek ve/veya toplu) istekleri

İstekleri onaylamak veya reddetmek istediğiniz seçin ve kararınızı ile karşılık gelen Eylem çubuğu düğmesine tıklayın:

![](media/azure-ad-pim-approval-workflow/image025.png)

## <a name="provide-justification-for-my-approvaldenial"></a>My onay/engelleme için gerekçe sağlayın

Bu onaylamak veya aynı anda birden fazla isteği reddetmek için yeni bir dikey pencere açar. Kararınız için bir gerekçe girin ve onaylayın (veya Reddet) alt ya da dikey penceresinde:

![](media/azure-ad-pim-approval-workflow/image029.png)

İstek işlemi tamamlandığında, durum simgesinde yaptığınız karar yansıtır (Bu örnekte, onaylama kararıdır):

![](media/azure-ad-pim-approval-workflow/image031.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
- [PIM e-posta bildirimleri](pim-email-notifications.md)
