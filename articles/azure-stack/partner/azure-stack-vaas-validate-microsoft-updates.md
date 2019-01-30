---
title: Microsoft Azure Stack doğrulama hizmet olarak yazılım güncelleştirmelerini doğrulama | Microsoft Docs
description: Microsoft ile doğrulama hizmet olarak yazılım güncelleştirmelerini doğrulamak hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 01/14/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 4e6f5a17544c1419eb6101acdd6590f034ea4aa3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241467"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft yazılım güncelleştirmeleri doğrulayın

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft Azure Stack yazılım güncelleştirmeleri düzenli olarak serbest bırakır. Bu güncelleştirmeler, Azure Stack iş ortakları coengineering sağlanır. Güncelleştirmeleri herkese, öncelikli sağlanır. Çözümünüzü karşı güncelleştirmeleri denetleyin ve Microsoft'a geri bildirim sağlayın.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aylık güncelleştirme uygulama

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Bir iş akışı oluşturma

Güncelleştirme doğrulamaları aynı iş akışı olarak kullanmak **çözüm doğrulama**.

## <a name="run-tests"></a>Testleri çalıştırma

1. Güncelleştirme doğrulamaları aynı iş akışı olarak kullanmak **çözüm doğrulama**. 

2. Konumundaki yönergeleri [çözüm doğrulama çalıştırmak testlerini](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests). Bunun yerine aşağıdaki testleri seçin:
    - Aylık Azure Stack güncelleştirme doğrulama
    - Bulut benzetimi altyapısı

Paket için güncelleştirme doğrulamaları imzalama isteği gerek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)