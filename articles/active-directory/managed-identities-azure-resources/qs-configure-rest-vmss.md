---
title: Sistem ve kullanıcı tarafından atanan yönetilen kimlikleri REST kullanarak bir Azure VMSS üzerinde yapılandırma
description: Adım adım bir sistem ve kullanıcı tarafından atanan yönetilen kimlikleri REST API'si yapmak için CURL kullanarak bir Azure VMSS üzerinde yapılandırma yönergeleri çağırır.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/25/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: cafb3c97befd64cc6413a2eefa5e5baa9e01bf93
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59009591"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Azure kaynakları için yönetilen kimlikleri REST API çağrıları kullanarak bir sanal makine ölçek kümesi üzerinde yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri otomatik olarak yönetilen sistem kimliği Azure Active Directory'de Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak, Azure kaynaklarını bir sanal makine ölçek kümesi işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

- Etkinleştirme ve devre dışı bir Azure sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik bir Azure sanal makine ölçek kümesinde ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:

    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.

    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturun ve etkinleştirin ve sistem ve/veya yönetilen kimlik kullanıcı tarafından atanan bir sanal makine ölçek kümesinden kaldırmak için.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- [Azure CLI'yı yerel konsolunu yükleme](/cli/azure/install-azure-cli), kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` Azure aboneliği ile ilişkili olan bir hesapla sistem veya kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istersiniz.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirme ve devre dışı yönetilen kimlik sistem tarafından atanan sanal makine ölçek kümesi Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak üzerinde öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Yönetilen kimlik sistem tarafından atanan bir sanal makine ölçek kümesi oluşturma sırasında etkinleştir

Sanal makine ölçek kümesi etkin sistem tarafından atanan yönetilen kimlik ile oluşturmak için bir sanal makine ölçek kümesi oluşturma ve sistem tarafından atanan yönetilen kimlik türü değeri ile Resource Manager uç noktasını çağırmak için CURL kullanmak için bir erişim belirteci almak.

1. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümenizi ve ilgili kaynaklarını kullanarak, dağıtım için [az grubu oluşturma](/cli/azure/group/#az-group-create). Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) için sanal makine ölçek kümesi:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Sanal makine ölçek kümesi Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak oluşturun. Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* içinde *myResourceGroup* bir sistem tarafından atanan ile kimliği, istek gövdesinde değeri tarafından tanımlandığı gibi yönetilen `"identity":{"type":"SystemAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }  
   ```  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği var olan bir sanal makine ölçek kümesi üzerinde yönetilen etkinleştir

Yönetilen kimlik sistemi atanmış mevcut bir sanal makine ölçek kümesi üzerinde etkinleştirmek için erişim belirteci alma ve sonra kimlik türü güncelleştirmek için Resource Manager REST uç noktasını çağırmak için CURL kullanın gerekir.

1. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Yönetilen kimlik sistem tarafından atanan sanal makine ölçek üzerinde etkinleştirmek için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutu, istek gövdesinde değeri tarafından tanımlanan kümesini kullanın `{"identity":{"type":"SystemAssigned"}` adlı bir sanal makine ölçek kümesi için*myVMSS*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
   
   > [!IMPORTANT]
   > Sanal makine ölçek kümesine atanmış olan tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Kullanıcı tarafından atanan kimlikleri tanımlandığı gibi sanal makine ölçek atanan varsa yönetilen `identity` değerini yanıt olarak, sistem tarafından atanan etkinleştirirken kullanıcı tarafından atanan yönetilen kimlikleri korumak nasıl oluşturulduğunu gösteren 3. adım için Atla sanal makine ölçek kümesinde yönetilen kimlik.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned"
       }
    }
   ```

3. Yönetilen kimlik sistem tarafından atanan kullanıcı tarafından atanan mevcut yönetilen kimliklerine sahip bir sanal makine ölçek etkinleştirmek için eklemeniz gerekir `SystemAssigned` için `type` değeri.  
   
   Sanal makine ölçek kümesi, örneğin, kullanıcı tarafından atanan kimlikleri yönettiği `ID1` ve `ID2` atandığını ve yönetilen kimlik sistem tarafından atanan sanal makine ölçek kümesine eklemek için aşağıdaki CURL çağrısı kullanmak istersiniz. Değiştirin `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>` ortamınıza uygun değerlerle.

   API sürümü `2018-06-01` kullanıcı tarafından atanan yönetilen kimliklerini depolar `userAssignedIdentities` başlangıcı yerine sonundan bir sözlük biçiminde bir değer `identityIds` API sürümünde kullanılan bir dizi biçiminde bir değer `2017-12-01`.
   
   **API SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. |
 
   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```
   
   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="disable-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Yönetilen kimlik sistem tarafından atanan sanal makine ölçek kümesinden devre dışı bırak

Mevcut bir sanal makine ölçek kümesi üzerinde bir sistem tarafından atanan kimliği devre dışı bırakmak için erişim belirteci alma ve sonra kimlik türü için güncelleştirilecek Resource Manager REST uç noktasını çağırmak için CURL kullanın için ihtiyaç duyduğunuz `None`.

1. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak yönetilen kimlik sistem tarafından atanan devre dışı bırakmak için sanal makine ölçek güncelleştirin.  Yönetilen kimlik sistem tarafından atanan aşağıdaki örnekte devre istek gövdesinde değeri tarafından tanımlanan dışı bırakan `{"identity":{"type":"None"}}` adlı bir sanal makine ölçek kümesi'nden *myVMSS*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   > [!IMPORTANT]
   > Sanal makine ölçek kümesine atanmış olan tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Sanal makine ölçek kümesine atanan kullanıcı tarafından atanan yönetilen kimlikleri varsa, sistem tarafından atanan bir yönetilen kimlik, sanal makine ölçek kümesinden kaldırılırken bir kullanıcı tarafından atanan yönetilen kimlikleri nasıl korunur gösteren adım 3 bölümüne atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"None"
       }
    }
   ```

   Yönetilen kimlik sistem tarafından atanan kullanıcı tarafından atanan yönetilen kimlikleri olan bir sanal makine ölçek kümesinden kaldırmak için `SystemAssigned` gelen `{"identity":{"type:" "}}` tutma bir değer `UserAssigned` değeri ve `userAssignedIdentities` sözlük değerler varsa, kullanmakta olduğunuz **API sürümü 2018-06-01**. Kullanıyorsanız **API Sürüm 2017-12-01** ya da daha önce koruma `identityIds` dizisi.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir sanal makine ölçek yönetilen kimlik kullanıcı tarafından atanan ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

1. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) için sanal makine ölçek kümesi:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturun: [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Sanal makine ölçek kümesi Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak oluşturun. Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* kaynak grubundaki *myResourceGroup* kullanıcı tarafından atanan bir yönetilen kimlikle `ID1`, istek gövdesinde tanımlanan değere göre `"identity":{"type":"UserAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
   **API SÜRÜMÜ 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```   

   **API SÜRÜM 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. |
 
   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
             "storageProfile":{
                "imageReference":{
                   "sku":"2016-Datacenter",
                   "publisher":"MicrosoftWindowsServer",
                   "version":"latest",
                   "offer":"WindowsServer"
                },
                "osDisk":{
                   "caching":"ReadWrite",
                   "managedDisk":{
                      "storageAccountType":"Standard_LRS"
                   },
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure sanal makine ölçek kümesine atama

1. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan yönetilen kimliği oluşturma [kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Varolan bir kullanıcı veya sistem tarafından atanan sanal makine ölçek kümesine atanmış olan yönetilen kimlikleri silme emin olmak için aşağıdaki CURL komutunu kullanarak sanal makine ölçek atanmış türleri ayarlanan kimlik listesi gerekir. Sanal makine ölçek kümesine atanan kimlikleri yönetiliyorsa listelendikleri `identity` değeri.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. |   
 

4. Herhangi bir kullanıcı yoksa veya sistem tarafından atanan yönetilen atanan için sanal makine ölçek kümesi, ilk kullanıcı tarafından atanan yönetilen kimlik kullanarak sanal makineye atamak için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın Ölçek kümesi.  Bir kullanıcı ya da sanal makine ölçek kümesine atanan yönetilen identity(s) sistem tarafından atanan varsa, birden çok kullanıcı tarafından atanan yönetilen kimlik sistem tarafından atanan yönetilen ayrıca korurken bir sanal makine ölçek kümesine ekleme gösteren adım 5 bölümüne atlayın kimlik.

   Aşağıdaki örnek, bir kullanıcı tarafından atanan yönetilen kimlik, atar `ID1` adlı bir sanal makine ölçek kümesi için *myVMSS* kaynak grubundaki *myResourceGroup*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   **API SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ``` 
    
   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```  

5. Varsa var olan bir kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen sanal makine ölçek kümenize atanan kimliği:
   
   **API SÜRÜMÜ 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliğe ekleme `userAssignedIdentities` sözlük değeri.

   Örneğin, yönetilen kimlik sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik varsa `ID1` şu anda sanal makine ölçek için atanan ve kullanıcı tarafından atanan bir yönetilen kimlik eklemek istediğiniz `ID2` ona:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   Tutmak istediğiniz kullanıcı tarafından atanan yönetilen kimlikleri korur `identityIds` dizi değeri kullanıcı tarafından atanan yeni yönetilen kimlik eklenirken.

   Örneğin, sistem tarafından atanan kimlik ve kullanıcı tarafından atanan bir yönetilen kimlik varsa `ID1` şu anda sanal makine ölçek kümenize atanan ve kullanıcı tarafından atanan bir yönetilen kimlik eklemek istediğiniz `ID2` ona:

    ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

    **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesinden bir kullanıcı tarafından atanan bir yönetilen kimlik Kaldır

1. Sistem tarafından atanan bir yönetilen kimlikle sanal makine ölçek kümenizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçek kümesine atanan tutun veya sistem tarafından atanan bir yönetilen kimlik kaldırmak istediğiniz tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri silme emin olmak için aşağıdaki CURL komutunu kullanarak yönetilen kimlikleri listesi gerekir :

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. |
   
   Sanal Makineye atanan kimlikleri yönetiliyorsa, yanıtta listelendikleri `identity` değeri. 
    
   Örneğin, kullanıcı tarafından atanan yönetilen kimlikleri varsa `ID1` ve `ID2` , sanal makine ölçek kümesine atanan ve yalnızca tutmak istiyorsanız `ID1` atanan ve sistem tarafından atanan bir yönetilen kimlik Beklet:

   **API SÜRÜMÜ 2018-06-01**

   Ekleme `null` yönetilen kimliği kaldırmak istediğiniz kullanıcı tarafından atanan için:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   Yalnızca kullanıcı tarafından atanan identity(s) tutmak istediğiniz yönetilen korumak `identityIds` dizisi:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

Sanal makine ölçek kümesi, hem de sistem tarafından atanan ve yönetilen kimlikleri, kullanıcı tarafından atanan tüm kullanmaya geçiş tarafından kullanıcı tarafından atanan yönetilen kimlikleri yalnızca sistem tarafından aşağıdaki komutu kullanarak atanan kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üstbilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"SystemAssigned"
   }
}
```
    
Sanal makine ölçek kümesi yönetilen kimlikleri yalnızca kullanıcı tarafından atanmış olan ve istediğiniz tüm bunları kaldırmak için aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üstbilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci. | 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"None"
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma, liste veya REST kullanarak yönetilen kimlikleri kullanıcı tarafından atanan silme hakkında bilgi için bkz:

- [Oluşturma, liste veya REST API çağrıları kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silme](how-to-manage-ua-identity-rest.md)
