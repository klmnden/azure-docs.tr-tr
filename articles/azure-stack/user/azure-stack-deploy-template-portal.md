---
title: Şablonları portal kullanarak Azure Stack'te dağıtma | Microsoft Docs
description: Şablonları dağıtmak için Azure Stack portal'ı kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: eafa60f2-16c9-4ef1-b724-47709e9ea29e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 4a8d4ab4d93831abbd9489d9023dd9b6f5c66d6d
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246900"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Şablonları Azure Stack portalını kullanarak dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Portal, Azure Stack için Azure Resource Manager şablonlarını dağıtmak için kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. Portal, select oturum **+ kaynak Oluştur**ve ardından **özel**.
2. **Şablon dağıtımı**'nı seçin.
3. Seçin **şablonu Düzen**ve ardından JSON şablon kodunuz kod penceresine yapıştırın. **Kaydet**’i seçin.
4. Seçin **parametreleri Düzenle**, gösterilir ve ardından bu parametrelerin değerlerini sağlamasını **Tamam**.
5. Seçin **abonelik**. Kullanın ve ardından istediğiniz aboneliği seçin **Tamam**.
6. Seçin **kaynak grubu**. Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun ve ardından **Tamam**.
7. **Oluştur**’u seçin. Yeni bir kutucuk Panoda şablon dağıtımınızın ilerlemesini izler.

## <a name="next-steps"></a>Sonraki adımlar

Şablon dağıtma hakkında daha fazla bilgi edinmek için şu makaleye bakın:

- [Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
