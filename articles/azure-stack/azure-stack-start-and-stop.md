---
title: Başlatma ve durdurma Azure yığın | Microsoft Docs
description: Başlat ve Azure yığın kapatma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 53015ba5c282bbe9c7b8185b080ffb6d834b6c75
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31391142"
---
# <a name="start-and-stop-azure-stack"></a>Azure yığın durdurup başlatın
Düzgün kapatılır ve Azure yığın hizmetlerini yeniden başlatmak için bu makaledeki yordamları izlemelisiniz. 

## <a name="stop-azure-stack"></a>Azure yığın Durdur 

Azure yığın aşağıdaki adımlarla kapatın:

1. Ayrıcalıklı uç nokta oturumu (CESARETLENDİRİCİ) ağ erişimi olan bir makineden Azure yığın ERCS VM'ler açın. Yönergeler için bkz: [Azure yığınında kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md).

2. CESARETLENDİRİCİ çalıştırın:

    ```powershell
      Stop-AzureStack
    ```

3. Devre dışı güç tüm fiziksel Azure yığın düğümlerine bekleyin.

> [!Note]  
> Özgün donatım üreticisi (kimlerin Azure yığın donanımınız sağlanan OEM'den) yönergeleri izleyerek bir fiziksel düğüm güç durumunu doğrulayabilirsiniz. 

## <a name="start-azure-stack"></a>Azure yığın Başlat 

Azure yığın aşağıdaki adımlarla başlatın. Azure yığın nasıl durduruldu bağımsız olarak aşağıdaki adımları izleyin.

1. Her fiziksel düğümlerin Azure yığın ortamınızdaki güç. Fiziksel düğümler için yönergeler gücüyle gelen özgün donanım üreticisi (kimin donanım Azure yığını için sağlanan OEM) yönergeleri izleyerek doğrulayın.

2. Azure yığın altyapı hizmetleri başlatana kadar bekleyin. Azure yığın altyapı hizmetleri başlatma işlemini tamamlama için iki saat gerektirebilir. Azure yığın ile başlangıç durumunu doğrulayabilirsiniz [ **Get-ActionStatus** cmdlet](#get-the-startup-status-for-azure-stack).


## <a name="get-the-startup-status-for-azure-stack"></a>Azure yığını için başlatma durumunu Al

Aşağıdaki adımlarla Azure yığın başlangıç yordamı için başlangıç alın:

1. Ayrıcalıklı bir uç nokta oturumu ağ erişimi olan bir makineden Azure yığın ERCS VM'ler açın.

2. CESARETLENDİRİCİ çalıştırın:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Başlatma ve kapatma Azure yığınının sorun giderme

Altyapı ve Kiracı Hizmetleri Azure yığın ortamınıza başarıyla 2 saat sonra güç başlatma, aşağıdaki adımları gerçekleştirin. 

1. Ayrıcalıklı bir uç nokta oturumu ağ erişimi olan bir makineden Azure yığın ERCS VM'ler açın.

2. Çalıştırın: 

    ```powershell
      Test-AzureStack
      ```

3. Çıktıyı gözden geçirin ve sistem durumu hataları çözümleyin. Daha fazla bilgi için bkz: [Azure yığınının bir doğrulama sınamasını çalıştırmanızı](azure-stack-diagnostic-test.md).

4. Çalıştırın:

    ```powershell
      Start-AzureStack
    ```

5. Çalışıyorsa **başlangıç AzureStack** hataya, Microsoft Müşteri Hizmetleri desteği'ne başvurun. 

## <a name="next-steps"></a>Sonraki adımlar 

Azure yığın Tanılama aracı hakkında daha fazla bilgi edinin ve günlüğe kaydetme bkz [Azure yığın tanılama araçları](azure-stack-diagnostics.md).
