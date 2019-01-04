---
ms.assetid: ''
title: Azure Key Vault güvenlik duvarları ve sanal ağları - Azure Key Vault yapılandırma
description: Key Vault güvenlik duvarları ve sanal ağları yapılandırmak için adım adım yönergeler
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: 09a19b92a496650f94be208d4f463f1fb3fa4256
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001864"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault güvenlik duvarları ve sanal ağları yapılandırma

Bu makalede, Azure Key Vault güvenlik duvarları ve anahtar kasanız için erişimi kısıtlamak için sanal ağları yapılandırmak için adım adım yönergeler sağlar. [Sanal ağ hizmet uç noktaları için Key Vault](key-vault-overview-vnet-service-endpoints.md) , belirtilen sanal ağ ve IPv4 (internet Protokolü sürüm 4) adres aralıkları kümesini erişimini sınırlamanıza olanak tanır.

> [!IMPORTANT]
> Güvenlik duvarı kuralları etkin olduktan sonra kullanıcılar yalnızca Key Vault gerçekleştirebilir [veri düzlemi](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) isteklerinde kaynaklanan olduğunda işlemlerine izin sanal ağlara veya IPv4 adres aralıklarını. Bu, aynı zamanda Key Vault Azure portalından erişmek için geçerlidir. Kullanıcılar, Azure portalından bir anahtar Kasası'na göz atabilir olsa da, kendi istemci makine izin verilenler listesinde değilse, bunlar liste anahtarlarını, gizli dizileri veya sertifikaları mümkün olmayabilir. Bu anahtar kasası Seçici tarafından diğer Azure hizmetleri de etkiler. Kullanıcılar anahtar kasalarının listesi bakın, ancak güvenlik duvarı kurallarını istemci makine engelliyorsa anahtarları listesi değil mümkün olabilir.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalını kullanarak Key Vault güvenlik duvarları ve sanal ağları yapılandırma aşağıda verilmiştir:

1. Korumak istediğiniz bir anahtar kasasına göz atın.
2. Seçin **güvenlik duvarları ve sanal ağlar**.
3. Altında **erişime izin verecek**seçin **seçili ağlar**.
4. Var olan sanal ağları güvenlik duvarları ve sanal ağ kuralları eklemek için seçin **+ var olan sanal ağları Ekle**.
5. Açılan yeni dikey pencerede, abonelik, sanal ağlar ve bu anahtar kasasına erişim vermek istediğiniz alt ağları seçin. Seçtiğiniz alt ağlar ve sanal ağ hizmet uç noktaları etkinleştirilmiş yoksa, hizmet uç noktalarının etkinleştirilmesi ve istediğinizi onaylayın **etkinleştirme**. İşlem etkili olması 15 dakika kadar sürebilir.
6. Altında **IP ağları**, IPv4 adres aralıklarını yazarak IPv4 adres aralıklarını ekleyin [(sınıfsız etki alanları arası yönlendirme) CIDR gösteriminde](https://tools.ietf.org/html/rfc4632) veya tek IP adresleri.
7. **Kaydet**’i seçin.

Ayrıca yeni sanal ağlar ve alt ağlar ekleyin ve ardından yeni oluşturulan sanal ağlar ve alt ağlar için hizmet uç noktası seçerek etkinleştirin **+ yeni sanal ağ Ekle**. Ardından yönergeleri izleyin.

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 kullanma

Azure CLI 2.0 kullanarak anahtar kasası güvenlik duvarları ve sanal ağları yapılandırma aşağıda verilmiştir:

1. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [oturum](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Kullanılabilir sanal ağ kuralları listesi. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız listesi boş olur.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde anahtar kasası için etkinleştirin.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Bir IP adresi aralığı, trafiğe izin verecek şekilde ekleyin.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gereken verilirse `bypass` için `AzureServices`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Varsayılan eylem ayarlayarak ağ kurallarını açma `Deny`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

PowerShell kullanarak Key Vault güvenlik duvarları ve sanal ağları yapılandırma aşağıda verilmiştir:

1. Son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps), ve [oturum](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Kullanılabilir sanal ağ kuralları listesi. Bu anahtar kasası için herhangi bir kuralın ayarlamadıysanız listesi boş olur.
   ```PowerShell
   (Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Hizmet uç noktası, bir var olan sanal ağı ve alt ağ üzerinde anahtar kasası için etkinleştirin.
   ```PowerShell
   Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
   ```

4. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.
   ```PowerShell
   $subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Bir IP adresi aralığı, trafiğe izin verecek şekilde ekleyin.
   ```PowerShell
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Bu anahtar kasası güvenilir hizmetlerin tarafından erişilebilir olması gereken verilirse `bypass` için `AzureServices`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Varsayılan eylem ayarlayarak ağ kurallarını açma `Deny`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Başvurular

* Azure CLI 2.0 komutlarını: [az keyvault ağ-rule](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell cmdlet'leri: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [ekleme AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [AzureRmKeyVaultNetworkRuleSet güncelleştirme](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ hizmet uç noktaları için Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md)