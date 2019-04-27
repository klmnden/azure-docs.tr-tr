---
title: Bir Azure sanal makinesi önyükleme yaparken dosya sistemi denetleniyor | Microsoft Docs
description: VM gösterildiğini denetleme dosya sistemi önyükleme yaparken sorunu çözmeyi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 51a97443f6b9ba2a37fa2db708b8520a9c450000
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594813"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>"Dosya sistemi denetleniyor" Windows gösterir bir Azure sanal makinesi önyükleme yaparken

Bu makalede, Microsoft Azure'da Windows sanal makinesi (VM) önyüklediğinizde karşılaşabileceğiniz "dosya sistemi denetleniyor" bir hata oluştu.

> [!NOTE] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz Resource Manager dağıtım modeli kullanılarak açıklanır.

## <a name="symptom"></a>Belirti 

Bir Windows VM başlamaz. Ne zaman iade önyükleme ekran görüntüleri [önyükleme tanılaması](boot-diagnostics.md), Disk denetleme işlemi (chkdsk.exe) şu iletilerden biri ile çalıştığını görürsünüz:

- Tarama ve onarma sürücünüz (C:)
- C: üzerindeki dosya sistemi denetleniyor

## <a name="cause"></a>Nedeni

Dosya sistemindeki bir NTFS hata bulunursa, Windows denetleyin ve sonraki yeniden başlatmada disk tutarlılığını onarın. Bu genellikle VM beklenmeyen herhangi bir yeniden başlatma olsaydı ya da VM kapatma işlemi beklenmedik biçimde kesildi gerçekleşir.

## <a name="solution"></a>Çözüm 

Windows Disk denetleme işlemi tamamlandıktan sonra normal önyükleme yapmaz. VM Disk denetleme işlemde takılıyorsa Check Disk çevrimdışı VM üzerinde çalıştırmayı deneyin:
1.  Etkilenen sanal makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).
2.  [İşletim sistemi diskini bir kurtarma VM'si ekleme](troubleshoot-recovery-disks-portal-windows.md).  
3.  Kurtarma sanal makinesinde kontrol Disk ekli işletim sistemi diskini üzerinde çalıştırın. Aşağıdaki örnekte, ekli işletim sistemi diskinin sürücü harfi E: olur. 
        
        chkdsk E: /f
4.  Check Disk tamamlandıktan sonra diski kurtarma VM'sini kullanımdan çıkarın ve sonra disk bir işletim sistemi diski olarak etkilenen sanal makineye yeniden ekleyin. Daha fazla bilgi için [işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek bir Windows sanal makinesinin sorunlarını giderme](troubleshoot-recovery-disks-portal-windows.md).
