---
title: Azure Vm'lere bağlı diskler sorunlarını giderme | Microsoft Docs
description: Sorun giderme kaynakları Azure sanal makine sanal sabit diskler (VHD'ler) için bağlantılar sağlar.
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 10/31/2018
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: bf1aa75399f28b8dd1732bc9ec09c15e46132939
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098108"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Azure Vm'lere bağlı diskler sorunlarını giderme 

Azure sanal makineleri (VM'ler) sanal sabit bağlı diskler (VHD'ler) ve her işletim sistemi diski için diskleri kullanır. VHD'ler sayfa BLOB'ları bir veya daha fazla Azure depolama hesaplarında depolanır. Bu makalede, içerik VHD'ler ile karşılaşabileceğiniz genel sorunları için sorun giderme için işaret eder. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Bir VM için depolama silme hatalarını giderme

Bazı durumlarda, bir Resource Manager dağıtımında bir VM içerdiğinde depolama kaynağı silme ekli VHD'ler sırada hatayla karşılaşabilirsiniz. Bu sorunu çözme konusunda yardım için aşağıdaki makalelerden birine bakın: 

  * Linux Vm'lerinde: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Windows Vm'lerinde: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Ekli VHD'ler ile VM'lerin beklenmeyen yeniden başlatmaları sorunlarını giderme

Çok sayıda ekli VHD'ler ile sanal makine beklenmeyen yeniden başlatmaları karşılaşırsanız, aşağıdaki makalelerden birine bakın:

  * Linux Vm'lerinde: [Ekli VHD’ler ile VM’lerin beklenmeyen yeniden başlatmaları](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Windows Vm'lerinde: [Ekli VHD’ler ile VM’lerin beklenmeyen yeniden başlatmaları](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
