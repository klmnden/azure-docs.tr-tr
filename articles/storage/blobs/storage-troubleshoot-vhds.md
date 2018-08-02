---
title: Azure Vm'lere bağlı diskler sorunlarını giderme | Microsoft Docs
description: Azure Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolamak için tasarlanmıştır. Uygulamalarınız, REST üzerinden veya Azure Storage istemci kitaplıkları aracılığıyla koddan, Azure CLI’dan ya da PowerShell’den Blob depolamadaki nesnelere erişebilir.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.component: disks
ms.openlocfilehash: 0dbd89c28d18d64908d92cd38d8bd1cf3138fd5c
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397976"
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
