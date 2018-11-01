---
title: Azure Vm'lere bağlı diskler sorunlarını giderme | Microsoft Docs
description: Sorun giderme kaynakları Azure sanal makine sanal sabit diskler (VHD'ler) için bağlantılar sağlar.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: bf1aa75399f28b8dd1732bc9ec09c15e46132939
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414183"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Azure Vm'lere bağlı diskler sorunlarını giderme 

Azure sanal makineleri (VM'ler) sanal sabit bağlı diskler (VHD'ler) ve her işletim sistemi diski için diskleri kullanır. VHD'ler sayfa BLOB'ları bir veya daha fazla Azure depolama hesaplarında depolanır. Bu makalede, içerik VHD'ler ile karşılaşabileceğiniz genel sorunları için sorun giderme için işaret eder. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Bir VM için depolama silme hatalarını giderme

Bazı durumlarda, bir Resource Manager dağıtımında bir VM içerdiğinde depolama kaynağı silme ekli VHD'ler sırada hatayla karşılaşabilirsiniz. Bu sorunu çözme konusunda yardım için aşağıdaki makalelerden birine bakın: 

  * Linux Vm'leri üzerinde: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Üzerinde Windows Vm'leri: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Ekli VHD'ler ile VM'lerin beklenmeyen yeniden başlatmaları sorunlarını giderme

Çok sayıda ekli VHD'ler ile sanal makine beklenmeyen yeniden başlatmaları karşılaşırsanız, aşağıdaki makalelerden birine bakın:

  * Linux Vm'leri üzerinde: [ekli VHD'ler ile VM'lerin beklenmeyen yeniden](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Üzerinde Windows Vm'leri: [ekli VHD'ler ile VM'lerin beklenmeyen yeniden](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
