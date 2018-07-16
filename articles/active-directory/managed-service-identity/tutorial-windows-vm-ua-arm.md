---
title: Azure Resource Manager’a erişmek için Windows VM kullanıcı tarafından atanan MSI'sini kullanma
description: Windows VM üzerinde bir Kullanıcı Tarafından Atanmış Yönetilen Hizmet Kimliği (MSI) kullanarak Azure Resource Manager'a erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: 67bb45f7bd27a142b978bedb48925cc41e8d1287
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904382"
---
# <a name="tutorial-use-a-user-assigned-managed-service-identity-msi-on-a-windows-vm-to-access-azure-resource-manager"></a>Öğretici: Azure Resource Manager’a erişmek için Windows VM üzerinde Kullanıcı Tarafından Atanmış Yönetilen Hizmet Kimliği (MSI) kullanma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide, kullanıcı tarafından atanan kimliği oluşturma, bunu Windows Sanal Makinesine (VM) atama ve bu kimliği Azure Resource Manager API’sine erişmek için kullanma işlemleri açıklanır. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir. Bunlar, kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmaya olanak tanır. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Windows VM oluşturma 
> * Kullanıcı tarafından atanan kimliği oluşturma
> * Kullanıcı tarafından atanan kimliğinizi Windows VM’nize atama
> * Azure Resource Manager’da Kaynak Grubuna kullanıcı tarafından atanan kimlik için erişim verme 
> * Kullanıcı tarafından atanan kimliği kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma 
> * Kaynak Grubunun özelliklerini okuma

## <a name="prerequisites"></a>Ön koşullar

- Yönetilen Hizmet Kimliği'ni bilmiyorsanız, [genel bakış](overview.md) bölümünü gözden geçirin. **[Sistem ve kullanıcı tarafından atanan kimlikler arasındaki farklılıkları](overview.md#how-does-it-work) gözden geçirmeyi unutmayın**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu öğreticideki gerekli kaynak oluşturma ve rol yönetimini adımlarını gerçekleştirmek için hesabınız uygun kapsamda (aboneliğiniz veya kaynak grubunuz) "Sahip" izinlerini gerektiriyor. Rol atamayla ilgili yardıma ihtiyacınız varsa bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için Rol Tabanlı Erişim Denetimi kullanma](/azure/role-based-access-control/role-assignments-portal).

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.7 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bu örnekte, *EastUS* bölgesinde *myResourceGroupVM* adlı bir kaynak grubu oluşturulur.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Kaynak grubu oluşturulduktan sonra Windows VM'sini oluşturun.

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

## <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği oluşturma

Kullanıcı tarafından atanan kimlik tek başına bir Azure kaynağı olarak oluşturulur. [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) kullanarak, Azure AD kiracınızda Azure bir veya daha fazla Azure hizmeti örneğine atanabilen bir kimlik oluşturur.

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1
```

Yanıt, aşağıdaki örneğe benzer biçimde, oluşturulmuş kullanıcı tarafından atanan kimliğin ayrıntılarını içerir. Kullanıcı tarafından atanan kimliğinizin `Id` değerini not alın, çünkü bu değer sonraki adımda kullanılacaktır:

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

## <a name="assign-the-user-assigned-identity-to-a-windows-vm"></a>Windows VM’sine kullanıcı tarafından atanan kimliği atama

Kullanıcı tarafından atanan kimlik, istemciler tarafından birden çok Azure kaynağında kullanılabilir. Aşağıdaki komutları kullanarak kullanıcı tarafından atanan kimliği tek bir VM'ye atayın. `-IdentityID` parametresi için önceki adımda döndürülen `Id` özelliğini kullanın.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVM -ResourceGroupName TestRG -VM $vm -IdentityType "UserAssigned" -IdentityID "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Azure Resource Manager’da Kaynak Grubu’na kullanıcı tarafından atanmış MSI erişiminizi verme 

Yönetilen Hizmet Kimliği (MSI), kodunuzun Azure AD kimlik doğrulamasını destekleyen kaynak API'lerinde kimlik doğrulaması yapmak amacıyla erişim belirteçleri istemek için kullanabileceği kimlikleri sağlar. Bu öğreticide, kodunuz Azure Resource Manager API’sine erişir. 

Kodunuzun API'ye erişebilmesi için önce Azure Resource Manager'da kaynağa kimlik erişimi vermeniz gerekir. Bu durumda, içinde VM'nin yer aldığı Kaynak Grubudur. `<SUBSCRIPTION ID>` değerini ortamınıza uyacak şekilde güncelleştirin.

```azurepowershell-interactive
$spID = (Get-AzureRmUserAssignedIdentity -ResourceGroupName myResourceGroupVM -Name ID1).principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<SUBSCRIPTIONID>/resourcegroups/myResourceGroupVM/"
```

Yanıt, aşağıdaki örneğe benzer biçimde, oluşturulan rol atamasının ayrıntılarını içerir:

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

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>VM kimliğini kullanarak erişim belirteci alma ve Resource Manager çağrısı yapmak için bunu kullanma 

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacaksınız.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

2. Portalda, **Sanal Makineler**'e ve Windows sanal makinesine gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın.

3. Windows VM'sini oluştururken kullandığınız **Kullanıcı adı** ve **Parola**’yı girin.

4. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda **PowerShell**'i açın.

5. PowerShell’in `Invoke-WebRequest` komutunu kullanarak, yerel MSI uç noktasına Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

    ```azurepowershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&client_id=73444643-8088-4d70-9532-c3a0fdc190fz&resource=https://management.azure.com' -Method GET -Headers @{Metadata="true"}
    $content = $response.Content | ConvertFrom-Json
    $ArmToken = $content.access_token
    ```

## <a name="read-the-properties-of-a-resource-group"></a>Kaynak Grubunun özelliklerini okuma

Azure Resource Manager’a erişmek için önceki adımda alınan erişim belirtecini kullanın ve kullanıcı tarafından atanan kimliğiniz için erişim verdiğiniz Kaynak Grubunun özelliklerini okuyun. <SUBSCRIPTION ID> öğesini ortamınızın abonelik kimliğiyle değiştirin.

```azurepowershell
(Invoke-WebRequest -Uri https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/myResourceGroupVM?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{Authorization ="Bearer $ArmToken"}).content
```
Yanıtta, aşağıdaki örneğe benzer belirli Kaynak Grubu bilgileri yer alır:

```json
{"id":"/subscriptions/<SUBSCRIPTIONID>/resourceGroups/TestRG","name":"myResourceGroupVM","location":"eastus","properties":{"provisioningState":"Succeeded"}}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanıcı tarafından atanan bir kimlik oluşturmayı ve Azure Resource Manager API'sine erişmek için bu kimliği bir Azure Sanal Makinesine eklemeyi öğrendiniz.  Azure Resource Manager hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)