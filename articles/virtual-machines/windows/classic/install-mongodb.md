---
title: "Windows Azure VM MongoDB yükleyin. | Microsoft Docs"
description: "Windows Server çalıştıran Klasik dağıtım modeli kullanılarak oluşturulmuş bir Azure VM MongoDB yüklemeyi öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Windows Azure VM MongoDB yükleyin.
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md).  Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Yükleme ve Resource Manager dağıtım modelini kullanarak MongoDB yapılandırmak için bkz: [bu makalede](../install-mongodb.md).

[MongoDB] [ MongoDB] bir popüler açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makalede, Windows Server kullanarak sanal makine (VM) oluşturma size rehberlik eder [Azure portal][AzurePortal]. Ardından oluşturun ve VM yükleyip MongoDB yapılandırmadan önce bir veri diski ekleyin. Kullanmak istediğiniz Azure içinde mevcut bir VM'yi varsa, doğrudan atlayabilirsiniz [yükleme ve yapılandırma MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Windows Server çalıştıran bir sanal makine oluşturma
Bir sanal makine oluşturmak için bu yönergeleri izleyin.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Sanal makine oluşturulurken MongoDB için bir uç nokta ekleyin ve aşağıdaki gibi yapılandırın: olarak adlandırın **Mongo**, kullanın **TCP** protokol olarak ve her iki ortak ve özel bağlantı noktası kümesine  **27017**.
>
>

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Sanal makine için depolama sağlamak için bir veri diski ekleyin ve Windows kullanabilmesi başlatılamıyor. Bir veri diski varsa, bu varolan bir diski ekleyebilirsiniz ya da boş bir disk ekleyin.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Diski başlatma ile ilgili yönergeler için bkz: "nasıl yapılır: Windows Server'da yeni bir veri diski başlatın" içinde [Windows sanal makine için bir veri diski eklemek nasıl](attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Yükleme ve sanal makinede MongoDB çalıştırma
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Özet
Bu öğreticide, Windows Server çalıştıran bir sanal makine oluşturma, uzaktan bağlanmak ve bir veri diskini öğrendiniz.  Aynı zamanda MongoDB Windows tabanlı sanal makineye yükleyip öğrendiniz. Gelişmiş konular, izleyerek MongoDB Windows tabanlı sanal makine artık erişebilir [MongoDB belgelerine][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

