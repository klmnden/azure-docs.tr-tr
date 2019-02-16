---
title: Windows VM'ler için Azure Disk Depolama yönetilen disk genel bakış | Microsoft Docs
description: Depolama hesapları, Azure Windows sanal makineleri kullanırken işleme yönetilen diskler, Azure genel bakış
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.workload: storage
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/11/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 85b2dcb73024ce786b78436b7070ad2e9a96e1d4
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328479"
---
# <a name="introduction-to-azure-managed-disks"></a>Yönetilen diskleri azure'a giriş

Azure yönetilen diski olan bir sanal sabit disk (VHD). Bunu, ancak, sanallaştırılmış bir şirket içi sunucu fiziksel bir disk gibi düşünebilirsiniz. Azure yönetilen diskler, azure'da rastgele g/ç depolama nesnesi olan sayfa blobları olarak depolanır. ', Sayfa blobları, blob kapsayıcıları ve Azure depolama hesabı üzerinde bir Özet olduğundan yönetilen' yönetilen disk diyoruz. Yönetilen disklerle yapmanız gereken tek şey sağlama disk ve Azure geri kalanını üstlenir.

Yönetilen diskler ile iş yüklerinizi Azure kullanmasını seçtiğinizde, Azure oluşturur ve diski oluşturup yönetebilmesi. Kullanılabilir diskler Ultra katı hal sürücüleri (SSD) (Önizleme), Premium SSD, standart bir SSD ve standart Sabit Disk sürücülerinin (HDD) türleridir. Her bağımsız disk türü hakkında daha fazla bilgi için bkz. [Iaas VM'ler için bir disk türü seçin](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Iaas VM'ler için bir disk türü seçin](disks-types.md)