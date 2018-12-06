---
title: Azure Stack geliştirme Seti'ni (ASDK) başlatıp | Microsoft Docs
description: Başlatın ve Azure Stack geliştirme Seti'ni (ASDK) aşağı kapatma hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0b1affb708a8d481a1378fa691b6547865b0b214
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52963598"
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Azure Stack geliştirme Seti'ni (ASDK) başlatıp
Yalnızca ASDK ana bilgisayarı yeniden başlatmak için önerilmez. Bunun yerine, düzgün bir şekilde kapatılmasını ve ASDK hizmetleri yeniden başlatmak için bu makaleyi yordamlarını izlemelisiniz. 

## <a name="stop-azure-stack"></a>Azure Stack Durdur 
Azure Stack hizmetlerinin ve ASDK ana bilgisayar doğru kapatmak için aşağıdaki PowerShell komutlarını kullanın:

1. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ardından, CESARETLENDİRİCİ oturumunda kullanın **Stop-AzureStack** ASDK ana bilgisayarı ve Azure Stack hizmetlerini durdurmak için cmdlet:

   ```powershell
   Stop-AzureStack
   ```
5. ASDK ana bilgisayar kapatılmadan önce tüm Azure Stack Hizmetleri başarılı bir şekilde kapatıldığından emin olmak için PowerShell çıkışını gözden geçirin. Kapatma işlemi birkaç dakika sürer.

## <a name="start-azure-stack"></a>Azure Stack Başlat 
ASDK Hizmetleri, ana bilgisayar başlatıldığında otomatik olarak başlayacaktır. Ancak, ASDK altyapı hizmetleri başlatma süresini ASDK ana bilgisayar donanım yapılandırması performansını göre değişir. Uygulamanın başarıyla bazı durumlarda yeniden başlatmak tüm hizmetleri için birkaç saat sürebilir.

ASDK nasıl kapatıldığı bağımsız olarak ana bilgisayar açılana sonra tüm Azure Stack hizmetleri kullanmaya başlama ve tam olarak işlevsel olduğunu doğrulamak için aşağıdaki adımları kullanmalısınız: 

1. Güç ASDK ana bilgisayarda. 
2. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
3. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
4. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Ardından, CESARETLENDİRİCİ oturumunda, Azure Stack hizmetlerinin başlangıç durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Azure Stack Hizmetleri başarıyla yeniden başlattığınızdan emin olmak için çıkışını gözden geçirin.

Düzgün bir şekilde kapatılmasını ve Azure Stack hizmetlerini yeniden başlatmak için önerilen yordamları hakkında daha fazla bilgi için bkz: [başlatma ve durdurma Azure Stack](../azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Başlatma ve kapatma sorunlarını giderme 
Azure Stack hizmetlerini, power ASDK ana bilgisayarınızda iki saat içinde başarıyla başlatma, aşağıdaki adımları gerçekleştirin:

1. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ardından, CESARETLENDİRİCİ oturumunda, Azure Stack hizmetlerinin başlangıç durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Test-AzureStack
   ```
5. Çıktıyı gözden geçirin ve hataları çözün. Daha fazla bilgi için [Azure Stack bir doğrulama sınamasını çalıştırmanızı](../azure-stack-diagnostic-test.md).
6. Azure Stack hizmetlerinden CESARETLENDİRİCİ oturumundan çalıştırarak yeniden **başlangıç AzureStack** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Çalışıyorsa **başlangıç AzureStack** ziyaret sonuçları, hataya [Azure Stack Destek Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) ASDK sorun giderme desteği almak için. 

## <a name="next-steps"></a>Sonraki adımlar 
Azure Stack'te Tanılama aracı hakkında daha fazla bilgi edinin ve günlük sorun için bkz: [Azure Stack'te tanılama araçları](../azure-stack-diagnostics.md).
