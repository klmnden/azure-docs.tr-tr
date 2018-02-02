---
title: "Oluşturma, değiştirme veya bir Azure sanal ağı silme | Microsoft Docs"
description: "Oluşturma, değiştirme veya silme Azure sanal ağında öğrenin."
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
ms.openlocfilehash: 0d3f4a83b654315a5ff9344594323c5dcb801e77
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Oluşturma, değiştirme veya bir sanal ağı silme

Oluşturmak ve sanal ağı silmek ve DNS sunucuları ve IP adresi alanlarını, varolan bir sanal ağ gibi ayarlarını değiştirmek öğrenin.

Bir sanal ağ kendi ağ bulutta gösterimidir. Azure bulutunun Azure aboneliğinize adanmış mantıksal ayırma bir sanal ağdır. Oluşturduğunuz her sanal ağ için şunları yapabilirsiniz:
- Atamak için bir adres alanı seçin. Bir adres alanı 10.0.0.0/16 gibi sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanılarak tanımlanmış bir veya daha fazla adres aralıklarını oluşur.
- Azure tarafından sağlanan DNS sunucusu kullanmayı veya kendi DNS sunucusu kullanmayı seçin. Sanal ağa bağlı tüm kaynaklar sanal ağ içindeki adları çözümlemek için bu DNS sunucusuna atanır.
- Sanal ağ alt ağlar, her biri kendi adres aralığında sanal ağın adres alanı içine kesiminde. Oluşturma, değiştirme ve alt ağları silme öğrenmek için bkz: [Ekle, değiştirme veya silme alt ağlar](virtual-network-manage-subnet.md).

Bu makalede, oluşturmak, değiştirmek ve Azure Resource Manager dağıtım modelini kullanarak sanal ağları silme açıklanmaktadır.

## <a name="before"></a>Başlamadan önce

Bu makalede açıklanan görevler başlamadan önce aşağıdaki önkoşulları tamamlayın:

- Sanal ağlar ile çalışmaya yeniyseniz alıştırmada gözden geçirmenizi öneririz [ilk Azure sanal ağınızı oluşturmak](quick-create-portal.md). Bu alıştırmada sanal ağlar ile daha öğrenmenize yardımcı olabilir.
- Sanal ağlar için sınırları hakkında bilgi edinmek için gözden [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Azure portalı, Azure komut satırı aracı (Azure CLI) veya Azure PowerShell'i Azure hesabınızı kullanarak oturum açın. Bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Bu makalede açıklanan görevleri tamamlamak için PowerShell komutlarını kullanmayı planlıyorsanız, öncelikle [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure PowerShell cmdlet'lerinin yüklü en son sürümüne sahip olduğunuzdan emin olun. Örneklerde PowerShell komutları için Yardım almak için girin `get-help <command> -full`.
- Bu makalede açıklanan görevleri tamamlamak için Azure CLI komutları kullanmayı planlıyorsanız, öncelikle [Azure CLI'yi yükleme ve yapılandırma](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure CLI yüklenmiş en son sürümüne sahip olduğunuzdan emin olun. Azure CLI komutlar hakkında Yardım almak için girin `az <command> --help`.


## <a name="create-vnet"></a>Bir sanal ağ oluşturma

Bir sanal ağ oluşturmak için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Tıklatın **yeni** > **ağ** > **sanal ağ**.
3. Üzerinde **sanal ağ** dikey penceresinde, **dağıtım modeli seçin** kutusunda, bırakın **Resource Manager** seçili ve ardından **oluşturma**.
4. Üzerinde **sanal ağ oluştur** dikey penceresinde girin veya aşağıdaki ayarları için değerleri seçin ve ardından **oluşturma**:
    - **Ad**: ad içinde benzersiz olmalıdır [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) sanal ağ oluşturmak için seçin. Sanal ağ oluşturulduktan sonra adı değiştirilemez. Zaman içinde birden çok sanal ağlar oluşturabilir. Öneriler adlandırma için bkz: [adlandırma kuralları](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Bir adlandırma kuralı birden çok sanal ağ yönetmeyi kolaylaştırmak yardımcı olabilir.
    - **Adres alanı**: CIDR gösteriminde adres alanını belirtin. Tanımladığınız adres alanı, ortak veya özel (RFC 1918) olabilir. Genel veya özel adres alanını tanımlamak, adres alanı birbirine bağlı sanal ağlar ve sanal ağa bağlı herhangi bir şirket içi ağlar sanal ağ içinde yalnızca erişilebilir olup. Aşağıdaki adres alanları ekleyemezsiniz:
        - 224.0.0.0/4 (çok noktaya yayın)
        - 255.255.255.255/32 (yayın)
        - 127.0.0.0/8 (geri döngü)
        - 169.254.0.0/16 (bağlantı yerel)
        - 168.63.129.16/32 (iç DNS)

      Sanal ağ oluşturduğunuzda, yalnızca bir adres alanı tanımlayabilirsiniz rağmen sanal ağ oluşturulduktan sonra daha fazla adres alanları ekleyebilirsiniz. Mevcut bir sanal ağa bir adres alanı eklemeyi öğrenmek için bkz: [ekleme veya kaldırma bir adres alanı](#add-address-spaces) bu makalede.

      >[!WARNING]
      >Bir sanal ağı başka bir sanal ağ ile çakışan adres alanları veya şirket içi ağ varsa, iki ağ bağlanamaz. Bir adres alanı tanımlamadan önce sanal ağ diğer sanal ağları veya şirket içi ağlar gelecekte bağlamak istediğiniz olup olmadığını göz önünde bulundurun.
      >
      >

    - **Alt ağ adı**: alt ağ adı sanal ağ içinde benzersiz olmalıdır. Alt ağ oluşturulduktan sonra alt ağ adı değiştiremezsiniz. Portal bir sanal ağ oluşturduğunuzda, bir sanal ağ alt ağı için gerekli değildir olsa da, bir alt ağ tanımlamanızı gerektirir. Bir sanal ağ oluşturduğunuzda, portalda, yalnızca bir alt ağ tanımlayabilirsiniz. Sanal ağ oluşturulduktan sonra daha fazla alt ağlar sanal ağa daha sonra ekleyebilirsiniz. Bir sanal ağa bir alt ağı eklemek için bkz: [bir alt ağ oluşturmak](virtual-network-manage-subnet.md#create-subnet) içinde [oluşturma, değiştirme veya silme alt](virtual-network-manage-subnet.md). Azure CLI veya PowerShell kullanarak birden çok alt ağa sahip bir sanal ağ oluşturabilirsiniz.

      >[!TIP]
      >Bazı durumlarda, yöneticileri filtre veya alt ağlar arasında trafiği yönlendirme denetlemek için farklı alt ağlar oluşturun. Alt ağlar tanımlamadan önce nasıl filtre uygulamak istediğiniz ve, alt ağlar arasında trafiği yönlendirme göz önünde bulundurun. Alt ağlar arasındaki trafik filtreleme hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](virtual-networks-nsg.md). Azure otomatik olarak alt ağlar, ancak arasındaki yolları trafiği Azure varsayılan yolların geçersiz kılabilirsiniz. Azure varsayılan alt ağ trafiği yönlendirmeyi geçersiz kılma öğrenmek için bkz: [kullanıcı tanımlı yollar](virtual-networks-udr-overview.md).
      >

    - **Alt ağ adres aralığı**: aralık sanal ağ için girilen adres alanı içinde olmalıdır. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, bir sanal ağ /29 bir alt ağ adres aralığı ile yalnızca üç kullanılabilir IP adresleri vardır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Belirli koşullar altında alt ağ oluşturulduktan sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağ ayarlarını değiştirme](#change-subnet) içinde [Ekle, değiştirme veya silme alt ağlar](virtual-network-manage-subnet.md).
    - **Abonelik**: seçin bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Aynı sanal ağ birden fazla Azure aboneliğinizde kullanılamıyor. Ancak, diğer Aboneliklerdeki sanal ağlar için bir abonelik sanal bir ağa bağlanabilir. Farklı Aboneliklerdeki sanal ağlara bağlanmak için Azure VPN ağ geçidi veya sanal ağ eşlemesi kullanın. Sanal ağa bağlanan herhangi bir Azure kaynak sanal ağ ile aynı abonelikte olması gerekir.
    - **Kaynak grubu**: Varolan seçin [kaynak grubu](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) veya yeni bir tane oluşturun. Sanal ağa bağlanan bir Azure kaynağı sanal ağ ile aynı kaynak grubunda veya farklı bir kaynak grubu olabilir.
    - **Konum**: Azure seçin [konumu](https://azure.microsoft.com/regions/), bir bölge olarak da bilinir. Bir sanal ağ içinde yalnızca bir Azure konumu olabilir. Ancak, tek bir konumda sanal ağ sanal bir ağa başka bir konuma bir VPN ağ geçidi kullanarak bağlayabilirsiniz. Sanal ağa bağlanan herhangi bir Azure kaynak sanal ağ ile aynı konumda olması gerekir.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ vnet oluşturma](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Sanal ağları görüntüle ve ayarları

Sanal ağlar ve ayarlarını görüntülemek için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında tıklatın **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde ayarlarını görüntülemek istediğiniz sanal ağ'ı tıklatın.
4. Aşağıdaki ayarlar dikey penceresinde, seçilen sanal ağ için listelenmiştir:
    - **Genel Bakış**: adres alanı ve DNS sunucuları da dahil olmak üzere sanal ağ hakkında bilgi sağlar. Aşağıdaki ekran görüntüsü adlı bir sanal ağ için genel ayarları gösterir **MyVNet**:

        ![Ağ arabirimi genel bakış](./media/virtual-network-manage-network/vnet-overview.png)

      Üzerinde **genel bakış** dikey penceresinde, sanal ağ için farklı bir abonelik veya kaynak grubu taşıyabilirsiniz. Bir sanal ağ taşıma hakkında bilgi edinmek için [farklı bir kaynak grubuna veya aboneliğe taşıma kaynakları](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Makaleyi önkoşulları ve Azure portal, PowerShell ve Azure CLI kullanarak kaynakları taşımak nasıl listeler. Sanal ağa bağlı tüm kaynaklar sanal ağ ile taşımanız gerekir.
    - **Adres alanı**: sanal ağa atanan adres alanları listelenir. Bir adres alanı ekleyip öğrenmek için adımları tamamlamanız [ekleme veya kaldırma bir adres alanı](#address-spaces) bu makalede.
    - **Bağlı cihazları**: sanal ağa bağlı tüm kaynaklar listelenir. Önceki ekran görüntüsünde, üç ağ arabirimleri ve bir yük dengeleyici sanal ağa bağlanır. Oluşturduğunuz ve sanal ağa bağlanmak yeni kaynaklar listelenir. Sanal ağa bağlı bir kaynak silerseniz, artık listede görüntülenir.
    - **Alt ağlar**: sanal ağ içindeki mevcut alt ağların bir listesi gösterilir. Bir alt ağı ekleyip öğrenmek için bkz: [bir alt ağ oluşturmak](virtual-network-manage-subnet.md#create-subnet) ve [bir alt ağını silmek](virtual-network-manage-subnet.md#delete-subnet) içinde [Ekle, değiştirme veya silme alt ağlar](virtual-network-manage-subnet.md).
    - **DNS sunucuları**: Azure iç DNS sunucusu veya özel bir DNS sunucusu ad çözümlemesi için sanal ağa bağlı olan aygıtları sağlayıp sağlamadığını belirtebilirsiniz. Azure portalı kullanarak bir sanal ağ oluşturduğunuzda, Azure'nın DNS sunucuları bir sanal ağ içinde ad çözümlemesi için varsayılan olarak kullanılır. DNS sunucuları değiştirmek için bölümündeki adımları tamamlamanız [ekleme, değiştirme veya kaldırma bir DNS sunucusu](#dns-servers) bu makalede.
    - **Eşlemeler**: abonelikte varolan eşlemeler varsa, bunlar burada listelenir. Var olan eşlemeleri için ayarları görüntüleyin veya oluşturun, değiştirin veya eşlemeler silin. Eşlemeler hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi](virtual-network-peering-overview.md).
    - **Özellikleri**: görüntüler sanal ağın kaynak kimliği ile içinde Azure aboneliği dahil olmak üzere sanal ağ ile ilgili ayarları.
    - **Diyagram**: görsel bir sanal ağa bağlı olan tüm aygıtları diyagramı sağlar. Diyagramın aygıtlar hakkında bazı önemli bilgiler vardır. Diyagramda, bu görünümde bir cihazı yönetmek için cihaz seçeneğine tıklayın.
    - **Ortak Azure ayarları**: ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki bilgilere bakın:
        *   [Etkinlik Günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Etiketler](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Otomasyon komut dosyası](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ vnet Göster](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Bir adres alanı Ekle Kaldır

Bir sanal ağ için adres alanları ekleyip çıkarabilirsiniz. Bir adres alanı CIDR gösteriminde belirtilmelidir ve aynı sanal ağda başka adres alanları ile örtüşemez. Tanımladığınız adres alanları ortak veya özel (RFC 1918) olabilir. Genel veya özel adres alanını tanımlamak, adres alanı birbirine bağlı sanal ağlar ve sanal ağa bağlı herhangi bir şirket içi ağlar sanal ağ içinde yalnızca erişilebilir olup. Aşağıdaki adres alanları ekleyemezsiniz:

- 224.0.0.0/4 (çok noktaya yayın)
- 255.255.255.255/32 (yayın)
- 127.0.0.0/8 (geri döngü)
- 169.254.0.0/16 (bağlantı yerel)
- 168.63.129.16/32 (iç DNS)

Eklemek veya bir adres alanı kaldırmak için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında seçin **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde istediğiniz eklemek veya bir adres alanı kaldırmak sanal ağ'ı tıklatın.
4. Sanal üzerinde dikey penceresinde altında ağ **ayarları**, tıklatın **adres alanı**.
5. Dikey penceresinde adres alanı, aşağıdaki seçeneklerden birini tamamlayın:
    - **Bir adres alanı Ekle**: yeni bir adres alanı girin. Adres alanı sanal ağ için tanımlı olan bir adres alanı ile örtüşemez.
    - **Bir adres alanı Kaldır**: bir adres alanı sağ tıklatın ve ardından **kaldırmak**. Bir alt ağ adres alanı varsa, adres alanını kaldıramazsınız. Bir adres alanı kaldırmak için hiçbir alt ağ (ve alt ağlara bağlı herhangi bir kaynağa) silmeniz gerekir adres alanında mevcut.
6. **Kaydet**’e tıklayın.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|Yalnızca Kaynak Yöneticisi|[az ağ vnet güncelleştirme](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Ekleme, değiştirme veya bir DNS sunucusunu kaldırma

Sanal ağ için belirttiğiniz DNS sunucuları ile sanal ağ kayıt bağlı tüm sanal makineleri. Bunlar ayrıca ad çözümlemesi için belirtilen DNS sunucusu kullanın. Bir VM her bir ağ arabirimine (NIC), kendi DNS sunucu ayarları olabilir. Bir NIC kendi DNS sunucu ayarları varsa, sanal ağın DNS sunucusu ayarlarını geçersiz kılar. NIC DNS ayarları hakkında daha fazla bilgi için bkz: [ağ arabirimi görevleri ve ayarlar](virtual-network-network-interface.md#change-dns-servers). VM'ler ve Azure Cloud Services rol örnekleri için ad çözümlemesi hakkında daha fazla bilgi edinmek için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md). Eklemek, değiştirmek veya bir DNS sunucusunu kaldırmak için:

1. Oturum [portal](https://portal.azure.com) aboneliğiniz için ağ katılımcı rolü (en az) için izinleri atanmış bir hesap ile. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında seçin **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde istediğiniz DNS ayarlarını değiştirmek için sanal ağ'ı tıklatın.
4. Sanal üzerinde dikey penceresinde altında ağ **ayarları**, tıklatın **DNS sunucuları**.
5. DNS sunucuları listeler dikey penceresinde aşağıdaki seçeneklerden birini seçin:
    - **Varsayılan (Azure tarafından sağlanan)**: tüm kaynak adları ve özel IP adresleri Azure DNS sunucularını otomatik olarak kaydedilir. Aynı sanal ağa bağlı herhangi bir kaynağa arasında adlarını çözümleyebilir. Sanal ağlar arasında adları çözümlemek için bu seçeneği kullanamazsınız. Sanal ağlar arasında adları çözümlemek için özel bir DNS sunucusu kullanmanız gerekir.
    - **Özel**: bir sanal ağ için Azure sınırına kadar bir veya daha fazla sunucu ekleyebilirsiniz. DNS sunucusu sınırları hakkında daha fazla bilgi için bkz: [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Aşağıdaki seçenekleriniz vardır:
        - **Adres Ekle**: sunucu, sanal ağ DNS sunucuları listesine ekler. Bu seçenek, DNS sunucusu Azure ile de kaydeder. Azure ile bir DNS sunucusu zaten kaydınız varsa, bu DNS sunucusu listesinde seçebilirsiniz.
        - **Bir adresi kaldırmak**: kaldırmak istediğiniz sunucuyu yanındaki tıklatın **X**. Sunucuyu silmek sunucuya yalnızca bu sanal ağ listesinden kaldırır. DNS sunucusu kullanmak için bir sanal ağlar için Azure kayıtlı kalır.
        - **DNS sunucusu adresleri yeniden sıralamak**: DNS sunucularınızın ortamınız için doğru sırada listesinde doğrulamak önemlidir. DNS sunucusu listeleri belirtildikleri sırada kullanılır. Hepsini ayarı olarak çalışmaz. Listedeki ilk DNS sunucusuna ulaşılabilirse, istemci olup DNS sunucusu düzgün bağımsız olarak, DNS sunucusunu kullanır. Listelenen tüm DNS sunucularına kaldırın ve sonra geri istediğiniz sırayla ekleyin.
        - **Adres değiştirme**: listedeki DNS sunucusunu vurgulayın ve ardından yeni bir ad girin.
6. **Kaydet**’e tıklayın.
7. Yeni DNS sunucusu ayarlarını atanmaları için sanal ağa bağlı sanal makineleri yeniden başlatın. Sanal makineleri yeniden başlatılana kadar geçerli DNS ayarlarını kullanmaya devam edin.

**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[az ağ vnet güncelleştirme](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Bir sanal ağı silme

Yalnızca ona bağlı hiçbir kaynak varsa, bir sanal ağ silebilirsiniz. Sanal ağ içindeki herhangi bir alt ağa bağlı kaynaklar varsa, sanal ağ içindeki tüm alt ağlara bağlı kaynakları silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt ağlara bağlı kaynakları silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Bir sanal ağı silmek için:

1. Oturum [portal](https://portal.azure.com) bir hesapla aboneliğiniz için ağ katılımcı rolü için diğer bir deyişle (en az) atanan izinleri. Rolleri ve izinleri hesaplarına atama hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Portal arama kutusuna **sanal ağlar**. Arama sonuçlarında tıklatın **sanal ağlar**.
3. Üzerinde **sanal ağlar** dikey penceresinde, silmek istediğiniz sanal ağı seçin.
4. Sanal ağ dikey penceresinde hiçbir cihaz olduğunu doğrulamak için sanal ağa bağlı, altında **ayarları**, tıklatın **bağlı cihazları**. Bağlı cihazlarınız varsa, sanal ağı silmeden önce silmeniz gerekir. Hiçbir bağlı cihazlarınız varsa, tıklatın **genel bakış**.
5. Dikey pencerenin üstündeki **silmek** simgesi.
6. Sanal ağı silme işlemini onaylamak için tıklatın **Evet**.


**Komutları**

|Aracı|Komut|
|---|---|
|Azure CLI|[Azure ağı vnet Sil](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Sonraki adımlar

- Bir VM oluşturun ve bir sanal ağa bağlanmak için bkz: [bir sanal ağ oluşturmak ve sanal makineleri bağlanmak](quick-create-portal.md#create-virtual-machines).
- Bir sanal ağ içindeki alt ağlara arasındaki ağ trafiğini filtrelemek için bkz: [ağ güvenlik grupları oluşturma](virtual-networks-create-nsg-arm-pportal.md).
- Başka bir sanal ağ sanal bir ağa eş için bkz: [bir sanal ağ eşlemesi oluşturma](virtual-network-create-peering.md#portal).
- Bir sanal ağ bir şirket içi ağa bağlamak için seçenekleri hakkında bilgi edinmek için [VPN Gateway hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
