---
title: Azure'da Windows sanal makineleri ile ilgili SSS | Microsoft Docs
description: Resource Manager modeli kullanılarak oluşturulmuş bir Windows sanal makineleri hakkında genel soruların yanıtlarını sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 61f24b3c13a53b23538327cd1458a54756b7caa5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65466365"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows sanal makineleri hakkında sık sorulan sorular
Bu makalede Azure Resource Manager dağıtım modeli kullanılarak oluşturulan Windows sanal makineleri hakkında bazı yaygın sorular ele alınmıştır. Bu konuda Linux sürümü için bkz: [Linux sanal makineleri hakkında sık sorulan soruya](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Azure'da çalışan Microsoft sunucu yazılımı için destek ilkesi hakkında daha fazla bilgi için bkz: [Microsoft sunucu yazılımı desteği Azure sanal makineleri için](https://support.microsoft.com/kb/2721672).

MSDN Azure avantajı aboneleri ve MSDN Geliştirme ve Test Kullandıkça Öde aboneleri geliştirme ve test görevleri belirli bir Windows 7, Windows 8.1 ve Windows 10 sürümleri kullanılabilir. Yönerge ve kısıtlamalar dahil olmak üzere ayrıntılı bilgi edinmek için bkz. [MSDN aboneleri için Windows İstemci görüntüleri](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski 4 TB'a kadar (4.095 GB) olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure yönetilen diskler önerilen disk depolama kullanmak için Azure sanal makineler ile veri kalıcı depolama için tekliflerdir. Her bir Sanal Makine ile birden fazla Yönetilen Disk kullanabilirsiniz. Yönetilen diskler teklif iki tür dayanıklı depolama seçeneği Premium ve standart yönetilen diskler. Fiyatlandırma bilgileri için bkz: [yönetilen diskler fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

Azure depolama hesapları, aynı zamanda işletim sistemi diski ve varsa veri diskleri için depolama sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal Makinem nasıl erişebilirim?
Bir Windows VM için Uzak Masaüstü Bağlantısı (RDP) kullanarak uzak bağlantı kurun. Yönergeler için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). En fazla iki eş zamanlı bağlantı desteklenir, sunucunun bir Uzak Masaüstü Hizmetleri oturum konağı olarak yapılandırılmadığı sürece.  

Uzak Masaüstü ile ilgili sorunlar yaşıyorsanız bkz [Uzak Masaüstü bağlantılarında sorun giderme için Windows tabanlı Azure sanal makine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Hyper-V deneyiminiz varsa VMConnect’e benzer bir araç arıyor olabilirsiniz. Sanal makineye konsol erişimi desteklenmediğinden, Azure benzer bir araç sunmaz.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Verileri depolamak için geçici diski (varsayılan olarak D: sürücüsü) kullanabilir miyim?
Geçici disk verilerini saklamak için kullanmayın. Kurtarılamaz veri kaybetme riskiyle, yalnızca geçici bir depolama alanıdır. Sanal makine farklı bir ana bilgisayara taşındığında, veri kaybı oluşabilir. Sanal makinenin yeniden boyutlandırılması, konağın güncelleştirilmesi veya konaktaki bir donanım hatası, sanal makinenin taşınmasını gerektirecek olası nedenler arasındadır.

D: sürücü harfini kullanmak için gereken bir uygulamanız varsa, böylece geçici disk D: dışında bir şey kullanır, sürücü harfi atayabilirsiniz. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Geçici diskin sürücü harfini nasıl değiştirebilirim?
Sayfa dosyası taşıma ve sürücü harflerini yeniden atayarak sürücü harfini değiştirebilirsiniz, ancak belirli bir sırayla adımların emin olmanız gerekir. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Bir kullanılabilirlik kümesine mevcut bir VM'yi ekleyebilir miyim?
Hayır. Bir kullanılabilirlik kümesinin parçası olacak şekilde sanal makinenizin istiyorsanız, VM kümesi içinde oluşturmanız gerekir. Şu anda hiç bir şekilde oluşturulduktan sonra kullanılabilirlik kümesine için bir VM ekleyin.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Azure'da bir sanal makine yükleyebilir miyim?
Evet. Yönergeler için [geçirme şirket içi Vm'leri azure'a](on-prem-to-azure.md).

## <a name="can-i-resize-the-os-disk"></a>İşletim sistemi diski yeniden boyutlandırma?
Evet. Yönergeler için [nasıl bir Azure kaynak grubunda bir sanal makinenin işletim sistemi sürücüsünü genişletme](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Ben kopyalayabilir veya mevcut bir Azure VM'yi kopyalama?
Evet. Yönetilen görüntüleri kullanarak bir sanal makine görüntüsü oluşturma ve ardından birden fazla yeni VM oluşturmak için görüntüyü kullanın. Yönergeler için [özel bir VM görüntüsü oluşturmak](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Neden Kanada Orta ve Kanada Doğu bölgelerinde Azure Resource Manager aracılığıyla görüyorum değil mi?

Kanada Orta ve Kanada Doğu, iki yeni bölgede otomatik olarak mevcut bir Azure aboneliği için sanal makine oluşturmak için kayıtlı değil. Azure Resource Manager kullanarak başka bir bölgeye Azure portalı üzerinden bir sanal makine dağıtıldığında bu kayıt otomatik olarak gerçekleştirilir. Diğer Azure bölgesi için bir sanal makine dağıtıldıktan sonra yeni bölgelere sonraki sanal makineler için kullanılabilir olması gerekir.

## <a name="does-azure-support-linux-vms"></a>Azure Linux sanal makineleri destekliyor mu?
Evet. Hızlı şekilde denemek için bir Linux VM oluşturmak için bkz: [Portal kullanarak Azure'da bir Linux VM oluşturma](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra sanal Makinem için bir NIC ekleyebilirim?
Evet, bu artık mümkündür. VM'nin ilk durdurulup serbest gerekir. Ardından ekleyebilir veya bir NIC (son NIC VM üzerinde olmadığı sürece) kaldırın. 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 15 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions#compute) kaynaklarınızı adlandırma etrafında daha fazla bilgi için.

## <a name="are-there-any-resource-group-name-requirements"></a>Herhangi bir kaynak grubu adı gereksinimleri vardır?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve kısıtlamalar](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) kaynak grupları hakkında daha fazla bilgi için.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Bir VM oluştururken, kullanıcı adı gereksinimleri nelerdir?

Kullanıcı adları en fazla 20 karakter uzunluğunda olabilir ve nokta ile bitemez ("."). 

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

* Daha düşük karakter içerebilir
* Üst karakter içerebilir
* Bir rakam olması
* Sahip bir özel karakter (Regex eşleşmesi [\W_])

Aşağıdaki parolalara izin verilmez:

<table>
    <tr>
        <td>abc@123</td>
        <td>ILOVEYOU veya!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$$word</td>
        <td>pass@word1</td>
        <td>Parola!</td>
        <td>Password1</td>
        <td>Password22</td>
    </tr>
</table>
