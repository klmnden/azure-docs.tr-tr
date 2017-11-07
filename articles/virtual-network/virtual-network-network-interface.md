---
title: "Oluşturma, değiştirme veya bir Azure ağı arabirimi silme | Microsoft Docs"
description: "Bilgi bir ağ arabirimi ve nasıl oluşturulacağını, ayarlarını değiştirin ve birini silin."
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 7dafb491cec908ffbb3683991919654f3d3eb452
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-network-interface"></a>Oluşturma, değiştirme veya bir ağ arabirimi silme

Oluşturma, ayarlarını değiştirin ve ağ arabirimi silme öğrenin. Bir ağ arabirimi, Internet, Azure ve şirket içi kaynakları ile iletişim kurmak için bir Azure sanal makine sağlar. Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Bunun yerine, ağ arabirimleri ile özel ayarları oluşturmak ve oluşturduğunuzda bir veya daha fazla ağ arabirimine bir sanal makineye eklemek seçebilirsiniz. Varolan bir ağ arabirimi için varsayılan ağ arabirimi ayarları değiştirmek isteyebilirsiniz. Bu makalede, bir ağ arabirimi ile özel ayarları oluşturmak, ağ filtre (ağ güvenlik grubu) ataması, alt ağ ataması, DNS sunucusu ayarlarını ve IP iletimini gibi varolan ayarlarını değiştirin ve ağ arabirimini silmek açıklanmaktadır.

Eklemeniz gerekiyorsa, değiştirmek veya okuma bir ağ arabirimi için IP adresleri kaldırmak [yönetmek IP adresleri](virtual-network-network-interface-addresses.md) makalesi. Ağ arabirimlerine eklemek veya ağ arabirimleri okuma sanal makinelerden kaldırmak gerekiyorsa [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md) makalesi.


## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bölümündeki tüm adımları tamamlanmadan önce aşağıdaki görevleri tamamlayın:

- Gözden geçirme [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makale ağ arabirimleri için sınırları hakkında bilgi edinin.
- Azure oturum açma [portal](https://portal.azure.com), Azure komut satırı arabirimi (CLI) ya da Azure PowerShell ile bir Azure hesabı. Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede, görevleri tamamlamak için PowerShell komutlarını kullanarak, [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure PowerShell cmdlet'leri'nın en son sürümüne sahip olun. PowerShell komutlarıyla örnekler, yardım almanın yazın `get-help <command> -full`.
- Bu makalede, görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Yüklü Azure CLI'ın en son sürümüne sahip olun. CLI komutları için Yardım almak için yazın `az <command> --help`. CLI ve ön koşullar yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın. **> _** en üstündeki düğmesi [portal](https://portal.azure.com).

## <a name="create-a-network-interface"></a>Bir ağ arabirimi oluştur

Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Tüm ağ arabirimi ayarları yerine belirtirsiniz, bir ağ arabirimi ile özel ayarları oluşturmak ve ağ arabirimi (PowerShell veya Azure CLI kullanarak) sanal makine oluştururken, bir sanal makineye Ekle. Ayrıca, bir ağ arabirimi oluşturabilir ve varolan bir sanal makineye (PowerShell veya Azure CLI kullanarak) ekleyin. Varolan bir ağ arabirimi ile bir sanal makine oluşturmak için veya eklemek veya var olan sanal makinelerden ağ arabirimleri kaldırma öğrenmek için [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md) makalesi. Bir ağ arabirimi oluşturmadan önce varolan olmalıdır [sanal ağ](virtual-networks-create-vnet-arm-pportal.md) aynı konumu ve abonelik bir ağ arabiriminde oluşturun.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey tıklayın **+ Ekle**.
4. İçinde **oluşturma ağ arabirimi** görünür, dikey girin veya aşağıdaki ayarları için değerleri seçin ve ardından **oluşturma**:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Ad|Evet|Adı kaynak grubun içinde benzersiz olmalıdır. Zamanla, Azure aboneliğinizde büyük olasılıkla birden fazla ağ arabirimleri sahip olacaksınız. Okuma [adlandırma kuralları](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) birkaç yönetme yapmak için bir adlandırma kuralını oluştururken önerileri ağ arabirimleri kolaylaştırmak için makalesi. Ağ arabirimi oluşturulduktan sonra adı değiştirilemez.|
    |Sanal ağ|Evet|Ağ arabirimi için sanal ağ seçin. Yalnızca aynı abonelik ve konum ağ arabirimi olarak mevcut bir sanal ağ bir ağ arabirimi atayabilirsiniz. Bir ağ arabirimi oluşturulduktan sonra sanal ağ atandığı değiştiremezsiniz. Ağ arabirimine eklediğiniz sanal makinenin de aynı konumu ve ağ arabirimi olarak abonelik bulunmalıdır.|
    |Alt ağ|Evet|Seçtiğiniz sanal ağ içindeki bir alt ağ seçin. Oluşturulduktan sonra ağ arabirimi atandığı alt değiştirebilirsiniz.|
    |Özel IP adresi ataması|Evet| Bu ayar, IPv4 adresi için atama yöntemi seçme. Aşağıdaki atama yöntemler arasından seçim yapın: **dinamik:** bu seçeneğin belirlenmesi, Azure kullanılabilir bir adresi seçtiğiniz alt ağ adresi alanından otomatik olarak atar. İçinde sanal makine durduruldu (serbest bırakıldığında) durumda bırakıldı sonra başlatıldığında azure bir ağ arabirimi için farklı bir adres atayabilir. Durduruldu (serbest bırakıldığında) durumda bırakıldı olmadan sanal makine yeniden başlatılırsa adresi aynı kalır. **Statik:** bu seçeneğin belirlenmesi, bir kullanılabilir IP adresi seçtiyseniz alt ağın adres alanı içinde el ile atamanız gerekir. Statik adresler, bunları değiştirebilir veya ağ arabirimi silinene kadar değiştirmeyin. Ağ arabirimi oluşturulduktan sonra atama yöntemi değiştirebilirsiniz. Azure DHCP sunucusu, ağ arabirimi sanal makinenin işletim sisteminde bu adresi atar.|
    |Ağ güvenlik grubu|Hayır| Ayarlanmış olarak bırakın **hiçbiri**, var olan seçin [ağ güvenlik grubu](virtual-networks-nsg.md), veya [bir ağ güvenlik grubu oluşturun](virtual-networks-create-nsg-arm-pportal.md). Ağ güvenlik grupları ağ trafiği bir ağ arabirimi filtrelemek için etkinleştirin. Sıfır veya bir ağ güvenlik grubu için bir ağ arabirimi uygulayabilirsiniz. Sıfır veya bir ağ güvenlik grubu, ağ arabiriminin atandığı alt ağa da uygulanabilir. Bir ağ arabirimi ve ağ arabirimi atandığı alt ağ için ağ güvenlik grubu uygulandığında, bazen beklenmeyen sonuçlar oluşur. Ağ arabirimleri ve alt ağları ile uygulanan ağ güvenlik grupları gidermek için okuma [ağ güvenlik grupları sorun giderme](virtual-network-nsg-troubleshoot-portal.md#nsg) makalesi.|
    |Abonelik|Evet|Azure birini [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Bir ağ arabirimi ve kendisine bağlanan sanal ağ ekleme sanal makine aynı abonelikte olması gerekir.|
    |Özel IP adresi (IPv6)|Hayır| Bu onay kutusunu seçerseniz, bir IPv6 adresi bir ağ arabirimine atanmış IPv4 adresi ek olarak ağ arabiriminin atanır. Bkz: [IPv6](#IPv6) hakkında önemli bilgiler için kullanım IPv6 ağ arabirimi ile bu makalenin. Bir IPv6 adresi için bir atama yöntemi seçemezsiniz. Bir IPv6 adresi atamak seçerseniz, dinamik yöntemiyle atanır.
    |IPv6 adı (yalnızca zaman görünür **özel IP adresi (IPv6)** onay kutusu işaretli) |Evet, varsa **özel IP adresi (IPv6)** onay kutusu işaretli.| Bu ad, bir ikincil ağ arabirimi IP yapılandırmasına atanır. IP yapılandırmaları hakkında daha fazla bilgi [görünüm ağ arabirimi ayarları](#view-network-interface-settings) bu makalenin.|
    |Kaynak grubu|Evet|Var olan seçin [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) veya bir tane oluşturun. Bir ağ arabirimi, eklediğiniz sanal makine daha aynı veya farklı kaynak grubunda var olabilir veya sanal ağ, ona bağlanın.|
    |Konum|Evet|Sanal makine bir ağ arabirimi ekleyin ve ona bağlandığınız sanal ağ aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions), de denilen bir bölge.|

Portal portal genel bir IP adresi oluşturun ve Portalı'nı kullanarak bir sanal makine oluşturduğunuzda, bir ağ arabirimine atayın olsa da, oluşturduğunuzda, ağ arabirimi genel bir IP adresi atamak için seçeneği sağlamaz. Oluşturduktan sonra ağ arabirimi genel bir IP adresi eklemeyi öğrenmek için okuma [yönetmek IP adresleri](virtual-network-network-interface-addresses.md) makalesi. Bir ortak IP adresiyle bir ağ arabirimi oluşturmak istiyorsanız, ağ arabiriminin oluşturmak için CLI veya PowerShell kullanmanız gerekir.

>[!Note]
> Azure yalnızca ağ arabirimi bir sanal makineye bağlı ve sanal makine ilk kez başlatıldığında Ağ arabirimi için bir MAC adresi atar. Azure ağ arabirimine atar MAC adresi belirtemezsiniz. Ağ arabirimi silinmiş veya birincil ağ arabirimi birincil IP yapılandırmasının atanan özel IP adresi değiştirilmiş kadar MAC adresi ağ arabirimine atanmış olarak kalır. IP adresleri ve IP yapılandırmaları hakkında daha fazla bilgi için okuma [yönetmek IP adresleri](virtual-network-network-interface-addresses.md) makalesi.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC oluşturun](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[AzureRmNetworkInterface yeni](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Ağ arabirimi ayarlarını görüntüleme

Görüntüleyin ve oluşturulduktan sonra bir ağ arabirimi için çoğu ayarlarını değiştirin.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey için ayarlarını görüntülemek veya değiştirmek istediğiniz ağ arabirimi'ı tıklatın.
4. Aşağıdaki ayarlar, görüntülenen dikey penceresinde, seçtiğiniz ağ arabirimi için listelenmiştir:
    - **Genel Bakış:** , sanal ağ/ağ arabirimi atanması alt ağ ve ağ arabirimi eklendiği (bağlı olduğu, sanal makine için atanan IP adresleri gibi ağ arabirimi hakkında bilgi sağlar bir tane). Aşağıdaki resimde adlı ağ arabirimi için genel ayarları gösterilmiştir **mywebserver256**: ![ağ arabirimi genel bakış](./media/virtual-network-network-interface/nic-overview.png) farklı bir kaynak grubu için bir ağ arabirimi taşıyabilirsiniz veya Abonelik tıklayarak (**değiştirme**) yanındaki **kaynak grubu** veya **abonelik adı**. Ağ arabirimi taşırsanız, ağ arabiriminin onunla ilişkili tüm kaynakları taşımanız gerekir. Örneğin, ağ arabirimi bir sanal makineye bağlıysa, aynı zamanda sanal makine ve diğer ilgili sanal makine kaynakları taşımalısınız. Bir ağ arabirimi taşımak için okuma [yeni kaynak grubu veya abonelik için kaynak taşıma](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) makalesi. Makaleyi önkoşulları ve Azure portalı, PowerShell ve Azure CLI kullanarak kaynakları taşıma listeler.
    - **IP yapılandırması:** IP yapılandırmaları için atanan ortak ve özel IPv4 ve IPv6 adreslerini burada listelenir. Bir IPv6 adresi bir IP yapılandırmasına atanırsa, adres görüntülenmez. IP yapılandırmaları hakkında daha fazla bilgi edinin ve ekleme ve kaldırma IP adresleri [yapılandırma IP adresleri için bir Azure ağı arabirimi](virtual-network-network-interface-addresses.md) makalesi. IP iletimi ve alt ağ ataması, bu bölümde de yapılandırılır. Bu ayarlar hakkında daha fazla bilgi için okuma [etkinleştirmek veya IP iletimini devre dışı](#enable-or-disable-ip-forwarding) ve [değiştirme alt ağ ataması](#change-subnet-assignment) bu makalenin bölümler.
    - **DNS sunucuları:** bir ağ arabirimi Azure DHCP sunucuları tarafından atanan hangi DNS sunucusunun belirtebilirsiniz. Ağ arabirimi ağ arabirimi için atanan sanal ağ ayarlarını devral veya atandığı sanal ağ ayarını geçersiz kılar özel bir ayar vardır. Görüntülenenleri değiştirmek için bölümündeki adımları tamamlamanız [değişiklik DNS sunucuları](#change-dns-servers) bu makalenin.
    - **Ağ güvenlik grubu (NSG):** , NSG (varsa) bir ağ arabirimine ilişkili olan görüntüler. Bir NSG'yi ağ arabirimi için ağ trafiğini filtrelemek için gelen ve giden kurallarını içerir. Bir NSG'yi bir ağ arabirimine ilişkiliyse, ilişkili NSG adı görüntülenir. Görüntülenenleri değiştirmek için bölümündeki adımları tamamlamanız [ağ güvenlik grubu ilişkileri yönetme](virtual-network-manage-nsg-arm-portal.md#manage-associations) makalesi.
    - **Özellikler:** görüntüler anahtar ayarları var, MAC adresini (ağ arabirimi bir sanal makineye bağlı değil, boş) ve abonelik dahil olmak üzere ağ arabiriminin hakkında.
    - **Etkin güvenlik kuralları:** güvenlik kuralları, ağ arabiriminin çalışan bir sanal makineye bağlı ve bir NSG ağ arabirimi, atanan için alt ağ veya her ikisi de ilişkili ise listelenir. Görüntülenenleri hakkında daha fazla bilgi için okuma [ağ güvenlik grupları sorun giderme](virtual-network-nsg-troubleshoot-portal.md#nsg) makalesi. Nsg'ler hakkında daha fazla bilgi için okuma [ağ güvenlik grupları](virtual-networks-nsg.md) makalesi.
    - **Etkin yollar:** ağ arabirimi çalışan bir sanal makineye bağlıysa yolları listelenir. Yolları, Azure varsayılan yollar, tüm kullanıcı tanımlı yolları (UDR) ve ağ arabirimi atandığı alt ağ için bulunabilecek tüm BGP yollarını birleşimidir. Görüntülenenleri hakkında daha fazla bilgi için okuma [sorun giderme yolları](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) makalesi. Azure varsayılan ve Udr'ler hakkında daha fazla bilgi için okuma [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md) makalesi.
    - **Ortak Azure Resource Manager ayarları:** ortak Azure Resource Manager ayarları hakkında daha fazla bilgi için okuma [etkinlik günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [etiketleri](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), ve [Otomasyon betiğini](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) makaleleri.

**Komutları**

Bir IPv6 adresi için bir ağ arabirimi atanırsa, PowerShell çıkış adresi atanmış, ancak atanan adresi döndürmez olgu döndürür. Benzer şekilde, CLI adresi atanmış, ancak döndürür olgu döndürür *null* çıktısını adresi için de.

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC listesi](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) ; aboneliğindeki ağ arabirimlerini görüntülemek için [az ağ NIC Göster](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) bir ağ arabiriminin ayarlarını görüntülemek için|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) bir ağ arabirimi için abonelik veya Görünümü Ayarları'nda ağ arabirimlerini görüntülemek için|

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

DNS sunucusu, ağ arabirimi sanal makine işletim sistemi içinde Azure DHCP sunucusu tarafından atanır. Atanan DNS sunucusu, bir ağ arabirimi için DNS sunucusu ayarını ne olursa olsun ' dir. Bir ağ arabirimi için ad çözümleme ayarları hakkında daha fazla bilgi için bkz: [ad çözümlemesi için sanal makineler](virtual-networks-name-resolution-for-vms-and-role-instances.md). Ağ arabirimi sanal ağdan ayarları devralır veya sanal ağ ayarını geçersiz kılmak kendi benzersiz ayarları kullanın.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey için ayarlarını görüntülemek veya değiştirmek istediğiniz ağ arabirimi'ı tıklatın.
4. Seçtiğiniz ağ arabirimi için dikey penceresinde tıklayın **DNS sunucuları** altında **ayarları**.
5. Şunlardan birini tıklatın:
    - **Sanal ağ (varsayılan) devralmak**: ağ arabirimi için atanan sanal ağ için tanımlanan DNS sunucusu ayarını devralmak için bu seçeneği seçin. Sanal ağ düzeyinde tanımlanan özel bir DNS sunucusu veya Azure tarafından sağlanan DNS sunucusu. Azure tarafından sağlanan DNS sunucusu ana bilgisayar adları aynı sanal ağa atanan kaynaklar için çözümleyebilir. FQDN farklı sanal ağlar için atanmış kaynaklar için çözümlemek için kullanılması gerekir.
    - **Özel**: birden çok sanal ağlar arasında adları çözümlemek için kendi DNS sunucusunu yapılandırabilirsiniz. Bir DNS sunucusu olarak kullanmak istediğiniz sunucunun IP adresini girin. Belirttiğiniz DNS sunucusu adresi, yalnızca bu ağ arabirimi atanır ve ağ arabirimi için atanan sanal ağı için tüm DNS ayarını geçersiz kılar.
6. **Kaydet** düğmesine tıklayın.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Etkinleştirmek veya IP iletimini devre dışı bırakma

IP iletimi bir ağ arabiriminin bağlı olduğu sanal makine sağlar:
- Ağ trafiği için herhangi bir ağ arabirimine atanmış IP yapılandırmalarının atanan IP adreslerinden biri hedeflemeyen alırsınız.
- Bir ağ arabiriminin IP yapılandırmaları birine atanan olandan farklı bir kaynak IP adresine sahip ağ trafiğinin gönderebilir.

Ayarı, sanal makine iletmek için gereken trafiği alan sanal makineye bağlı her ağ arabirimi için etkinleştirilmelidir. Bir sanal makine trafiği birden çok ağ arabirimi bulunur veya tek bir ağ arabirimine ekli olmadığını iletebilirsiniz. IP iletimi Azure bir ayar olmakla birlikte, sanal makine trafiğin güvenlik duvarı, WAN iyileştirmesi ve Yük Dengeleme uygulamalar gibi iletebilir bir uygulama da çalıştırmanız gerekir. Bir sanal makine ağ uygulamaları çalıştırırken, sanal makine genellikle ağ sanal gereç olarak adlandırılır. Ağ, sanal uygulamaları dağıtmak hazır bir listesini görüntüleyebileceğiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). IP iletimi genellikle kullanıcı tanımlı yollar ile kullanılır. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için, [Kullanıcı tanımlı yollar](virtual-networks-udr-overview.md) makalesini okuyun.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey penceresinde, etkinleştirmek veya devre dışı bırakmak için iletme IP istediğiniz ağ arabirimi'ı tıklatın.
4. Seçtiğiniz ağ arabirimi için dikey penceresinde tıklayın **IP yapılandırmaları** içinde **ayarları** bölümü.
5. Tıklatın **etkin** veya **devre dışı** (varsayılan ayar) ayarını değiştirmek için.
6. **Kaydet** düğmesine tıklayın.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Alt ağ ataması değiştirme

Alt ağ, ancak bir ağ arabirimi atanan sanal ağda değil, değiştirebilirsiniz.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. İçinde **ağ arabirimleri** görünür, dikey için ayarlarını görüntülemek veya değiştirmek istediğiniz ağ arabirimi'ı tıklatın.
4. Tıklatın **IP yapılandırmaları** altında **ayarları** dikey penceresinde, seçtiğiniz ağ arabirimi için. Tüm özel IP adresleri IP yapılandırmaları için listelenen varsa **(statik)** bunları yanında, IP adres ataması yöntemi için dinamik adımları tamamlayarak değiştirmeniz gerekir. Ağ arabirimi için alt ağ ataması değiştirmek için tüm özel IP adresleri dinamik atama yöntemiyle atanması gerekir. Dinamik yöntemiyle atanmış adresleri, 5. adıma geçin. Statik atama yöntemiyle herhangi bir IPv4 adresi atanmışsa atama yöntemi dinamik olarak değiştirmek için aşağıdaki adımları tamamlayın:
    - IPv4 adres atama yöntemi için IP yapılandırmaları listesinden değiştirmek istediğiniz IP Yapılandırması'nı tıklatın.
    - Görüntülenen dikey penceresinde IP yapılandırması için tıklayın **dinamik** için **atama** yöntemi. Statik atama yöntemi ile bir IPv6 adresi atanamıyor.
    - **Kaydet** düğmesine tıklayın.
5. İçin ağ arabiriminden bağlanmak istediğiniz alt ağ seçin **alt** aşağı açılan liste.
6. **Kaydet** düğmesine tıklayın. Yeni dinamik adresleri yeni alt ağ için alt ağ adres aralığından atanır. İsterseniz yeni bir alt ağ ağ arabiriminin atadıktan sonra statik bir IPv4 adresi yeni alt ağ adres aralığından atayabilirsiniz. Daha fazla hakkında ekleme, değiştirme ve bir ağ arabirimi için IP adresleri kaldırma öğrenmek için [yönetmek IP adresleri](virtual-network-network-interface-addresses.md) makalesi.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC IP yapılandırmasını güncelleştir](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Bir ağ arabirimi Sil

Bir sanal makineye bağlı olmayan sürece, bir ağ arabirimi silebilirsiniz. Bir sanal makineye bağlıysa, gerekir ilk sanal makine durduruldu (serbest bırakıldığında) durumda yerleştirin ve ardından ağ arabirimi silmeden önce sanal makineden ağ arabirimini ayır. Bir sanal makineden ağ arabirimini ayırmak için adımları tamamlamanız [bir sanal makineden ağ arabirimini Ayır](virtual-network-network-interface-vm.md#vm-remove-nic) bölümünü [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md) makalesi. Bir sanal makine silme bağlı tüm ağ arabirimleri ayırır, ancak ağ arabirimleri silmez.

1. Oturum [Azure portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Okuma [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için makalenin.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** görünür arama sonuçlarında tıklatın.
3. Sağ tıklayın, önce silmek istediğiniz ağ arabirimi **silmek**.
4. Tıklatın **Evet** ağ arabiriminin silme işlemini onaylamak için.

Bir ağ arabirimi sildiğinizde, kendisine atanmış MAC veya IP adresi yayınlanır.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC Sil](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Sonraki adımlar
Birden çok ağ ile bir sanal makine oluşturmak için arabirimleri veya IP adresleri, aşağıdaki makaleler okuyun:

**Komutları**

|Görev|Aracı|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresleriyle tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel bir IPv6 adresi (arkasında bir Azure yük dengeleyici) ile tek bir NIC VM oluşturma|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
