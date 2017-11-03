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
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 7df1dfbea8c985907d5330819dc1e7bf1578aafa
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Sanal makinelerden ağ arabirimlerine Ekle Kaldır

Bir VM oluştururken, varolan bir ağ arabirimi eklemeyi öğrenin veya ekleyebilir veya var olan bir VM durduruldu (serbest bırakıldığında) durumunda ağ arabirimleri kaldırın. Bir ağ arabirimi bir Azure sanal makine (Internet, Azure ve şirket içi kaynakları ile iletişim kurmak için VM) sağlar. Bir VM, bir veya daha fazla ağ arabirimine sahip olabilir. 

Eklemeniz gerekiyorsa, değiştirmek veya okuma bir ağ arabirimi için IP adresleri kaldırmak [ağ arabirimi IP adreslerini yönetmek](virtual-network-network-interface-addresses.md) makalesi. Gerektiğinde oluşturmak için değiştirmek veya ağ arabirimleri silin, okuyun [ağ arabirimlerini yönetme](virtual-network-network-interface.md) makalesi.

## <a name="before"></a>Başlamadan önce

Bu makalenin herhangi bölümündeki tüm adımları tamamlanmadan önce aşağıdaki görevleri tamamlayın:

- Kaç tane ağ arabirimleri hakkındaki gözden geçirerek her Linux ve Windows VM boyutu destekler öğrenin [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri.
- Azure oturum açma [portal](https://portal.azure.com), Azure komut satırı arabirimi (CLI) ya da Azure PowerShell ile bir Azure hesabı. Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede, görevleri tamamlamak için PowerShell komutlarını kullanarak, [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure PowerShell cmdlet'leri'nın en son sürümüne sahip olun. PowerShell komutlarıyla örnekler, yardım almanın yazın `get-help <command> -full`.
- Bu makalede, görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com).

## <a name="about"></a>Ağ arabirimleri ve sanal makineleri hakkında

Ekleyebileceğiniz (Ekle) var olan bir ağ arabirimine bir VM ağ arabirimi sağlanan VM oluşturma zamanı şu anda başka bir VM'ye bağlı değil. Bir ağ arabirimi ekleme veya kaldırma (detach) bir ağ arabiriminden için VM sağlanan mevcut bir VM'yi durduruldu (serbest bırakıldığında) durumdadır. Azure Portalı'nı kullanarak bir VM oluşturun, portal ağ arabirimi sizin için varsayılan ayarlarla oluşturur. Portal için izin vermez:

- VM oluşturulurken eklemek için var olan bir ağ arabirimi belirtin
- Birden çok ağ arabirimi ile VM oluşturma
- (Portal varsayılan adı ile ağ arabirimi oluşturur) ağ arabirimi için bir ad belirtin

Portal kullanamazsınız önceki özniteliklere sahip bir ağ arabirimi veya VM oluşturmak için Azure PowerShell veya CLI kullanın. Aşağıdaki bölümlerde görevleri tamamlamadan önce aşağıdaki kısıtlamalar ve davranışları göz önünde bulundurun:

- Tüm VM boyutları en az iki ağ arabirimi destekler ancak ikiden fazla ağ arabirimleri bazı VM boyutlarını destekler. Geçmişte, bazı VM boyutları yalnızca bir ağ arabirimi desteklenir. Okuma her VM boyutu destekler kaç ağ arabirimleri öğrenmek için [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri. 
- Geçmişte, ağ arabirimleri yalnızca desteklenen birden çok ağ arabirimleri ve en az iki ağ arabirimi ile oluşturulan VM'ler eklenemedi. VM boyutu desteklenen birden çok ağ arabirimleri olsa bile bir ağ arabirimi ile oluşturulmuş bir VM ağ arabirimi eklenemedi. Buna karşılık, VM'ler ile en az iki ağ arabirimleri her zaman en az iki ağ arabirimine sahip gerekiyordu oluşturduğundan ağ arabirimleri bir VM'den en az üç ağ arabirimlerine sahip yalnızca kaldırabilirsiniz. Bu kısıtlamaların hiçbiri artık geçerli. Artık herhangi bir sayıda ağ arabirimleri (kadar VM boyutu tarafından desteklenen numarası) ile bir VM oluşturun ve da ekleyebilir veya VM her zaman en az bir ağ arabirimine sahip olduğu sürece herhangi bir sayıda ağ arabirimleri (Vm'lerden durduruldu (serbest bırakıldığında) durumunda), kaldırabilirsiniz.
- Varsayılan olarak, bir VM'de ilk ağ arabirimi olarak tanımlanan *birincil* ağ arabirimi. Diğer tüm ağ arabirimleri VM'deki olan *ikincil* ağ arabirimleri.
- Birincil ağ arabirimine bir varsayılan ağ geçidi Azure DHCP sunucuları tarafından atanmış, ancak ikincil ağ arabirimlerine değildir. İkincil ağ arabirimlerine bir varsayılan ağ geçidi atanmamış olduğundan, bunlar kendi alt dışında kaynaklarla varsayılan olarak iletişim kuramıyor. Kendi alt ağı dışındaki kaynaklar ile iletişim kurmak ikincil ağ arabirimlerine etkinleştirmek için bkz: [birden çok ağ arabirimlerine sahip bir sanal makine işletim sistemi içinde yönlendirme](#routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces).
- Varsayılan olarak, sanal makineden giden tüm trafiği birincil ağ arabirimi birincil IP yapılandırması için atanan IP adresi çıkışı gönderilir. VM'ın işletim sistemi içinde giden trafik için hangi IP adresinin kullanıldığını denetleyen ancak varsayılan olarak, birincil ağ arabirimi üzerinden değil.
- Geçmişte, tüm sanal makineleri aynı kullanılabilirlik kümesinde, tek veya birden çok ağ arabirimine sahip olmaları gerekirdi. Tüm ağ arabirimleri sayısı olan VM'ler artık aynı kullanılabilirlik kümesinde en fazla VM boyutu tarafından desteklenen sayı bulunabilir. Kullanılabilirlik ancak oluşturulduğunda kümesi için yalnızca bir VM ekleyebilirsiniz. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için okuma [azure'da VM kullanılabilirliğini yönetme](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) makalesi.
- Ağ arabirimleri aynı VM ağ arabirimi bir sanal ağ içindeki farklı alt ağlara bağlı olsa da, tüm aynı Vnet'e bağlanmalıdır.
- Herhangi bir IP adresi herhangi bir birincil veya ikincil ağ arabirimi tüm IP yapılandırması için bir Azure yük dengeleyici arka uç havuzuna ekleyebilirsiniz. Geçmişte, yalnızca birincil IP adresi birincil ağ arabirimi için bir arka uç havuzuna eklenemiyor. IP adresleri ve yapılandırmaları hakkında daha fazla bilgi için okuma [Ekle, Değiştir veya Kaldır IP adresleri](virtual-network-network-interface-addresses.md) makalesi.
- Bir VM silindiğinde, ona bağlı olan ağ arabirimleri silinmez. Bir VM silindiğinde, ağ arabirimleri VM'den ayrılır. Ağ arabirimleri için farklı sanal makineleri ekleyin veya silin.
- Bir ağ arabirimine atanmış özel bir IPv6 adresi varsa, VM oluştururken, onu bir VM'ye ekleyebilirsiniz. VM oluşturulduktan sonra atanan bir IPv6 adresine sahip bir ağ arabirimi bir VM'ye bağlanamaz. Bir sanal makine oluşturulurken özel bir IPv6 adresi atanmış bir ağ arabirimiyle eklerseniz, VM boyutunu destekler kaç ağ arabirimleri bağımsız olarak sanal makine için ağ arabirimini, yalnızca ekleyebilirsiniz. Bkz: [ağ arabirimi IP adresleri](virtual-network-network-interface-addresses.md) ağ arabirimleri için IP adresleri atama hakkında daha fazla bilgi edinmek için.

## <a name="vm-create"></a>Yeni bir sanal makineye mevcut ağ arabirimlerinin ekleme

Portal üzerinden VM oluşturduğunuzda, portal varsayılan ayarlarla bir ağ arabirimi oluşturur ve VM sizin için ekler. Yeni bir sanal makineye mevcut ağ arabirimlerinin ekleyemez veya Azure portalını kullanarak birden çok ağ arabirimi ile bir VM oluşturun. Her iki CLI veya PowerShell kullanarak yapabilirsiniz. Oluşturmakta olduğunuz VM boyutu destekliyorsa sayıda ağ arabirimleri bir VM ekleyebilirsiniz. Her VM boyutu kaç ağ arabirimleri hakkındaki destekleyen daha fazla bilgi için okuma [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri. Bir VM'ye ekleyin ağ arabirimleri, şu anda başka bir VM eklenemiyor. Ağ arabirimleri oluşturma hakkında daha fazla bilgi için okuma [ağ arabirimlerini yönetme](virtual-network-network-interface.md#create-a-network-interface) makalesi.

### <a name="routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces"></a>Birden çok ağ arabirimlerine sahip bir sanal makine işletim sistemi içinde yönlendirme

Azure sanal makineye bağlı ilk (birincil) ağ arabirimi için varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimleri için varsayılan ağ geçidi atamaz. Bu nedenle, bir ikincil ağ arabirimi, varsayılan olarak alt ağı dışındaki kaynaklar ile iletişim kuramıyor. İletişimi etkinleştirmek için adımları farklı işletim sistemleri için farklı ancak ikincil ağ arabirimlerine ancak, kendi alt ağı dışından kaynaklarla iletişim kurabilirsiniz.

### <a name="windows"></a>Windows

Bir Windows komut isteminde aşağıdaki adımları tamamlayın:

1. Çalıştırma `route print` çıkış iki bağlı ağ arabirimine sahip bir sanal makine için çıktı aşağıdakine benzer döndürür komutu:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    Bu örnekte, **Microsoft Hyper-V ağ bağdaştırıcısı #4** (7) arabirimidir kendisine atanmış bir varsayılan ağ geçidi yok ikincil ağ arabirimi.

2. Bir komut isteminden çalıştırın `ipconfig` ikincil ağ arabirimi için hangi IP adresi atanır görmek için komutu. Bu örnekte, 192.168.2.4 arabirimi 7 atanır. İkincil ağ arabirimi için bir varsayılan ağ geçidi adresi döndürülür.

3. Alt ağ için ağ geçidi için ikincil ağ arabirimi alt ağı dışında adresler için giden tüm trafiği yönlendirmek için aşağıdaki komutu çalıştırın:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Alt ağ için ağ geçidi adresi, alt ağ için tanımlanan adres aralığındaki (Bitiş içinde.1) ilk IP adresi değil. Alt ağ dışında tüm trafiği yönlendirmek istemiyorsanız, bireysel rotaların belirli hedeflere, bunun yerine ekleyebilirsiniz. Örneğin, yalnızca 192.168.3.0 ikincil ağ arabirimi trafiği yönlendirmek istiyorsanız ağ, aşağıdaki komutu girin:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. 192.168.3.0 kaynakta ile başarıyla iletişim onaylamak için ağ, örneğin, 7 (192.168.2.4) arabirimini kullanarak 192.168.3.4 ping işlemi yapmak için aşağıdaki komutu girin:

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Aşağıdaki komutla ping işlemi sırasında aygıt Windows Güvenlik Duvarı üzerinden ICMP açmanız gerekebilir:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Eklenen yoldur rota tablosunda onaylamak için girin `route print` çıkışı aşağıdaki metni benzer döndürür komutu:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    Listelenen rota *192.168.1.1* altında **ağ geçidi**, birincil ağ arabirimi için varsayılan olarak olduğundan yoldur. Rota ile *192.168.2.1* altında **ağ geçidi**, eklediğiniz yoldur.

### <a name="linux"></a>Linux

Varsayılan davranış zayıf ana bilgisayar yönlendirme kullandığından, aynı alt ağdaki kaynaklar arasındaki ikincil ağ arabirimi trafiği kısıtlama öneririz. Alt ağı dışından iletişim için ikincil ağ arabirimleri gerektiriyorsa, sanal makinenin belirli bir ağ arabirimi üzerinden trafik gönderip izin veren yönlendirme kuralları oluşturmanız gerekir. Aksi takdirde eth1 için ait olduğu trafiği Örneğin, doğru tarafından tanımlanan varsayılan rota işlenemiyor. Yönlendirme kurallarını yapılandırma hakkında bilgi edinmek için [yapılandırma Linux birden çok NIC için](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics).

> [!WARNING]
> Bir ağ arabirimine atanmış özel bir IPv6 adresi varsa, yalnızca ağ arabirimi sanal makineye sanal makine oluştururken ekleyebilirsiniz. Sanal makine oluşturduğunuzda ya da sanal makineyi sonra oluşturulan, bir IPv6 adresi, bir sanal makineye bağlı bir ağ arabirimine atanmış sürece sanal makineye birden fazla ağ arabirimi eklenemiyor. Bkz: [ağ arabirimi IP adresleri](virtual-network-network-interface-addresses.md) ağ arabirimleri için IP adresleri atama hakkında daha fazla bilgi edinmek için.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az vm oluşturma](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Varolan bir ağ arabirimi için mevcut bir VM'yi ekleme

Ağ arabirimlerine ekleme ve VM boyutunu destekler sayıda ağ arabirimleri bir VM ekleyebilirsiniz. Okuma her VM boyutu destekler kaç ağ arabirimleri öğrenmek için [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM boyutları makaleleri. Bir ağ arabirimi için eklemek istediğiniz VM eklemek istediğiniz ve durduruldu (serbest bırakıldığında) durumda olmalıdır ağ arabirimleri sayısı desteklemesi gerekir. Eklemek istediğiniz ağ arabirimleri, şu anda başka bir VM eklenemiyor. Azure Portalı'nı kullanarak var olan bir VM ağ arabirimleri ekleyemezsiniz. Ağ arabirimleri için mevcut bir VM'yi eklemek için CLI veya PowerShell kullanmanız gerekir. 

Azure sanal makineye bağlı ilk (birincil) ağ arabirimi için varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimleri için varsayılan ağ geçidi atamaz. Bu nedenle, bir ikincil ağ arabirimi, varsayılan olarak alt ağı dışındaki kaynaklar ile iletişim kuramıyor. İkincil ağ arabirimlerine ancak, kendi alt ağı dışından kaynaklarla iletişim kurabilirsiniz. İkincil ağ kendi alt ağı dışından kaynaklarla iletişim arabirimleri ihtiyacınız varsa bkz [birden çok ağ arabirimlerine sahip bir sanal makine işletim sistemi içinde yönlendirme](#routing-within-a virtual-machine-operating-system-with-multiple-network-interfaces).

> [!WARNING]
> Bir ağ arabirimine atanmış özel bir IPv6 adresi varsa, varolan bir sanal makineye eklenemez. Bir sanal makine oluşturduğunuzda, yalnızca bir sanal makineye atanan özel bir IPv6 adresine sahip bir ağ arabirimi ekleyebilirsiniz. Bkz: [ağ arabirimi IP adresleri](virtual-network-network-interface-addresses.md) ağ arabirimleri için IP adresleri atama hakkında daha fazla bilgi edinmek için.

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC eklemeniz](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Ekleme AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımları](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Bir VM için görünümü ağ arabirimleri

Her ağ arabiriminin yapılandırma hakkında bilgi edinmek için bir VM için şu anda bağlı ağ arabirimleri ve her bir ağ arabirimine atanmış IP adreslerini görüntüleyebilirsiniz. 

1. Oturum [Azure portal](https://portal.azure.com) aboneliğiniz için sahip, katkıda bulunan veya ağ Katılımcısı rolüne atanmış bir hesap ile. Hesaplarına rol atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *sanal makineleri*. Zaman **sanal makineleri** görünür arama sonuçlarında tıklatın.
3. İçinde **sanal makineleri** görünür, dikey ağ arabirimleri için görüntülemek istediğiniz VM adını tıklatın.
4. İçinde **ayarları** VM için görünür sanal makine dikey bölümünde seçili, tıklatın **ağ**. Ağ arabirimi ayarları ve bunları değiştirme hakkında bilgi edinmek için okuma [ağ arabirimlerini yönetme](virtual-network-network-interface.md) makalesi. Değiştirerek veya kaldırarak bir ağ arabirimine atanmış IP adresleri ekleme hakkında bilgi için bkz [yönetmek IP adresleri](virtual-network-network-interface-addresses.md).

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az vm Göster](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Bir ağ arabirimi bir sanal makineden kaldırın

Bir ağ arabirimi Kaldır (veya ayırmak için) istediğiniz VM durduruldu (serbest bırakıldığında) durumda olması gerekir ve şu anda en az iki ağ arabirimi bağlı olması gerekir. Herhangi bir ağ arabirimi kaldırabilirsiniz, ancak VM her zaman bağlı en az bir ağ arabirimine sahip olmalıdır. Birincil Ağ arabirimi kaldırırsanız, Azure VM'ye bağlı ağ arabirimi birincil öznitelik atar uzun. 

1. Oturum [Azure portal](https://portal.azure.com) aboneliğiniz için sahip, katkıda bulunan veya ağ Katılımcısı rolüne atanmış bir hesap ile. Hesaplarına rol atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *sanal makineleri*. Zaman **sanal makineleri** görünür arama sonuçlarında tıklatın.
3. İçinde **sanal makineleri** görünür, dikey bir ağ arabirimi için kaldırmak istediğiniz VM adını tıklatın.
4. İçinde **ayarları** VM için görünür sanal makine dikey bölümünde seçili, tıklatın **ağ**. Ağ arabirimi ayarları ve bunları değiştirme hakkında bilgi edinmek için okuma [ağ arabirimlerini yönetme](virtual-network-network-interface.md) makalesi. Değiştirerek veya kaldırarak bir ağ arabirimine atanmış IP adresleri ekleme hakkında bilgi için bkz [yönetmek IP adresleri](virtual-network-network-interface-addresses.md).
5. Tıklatın **X Detach ağ arabirimi**.
6. Aşağı açılan listeden ayırın ve ardından istediğiniz ağ arabirimi seçin **Tamam**.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az vm NIC kaldırmak](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (başvuru) veya [ayrıntılı adımları](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru) veya [ayrıntılı adımları](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ arabirimlerine veya IP adreslerini bir VM oluşturmak için aşağıdaki makaleyi okuyun:

**Komutları**

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
