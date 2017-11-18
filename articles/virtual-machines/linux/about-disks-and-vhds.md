---
title: "Microsoft Azure Linux VM'ler için disk depolaması hakkında | Microsoft Docs"
description: "Azure'daki Linux sanal makineleri için disk ve VHD depolama temelleri hakkında bilgi edinin."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: robinsh
ms.openlocfilehash: ad55898806024a9f0562b32e7bdd990fd7dfd8d2
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="about-disk-storage-for-azure-linux-vms"></a>Azure Linux VM'ler için disk depolaması hakkında
Yalnızca başka bir bilgisayarda gibi azure'daki sanal makinelerde bir işletim sistemini, uygulamaları ve verileri depolamak için bir yer olarak diskleri kullanın. Tüm Azure sanal makineler en az iki disk – Linux işletim sistemi diski ve geçici bir diske sahip. İşletim sistemi diski bir görüntüden oluşturulur ve hem işletim sistemi diski ve görüntünün gerçekte sanal bir Azure depolama hesabında depolanan sabit diskler (VHD). Sanal makineler ayrıca VHD'ler olarak da depolanan bir veya daha fazla veri diski olabilir. 

Bu makalede, biz diskler için farklı kullanımlar hakkında konuşun ve oluşturma ve kullanma disklerinin farklı türleri açıklanmaktadır. Bu makalede ayrıca kullanılabilir [Windows sanal makineleri](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>VM'ler tarafından kullanılan diskler

Diskleri VM'ler tarafından nasıl kullanıldığı bir bakalım.

## <a name="operating-system-disk"></a>İşletim sistemi diski
Her bir sanal makinede bir ekli işletim sistemi diski var. SATA sürücüsü olarak kaydedilir ve/dev/sda varsayılan olarak etiketlenir. Bu disk 2048 gigabayt (GB) en fazla kapasiteye sahiptir. 

## <a name="temporary-disk"></a>Geçici disk
Her VM geçici disk içerir. Geçici disk uygulamalar ve işlemler için kısa vadeli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için kullanılması amaçlanmıştır. Geçici disk üzerindeki verileri kaybolabilir sırasında bir [bakım olayı](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) veya ne zaman, [VM yeniden](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sırasında standart yeniden başlatma VM geçici sürücüdeki verilerin kalıcı olması.

Linux sanal makineleri, genellikle disktir **/dev/sdb** ve biçimlendirilmiş ve bağlanan **/mnt** Azure Linux aracısı tarafından. Geçici diskin boyutunu, sanal makine boyutuna göre değişir. Daha fazla bilgi için bkz: [Linux sanal makineler için Boyutlar](../windows/sizes.md).

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz: [geçici sürücü Microsoft Azure sanal makineler üzerinde anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Veri diski
Uygulama verileri veya tutmak için gereksinim duyduğunuz diğer veri depolamak için bir sanal makineye bağlı VHD veri disktir. Veri diskleri SCSI sürücüsü olarak kaydedilir ve seçtiğiniz bir harf ile etiketlenir. Her veri diski maksimum 4095 GB kapasitesine sahiptir. Kaç tane veri diskleri ve depolama türü için iliştirebilirsiniz diskleri barındırmak için kullanabileceğiniz sanal makine boyutunu belirler.

> [!NOTE]
> Sanal makineler kapasiteleri hakkında daha fazla ayrıntı için [Linux sanal makineler için Boyutlar](../windows/sizes.md).
> 

Bir görüntüden sanal makine oluşturduğunuzda azure bir işletim sistemi diski oluşturur. Veri diskleri içeren bir görüntü kullanırsanız, sanal makine oluştururken Azure ayrıca veri diskleri oluşturur. Aksi halde, sanal makineyi oluşturduktan sonra veri diski ekleyin.

Veri diski bir sanal makine için herhangi bir zamanda göre ekleyebileceğiniz **ekleme** sanal makineye disk. Karşıya veya depolama hesabınız veya bir Azure sizin için oluşturduğu kopyalanan VHD kullanabilirsiniz. Bir veri diski eklemeyi hala bağlıyken depolama biriminden silinemez şekilde VHD 'kira' koyarak VHD dosyasını VM ile ilişkilendirir.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir diski kullanıma açın](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM için ek depolama alanı eklemek için.
* [Anlık Görüntü](snapshot-copy-managed-disk.md).
* [Yönetilen Diske Dönüştür](convert-unmanaged-to-managed-disks.md).

