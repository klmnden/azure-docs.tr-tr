---
title: Azure Resource Manager’a erişmek için Linux VM kullanıcı tarafından atanan MSI'sini kullanma
description: Linux VM üzerinde bir Kullanıcı Tarafından Atanmış Yönetilen Hizmet Kimliği (MSI) kullanarak Azure Resource Manager'a erişme işleminde size yol gösteren bir öğretici.
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 06abd7867a99c20597ed17faf6fa61b91f70baaa
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007715"
---
# <a name="tutorial-use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Öğretici: Azure Resource Manager’a erişmek için Linux VM’de kullanıcı tarafından atanan kimliği kullanma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide, kullanıcı tarafından atanan kimliği oluşturma, bunu Linux Sanal Makinesine (VM) atama ve bu kimliği Azure Resource Manager API’sine erişmek için kullanma işlemleri açıklanır. Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir. Bunlar, kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmaya olanak tanır. 

Yönetilen Hizmet Kimlikleri Azure tarafından otomatik olarak yönetilir. Bunlar, kodunuza kimlik bilgileri girmenize gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmaya olanak tanır.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Kullanıcı tarafından atanan kimliği oluşturma
> * Linux VM’sine kullanıcı tarafından atanan kimliği atama 
> * Azure Resource Manager’da Kaynak Grubuna kullanıcı tarafından atanan kimlik için erişim verme 
> * Kullanıcı tarafından atanan kimliği kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma 

## <a name="prerequisites"></a>Ön koşullar

- Yönetilen Hizmet Kimliği'ni bilmiyorsanız, [genel bakış](overview.md) bölümünü gözden geçirin. **[Sistem ve kullanıcı tarafından atanan kimlikler arasındaki farklılıkları](overview.md#how-does-it-work) gözden geçirmeyi unutmayın**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu öğreticideki gerekli kaynak oluşturma ve rol yönetimini adımlarını gerçekleştirmek için hesabınız uygun kapsamda (aboneliğiniz veya kaynak grubunuz) "Sahip" izinlerini gerektiriyor. Rol atamayla ilgili yardıma ihtiyacınız varsa bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için Rol Tabanlı Erişim Denetimi kullanma](/azure/role-based-access-control/role-assignments-portal).

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir Kaynak Grubunda Linux Sanal Makinesi oluşturma

Bu öğretici için önce yeni bir Linux VM'si oluşturursunuz. Var olan bir VM'yi kullanmayı da tercih edebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. **Kimlik doğrulama türü** olarak **SSH ortak anahtarı**'nı veya **Parola**'yı seçin. Oluşturulan kimlik bilgileri VM'de oturum açmanıza olanak tanır.

    ![Linux VM oluşturma](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Açılan listede sanal makine için bir **Abonelik** seçin.
5. İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir **Kaynak Grubu** seçmek için, **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya Desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği oluşturma

1. CLI konsolu kullanıyorsanız (Azure Cloud Shell oturumu yerine) Azure’de oturum açarak başlayın. Kullanıcı tarafından atanan yeni kimliği oluşturmak istediğiniz Azure aboneliğiyle ilişkilendirilmiş bir hesap kullanın:

    ```azurecli
    az login
    ```

2. Kullanıcı tarafından atanan kimliği oluşturmak için [az identity create](/cli/azure/identity#az_identity_create) kullanın. `-g` parametresi MSI’nin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir. `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <MSI NAME>
```

Yanıt, aşağıdaki örneğe benzer biçimde, oluşturulmuş kullanıcı tarafından atanan kimliğin ayrıntılarını içerir. Kullanıcı tarafından atanan kimliğinizin `id` değerini not alın, çünkü bu değer sonraki adımda kullanılacaktır:

```json
{
"clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
"clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
"id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
"location": "westcentralus",
"name": "<MSI NAME>",
"principalId": "9012",
"resourceGroup": "<RESOURCE GROUP>",
"tags": {},
"tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
"type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Kullanıcı tarafından atanan kimliği Linux VM’nize atama

Kullanıcı tarafından atanan kimlik, istemciler tarafından birden çok Azure kaynağında kullanılabilir. Aşağıdaki komutları kullanarak kullanıcı tarafından atanan kimliği tek bir VM'ye atayın. `-IdentityID` parametresi için önceki adımda döndürülen `Id` özelliğini kullanın.

Kullanıcı tarafından atanan MSI'yi, [az vm assign-identity](/cli/azure/vm#az_vm_assign_identity) komutunu kullanarak Linux VM'nize atayın. `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `--identities` parametre değeri için önceki adımda döndürülen `id` özelliğini kullanın.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Azure Resource Manager’da Kaynak Grubuna kullanıcı tarafından atanan kimliğiniz için erişim verme 

Yönetilen Hizmet Kimliği (MSI), kodunuzun Azure AD kimlik doğrulamasını destekleyen kaynak API'lerinde kimlik doğrulaması yapmak amacıyla erişim belirteçleri istemek için kullanabileceği kimlikleri sağlar. Bu öğreticide, kodunuz Azure Resource Manager API’sine erişir.  

Kodunuzun API'ye erişebilmesi için önce Azure Resource Manager'da kaynağa kimlik erişimi vermeniz gerekir. Bu durumda, içinde VM'nin yer aldığı Kaynak Grubudur. `<SUBSCRIPTION ID>` ve `<RESOURCE GROUP>` değerini ortamınıza uyacak şekilde güncelleştirin. Buna ek olarak, `<MSI PRINCIPALID>` özelliğini [Kullanıcı tarafından atanan MSI oluşturma](#create-a-user-assigned-msi) bölümünde `az identity create` komutuyla döndürülen `principalId` özelliğiyle değiştirin:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

Yanıt, aşağıdaki örneğe benzer biçimde, oluşturulmuş atamasının ayrıntılarını içerir:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>VM kimliğini kullanarak erişim belirteci alma ve Resource Manager çağrısı yapmak için bunu kullanma 

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız.

Bu adımları tamamlamak bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. 

1. Azure [portalında](https://portal.azure.com) oturum açın.
2. Portalda, **Sanal Makineler**'e ve Linux sanal makinesine gidin, ardından **Genel Bakış**'ta **Bağlan**'a tıklayın. VM'nize bağlanma dizesini kopyalayın.
3. Tercih ettiğiniz SSH istemcisiyle VM'ye bağlanın. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/about)'ndeki SSH istemcisini kullanabilirsiniz. SSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](~/articles/virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
4. Terminal penceresinde, Azure Resource Manager erişim belirtecini almak için CURL'yi kullanarak Azure Instance Metadata Service (IMDS) kimlik uç noktasına bir istek gönderin.  

   Erişim belirtecini almaya yönelik CURL isteği aşağıdaki örnekte gösterilmektedir. `<CLIENT ID>` özelliğinin [Kullanıcı tarafında atanan MSI oluşturma](#create-a-user-assigned-msi) bölümünde `az identity create` komutuyla döndürülen `clientId` özelliğiyle değiştirildiğinden emin olun: 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > `resource` parametresinin değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz. 
    
    Yanıtta, Azure Resource Manager’a erişmek için ihtiyacınız olan erişim belirteci vardır. 
    
    Yanıt örneği:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

5. Azure Resource Manager’a erişmek için erişim belirtecini kullanın ve önceden kullanıcı tarafından atanan kimliğiniz için erişim verdiğiniz Kaynak Grubunun özelliklerini okuyun. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` değerlerinin daha önce belirttiğiniz değerlerle ve `<ACCESS TOKEN>` öğesinin önceki adımda döndürülen belirteçle değiştirildiğinden emin olun.

    > [!NOTE]
    > URL büyük/küçük harfe duyarlıdır; dolayısıyla daha önce Kaynak Grubunu adlandırırken kullandığınız büyük/küçük harf düzenini ve `resourceGroups` içindeki büyük "G" harfini kullanmaya dikkat edin.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Yanıtta, aşağıdaki örneğe benzer belirli Kaynak Grubu bilgileri yer alır: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanıcı tarafından atanan bir kimlik oluşturmayı ve Azure Resource Manager API'sine erişmek için bu kimliği bir Linux sanal makinesine eklemeyi öğrendiniz.  Azure Resource Manager hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

