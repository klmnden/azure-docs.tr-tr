---
title: Azure VM'ler için bağlı diskler sorunlarını giderme | Microsoft Docs
description: Azure Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolamak için tasarlanmıştır. Uygulamalarınız, REST üzerinden veya Azure Storage istemci kitaplıkları aracılığıyla koddan, Azure CLI’dan ya da PowerShell’den Blob depolamadaki nesnelere erişebilir.
services: storage
author: genlin
manager: cshepard
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 766062b085c359499046151f337921a51d948715
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Azure VM'ler için bağlı diskler sorun giderme 

Azure sanal makineleri (VM'ler) bağlı sanal sabit disk (VHD) işletim sistemi diski ve diğer veri disklerde kullanır. VHD'ler, bir veya daha fazla Azure Storage hesapları sayfa bloblarını olarak depolanır. Bu makalede, içerik ile VHD'ler kaynaklanabilecek sık karşılaşılan sorunları için sorun giderme için işaret eder. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Bir VM için depolama silme hatalarıyla ilgili sorunları giderme

Bazı durumlarda, VHD bağlı bir Resource Manager dağıtımında VM içerdiğinde depolama kaynağı silme sırasında hatayla karşılaşabilirsiniz. Bu sorunu çözme hakkında Yardım için aşağıdaki makalelere bakın: 

  * Üzerinde Linux VM'ler: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Üzerinde Windows Vm'lerini: [Resource Manager dağıtımında depolama silme hataları](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Beklenmeyen yeniden başlatmalar VM'lerin ekli VHD ile ilgili sorunları giderme

Beklenmeyen yeniden başlatmalar VM çok sayıda ekli VHD ile karşılaşırsanız, aşağıdaki makaleler birine bakın:

  * Üzerinde Linux VM'ler: [beklenmeyen yeniden VM'lerin ekli VHD ile](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Üzerinde Windows Vm'lerini: [beklenmeyen yeniden VM'lerin ekli VHD ile](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
