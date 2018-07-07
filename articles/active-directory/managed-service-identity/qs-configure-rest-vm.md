---
title: Sistem ve kullanıcı yapılandırma tarafından atanan kimliklerle REST kullanarak Azure sanal makinesinde
description: Adım adım sistem ve kullanıcı yapılandırmaya yönelik yönergeler REST API çağrıları gerçekleştirmek için CURL kullanarak bir Azure sanal makinesinde kimlikleri atanır.
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
ms.openlocfilehash: 9cc645d91bebf07ed7e657ed39c2454254958959
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902896"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>REST API çağrıları kullanarak bir Azure sanal makinesinde yönetilen kimlik yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimlik otomatik olarak yönetilen sistem kimliği Azure Active Directory'de Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir Azure sanal makinesinde aşağıdaki yönetilen kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:

- Etkinleştirme ve sistem tarafından atanan kimlik Azure sanal makinesinde devre dışı
- Bir kullanıcı tarafından atanan kimliği Azure VM'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Windows kullanıyorsanız, yükleme [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalında.
- [Azure CLI'yı yerel konsolunu yükleme](/azure/install-azure-cli), kullanırsanız [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım işletim sistemi](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Azure CLI'yı yerel Konsolu kullanıyorsanız, Azure kullanarak oturum açın `az login` Azure ile ilişkili olan bir hesapla kimlik sistem veya kullanıcı yönetmek istediğiniz aboneliği atanmış.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirme ve sistem tarafından atanan kimlik Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir Azure sanal makinesinde devre dışı öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

Etkin kimlik atanan sistemi ile bir Azure VM oluşturmak için bir VM oluşturun ve sistem tarafından atanan kimlik türü değeri ile Resource Manager uç noktasını çağırmak için CURL kullanmak için bir erişim belirteci almak.

1. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve VM'nizi ve ilgili kaynaklarını kullanarak, dağıtım için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) VM'niz için:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* istek gövdesinde değeri tarafından tanımlandığı gibi bir sistem tarafından atanan kimlikle `"identity":{"type":"SystemAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Atanan kimliği mevcut bir Azure sanal makinesinde Sistemi'ni etkinleştir

Sistem tarafından atanan kimliği mevcut bir VM üzerinde etkinleştirmek için erişim belirteci alma ve sonra kimlik türü güncelleştirmek için Resource Manager REST uç noktasını çağırmak için CURL kullanın gerekir.

1. Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. İstek gövdesinde değeri tarafından tanımlandığı gibi sistem tarafından atanan kimlik vm'nizde etkinleştirmek için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın `{"identity":{"type":"SystemAssigned"}` adlı bir VM için *myVM*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
   
   > [!IMPORTANT]
   > Varolan bir kullanıcı tarafından atanan sanal Makineye atanmış olan yönetilen kimliklerle Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Tanımlandığı gibi VM'ye atanmış bir kullanıcı tarafından atanan kimlikleri varsa `identity` değerini yanıt olarak, sistem tarafından atanan kimlik vm'nizde etkinleştirirken kullanıcı tarafından atanan kimlikleri korumak nasıl oluşturulduğunu gösteren adım 3 bölümüne atlayın.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Sistem tarafından atanan kimliği bir VM üzerinde var olan bir kullanıcı tarafından atanan kimliklerle etkinleştirmek için eklemeniz gerekir `SystemAssigned` için `type` değeri.  
   
   Örneğin, sanal makinenize atanmış kullanıcı kimliklerini varsa `ID1` ve `ID2` atandığını ve sistem tarafından atanan kimlik VM'ye eklemek aşağıdaki CURL çağrısı kullanmak istersiniz. Değiştirin `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>` ortamınıza uygun değerlerle.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Bir Azure VM'den atanan kimliği sistemi devre dışı bırak

Mevcut bir VM üzerinde bir sistem tarafından atanan kimliği devre dışı bırakmak için erişim belirteci alma ve sonra kimlik türü için güncelleştirilecek Resource Manager REST uç noktasını çağırmak için CURL kullanın için ihtiyaç duyduğunuz `None`.

1. Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sistem tarafından atanan kimliği devre dışı bırakmak için Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak VM'yi güncelleştirin.  Sistem tarafından atanan kimlik aşağıdaki örnekte devre istek gövdesinde değeri tarafından tanımlanan dışı bırakan `{"identity":{"type":"None"}}` adlı bir VM'den *myVM*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   > [!IMPORTANT]
   > Varolan bir kullanıcı tarafından atanan sanal Makineye atanmış olan yönetilen kimliklerle Sil yoksa emin olmak için bu CURL komutu kullanarak kullanıcı tarafından atanan kimlikleri listesi gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Tanımlandığı gibi VM'ye atanmış bir kullanıcı tarafından atanan kimlikleri varsa `identity` değerini yanıt olarak, sistem tarafından atanan kimlik vm'nizde devre dışı bırakılırken kullanıcı tarafından atanan kimlikleri korumak nasıl oluşturulduğunu gösteren adım 3 bölümüne atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Sistem atanan kimliği kullanıcı tarafından atanan kimlikleri olan bir sanal makineden kaldırmak için `SystemAssigned` gelen `{"identity":{"type:" "}}` tutma bir değer `UserAssigned` değeri ve `identityIds` hangi kullanıcı tarafından atanan kimlikleri VM'ye atanan tanımlayan bir dizi.

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, kullanıcı tarafından atanan kimliği Azure Resource Manager REST uç noktasına çağrı yapmak için CURL kullanarak bir Azure sanal makinesinde ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği bir Azure VM oluşturma sırasında atama

1. Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Oluşturma bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) VM'niz için:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan kimliği oluşturma: [yönetilen kimlik atanmış bir kullanıcı oluşturmak](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Azure Resource Manager REST uç noktasını çağırmak için CURL kullanarak bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* kaynak grubundaki *myResourceGroup* bir kullanıcı tarafından atanan kimlikle `ID1`, istek gövdesinde değeri tarafından tanımlanan `"identity":{"type":"UserAssigned"}`. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir Azure VM'ye atayın

1. Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak bir kullanıcı tarafından atanan kimliği oluşturma [yönetilen kimlik atanmış bir kullanıcı oluşturmak](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Mevcut kullanıcı silme veya sistem tarafından atanan VM'ye atanmış olan yönetilen kimliklerle emin olmak için aşağıdaki CURL komutunu kullanarak sanal Makineye atanan kimlik türlerini listelemek gerekir. Sanal makine ölçek kümesine atanan kimlikleri yönetiliyorsa, bunlar altında listelenen `identity` değeri.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Herhangi bir kullanıcı veya sistem tarafından atanan kimlikleri tanımlandığı gibi VM'ye atanmış varsa `identity` değerini yanıt olarak, sistem tarafından atanan kimlik vm'nizde etkinleştirirken kullanıcı tarafından atanan kimlikleri korumak nasıl oluşturulduğunu gösteren adım 5 bölümüne atlayın.

4. Herhangi bir kullanıcı tarafından atanan kimliklerle VM'nize atanan yoksa, ilk kullanıcı tarafından VM'ye atanan kimliği atamak için Azure Resource Manager REST uç noktasını çağırmak için aşağıdaki CURL komutunu kullanın.  VM'ye atanmış identity(s) atanmış bir kullanıcı varsa, bir VM'ye birden çok kullanıcı tarafından atanan kimlikleri ekleme gösteren bir sonraki adıma atlayın.

   Aşağıdaki örnek, bir kullanıcı tarafından atanan kimlik atar `ID1` adlı bir sanal makineye *myVM* kaynak grubundaki *myResourceGroup*.  Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde önceki adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Kullanıcı veya sistem tarafından atanan kimlikleri VM'nize atanan varsa, yeni kullanıcı tarafından atanan kimliği eklemenize gerek `identityIDs` korurken aynı zamanda kullanıcı ve VM'ye atanmış olan sistem tarafından atanan kimlikleri dizisi.

   Kimlik ve kullanıcı tarafından atanan kimlik sistemine sahip bir örnek atanmış `ID1` şu anda VM'nize atanan ve kullanıcı kimliği eklemek istediğiniz `ID2` , aşağıdaki CURL komutunu kullanın. Değiştirin `<ACCESS TOKEN>` değeriyle bir taşıyıcı belirteç istendiğinde adımda alınan ve `<SUBSCRIPTION ID>` değeri ortamınız için uygun şekilde.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği bir Azure VM'den kaldırın

1. Yönetilen kimlik atanmış bir sistemiyle VM'nizi oluşturmak için yetkilendirme üst bilgisinde bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alır.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Varolan bir kullanıcı tarafından atanan sanal Makineye atanan tutun veya sistem tarafından atanan kimlik kaldırmak istediğiniz yönetilen kimliklerle silme emin olmak için aşağıdaki CURL komutunu kullanarak yönetilen kimlikleri listesi gerekir: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Sanal Makineye atanan kimlikleri yönetiliyorsa, yanıtta listelendikleri `identity` değeri.

   Örneğin, kullanıcı tarafından atanan kimlikleri varsa `ID1` ve `ID2` VM'nize atanan ve yalnızca almak istediğiniz `ID1` atanmış ve sistem tarafından atanan kimlik korumak, atanan kullanıcı atama olarak aynı CURL komutu kullanırsınız. kimlik yalnızca tutarak bir VM'ye yönetilen `ID1` tutun ve değer `SystemAssigned` değeri. Bu kaldırır `ID2` sistem tarafından atanan kimlik korurken VM'den kullanıcı tarafından atanan kimlik.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Sanal makinenize atanmış sistem ve kullanıcı tarafından atanan kimliklerle varsa, aşağıdaki komutu kullanarak atanmış sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Yalnızca kullanıcı tarafından atanan kimliklerle VM'niz varsa ve tüm bunları kaldırmak istiyor musunuz, aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Sonraki adımlar

Oluşturma, liste veya REST kullanarak atanmış kullanıcı silme hakkında bilgi için bkz:

- [Oluşturma, liste veya REST API çağrıları kullanarak bir kullanıcı tarafından atanan kimliği silme](how-to-manage-ua-identity-rest.md)