---
title: Oluşturma, değiştirme veya bir Azure ağı arabirimi silme | Microsoft Docs
description: Bilgi bir ağ arabirimi ve nasıl oluşturulacağını, ayarlarını değiştirin ve birini silin.
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 72c3968b59fda10d81af553cbf2324a2683c596b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-change-or-delete-a-network-interface"></a>Oluşturma, değiştirme veya bir ağ arabirimi silme

Oluşturma, ayarlarını değiştirin ve ağ arabirimi silme öğrenin. Bir ağ arabirimi, internet, Azure ve şirket içi kaynakları ile iletişim kurmak için bir Azure sanal makine sağlar. Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Bunun yerine, ağ arabirimleri ile özel ayarları oluşturmak ve oluşturduğunuzda bir veya daha fazla ağ arabirimine bir sanal makineye eklemek seçebilirsiniz. Varolan bir ağ arabirimi için varsayılan ağ arabirimi ayarları değiştirmek isteyebilirsiniz. Bu makalede, bir ağ arabirimi ile özel ayarları oluşturmak, ağ filtre (ağ güvenlik grubu) ataması, alt ağ ataması, DNS sunucusu ayarlarını ve IP iletimini gibi varolan ayarlarını değiştirin ve ağ arabirimini silmek açıklanmaktadır.

Eklemek, değiştirmek veya bir ağ arabirimi için IP adreslerini kaldırın, bkz: [yönetmek IP adresleri](virtual-network-network-interface-addresses.md). Ağ arabirimleri veya ağ arabirimleri sanal makinelerden kaldırmak için bkz: için eklemeniz gerekiyorsa, [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md).


## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.28 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

Aboneliğiniz için ağ katılımcı rolü için en düşük izinleri adresindeki Azure ile içine oturum hesabı atanmalıdır. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).

## <a name="create-a-network-interface"></a>Bir ağ arabirimi oluştur

Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Tüm ağ arabirimi ayarları yerine belirtirsiniz, bir ağ arabirimi ile özel ayarları oluşturmak ve ağ arabirimi (PowerShell veya Azure CLI kullanarak) sanal makine oluştururken, bir sanal makineye Ekle. Ayrıca, bir ağ arabirimi oluşturabilir ve varolan bir sanal makineye (PowerShell veya Azure CLI kullanarak) ekleyin. Varolan bir ağ arabirimi ile bir sanal makine oluşturmak için eklemek veya var olan sanal makinelerden ağ arabirimleri Kaldır öğrenmek için bkz: [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md). Bir ağ arabirimi oluşturmadan önce varolan olmalıdır [sanal ağ](manage-virtual-network.md#create-a-virtual-network) aynı konumu ve abonelik bir ağ arabiriminde oluşturun.

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Seçin **+ Ekle** altında **ağ arabirimleri**.
3. Girin veya aşağıdaki ayarları için değerleri seçin, sonra seçin **oluşturma**:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Ad|Evet|Adı kaynak grubun içinde benzersiz olmalıdır. Zamanla, Azure aboneliğinizde büyük olasılıkla birden fazla ağ arabirimleri sahip olacaksınız. Çeşitli ağ arabirimleri daha kolay yönetme yapmak bir adlandırma kuralını oluştururken öneriler için bkz [adlandırma kuralları](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Ağ arabirimi oluşturulduktan sonra adı değiştirilemez.|
    |Sanal ağ|Evet|Ağ arabirimi için sanal ağ seçin. Yalnızca aynı abonelik ve konum ağ arabirimi olarak mevcut bir sanal ağ bir ağ arabirimi atayabilirsiniz. Bir ağ arabirimi oluşturulduktan sonra sanal ağ atandığı değiştiremezsiniz. Ağ arabirimine eklediğiniz sanal makinenin de aynı konumu ve ağ arabirimi olarak abonelik bulunmalıdır.|
    |Alt ağ|Evet|Seçtiğiniz sanal ağ içindeki bir alt ağ seçin. Oluşturulduktan sonra ağ arabirimi atandığı alt değiştirebilirsiniz.|
    |Özel IP adresi ataması|Evet| Bu ayar, IPv4 adresi için atama yöntemi seçme. Aşağıdaki atama yöntemler arasından seçim yapın: **dinamik:** bu seçeneğin belirlenmesi, Azure sonraki kullanılabilir adrese seçtiğiniz alt ağ adresi alanından otomatik olarak atar. **Statik:** bu seçeneğin belirlenmesi, bir kullanılabilir IP adresi seçtiyseniz alt ağın adres alanı içinde el ile atamanız gerekir. Statik ve dinamik adresler, bunları değiştirebilir veya ağ arabirimi silinene kadar değiştirmeyin. Ağ arabirimi oluşturulduktan sonra atama yöntemi değiştirebilirsiniz. Azure DHCP sunucusu, ağ arabirimi sanal makinenin işletim sisteminde bu adresi atar.|
    |Ağ güvenlik grubu|Hayır| Ayarlanmış olarak bırakın **hiçbiri**, var olan seçin [ağ güvenlik grubu](virtual-networks-nsg.md), veya [bir ağ güvenlik grubu oluşturun](virtual-networks-create-nsg-arm-pportal.md). Ağ güvenlik grupları ağ trafiği bir ağ arabirimi filtrelemek için etkinleştirin. Sıfır veya bir ağ güvenlik grubu için bir ağ arabirimi uygulayabilirsiniz. Sıfır veya bir ağ güvenlik grubu, ağ arabiriminin atandığı alt ağa da uygulanabilir. Bir ağ arabirimi ve ağ arabirimi atandığı alt ağ için ağ güvenlik grubu uygulandığında, bazen beklenmeyen sonuçlar oluşur. Ağ arabirimleri ve alt ağları ile uygulanan ağ güvenlik grupları gidermek için bkz: [ağ güvenlik grupları sorun giderme](virtual-network-nsg-troubleshoot-portal.md#nsg).|
    |Abonelik|Evet|Azure birini [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Bir ağ arabirimi ve kendisine bağlanan sanal ağ ekleme sanal makine aynı abonelikte olması gerekir.|
    |Özel IP adresi (IPv6)|Hayır| Bu onay kutusunu seçerseniz, bir IPv6 adresi bir ağ arabirimine atanmış IPv4 adresi ek olarak ağ arabiriminin atanır. Bkz: [IPv6](#IPv6) hakkında önemli bilgiler için kullanım IPv6 ağ arabirimi ile bu makalenin. Bir IPv6 adresi için bir atama yöntemi seçemezsiniz. Bir IPv6 adresi atamak seçerseniz, dinamik yöntemiyle atanır.
    |IPv6 adı (yalnızca zaman görünür **özel IP adresi (IPv6)** onay kutusu işaretli) |Evet, varsa **özel IP adresi (IPv6)** onay kutusu işaretli.| Bu ad, bir ikincil ağ arabirimi IP yapılandırmasına atanır. IP yapılandırmaları hakkında daha fazla bilgi için bkz: [görünüm ağ arabirimi ayarları](#view-network-interface-settings).|
    |Kaynak grubu|Evet|Var olan seçin [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) veya bir tane oluşturun. Bir ağ arabirimi, eklediğiniz sanal makine daha aynı veya farklı kaynak grubunda var olabilir veya sanal ağ, ona bağlanın.|
    |Konum|Evet|Sanal makine bir ağ arabirimi ekleyin ve ona bağlandığınız sanal ağ aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions), de denilen bir bölge.|

Portal portal genel bir IP adresi oluşturun ve Portalı'nı kullanarak bir sanal makine oluşturduğunuzda, bir ağ arabirimine atayın olsa da, oluşturduğunuzda, ağ arabirimi genel bir IP adresi atamak için seçeneği sağlamaz. Oluşturduktan sonra ağ arabirimi genel bir IP adresi eklemeyi öğrenmek için bkz: [yönetmek IP adresleri](virtual-network-network-interface-addresses.md). Bir ortak IP adresiyle bir ağ arabirimi oluşturmak istiyorsanız, ağ arabiriminin oluşturmak için CLI veya PowerShell kullanmanız gerekir.

Portal ağ arabirimi için uygulama güvenlik grupları atama seçeneğiniz sağlamaz, ancak Azure CLI ve PowerShell yapın. Uygulama güvenlik grupları hakkında daha fazla bilgi için bkz: [uygulama güvenlik grupları](security-overview.md#application-security-groups).

>[!Note]
> Azure yalnızca ağ arabirimi bir sanal makineye bağlı ve sanal makine ilk kez başlatıldığında Ağ arabirimi için bir MAC adresi atar. Azure ağ arabirimine atar MAC adresi belirtemezsiniz. Ağ arabirimi silinmiş veya birincil ağ arabirimi birincil IP yapılandırmasının atanan özel IP adresi değiştirilmiş kadar MAC adresi ağ arabirimine atanmış olarak kalır. IP adresleri ve IP yapılandırmaları hakkında daha fazla bilgi için bkz: [yönetmek IP adresleri](virtual-network-network-interface-addresses.md)

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic#az_network_nic_create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface#create)|

## <a name="view-network-interface-settings"></a>Ağ arabirimi ayarlarını görüntüleme

Görüntüleyin ve oluşturulduktan sonra bir ağ arabirimi için çoğu ayarlarını değiştirin. Portal DNS soneki ya da uygulama güvenlik grubu üyeliği ağ arabirimi için görüntülemez. PowerShell veya Azure CLI kullanabilirsiniz [komutları](#view-settings-commands) DNS soneki ve uygulama güvenlik grubu üyeliği görüntülemek için.

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Görüntülemek veya listeden ayarlarını değiştirmek istediğiniz ağ arabirimi seçin.
3. Aşağıdaki öğeler, seçtiğiniz ağ arabirimi için listelenmiştir:
    - **Genel Bakış:** , sanal ağ/ağ arabirimi atanması alt ağ ve ağ arabirimi eklendiği (bağlı olduğu, sanal makine için atanan IP adresleri gibi ağ arabirimi hakkında bilgi sağlar bir tane). Aşağıdaki resimde adlı ağ arabirimi için genel ayarları gösterilmiştir **mywebserver256**: ![ağ arabirimi genel bakış](./media/virtual-network-network-interface/nic-overview.png) farklı bir kaynak grubu için bir ağ arabirimi taşıyabilirsiniz veya Abonelik seçerek (**değiştirme**) yanındaki **kaynak grubu** veya **abonelik adı**. Ağ arabirimi taşırsanız, ağ arabiriminin onunla ilişkili tüm kaynakları taşımanız gerekir. Örneğin, ağ arabirimi bir sanal makineye bağlıysa, aynı zamanda sanal makine ve diğer ilgili sanal makine kaynakları taşımalısınız. Bir ağ arabirimi taşımak için bkz: [yeni kaynak grubu veya abonelik için kaynak taşıma](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Makaleyi önkoşulları ve Azure portalı, PowerShell ve Azure CLI kullanarak kaynakları taşıma listeler.
    - **IP yapılandırması:** IP yapılandırmaları için atanan ortak ve özel IPv4 ve IPv6 adreslerini burada listelenir. Bir IPv6 adresi bir IP yapılandırmasına atanırsa, adres görüntülenmez. IP yapılandırmaları ve IP adresi eklemesine ve kaldırmasına nasıl hakkında daha fazla bilgi için bkz: [yapılandırma IP adresleri için bir Azure ağı arabirimi](virtual-network-network-interface-addresses.md). IP iletimi ve alt ağ ataması, bu bölümde de yapılandırılır. Bu ayarlar hakkında daha fazla bilgi için bkz: [etkinleştirmek veya IP iletimini devre dışı](#enable-or-disable-ip-forwarding) ve [değiştirme alt ağ ataması](#change-subnet-assignment).
    - **DNS sunucuları:** bir ağ arabirimi Azure DHCP sunucuları tarafından atanan hangi DNS sunucusunun belirtebilirsiniz. Ağ arabirimi ağ arabirimi için atanan sanal ağ ayarlarını devral veya atandığı sanal ağ ayarını geçersiz kılar özel bir ayar vardır. Görüntülenenleri değiştirmek için bkz: [değişiklik DNS sunucuları](#change-dns-servers).
    - **Ağ güvenlik grubu (NSG):** , NSG (varsa) bir ağ arabirimine ilişkili olan görüntüler. Bir NSG'yi ağ arabirimi için ağ trafiğini filtrelemek için gelen ve giden kurallarını içerir. Bir NSG'yi bir ağ arabirimine ilişkiliyse, ilişkili NSG adı görüntülenir. Görüntülenenleri değiştirmek için bkz: [ilişkilendirmek veya bir ağ güvenlik grubu ilişkilendirmesini](#associate-or-dissociate-a-network-security-group).
    - **Özellikler:** görüntüler anahtar ayarları var, MAC adresini (ağ arabirimi bir sanal makineye bağlı değil, boş) ve abonelik dahil olmak üzere ağ arabiriminin hakkında.
    - **Etkin güvenlik kuralları:** güvenlik kuralları, ağ arabiriminin çalışan bir sanal makineye bağlı ve bir NSG ağ arabirimi, atanan için alt ağ veya her ikisi de ilişkili ise listelenir. Görüntülenenleri hakkında daha fazla bilgi için bkz: [etkin güvenlik kuralları](#view-effective-security-rules). Nsg'ler hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](security-overview.md).
    - **Etkin yollar:** ağ arabirimi çalışan bir sanal makineye bağlıysa yolları listelenir. Yolları, Azure varsayılan yolların, tüm kullanıcı tanımlı yollar ve ağ arabirimi atandığı alt ağ için bulunabilecek tüm BGP yollarını birleşimidir. Görüntülenenleri hakkında daha fazla bilgi için bkz: [görüntülemek etkili yolları](#view-effective-routes). Azure varsayılan yollar ve kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
    - **Ortak Azure Resource Manager ayarları:** ortak Azure Resource Manager ayarları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [etiketleri](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), ve [Otomasyon betiğini](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

<a name="view-settings-commands"></a>**Komutları**

Bir IPv6 adresi için bir ağ arabirimi atanırsa, PowerShell çıkış adresi atanmış, ancak atanan adresi döndürmez olgu döndürür. Benzer şekilde, CLI adresi atanmış, ancak döndürür olgu döndürür *null* çıktısını adresi için de.

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC listesi](/cli/azure/network/nic#az_network_nic_list) ; aboneliğindeki ağ arabirimlerini görüntülemek için [az ağ NIC Göster](/cli/azure/network/nic#az_network_nic_show) bir ağ arabiriminin ayarlarını görüntülemek için|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) bir ağ arabirimi için abonelik veya Görünümü Ayarları'nda ağ arabirimlerini görüntülemek için|

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

DNS sunucusu, ağ arabirimi sanal makine işletim sistemi içinde Azure DHCP sunucusu tarafından atanır. Atanan DNS sunucusu, bir ağ arabirimi için DNS sunucusu ayarını ne olursa olsun ' dir. Bir ağ arabirimi için ad çözümleme ayarları hakkında daha fazla bilgi için bkz: [ad çözümlemesi için sanal makineler](virtual-networks-name-resolution-for-vms-and-role-instances.md). Ağ arabirimi sanal ağdan ayarları devralır veya sanal ağ ayarını geçersiz kılmak kendi benzersiz ayarları kullanın.

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Listeden bir DNS sunucusu için değiştirmek istediğiniz ağ arabirimi seçin.
3. Seçin **DNS sunucuları** altında **ayarları**.
4. Şunlardan birini seçin:
    - **Sanal ağdan devral**: ağ arabirimi için atanan sanal ağ için tanımlanan DNS sunucusu ayarını devralmak için bu seçeneği seçin. Sanal ağ düzeyinde tanımlanan özel bir DNS sunucusu veya Azure tarafından sağlanan DNS sunucusu. Azure tarafından sağlanan DNS sunucusu ana bilgisayar adları aynı sanal ağa atanan kaynaklar için çözümleyebilir. FQDN farklı sanal ağlar için atanmış kaynaklar için çözümlemek için kullanılması gerekir.
    - **Özel**: birden çok sanal ağlar arasında adları çözümlemek için kendi DNS sunucusunu yapılandırabilirsiniz. Bir DNS sunucusu olarak kullanmak istediğiniz sunucunun IP adresini girin. Belirttiğiniz DNS sunucusu adresi, yalnızca bu ağ arabirimi atanır ve ağ arabirimi için atanan sanal ağı için tüm DNS ayarını geçersiz kılar.
5. **Kaydet**’i seçin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>Etkinleştirmek veya IP iletimini devre dışı bırakma

IP iletimi bir ağ arabiriminin bağlı olduğu sanal makine sağlar:
- Ağ trafiği için herhangi bir ağ arabirimine atanmış IP yapılandırmalarının atanan IP adreslerinden biri hedeflemeyen alırsınız.
- Bir ağ arabiriminin IP yapılandırmaları birine atanan olandan farklı bir kaynak IP adresine sahip ağ trafiğinin gönderebilir.

Ayarı, sanal makine iletmek için gereken trafiği alan sanal makineye bağlı her ağ arabirimi için etkinleştirilmelidir. Bir sanal makine trafiği birden çok ağ arabirimi bulunur veya tek bir ağ arabirimine ekli olmadığını iletebilirsiniz. IP iletimi Azure bir ayar olmakla birlikte, sanal makine trafiğin güvenlik duvarı, WAN iyileştirmesi ve Yük Dengeleme uygulamalar gibi iletebilir bir uygulama da çalıştırmanız gerekir. Bir sanal makine ağ uygulamaları çalıştırırken, sanal makine genellikle ağ sanal gereç olarak adlandırılır. Ağ, sanal uygulamaları dağıtmak hazır bir listesini görüntüleyebileceğiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). IP iletimi genellikle kullanıcı tanımlı yollar ile kullanılır. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md).

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Etkinleştirmek veya IP için iletme devre dışı bırakmak istediğiniz ağ arabirimi seçin.
3. Seçin **IP yapılandırmaları** içinde **ayarları** bölümü.
4. Seçin **etkin** veya **devre dışı** (varsayılan ayar) ayarını değiştirmek için.
5. **Kaydet**’i seçin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="change-subnet-assignment"></a>Alt ağ ataması değiştirme

Alt ağ, ancak bir ağ arabirimi atanan sanal ağda değil, değiştirebilirsiniz.

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Alt ağ ataması için değiştirmek istediğiniz ağ arabirimi seçin.
3. Seçin **IP yapılandırmaları** altında **ayarları**. Tüm özel IP adresleri IP yapılandırmaları için listelenen varsa **(statik)** bunları yanında, IP adres ataması yöntemi için dinamik adımları tamamlayarak değiştirmeniz gerekir. Ağ arabirimi için alt ağ ataması değiştirmek için tüm özel IP adresleri dinamik atama yöntemiyle atanması gerekir. Dinamik yöntemiyle atanmış adresleri, 5. adıma geçin. Statik atama yöntemiyle herhangi bir IPv4 adresi atanmışsa atama yöntemi dinamik olarak değiştirmek için aşağıdaki adımları tamamlayın:
    - IPv4 adres atama yöntemi için IP yapılandırmaları listesinden değiştirmek istediğiniz IP yapılandırması seçin.
    - Seçin **dinamik** özel IP adresi için **atama** yöntemi. Statik atama yöntemi ile bir IPv6 adresi atanamıyor.
    - **Kaydet**’i seçin.
4. İçin ağ arabiriminden taşımak istediğiniz alt ağ seçin **alt** aşağı açılan liste.
5. **Kaydet**’i seçin. Yeni dinamik adresleri yeni alt ağ için alt ağ adres aralığından atanır. İsterseniz yeni bir alt ağ ağ arabiriminin atadıktan sonra statik bir IPv4 adresi yeni alt ağ adres aralığından atayabilirsiniz. Daha fazla hakkında ekleme, değiştirme ve bir ağ arabirimi için IP adresleri kaldırma bilgi edinmek için [yönetmek IP adresleri](virtual-network-network-interface-addresses.md).

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC IP yapılandırmasını güncelleştir](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig)|

## <a name="add-to-or-remove-from-application-security-groups"></a>Ekleme veya uygulama güvenlik gruplarından kaldırın

Portal olmayan bir ağ arabirimine atamak için seçeneği belirtin veya bir ağ arabirimi uygulama güvenlik gruplarından kaldırın, ancak Azure CLI ve PowerShell yapın. Uygulama güvenlik grupları hakkında daha fazla bilgi için bkz: [uygulama güvenlik grupları](security-overview.md#application-security-groups) ve [uygulama güvenlik grubu oluşturma](#create-an-application-security-group).

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="associate-or-dissociate-a-network-security-group"></a>İlişkilendirme veya bir ağ güvenlik grubu ilişkilendirmesini Kaldır

1. Portal üstündeki arama kutusuna girin *ağ arabirimleri* arama kutusuna. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Ağ arabirimi için ağ güvenlik grubu ilişkilendirmek istediğiniz listesinden seçin veya bir ağ güvenlik grubundan ilişkisini kaldırın.
3. Seçin **ağ güvenlik grubu** altında **ayarları**.
4. **Düzenle**’yi seçin.
5. Seçin **ağ güvenlik grubu** ve ardından istediğiniz seçin veya ağ arabirimine ilişkilendirmek için ağ güvenlik grubu seçin **hiçbiri**, bir ağ güvenlik grubu ilişkilendirmesini kaldırmak.
6. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ NIC güncelleştirme](/cli/azure/network/nic#az-network-nic-update)
- PowerShell: [kümesi AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)

## <a name="delete-a-network-interface"></a>Bir ağ arabirimi Sil

Bir sanal makineye bağlı olmayan sürece, bir ağ arabirimi silebilirsiniz. Bir ağ arabirimi bir sanal makineye bağlıysa, gerekir ilk sanal makine durduruldu (serbest bırakıldığında) durumda yerleştirin ve ardından sanal makineden ağ arabirimini ayır. Bir sanal makineden ağ arabirimini ayırmak için adımları tamamlamanız [bir sanal makineden ağ arabirimini Ayır](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). Ancak sanal makineye bağlı yalnızca ağ arabirimi ise, bir ağ arabirimi bir sanal makineden ayıramazsınız. Bir sanal makine her zaman bağlı en az bir ağ arabirimine sahip olmalıdır. Bir sanal makine silme bağlı tüm ağ arabirimleri ayırır, ancak ağ arabirimleri silmez.

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünecek, onu seçin.
2. Seçin **...**  istediğiniz ağ arabirimleri listeden silmek için ağ arabiriminin sağ taraftaki.
3. **Sil**’i seçin.
4. Seçin **Evet** ağ arabiriminin silme işlemini onaylamak için.

Bir ağ arabirimi sildiğinizde, kendisine atanmış MAC veya IP adresi yayınlanır.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC Sil](/cli/azure/network/nic#az_network_nic_delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface)|

## <a name="resolve-connectivity-issues"></a>Bağlantı sorunlarını gidermek

İçin veya bir sanal makineden iletişim kurmak için güvenlik grubu güvenlik kuralları ağ sorunu yaşıyor veya yolları bir ağ arabirimi için etkili soruna neden Sorunu gidermek için aşağıdaki seçenekleriniz vardır:

### <a name="view-effective-security-rules"></a>Etkin güvenlik kurallarını görüntüle

Bir sanal makineye bağlı her ağ arabirimi için etkili güvenlik kuralları içinde bir ağ güvenlik grubu oluşturulan kuralları bir bileşimidir ve [güvenlik kuralları varsayılan](security-overview.md#default-security-rules). Bir ağ arabirimi için etkili güvenlik kurallarını anlama neden için veya bir sanal makineden iletişim kuramıyor belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için etkili kuralları görüntüleyebilirsiniz.

1. Portal üstündeki arama kutusuna için etkili güvenlik kuralları görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineleri* arama kutusuna. Zaman **sanal makineleri** arama sonuçlarında görünecek, onu seçin ve sonra listeden bir sanal makineyi seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adı seçin.
4. Seçin **etkin güvenlik kuralları** altında **destek + sorun giderme**.
5. Doğru kuralları gerekli gelen ve giden iletişim için olup olmadığını belirlemek için etkili güvenlik kuralları listesini gözden geçirin. Listede gördüğünüz hakkında daha fazla bilgi [ağ güvenlik grubu genel bakış](security-overview.md).

IP akış doğrulayın Özelliği Azure Ağ İzleyicisi, ayrıca güvenlik kuralları bir sanal makine ve bir uç nokta arasındaki iletişimi engelliyor belirlemenize yardımcı olabilir. Daha fazla bilgi için bkz: [IP akış doğrulayın](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Komutları**

- Azure CLI: [az ağ NIC listesi etkili nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg)
- PowerShell: [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup) 

### <a name="view-effective-routes"></a>Görünüm etkili yolları

Etkin bir sanal makineye bağlı ağ arabirimleri için varsayılan yollar bileşimi, oluşturduğunuz tüm yollar ve ağlardan şirket içi BGP aracılığıyla bir Azure sanal ağı ağ geçidi üzerinden yayılan yollar yollardır. Bir ağ arabirimi için etkili rotaları anlama neden için veya bir sanal makineden iletişim kuramıyor belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için etkili rotaları görüntüleyebilirsiniz.

1. Portal üstündeki arama kutusuna için etkili güvenlik kuralları görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineleri* arama kutusuna. Zaman **sanal makineleri** arama sonuçlarında görünecek, onu seçin ve sonra listeden bir sanal makineyi seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adı seçin.
4. Seçin **etkili yolları** altında **destek + sorun giderme**.
5. Doğru yol gerekli gelen ve giden iletişim için olup olmadığını belirlemek için etkili yolların listesini gözden geçirin. Listede gördüğünüz hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

Azure Ağ İzleyicisi'nin sonraki atlama özelliği yolları bir sanal makine ve bir uç nokta arasındaki iletişimi engelliyor belirlemenize de yardımcı olabilir. Daha fazla bilgi için bkz: [sonraki atlama](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Komutları**

- Azure CLI: [az ağ NIC Göster-etkin-yol-tablosu](/cli/azure/network/nic#az-network-nic-show-effective-route-table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable)

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ arabirimlerine veya IP adresleri ile bir sanal makine oluşturmak için aşağıdaki makalelere bakın:

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
