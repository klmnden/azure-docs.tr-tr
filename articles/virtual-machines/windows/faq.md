---
title: Azure'da Windows sanal makineleri hakkında SSS | Microsoft Docs
description: Bazı Windows sanal makineleri Resource Manager modeli kullanılarak oluşturulmuş ilgili sık sorulan soruların yanıtlarını içerir.
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
ms.date: 10/20/2017
ms.author: cynthn
ms.openlocfilehash: f427035f413dde304c2270006c6665120cb3e1e1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows sanal makineler hakkında sık sorulan sorular
Bu makalede Azure Resource Manager dağıtım modeli kullanılarak oluşturulan Windows sanal makineler hakkında bazı sık sorulan soruları giderir. Bu konuda Linux sürümü için bkz: [Linux sanal makineleri hakkında sık sorulan bir soru](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Azure'da çalışan Microsoft sunucu yazılımı için destek ilkesi hakkında daha fazla bilgi için bkz: [Microsoft sunucu yazılımı desteği için Azure sanal makineler](https://support.microsoft.com/kb/2721672)

Belirli Windows 7, Windows 8.1 ve Windows 10 sürümleri MSDN Azure avantajı aboneleri ve geliştirme ve test görevler için MSDN Geliştirme ve Test Kullandıkça Öde aboneleri için kullanılabilir. Yönerge ve kısıtlamalar dahil olmak üzere ayrıntılı bilgi edinmek için bkz. [MSDN aboneleri için Windows İstemci görüntüleri](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diski en fazla 4 TB (4.095 GB) olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure yönetilen önerilen disk depolama teklifleri kullanmak için Azure Virtual Machines ile kalıcı depolama alanı için disklerdir. Her bir Sanal Makine ile birden fazla Yönetilen Disk kullanabilirsiniz. Yönetilen Diskler iki tür dayanıklı depolama seçeneği sunar: Premium ve Standart Yönetilen Diskler. Fiyatlandırma bilgileri için bkz: [yönetilen diskleri fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks).

Azure depolama hesapları, aynı zamanda işletim sistemi diski ve veri diskleri için depolama sağlayabilir. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Sanal Makinem nasıl erişebilir mi?
Bir Windows VM için Uzak Masaüstü Bağlantısı (RDP) kullanarak uzak bağlantı kurun. Yönergeler için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). En fazla iki eşzamanlı bağlantı desteklenir, sunucunun bir Uzak Masaüstü Hizmetleri oturumu ana bilgisayarı yapılandırılmadığı sürece.  

Uzak Masaüstü ile sorun yaşıyorsanız, bkz: [sorun giderme Uzak Masaüstü bağlantıları için Windows tabanlı Azure sanal makine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Hyper-V deneyiminiz varsa VMConnect’e benzer bir araç arıyor olabilirsiniz. Sanal makineye konsol erişimi desteklenmediğinden, Azure benzer bir araç sunmaz.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Verileri depolamak için geçici disk (varsayılan olarak D: sürücüsü) kullanabilir miyim?
Geçici disk verilerini depolamak için kullanmayın. Kurtarılamaz veri kaybetme riskini böylece yalnızca geçici depolama mümkündür. Sanal makineyi farklı bir ana bilgisayara taşındığında veri kaybı oluşabilir. Sanal makinenin yeniden boyutlandırılması, konağın güncelleştirilmesi veya konaktaki bir donanım hatası, sanal makinenin taşınmasını gerektirecek olası nedenler arasındadır.

D: sürücü harfi kullanması gereken bir uygulamanız varsa, geçici disk D: dışında bir şey kullanmayacağından sürücü harfi atayabilirsiniz. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Geçici diskin sürücü harfini nasıl değiştirebilirim?
Sayfa dosyası taşıma ve sürücü harflerini yeniden atama sürücü harfini değiştirebilirsiniz, ancak belirli bir sırada adımlarda yaptığınızdan emin olmanız gerekir. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Mevcut bir VM'yi bir kullanılabilirlik kümesi ekleyebilir miyim?
Hayır. Bir kullanılabilirlik kümesinin parçası olarak, VM istiyorsanız, VM kümesi içinde oluşturmanız gerekir. Şu anda hiç kullanılabilirlik oluşturulduktan sonra kümesi için bir VM ekleme olanağı.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Bir sanal makine için Azure yükleyebilir miyim?
Evet. Yönergeler için bkz: [geçiş şirket içi Azure VM'ler](on-prem-to-azure.md).

## <a name="can-i-resize-the-os-disk"></a>İşletim sistemi disk boyutunu değiştirebilir miyim?
Evet. Yönergeler için bkz: [Azure kaynak grubu içindeki bir sanal makineye işletim sistemi sürücüsünde genişletmek nasıl](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>I kopyalayabilir veya mevcut bir Azure VM'yi kopyalama?
Evet. Yönetilen görüntüleri kullanarak, bir sanal makinenin görüntü oluşturma ve birden çok yeni VM oluşturmak için görüntüyü kullanın. Yönergeler için bkz: [özel bir görüntü bir VM oluşturun](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Neden Kanada merkezi ve Doğu Kanada bölgeler arasında Azure Resource Manager görüyorum değil mi?

Kanada Orta ve Doğu Kanada iki yeni bölgeleri otomatik olarak mevcut Azure abonelikleri için sanal makine oluşturmak için kayıtlı değil. Bu kayıt, Azure Kaynak Yöneticisi'ni kullanarak diğer bir bölge için Azure portal aracılığıyla bir sanal makine dağıtıldığında otomatik olarak gerçekleştirilir. Diğer Azure bölgesi için bir sanal makine dağıtıldıktan sonra yeni bölgeler sonraki sanal makineler için kullanılabilir olması gerekir.

## <a name="does-azure-support-linux-vms"></a>Azure Linux VM'ler destekliyor mu?
Evet. Hızlı şekilde denemek için bir Linux VM oluşturmak için bkz: [Portal kullanarak Azure'da bir Linux VM oluşturma](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Oluşturulduktan sonra VM'im için bir NIC ekleyebilirim?
Evet, bu artık mümkündür. VM ilk durdurulması deallocated gerekir. Ardından ekleyebilir veya bir NIC (son NIC VM üzerinde olmadığı sürece) kaldırabilirsiniz. 

## <a name="are-there-any-computer-name-requirements"></a>Herhangi bir bilgisayar adı gereksinimleri var mı?
Evet. Bilgisayar adı en fazla 15 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve sınırlamaları](/azure/architecture/best-practices/naming-conventions#compute) kaynaklarınızı adlandırma geçici daha fazla bilgi için.

## <a name="are-there-any-resource-group-name-requirements"></a>Herhangi bir kaynak grubu adı gereksinimleri var mı?
Evet. Kaynak grubu adı en fazla 90 karakter uzunluğunda olabilir. Bkz: [adlandırma kuralları kuralları ve sınırlamaları](/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions) kaynak grupları hakkında daha fazla bilgi için.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Bir VM oluşturulurken kullanıcıadı gereksinimleri nelerdir?

Kullanıcı adları en fazla 20 karakter uzunluğunda olabilir ve bir noktayla bitemez ("."). 


Aşağıdaki kullanıcı adları izin verilmiyor:
<table>
    <tr>
        <td style="text-align:center">Yönetici </td><td style="text-align:center"> yönetici </td><td style="text-align:center"> kullanıcı </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">test </td><td style="text-align:center"> user2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> KULLANICI3</td>
    </tr>    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> a</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">yedekleme </td><td style="text-align:center"> console </td><td style="text-align:center"> david </td><td style="text-align:center"> Konuk</td>
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
Parolalar 12-123 karakter uzunluğunda olmalıdır ve 3 dışında aşağıdaki 4 karmaşıklık gereksinimlerini karşılaması gerekir:

* Alt karakterler
* Üst karakter
* Bir rakam olması
* (Regex eşleşen [\W_]) bir özel karakter sahip

Aşağıdaki parolalara izin verilmez:

<table>
    <tr>
        <td>abc@123 </td>
        <td>P@$$w0rd </td>
        <td>P@ssw0rd </td>
        <td>P@ssword123 </td>
        <td>Pa$$word </td>
    </tr>
    <tr>
        <td>pass@word1 </td>
        <td>Parola! </td>
        <td>Parola1 </td>
        <td>Password22 </td>
        <td>ILOVEYOU veya! </td>
    </tr>
</table>
