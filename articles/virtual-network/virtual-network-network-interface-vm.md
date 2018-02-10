---
title: "Ağ arabirimlerine ekleme veya Azure sanal makinelerden kaldırma | Microsoft Docs"
description: "Ağ arabirimlerine ekleyip ağ arabirimleri sanal makinelerden öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: 225dd64c46bf9af3e058bbe3cfacf8f8a693b565
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Ağ arabirimlerine ekleme veya sanal makinelerden ağ arabirimleri Kaldır

Bir VM oluştururken, varolan bir ağ arabirimi eklemeyi öğrenin veya ekleyebilir veya var olan bir VM durduruldu (serbest bırakıldığında) durumunda ağ arabirimleri kaldırın. Bir ağ arabirimi bir Azure sanal makine (Internet, Azure ve şirket içi kaynakları ile iletişim kurmak için VM) sağlar. Bir VM, bir veya daha fazla ağ arabirimine sahip olabilir. 

Eklemeniz gerekiyorsa, değiştirmek veya okuma bir ağ arabirimi için IP adresleri kaldırmak [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md) makalesi. Gerektiğinde oluşturmak için değiştirmek veya ağ arabirimleri silin, okuyun [ağ arabirimlerini yönetme](virtual-network-network-interface.md) makalesi.

## <a name="before"></a>Başlamadan önce

Bu makalenin herhangi bölümündeki tüm adımları tamamlanmadan önce aşağıdaki görevleri tamamlayın:

- Azure oturum açma [portal](https://portal.azure.com), Azure komut satırı arabirimi (CLI) ya da Azure PowerShell ile bir Azure hesabı. Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede, görevleri tamamlamak için PowerShell komutlarını kullanarak, [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure PowerShell cmdlet'leri'nın en son sürümüne sahip olun. PowerShell komutlarıyla örnekler, yardım almanın yazın `get-help <command> -full`. Azure PowerShell'i yüklemek yerine Azure bulut Kabuğu'nu kullanabilirsiniz. Azure bulut Kabuğu doğrudan Azure Portalı'ndan çalıştırabileceğiniz boş bir powershell'dir. Azure önceden yüklenmiş ve yapılandırılmış hesabınızla birlikte kullanmak için PowerShell sahiptir. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com) ve Kabuk penceresi açıldığında, sol üst köşede PowerShell seçin.
- Bu makalede, görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com) ve Kabuk penceresi açıldığında, sol üst köşede Bash seçin.

## <a name="vm-create"></a>Yeni bir sanal makineye mevcut ağ arabirimlerinin ekleme

Portal üzerinden VM oluşturduğunuzda, portal varsayılan ayarlarla bir ağ arabirimi oluşturur ve VM sizin için ekler. Yeni bir sanal makineye mevcut ağ arabirimlerinin ekleyemez veya Azure portalını kullanarak birden çok ağ arabirimi ile bir VM oluşturun. Her iki CLI veya PowerShell kullanarak yapabilirsiniz. PowerShell veya CLI VM var olan bir ağ arabirimiyle ancak oluşturmak amacıyla kullanmadan önce ile öğrenmeniz [kısıtlamaları](#constraints). Birden çok ağ arabirimi ile bir sanal makine oluşturursanız, işletim sisteminin VM oluşturulduktan sonra bunları düzgün bir şekilde kullanma yapılandırmanız gerekir. Ayrıntılar için bkz. Configure [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimleri için.

**Komutları** VM oluşturmadan önce adımları kullanarak bir ağ arabirimi oluştur [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface).

|Aracı|Komut|
|---|---|
|CLI|[az vm oluşturma](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Bir ağ arabirimi için mevcut bir VM'yi Ekle

1. Azure portalında oturum açın.
2. Ağ arabirimine eklemek istediğiniz VM adını portalı üstündeki arama kutusuna, aramak veya Gözat VM için tıklayarak **tüm hizmetleri**, ardından **sanal makineleri**. VM bulduktan sonra'ı tıklatın. Bir ağ arabirimi için eklemek istediğiniz VM eklemek istediğiniz ağ arabirimleri sayısı desteklemesi gerekir. Okuma her VM boyutu destekler kaç ağ arabirimleri öğrenmek için [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri.  
3. Tıklatın **genel bakış**altında **ayarları**. Tıklatın **durdurmak**ve kadar beklemeniz **durum** VM değişikliklerini *durduruldu (serbest bırakıldı)*. 
4. Tıklatın **ağ**altında **ayarları**.
5. Tıklatın **Attach ağ arabirimi**. Şu anda başka bir VM'ye bağlı olmayan mevcut ağ arabirimlerinin listesinden eklemek istediğiniz ağ arabirimi'ı tıklatın. Seçtiğiniz ağ arabirimi etkin ağ hızlandırılmış olamaz, kendisine atanmış bir IPv6 adresi olamaz ve ağ arabirimi şu anda VM'ye bağlı sanal ağ içinde olduğu gibi aynı sanal ağda bulunması gerekir. Varolan bir ağ arabirimi yoksa, öncelikle bir oluşturmanız gerekir. Bir ağ arabirimi oluşturmak için tıklatın **oluşturma ağ arabirimi**. Bir ağ arabirimi oluşturma hakkında daha fazla bilgi için bkz: [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface). Ağ arabirimleri sanal makinelere eklerken ek sınırlamalar hakkında daha fazla bilgi için bkz: [kısıtlamaları](#constraints).
6. **Tamam**’a tıklayın.
7. Tıklatın **genel bakış**altında **ayarları**. Tıklatın **Başlat** sanal makineyi başlatmak için.
8. Birden çok ağ arabirimi düzgün bir şekilde kullanmak için VM işletim sistemi yapılandırın. Ayrıntılar için bkz. Configure [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimleri için.

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC eklemeniz](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (reference) or [detailed steps](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Bir VM için görünümü ağ arabirimleri

Her ağ arabiriminin yapılandırma hakkında bilgi edinmek için bir VM için şu anda bağlı ağ arabirimleri ve her bir ağ arabirimine atanmış IP adreslerini görüntüleyebilirsiniz. 

1. Oturum [Azure portal](https://portal.azure.com) aboneliğiniz için sahip, katkıda bulunan veya ağ Katılımcısı rolüne atanmış bir hesap ile. Hesaplarına rol atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *sanal makineleri*. Zaman **sanal makineleri** görünür arama sonuçlarında tıklatın.
3. Ağ arabirimleri için görüntülemek istediğiniz VM adını tıklatın.
4. İçinde **ayarları** bölümünde seçtiğiniz VM için **ağ**. Ağ arabirimi ayarları ve bunları değiştirme hakkında bilgi edinmek için [ağ arabirimlerini yönetme](virtual-network-network-interface.md). Değiştirerek veya kaldırarak bir ağ arabirimine atanmış IP adresleri ekleme hakkında bilgi için bkz [yönetmek IP adresleri](virtual-network-network-interface-addresses.md).

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az vm Göster](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Bir ağ arabirimi bir sanal makineden kaldırın

1. Azure portalında oturum açın.
2. Kaldırmak istediğiniz VM adını portalı üstündeki arama kutusuna, aramak (detach) ağ arabiriminden ya da tıklayarak VM Gözat **tüm hizmetleri**, ardından **sanal makineleri**. VM bulduktan sonra'ı tıklatın.
3. Tıklatın **genel bakış**altında **ayarları**. Tıklatın **durdurmak**ve kadar beklemeniz **durum** VM değişikliklerini *durduruldu (serbest bırakıldı)*. 
4. Tıklatın **ağ**altında **ayarları**.
5. Tıklatın **ayırma ağ arabirimi**. Şu anda sanal makineye bağlı ağ arabirimleri listesinden ayırmak istediğiniz ağ arabirimi'ı tıklatın. Yalnızca bir ağ arabirimi listelenen bir sanal makineyi her zaman en az bir ağ arabirimi bağlı olması gerektiğinden, ayıramazsınız.
6. **Tamam**’a tıklayın.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC kaldırmak](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (reference) or [detailed steps](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ arabirimlerine veya IP adreslerini bir VM oluşturmak için aşağıdaki makaleyi okuyun:

**Komutları**

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="constraints"></a>Kısıtlamalar

- Bir VM ekli en az bir ağ arabirimine sahip olmalıdır.
- Birçok ağ arabirimleri VM boyutu destekler bağlı olarak bir VM yalnızca olabilir. Kaç tane ağ arabirimleri hakkındaki her VM boyutu destekler daha fazla bilgi için bkz [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları. Tüm boyutları en az iki ağ arabirimi destekler.
- Bir VM'ye ekleyin ağ arabirimleri, şu anda başka bir VM eklenemiyor. Ağ arabirimleri oluşturma hakkında daha fazla bilgi için bkz: [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface).
- Geçmişte, ağ arabirimleri yalnızca desteklenen birden çok ağ arabirimleri ve en az iki ağ arabirimi ile oluşturulan VM'ler eklenemedi. VM boyutu desteklenen birden çok ağ arabirimleri olsa bile bir ağ arabirimi ile oluşturulmuş bir VM ağ arabirimi eklenemedi. Buna karşılık, VM'ler ile en az iki ağ arabirimleri her zaman en az iki ağ arabirimine sahip gerekiyordu oluşturduğundan ağ arabirimleri bir VM'den en az üç ağ arabirimlerine sahip yalnızca kaldırabilirsiniz. Bu kısıtlamaların hiçbiri artık geçerli. Bir VM artık herhangi bir sayıda ağ arabirimleri (kadar VM boyutu tarafından desteklenen numarası) ile de oluşturabilirsiniz.
- Varsayılan olarak, bir VM'ye bağlı ilk ağ arabirimi olarak tanımlanan *birincil* ağ arabirimi. Diğer tüm ağ arabirimleri VM'deki olan *ikincil* ağ arabirimleri.
- Hangi ağ arabirimi kontrol edebilirsiniz ancak giden trafik için varsayılan olarak gönderilen, sanal makineden giden tüm trafiği birincil ağ arabirimi birincil IP yapılandırması için atanan IP adresi çıkışı gönderilir.
- Geçmişte, tüm sanal makineleri aynı kullanılabilirlik kümesinde, tek veya birden çok ağ arabirimine sahip olmaları gerekirdi. Tüm ağ arabirimleri sayısı olan VM'ler artık aynı kullanılabilirlik kümesinde en fazla VM boyutu tarafından desteklenen sayı bulunabilir. Kullanılabilirlik ancak oluşturulduğunda kümesi için yalnızca bir VM ekleyebilirsiniz. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için okuma [azure'da VM kullanılabilirliğini yönetme](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) makalesi.
- Ağ arabirimleri aynı VM ağ arabirimi bir sanal ağ içindeki farklı alt ağlara bağlı olsa da, tüm aynı Vnet'e bağlanmalıdır.
- Herhangi bir IP adresi herhangi bir birincil veya ikincil ağ arabirimi tüm IP yapılandırması için bir Azure yük dengeleyici arka uç havuzuna ekleyebilirsiniz. Geçmişte, yalnızca birincil IP adresi birincil ağ arabirimi için bir arka uç havuzuna eklenemiyor. IP adresleri ve yapılandırmaları hakkında daha fazla bilgi için okuma [Ekle, Değiştir veya Kaldır IP adresleri](virtual-network-network-interface-addresses.md) makalesi.
- Bir VM silindiğinde, ona bağlı olan ağ arabirimleri silinmez. Bir VM silindiğinde, ağ arabirimleri VM'den ayrılır. Ağ arabirimleri için farklı sanal makineleri ekleyin veya silin.
- Bir ağ arabirimine atanmış özel bir IPv6 adresi olup olmadığını eklemeniz gerekir (Ekle) VM oluştururken, bir VM için. VM oluşturulduktan sonra bir VM'ye atanan bir IPv6 adresine sahip bir ağ arabirimi ekleyemezsiniz. Bir sanal makine oluşturulurken özel bir IPv6 adresi atanmış bir ağ arabirimiyle eklerseniz, VM boyutunu destekler kaç ağ arabirimleri bağımsız olarak sanal makine ağ arabirimini, yalnızca ekleyebilirsiniz. Bkz: [ağ arabirimi IP adresleri](virtual-network-network-interface-addresses.md) ağ arabirimleri için IP adresleri atama hakkında daha fazla bilgi edinmek için.
- Benzer IPv6 ağ arabirimi ile hızlandırılmış ağ VM oluşturulduktan sonra bir VM'ye etkin eklenemiyor. Ayrıca, hızlandırılmış ağ yararlanmak için ayrıca VM işletim sistemi içindeki adımları da tamamlamanız gerekir. Bunu kullanırken hızlandırılmış ağ ve diğer kısıtlamaları hakkında daha fazla bilgi için için ağ hızlandırılmış bkz [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineler.
