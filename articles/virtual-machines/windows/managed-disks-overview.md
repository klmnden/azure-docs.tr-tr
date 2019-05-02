---
title: Windows VM'ler için Azure Disk Depolama yönetilen disk genel bakış | Microsoft Docs
description: Depolama hesapları, Azure Windows sanal makineleri kullanırken işleme yönetilen diskler, Azure genel bakış
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.workload: storage
ms.tgt_pltfrm: vm-windows
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: fb1ee8556935b141dfee6a18c96ecafb476aa584
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725823"
---
# <a name="introduction-to-azure-managed-disks"></a>Yönetilen diskleri azure'a giriş

Azure yönetilen diski olan bir sanal sabit disk (VHD). Bunu, ancak, sanallaştırılmış bir şirket içi sunucu fiziksel bir disk gibi düşünebilirsiniz. Azure yönetilen diskler, azure'da rastgele g/ç depolama nesnesi olan sayfa blobları olarak depolanır. ', Sayfa blobları, blob kapsayıcıları ve Azure depolama hesabı üzerinde bir Özet olduğundan yönetilen' yönetilen disk diyoruz. Yönetilen disklerle yapmanız gereken tek şey sağlama disk ve Azure geri kalanını üstlenir.

Yönetilen diskler ile iş yüklerinizi Azure kullanmasını seçtiğinizde, Azure oluşturur ve diski oluşturup yönetebilmesi. Kullanılabilir diskler Ultra katı hal sürücüleri (SSD) (Önizleme), Premium SSD, standart bir SSD ve standart Sabit Disk sürücülerinin (HDD) türleridir. Her bağımsız disk türü hakkında daha fazla bilgi için bkz. [Iaas VM'ler için bir disk türü seçin](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Iaas VM'ler için bir disk türü seçin](disks-types.md)