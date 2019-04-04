---
title: Azure stack'teki fiziksel disk değiştirme | Microsoft Docs
description: Azure Stack fiziksel diskleri değiştirme işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.lastreviewed: 01/22/2019
ms.openlocfilehash: a66217641c833061d4626b7d393fd3cdd0fd56cc
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483618"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Azure stack'teki fiziksel disk değiştirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack'te fiziksel diski değiştirmek için genel süreç açıklanır. Bir fiziksel disk başarısız olursa, olabildiğince çabuk değiştirmelisiniz.

Tümleşik sistemler ve kullanılan diskleri olan Geliştirme Seti dağıtımlar için bu yordamı kullanabilirsiniz.

Gerçek disk değiştirme adımları farklılık gösterir, orijinal ekipman üreticisi (OEM) donanım satıcınıza temel. Sisteme özgü ayrıntılı adımlar için satıcınızın alan bulunduğu yerde değiştirilebilen biriminin (FRU) belgelerine bakın.

## <a name="review-disk-alert-information"></a>Disk uyarı bilgileri gözden geçirin
Bir disk başarısız olduğunda, bir fiziksel disk için bağlantı kaybedildi bildiren bir uyarı alırsınız.

 ![Fiziksel diske uyarı gösteren bağlantısı kesilmiş](media/azure-stack-replace-disk/DiskAlert.png)

Uyarı açarsanız, uyarı açıklaması ölçek birimi düğüm ve tam fiziksel yuvası konumunu değiştirmeniz gereken diski içerir. Daha fazla Azure Stack LED göstergesi özelliklerini kullanarak hatalı bir diski tanımlamanıza yardımcı olur.

## <a name="replace-the-disk"></a>Disk değiştirme

Gerçek disk değiştirme OEM donanım satıcınızın FRU yönergelerini izleyin.

> [!note]
> Bir ölçek birimi düğümü disklerinde aynı anda değiştirin. Sanal disk onarma işleri sonraki ölçek birimi düğüme geçmeden önce tamamlanması bekle

Tümleşik bir sistemde desteklenmeyen bir disk kullanımını önlemek için sistemi satıcınız tarafından desteklenmeyen diskleri engeller. Desteklenmeyen bir disk kullanmayı denerseniz, bir diski bir desteklenmeyen model veya üretici yazılımı nedeniyle karantinaya alındı, yeni bir uyarı bildirir.

Disk değiştirdikten sonra Azure Stack, otomatik olarak yeni disk bulur ve sanal disk onarım işlemini başlatır.
 
## <a name="check-the-status-of-virtual-disk-repair"></a>Sanal disk onarma durumunu denetleyin
 
 Disk değiştirdikten sonra sanal disk sistem durumunu izleyebilir ve ayrıcalıklı uç nokta kullanarak işin ilerleme durumunu onarın. Ayrıcalıklı uç noktasına ağ bağlantısı olan herhangi bir bilgisayardan aşağıdaki adımları izleyin.

1. Bir Windows PowerShell oturumu açın ve ayrıcalıklı uç noktasına bağlanın.
    ```powershell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ``` 
  
2. Sanal disk durumunu görüntülemek için aşağıdaki komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster
    ```
   ![PowerShell Get-VirtualDisk komutunun çıktısı](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Geçerli depolama iş durumunu görüntülemek için aşağıdaki komutu çalıştırın:
    ```powershell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ```
      ![PowerShell Get-StorageJob komutunun çıktısı](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Sanal disk onarım sorunlarını giderme

Sanal disk onarma, işi takılan, olarak gösterilir ve işi yeniden başlatmak için aşağıdaki komutu çalıştırın:
  ```powershell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ``` 
