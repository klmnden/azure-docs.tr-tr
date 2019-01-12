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
ms.date: 1/07/2018
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: 8ea0f36f3d167109fc0dc0d91707a08cff1c6a17
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243993"
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