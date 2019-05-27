---
title: REST kullanarak bir Azure sanal makinesinde sistem ve kullanıcı tarafından atanan yönetilen kimlik yapılandırma
description: Adım adım bir sistem ve kullanıcı tarafından atanan yönetilen kimlikleri REST API'si yapmak için CURL kullanarak bir Azure sanal makinesinde yapılandırmaya yönelik yönergeler çağırır.
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
ms.openlocfilehash: 18350337ba44d969173d518a4bc8dfe40185de21
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66112722"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-rest-api-calls"></a>REST API çağrıları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri otomatik olarak yönetilen sistem kimliği Azure Active Directory'de Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak, Azure sanal makinesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

- Enable ve disable Azure VM'de sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik Azure VM'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- [Azure CLI'yı yerel konsolunu yükleme](/cli/azure/install-azure-cli), kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` Azure aboneliği ile ilişkili olan bir hesapla sistem veya kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istersiniz.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirme ve devre dışı yönetilen kimlik sistem tarafından atanan Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir Azure sanal makinesinde öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Etkin sistem tarafından atanan yönetilen kimlik ile bir Azure VM oluşturmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az-group-create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) VM'niz için:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* bir sistem tarafından atanan ile kimliği, istek gövdesinde değeri tarafından tanımlandığı gibi yönetilen `"identity":{"type":"SystemAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"<SECURE PASSWORD STRING>"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**
   
   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 
   
   **İstek gövdesi**

   ```JSON
     {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
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
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }  
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan yönetilen kimlik olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. İstek gövdesinde değeri tarafından tanımlandığı gibi sistem tarafından atanan yönetilen kimlik vm'nizde etkinleştirmek için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın `{"identity":{"type":"SystemAssigned"}` adlı bir VM için *myVM*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
   
   > [!IMPORTANT]
   > VM'ye atanmış olan tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Kullanıcı tarafından atanan kimlikleri tanımlandığı gibi VM'ye atanmış varsa yönetilen `identity` değerini yanıt olarak, sistem tarafından atanan yönetilen kimlik vm'nizde etkinleştirirken kullanıcı tarafından atanan yönetilen kimlikleri korumak nasıl oluşturulduğunu gösteren adım 3 bölümüne atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 
   
   **İstek gövdesi**
    
   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned"
       }
    }
   ```

3. Sistem tarafından atanan yönetilen kimlikle bir VM üzerinde kullanıcı tarafından atanan mevcut yönetilen kimliklerini etkinleştirmek için eklemeniz gerekir `SystemAssigned` için `type` değeri.  
   
   Örneğin, kullanıcı tarafından atanan bir yönetilen kimlik VM'niz varsa `ID1` ve `ID2` atandığını ve VM'ye yönetilen kimlik sistem tarafından atanan eklemek için aşağıdaki CURL çağrısı kullanmak istersiniz. Değiştirin `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>` ortamınıza uygun değerlerle.

   API sürümü `2018-06-01` kullanıcı tarafından atanan yönetilen kimliklerini depolar `userAssignedIdentities` başlangıcı yerine sonundan bir sözlük biçiminde bir değer `identityIds` API sürümünde kullanılan bir dizi biçiminde bir değer `2017-12-01`.
   
   **API SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{  
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{  
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
    
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```   

### <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Yönetilen kimlik sistem tarafından atanan bir Azure VM'den devre dışı bırak

Yönetilen kimlik sistem tarafından atanan bir VM'de devre dışı bırakmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Yönetilen kimlik sistem tarafından atanan devre dışı bırakmak için Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak VM'yi güncelleştirin.  Yönetilen kimlik sistem tarafından atanan aşağıdaki örnekte devre istek gövdesinde değeri tarafından tanımlanan dışı bırakan `{"identity":{"type":"None"}}` adlı bir VM'den *myVM*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   > [!IMPORTANT]
   > VM'ye atanmış olan tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan yönetilen kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Kullanıcı tarafından atanan kimlikleri tanımlandığı gibi VM'ye atanmış varsa yönetilen `identity` değerini yanıt olarak, sistem tarafından atanan yönetilen kimlik vm'nizde devre dışı bırakılırken kullanıcı tarafından atanan yönetilen kimlikleri korumak nasıl oluşturulduğunu gösteren adım 3 bölümüne atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"None"
       }
    }
   ```

   Yönetilen kimlik sistem tarafından atanan kullanıcı tarafından atanan yönetilen kimlikleri olan bir sanal makineden kaldırmak için `SystemAssigned` gelen `{"identity":{"type:" "}}` tutma bir değer `UserAssigned` değeri ve `userAssignedIdentities` sözlüğü değerlerini kullanıyorsanız **API sürümü 2018-06-01**. Kullanıyorsanız **API Sürüm 2017-12-01** ya da daha önce koruma `identityIds` dizisi.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir Azure sanal makinesinde yönetilen kimlik kullanıcı tarafından atanan ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) VM'niz için:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturun: [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kaynak grubundaki *myResourceGroup* kullanıcı tarafından atanan bir yönetilen kimlikle `ID1`, istek gövdesinde değeritarafındantanımlanan`"identity":{"type":"UserAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
   **API SÜRÜMÜ 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
    
   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "location":"westus",
       "name":"myVM",
       "identity":{  
          "type":"UserAssigned",
          "identityIds":[  
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{  
          "hardwareProfile":{  
             "vmSize":"Standard_D2_v2"
          },
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
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[  
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{  
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{  
             "networkInterfaces":[  
                {  
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{  
                      "primary":true
                   }
                }
             ]
          }
       }
    }

   ```
  
   **API SÜRÜM 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
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
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan yönetilen kimliği oluşturma [kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Varolan bir kullanıcı veya sistem tarafından atanan sanal Makineye atanmış olan yönetilen kimlikleri silme emin olmak için aşağıdaki CURL komutunu kullanarak sanal Makineye atanan kimlik türlerini listelemek gerekir. Sanal makine ölçek kümesine atanan kimlikleri yönetiliyorsa, bunlar altında listelenen `identity` değeri.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.

    Herhangi bir kullanıcı veya sistem tarafından atanan yönetilen kimlikleri tanımlandığı gibi VM'ye atanmış varsa `identity` değerini yanıt olarak, sistem tarafından atanan bir yönetilen kimlik üzerinde kullanıcı tarafından atanan bir yönetilen kimlik eklenirken korumak nasıl oluşturulduğunu gösteren 5. adım için Atla Sanal makinenizin.

4. VM'nize atanan bir kullanıcı tarafından atanan yönetilen kimlikleri yoksa, kullanıcı tarafından atanan ilk yönetilen kimlik sanal Makineye atamak için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın.

   Aşağıdaki örnekler, bir kullanıcı tarafından atanan yönetilen kimlik, atar `ID1` adlı bir sanal makineye *myVM* kaynak grubundaki *myResourceGroup*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   **API SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        |
 
   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ```

   **API SÜRÜM 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

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

5. Mevcut bir kullanıcı tarafından atanan veya sistem tarafından atanan varsa yönetilen VM'nize atanan kimliği:
   
   **API SÜRÜMÜ 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliğe ekleme `userAssignedIdentities` sözlük değeri.
    
   Örneğin, yönetilen kimlik sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik varsa `ID1` şu anda VM'nize atanan ve kullanıcı tarafından atanan bir yönetilen kimlik eklemek istediğiniz `ID2` ona:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

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

   Örneğin, yönetilen kimlik sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik varsa `ID1` şu anda VM'nize atanan ve kullanıcı tarafından atanan bir yönetilen kimlik eklemek istediğiniz `ID2` ona: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

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

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir Azure VM'den kaldırın.

Bir VM için bir kullanıcı tarafından atanan kimliği kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.

1. Sistem tarafından atanan bir yönetilen kimlikle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. VM'ye atanmış tutun veya sistem tarafından atanan bir yönetilen kimlik kaldırmak istediğiniz tüm mevcut kullanıcı tarafından atanan yönetilen kimlikleri silme emin olmak için aşağıdaki CURL komutunu kullanarak yönetilen kimlikleri listesi gerekir: 

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.
 
   Sanal Makineye atanan kimlikleri yönetiliyorsa, yanıtta listelendikleri `identity` değeri.

   Örneğin, kullanıcı tarafından atanan yönetilen kimlikleri varsa `ID1` ve `ID2` VM'nize atanan ve yalnızca tutmak istiyorsanız `ID1` atanan ve sistem tarafından atanan kimliğini Koru:
   
   **API SÜRÜMÜ 2018-06-01**

   Ekleme `null` yönetilen kimliği kaldırmak istediğiniz kullanıcı tarafından atanan için:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

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
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üstbilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

Sistem tarafından atanan sanal makinenizin sahiptir ve yönetilen kimlikleri, kullanıcı tarafından atanan tüm kullanıcı tarafından atanan yönetilen kimlikleri aşağıdaki komutu kullanarak yalnızca sistem tarafından atanan yönetilen kimliği kullanma geçerek kaldırabilirsiniz varsa:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
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
    
Yönetilen kimlikleri yalnızca kullanıcı tarafından atanan VM'niz varsa ve tüm bunları kaldırmak istiyor musunuz, aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üstbilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gereklidir. Kümesine `application/json`.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir kümesi `Bearer` erişim belirteci.| 

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

- [Oluşturma, liste veya REST API çağrıları kullanan bir kullanıcı tarafından atanan yönetilen kimlikleri silme](how-to-manage-ua-identity-rest.md)
