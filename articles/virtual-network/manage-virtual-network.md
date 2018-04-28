---
title: Oluşturma, değiştirme veya bir Azure sanal ağı silme | Microsoft Docs
description: Oluşturma, değiştirme veya silme Azure sanal ağında öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: ce858553a67bce714ceae43a5bb2f86839d9c507
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Oluşturma, değiştirme veya bir sanal ağı silme

Oluşturmak ve sanal ağı silmek ve DNS sunucuları ve IP adresi alanlarını, varolan bir sanal ağ gibi ayarlarını değiştirmek öğrenin.

Bir sanal ağ kendi ağ bulutta gösterimidir. Azure bulutunun Azure aboneliğinize adanmış mantıksal ayırma bir sanal ağdır. Oluşturduğunuz her sanal ağ için şunları yapabilirsiniz:
- Atamak için bir adres alanı seçin. Bir adres alanı 10.0.0.0/16 gibi sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanılarak tanımlanmış bir veya daha fazla adres aralıklarını oluşur.
- Azure tarafından sağlanan DNS sunucusu kullanmayı veya kendi DNS sunucusu kullanmayı seçin. Sanal ağa bağlı tüm kaynaklar sanal ağ içindeki adları çözümlemek için bu DNS sunucusuna atanır.
- Sanal ağ alt ağlar, her biri kendi adres aralığında sanal ağın adres alanı içine kesiminde. Oluşturma, değiştirme ve alt ağları silme öğrenmek için bkz: [Ekle, değiştirme veya silme alt ağlar](virtual-network-manage-subnet.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğreticide Azure PowerShell modülü sürümü 5.2.0 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.26 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak oluşturma** > **ağ** > **sanal ağ**.
2. Girin veya aşağıdaki ayarları için değerleri seçin ve ardından seçin **oluşturma**:
    - **Ad**: ad içinde benzersiz olmalıdır [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) sanal ağ oluşturmak için seçin. Sanal ağ oluşturulduktan sonra adı değiştirilemez. Zaman içinde birden çok sanal ağlar oluşturabilir. Öneriler adlandırma için bkz: [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). Bir adlandırma kuralı birden çok sanal ağ yönetmeyi kolaylaştırmak yardımcı olabilir.
    - **Adres alanı**: bir sanal ağın adres alanı CIDR gösteriminde belirtilen bir veya daha fazla çakışmayan adres aralıklarını oluşur. Tanımladığınız adres aralığı ortak veya özel (RFC 1918) olabilir. Adres aralığı ortak veya özel olarak tanımlamak, adres aralığı birbirine bağlı sanal ağlar ve sanal ağa bağlı herhangi bir şirket içi ağlar sanal ağ içinde yalnızca erişilebilir olup. Aşağıdaki adres aralıklarını ekleyemezsiniz:
        - 224.0.0.0/4 (çok noktaya yayın)
        - 255.255.255.255/32 (yayın)
        - 127.0.0.0/8 (geri döngü)
        - 169.254.0.0/16 (bağlantı yerel)
        - 168.63.129.16/32 (iç DNS)

      Sanal ağ oluşturduğunuzda, yalnızca bir adres aralığı tanımlayabilirsiniz rağmen sanal ağ oluşturulduktan sonra daha fazla adres aralığı için adres alanı ekleyebilirsiniz. Mevcut bir sanal ağa bir adres aralığı eklemeyi öğrenmek için bkz: [ekleme veya kaldırma bir adres aralığı](#add-or-remove-an-address-range).

      >[!WARNING]
      >Bir sanal ağı başka bir sanal ağ ile çakışıyor adres aralıklarını veya şirket içi ağ varsa, iki ağ bağlanamaz. Bir adres aralığı tanımlamadan önce sanal ağ diğer sanal ağları veya şirket içi ağlar gelecekte bağlamak istediğiniz olup olmadığını göz önünde bulundurun.
      >
      >

    - **Alt ağ adı**: alt ağ adı sanal ağ içinde benzersiz olmalıdır. Alt ağ oluşturulduktan sonra alt ağ adı değiştiremezsiniz. Portal bir sanal ağ oluşturduğunuzda, bir sanal ağ alt ağı için gerekli değildir olsa da, bir alt ağ tanımlamanızı gerektirir. Bir sanal ağ oluşturduğunuzda, portalda, yalnızca bir alt ağ tanımlayabilirsiniz. Sanal ağ oluşturulduktan sonra daha fazla alt ağlar sanal ağa daha sonra ekleyebilirsiniz. Bir sanal ağa bir alt ağı eklemek için bkz: [alt ağlarını yönetin](virtual-network-manage-subnet.md). Azure CLI veya PowerShell kullanarak birden çok alt ağa sahip bir sanal ağ oluşturabilirsiniz.

      >[!TIP]
      >Bazı durumlarda, yöneticiler filtre veya alt ağlar arasında trafiği yönlendirme denetlemek için farklı alt ağlar oluşturun. Alt ağlar tanımlamadan önce nasıl filtre uygulamak istediğiniz ve, alt ağlar arasında trafiği yönlendirme göz önünde bulundurun. Alt ağlar arasındaki trafik filtreleme hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](security-overview.md). Azure otomatik olarak alt ağlar, ancak arasındaki yolları trafiği Azure varsayılan yolların geçersiz kılabilirsiniz. Azures varsayılan alt ağ trafiği yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
      >

    - **Alt ağ adres aralığı**: aralık sanal ağ için girilen adres alanı içinde olmalıdır. Belirleyebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralıktır. Azure her alt ağ protokolü uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri Azure hizmetinin kullanım için ayrılmıştır. Sonuç olarak, bir sanal ağ /29 bir alt ağ adres aralığı ile yalnızca üç kullanılabilir IP adresleri vardır. Bir sanal ağ VPN ağ geçidi bağlanmak istiyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinmek [ağ geçidi alt ağları için belirli bir adresi aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Belirli koşullar altında alt ağ oluşturulduktan sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adresi aralığını değiştirmek konusunda bilgi almak için bkz: [alt ağlarını yönetin](virtual-network-manage-subnet.md).
    - **Abonelik**: seçin bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Aynı sanal ağ birden fazla Azure aboneliğinizde kullanılamıyor. Ancak, diğer aboneliklerle sanal ağlarda bir abonelik sanal bir ağa bağlanabilir [sanal ağ eşlemesi](virtual-network-peering-overview.md). Sanal ağa bağlanan herhangi bir Azure kaynak sanal ağ ile aynı abonelikte olması gerekir.
    - **Kaynak grubu**: Varolan seçin [kaynak grubu](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) veya yeni bir tane oluşturun. Sanal ağa bağlanan bir Azure kaynağı sanal ağ ile aynı kaynak grubunda veya farklı bir kaynak grubu olabilir.
    - **Konum**: Azure seçin [konumu](https://azure.microsoft.com/regions/), bir bölge olarak da bilinir. Bir sanal ağ içinde yalnızca bir Azure konumu olabilir. Ancak, tek bir konumda sanal ağ sanal bir ağa başka bir konuma bir VPN ağ geçidi kullanarak bağlayabilirsiniz. Sanal ağa bağlanan herhangi bir Azure kaynak sanal ağ ile aynı konumda olması gerekir.

**Komutları**

- Azure CLI: [az ağ vnet oluşturma](/cli/azure/network/vnet)
- PowerShell: [yeni-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Sanal ağları görüntüle ve ayarları

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden ayarlarını görüntülemek istediğiniz sanal ağı seçin.
3. Aşağıdaki ayarlar seçtiğiniz sanal ağ için listelenmiştir:
    - **Genel Bakış**: adres alanı ve DNS sunucuları da dahil olmak üzere sanal ağ hakkında bilgi sağlar. Aşağıdaki ekran görüntüsü adlı bir sanal ağ için genel ayarları gösterir **MyVNet**:

        ![Ağ arabirimi genel bakış](./media/manage-virtual-network/vnet-overview.png)

      Seçerek bir sanal ağ için farklı bir abonelik veya kaynak grubu taşıyabilirsiniz **değişiklik** yanına **kaynak grubu** veya **abonelik adı**. Bir sanal ağ taşıma hakkında bilgi edinmek için [farklı bir kaynak grubuna veya aboneliğe taşıma kaynakları](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Makaleyi önkoşulları ve Azure portal, PowerShell ve Azure CLI kullanarak kaynakları taşımak nasıl listeler. Sanal ağa bağlı tüm kaynaklar sanal ağ ile taşımanız gerekir.
    - **Adres alanı**: sanal ağa atanan adres alanları listelenir. Adres alanı için bir adres aralığı ekleyip öğrenmek için adımları tamamlamanız [ekleme veya kaldırma bir adres aralığı](#add-or-remove-an-address-range).
    - **Bağlı cihazları**: sanal ağa bağlı tüm kaynaklar listelenir. Önceki ekran görüntüsünde, üç ağ arabirimleri ve bir yük dengeleyici sanal ağa bağlanır. Oluşturduğunuz ve sanal ağa bağlanmak yeni kaynaklar listelenir. Sanal ağa bağlı bir kaynak silerseniz, artık listede görüntülenir.
    - **Alt ağlar**: sanal ağ içindeki mevcut alt ağların bir listesi gösterilir. Bir alt ağı ekleyip öğrenmek için bkz: [alt ağlarını yönetin](virtual-network-manage-subnet.md).
    - **DNS sunucuları**: Azure iç DNS sunucusu veya özel bir DNS sunucusu ad çözümlemesi için sanal ağa bağlı olan aygıtları sağlayıp sağlamadığını belirtebilirsiniz. Azure portalı kullanarak bir sanal ağ oluşturduğunuzda, Azure'nın DNS sunucuları bir sanal ağ içinde ad çözümlemesi için varsayılan olarak kullanılır. DNS sunucuları değiştirmek için bölümündeki adımları tamamlamanız [değişiklik DNS sunucuları](#change-dns-servers) bu makalede.
    - **Eşlemeler**: abonelikte varolan eşlemeler varsa, bunlar burada listelenir. Var olan eşlemeleri için ayarları görüntüleyin veya oluşturun, değiştirin veya eşlemeler silin. Eşlemeler hakkında daha fazla bilgi için bkz: [sanal ağ eşlemesi](virtual-network-peering-overview.md).
    - **Özellikleri**: görüntüler sanal ağın kaynak kimliği ile içinde Azure aboneliği dahil olmak üzere sanal ağ ile ilgili ayarları.
    - **Diyagram**: görsel bir sanal ağa bağlı olan tüm aygıtları diyagramı sağlar. Diyagramın aygıtlar hakkında bazı önemli bilgiler vardır. Diyagramda, bu görünümde bir cihazı yönetmek için cihazı seçin.
    - **Ortak Azure ayarları**: ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki bilgilere bakın:
        *   [Etkinlik Günlüğü](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Erişim denetimi (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Etiketler](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Kilitler](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Otomasyon komut dosyası](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Komutları**

- Azure CLI: [az ağ vnet Göster](/cli/azure/network/vnet#az_network_vnet_show)
- PowerShell: [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Bir adres aralığı Ekle Kaldır

Bir sanal ağ adres aralıklarını ekleyip çıkarabilirsiniz. Bir adres aralığı CIDR gösteriminde belirtilmelidir ve aynı sanal ağda başka adres aralıklarıyla çakışamaz. Tanımladığınız adres aralıklarını ortak veya özel (RFC 1918) olabilir. Adres aralığı ortak veya özel olarak tanımlamak, adres aralığı birbirine bağlı sanal ağlar ve sanal ağa bağlı herhangi bir şirket içi ağlar sanal ağ içinde yalnızca erişilebilir olup. Aşağıdaki adres aralıklarını ekleyemezsiniz:

- 224.0.0.0/4 (çok noktaya yayın)
- 255.255.255.255/32 (yayın)
- 127.0.0.0/8 (geri döngü)
- 169.254.0.0/16 (bağlantı yerel)
- 168.63.129.16/32 (iç DNS)

Eklemek veya bir adres aralığı kaldırmak için:

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden eklemek veya bir adres aralığı kaldırmak istediğiniz sanal ağı seçin.
3. Seçin **adres alanı**altında **ayarları**.
4. Aşağıdaki seçeneklerden birini tamamlayın:
    - **Bir adres aralığı Ekle**: yeni adres aralığı girin. Adres aralığı, sanal ağ için tanımlı olan bir adres aralığı ile örtüşemez.
    - **Bir adres aralığı kaldırmak**: kaldırmak istediğiniz adres aralığını sağ tarafta seçin **...** seçeneğini belirleyip **kaldırmak**. Bir alt ağ adres aralığındaki varsa, adres aralığı kaldıramazsınız. Bir adres aralığı kaldırmak için hiçbir alt ağ (ve tüm alt ağlar kaynaklarında) silmeniz gerekir adres aralığındaki mevcut.
5. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ vnet güncelleştirme](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

Sanal ağ için belirttiğiniz DNS sunucuları ile sanal ağ kayıt bağlı tüm sanal makineleri. Bunlar ayrıca ad çözümlemesi için belirtilen DNS sunucusu kullanın. Bir VM her bir ağ arabirimine (NIC), kendi DNS sunucu ayarları olabilir. Bir NIC kendi DNS sunucu ayarları varsa, sanal ağın DNS sunucusu ayarlarını geçersiz kılar. NIC DNS ayarları hakkında daha fazla bilgi için bkz: [ağ arabirimi görevleri ve ayarlar](virtual-network-network-interface.md#change-dns-servers). VM'ler ve Azure Cloud Services rol örnekleri için ad çözümlemesi hakkında daha fazla bilgi edinmek için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md). Eklemek, değiştirmek veya bir DNS sunucusunu kaldırmak için:

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden DNS sunucuları için değiştirmek istediğiniz sanal ağı seçin.
3.  Seçin **DNS sunucuları**altında **ayarları**.
4. Aşağıdaki seçeneklerden birini seçin:
    - **Varsayılan (Azure tarafından sağlanan)**: tüm kaynak adları ve özel IP adresleri Azure DNS sunucularını otomatik olarak kaydedilir. Aynı sanal ağa bağlı herhangi bir kaynağa arasında adlarını çözümleyebilir. Sanal ağlar arasında adları çözümlemek için bu seçeneği kullanamazsınız. Sanal ağlar arasında adları çözümlemek için özel bir DNS sunucusu kullanmanız gerekir.
    - **Özel**: bir sanal ağ için Azure sınırına kadar bir veya daha fazla sunucu ekleyebilirsiniz. DNS sunucusu sınırları hakkında daha fazla bilgi için bkz: [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Aşağıdaki seçenekleriniz vardır:
        - **Adres Ekle**: sunucu, sanal ağ DNS sunucuları listesine ekler. Bu seçenek, DNS sunucusu Azure ile de kaydeder. Azure ile bir DNS sunucusu zaten kaydınız varsa, bu DNS sunucusu listesinde seçebilirsiniz.
        - **Bir adresi kaldırmak**: kaldırmak istediğiniz sunucuyu yanındaki seçin **...** , ardından **kaldırmak**. Sunucuyu silmek sunucuya yalnızca bu sanal ağ listesinden kaldırır. DNS sunucusu kullanmak için bir sanal ağlar için Azure kayıtlı kalır.
        - **DNS sunucusu adresleri yeniden sıralamak**: DNS sunucularınızın ortamınız için doğru sırada listesinde doğrulamak önemlidir. DNS sunucusu listeleri belirtildikleri sırada kullanılır. Hepsini ayarı olarak çalışmaz. Listedeki ilk DNS sunucusuna ulaşılabilirse, istemci olup DNS sunucusu düzgün bağımsız olarak, DNS sunucusunu kullanır. Listelenen tüm DNS sunucularına kaldırın ve sonra geri istediğiniz sırayla ekleyin.
        - **Adres değiştirme**: listedeki DNS sunucusunu vurgulayın ve ardından yeni adresini girin.
5. **Kaydet**’i seçin.
6. Yeni DNS sunucusu ayarlarını atanmaları için sanal ağa bağlı sanal makineleri yeniden başlatın. Sanal makineleri yeniden başlatılana kadar geçerli DNS ayarlarını kullanmaya devam edin.

**Komutları**

- Azure CLI: [az ağ vnet güncelleştirme](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Bir sanal ağı silme

Yalnızca ona bağlı hiçbir kaynak varsa, bir sanal ağ silebilirsiniz. Sanal ağ içindeki herhangi bir alt ağa bağlı kaynaklar varsa, sanal ağ içindeki tüm alt ağlara bağlı kaynakları silmeniz gerekir. Bir kaynak silmek için uygulayacağınız adımlar kaynak bağlı olarak değişir. Alt ağlara bağlı kaynakları silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Bir sanal ağı silmek için:

1. Portal üstündeki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünür.
2. Sanal ağlar listesinden silmek istediğiniz sanal ağı seçin.
3. Hiçbir aygıt seçerek sanal ağa bağlı olduğundan emin olun **bağlı cihazları**altında **ayarları**. Bağlı cihazlarınız varsa, sanal ağı silmeden önce silmeniz gerekir. Hiçbir bağlı cihazlarınız varsa, seçin **genel bakış**.
4. **Sil**’i seçin.
5. Sanal ağı silme işlemini onaylamak için seçin **Evet**.

**Komutları**

- Azure CLI: [azure ağ vnet Sil](/cli/azure/network/vnet#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork)

## <a name="permissions"></a>İzinler

Sanal ağlar üzerinde görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun izinleri atanmış rolü aşağıdaki tabloda listelenen:

|İşlem                                    |   İşlem adı                    |
|-------------------------------------------  |   --------------------------------  |
|Microsoft.Network/virtualNetworks/read       |   Sanal Ağı Al               |
|Microsoft.Network/virtualNetworks/write      |   Sanal ağ oluştur veya güncelleştir  |
|Microsoft.Network/virtualNetworks/delete     |   Sanal ağ silinemedi            |

## <a name="next-steps"></a>Sonraki adımlar

- Bir VM oluşturun ve bir sanal ağa bağlanmak için bkz: [bir sanal ağ oluşturmak ve sanal makineleri bağlanmak](quick-create-portal.md#create-virtual-machines).
- Bir sanal ağ içindeki alt ağlara arasındaki ağ trafiğini filtrelemek için bkz: [ağ güvenlik grupları oluşturma](virtual-networks-create-nsg-arm-pportal.md).
- Başka bir sanal ağ sanal bir ağa eş için bkz: [bir sanal ağ eşlemesi oluşturma](tutorial-connect-virtual-networks-portal.md).
- Bir sanal ağ bir şirket içi ağa bağlamak için seçenekleri hakkında bilgi edinmek için [VPN Gateway hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
