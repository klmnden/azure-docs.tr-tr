---
title: Azure Vm'lere bağlı diskler sorunlarını giderme | Microsoft Docs
description: Sorun giderme kaynakları Azure sanal makine sanal sabit diskler (VHD'ler) için bağlantılar sağlar.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/31/2018
ms.author: rogarana
ms.reviewer: wmgries
ms.openlocfilehash: c00fdf3ad02edc1faf0d3257d3836a1c2f44d682
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65150780"
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
