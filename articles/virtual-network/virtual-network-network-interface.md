---
title: Oluşturma, değiştirme veya bir Azure ağ arabirimini Sil
titlesuffix: Azure Virtual Network
description: Bilgi bir ağ arabiriminin ne olduğu ve nasıl oluşturulacağını, için ayarları değiştirin ve bir akışınızı silin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: kumud
ms.openlocfilehash: f25840c21ec64ca8d8e9e17eb39637cff7524c76
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755249"
---
# <a name="create-change-or-delete-a-network-interface"></a>Oluşturma, değiştirme veya bir ağ arabirimini Sil

Ağ arabirimini Sil oluşturmak ve ayarlarını değiştirme hakkında bilgi edinin. Bir ağ arabirimi, internet, Azure ve şirket içi kaynakları ile iletişim kurmak için bir Azure sanal makine sağlar. Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Bunun yerine, ağ arabirimleri ile özel ayarları oluşturmak ve oluşturduğunuz bir veya daha fazla ağ arabirimi bir sanal makineye ekleyin. seçebilirsiniz. Var olan bir ağ arabirimi için varsayılan ağ arabirimi ayarları değiştirmek isteyebilirsiniz. Bu makalede, özel ayarlara sahip bir ağ arabirimi oluşturun, ağ filtresi (ağ güvenlik grubu) atama, alt ağ ataması, DNS sunucusu ayarlarını ve IP iletme gibi mevcut ayarları değiştirme ve ağ arabirimini Sil açıklanmaktadır.

Eklemek, değiştirmek veya bir ağ arabirimi için IP adreslerini kaldırın, bkz. [yönetme IP adresleri](virtual-network-network-interface-addresses.md). Ağ arabirimleri veya ağ arabirimleri, sanal makinelerden kaldırmak için bkz için eklemeniz gerekiyorsa [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.com ve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici Azure PowerShell modülü sürüm 1.0.0 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI 2.0.28 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="create-a-network-interface"></a>Ağ arabirimini oluşturun

Azure portalını kullanarak bir sanal makine oluştururken, portal sizin için varsayılan ayarlarla bir ağ arabirimi oluşturur. Bunun yerine, tüm ağ arabirimi ayarları belirtmeniz gerekir, özel ayarlara sahip bir ağ arabirimi oluşturun ve ağ arabirimi (PowerShell veya Azure CLI kullanarak) sanal makine oluştururken bir sanal makineye ekleyin. Ayrıca, bir ağ arabirimi oluşturabilir ve varolan bir sanal makineye (PowerShell veya Azure CLI kullanarak) ekleyin. Var olan bir ağ arabirimi ile bir sanal makine oluşturmak için veya eklemek veya mevcut sanal makinelerden ağ arabirimleri Kaldır hakkında bilgi edinmek için bkz: [ekleme veya kaldırma ağ arabirimleri](virtual-network-network-interface-vm.md). Bir ağ arabirimi oluşturmadan önce varolan olmalıdır [sanal ağ](manage-virtual-network.md) aynı konum ve abonelikte bir ağ arabirimi oluşturun.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Seçin **+ Ekle** altında **ağ arabirimleri**.
3. Girin veya aşağıdaki ayarları için değerleri seçip **Oluştur**:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Ad|Evet|Adı kaynak grubu içinde benzersiz olmalıdır. Zamanla, büyük olasılıkla Azure aboneliğinizde birden çok ağ arabirimi gerekir. Birden çok ağ arabirimi daha kolay hale bir adlandırma kuralı oluşturulurken öneriler için bkz [adlandırma kuralları](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Ağ arabirimi oluşturulduktan sonra ad değiştirilemez.|
    |Sanal ağ|Evet|Sanal ağ için ağ arabirimi seçin. Yalnızca aynı abonelik ve konumdaki ağ arabirimi olarak var olan bir sanal ağ bir ağ arabirimine atayabilirsiniz. Bir ağ arabirimi oluşturulduktan sonra atanan sanal ağ değiştirilemez. Sanal makine için ağ arabirimi eklemek de aynı konum ve abonelikte ağ arabirimi mevcut olması gerekir.|
    |Alt ağ|Evet|Seçtiğiniz sanal ağ içindeki bir alt ağ seçin. Alt ağ oluşturulduktan sonra ağ arabirimine atanan değiştirebilirsiniz.|
    |Özel IP adresi ataması|Evet| Bu ayarda, IPv4 adresi için atama yöntemi seçersiniz. Aşağıdaki atama yöntemler arasından seçim yapın: **Dinamik:** Bu seçeneğin belirlenmesi, Azure, seçilen alt ağın adres alanından sonraki kullanılabilir adresi otomatik olarak atar. **Statik:** Bu seçeneğin belirlenmesi, kullanılabilir ağdan bir IP adresi seçilen alt ağın adres alanı içinde el ile atamanız gerekir. Statik ve dinamik adresler, bunları değiştirebilir veya ağ arabirimi silinmiş kadar değiştirmeyin. Ağ arabirimi oluşturulduktan sonra atama yöntemini değiştirebilirsiniz. Azure DHCP sunucusu, sanal makinenin işletim sistemi içinde ağ arabirimine bu adresi atar.|
    |Ağ güvenlik grubu|Hayır| İzin kümesine **hiçbiri**, varolan seçin [ağ güvenlik grubu](security-overview.md), veya [ağ güvenlik grubu oluşturma](tutorial-filter-network-traffic.md). Ağ güvenlik grupları filtresi ağ trafiği bir ağ arabirimi içine ve dışına sağlar. Sıfır veya bir ağ güvenlik grubu için bir ağ arabirimi uygulayabilirsiniz. Sıfır veya bir ağ güvenlik grubu, ağ arabirimine atanmış bir alt ağ için de uygulanabilir. Bir ağ arabirimi ve alt ağ arabirimine atanmış bir ağ güvenlik grubu uygulanır, bazen beklenmedik sonuçlar ortaya çıkar. Ağ arabirimine ve alt ağa uygulanan ağ güvenlik gruplarında sorun giderme için bkz: [ağ güvenlik gruplarında sorun giderme](diagnose-network-traffic-filter-problem.md).|
    |Abonelik|Evet|Azure birini [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Sanal makine, bir ağ arabirimine ve buna bağlanmak sanal ağ ekleyebilir, aynı abonelikte bulunması gerekir.|
    |Özel IP adresi (IPv6)|Hayır| Bu onay kutusunu seçerseniz, ağ arabirimine atanmış IPv4 adresini ek olarak ağ arabiriminin IPv6 adresi atanır. Bu makalede hakkında önemli bilgiler için kullanım, IPv6 ağ arabirimine sahip IPv6 bölümüne bakın. IPv6 adresi için bir atama yöntemi seçemezsiniz. Bir IPv6 adresi atamak isterseniz, dinamik yöntem ile atanır.
    |IPv6 adı (yalnızca **özel IP adresi (IPv6)** onay kutusu işaretli) |Evet, varsa **özel IP adresi (IPv6)** onay kutusu işaretli.| Bu ad, bir ikincil ağ arabirimi IP yapılandırması için atanır. IP yapılandırması hakkında daha fazla bilgi için bkz: [ağ arabirimi ayarları görüntüle](#view-network-interface-settings).|
    |Kaynak grubu|Evet|Mevcut bir seçin [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) veya bir tane oluşturabilirsiniz. Bir ağ arabirimi, ona eklediğiniz sanal makineden aynı veya farklı bir kaynak grubunda mevcut olabilir veya sanal ağ, ona bağlanın.|
    |Location|Evet|Sanal makineyi bir ağ arabirimine ekleyebilir ve kendisine bağlandığınız sanal ağ aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions)de denilen bir bölge.|

Portal, portalı ve genel IP adresi oluşturma portalı kullanarak bir sanal makine oluşturduğunuzda, bir ağ arabirimine atamak, oluşturduğunuz ağ arabirimine bir genel IP adresi atamak için seçeneği sağlamaz. Oluşturduktan sonra ağ arabirimine bir genel IP adresi ekleme konusunda bilgi edinmek için [yönetme IP adresleri](virtual-network-network-interface-addresses.md). Bir genel IP adresiyle bir ağ arabirimi oluşturmak istiyorsanız, ağ arabirimi oluşturmak için CLI veya PowerShell kullanmanız gerekir.

Portal bir ağ arabirimi oluşturulurken ağ arabiriminin uygulama güvenlik gruplarına atama seçeneğini sağlamaz, ancak Azure CLI ve PowerShell. Ağ arabirimi bir sanal makineye bağlı olduğu sürece, var olan bir ağ arabirimi ancak, portalı kullanarak bir uygulama güvenlik grubuna atayabilirsiniz. Bir ağ arabirimi bir uygulama güvenlik grubuna atama hakkında bilgi edinmek için [Ekle veya Kaldır uygulama güvenlik grupları '](#add-to-or-remove-from-application-security-groups).

>[!Note]
> Azure yalnızca ağ arabirimi bir sanal makineye bağlı ve sanal makine ilk kez başlatıldığında Ağ arabirimi ile bir MAC adresi atar. Azure ağ arabirimi ile atar MAC adresini belirtemezsiniz. Ağ arabirimi silinmiş veya birincil ağ arabiriminin birincil IP yapılandırması için atanmış özel IP adresi değiştirildiğinde kadar MAC adresinin ağ arabirimine atanmış olarak kalır. IP adresleri ve IP yapılandırmaları hakkında daha fazla bilgi için bkz: [yönetme IP adresleri](virtual-network-network-interface-addresses.md)

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic)|
|PowerShell|[Yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)|

## <a name="view-network-interface-settings"></a>Görünümü ağ arabirimi ayarları

Görüntüleyebilir ve oluşturulduktan sonra bir ağ arabirimi için çoğu ayarları değiştirin. Portal, DNS son eki veya uygulama güvenlik grubu üyeliğine ağ arabirimi için görüntülemez. PowerShell veya Azure CLI kullanabilirsiniz [komutları](#view-settings-commands) DNS soneki ve uygulama güvenlik grubu üyeliğine görüntülemek için.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Görüntülemek veya listeden ayarlarını değiştirmek istediğiniz ağ arabirimi seçin.
3. Aşağıdaki öğeler, seçtiğiniz ağ arabirimi için listelenmiştir:
   - **Genel Bakış:** Ağ arabirimine atanan sanal ağ/alt ve ağ arabiriminin bağlı olduğu (birine bağlı olduğu varsa) sanal makineye atanan IP adresleri gibi ağ arabirimi hakkında bilgi sağlar. Adlı bir ağ arabirimi için genel bakış ayarları aşağıdaki resimde gösterilmiştir **mywebserver256**: ![Ağ arabirimine genel bakış](./media/virtual-network-network-interface/nic-overview.png)

     Seçerek farklı kaynak grubuna veya aboneliğe bir ağ arabirimi taşıyabilirsiniz (**değiştirme**) yanındaki **kaynak grubu** veya **abonelik adı**. Ağ arabirimi taşırsanız, bu ağ arabirimiyle ilişkili tüm kaynakları taşımanız gerekir. Örneğin, ağ arabirimi bir sanal makineye bağlıysa, aynı zamanda sanal makine ve sanal makine ile ilgili diğer kaynaklar taşımalısınız. Bir ağ arabirimi taşımak için bkz [bir yeni kaynak grubuna veya aboneliğe taşıma kaynak](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). Makalede Önkoşullar ve Azure portalı, PowerShell ve Azure CLI kullanarak kaynaklara nasıl taşırım listelenmektedir.
   - **IP yapılandırması:** IP yapılandırması için atanan ortak ve özel IPv4 ve IPv6 adresleri burada listelenir. Bir IP yapılandırması için bir IPv6 adresi atanmışsa, adresin görüntülenmez. IP yapılandırmaları ve eklemek ve IP adresleri kaldırma hakkında daha fazla bilgi için bkz: [yapılandırma IP adresleri için bir Azure ağı arabirimi](virtual-network-network-interface-addresses.md). IP iletimi ve alt ağ ataması, bu bölümde ayrıca yapılandırılır. Bu ayarlar hakkında daha fazla bilgi için bkz: [etkinleştirmek veya devre dışı IP iletme](#enable-or-disable-ip-forwarding) ve [değiştirmek alt ağ ataması](#change-subnet-assignment).
   - **DNS sunucuları:** Bir ağ arabirimi Azure DHCP sunucuları tarafından atanan hangi DNS sunucusunun belirtebilirsiniz. Ağ arabirimi ağ arabirimine atanan sanal ağdan ayarı devralan veya atanan sanal ağ için ayarını geçersiz kılar, özel bir ayara sahip. Nelerin görüntüleneceğini değiştirmek için bkz: [değişiklik DNS sunucuları](#change-dns-servers).
   - **Ağ güvenlik grubu (NSG):** NSG (varsa) ağ arabirimi ile ilişkili olan görüntüler. Bir NSG ağ arabirimi için ağ trafiğini filtreleme için gelen ve giden kurallar içerir. Bir NSG ağ arabirimi ile ilişkili ise, ilişkili NSG adı görüntülenir. Nelerin görüntüleneceğini değiştirmek için bkz: [ilişkilendirmek veya bir ağ güvenlik grubu ilişkilendirmesini](#associate-or-dissociate-a-network-security-group).
   - **Özellikler:** İçinde mevcut MAC adresini (ağ arabirimi bir sanal makineye bağlı değil ise boş) ve abonelik dahil olmak üzere ağ arabirimi, anahtar ayarlarını görüntüler.
   - **Geçerli güvenlik kuralları:**  Güvenlik kuralları, ağ arabirimi çalışan bir sanal makineye bağlı ve bir NSG ağ arabirimi, atanan alt ağ veya her ikisi de ilişkili listelenir. Nelerin görüntüleneceğini hakkında daha fazla bilgi için bkz: [geçerli güvenlik kuralları görüntüleyebiliriz](#view-effective-security-rules). Nsg'ler hakkında daha fazla bilgi edinmek için [ağ güvenlik grupları](security-overview.md).
   - **Geçerli rotalar:** Ağ arabirimi çalışan bir sanal makineye bağlı yollar listelenir. Azure varsayılan yolları, tüm kullanıcı tanımlı rotalar ve ağ arabirimine atanmış bir alt ağ için mevcut tüm BGP yolları yollardır. Nelerin görüntüleneceğini hakkında daha fazla bilgi için bkz: [görüntülemek geçerli rotalar](#view-effective-routes). Azure varsayılan yollar ve kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
   - **Yaygın Azure Resource Manager ayarları:**  Yaygın Azure Resource Manager ayarları hakkında daha fazla bilgi edinmek için [etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md), [erişim denetimi (IAM)](../role-based-access-control/overview.md), [etiketleri](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)ve [ Otomasyon betiği](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).

<a name="view-settings-commands"></a>**Komutları**

Bir ağ arabirimi için bir IPv6 adresi atanmışsa, PowerShell çıkış adresi atanır ancak atanan adresi döndürmüyor olgu döndürür. Benzer şekilde, CLI adresi atanır ancak döndürür gerçeği döndürür *null* çıktısını adresi için de.

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC listesi](/cli/azure/network/nic) abonelikte; ağ arabirimlerini görüntülemek için [az ağ nic show](/cli/azure/network/nic) bir ağ arabirimi ayarlarını görüntülemek için|
|PowerShell|[Get-AzNetworkInterface](/powershell/module/az.network/get-aznetworkinterface) abonelik veya Görünüm ayarlarında bir ağ arabirimi için ağ arabirimlerini görüntülemek için|

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

DNS sunucusu, sanal makine işletim sistemi içinde ağ arabirimi Azure DHCP sunucusu tarafından atanır. Atanan DNS sunucusu, bir ağ arabirimi için DNS sunucusu ayarını ne olursa olsun ' dir. Bir ağ arabirimi adı çözümleme ayarları hakkında daha fazla bilgi için bkz: [ad çözümlemesi için sanal makineler](virtual-networks-name-resolution-for-vms-and-role-instances.md). Ağ arabirimi ayarları sanal ağdan devral veya ayarınızı sanal ağın kendi benzersiz ayarları kullanın.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Listeden bir DNS sunucusu için değiştirmek istediğiniz ağ arabirimi seçin.
3. Seçin **DNS sunucuları** altında **ayarları**.
4. Şunlardan birini seçin:
   - **Sanal ağdan devral**: Ağ arabirimine atanan sanal ağ için tanımlanmış DNS sunucusu ayarını devralmak için bu seçeneği belirleyin. Sanal ağ düzeyinde özel bir DNS sunucusu ya da Azure tarafından sağlanan DNS sunucusu tanımlanır. Azure tarafından sağlanan DNS sunucusunun ana bilgisayar adları aynı sanal ağa atanan kaynaklar için çözebilirsiniz. FQDN, farklı sanal ağlar için atanan kaynaklar için çözmek için kullanılmalıdır.
   - **Özel**: Birden çok sanal ağlarda adlarını çözümlemek için kendi DNS sunucunuzu yapılandırabilirsiniz. Bir DNS sunucusu olarak kullanmak istediğiniz sunucusunun IP adresini girin. Belirttiğiniz DNS sunucusu adresi, yalnızca bu ağ arabirimi atanır ve ağ arabirimine atanan sanal ağ için herhangi bir DNS ayarını geçersiz kılar.
     >[!Note]
     >Sanal Makineyi bir kullanılabilirlik kümesinin bir parçası olan bir NIC kullanıyorsa, her kullanılabilirlik kümesinin parçası olan tüm NIC'ler Vm'lerden biri için belirtilen tüm DNS sunucularına devralınır.
5. **Kaydet**’i seçin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>Etkinleştirmek veya devre dışı IP iletme

Bir ağ arabiriminin bağlı olduğu sanal makine IP iletimini etkinleştirir:
- Ağ trafiği için ağ arabirimine atanan IP yapılandırmaların herhangi birinde atanmış IP adreslerinden birini hedeflemeyen alırsınız.
- Atanmış bir ağ arabiriminin IP yapılandırmalarından birini olandan farklı bir kaynak IP adresine sahip ağ trafiğini göndermek.

Ayar, sanal makine iletmek için gereken trafiği alan sanal makineye bağlı her ağ arabirimi için etkinleştirilmesi gerekir. Bir sanal makine trafiği birden çok ağ arabirimine sahip ya da tek bir ağ arabirimine bağlı iletebilir. IP iletimi bir Azure ayarı olsa da, sanal makine güvenlik duvarı, WAN iyileştirmesi ve uygulamaları yük dengelemeyi gibi trafiğini iletebilmesi için bir uygulama da çalıştırmanız gerekir. Bir sanal makine ağ uygulamaları çalıştırırken, sanal makine genellikle bir ağ sanal Gereci adlandırılır. Ağ sanal gereçlerini dağıtma hazır listesini görüntüleyebileceğiniz [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). IP iletimi genellikle kullanıcı tanımlı yollar ile kullanılır. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md).

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Etkinleştirme veya devre dışı için NIC'de IP istediğiniz ağ arabirimi seçin.
3. Seçin **IP yapılandırmaları** içinde **ayarları** bölümü.
4. Seçin **etkin** veya **devre dışı bırakılmış** (varsayılan ayar) ayarını değiştirmek için.
5. **Kaydet**’i seçin.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="change-subnet-assignment"></a>Alt ağ ataması değiştirme

Alt ağ, ancak bir ağ arabirimine atanan sanal ağda değil değiştirebilirsiniz.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Alt ağ ataması için değiştirmek istediğiniz ağ arabirimi seçin.
3. Seçin **IP yapılandırmaları** altında **ayarları**. Tüm özel IP adresleri her IP yapılandırmaları için listelenen varsa **(statik)** onlarla, IP adresi ataması yöntemi dinamik olarak aşağıdaki adımları tamamlayarak değiştirmeniz gerekir. Ağ arabirimi için alt ağ ataması değiştirmek için tüm özel IP adresleri dinamik atama yöntemiyle atanması gerekir. Adresleri ile dinamik yöntem atanır, 5. adıma geçin. Herhangi bir IPv4 adresi statik atama yöntemi ile atanırsa, atama yöntemini dinamik olarak değiştirmek için aşağıdaki adımları tamamlayın:
   - IPv4 adres ataması yöntemi için IP yapılandırmaları listesinden değiştirmek istediğiniz IP Yapılandırması'nı seçin.
   - Seçin **dinamik** özel IP adresi için **atama** yöntemi. Bir IPv6 adresi statik atama yöntemi ile atanamaz.
   - **Kaydet**’i seçin.
4. Ağ arabirimi için taşımak istediğiniz alt ağ seçin **alt** aşağı açılan listesi.
5. **Kaydet**’i seçin. Yeni dinamik adresler, yeni bir alt ağ için alt ağ adres aralığından atanır. Seçerseniz ağ arabirimine yeni bir alt ağa atadıktan sonra statik bir IPv4 adresi yeni bir alt ağ adres aralığından atayabilirsiniz. Daha fazla ilgili ekleme, değiştirme ve bir ağ arabirimi için IP adreslerini kaldırma bilgi edinmek için [yönetme IP adresleri](virtual-network-network-interface-addresses.md).

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC IP-config update](/cli/azure/network/nic/ip-config)|
|PowerShell|[Set-AzNetworkInterfaceIpConfig](/powershell/module/az.network/set-aznetworkinterfaceipconfig)|

## <a name="add-to-or-remove-from-application-security-groups"></a>Uygulama güvenlik grupları'ndan kaldırın veya ekleyin

Yalnızca bir ağ arabirimine ekleyebilir veya bir ağ arabirimi ağ arabirimi bir sanal makineye bağlıysa, portalı kullanarak bir uygulama güvenlik grubundan kaldırın. Bir ağ arabirimine eklemek için PowerShell veya Azure CLI kullanın veya ağ arabirimi bir sanal makineye veya bağlı olup olmadığını bir ağ arabirimi bir uygulama güvenlik grubundan kaldırın. Daha fazla bilgi edinin [uygulama güvenlik grupları](security-overview.md#application-security-groups) ve nasıl [uygulama güvenlik grubu oluşturma](manage-network-security-group.md).

1. İçinde *kaynakları, hizmetleri ve belgeleri arayın* kutusunda portalının üst kısmında, eklemek veya bir uygulama güvenlik grubu, kaldırmak istediğiniz bir ağ arabirimi bir sanal makine adını yazmaya başlayın. Sanal makinenizin adı arama sonuçlarında görüntülendiğinde seçin.
2. **AYARLAR**'ın altında **Ağ İletişimi**’ni seçin.  Seçin **uygulama güvenlik gruplarını yapılandırma**, ağ arabirimi için eklemek istediğiniz uygulama güvenlik gruplarını seçin veya ağ arabirimi, kaldırmak istediğiniz uygulama güvenlik grupları seçimini kaldırın ve ardından **Kaydet**. Aynı sanal ağda bulunan ağ arabirimleri, aynı uygulama güvenlik grubuna eklenebilir. Uygulama güvenlik grubu, ağ arabirimi ile aynı konumda bulunmalıdır.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC güncelleştirme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="associate-or-dissociate-a-network-security-group"></a>İlişkilendirme veya bir ağ güvenlik grubu ilişkilendirmesini Kaldır

1. Portalın üst kısmındaki arama kutusuna girin *ağ arabirimleri* arama kutusuna. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Ağ arabirimi için ağ güvenlik grubu ile ilişkilendirmek istediğiniz listeyi seçin veya gelen ağ güvenlik grubu ilişkilendirmesini Kaldır.
3. Seçin **ağ güvenlik grubu** altında **ayarları**.
4. **Düzenle**’yi seçin.
5. Seçin **ağ güvenlik grubu** ve ağ arabirimine ilişkilendirin veya seçmek istediğiniz ağ güvenlik grubu seçip **hiçbiri**, bir ağ güvenlik grubu ilişkilendirmesini kaldırmak.
6. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ NIC güncelleştirme](/cli/azure/network/nic#az-network-nic-update)
- PowerShell: [Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)

## <a name="delete-a-network-interface"></a>Ağ arabirimini Sil

Bir sanal makineye bağlı değil sürece, bir ağ arabirimi silebilirsiniz. Bir ağ arabirimi bir sanal makineye bağlıysa, önce sanal makinenin durdurulmuş (serbest bırakıldığında) duruma getirin, ardından gerekir sanal makineden ağ arabirimi ayrılamadı. Bir ağ arabirimi bir sanal makineden ayırmak için adımları tamamlamanız [bir ağ arabirimi bir sanal makineden ayrılamadı](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). Ancak sanal makinesine bağlı yalnızca bir ağ arabirimi olması durumunda bir sanal makinenin ağ arabiriminden ayıramazsınız. Bir sanal makinenin her zaman bağlı en az bir ağ arabirimi olması gerekir. Bir sanal makine siliniyor bağlı tüm ağ arabirimleri ayırır, ancak ağ arabirimleri silmez.

1. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *ağ arabirimleri*. Zaman **ağ arabirimleri** arama sonuçlarında görünmesini, onu seçin.
2. Seçin **...**  işlecin sağ tarafındaki ağ arabiriminin ağ arabirimleri listesinden silmek istiyor.
3. **Sil**’i seçin.
4. Seçin **Evet** ağ arabiriminin silme işlemini onaylamak için.

Bir ağ arabirimi sildiğinizde, kendisine atanan MAC ya da IP adresleri serbest bırakılır.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ NIC Sil](/cli/azure/network/nic)|
|PowerShell|[Remove-AzNetworkInterface](/powershell/module/az.network/remove-aznetworkinterface)|

## <a name="resolve-connectivity-issues"></a>Bağlantı sorunlarını gidermek

İçin veya bir sanal makine, ağ güvenlik grubu güvenlik kuralları ya da bir ağ arabirimi için geçerli rotalar iletişim kuramıyor, soruna neden. Sorunu gidermek için aşağıdaki seçenekleriniz:

### <a name="view-effective-security-rules"></a>Geçerli güvenlik kurallarını görüntüle

Bir sanal makineye bağlı her ağ arabirimi için geçerli güvenlik kuralları bir ağ güvenlik grubunda oluşturulan kuralların bir bileşimidir ve [varsayılan güvenlik kuralları](security-overview.md#default-security-rules). Bir ağ arabirimi için geçerli güvenlik kurallarını anlama neden ya da bir sanal makineden iletişim kuramadı belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı her ağ arabirimi için geçerli kuralları görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, geçerli güvenlik kuralları için görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineler* arama kutusuna. Zaman **sanal makineler** arama sonuçlarında görünmesini, onu seçin ve ardından listeden bir sanal makine seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adını seçin.
4. Seçin **geçerli güvenlik kuralları** altında **destek + sorun giderme**.
5. Doğru kuralları, gerekli gelen ve giden iletişim için mevcut olup olmadığını belirlemek için geçerli güvenlik kuralları listesini gözden geçirin. Listede göreceğiniz hakkında daha fazla bilgi [ağ güvenlik grubu genel bakış](security-overview.md).

IP akışı doğrulama özelliği Azure Ağ İzleyicisi, ayrıca güvenlik kuralları bir sanal makineye bir uç nokta arasındaki iletişimi engelliyor belirlemenize yardımcı olabilir. Daha fazla bilgi için bkz. [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Komutları**

- Azure CLI: [az NIC liste-etkin-nsg'yi ağ](/cli/azure/network/nic#az-network-nic-list-effective-nsg)
- PowerShell: [Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup)

### <a name="view-effective-routes"></a>Geçerli yollar bölümünü inceleyin

Bir sanal makineye bağlı ağ arabirimleri için geçerli rotalar varsayılan yollar birleşimi, oluşturduğunuz tüm rotalar ve ağlardan şirket içi BGP üzerinden bir Azure sanal ağ geçidinden yayılan her yol ' dir. Bir ağ arabirimi için geçerli rotalar anlama neden ya da bir sanal makineden iletişim kuramadı belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı her ağ arabirimi için geçerli rotalar görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, geçerli güvenlik kuralları için görüntülemek istediğiniz bir sanal makine adını girin. Bir sanal makinenin adını bilmiyorsanız, girin *sanal makineler* arama kutusuna. Zaman **sanal makineler** arama sonuçlarında görünmesini, onu seçin ve ardından listeden bir sanal makine seçin.
2. Seçin **ağ** altında **ayarları**.
3. Bir ağ arabirimi adını seçin.
4. Seçin **geçerli rotalar** altında **destek + sorun giderme**.
5. Doğru yol gerekli gelen ve giden iletişim için mevcut olup olmadığını belirlemek için geçerli rotalar listesini gözden geçirin. Listede göreceğiniz hakkında daha fazla bilgi [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

Azure Ağ İzleyicisi'nin sonraki atlama özelliği de yollar bir sanal makineye bir uç nokta arasındaki iletişimi engelliyor belirlemenize yardımcı olabilir. Daha fazla bilgi için bkz. [sonraki atlama](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Komutları**

- Azure CLI: [az network nic show-etkin-yönlendirme-tablosunu](/cli/azure/network/nic#az-network-nic-show-effective-route-table)
- PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="permissions"></a>İzinler

Ağ arabirimleri üzerinde görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) iznine atanmış bir rol aşağıdaki tabloda listelenen:

| Eylem                                                                     | Ad                                                      |
| ---------                                                                  | -------------                                             |
| Microsoft.Network/networkInterfaces/read                                   | Ağ arabirimini Al                                     |
| Microsoft.Network/networkInterfaces/write                                  | Ağ arabirimi güncelle                        |
| Microsoft.Network/networkInterfaces/join/action                            | Bir sanal makineye bir ağ arabirimi ekleyin           |
| Microsoft.Network/networkInterfaces/delete                                 | Ağ arabirimini Sil                                  |
| Microsoft.Network/networkInterfaces/joinViaPrivateIp/action                | Bir ağ arabirimi bir Hi aracılığıyla bir kaynak alanına...     |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action             | Ağ arabirimi etkin yönlendirme tablosu alma               |
| Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action  | Ağ arabirimi etkin güvenlik grupları Al           |
| Microsoft.Network/networkInterfaces/loadBalancers/read                     | Ağ arabirimi yük Dengeleyiciler Al                      |
| Microsoft.Network/networkInterfaces/serviceAssociations/read               | Hizmet ilişkilendirmesini alın                                   |
| Microsoft.Network/networkInterfaces/serviceAssociations/write              | Bir hizmet ilişkilendirmesini güncelle                    |
| Microsoft.Network/networkInterfaces/serviceAssociations/delete             | Hizmet ilişkilendirmesini silme                                |
| Microsoft.Network/networkInterfaces/serviceAssociations/validate/action    | Hizmet ilişkilendirmesini doğrula                              |
| Microsoft.Network/networkInterfaces/ipconfigurations/read                  | Ağ arabirimi IP Yapılandırması Al                    |

## <a name="next-steps"></a>Sonraki adımlar

- Kullanarak birden çok NIC ile VM oluşturma [Azure CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Tek bir NIC VM ile birden çok IPv4 adresleri kullanarak oluşturma [Azure CLI](virtual-network-multiple-ip-addresses-cli.md) veya [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
- Özel IPv6 adresi (arkasında, Azure Load Balancer) kullanarak bir tek bir NIC VM oluşturma [Azure CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), veya [Azure Resource Manager şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Kullanarak bir ağ arabirimi oluşturun [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonu](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
