---
title: Azure Stack başlatıp | Microsoft Docs
description: Başlat ve Azure Stack kapatma hakkında bilgi edinin.
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
ms.openlocfilehash: dd1e64d5ad6982c85a8205e3036d30a2ede92f7c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930299"
---
# <a name="start-and-stop-azure-stack"></a>Azure Stack başlatıp
Düzgün bir şekilde kapatılmasını ve Azure Stack hizmetlerini yeniden başlatmak için bu makaleyi yordamlarını izlemelisiniz. Kapatma fiziksel olarak tüm Azure Stack ortamı güç. Başlangıç tüm altyapı rollerinde güçlendirir ve Kiracı kaynaklarını bunlar önce kapatma olan güç durumunu döndürür.

## <a name="stop-azure-stack"></a>Azure Stack Durdur 

Azure Stack aşağıdaki adımlarla kapatın:

1. Yaklaşan kapatma için Azure Stack ortamınızın Kiracı kaynaklar üzerinde çalışan tüm iş yükleri hazırlama. 

2. Ayrıcalıklı uç nokta oturum (CESARETLENDİRİCİ) ağ erişimi olan bir makineden Azure Stack ERCS Vm'lere açın. Yönergeler için [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).

3. CESARETLENDİRİCİ çalıştırın:

    ```powershell
      Stop-AzureStack
    ```

4. Devre dışı güç fiziksel tüm Azure Stack düğümlere bekleyin.

> [!Note]  
> Özgün ekipman üreticisi (kimin, Azure Stack donanımı tarafından sağlanan OEM) yönergeleri izleyerek bir fiziksel düğüm güç durumunu doğrulayabilirsiniz. 

## <a name="start-azure-stack"></a>Azure Stack Başlat 

Azure Stack, aşağıdaki adımlarla başlayın. Azure Stack nasıl durduruldu bağımsız olarak bu adımları izleyin.

1. Her Azure Stack ortamınıza fiziksel düğüm güç. Fiziksel düğümler için yönergeler gücüyle gelen orijinal ekipman üreticisi (kimin donanım, Azure Stack için sağlanan OEM) yönergeleri takip ederek doğrulayın.

2. Azure Stack altyapı hizmetleri başlatana kadar bekleyin. Azure Stack altyapı hizmetleri başlatma işlemini tamamlama için iki saat gerektirebilir. Azure Stack ile başlangıç durumunu doğrulayabilirsiniz [ **Get-ActionStatus** cmdlet'i](#get-the-startup-status-for-azure-stack).

3. Tüm Kiracı kaynaklarınızın bunlar önce kapatma olan durumuna döndüğünden emin olun. Kiracı kaynaklar üzerinde çalışan iş yüklerini başlatma işleminden sonra iş yükü Yöneticisi tarafından yeniden yapılandırılması gerekebilir.

## <a name="get-the-startup-status-for-azure-stack"></a>Azure Stack için başlangıç durumunu Al

Başlangıç için aşağıdaki adımlarla Azure Stack başlangıç yordamını alın:

1. Azure Stack ERCS Vm'leri için ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturum açın.

2. CESARETLENDİRİCİ çalıştırın:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Başlatma ve kapatma Azure Stack sorunlarını giderme

Altyapı ve Kiracı Hizmetleri, Azure Stack ortamınıza başarıyla 2 saat sonra güç başlatma, aşağıdaki adımları gerçekleştirin. 

1. Azure Stack ERCS Vm'leri için ağ erişimi olan bir makineden ayrıcalıklı bir uç nokta oturum açın.

2. Çalıştırın: 

    ```powershell
      Test-AzureStack
      ```

3. Çıktıyı gözden geçirin ve sistem durumu hataları çözün. Daha fazla bilgi için [Azure Stack bir doğrulama sınamasını çalıştırmanızı](azure-stack-diagnostic-test.md).

4. Çalıştırın:

    ```powershell
      Start-AzureStack
    ```

5. Çalışıyorsa **başlangıç AzureStack** bir hata sonuçlarında, Microsoft Müşteri Hizmetleri desteği ile iletişime geçin. 

## <a name="next-steps"></a>Sonraki adımlar 

Azure Stack'te Tanılama aracı hakkında daha fazla bilgi edinin ve günlük sorun için bkz: [Azure Stack'te tanılama araçları](azure-stack-diagnostics.md).
