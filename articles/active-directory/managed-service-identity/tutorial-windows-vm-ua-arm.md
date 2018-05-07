---
title: Azure Resource Manager erişmek için bir Windows VM kullanıcıya MSI atanan kullanın
description: Azure Resource Manager erişmek için bir Windows VM üzerinde bir kullanıcı atanan yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: bdd7966721b22d75023c593593e69ab651b3aaca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Bir kullanıcı atanan yönetilen hizmet kimliği (MSI), Azure Resource Manager erişmek için bir Windows VM üzerinde kullanın.

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici, bir kullanıcı kimliği atanır oluşturmak, bir Windows sanal makine (VM) atayın ve Azure Kaynak Yöneticisi API'si erişmek için kimliğini kullanın açıklanmaktadır. Yönetilen hizmet kimliği, Azure tarafından otomatik olarak yönetilir. Bunlar Azure AD kimlik doğrulaması, kimlik bilgileri kodunuza katıştırmak gerek kalmadan destekleyen hizmetler için kimlik doğrulaması sağlar. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Windows VM oluşturma 
> * Bir kullanıcı kimliği atanır oluşturun
> * Windows VM'nizi kimliği atanır, kullanıcı atama
> * Atanan kullanıcı kimliğini bir kaynak grubu Azure Kaynak Yöneticisi'nde erişim 
> * Kullanıcı kimliği atanır kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın 
> * Bir kaynak grubunun özelliklerini okuma

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile tanınmayan olduğunuz kullanıma [genel bakış](overview.md) bölümü. **Gözden geçirmeyi unutmayın [sistem ve kullanıcı arasındaki farklar atanan kimlikleri](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Bu öğreticide gerekli kaynak oluşturulması ve rol yönetimi adımları gerçekleştirmek için hesabınızı (abonelik veya kaynak grubunuz) uygun kapsamda "Sahip" izinleri gerekiyor. Rol ataması yardıma ihtiyacınız varsa bkz [Azure aboneliği kaynaklarınıza erişimi yönetmek üzere Use Role-Based erişim denetimi](/azure/role-based-access-control/role-assignments-portal).

Yükleme ve yerel olarak PowerShell kullanma seçerseniz, bu öğreticide Azure PowerShell modülü sürümü 5.7 veya üstünün gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki örnekte, bir kaynak grubu adında, *myResourceGroupVM* oluşturulan *EastUS* bölge.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Kaynak grubu oluşturulduktan sonra bir Windows VM oluşturun.

[Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile sanal makinede yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```
[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="create-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır oluşturun

Bir kullanıcı kimliği atanır tek başına Azure kaynak olarak oluşturulur. Kullanarak [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity), Azure, Azure AD kiracınızda bir veya daha fazla Azure hizmet örneklerine atanabilir bir kimlik oluşturur.

> [!IMPORTANT]
> Atanan kullanıcı kimlikleri yalnızca destekler alfasayısal oluşturma ve tire (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Geri güncelleştirmeleri denetleyin. Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Yanıt kimlik oluşturulan, aşağıdaki örneğe benzer şekilde atanmış kullanıcı ayrıntılarını içerir. Not `Id` sonraki adımda kullanılacak olan olarak atanan kullanıcı kimliğinizi için değer:

```azurepowershell
{
Id: /subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1
ResourceGroupName : myResourceGroupVM
Name: ID1
Location: westus
TenantId: 733a8f0e-ec41-4e69-8ad8-971fc4b533f8
PrincipalId: e591178e-b785-43c8-95d2-1397559b2fb9
ClientId: af825a31-b0e0-471f-baea-96de555632f9
ClientSecretUrl: https://control-westus.identity.azure.net/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1/credentials?tid=733a8f0e-ec41-4e69-8ad8-971fc4b533f8&oid=e591178e-b785-43c8-95d2-1397559b2fb9&aid=af825a31-b0e0-471f-baea-96de555632f9
Type: Microsoft.ManagedIdentity/userAssignedIdentities
}
```

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Kimlik bir Windows VM'ye atanan kullanıcı atama

Bir kullanıcı kimliği atanır, birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Tek bir VM'ye atanan kullanıcı kimliğini atamak için aşağıdaki komutları kullanın. Kullanım `Id` özelliği döndürülen için önceki adımda `-IdentityID` parametresi.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Kullanıcı bir kaynak grubu Azure Kaynak Yöneticisi'nde MSI erişim atanmış verin 

Yönetilen hizmet kimliği (MSI) kodunuzu kaynak API'leri, destek Azure AD kimlik doğrulaması kimlik doğrulaması için erişim belirteci istemek için kullanabileceği kimlikleri sağlar. Bu öğreticide, Azure Resource Manager API kodunuzu erişir. 

Kodunuzu API erişebilmeniz için önce bir kaynağa Azure Kaynak Yöneticisi'nde kimlik erişim vermeniz gerekir. Bu durumda, kaynak grubu VM yer alır. Değeri güncelleştirme `<SUBSCRIPTION ID>` ortamınız için uygun şekilde.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Yanıt oluşturulan, aşağıdaki örneğe benzer şekilde rol ataması ayrıntılarını içerir:

```azurepowershell
RoleAssignmentId: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM/providers/Microsoft.Authorization/roleAssignments/f9cc753d-265e-4434-ae19-0c3e2ead62ac
Scope: /subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/myResourceGroupVM
DisplayName: ID1
SignInName:
RoleDefinitionName: Reader
RoleDefinitionId: acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId: e591178e-b785-43c8-95d2-1397559b2fb9
ObjectType: ServicePrincipal
CanDelegate: False
```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Resource Manager çağırmak için kullanın 

Öğretici kalanı için daha önce oluşturduğumuz sanal makineden çalışmaz.

1. Azure portalında oturum açın [https://portal.azure.com](https://portal.azure.com)

2. Portalı'nda gidin **sanal makineleri** ve Windows sanal makineye gidin ve buna **genel bakış**, tıklatın **Bağlan**.

3. Girin **kullanıcıadı** ve **parola** Windows VM oluştururken kullandığınız.

4. Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle açmak **PowerShell** uzak oturumda.

5. PowerShell'in kullanarak `Invoke-WebRequest`, Azure kaynak yöneticisi için bir erişim belirteci almak üzere yerel MSI uç nokta için bir istek olun.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Bir kaynak grubunun özelliklerini okuma

Erişim belirteci Azure Resource Manager erişmek ve kaynak özelliklerini okumak için önceki adımda alınan kullanım grubunu verilen atanan kullanıcı kimliğini erişim. Değiştir <SUBSCRIPTION ID> ortamınızı abonelik kimliği.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Yanıt belirli kaynak grubu, aşağıdaki örneğe benzer bilgiler içerir:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
