---
title: Bir Windows AWS Vm'lerini Azure'a taşımak | Microsoft Docs
description: Amazon Web Hizmetleri (AWS) EC2 Windows örneğini Azure PowerShell kullanarak Azure sanal makineleri için taşıyın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
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
ms.openlocfilehash: cb5b68e7bd0a1b247327e7147fe38eae19395f50
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726542"
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-to-azure-using-powershell"></a>Bir Windows VM Amazon Web Hizmetleri (AWS) için Azure PowerShell kullanarak Taşı

İş yüklerini barındırmak için Azure sanal makineleri değerlendirirken, mevcut Amazon Web Hizmetleri (AWS) EC2 Windows VM örneği dışarı sonra sanal sabit disk (VHD) Azure'a yükleyin. VHD yüklendikten sonra VHD'den azure'da yeni bir VM oluşturun. 

Bu makalede, tek bir VM'ye AWS Azure'a taşıma yer almaktadır. VM'ler AWS ölçekte Azure'a taşımak istiyorsanız, bkz: [sanal makineleri Amazon Web Hizmetleri (AWS) Azure Site Recovery ile Azure'a geçirmek](../../site-recovery/site-recovery-migrate-aws-to-azure.md).

## <a name="prepare-the-vm"></a>VM’yi hazırlama 
 
Azure'a genelleştirilmiş ve özelleştirilmiş VHD'leri karşıya yükleyebilirsiniz. Her tür AWS dışarı aktarmadan önce VM hazırlama gerektirir. 

- **VHD genelleştirilmiş** -genelleştirilmiş bir VHD tüm kişisel hesap bilgilerinizi Sysprep kullanılarak kaldırılıncaya oluşturdu. Yeni VM'lerin oluşturmak için bir resim olarak VHD kullanmayı düşünüyorsanız, aşağıdakileri yapmalısınız: 
 
    * [Bir Windows VM hazırlama](prepare-for-upload-vhd-image.md).  
    * Sysprep kullanarak sanal makine genelleştirin.  

 
- **Özelleştirilmiş VHD** -özel bir VHD kullanıcı hesapları, uygulamaları ve diğer özgün VM Durum verilerini korur. VHD olarak kullanmak istiyorsanız-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandıktan emin olun.  
    * [Windows Azure'a yüklemeniz VHD hazırlama](prepare-for-upload-vhd-image.md). **Sağlamadığı** Sysprep kullanarak VM genelleştirin. 
    * Konuk sanallaştırma araçları ve VM'de (yani VMware araçları) yüklü aracıları kaldırın. 
    * VM kendi IP adresini ve DNS ayarlarını DHCP yoluyla isteyecek şekilde yapılandırıldığından emin olun. Bu başladığında sunucu VNet içindeki bir IP adresi alacağını sağlar.  


## <a name="export-and-download-the-vhd"></a>Dışarı aktarma ve VHD indirin 

EC2 örneği bir Amazon S3 demetini VHD'yi verin. Amazon belgelerine makalesindeki adımları izleyin [örneği bir VM kullanarak VM içeri/dışarı aktarma olarak dışarı aktarma](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) çalıştırıp [-örnek-export-görev oluştur](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) EC2 örneği bir VHD dosyasına aktarmak için komutu. 

Dışarı aktarılan VHD dosyasının belirttiğiniz Amazon S3 demetini kaydedilir. Temel sözdizimi oranın altında VHD dışarı aktarma için yalnızca değiştirmesini yer tutucu metni <brackets> bilgilerinizi ile.

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

VHD dışarı aktarıldıktan sonra ' ndaki yönergeleri izleyin [nasıl karşıdan bir nesne S3 Demetini?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) VHD dosyasını S3 demetini karşıdan yüklemek için. 

> [!IMPORTANT]
> VHD indirme ücretleri AWS ücretleri veri aktarın. Bkz: [Amazon S3 fiyatlandırma](https://aws.amazon.com/s3/pricing/) daha fazla bilgi için.


## <a name="next-steps"></a>Sonraki adımlar

Artık Azure'a VHD yükleyin ve yeni bir VM oluşturun. 

- Kaynağınız için Sysprep çalıştırdıysanız **generalize** dışarı aktarmadan önce görmek [genelleştirilmiş bir VHD yüklemek ve yeni sanal makineleri oluşturmak için kullanın](upload-generalized-managed.md)
- Sysprep dışarı aktarmadan önce çalıştırmadıysanız VHD olarak kabul edilir **özel**, bkz: [Azure'a özel bir VHD yükleyin ve yeni bir VM oluşturun](create-vm-specialized.md)

 
