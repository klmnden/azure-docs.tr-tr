---
title: Linux VM'ler için Azure Disk Depolama yönetilen disk genel bakış | Microsoft Docs
description: Depolama hesapları, Linux sanal makineleri kullanırken işleme yönetilen diskler, Azure genel bakış
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 18dc1bd2eea232d0c2eb73d496dd4bd9d2d5016e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60613694"
---
# <a name="introduction-to-azure-managed-disks"></a>Yönetilen diskleri azure'a giriş

Azure yönetilen diski olan bir sanal sabit disk (VHD). Bunu, ancak, sanallaştırılmış bir şirket içi sunucu fiziksel bir disk gibi düşünebilirsiniz. Azure yönetilen diskler, azure'da rastgele g/ç depolama nesnesi olan sayfa blobları olarak depolanır. ', Sayfa blobları, blob kapsayıcıları ve Azure depolama hesabı üzerinde bir Özet olduğundan yönetilen' yönetilen disk diyoruz. Yönetilen disklerle yapmanız gereken tek şey sağlama disk ve Azure geri kalanını üstlenir.

Yönetilen diskler ile iş yüklerinizi Azure kullanmasını seçtiğinizde, Azure oluşturur ve diski oluşturup yönetebilmesi. Ultra yüksek disklerinin (Önizleme), premium katı hal sürücüsü (SSD), standart SSD disk kullanılabilir türleri olduğundan ve standart sabit disk sürücüleri (HDD). Her bağımsız disk türü hakkında daha fazla bilgi için bkz. [Iaas VM'ler için bir disk türü seçin](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Iaas VM'ler için bir disk türü seçin](disks-types.md)
