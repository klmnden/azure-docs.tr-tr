---
title: "Bir Azure sanal ağ (Klasik) - ağ yapılandırma dosyası yapılandırma | Microsoft Docs"
description: "Oluşturma ve dışarı aktarma, değiştirme ve bir ağ yapılandırma dosyasını içeri sanal ağları (Klasik) değiştirme öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f1e3ae26b6525f2235a6b0d53546b334dc027b94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Bir ağ yapılandırma dosyası kullanarak bir sanal ağ (Klasik) yapılandırma
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir.

Oluşturun ve bir ağ yapılandırma dosyası Azure komut satırı arabirimi (CLI) 1.0 veya Azure PowerShell kullanarak bir sanal ağ (Klasik) yapılandırın. Oluşturma veya bir ağ yapılandırma dosyası kullanarak Azure Resource Manager dağıtım modeli aracılığıyla bir sanal ağ değiştiremezsiniz. Bir ağ yapılandırma dosyası kullanmadan oluşturmak veya bir sanal ağ (Klasik) oluşturmak için Azure portalını kullanabilirsiniz ancak bir ağ yapılandırma dosyası kullanarak bir sanal ağ (Klasik) değiştirmek için Azure Portalı'nı kullanamazsınız.

Oluşturma ve bir ağ yapılandırma dosyası ile bir sanal ağ (Klasik) yapılandırma dosyasını içeri dışarı aktarma ve değiştirme gerektirir.

## <a name="export"></a>Bir ağ yapılandırma dosyası dışarı aktarma

Ağ yapılandırma dosyasını dışarı aktarmak için PowerShell veya Azure CLI kullanın. Azure CLI bir json dosyası verirken PowerShell bir XML dosyasına dışarı aktarır.

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell'i yükleyin ve Azure'da oturum aç](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Dizini değiştirmek (ve onu var olduğundan emin olun) ve istediğiniz olarak aşağıdaki komutta filename ağ yapılandırma dosyasını dışarı aktarmak için komutu çalıştırın:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Azure CLI 1.0 yüklemek](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure CLI 1.0 komut isteminden kalan adımları tamamlayın.
2. Azure'a girerek oturum açma `azure login` komutu.
3. Girerek asm modunda olduğunuzdan emin olun `azure config mode asm` komutu.
4. Dizini değiştirmek (ve onu var olduğundan emin olun) ve istediğiniz olarak aşağıdaki komutta filename ağ yapılandırma dosyasını dışarı aktarmak için komutu çalıştırın:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Oluşturma veya bir ağ yapılandırma dosyasını değiştirme

Bir ağ yapılandırma XML dosyasını (PowerShell kullanırken) veya bir json dosyası (Azure CLI kullanırken) dosyasıdır. Tüm metin veya XML/json Düzenleyicisi dosyasında düzenleyebilirsiniz. [Ağ yapılandırma dosyası şeması ayarları](https://msdn.microsoft.com/library/azure/jj157100.aspx) makale tüm ayarları için ayrıntılarını içerir. Ayarları ek açıklaması için bkz: [sanal ağlar ve ayarları görüntüle](virtual-network-manage-network.md#view-vnet). Dosyada yaptığınız değişiklikler:

- Şema veya ağ yapılandırma dosyası başarısız olur alma ile uyumlu olmalıdır.
- Aboneliğiniz için var olan tüm ağ ayarlarını üzerine yazma, bu nedenle değişiklikler yaparken dikkatli. Örneğin, izleyin örnek ağ yapılandırması dosyaları başvuru. Özgün dosya bulunan iki söyleyin **VirtualNetworkSite** örnekleri ve değişti, örnekte gösterildiği gibi. Dosyasını içe aktardığınızda, Azure sanal ağ için siler **VirtualNetworkSite** dosyasında kaldırdığınız örneği. Hiçbir kaynak olan sanal ağ, Basitleştirilmiş bu senaryo varsayar gibi olsaydı, sanal ağ silinemedi ve içeri aktarma başarısız olur.

> [!IMPORTANT]
> Azure, bir şey olarak dağıtılan sahip bir alt ağ göz önünde bulundurur **kullanımda**. Bir alt ağ kullanımdayken değiştirilemez. Ağ yapılandırma dosyasında alt ağ bilgilerini değiştirmeden önce alt değiştirilen olmayan farklı bir alt ağa dağıttığınız herhangi bir şey taşıyın. Bkz: [bir VM veya rol örneği farklı bir alt ağa taşıma](virtual-networks-move-vm-role-to-subnet.md) Ayrıntılar için.

### <a name="example-xml-for-use-with-powershell"></a>PowerShell ile kullanılmak üzere XML örneği

Aşağıdaki örnek ağ yapılandırma dosyası adlı bir sanal ağ oluşturur *myVirtualNetwork* bir adres alanı ile *10.0.0.0/16* içinde *Doğu ABD* Azure bölgesi. Sanal ağ bir alt ağ içeren *mySubnet* bir adres öneki ile *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Hiç içerik dışa aktardığınız ağ yapılandırma dosyasını içeriyorsa, önceki örnekte XML kopyalayın ve yeni bir dosyaya yapıştırın.

### <a name="example-json-for-use-with-the-azure-cli-10"></a>Örnek JSON Azure CLI 1.0 ile kullanmak için

Aşağıdaki örnek ağ yapılandırma dosyası adlı bir sanal ağ oluşturur *myVirtualNetwork* bir adres alanı ile *10.0.0.0/16* içinde *Doğu ABD* Azure bölgesi. Sanal ağ bir alt ağ içeren *mySubnet* bir adres öneki ile *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Hiç içerik dışa aktardığınız ağ yapılandırma dosyasını içeriyorsa, önceki örnekte json kopyalayın ve yeni bir dosyaya yapıştırın.

## <a name="import"></a>Ağ yapılandırma dosyasını içeri aktarın

Bir ağ yapılandırma dosyası içeri aktarmak için PowerShell veya Azure CLI kullanın. Azure CLI bir json dosyası alırken PowerShell bir XML dosyasına aktarır. Alma işlemi başarısız olursa, dosya ile uyumlu olduğunu onaylayın [ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell'i yükleyin ve Azure'da oturum aç](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Dizin ve dosya adı gerekirse aşağıdaki komutta ağ yapılandırma dosyasını içeri aktarmak için komutu çalıştırmak değiştirin:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Azure CLI 1.0 yüklemek](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure CLI 1.0 komut isteminden kalan adımları tamamlayın.
2. Azure'a girerek oturum açma `azure login` komutu.
3. Girerek asm modunda olduğunuzdan emin olun `azure config mode asm` komutu.
4. Dizin ve dosya adı gerekirse aşağıdaki komutta ağ yapılandırma dosyasını içeri aktarmak için komutu çalıştırmak değiştirin:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
