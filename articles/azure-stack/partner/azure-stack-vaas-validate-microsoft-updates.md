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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 8e0009bf0fc34d3e0d22755d93d941b85db62ffd
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334474"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft yazılım güncelleştirmeleri doğrulayın

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft Azure Stack yazılım güncelleştirmeleri düzenli olarak serbest bırakır. Bu güncelleştirmeler, güncelleştirmeleri çözümleri karşı doğrulamak ve Microsoft'a geri bildirimde genel kullanıma açık yapılan tarifelerindeki ortak mühendislik iş ortaklarının Azure Stack için sağlanır.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aylık güncelleştirme uygulama

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Bir iş akışı oluşturma

Güncelleştirme doğrulamaları aynı iş akışı olarak kullanmak **paket doğrulama**. Konumundaki yönergeleri [paket doğrulama iş akışı oluşturma](azure-stack-vaas-validate-oem-package.md#create-a-package-validation-workflow).

## <a name="run-tests"></a>Testleri çalıştırın

Güncelleştirme doğrulamaları aynı iş akışı olarak kullanmak **paket doğrulama**. Konumundaki yönergeleri [paket doğrulama çalıştırmak testlerini](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests).

Paket için güncelleştirme doğrulamaları imzalama isteği gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)
