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
ms.date: 12/20/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 12/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 57aa0146f29d45dbcb5b1a0ac2f4fbdf31cb045b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238763"
---
# <a name="validate-a-new-azure-stack-solution"></a>Yeni bir Azure Stack çözüm doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Nasıl kullanacağınızı öğrenin **çözüm doğrulama** yeni Azure Stack çözümleri onaylamak için iş akışı.

Tüm dünyada Microsoft ve iş ortağı arasında Windows Server logo sertifikası gereksinimleri sağladıktan sonra anlaşılan donanım ürün reçetesi (BoM) bir Azure Stack çözümüdür. BoM donanım bir değişiklik olduğunda, bir çözüm recertified gerekir. Ekip, ne zaman yeniden Onayla çözümleri hakkında daha fazla soru için kişi [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Çözümünüzü onaylamak için çözüm doğrulama iş akışı iki kez çalıştırın. İçin bir kez çalıştır *en düşük düzeyde* yapılandırması desteklenir. İkinci bir kez çalıştır *maximally* yapılandırması desteklenir. Her iki yapılandırma tüm sınamaları geçmesi durumunda Microsoft çözümü onaylar.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Bir çözüm doğrulama iş akışı oluşturma

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Seçin **Başlat** üzerinde **çözüm doğrulamaları** Döşe.

    ![Çözüm doğrulama iş akışı kutucuğu](media/tile_validation-solution.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Seçin **çözüm yapılandırması**.
    - **En düşük**: Çözüm desteklenen düğüm sayısı alt sınırı ile yapılandırılır.
    - **En fazla**: çözüm en fazla desteklenen düğüm sayısını ile yapılandırılır.
6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Çözüm doğrulama bilgileri](media/workflow_validation-solution_info.png)

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Bir iş akışını oluşturduktan sonra ortam parametrelerini değiştirilemez.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Testleri Özet sayfasına yönlendirilirsiniz.

## <a name="run-solution-validation-tests"></a>Çözüm doğrulama testlerini çalıştırın

İçinde **çözüm doğrulama testleri özeti** sayfasında doğrulamayı tamamlamak için gereken testlerin bir listesini görürsünüz.

Doğrulama iş akışlarında **zamanlama** test iş akışı oluşturma işlemi sırasında belirtilen düzey iş akışı ortak parametreleri kullanır (bkz [hizmetolarakAzureStackdoğrulamaişakışıortakparametreleri](azure-stack-vaas-parameters.md)). Herhangi bir test parametre değerleri geçersiz hale gelirse, bunları belirtildiği gibi resupply gerekir [iş akışı parametreleri değiştirmek](azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Var olan bir örneği üzerinde bir doğrulama testi zamanlama portalda eski örneği yerine yeni bir örneğini oluşturur. Eski örneği için günlükleri korunur ancak Portalı'ndan erişilebilir değildir.  
Bir test başarıyla tamamlandıktan sonra **zamanlama** eylemi devre dışı kalır.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

2. Aşağıdaki testler seçin:
    - Bulut benzetimi altyapısı
    - SDK'sı işletimsel Suite işlem
    - Disk kimliği Test
    - KeyVault uzantı SDK'sı işletimsel paketi
    - KeyVault SDK Operational Suite
    - Ağ SDK işletimsel paketi
    - Depolama hesabı SDK işletimsel paketi

3. Seçin **zamanlama** bağlam menüsünden test örneği zamanlamak için bir istem açın.

4. Test parametreleri gözden geçirin ve ardından **Gönder** test yürütme için zamanlamak için.

![Zamanlama çözümü doğrulama sınaması](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Sonraki adımlar

- [İzleme ve testleri VaaS portalında yönetme](azure-stack-vaas-monitor-test.md)