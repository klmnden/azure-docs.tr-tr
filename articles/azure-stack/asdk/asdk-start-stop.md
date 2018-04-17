---
title: Başlatma ve durdurma Azure yığın Geliştirme Seti (ASDK) | Microsoft Docs
description: Başlangıç ve Azure yığın Geliştirme Seti (ASDK) aşağı kapatma hakkında bilgi edinin.
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
ms.date: 04/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: dfb565803746ecdda9b36a4e12a3c3f2b4d9e0d0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="start-and-stop-the-azure-stack-development-kit-asdk"></a>Başlatma ve durdurma Azure yığın Geliştirme Seti (ASDK)
Yalnızca ASDK ana bilgisayarı yeniden başlatmak için önerilmez. Bunun yerine, düzgün kapatılır ve ASDK hizmetlerini yeniden başlatmak için bu makaledeki yordamları izlemelisiniz. 

## <a name="stop-azure-stack"></a>Azure yığın Durdur 
Azure yığın Hizmetleri ve ASDK ana bilgisayar aşağı doğru kapatmak için aşağıdaki PowerShell komutlarını kullanın:

1. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın: 

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ardından, CESARETLENDİRİCİ oturumunda kullanmak **Stop-AzureStack** Azure yığın hizmetleri durdurun ve ASDK ana bilgisayarı kapatmak için cmdlet:

   ```powershell
   Stop-AzureStack
   ```
5. ASDK ana bilgisayar kapatılmadan önce tüm Azure yığın Hizmetleri başarılı bir şekilde kapatıldığından emin olmak için PowerShell çıkışı gözden geçirin. Kapatma işlemi birkaç dakika sürer.

## <a name="start-azure-stack"></a>Azure yığın Başlat 
ASDK Hizmetleri ana bilgisayar başlatıldığında otomatik olarak başlamanız gerekir. Ancak, ASDK altyapı hizmetleri başlangıç zamanını ASDK ana bilgisayar donanım yapılandırması performansını göre değişir. Tüm hizmetlerin başarılı bir şekilde bazı durumlarda yeniden birkaç saat sürebilir.

ASDK nasıl kapatıldığı bağımsız olarak ana bilgisayarın açık olduğundan sonra tüm Azure yığın Hizmetleri başlatıldı ve tam olarak işlevsel olduğunu doğrulamak için aşağıdaki adımları kullanmalısınız: 

1. Güç ASDK ana bilgisayar. 
2. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
3. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
4. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
5. Ardından, CESARETLENDİRİCİ oturumunda Azure yığın Hizmetleri başlatma durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Get-ActionStatus Start-AzureStack
   ```
6. Azure yığın Hizmetleri başarıyla yeniden başlattığınızdan emin olmak için çıktıyı inceleyin.

Düzgün kapatılır ve Azure yığın hizmetlerini yeniden başlatmak için önerilen yordamları hakkında daha fazla bilgi için bkz: [başlatma ve durdurma Azure yığın](.\.\azure-stack-start-and-stop.md). 

## <a name="troubleshoot-startup-and-shutdown"></a>Başlatma ve kapatma sorunlarını giderme 
Azure yığın Hizmetleri başarıyla ASDK ana bilgisayarınız gücüyle sonra iki saat içinde başlatma gerekiyorsa, aşağıdaki adımları gerçekleştirin:

1. AzureStack\CloudAdmin ASDK ana bilgisayarda oturum açın.
2. PowerShell'i yönetici olarak (PowerShell ISE değil) açın.
3. Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) oturum oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint
   ```
4. Ardından, CESARETLENDİRİCİ oturumunda Azure yığın Hizmetleri başlatma durumunu denetlemek için aşağıdaki komutları çalıştırın:

   ```powershell
   Test-AzureStack
   ```
5. Çıktıyı gözden geçirin ve varsa hataları çözümleyin. Daha fazla bilgi için bkz: [Azure yığınının bir doğrulama sınamasını çalıştırmanızı](.\.\azure-stack-diagnostic-test.md).
6. Azure yığın Hizmetleri'nden CESARETLENDİRİCİ oturumundan çalıştırarak yeniden **başlangıç AzureStack** cmdlet:

   ```powershell
   Start-AzureStack
   ```

Çalışıyorsa **başlangıç AzureStack** hataya, ziyaret [Azure yığın Destek Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurestack) sorun giderme desteği ASDK almak için. 

## <a name="next-steps"></a>Sonraki adımlar 
Azure yığın Tanılama aracı hakkında daha fazla bilgi edinin ve günlüğe kaydetme bkz [Azure yığın tanılama araçları](.\.\azure-stack-diagnostics.md).
