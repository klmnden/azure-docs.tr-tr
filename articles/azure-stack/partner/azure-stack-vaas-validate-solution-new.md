---
title: Yeni bir Azure Stack çözüm doğrulama | Microsoft Docs
description: Hizmet olarak doğrulama ile yeni bir Azure Stack çözüm doğrulamak hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 777609b89bc08cd61489d2c3a3669ec07ccbc372
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647026"
---
# <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözüm doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Nasıl kullanacağınızı öğrenin **çözüm doğrulama** yeni Azure Stack çözümleri onaylamak için iş akışı.

Tüm dünyada Microsoft ve iş ortağı arasında Windows Server logo sertifikası gereksinimleri sağladıktan sonra anlaşılan donanım ürün reçetesi (BoM) bir Azure Stack çözümüdür. BoM donanım bir değişiklik olduğunda, bir çözüm recertified gerekir. Ekip, ne zaman yeniden Onayla çözümleri hakkında daha fazla soru için kişi [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Çözümünüzü onaylamak için çözüm doğrulama iş akışı iki kez çalıştırın. İçin bir kez çalıştır *en düşük düzeyde* yapılandırması desteklenir. İkinci bir kez çalıştır *maximally* yapılandırması desteklenir. Her iki yapılandırma tüm sınamaları geçmesi durumunda Microsoft çözümü onaylar.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Bir çözüm doğrulama iş akışı oluşturma

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]
2. Seçin **Başlat** üzerinde **çözüm doğrulamaları** Döşe.

    ![Çözüm doğrulama iş akışı kutucuğu](media/tile_validation-solution.png)

3. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
4. Seçin **çözüm yapılandırması**.
    - **En düşük**: Çözüm desteklenen düğüm sayısı alt sınırı ile yapılandırılır.
    - **En fazla**: çözüm en fazla desteklenen düğüm sayısını ile yapılandırılır.
5. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Çözüm doğrulama bilgileri](media/workflow_validation-solution_info.png)

6. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Bir iş akışını oluşturduktan sonra ortam parametrelerini değiştirilemez.

7. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
8. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testleri Özet sayfasına yönlendirilirsiniz.

## <a name="execute-solution-validation-tests"></a>Çözüm doğrulama testleri yürütün

İçinde **çözüm doğrulama testleri özeti** sayfasında doğrulamayı tamamlamak için gereken testlerin bir listesini görürsünüz.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

![Zamanlama çözümü doğrulama sınaması](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)