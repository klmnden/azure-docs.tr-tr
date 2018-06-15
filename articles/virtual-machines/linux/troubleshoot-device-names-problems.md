---
title: Linux VM aygıt adı değişiklikleri Azure sorunlarını giderme | Microsoft Docs
description: Linux VM aygıt değişiklik neden adları ve sorunun nasıl çözüleceği konusunda açıklanmaktadır.
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 1b500cc279cf9b92a3358d66e6db59dbf41d73b8
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34072360"
---
# <a name="troubleshoot-linux-vm-device-name-changes"></a>Linux VM aygıt adı değişiklikleri sorun giderme

Bu makalede, bir Linux VM yeniden başlatın veya veri diski yeniden sonra neden aygıt adlarını değiştirmek açıklanmaktadır. Makaleyi Ayrıca bu sorun için çözüm sağlar.

## <a name="symptoms"></a>Belirtiler
Microsoft Azure Linux VM'ler çalıştırırken aşağıdaki sorunlarla karşılaşabilirsiniz:

- VM yeniden başlatma sonrasında önyükleme başarısız olur.
- Veri diskleri ayrılmış ve yeniden yüklendiğinde disk aygıt adlarının değiştirilir.
- Aygıt adı değiştiği için bir uygulama veya cihaz adını kullanarak bir disk başvuruda bulunan komut dosyası başarısız olur.

## <a name="cause"></a>Nedeni

Linux yollarında cihaz yeniden başlatmaları arasındaki tutarlı olması garanti değil. Aygıt adlarının (harf) büyük sayılar ve küçük sayı oluşur. Linux depolama aygıtı sürücüsü yeni bir cihaz algıladığında, sürücü aygıta kullanılabilir aralıktan büyük ve küçük numaraları atar. Bir cihaz kaldırıldığında, cihaz numaralarını yeniden kullanılmak üzere kurtulurlar.

Linux Tarama aygıtı SCSI alt sistemi tarafından zaman uyumsuz olarak gerçekleşecek şekilde zamanlanır sorun oluşur. Sonuç olarak, bir cihaz yol adı yeniden başlatmaları arasında farklılık gösterebilir. 

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için kalıcı adlandırma kullanın. Kalıcı adlandırma kullanmak için dört yolu vardır: dosya sistemi etiketine göre UUID, Kimliğe göre veya yolu. Dosya sistemi etiketi veya UUID Azure Linux VM'ler için kullanmanızı öneririz. 

Çoğu dağıtımları sağlamak `fstab` **nofail** veya **nobootwait** parametreleri. Bu parametreler başlangıçta bağlamak disk başarısız olduğunda önyükleme sistemi etkinleştirin. Bu parametreler hakkında daha fazla bilgi için dağıtım belgelerinize bakın. Bir veri diski eklediğinizde bir UUID kullanmak için bir Linux VM yapılandırma hakkında daha fazla bilgi için bkz: [yeni diski Linux VM Bağlan](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Azure Linux Aracısı VM üzerinde yüklendiğinde, aracı Udev kuralları /dev/disk/azure yolunda sembolik bağlantılar kümesi oluşturmak için kullanılır. Uygulamalar ve komut dosyaları Udev kuralları disk türü ile birlikte VM'ye bağlı diskler ve disk LUN'ları tanımlamak için kullanın.

### <a name="identify-disk-luns"></a>Diski LUN'ları tanımlayın

Uygulamaları LUN bağlı disklerin tümünü bulmak ve sembolik bağlantılar oluşturmak için kullanın. Azure Linux Aracısı'nı sembolik bağlantısı bir LUN cihazlara Ayarla Udev kurallarını içerir:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 
LUN bilgileri Linux Konuk hesabı kullanarak alınır `lsscsi` veya benzer bir aracı:

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Konuk LUN bilgileri Azure aboneliği meta verileri ile Azure depolama alanında bölüm verileri içeren VHD bulmak için kullanılır. Örneğin, kullanabileceğiniz `az` CLI:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                            
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Dosya sistemi UUID'ler blkid kullanarak Bul

Uygulama ve komut çıktısı okuma `blkid`, veya benzer kaynakları bilgilerinin /dev yolundaki sembolik bağlantılar oluşturmak için. Çıkış VM ve bunların ilişkili aygıt dosyası bağlı tüm diskleri UUID'ler gösterir:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Azure Linux Aracısı Udev kuralları /dev/disk/azure yolunda sembolik bağlantılar kümesi oluşturun:

    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1

Uygulamaları önyükleme diski cihaz ve kaynak (kısa ömürlü) disk tanımlamak için bağlantıları kullanın. Azure'da, uygulamalar bu bölümler bulmak için /dev/disk/azure/root-part1 veya /dev/disk/azure-resource-part1 yollarında görünmelidir.

Tüm ek bölümlerden `blkid` listesi bir veri diski bulunur. Uygulamalar bu bölümler UUID'si korumak ve çalışma zamanında cihaz adını bulmak için bir yol kullanın:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-the-latest-azure-storage-rules"></a>En son Azure Storage kuralları Al

En son Azure Storage kuralları almak için aşağıdaki komutları çalıştırın:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block

## <a name="see-also"></a>Ayrıca bkz.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Ubuntu: UUID kullanma](https://help.ubuntu.com/community/UsingUUID)
- [Red Hat: Kalıcı adlandırma](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)
- [Linux: UUID'ler için yapabilecekleriniz](https://www.linux.com/news/what-uuids-can-do-you)
- [Udev: Aygıt Yönetimi modern Linux sistemindeki giriş](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

