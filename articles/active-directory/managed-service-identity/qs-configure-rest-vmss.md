---
title: Sistem ve kullanıcı yapılandırma tarafından atanan kimliklerle REST kullanarak bir Azure vmss'de
description: Adım adım bir sistem ve kullanıcı yapılandırmaya yönelik yönergeler, REST API çağrıları gerçekleştirmek için CURL kullanarak kimlikleri temelinde Azure VMSS atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: b590bb4f5eca0041cda97204b368de0da31620d0
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886268"
---
# <a name="configure-managed-identity-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Bir sanal makine REST API çağrıları kullanarak ölçek kümesi üzerinde yönetilen kimlik yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimlik otomatik olarak yönetilen sistem kimliği Azure Active Directory'de Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir sanal makine ölçek kümesinde aşağıdaki yönetilen kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:

- Etkinleştirme ve devre dışı bir Azure sanal makine ölçek kümesinde sistem tarafından atanan kimlik
- Bir Azure sanal makine ölçek kümesinde bir kullanıcı tarafından atanan kimliği ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) oluşturma bir sanal makine ölçek kümesi ve etkinleştir ve Kaldır sistem ve/veya kullanıcı bir sanal makine ölçek kümesinden yönetilen kimlik atanır.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- [Azure CLI'yı yerel konsolunu yükleme](/cli/azure/install-azure-cli), kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` Azure ile ilişkili olan bir hesapla kimlik sistem veya kullanıcı yönetmek istediğiniz aboneliği atanmış.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirme ve devre dışı sistem tarafından atanan kimliği üzerinde bir sanal makine ölçek kümesi Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan kimlik etkinleştir

Sanal makine ölçek kümesi etkin kimliği atanan sistemiyle oluşturmak için bir sanal makine ölçek kümesi oluşturma ve sistem tarafından atanan kimlik türü değeri ile Resource Manager uç noktasını çağırmak için CURL kullanmak için bir erişim belirteci almak.

1. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümenizi ve ilgili kaynaklarını kullanarak, dağıtım için [az grubu oluşturma](/cli/azure/group/#az-group-create). Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) için sanal makine ölçek kümesi:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Sanal makine ölçek kümesi Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak oluşturun. Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* içinde *myResourceGroup* istek gövdesinde değeri tarafından tanımlandığı gibi bir sistem tarafından atanan kimlikle `"identity":{"type":"SystemAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
    ```bash   
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesinde etkinleştirin

Sistem tarafından atanan kimliği var olan bir sanal makine ölçek kümesi üzerinde etkinleştirmek için erişim belirteci alma ve sonra kimlik türü güncelleştirmek için Resource Manager REST uç noktasını çağırmak için CURL kullanın gerekir.

1. Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. İstek gövdesinde değeri tarafından tanımlandığı gibi sistem tarafından atanan kimliği, sanal makine ölçek üzerinde etkinleştirmek için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutu kümesini kullanın `{"identity":{"type":"SystemAssigned"}` adlı bir sanal makine ölçek kümesi için  *myVMSS*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
   
   > [!IMPORTANT]
   > Varolan bir kullanıcı tarafından atanan sanal makine ölçek kümesine atanmış olan yönetilen kimliklerle Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Hiç atanan kullanıcı kimlik tanımlandığı gibi sanal makine ölçek atanan varsa `identity` değerini yanıt olarak, kullanıcı tarafından atanan kimlikleri sanal sistem tarafından atanan kimliği sağlarken korumak nasıl oluşturulduğunu gösteren 3. adım için Atla Makine ölçek kümesi.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Sistem tarafından atanan kimliği üzerinde bir sanal makine ölçek kümesi mevcut kullanıcı tarafından atanan kimliklerle etkinleştirmek için eklemeniz gerekir `SystemAssigned` için `type` değeri.  
   
   Sanal makine ölçek kümesi, kullanıcı tarafından atanan kimlikleri sahipse `ID1` ve `ID2` atanmış ve sanal makine ölçek kümesi için sistem tarafından atanan kimlik eklemek için aşağıdaki CURL çağrısı kullanmak istersiniz. Değiştirin `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>` ortamınıza uygun değerlerle.

   API sürümü `2018-06-01` atanan kullanıcı kimliklerini depolar `userAssignedIdentities` başlangıcı yerine sonundan bir sözlük biçiminde bir değer `identityIds` API sürümünde kullanılan bir dizi biçiminde bir değer `2017-12-01` ve önceki sürümleri.
   
   **API SÜRÜMÜ 2018-06-01**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```
   
   **API Sürüm 2017-12-01 ve önceki sürümleri**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden sistem tarafından atanan kimliği devre dışı

Mevcut bir sanal makine ölçek kümesi üzerinde bir sistem tarafından atanan kimliği devre dışı bırakmak için erişim belirteci alma ve sonra kimlik türü için güncelleştirilecek Resource Manager REST uç noktasını çağırmak için CURL kullanın için ihtiyaç duyduğunuz `None`.

1. Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak sistem tarafından atanan kimliği devre dışı bırakmak için sanal makine ölçek güncelleştirin.  Sistem tarafından atanan kimlik aşağıdaki örnekte devre istek gövdesinde değeri tarafından tanımlanan dışı bırakan `{"identity":{"type":"None"}}` adlı bir sanal makine ölçek kümesi'nden *myVMSS*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   > [!IMPORTANT]
   > Varolan bir kullanıcı tarafından atanan sanal makine ölçek kümesine atanmış olan yönetilen kimliklerle Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Hiçbir kullanıcı tarafından atanan sanal makine ölçek kümesine atanan kimliği varsa, sanal makine ölçek kümesinden atanan kimliği sistem kaldırılırken tarafından atanan kimliklerle kullanıcı nasıl korumak gösteren adım 3 bölümüne atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Sistem atanan kimliği kullanıcı tarafından atanan kimlikleri olan bir sanal makine ölçek kümesinden kaldırmak için `SystemAssigned` gelen `{"identity":{"type:" "}}` tutma bir değer `UserAssigned` değeri ve `userAssignedIdentities` sözlüğü değerlerini kullanıyorsanız**API sürümü 2018-06-01**. Kullanıyorsanız **API Sürüm 2017-12-01** ya da daha önce koruma `identityIds` dizisi.

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, kullanıcı tarafından atanan kimliği üzerinde bir sanal makine ölçek kümesi Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında bir kullanıcıya atanan kimlik atama

1. Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) için sanal makine ölçek kümesi:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan kimliği oluşturma: [yönetilen kimlik atanmış bir kullanıcı oluşturmak](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Sanal makine ölçek kümesi Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak oluşturun. Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* kaynak grubundaki *myResourceGroup* bir kullanıcı tarafından atanan kimlikle `ID1`, istek gövdesi tarafından belirlenen şekilde değer `"identity":{"type":"UserAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
   **API SÜRÜMÜ 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   **API Sürüm 2017-12-01 ve önceki sürümleri**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesine atama

1. Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan kimliği oluşturma [yönetilen kimlik atanmış bir kullanıcı oluşturmak](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Atanan sanal makine ölçek kümesine atanmış olan yönetilen kimliklerle var olan bir kullanıcı veya sistem silme emin olmak için aşağıdaki CURL komutunu kullanarak sanal makine ölçek atanmış türleri ayarlanan kimlik listesi gerekir. Sanal makine ölçek kümesine atanan kimlikleri yönetiliyorsa listelendikleri `identity` değeri.
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

4. Herhangi bir kullanıcı veya sistem tarafından atanan kimliklerle, sanal makine ölçek kümesine atanan yoksa, sanal makine ölçek kümesine atanan kimliği ilk kullanıcı atamak için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın.  Bir kullanıcı varsa veya sanal makine ölçek kümesine atanan identity(s) sistem atanan sanal makine ölçek kümesi aynı zamanda sistem tarafından atanan kimlik korurken birden çok kullanıcı tarafından atanan kimlikleri ekleme gösteren adım 5 bölümüne atlayın.

   Aşağıdaki örnek, bir kullanıcı tarafından atanan kimlik atar `ID1` adlı bir sanal makine ölçek kümesi için *myVMSS* kaynak grubundaki *myResourceGroup*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   **API SÜRÜMÜ 2018-06-01**

    ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   
    
   **API Sürüm 2017-12-01 ve önceki sürümleri**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Atanan varolan bir kullanıcı veya sistem tarafından atanan kimlik, sanal makine ölçek kümesine atanan varsa:
   
   **API SÜRÜMÜ 2018-06-01**

   Kullanıcı tarafından atanan kimliği ekleme `userAssignedIdentities` sözlük değeri.

   Varsa, sistem kimlik ve kullanıcı tarafından atanan kimlik atanan `ID1` şu anda sanal makine ölçek için atanan ve atanan kullanıcı kimliği eklemek istediğiniz `ID2` ona:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API Sürüm 2017-12-01 ve önceki sürümleri**

   Tutmak istediğiniz kullanıcı tarafından atanan kimlikleri korur `identityIds` dizi değeri eklenirken yeni kullanıcı tarafından atanan kimlik.

   Varsa, sistem kimlik ve kullanıcı tarafından atanan kimlik atanan `ID1` şu anda sanal makine ölçek kümenize atanan ve kullanıcı kimliği eklemek istediğiniz `ID2` ona: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Bir kullanıcı tarafından atanan kimliği bir sanal makine ölçek kümesinden Kaldır

1. Yönetilen kimlik atanmış bir sistemi ile sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Varolan bir kullanıcı tarafından atanan sanal makine ölçek kümesine atanan tutun veya sistem tarafından atanan kimlik kaldırmak istediğiniz yönetilen kimliklerle silme emin olmak için aşağıdaki CURL komutunu kullanarak yönetilen kimlikleri listesi gerekir: 
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```
   
   Sanal Makineye atanan kimlikleri yönetiliyorsa, yanıtta listelendikleri `identity` değeri. 
    
   Örneğin, kullanıcı tarafından atanan kimlikleri varsa `ID1` ve `ID2` , sanal makine ölçek kümesine atanan ve yalnızca tutmak istiyorsanız `ID1` atanan ve atanan sistem kimliğini Koru:

   **API SÜRÜMÜ 2018-06-01**

   Ekleme `null` kaldırmak istediğiniz kullanıcı tarafından atanan kimliği:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API Sürüm 2017-12-01 ve önceki sürümleri**

   Yalnızca kullanıcı tutmak istediğiniz identity(s) atandığı korumak `identityIds` dizisi:   

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Sanal makine ölçek kümenize atanan sistem ve kullanıcı tarafından atanan kimliklerle varsa, aşağıdaki komutu kullanarak atanmış sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Yalnızca kullanıcı tarafından atanan kimliklerle sanal makine ölçek kümeniz varsa ve tüm bunları kaldırmak istiyor musunuz, aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma, liste veya REST kullanarak atanmış kullanıcı silme hakkında bilgi için bkz:

- [Oluşturma, liste veya REST API çağrıları kullanarak bir kullanıcı tarafından atanan kimliği silme](how-to-manage-ua-identity-rest.md)
