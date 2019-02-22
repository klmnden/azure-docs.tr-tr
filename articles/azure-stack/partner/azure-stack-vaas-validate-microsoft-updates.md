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
ms.openlocfilehash: b7fa03cdf52fc3218e9556c9664daafdc60243f3
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593226"
---
# <a name="validate-software-updates-from-microsoft"></a>Microsoft yazılım güncelleştirmeleri doğrulayın

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Microsoft Azure Stack yazılım güncelleştirmeleri düzenli olarak serbest bırakır. Bu güncelleştirmeler, Azure Stack iş ortakları coengineering sağlanır. Güncelleştirmeleri herkese, öncelikli sağlanır. Çözümünüzü karşı güncelleştirmeleri denetleyin ve Microsoft'a geri bildirim sağlayın.

Bir adlandırma kuralı kullanarak Microsoft Azure Stack yazılım güncelleştirmeleri belirlenmiş, örneğin, Mart 2018 için 1803 belirten güncelleştirme. Azure Stack güncelleştirme ilkesi hakkında daha fazla bilgi için kullanılabilen temposu ve sürüm notları, bakın [Azure Stack hizmet İlkesi](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

## <a name="prerequisites"></a>Önkoşullar

Aylık güncelleştirme sürecinde VaaS alıştırma önce aşağıdaki öğeleri ile ilgili bilgi sahibi olmanız gerekir:

- [Hizmet temel kavramları olarak doğrulama](azure-stack-vaas-key-concepts.md)
- [Etkileşimli özellik doğrulama testi](azure-stack-vaas-interactive-feature-verification.md)

## <a name="required-tests"></a>Gerekli testleri

Şu testler, aylık yazılım doğrulama için aşağıdaki sırayla yürütülmesi gereken:

1. Aylık Azure Stack güncelleştirme doğrulama
2. Bulut benzetimi altyapısı

## <a name="validating-software-updates"></a>Doğrulama yazılım güncelleştirmeleri

1. Yeni bir **paket doğrulama** iş akışı.
1. Gerekli testler için yukarıdaki yönergeleri uygulayın [paket doğrulama çalıştırmak testlerini](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests). Ek yönergeler için aşağıdaki bölüme bakın **aylık Azure Stack güncelleştirme doğrulama** test edin.

### <a name="apply-the-monthly-update"></a>Aylık güncelleştirme uygulama

1. Karşı testleri yürütmek için bir aracı seçin.
1. Zamanlama **aylık Azure Stack güncelleştirme doğrulaması**.
1. Damgada şu anda dağıtılmış OEM uzantı paketinin konumu ve güncelleştirme sırasında uygulanacak OEM uzantı paketinin konumu belirtin. Bu paketler URL'leri yapılandırmak için bkz: [doğrulama için paketleri yönetme](azure-stack-vaas-validate-oem-package.md#managing-packages-for-validation).
1. Seçili Aracı kullanıcı Arabirimi adımları izleyin.

Sorularınız veya endişeleriniz varsa lütfen iletişime geçin [VaaS Yardım](mailto:vaashelp@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)