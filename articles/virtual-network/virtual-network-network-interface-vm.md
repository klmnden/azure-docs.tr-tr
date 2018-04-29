---
title: Ağ arabirimlerine ekleme veya Azure sanal makinelerden kaldırma | Microsoft Docs
description: Ağ arabirimlerine ekleyip ağ arabirimleri sanal makinelerden öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: 6193dcc6ba2e78c55ed6c6f769aea50fcfb3ac40
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Ağ arabirimlerine ekleme veya sanal makinelerden ağ arabirimleri Kaldır

Bir Azure sanal makinesini (VM) oluşturduğunuzda, mevcut bir ağ arabirimi ekleyin ya da ekleyebilir veya var olan bir VM durduruldu (serbest bırakıldığında) durumunda ağ arabirimleri kaldırabilirsiniz öğrenin. Bir ağ arabirimi, internet, Azure ve şirket içi kaynakları ile iletişim kurmak bir Azure sanal makinesi sağlar. Bir VM, bir veya daha fazla ağ arabirimine sahip olabilir. 

Eklemek, değiştirmek veya bir ağ arabirimi için IP adreslerini kaldırın, bkz: [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md). Oluşturmanız gerekiyorsa, değiştirmek veya ağ arabirimleri, silme, bkz: [ağ arabirimlerini yönetme](virtual-network-network-interface.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.2.0 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.26 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Yeni bir sanal makineye mevcut ağ arabirimlerinin ekleme

Portal üzerinden sanal makine oluşturduğunuzda, portal varsayılan ayarlarla bir ağ arabirimi oluşturur ve VM sizin için ekler. Yeni bir sanal makineye mevcut ağ arabirimlerini ekleyin veya Azure portalını kullanarak birden çok ağ arabirimi ile bir VM oluşturun. Her ikisi de CLI veya PowerShell kullanarak bunu, ancak ile tanımak emin [kısıtlamaları](#constraints). Bir VM ile birden çok ağ arabirimi oluşturursanız, işletim sisteminin VM oluşturduktan sonra bunları düzgün bir şekilde kullanmak için yapılandırmanız gerekir. Nasıl yapılandırılacağını öğrenmek [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimleri için.

### <a name="commands"></a>Komutlar

VM oluşturmadan önce bir ağ arabirimi'ndaki adımları kullanarak oluşturma [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface).

|Aracı|Komut|
|---|---|
|CLI|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Bir ağ arabirimi için mevcut bir VM'yi Ekle

1. Azure Portal’da oturum açın.
2. Ağ arabirimi ekleyin veya seçerek VM için gözatmak istediğiniz VM adını portalı üstündeki arama kutusuna yazın **tüm hizmetleri**ve ardından **sanal makineleri**. VM bulduktan sonra onu seçin. VM eklemek istediğiniz ağ arabirimleri sayısı desteklemesi gerekir. Her VM boyutu kaç ağ arabirimleri öğrenmek için destekler, bkz: [azure'daki Linux sanal makineler için Boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [boyutları için Windows Azure sanal makineleri](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Seçin **genel bakış**altında **ayarları**. Seçin **durdurmak**ve ardından bekle **durum** VM değişikliklerini **durduruldu (serbest bırakıldı)**. 
4. Seçin **ağ**altında **ayarları**.
5. Seçin **Attach ağ arabirimi**. Şu anda başka bir VM'ye bağlı olmayan ağ arabirimleri listesinden eklemek istediğiniz bir tanesini seçin. 

    >[!NOTE]
    Seçtiğiniz ağ arabirimi etkin ağ hızlandırılmış olamaz, kendisine atanmış bir IPv6 adresi olamaz ve şu anda VM'ye bağlı ağ arabirimi içeren aynı sanal ağ mevcut olmalıdır. 

    Varolan bir ağ arabirimi yoksa, öncelikle bir oluşturmanız gerekir. Bunu yapmak için seçin **oluşturma ağ arabirimi**. Bir ağ arabirimi oluşturma hakkında daha fazla bilgi için bkz: [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface). Ağ arabirimleri sanal makinelere eklerken ek sınırlamalar hakkında daha fazla bilgi için bkz: [kısıtlamaları](#constraints).

6. **Tamam**’ı seçin.
7. Seçin **genel bakış**altında **ayarları**ve ardından **Başlat** sanal makineyi başlatmak için.
8. Birden çok ağ arabirimi düzgün bir şekilde kullanmak için VM işletim sistemi yapılandırın. Nasıl yapılandırılacağını öğrenmek [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimleri için.

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC eklemeniz](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Ekleme AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımları](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Bir VM için görünümü ağ arabirimleri

Her ağ arabiriminin yapılandırma hakkında bilgi edinmek için bir VM için şu anda bağlı ağ arabirimleri ve her bir ağ arabirimine atanmış IP adreslerini görüntüleyebilirsiniz. 

1. Oturum [Azure portal](https://portal.azure.com) aboneliğiniz için sahip, katkıda bulunan veya ağ Katılımcısı rolüne atanmış bir hesap ile. Hesaplarına roller atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Metni içeren kutusunda **arama kaynakları** Azure portalının en üstünde yazın **sanal makineleri**. Zaman **sanal makineleri** arama sonuçlarında görünür.
3. Ağ arabirimlerini görüntülemek istediğiniz VM adını seçin.
4. İçinde **ayarları** bölümü, seçtiğiniz VM seçin için **ağ**. Ağ arabirimi ayarları ve bunları değiştirme hakkında bilgi edinmek için [ağ arabirimlerini yönetme](virtual-network-network-interface.md). Ekleme, değiştirme veya bir ağ arabirimine atanmış IP adresleri kaldırma konusunda bilgi edinmek için [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md).

### <a name="commands"></a>Komutlar

|Aracı|Komut|
|---|---|
|CLI|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>Bir ağ arabirimi bir sanal makineden kaldırın

1. Azure Portal’da oturum açın.
2. Kaldırmak istediğiniz VM adını portalı üstündeki arama kutusuna arama (detach) ağ arabiriminden ya da seçerek VM Gözat **tüm hizmetleri**ve ardından **sanal makineleri**. VM bulduktan sonra onu seçin.
3. Seçin **genel bakış**altında **ayarları**ve ardından **durdurmak**. Bekle **durum** VM değişikliklerini **durduruldu (serbest bırakıldı)**. 
4. Seçin **ağ**altında **ayarları**.
5. Seçin **ayırma ağ arabirimi**. Şu anda sanal makineye bağlı ağ arabirimleri listesinden ayırmak istediğiniz ağ arabirimi seçin. 

    >[!NOTE]
    Yalnızca bir ağ arabirimi listelenen bir sanal makineyi her zaman en az bir ağ arabirimi bağlı olması gerektiğinden, ayıramazsınız.
6. **Tamam**’ı seçin.

### <a name="commands"></a>Komutlar

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC kaldırmak](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımları](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Kısıtlamalar

- Bir VM ekli en az bir ağ arabirimine sahip olmalıdır.
- Birçok ağ arabirimleri VM boyutu destekler bağlı olarak bir VM yalnızca olabilir. Kaç tane ağ arabirimleri hakkındaki her VM boyutu destekler daha fazla bilgi için bkz [azure'daki Linux sanal makineler için Boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [boyutları için Windows Azure sanal makineleri](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tüm boyutları en az iki ağ arabirimi destekler.
- Bir VM'ye ekleyin ağ arabirimleri, şu anda başka bir VM eklenemiyor. Ağ arabirimleri oluşturma hakkında daha fazla bilgi için bkz: [bir ağ arabirimi oluştur](virtual-network-network-interface.md#create-a-network-interface).
- Geçmişte, ağ arabirimleri yalnızca desteklenen birden çok ağ arabirimleri ve en az iki ağ arabirimi ile oluşturulan VM'ler eklenemedi. VM boyutu desteklenen birden çok ağ arabirimleri olsa bile bir ağ arabirimi ile oluşturulmuş bir VM ağ arabirimi eklenemedi. Buna karşılık, VM'ler ile en az iki ağ arabirimleri her zaman en az iki ağ arabirimine sahip gerekiyordu oluşturduğundan ağ arabirimleri bir VM'den en az üç ağ arabirimlerine sahip yalnızca kaldırabilirsiniz. Bu kısıtlamaların hiçbiri artık geçerli. Bir VM artık herhangi bir sayıda ağ arabirimleri (kadar VM boyutu tarafından desteklenen numarası) ile de oluşturabilirsiniz.
- Varsayılan olarak, bir VM'ye bağlı ilk ağ arabirimi olarak tanımlanan *birincil* ağ arabirimi. Diğer tüm ağ arabirimleri VM'deki olan *ikincil* ağ arabirimleri.
- Hangi ağ arabirimi kontrol edebilirsiniz ancak giden trafik için varsayılan olarak gönderilen, sanal makineden giden tüm trafiği birincil ağ arabirimi birincil IP yapılandırması için atanan IP adresi çıkışı gönderilir.
- Geçmişte, tüm sanal makineleri aynı kullanılabilirlik kümesinde, tek veya birden çok ağ arabirimine sahip olmaları gerekirdi. Tüm ağ arabirimleri sayısı olan VM'ler artık aynı kullanılabilirlik kümesinde en fazla VM boyutu tarafından desteklenen sayı bulunabilir. Kullanılabilirlik oluşturulduğunda kümesi için yalnızca bir VM ekleyebilirsiniz. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz: [azure'da VM kullanılabilirliğini yönetme](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Ağ arabirimleri aynı VM'deki sanal ağ içindeki farklı alt ağlara bağlı olsa da, ağ arabirimleri tüm aynı sanal ağa bağlı gerekir.
- Herhangi bir IP adresi herhangi bir birincil veya ikincil ağ arabirimi tüm IP yapılandırması için bir Azure yük dengeleyici arka uç havuzuna ekleyebilirsiniz. Geçmişte, yalnızca birincil IP adresi birincil ağ arabirimi için bir arka uç havuzuna eklenemiyor. IP adresleri ve yapılandırmaları hakkında daha fazla bilgi için bkz: [Ekle, Değiştir veya Kaldır IP adresleri](virtual-network-network-interface-addresses.md).
- Bir VM silindiğinde, ona bağlı olan ağ arabirimleri silinmez. Bir VM sildiğinizde, sanal makineden ağ arabirimleri ayrılır. Ağ arabirimleri için farklı sanal makineleri ekleyin veya silin.
- Bir ağ arabirimine atanmış özel bir IPv6 adresi olup olmadığını eklemeniz gerekir (Ekle) VM oluşturduğunuzda, bir VM için. VM oluşturduktan sonra bir VM'ye atanan bir IPv6 adresine sahip bir ağ arabirimi ekleyemezsiniz. Bir sanal makine oluşturduğunuzda, özel bir IPv6 adresi atanmış bir ağ arabirimiyle eklerseniz, VM boyutunu destekler kaç ağ arabirimleri bağımsız olarak sanal makine için yalnızca bu ağ arabirimini ekleyebilirsiniz. Bkz: [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md) ağ arabirimleri için IP adresleri atama hakkında daha fazla bilgi için.
- IPv6 gibi bir ağ arabirimi ile hızlandırılmış ağ oluşturduktan sonra bir VM'ye etkin eklenemiyor. Ayrıca, hızlandırılmış ağ yararlanmak için ayrıca VM işletim sistemi içindeki adımları da tamamlamanız gerekir. Hızlandırılmış ağ hakkında daha fazla bilgi ve diğer kısıtlamalar, için kullanırken öğrenin [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineler.

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ arabirimlerine veya IP adreslerini bir VM oluşturmak için aşağıdaki makaleyi okuyun:

### <a name="commands"></a>Komutlar

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|


