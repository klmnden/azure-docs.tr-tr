---
title: Kullanıcı bir VHD'yi bir Azure VM'den dağıtma | Microsoft Docs
description: Bir Azure VM örneği oluşturmak için bir kullanıcı VHD görüntüsü dağıtma işlemi açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 48be60a7ba5770f8c329cb6323a5caa8fcf7f961
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265065"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>VHD bir kullanıcıdan bir Azure VM dağıtma

Bu makalede, Azure PowerShell Betiği ve sağlanan Azure Resource Manager şablonu kullanarak yeni bir Azure VM kaynak oluşturmak için genelleştirilmiş bir VHD görüntüsünün nasıl dağıtılacağı açıklanmaktadır.


## <a name="vhd-deployment-template"></a>VHD dağıtım şablonu

Azure Resource Manager şablonu kopyalayın [VHD dağıtım](cpp-deploy-json-template.md) adlı yerel bir dosyaya `VHDtoImage.json`.  Aşağıdaki parametreler için değerler sağlamak için bu dosyayı düzenleyin. 

|  **Parametre**             |   **Açıklama**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Var olan Azure kaynak grubu adı.  Genellikle, key vault ile ilişkili aynı RG kullanın  |
| TemplateFile               | Dosyanın tam yol adı `VHDtoImage.json`                                    |
| userStorageAccountName     | Depolama hesabının adı                                                    |
| sNameForPublicIP           | Genel IP için DNS adı. Küçük olmalıdır                                  |
| subscriptionId             | Azure abonelik tanımlayıcısı                                                  |
| Konum                   | Kaynak grubunun Azure standart coğrafi konum                       |
| vmName                     | Sanal makinenin adı                                                    |
| VaultName                  | Anahtar kasasının adı                                                          |
| vaultResourceGroup         | Anahtar kasasının kaynak grubu
| certificateUrl             | Örneğin anahtar kasasında depolanan sürüm dahil olmak üzere sertifika URL'si:  `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | Sanal sabit disk URL'si                                                   |
| vmSize                     | Sanal makine örnek boyutu                                           |
| publicIPAddressName        | Genel IP adresinin adı                                                  |
| virtualNetworkName         | Sanal ağ adı                                                    |
| NIC adı                    | Sanal ağ için ağ arabirimi kartı adı                     |
| adminUserName              | Yönetici hesabının kullanıcı adı                                          |
| adminPassword              | Yönetici parolası                                                          |
|  |  |


## <a name="powershell-script"></a>PowerShell Betiği

Kopyalama ve değerlerini sağlamak için aşağıdaki betiği düzenleyin `$storageaccount` ve `$vhdUrl` değişkenleri.  Bir Azure VM kaynağı mevcut genelleştirilmiş VHD'den oluşturmak üzere yürütün.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizin dağıtıldıktan sonra hazır olduğunuz [VM görüntünüzü sertifika](./cpp-certify-vm.md).
