---
title: Azure Linux VM'ler için sık sorulan sorular | Microsoft Docs
description: Bazı Linux sanal makineleri Resource Manager modeli kullanılarak oluşturulmuş ilgili sık sorulan soruların yanıtlarını içerir.
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
ms.date: 03/22/2018
ms.author: cynthn
ms.openlocfilehash: 8a4d93ff12affac56c12c0eab85168c609400ee2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30242447"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Linux sanal makineleri hakkında sık sorulan sorular
Bu makalede Azure Resource Manager dağıtım modeli kullanılarak oluşturulan Linux sanal makineleri hakkında bazı sık sorulan soruları giderir. Bu konu Windows sürümü için bkz: [Windows sanal makineler hakkında sık sorulan bir soru](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Daha fazla bilgi için bkz: [Azure-Endorsed dağıtımları Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski en fazla 4 TB (4.095 GB) olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure yönetilen önerilen disk depolama teklifleri kullanmak için Azure Virtual Machines ile kalıcı depolama alanı için disklerdir. Her bir Sanal Makine ile birden fazla Yönetilen Disk kullanabilirsiniz. Yönetilen Diskler iki tür dayanıklı depolama seçeneği sunar: Premium ve Standart Yönetilen Diskler. Fiyatlandırma bilgileri için bkz: [yönetilen diskleri fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

Azure depolama hesapları, aynı zamanda işletim sistemi diski ve veri diskleri için depolama sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal Makinem nasıl erişebilir mi?
Güvenli Kabuk (SSH) kullanarak sanal makine için oturum açmak için Uzak bağlantı kurun. Bağlanma konusunda yönergelere bakın [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [Linux ve Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Varsayılan olarak, SSH en fazla 10 eş zamanlı bağlantıya izin verir. Yapılandırma dosyasını düzenleyerek bu sayıyı artırabilirsiniz.

Sorun yaşıyorsanız, kullanıma [sorun giderme güvenli Kabuk (SSH) bağlantı](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Verileri depolamak için geçici disk (/ dev/sdb1) kullanabilir miyim?
Geçici disk (/ dev/sdb1) verileri depolamak için kullanmayın. Yalnızca geçici depolama için yoktur. Kurtarılamaz veri kaybı riski oluşur.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>I kopyalayabilir veya mevcut bir Azure VM'yi kopyalama?
Evet. Yönergeler için bkz: [Resource Manager dağıtım modelinde bir Linux sanal makinenin bir kopyasını oluşturmak nasıl](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Neden Kanada merkezi ve Doğu Kanada bölgeler arasında Azure Resource Manager görüyorum değil mi?
Kanada Orta ve Doğu Kanada iki yeni bölgeleri otomatik olarak mevcut Azure abonelikleri için sanal makine oluşturmak için kayıtlı değil. Bu kayıt, Azure Kaynak Yöneticisi'ni kullanarak diğer bir bölge için Azure portal aracılığıyla bir sanal makine dağıtıldığında otomatik olarak gerçekleştirilir. Diğer Azure bölgesi için bir sanal makine dağıtıldıktan sonra yeni bölgeler sonraki sanal makineler için kullanılabilir olması gerekir.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra VM'im için bir NIC ekleyebilirim?
Evet, bu artık mümkündür. VM ilk durdurulması deallocated gerekir. Ardından ekleyebilir veya bir NIC (son NIC VM üzerinde olmadığı sürece) kaldırabilirsiniz. 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 64 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve sınırlamaları](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynaklarınızı adlandırma geçici daha fazla bilgi için.

## <a name="are-there-any-resource-group-name-requirements"></a>Herhangi bir kaynak grubu adı gereksinimleri var mı?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve sınırlamaları](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kaynak grupları hakkında daha fazla bilgi için.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Bir VM oluşturulurken kullanıcıadı gereksinimleri nelerdir?

Kullanıcı adları, 1-32 karakter uzunluğunda olmalıdır.

Aşağıdaki kullanıcı adları izin verilmiyor:

<table>
    <tr>
        <td style="text-align:center">Yönetici </td><td style="text-align:center"> yönetici </td><td style="text-align:center"> kullanıcı </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> KULLANICI3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> a</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">yedekleme </td><td style="text-align:center"> Konsol </td><td style="text-align:center"> david </td><td style="text-align:center"> Konuk</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> sahip </td><td style="text-align:center"> kök </td><td style="text-align:center"> sunucu</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> destek </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> user4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Bir VM oluşturulurken parola gereksinimleri nelerdir?
Parolalar 6 72 karakter uzunluğunda ve 3 dışında aşağıdaki 4 karmaşıklık gereksinimlerini karşılaması gerekir:

* Alt karakterler
* Üst karakter
* Bir rakam olması
* (Regex eşleşen [\W_]) bir özel karakter sahip

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
        <td style="text-align:center">Parola1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU veya!</td>
    </tr>
</table>
