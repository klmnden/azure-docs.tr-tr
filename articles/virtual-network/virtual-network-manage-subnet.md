---
title: "Ekleme, değiştirme veya bir Azure sanal ağ alt ağını silmek | Microsoft Docs"
description: "Ekleme, değiştirme veya azure'da bir sanal ağ alt silme öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 413ec2ef4fcc7752b95984a209818eeba535746e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Ekleme, değiştirme veya bir sanal ağ alt ağı silme

Ekleme, değiştirme veya bir sanal ağ alt silme öğrenin. 

Ekleme, değiştirme veya bir alt ağ silme önce sanal ağlar ile bilmiyorsanız, okumanızı öneririz [Azure Virtual Network'e genel bakış](virtual-networks-overview.md) ve [oluşturma, değiştirme veya silme bir sanal ağ](virtual-network-manage-network.md). Bir sanal ağ içinde dağıtılan tüm Azure kaynaklarını bir sanal ağ içindeki bir alt ağ içinde dağıtılır. Genellikle, bir sanal ağ içinde birden fazla alt ağ oluşturulur:
- **Alt ağlar arasında trafiği filtrelemek**. Sanal ağ olan tüm kaynakların (örneğin, sanal makineler) gelen ve giden ağ trafiğini filtrelemek için alt ağları için ağ güvenlik grupları uygulayabilirsiniz. Bir ağ güvenlik grubu oluşturma hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları oluşturma](virtual-networks-create-nsg-arm-pportal.md).
- **Denetim alt ağlar arasında yönlendirme**. Azure varsayılan yolların oluşturur, böylece trafik alt ağlar arasında otomatik olarak yönlendirilir. Kullanıcı tanımlı yollar oluşturarak Azure varsayılan yolların geçersiz kılabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar oluşturmayı](virtual-network-create-udr-arm-ps.md). 

Bu makalede, ekleme, değiştirme ve Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş sanal ağlar için bir alt ağ silme açıklanmaktadır.
 
## <a name="before"></a>Başlamadan önce

Bu makalede açıklanan görevler başlamadan önce aşağıdaki önkoşulları tamamlayın:

- Sanal ağlar ile çalışmaya yeniyseniz alıştırmada gözden geçirmenizi öneririz [ilk Azure sanal ağınızı oluşturmak](quick-create-portal.md). Bu alıştırmada sanal ağlar ile daha öğrenmenize yardımcı olabilir.
- Sanal ağlar için sınırları hakkında bilgi edinmek için gözden [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Azure portalı, Azure komut satırı aracı (Azure CLI) veya Azure PowerShell'i Azure hesabınızı kullanarak oturum açın. Bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede açıklanan görevleri tamamlamak için PowerShell komutlarını kullanmayı planlıyorsanız, öncelikle [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure PowerShell cmdlet'lerinin yüklü en son sürümüne sahip olduğunuzdan emin olun. Örneklerde PowerShell komutları için Yardım almak için girin `get-help <command> -full`.
- Bu makalede açıklanan görevleri tamamlamak için Azure CLI komutları kullanmayı planlıyorsanız, aşağıdakilerden birini yapmanız gerekir:
    - [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure CLI yüklenmiş en son sürümüne sahip olduğunuzdan emin olun.
    - Azure bulut Kabuğu'nu kullanın. CLI ve bağımlılıklarını yüklemek yerine, Azure bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bulut Kabuğu'nu kullanmak için bulut Kabuğu'nu tıklatın (**> _**) Azure portalının simgesine tıklayın. 

  Azure CLI komutlar hakkında Yardım almak için girin `az <command> --help`.

## <a name="create-subnet"></a>Bir alt ağ Ekle

Bir alt ağı eklemek için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında tıklatın **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde, bir alt ağa eklemek istediğiniz sanal ağ'ı tıklatın.
4. Sanal ağ dikey penceresinde **alt ağlar**.
5. Tıklatın **+ alt**.
6. Üzerinde **alt ağ Ekle** dikey penceresinde, aşağıdaki parametreler için değerler girin:
    - **Ad**: ad sanal ağ içinde benzersiz olmalıdır.
    - **Adres aralığı**: aralık sanal ağın adres alanı içinde benzersiz olmalıdır. Aralık, diğer sanal ağ içinde alt ağ adres aralığı ile örtüşemez. Adres alanı, sınıfsız etki alanları arası yönlendirme (CIDR) gösterimini kullanarak belirtilmelidir. Örneğin, bir sanal ağda adres alanı 10.0.0.0/16, 10.0.0.0/24 alt ağ adres alanının tanımlayabilirsiniz. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, bir alt ağ/29 ile tanımlama adres alt ağda üç kullanılabilir IP adresleri aralığı sonuçlanır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Alt ağ, belirli koşullar altında ekledikten sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağ ayarlarını değiştirme](#change-subnet) bu makalede.
    - **Ağ güvenlik grubu**: isteğe bağlı olarak, alt ağ için filtreleme gelen ve giden ağ trafiğini denetlemek için alt ağı içeren mevcut bir ağ güvenlik grubunu ilişkilendirebilirsiniz. Ağ güvenlik grubu aynı abonelikte ve konumda sanal ağ mevcut olmalıdır. Bu aynı zamanda Resource Manager dağıtım modeli kullanılarak oluşturulmuş olması gerekir. Bir ağ güvenlik grubu oluşturma hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](virtual-networks-create-nsg-arm-pportal.md).
    - **Yol tablosu**: isteğe bağlı olarak, ağ trafiği diğer ağlara yönlendirme denetlemek için alt ağ ile varolan bir yol tablosu ilişkilendirebilirsiniz. Yol tablosu, aynı abonelikte ve konumda sanal ağ içinde bulunmalıdır. Bu aynı zamanda Resource Manager dağıtım modeli kullanılarak oluşturulmuş olması gerekir. Yönlendirme tabloları oluşturma hakkında daha fazla bilgi için bkz: [kullanıcı tanımlı yollar](virtual-network-create-udr-arm-ps.md).
    - **Kullanıcıların**: yerleşik roller veya kendi özel roller kullanarak alt ağa erişimi denetleyebilirsiniz. Rol ve alt ağ erişmek için kullanıcı atama hakkında daha fazla bilgi için bkz: [Azure kaynaklarınıza erişimi yönetmek için rol ataması kullanın](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Seçtiğiniz sanal ağ alt ağı eklemek için tıklatın **Tamam**.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ sanal alt oluşturma](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Alt ağ ayarlarını değiştirme

Bir alt ağdaki kaynakları yöneterek, bir alt ağ için ağ güvenlik grupları, yol tablolarını ve kullanıcı erişimini değiştirebilirsiniz. İçinde bu ayarlar hakkında bilgi edinmek için [bir alt ağ Ekle](#create-subnet), 6. adıma bakın. Bir alt ağ adres alanının değiştirmek isterseniz alt ağdaki herhangi bir kaynağa silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt kaynaklarını silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Bir alt ağ adres aralığı değiştirmek için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında tıklatın **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde, bir alt ağ adres aralığı değiştirmek istediğiniz sanal ağ'ı tıklatın.
4. Adres aralığını değiştirmek istediğiniz alt ağ'ı tıklatın.
5. Alt ağ dikey olarak **adres aralığı** kutusuna, yeni adres aralığı girin. Aralığın sanal ağın adres alanı içinde benzersiz olmalıdır. Aralık, diğer sanal ağ içinde alt ağ adres aralığı ile örtüşemez. Adres alanı CIDR gösterimini kullanarak belirtilmelidir. Örneğin, bir sanal ağda adres alanı 10.0.0.0/16, 10.0.0.0/24 alt ağ adres alanının tanımlayabilirsiniz. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, / 29 alt ağı adres aralığı üç kullanılabilir IP adresleri vardır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Belirli koşullar altında alt ağ oluşturulduktan sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağ ayarlarını değiştirme](#change-subnet) bu makalede.
6. **Kaydet**’e tıklayın.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Bir alt ağı silme

Alt ağ içindeki kaynak varsa, bir alt ağ silebilirsiniz. Alt ağ içindeki kaynaklar varsa, alt ağı silmeden önce alt ağdaki kaynakları silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt kaynaklarını silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Bir alt ağını silmek için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında tıklatın **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde, bir alt ağdan silmek istediğiniz sanal ağ'ı tıklatın.
4. Sanal üzerinde dikey penceresinde altında ağ **ayarları**, tıklatın **alt ağlar**.
5. İstediğiniz silmek için alt ağ alt ağlar dikey penceresinde görünen listesinde alt ağlar, sağ **silmek**ve ardından **Evet** alt ağını silmek için.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ vnet Sil](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Sonraki adımlar

Bir alt ağda bir sanal makine oluşturmak için bkz: [bir sanal ağ oluşturma ve alt ağ içindeki VM'ler dağıtma](quick-create-portal.md#create-virtual-machines).
