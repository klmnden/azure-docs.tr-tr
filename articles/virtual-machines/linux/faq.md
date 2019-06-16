---
title: Azure'da Linux VM'ler için sık sorulan sorular | Microsoft Docs
description: Resource Manager modeli kullanılarak oluşturulmuş bir Linux sanal makineleri hakkında genel soruların yanıtlarını sağlar.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 0623a7aff15184822ee8abde0b3c751f8a105b5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65463575"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Linux sanal makineleri hakkında sık sorulan sorular
Bu makalede Azure Resource Manager dağıtım modeli kullanılarak oluşturulan Linux sanal makineleri hakkında bazı yaygın sorular ele alınmıştır. Bu konuda Windows sürümü için bkz: [Windows sanal makineleri hakkında sık sorulan soruya](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Daha fazla bilgi için [Azure-Endorsed dağıtımlarda Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski 4 TB'a kadar (4.095 GB) olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure yönetilen diskler önerilen disk depolama kullanmak için Azure sanal makineler ile veri kalıcı depolama için tekliflerdir. Her bir Sanal Makine ile birden fazla Yönetilen Disk kullanabilirsiniz. Yönetilen diskler teklif iki tür dayanıklı depolama seçeneği Premium ve standart yönetilen diskler. Fiyatlandırma bilgileri için bkz: [yönetilen diskler fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

Azure depolama hesapları, aynı zamanda işletim sistemi diski ve varsa veri diskleri için depolama sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal Makinem nasıl erişebilirim?
Güvenli Kabuk (SSH) kullanarak sanal makineye, oturum açmak için bir uzak bağlantı kurun. Bağlanma konusunda yönergelere bakın [Windows gelen](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [Linux ve Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Varsayılan olarak, SSH en fazla 10 eş zamanlı bağlantıya izin verir. Yapılandırma dosyasını düzenleyerek bu sayıyı artırabilirsiniz.

Sorun yaşıyorsanız, kullanıma [sorun giderme güvenli Kabuk (SSH) bağlantılarında](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Verileri depolamak için geçici diski (/ dev/sdb1) kullanabilir miyim?
Verileri depolamak için geçici diski (/ dev/sdb1) kullanmayın. Bunu yalnızca geçici depolama için yoktur. Kurtarılamaz veri kaybedebilirsiniz.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Ben kopyalayabilir veya mevcut bir Azure VM'yi kopyalama?
Evet. Yönergeler için [Resource Manager dağıtım modelinde bir Linux sanal makinesinin kopyasını oluşturma](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Neden Kanada Orta ve Kanada Doğu bölgelerinde Azure Resource Manager aracılığıyla görüyorum değil mi?
Kanada Orta ve Kanada Doğu, iki yeni bölgede otomatik olarak mevcut bir Azure aboneliği için sanal makine oluşturmak için kayıtlı değil. Azure Resource Manager kullanarak başka bir bölgeye Azure portalı üzerinden bir sanal makine dağıtıldığında bu kayıt otomatik olarak gerçekleştirilir. Diğer Azure bölgesi için bir sanal makine dağıtıldıktan sonra yeni bölgelere sonraki sanal makineler için kullanılabilir olması gerekir.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra sanal Makinem için bir NIC ekleyebilirim?
Evet, bu artık mümkündür. VM'nin ilk durdurulup serbest gerekir. Ardından ekleyebilir veya bir NIC (son NIC VM üzerinde olmadığı sürece) kaldırın. 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 64 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions) kaynaklarınızı adlandırma etrafında daha fazla bilgi için.

## <a name="are-there-any-resource-group-name-requirements"></a>Herhangi bir kaynak grubu adı gereksinimleri vardır?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions) kaynak grupları hakkında daha fazla bilgi için.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Bir VM oluştururken, kullanıcı adı gereksinimleri nelerdir?

Kullanıcı adlarını, 1-32 karakter uzunluğunda olmalıdır.

Aşağıdaki kullanıcı adlarını izin verilmez:

| | | | |
|-----------------|-----------|--------------------|----------|
| `administrator` | `admin`   | `user`             | `user1`  |
| `test`          | `user2`   | `test1`            | `user3`  |
| `admin1`        | `1`       | `123`              | `a`      |
| `actuser`       | `adm`     | `admin2`           | `aspnet` |
| `backup`        | `console` | `david`            | `guest`  |
| `john`          | `owner`   | `root`             | `server` |
| `sql`           | `support` | `support_388945a0` | `sys`    |
| `test2`         | `test3`   | `user4`            | `user5`  |

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Bir VM oluşturulurken parola gereksinimleri nelerdir?

Uzunluk gereksinimlerini, kullanmakta olduğunuz aracı bağlı olarak değişen parola vardır:
 - Portalı - 12-72 karakter
 - PowerShell - 8-123 karakter arasında
 - 12-123 arasında - CLI
 

Parolaları da 3 şu 4 karmaşıklık gereksinimini karşılamalıdır:

* Daha düşük karakter içerebilir
* Üst karakter içerebilir
* Bir rakam olması
* Sahip bir özel karakter (Regex eşleşmesi [\W_])

Aşağıdaki parolalara izin verilmez:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Parola!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU veya!</td>
    </tr>
</table>
