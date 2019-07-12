---
title: Bir Windows AWS Vm'leri Azure'a taşıma | Microsoft Docs
description: Amazon Web Services (AWS) EC2 Windows örneği için bir Azure sanal makinesi taşıyın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: bc738a33ba50935a2118b8bd0bbfafed83e5f461
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722785"
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-an-azure-virtual-machine"></a>Bir Windows VM Amazon Web Services'dan (AWS) için bir Azure sanal makinesi taşıyın.

İş yüklerinizi barındırmak için Azure sanal makineleri değerlendiriyorsanız mevcut Amazon Web Services (AWS) EC2 Windows VM örneği dışarı sonra sanal sabit disk (VHD) Azure'a yükleyin. VHD'yi karşıya yüklendikten sonra VHD'den azure'da yeni bir VM oluşturabilirsiniz. 

Bu makalede tek bir VM AWS'den Azure'a taşıma anlatılmaktadır. Vm'leri AWS'den Azure'a ölçekte taşımak istiyorsanız, bkz. [Amazon Web Services (AWS) sanal makineler Azure Site Recovery ile Azure'a geçiş](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-the-vm"></a>VM’yi hazırlama 
 
Azure için genelleştirilmiş ve özelleştirilmiş VHD yükleyebilirsiniz. Her tür AWS'den dışarı aktarmadan önce VM'yi hazırlama gerektirir. 

- **Genelleştirilmiş VHD** -genelleştirilmiş VHD tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırılan oluşturdu. VHD, yeni sanal makineler oluşturmak için bir görüntü olarak kullanmak istiyorsanız, şunları yapmalısınız: 
 
    * [Bir Windows VM hazırlama](prepare-for-upload-vhd-image.md).  
    * Sanal makine Sysprep kullanarak genelleştirin.  

 
- **Özelleştirilmiş VHD** -özelleştirilmiş bir VHD'yi kullanıcı hesaplarını, uygulamaları ve orijinal VM'yi diğer Durum verilerini tutar. VHD olarak kullanmak istiyorsanız,-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandığından emin olun.  
    * [Windows Azure'a karşıya yüklenecek VHD'yi hazırlama](prepare-for-upload-vhd-image.md). **Sağlamadığı** Sysprep kullanarak VM'yi Genelleştirme. 
    * Herhangi bir konuk sanallaştırma araçları ve (yani, VMware araçları) VM'de yüklü aracıları kaldırın. 
    * VM, IP adresi ve DNS ayarlarını DHCP yoluyla çekmek için yapılandırılan emin olun. Bu, başladığında sunucunun sanal ağ içindeki bir IP adresi alacağını sağlar.  


## <a name="export-and-download-the-vhd"></a>Dışarı aktarma ve VHD'yi indirin 

EC2 örneği, bir Amazon S3 demetini VHD dışarı aktarın. Amazon belgeleri makaledeki adımları izleyin [örneği bir VM kullanarak VM'yi içeri/dışarı aktarma olarak dışarı aktarma](https://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) çalıştırıp [oluşturma-örnek-dışarı aktarma-görev](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) EC2 örneği için bir VHD dosyasını dışarı aktarmak için komutu. 

Dışarı aktarılan VHD dosyasının belirttiğiniz Amazon S3 demetini kaydedilir. Temel sözdizimi aşağıda olan VHD dışarı aktarma için yalnızca değiştirmek yer tutucu metni \<ayraçlar > bilgilerinizle.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

VHD dışarı sonra yönergeleri izleyin [nasıl indiririm bir nesne bir S3 Demetini?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) S3 demetini VHD dosyasını indirmek için. 

> [!IMPORTANT]
> VHD indirme ücretleri AWS ücretleri veri aktarımı. Bkz: [Amazon S3 fiyatlandırma](https://aws.amazon.com/s3/pricing/) daha fazla bilgi için.


## <a name="next-steps"></a>Sonraki adımlar

Artık Azure'a VHD yükleme ve yeni bir VM oluşturun. 

- Kaynağınıza Sysprep çalıştırdıysanız **generalize** dışarı aktarma, önce bkz [genelleştirilmiş VHD yükleme ve Azure'da yeni bir VM oluşturmak için bunu kullanın](upload-generalized-managed.md)
- Dışarı aktarmadan önce Sysprep çalıştırmıyorsa, VHD olarak kabul edilir **özelleştirilmiş**, bkz: [Azure'a özelleştirilmiş bir VHD'yi karşıya yükleme ve yeni VM oluşturma](create-vm-specialized.md)

 
