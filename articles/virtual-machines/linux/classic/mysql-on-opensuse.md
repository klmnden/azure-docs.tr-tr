---
title: "MySQL bir OpenSUSE VM olanağına yükle | Microsoft Docs"
description: "Azure'da OpenSUSE Linux VMirtual makine MySQL yüklemek öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Azure'da OpenSUSE Linux çalıştıran bir sanal makineye MySQL yükleme
[MySQL] [ MySQL] popüler, açık kaynaklı bir SQL veritabanı. Bu öğretici OpenSUSE Linux çalıştıran bir sanal makine oluşturun ve MySQL yükleme gösterilmektedir.

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linux çalıştıran bir sanal makine oluşturma
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Yükleme ve sanal makinede MySQL çalıştırma
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Sonraki adımlar
MySQL hakkında daha fazla ayrıntı için bkz: [MySQL belgeleri][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

