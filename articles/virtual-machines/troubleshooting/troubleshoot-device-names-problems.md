---
title: Azure'da Linux VM cihaz adı değişikliklerle ilgili sorunları giderme | Microsoft Docs
description: Linux VM cihaz değişiklik neden adları ve sorunun nasıl çözüleceği konusunda açıklanmaktadır.
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: d636d5f31e78828a518882091af29b25f7219304
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443983"
---
# <a name="troubleshoot-linux-vm-device-name-changes"></a>Linux VM cihaz adı değişikliklerle ilgili sorunları giderme

Bu makalede, bir Linux VM'yi yeniden başlatın ya da veri disklerini yeniden sonra cihaz adları neden değişiyor açıklanmaktadır. Makalede, bu sorun için çözümleri de sağlanır.

## <a name="symptoms"></a>Belirtiler
Linux Vm'leri, Microsoft Azure'da çalıştırırken aşağıdaki sorunlarla karşılaşabilirsiniz:

- VM yeniden başlatma sonrası önyükleneceği başarısız olur.
- Veri diskleri kullanımdan çıkarıldı ve eklenemeyeceği disk cihaz adları değiştirildi.
- Cihaz adının değiştirilmesi için bir uygulama veya cihaz adı'nı kullanarak bir diski başvuran bir betik başarısız olur.

## <a name="cause"></a>Nedeni

Linux yollarında cihaz yeniden başlatmaları arasında tutarlı olması garanti değildir. Cihaz adları (harf) büyük sayılar ve küçük sayı oluşur. Linux depolama cihazı sürücüsünün yeni bir cihaz algıladığında, sürücü cihaza büyük ve küçük sayı kullanılabilir aralıktan atar. Bir cihaz kaldırıldığında, cihaz sayıları için yeniden kullanılması kurtulurlar.

Zaman uyumsuz olarak gerçekleştirilecek zamanlanmış tarama Linux'ta cihaz SCSI alt sistemi tarafından olduğundan sorun oluşur. Sonuç olarak, bir cihaz yolu adı yeniden başlatmaları arasında farklılık gösterebilir.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için kalıcı adlandırma kullanın. Kalıcı adlandırma kullanmak için dört yolu vardır: dosya sistemi etiketine göre UUID, Kimliğine göre veya yolu. Azure Linux Vm'leri için dosya sistemi etiket veya UUID kullanmanızı öneririz.

Çoğu dağıtımlar sağlamak `fstab` **nofail** veya **nobootwait** parametreleri. Bu parametreleri başlangıçta bağlanacak bir disk başarısız olduğunda önyükleme için bir sisteme olanak tanır. Bu parametreler hakkında daha fazla bilgi için dağıtım belgelerinize bakın. Bir veri diski eklediğinizde bir UUID'ye kullanmak üzere bir Linux VM yapılandırma hakkında daha fazla bilgi için bkz: [yeni disk bağlanacak Linux VM'ye bağlanma](../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).

Bir VM'de Azure Linux Aracısı yüklendiğinde, aracı Udev kuralları /dev/disk/azure yolunda simgesel bağlantılar gibi bir dizi oluşturmak için kullanır. Uygulamalar ve betiklerde Udev kuralları disk türü ile birlikte bir VM'ye bağlı diskler ve diskin LUN'ları tanımlamak için kullanın.

VM'niz değil önyükleme ve sanal Makinenize SSH oluşturulamıyor şekilde, fstab düzenlediyseniz, kullanabileceğiniz [VM seri konsol](./serial-console-linux.md) girmek için [tek kullanıcı modunda](./serial-console-grub-single-user-mode.md) ve, fstab değiştirin.

### <a name="identify-disk-luns"></a>Diskin LUN'ları tanımlama

Uygulamalar, tüm ekli disklerin bulmak ve simgesel bağlantılar oluşturmak için LUN'ları kullanın. Azure Linux Aracısı sembolik bağlantılar bir LUN cihazlar için ayarlanan Udev kurallar içerir:

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

LUN bilgileri Linux Konuk hesabı kullanılarak alınır `lsscsi` veya benzer bir araç:

      $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

Konuk LUN bilgileri Azure aboneliği meta verilerini Azure Depolama'da bölüm verileri içeren VHD bulmak için kullanılır. Örneğin, kullanabileceğiniz `az` CLI:

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

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Dosya sistemi UUID'ler blkid'ı kullanarak keşfedin

Uygulamalar ve betiklerde çıkışını okuma `blkid`, ya da benzer /dev yolunda simgesel bağlantılar oluşturmak için bilgi kaynakları. Çıktı, VM ve bunların ilişkili cihaz dosyasını bağlı tüm diskleri Uuıd'lerini gösterir:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

Azure Linux Aracısı Udev kuralları /dev/disk/azure yolunda simgesel bağlantılar gibi bir dizi oluşturun:

    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1

Uygulamalar, önyükleme diski cihaz ve kaynak (kısa ömürlü) diski tanımlamak için bağlantıları kullanın. Azure'da, uygulamaları bu bölümleri bulmak için /dev/disk/azure/root-part1 veya /dev/disk/azure-resource-part1 yollarında görünmelidir.

Herhangi bir ek bölümlerden `blkid` listesinde bulunan bir veri diski üzerinde. Uygulamalar için bu bölümleri UUID korumak ve çalışma zamanında cihaz adını bulmak için bir yol kullanın:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1


### <a name="get-the-latest-azure-storage-rules"></a>En son Azure depolama kurallarını Al

En son Azure depolama kuralları almak için aşağıdaki komutları çalıştırın:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block

## <a name="see-also"></a>Ayrıca bkz.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Ubuntu: UUID kullanma](https://help.ubuntu.com/community/UsingUUID)
- [Red Hat: Kalıcı adlandırma](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)
- [Linux: UUID'ler sizin için neler](https://www.linux.com/news/what-uuids-can-do-you)
- [Udev: Modern bir Linux sisteminde cihaz yönetimine giriş](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

