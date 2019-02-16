---
title: Linux VM'ler için Azure Disk Depolama yönetilen disk genel bakış | Microsoft Docs
description: Depolama hesapları, Linux sanal makineleri kullanırken işleme yönetilen diskler, Azure genel bakış
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/11/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d8394121019338b53c87f7229d0ccf52fbdf8f21
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327036"
---
# <a name="introduction-to-azure-managed-disks"></a>Yönetilen diskleri azure'a giriş

Azure yönetilen diski olan bir sanal sabit disk (VHD). Bunu, ancak, sanallaştırılmış bir şirket içi sunucu fiziksel bir disk gibi düşünebilirsiniz. Azure yönetilen diskler, azure'da rastgele g/ç depolama nesnesi olan sayfa blobları olarak depolanır. ', Sayfa blobları, blob kapsayıcıları ve Azure depolama hesabı üzerinde bir Özet olduğundan yönetilen' yönetilen disk diyoruz. Yönetilen disklerle yapmanız gereken tek şey sağlama disk ve Azure geri kalanını üstlenir.

Yönetilen diskler ile iş yüklerinizi Azure kullanmasını seçtiğinizde, Azure oluşturur ve diski oluşturup yönetebilmesi. Ultra yüksek disklerinin (Önizleme), premium katı hal sürücüsü (SSD), standart SSD disk kullanılabilir türleri olduğundan ve standart sabit disk sürücüleri (HDD). Her bağımsız disk türü hakkında daha fazla bilgi için bkz. [Iaas VM'ler için bir disk türü seçin](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Iaas VM'ler için bir disk türü seçin](disks-types.md)
