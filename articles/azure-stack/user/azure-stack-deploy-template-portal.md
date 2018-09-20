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
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 4013b7136fd365b1d37348cfc1f0b46cce787ce2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363132"
---
# <a name="deploy-templates-using-the-azure-stack-portal"></a>Şablonları Azure Stack portalını kullanarak dağıtma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Portal, Azure Stack için Azure Resource Manager şablonlarını dağıtmak için kullanabilirsiniz.

## <a name="to-deploy-a-template"></a>Bir şablonu dağıtmak için

1. Portal, select oturum **+ kaynak Oluştur**ve ardından **özel**.
2. Seçin **şablon dağıtımı**.
3. Seçin **şablonu Düzen**ve ardından JSON şablon kodunuz kod penceresine yapıştırın. **Kaydet**’i seçin.
4. Seçin **parametreleri Düzenle**, gösterilir ve ardından bu parametrelerin değerlerini sağlamasını **Tamam**.
5. Seçin **abonelik**. Kullanın ve ardından istediğiniz aboneliği seçin **Tamam**.
6. Seçin **kaynak grubu**. Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun ve ardından **Tamam**.
7. **Oluştur**’u seçin. Yeni bir kutucuk Panoda şablon dağıtımınızın ilerlemesini izler.

## <a name="next-steps"></a>Sonraki adımlar

Şablon dağıtımı hakkında daha fazla bilgi için bkz:

[Şablonları PowerShell ile dağıtma](azure-stack-deploy-template-powershell.md)
