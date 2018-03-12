---
title: "Bir Azure sanal ağı ile birden fazla alt ağ (Klasik) oluşturun | Microsoft Docs"
description: "Bir sanal ağ (Klasik) Azure içinde birden fazla alt ağı oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
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
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 1ec6d8d5327ec6d5ebb92e125cb4c52a7a929c0e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Bir sanal ağ ile birden fazla alt ağ (Klasik) oluşturun

> [!IMPORTANT]
> Azure sahip iki [farklı dağıtım modellerini](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmak ve kaynaklarla çalışmak için: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft önerir üzerinden çoğu yeni sanal ağlar oluşturma [Resource Manager](quick-create-portal.md) dağıtım modeli.

Bu öğreticide, ayrı ortak ve özel alt ağları olan bir temel Azure sanal ağ (Klasik) oluşturmayı öğrenin. Sanal makineler ve bir alt ağdaki bulut Hizmetleri gibi Azure kaynaklarını oluşturabilirsiniz. Sanal ağları (Klasik) oluşturulan kaynakları birbirleriyle ve diğer ağlarda, sanal bir ağa bağlı kaynaklar ile iletişim kurabilir.

Tüm hakkında daha fazla bilgi [sanal ağ](manage-virtual-network.md) ve [alt](virtual-network-manage-subnet.md) ayarlar.

> [!WARNING]
> Sanal ağları (Klasik), Azure tarafından hemen silinir, bir [abonelik devre dışı](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Sanal ağları (Klasik) kaynakları sanal ağında mevcut bağımsız olarak silinir. Daha sonra aboneliği yeniden etkinleştirirseniz, sanal ağ içinde var olan kaynakların yeniden oluşturulmalıdır.

Bir sanal ağ (Klasik) kullanarak oluşturabileceğiniz [Azure portal](#portal), [Azure komut satırı arabirimi (CLI) 1.0](#azure-cli), veya [PowerShell](#powershell).

## <a name="portal"></a>Portal

1. Bir Internet tarayıcısında Git [Azure portal](https://portal.azure.com). Kullanarak oturum açın, [Azure hesabı](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/offers/ms-azr-0044p) kaydolabilirsiniz.
2. Tıklatın **kaynak oluşturma** Portalı'nda.
3. Girin *sanal ağ* içinde **Market arama** kutusunun üstündeki **yeni** bölmesinde görüntülenir. Tıklatın **sanal ağ** arama sonuçlarında görüntülendiğinde.
4. Seçin **Klasik** içinde **dağıtım modeli seçin** kutusunda **sanal ağ** görünür, ardından bölmesinde **oluşturma**. 
5. Aşağıdaki değerleri girin **sanal ağ oluştur (Klasik)** bölmesi ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVnet|
    |Adres alanı|10.0.0.0/16|
    |Alt ağ adı|Genel|
    |Alt ağ adres aralığı|10.0.0.0/24|
    |Kaynak grubu|Bırakın **Yeni Oluştur** seçili yazıp enter **myResourceGroup**.|
    |Abonelik ve konum|Abonelik ve konum seçin.

    Hakkında daha fazla bilgi için Azure yeniyseniz, [kaynak grupları](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonelikleri](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), ve [konumları](https://azure.microsoft.com/regions) (olarak da adlandırılan *bölgeleri*).
4. Bir sanal ağ oluşturduğunuzda, portalda, yalnızca bir alt ağ oluşturabilirsiniz. Sanal ağ oluşturduktan sonra Bu öğreticide, ikinci bir alt ağ oluşturun. Daha sonra Internet'ten erişilebilen kaynaklara oluşturabilirsiniz **ortak** alt ağ. Internet'ten erişilemez kaynakları de oluşturabilir **özel** alt ağ. İkinci alt ağ oluşturmak için girin **myVnet** içinde **arama kaynakları** sayfanın üst kısmındaki kutusu. Tıklatın **myVnet** arama sonuçlarında görüntülendiğinde.
5. ' I tıklatın **alt ağlar** (içinde **ayarları** bölüm) üzerinde **sanal ağ oluştur (Klasik)** bölmesinde görüntülenir.
6. Tıklatın **+ Ekle** üzerinde **myVnet - alt ağlar** bölmesinde görüntülenir.
7. Girin **özel** için **adı** üzerinde **alt ağ Ekle** bölmesi. Girin **10.0.1.0/24** için **adres aralığı**.  **Tamam**’a tıklayın.
8. Üzerinde **myVnet - alt ağlar** bölmesinde görebilirsiniz **ortak** ve **özel** oluşturduğunuz alt ağlar.
9. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi yok, oluşturduğunuz kaynakları silmek isteyebilirsiniz:
    - Tıklatın **genel bakış** üzerinde **myVnet** bölmesi.
    - Tıklatın **silmek** simgesine **myVnet** bölmesi.
    - Silme işlemini onaylamak için tıklatın **Evet** içinde **Delete sanal ağ** kutusu.

## <a name="azure-cli"></a>Azure CLI

1. Seçebilir ya da [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), veya Azure bulut kabuktan CLI kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. CLI komutları için Yardım almak için yazın `azure <command> --help`. 
2. CLI oturumunda, Azure için aşağıdaki komutu ile oturum açın. Tıklatırsanız **deneyin** kutuya bir bulut Kabuğu'nu açar. Azure aboneliğiniz için aşağıdaki komutu girmeden oturum:

    ```azurecli-interactive
    azure login
    ```

3. CLI Hizmet Yönetimi modunda olduğundan emin olmak için aşağıdaki komutu girin:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Bir sanal ağ ile özel bir alt ağ oluşturun:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Sanal ağ içindeki ortak bir alt ağ oluşturun:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Sanal ağ ve alt ağları gözden geçirin:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi olmayan oluşturduğunuz kaynakları silmek isteyebilirsiniz:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> CLI kullanarak bir sanal ağ (Klasik) oluşturmak için bir kaynak grubu belirtemezsiniz olsa, Azure sanal ağ içinde adlı bir kaynak grubu oluşturur. *varsayılan ağ*.

## <a name="powershell"></a>PowerShell

1. PowerShell'ın en son sürümünü yüklemek [Azure](https://www.powershellgallery.com/packages/Azure) modülü. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. Bir PowerShell oturumu başlatın.
3. PowerShell'de `Add-AzureAccount` komutunu girerek Azure oturumu açın.
4. Aşağıdaki yolunu ve dosya adı, uygun şekilde değiştirme sonra mevcut ağ yapılandırma dosyasını dışarı aktar:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. Ortak ve özel alt ağları ile bir sanal ağ oluşturmak için eklemek için herhangi bir metin düzenleyicisi kullanın **VirtualNetworkSite** ağ yapılandırma dosyasına aşağıdaki öğesi.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Tam gözden [ağ yapılandırma dosyası şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Ağ yapılandırma dosyasını içeri aktarın:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Değiştirilen ağ yapılandırma dosyasını içeri varolan sanal ağlar (aboneliğinizde Klasik) yapılan değişiklikler neden olabilir. Yalnızca önceki sanal ağ ekleyin ve verme değiştirir veya var olan tüm sanal ağları aboneliğinizden kaldırırsanız, emin olun. 

7. Sanal ağ ve alt ağları gözden geçirin:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **İsteğe bağlı**: Bu öğreticiyi tamamladığınızda, böylece kullanım ücretlerine tabi olmayan oluşturduğunuz kaynakları silmek isteyebilirsiniz. Sanal ağı silmek için 4-6 yeniden, bu zaman kaldırma adımları tamamlayın **VirtualNetworkSite** 5. adımda eklediğiniz öğesi.
 
> [!NOTE]
> PowerShell kullanarak bir sanal ağ (Klasik) oluşturmak için bir kaynak grubu belirtemezsiniz olsa, Azure sanal ağ içinde adlı bir kaynak grubu oluşturur. *varsayılan ağ*.

---

## <a name="next-steps"></a>Sonraki adımlar

- Tüm sanal ağ ve alt ağ ayarları hakkında bilgi edinmek için bkz: [sanal ağlarını yönetmeleri](manage-virtual-network.md) ve [sanal ağ alt ağlarını yönetin](virtual-network-manage-subnet.md). Sanal ağlar ve alt ağları farklı gereksinimlerini karşılamak için bir üretim ortamında kullanmadan yönelik çeşitli seçenekleriniz vardır.
- Alt ağa gelen ve giden trafiği filtrelemek, oluşturmak ve uygulamak için [ağ güvenlik grubu](virtual-networks-nsg.md) alt ağlara.
- Oluşturma bir [Windows](../virtual-machines/windows/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine ve mevcut bir sanal ağa bağlayın.
- İki sanal ağ ile aynı konumda Azure bağlanmak için oluşturmanız bir [sanal ağ eşlemesi](create-peering-different-deployment-models.md) sanal ağlar arasında. Sanal ağ (Klasik) için bir sanal ağ (Resource Manager) eşi, ancak eşlemesi iki sanal ağ arasında (Klasik) oluşturulamıyor.
- Sanal ağ kullanarak bir şirket ağına bağlanan bir [VPN ağ geçidi](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hattı.
