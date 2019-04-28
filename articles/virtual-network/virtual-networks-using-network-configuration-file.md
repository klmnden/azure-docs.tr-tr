---
title: Bir Azure sanal ağ (Klasik) - ağ yapılandırma dosyasını yapılandırma | Microsoft Docs
description: Oluşturma ve dışarı aktarma, değiştirme ve bir ağ yapılandırma dosyasını içe aktarma sanal ağlar (Klasik) değiştirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.custom: ''
ms.openlocfilehash: e26ec4d268b9bd8852ef8cd2c522995902e15923
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108021"
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Bir ağ yapılandırma dosyası kullanarak bir sanal ağ (Klasik) yapılandırma
> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir.

Oluşturun ve bir ağ yapılandırma dosyası Klasik Azure komut satırı arabirimi (CLI) veya Azure PowerShell kullanarak sanal ağ (Klasik) yapılandırma. Oluşturamaz veya bir sanal ağ bir ağ yapılandırma dosyasını kullanarak Azure Resource Manager dağıtım modeliyle değiştirin. Azure portalı, bir ağ yapılandırma dosyası kullanmadan oluşturmak veya bir sanal ağ (Klasik) oluşturmak için Azure portalını kullanabilirsiniz ancak bir ağ yapılandırma dosyası kullanarak bir sanal ağ (Klasik) değiştirmek için kullanamazsınız.

Oluşturma ve bir ağ yapılandırma dosyası ile bir sanal ağ (Klasik) yapılandırma dosyasını içeri dışarı aktarma ve değiştirme gerektirir.

## <a name="export"></a>Bir ağ yapılandırma dosyasını dışarı aktar

Bir ağ yapılandırma dosyasını dışarı aktarmak için PowerShell veya Azure Klasik CLI'yı kullanabilirsiniz. PowerShell, Azure Klasik CLI bir json dosyası verirken bir XML dosyasına dışarı aktarır.

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell'i yükleyin ve Azure'da oturum açma](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Dizini değiştirmek (ve onu var olduğundan emin olun) ve aşağıdaki komutu istenen dosya ardından ağ yapılandırma dosyasını dışarı aktarmak için komutu çalıştırın:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-classic-cli"></a>Azure klasik CLI

1. [Klasik Azure CLI yükleme](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Klasik CLI komut isteminde kalan adımları tamamlayın.
2. Azure'da girerek oturum açma `azure login` komutu.
3. Asm moduna girerek emin olun `azure config mode asm` komutu.
4. Dizini değiştirmek (ve onu var olduğundan emin olun) ve aşağıdaki komutu istenen dosya ardından ağ yapılandırma dosyasını dışarı aktarmak için komutu çalıştırın:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Bir ağ yapılandırma dosyasını oluşturun veya değiştirin

Bir ağ yapılandırma (PowerShell kullanırken) bir XML dosyasına veya bir json dosyası (Klasik CLI'yı kullanırken) dosyasıdır. Tüm metin veya XML/json düzenleyicide dosyayı düzenleyebilirsiniz. [Ağ yapılandırma dosyası şeması ayarları](https://msdn.microsoft.com/library/azure/jj157100.aspx) makale tüm ayarlarının ayrıntılarını içerir. Ayarları ek açıklaması için bkz: [sanal ağlar ve ayarları görüntüleme](manage-virtual-network.md#view-virtual-networks-and-settings). Dosyaya yaptığınız değişiklikler:

- Şema veya ağ yapılandırma dosyasını başarısız olur alma ile uyumlu olmalıdır.
- Aboneliğiniz için var olan tüm ağ ayarlarının üzerine, bu nedenle değişiklikler yaparken dikkatli. Örneğin, aşağıdaki örnek ağ yapılandırma dosyalarını başvuru. Söyleyin özgün dosya bulunan iki **VirtualNetworkSite** örnekleri ve değiştirildi, örnekte gösterildiği gibi. Dosyayı içeri aktardığınızda, Azure sanal ağ için siler **VirtualNetworkSite** örneği dosyasında kaldırıldı. Kaynak sanal ağında olan Basitleştirilmiş bu senaryo varsayar yokmuş gibi sanal ağ silinemedi ve içeri aktarma başarısız olur.

> [!IMPORTANT]
> Azure, bir şey olarak dağıtılmış olan bir alt ağ göz önünde bulundurur **kullanımda**. Bir alt ağ kullanımdayken değiştirilemez. Ağ yapılandırma dosyasında bir alt ağ bilgilerini değiştirmeden önce değiştirilen olmayan farklı bir alt ağ için alt ağa dağıttığınız herhangi bir şey taşıyın. Bkz: [bir VM veya rol örneğini farklı bir alt ağa taşıma](virtual-networks-move-vm-role-to-subnet.md) Ayrıntılar için.

### <a name="example-xml-for-use-with-powershell"></a>PowerShell ile kullanılmak üzere XML örneği

Aşağıdaki örnek ağ yapılandırma dosyası adlı bir sanal ağ oluşturur *myVirtualNetwork* bir adres alanı ile *10.0.0.0/16* içinde *Doğu ABD* Azure bölge. Sanal ağ adlı bir alt ağ içeren *mySubnet* bir adres ön eki ile *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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

Ağ yapılandırma dosyasını dışarı aktardığınız hiç içerik içeriyorsa, önceki örnekte XML'ini kopyalayın ve yeni bir dosyaya yapıştırın.

### <a name="example-json-for-use-with-the-classic-cli"></a>Örnek JSON Klasik CLI ile kullanmak için

Aşağıdaki örnek ağ yapılandırma dosyası adlı bir sanal ağ oluşturur *myVirtualNetwork* bir adres alanı ile *10.0.0.0/16* içinde *Doğu ABD* Azure bölge. Sanal ağ adlı bir alt ağ içeren *mySubnet* bir adres ön eki ile *10.0.0.0/24*.

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

Ağ yapılandırma dosyasını dışarı aktardığınız hiç içerik içeriyorsa önceki örnek JSON dosyasını kopyalayın ve yeni bir dosyaya yapıştırın.

## <a name="import"></a>Bir ağ yapılandırma dosyasını içe aktarın

Bir ağ yapılandırma dosyasını içeri aktarmak için PowerShell veya Klasik CLI'yı kullanabilirsiniz. PowerShell Klasik CLI bir json dosyası içeri aktarırken bir XML dosyasını içeri aktarır. İçeri aktarma başarısız olursa, dosyanın uygun onaylayın [ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell'i yükleyin ve Azure'da oturum açma](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Ardından ağ yapılandırma dosyasını içeri aktarma komutu çalıştırın dizin ve dosya adı olduğu aşağıdaki komutta değiştirin:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-classic-cli"></a>Azure klasik CLI

1. [Klasik Azure CLI yükleme](/cli/azure/install-classic-cli). Klasik CLI komut isteminde kalan adımları tamamlayın.
2. Azure'da girerek oturum açma `azure login` komutu.
3. Asm moduna girerek emin olun `azure config mode asm` komutu.
4. Ardından ağ yapılandırma dosyasını içeri aktarma komutu çalıştırın dizin ve dosya adı olduğu aşağıdaki komutta değiştirin:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
