---
title: Azure Vm'lere bağlı diskler sorunlarını giderme | Microsoft Docs
description: Sorun giderme kaynakları Azure sanal makine sanal sabit diskler (VHD'ler) için bağlantılar sağlar.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 81db3a819c28aab8f4b644a940eeb5e6c5ecf3d6
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307181"
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
