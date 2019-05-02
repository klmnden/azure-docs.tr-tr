---
title: Azure sanal makinelerden ağ arabirimlerine Ekle Kaldır | Microsoft Docs
description: Ağ arabirimlerine ekleme veya sanal makinelerden ağ arabirimleri Kaldır öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: kumud
ms.openlocfilehash: 23e46290af6bdb4c217d8fa0cd836673652fc81d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64701385"
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Ağ arabirimlerine ekleme veya sanal makinelerden ağ arabirimleri Kaldır

Azure sanal makine'de (VM) oluşturduğunuzda, mevcut bir ağ arabirimi eklemek veya ekleyip ağ arabirimleri durduruldu (serbest bırakıldı) durumdaki mevcut bir VM'den öğrenin. İnternet, Azure ve şirket içi kaynakları ile iletişim kurmak bir Azure sanal makinesine bir ağ arabirimi sağlar. Bir veya daha fazla ağ arabirimi bir sanal makine olabilir. 

Eklemek, değiştirmek veya bir ağ arabirimi için IP adreslerini kaldırın, bkz. [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md). Oluşturmanız gerekiyorsa, değiştirme veya ağ arabirimlerini silin, bkz: [ağ ara birimlerini](virtual-network-network-interface.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.26 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Mevcut ağ arabirimi için yeni bir sanal makine ekleyin

Portal üzerinden bir sanal makine oluşturduğunuzda, portal varsayılan ayarlarla bir ağ arabirimi oluşturur ve bunu sizin için sanal Makineye ekler. Mevcut ağ arabirimlerinin yeni bir VM'ye ekleme ve Azure portalını kullanarak birden çok ağ arabirimi ile VM oluşturma. Her ikisi de CLI veya PowerShell kullanarak yapmak için ancak ile kendinizi alıştırın mutlaka [kısıtlamaları](#constraints). Birden çok ağ arabirimi ile bir VM oluşturursanız, işletim sisteminin VM oluşturduktan sonra bunları düzgün bir şekilde kullanma yapılandırmanız da gerekir. Nasıl yapılandıracağınızı öğrenin [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimi için.

### <a name="commands"></a>Komutlar

VM oluşturmadan önce adımları kullanarak bir ağ arabirimi oluşturma [ağ arabirimini oluşturun](virtual-network-network-interface.md#create-a-network-interface).

|Tool|Komut|
|---|---|
|CLI|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|PowerShell|[New-AzVM](/powershell/module/az.compute/new-azvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Mevcut bir VM'ye ağ arabirimi ekleyin

1. Azure Portal’da oturum açın.
2. Ağ arabirimi ekleyin ya da seçerek VM için Gözat istediğiniz VM adını portalın üst kısmındaki arama kutusuna yazın **tüm hizmetleri**, ardından **sanal makineler**. VM bulduktan sonra onu seçin. VM, eklemek istediğiniz ağ arabirimlerinin sayısını desteklemesi gerekir. Her VM boyutunun kaç ağ arabirimleri destekleyen öğrenmek için bkz: [azure'da Linux sanal makine boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [boyutları için Windows azure'da sanal makineler](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Seçin **genel bakış**altında **ayarları**. Seçin **Durdur**ve ardından bekle **durumu** sanal makinenin değişiklikleri **durduruldu (serbest bırakıldı)**.
4. Seçin **ağ**altında **ayarları**.
5. Seçin **ek ağ arabirimi**. Şu anda başka bir VM'ye bağlı olmayan ağ arabirimleri listesinden iliştirmek istediğiniz birini seçin.

   >[!NOTE]
   >Seçtiğiniz ağ arabiriminin etkin hızlandırılmış olamaz, kendisine atanmış bir IPv6 adresi olamaz ve şu anda sanal Makineye bağlı ağ arabirimi içeren aynı sanal ağda bulunması gerekir.

   Var olan bir ağ arabirimi yoksa, öncelikle bir oluşturmanız gerekir. Bunu yapmak için **oluşturma ağ arabirimi**. Bir ağ arabirimi oluşturma hakkında daha fazla bilgi için bkz. [ağ arabirimini oluşturun](virtual-network-network-interface.md#create-a-network-interface). Ağ arabirimleri için sanal makineler eklerken ek kısıtlamaları hakkında daha fazla bilgi için bkz [kısıtlamaları](#constraints).

6. **Tamam**’ı seçin.
7. Seçin **genel bakış**altında **ayarları**, ardından **Başlat** sanal makineyi başlatmak için.
8. Birden çok ağ arabirimi düzgün bir şekilde kullanmak için sanal makine işletim sistemini yapılandırın. Nasıl yapılandıracağınızı öğrenin [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) birden çok ağ arabirimi için.

### <a name="commands"></a>Komutlar
|Tool|Komut|
|---|---|
|CLI|[az vm nic ekleme](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımlar](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Ekleme AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımlar](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Bir VM için ağ arabirimlerini görüntüleme

Şu anda her ağ arabiriminin yapılandırma hakkında bilgi edinmek için bir sanal makineye bağlı ağ arabirimleri ve her bir ağ arabirimine atanan IP adreslerini görüntüleyebilirsiniz. 

1. Oturum [Azure portalında](https://portal.azure.com) aboneliğiniz için sahip, katkıda bulunan veya ağ Katılımcısı rolü atanmış bir hesap. Hesapları için rol atama hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Metni içeren kutuya **kaynak Ara** Azure portalının üst kısmında, yazın **sanal makineler**. Zaman **sanal makineler** arama sonuçlarında görünür.
3. Ağ arabirimlerini görüntülemek istediğiniz VM adını seçin.
4. İçinde **ayarları** bölümü, seçtiğiniz sanal makine seçin için **ağ**. Ağ arabirimi ayarları ve ayarların nasıl değiştirileceği hakkında bilgi edinmek için [ağ ara birimlerini](virtual-network-network-interface.md). Ekleme, değiştirme veya bir ağ arabirimine atanan IP adresleri kaldırma hakkında bilgi edinmek için bkz: [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md).

### <a name="commands"></a>Komutlar

|Tool|Komut|
|---|---|
|CLI|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|PowerShell|[Get-AzVM](/powershell/module/az.compute/get-azvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>Bir ağ arabirimi bir sanal makineden kaldırın

1. Azure Portal’da oturum açın.
2. Kaldırmak istediğiniz VM adını portalın üst kısmındaki arama kutusuna arama (ayırma) seçerek VM için Gözat veya ağ arabiriminden **tüm hizmetleri**ve ardından **sanal makineler**. VM bulduktan sonra onu seçin.
3. Seçin **genel bakış**altında **ayarları**, ardından **Durdur**. Bekle **durumu** sanal makinenin değişiklikleri **durduruldu (serbest bırakıldı)**.
4. Seçin **ağ**altında **ayarları**.
5. Seçin **ağ arabirimini Ayır**. Şu anda sanal makineye bağlı ağ arabirimleri listesinden, ayırmak istediğiniz ağ arabirimi seçin.

   >[!NOTE]
   >Bir ağ arabirimi listelenen yalnızca, bir sanal makinenin her zaman bağlı en az bir ağ arabirimi olması gerektiğinden, ayıramazsınız.
6. **Tamam**’ı seçin.

### <a name="commands"></a>Komutlar

|Tool|Komut|
|---|---|
|CLI|[az vm nic kaldırmak](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımlar](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzVMNetworkInterface](/powershell/module/az.compute/remove-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımlar](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Kısıtlamalar

- Bir VM'ye bağlı en az bir ağ arabirimi olması gerekir.
- Birçok ağ arabirimleri VM boyutu destekler bağlı olarak bir VM yalnızca olabilir. Her VM boyutunun kaç ağ arabirimleri hakkında destekleyen daha fazla bilgi için bkz: [azure'da Linux sanal makine boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [boyutları için Windows azure'da sanal makineler](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tüm boyutları en az iki ağ arabirimi destekler.
- Bir sanal makineye eklediğiniz ağ arabirimleri, şu anda başka bir sanal makineye eklenemez. Ağ arabirimlerini oluşturma hakkında daha fazla bilgi için bkz. [ağ arabirimini oluşturun](virtual-network-network-interface.md#create-a-network-interface).
- Geçmişte, ağ arabirimleri yalnızca desteklenen birden çok ağ arabirimi ve en az iki ağ arabirimi ile oluşturulan sanal makineler için eklenemedi. Desteklenen VM boyutu birden çok ağ arabirimi olsa bile bir ağ arabirimi ile oluşturulan bir sanal makineye bir ağ arabirimi eklenemedi. Buna karşılık, VM'ler ile en az iki ağ arabirimlerinin her zaman en az iki ağ arabirimi olması gerekiyordu oluşturduğundan ağ arabirimleri ile en az üç ağ arabirimi, bir VM'den yalnızca kaldırabilirsiniz. Bu kısıtlamalar hiçbiri artık geçerli. Artık, herhangi bir sayıda ağ arabirimine (en fazla sanal makine boyutu tarafından desteklenen sayı) sahip bir VM oluşturabilirsiniz.
- Varsayılan olarak, bir VM'ye bağlı ilk ağ arabirimini olarak tanımlanan *birincil* ağ arabirimi. Sanal makine içindeki diğer tüm ağ arabirimleri olan *ikincil* ağ arabirimleri.
- Hangi ağ arabirimi kontrol edebilirsiniz ancak varsayılan olarak, giden trafiği gönderilen, VM'den tüm giden trafiği birincil ağ arabiriminin birincil IP yapılandırması için atanan IP adresi kullanıma gönderilir.
- Geçmişte, tüm VM'lerin aynı kullanılabilirlik kümesindeki tek veya birden çok ağ arabirimi olması gerekir. Herhangi bir sayıda ağ arabirimine sahip VM'ler artık aynı kullanılabilirlik kümesinde en fazla sanal makine boyutu tarafından desteklenen sayı bulunabilir. Yalnızca bir VM kullanılabilirlik oluşturulduğunda kümesine ekleyebilirsiniz. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz. [azure'da VM kullanılabilirliğini yönetme](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Ağ arabirimleri aynı VM ağ arabirimi bir sanal ağdaki farklı alt ağlara bağlı durumdayken, tüm aynı sanal ağa bağlanmalıdır.
- Herhangi bir IP adresi herhangi bir birincil veya ikincil ağ arabirimi IP yapılandırması için bir Azure Load Balancer arka uç havuzuna ekleyebilirsiniz. Geçmişte, arka uç havuzuna yalnızca birincil ağ arabiriminin birincil IP adresi eklenemedi. IP adresleri ve yapılandırmalar hakkında daha fazla bilgi için bkz. [ekleme, değiştirme veya kaldırma IP adresleri](virtual-network-network-interface-addresses.md).
- Sanal Makineyi silme kendisine bağlı ağ arabirimleri silmez. Bir VM'yi sildiğinizde sanal makineden ağ arabirimleri ayrılır. Ağ arabirimleri farklı Vm'lere ekleyebilir veya silebilirsiniz.
- Bir ağ arabirimine atanmış özel bir IPv6 adresi olup olmadığını eklemeniz gerekir (Ekle), VM'yi oluştururken bir VM. VM oluşturduktan sonra bir VM'ye ağ arabirimi ile atanan bir IPv6 adresi ekleyemezsiniz. Atanan özel bir IPv6 adresi ile bir ağ arabirimi bir sanal makine oluşturduğunuzda eklerseniz, yalnızca VM boyutunu destekler kaç ağ arabirimleri bakılmaksızın sanal makinenin ağ arabirimi ekleyebilirsiniz. Bkz: [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md) IP adreslerini ağ arabirimlerine atama hakkında daha fazla bilgi için.
- IPv6 olarak ile hızlandırılmış ağ ile bir VM oluşturduktan sonra etkin bir ağ arabirimine eklenemiyor. Ayrıca, hızlandırılmış ağ yararlanmak için ayrıca VM'nin işletim sistemi içindeki adımları da tamamlamanız gerekir. Hızlandırılmış ağ hakkında daha fazla bilgi ve diğer kısıtlamalar da kullanılırken öğrenin [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineler.

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ arabirimleri veya IP adresi ile bir VM oluşturmak için aşağıdaki makalelere bakın:

|Görev|Tool|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresi ile tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında, Azure Load Balancer) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
