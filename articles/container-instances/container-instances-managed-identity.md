---
title: Azure Container Instances ile yönetilen bir kimlik kullanın
description: Diğer Azure hizmetlerini Azure Container Instances ile kimlik doğrulaması için bir yönetilen kimlik kullanmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 10/22/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: ac0a84aa3121c6ebb91860c96c0f6692827c8a3f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66152332"
---
# <a name="how-to-use-managed-identities-with-azure-container-instances"></a>Azure Container Instances ile yönetilen kimliklerini kullanma

Kullanım [kimliklerini Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md) Azure Container Instances, herhangi bir gizli dizileri veya kimlik bilgilerini kodda korumadan diğer Azure hizmetleriyle - etkileşim içinde kodu çalıştırmak için. Bu özellik otomatik olarak yönetilen bir kimlikle Azure Active Directory'de bir Azure Container Instances'a dağıtımını sağlar.

Bu makalede, Azure Container ınstances'da yönetilen kimlikleri hakkında daha fazla bilgi edinin ve:

> [!div class="checklist"]
> * Bir kapsayıcı grubundaki bir kullanıcı tarafından atanan veya sistem tarafından atanan kimliği etkinleştirme
> * Bir Azure anahtar Kasası'na kimlik erişim
> * Çalışan bir kapsayıcıdan bir anahtar kasasına erişmek için yönetilen kimliği kullanma

Etkinleştirin ve diğer Azure hizmetlerine erişmek için Azure Container Instances'da kimlikleri kullanmak için örnekleri uyarlayın. Bu örnekler etkileşimlidir. Ancak, uygulamada kapsayıcı görüntülerinizi Azure hizmetlerine erişmek için kodu çalıştırın.

> [!NOTE]
> Şu anda dağıtılan bir sanal ağ için bir kapsayıcı grubundaki bir yönetilen kimlik kullanamazsınız.

## <a name="why-use-a-managed-identity"></a>Yönetilen bir kimlik neden kullanmalısınız?

Herhangi bir kimlik doğrulaması için çalışan bir kapsayıcı içinde yönetilen bir kimlik kullanın [Azure AD kimlik doğrulamasını destekleyen hizmet](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) kapsayıcı kodunuzda kimlik bilgilerinin yönetimiyle olmadan. AD kimlik doğrulamayı desteklemeyen Hizmetleri için gizli dizileri Azure Key Vault'ta depolamak ve kimlik bilgilerini almak için anahtar kasasına erişmek için yönetilen bir kimlik kullanın. Bir yönetilen kimliği kullanma hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md)

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir. Şu anda yönetilen kimlikleri yalnızca Linux kapsayıcı örneklerinde desteklenir.
>  

### <a name="enable-a-managed-identity"></a>Yönetilen bir kimlik etkinleştir

 Azure Container Instances'da REST API sürümü 2018-10-01 ve karşılık gelen bir SDK'ları ve araçları itibarıyla Azure kaynakları için yönetilen kimlik desteklenir. Kapsayıcı grubu oluşturduğunuzda, bir veya daha fazla yönetilen kimlik ayarlayarak etkinleştirin bir [ContainerGroupIdentity](/rest/api/container-instances/containergroups/createorupdate#containergroupidentity) özelliği. Etkinleştirebilir veya bir kapsayıcı grubu çalışmaya başladıktan sonra yönetilen kimlikleri güncelleştirin; iki eylem yeniden başlatmak kapsayıcı grubunun neden olur. Yeni veya mevcut kapsayıcı grubunda kimlikleri ayarlamak için Azure CLI, Resource Manager şablonu veya bir YAML dosyası kullanın. 

Azure Container Instances, yönetilen Azure kimlikleri her iki tür destekler: kullanıcı tarafından atanan ve sistem tarafından atanan. Bir kapsayıcı grubu üzerinde bir sistem tarafından atanan kimliği, bir veya daha fazla kullanıcı tarafından atanan kimlikleri veya her iki tür kimlik etkinleştirebilirsiniz. 

* A **kullanıcı tarafından atanan** yönetilen kimlik olarak tek başına bir Azure kaynağında Kullanımdaki abonelik tarafından güvenilen bir Azure AD kiracısı oluşturulur. Kimlik oluşturduktan sonra kimlik (içinde Azure Container Instances veya diğer Azure Hizmetleri) bir veya daha fazla Azure kaynaklarına atanabilir. Bir kullanıcı tarafından atanan kimlik yaşam döngüsü, kapsayıcı gruplarını veya diğer hizmet kaynakları için atandığı döngüsü ayrı olarak yönetilir. Bu davranış, Azure Container Instances'da özellikle yararlıdır. Kimliği bir kapsayıcı grubunun ömründen uzun genişlettiğinden, kapsayıcı grubu dağıtımlarınızı yüksek oranda yinelenebilir hale getirmek için standart diğer ayarları ile birlikte yeniden kullanabilirsiniz.

* A **sistem tarafından atanan** yönetilen kimlik, doğrudan Azure Container ınstances'da bir kapsayıcı grubu üzerinde etkindir. Etkinleştirildiğinde, Azure grubu için bir kimlik örneğinin abonelik tarafından güvenilen ve Azure AD kiracısı içinde oluşturur. Kimlik oluşturduktan sonra kimlik bilgilerini kapsayıcı grubundaki her bir kapsayıcıdaki sağlanır. Sistem tarafından atanan bir kimlik yaşam döngüsü üzerinde etkin kapsayıcı grubuna doğrudan bağlanır. Grup silindiğinde, Azure otomatik olarak kimlik ve kimlik Azure AD'de temizler.

### <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

Yönetilen bir kimlik kullanacak şekilde kimlik başlangıçta (örneğin, bir Web uygulaması, bir anahtar kasası veya bir depolama hesabı) bir veya daha fazla Azure hizmeti kaynaklarına erişimi abonelikte verilmesi gerekir. Çalışan bir kapsayıcıdan Azure kaynaklarına erişmek için kodunuzu almalıdır bir *erişim belirteci* bir Azure AD uç noktasından. Ardından, kodunuzu çağırması Azure AD kimlik doğrulamasını destekleyen bir hizmet erişim belirteci gönderir. 

Çalışan bir kapsayıcıdaki bir yönetilen kimlik kullanarak aslında bir kimlik kullanarak bir Azure VM ile aynı olur. VM kılavuzu kullanarak bkz bir [belirteci](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md), [Azure PowerShell veya Azure CLI](../active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in.md), veya [Azure SDK'ları](../active-directory/managed-identities-azure-resources/how-to-use-vm-sdk.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.49 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma

Bu makaledeki örneklerde, Azure Container Instances'da bir Azure Key Vault gizli erişmek için yönetilen bir kimlik kullanın. 

İlk olarak, adlı bir kaynak grubu oluşturma *myResourceGroup* içinde *eastus* aşağıdaki konum [az grubu oluşturma](/cli/azure/group?view=azure-cli-latest#az-group-create) komutu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kullanım [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu bir Key Vault oluşturun. Key Vault benzersiz bir ad belirttiğinizden emin olun. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Bir örnek gizli dizi kullanarak anahtar kasası Store [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutu:

```azurecli-interactive
az keyvault secret set --name SampleSecret --value "Hello Container Instances!" --description ACIsecret  --vault-name mykeyvault
```

Azure Container ınstances'da bir kullanıcı tarafından atanan veya sistem tarafından atanan yönetilen kimlik kullanarak anahtar kasası erişim için aşağıdaki örnekleri ile devam edin.

## <a name="example-1-use-a-user-assigned-identity-to-access-azure-key-vault"></a>Örnek 1: Azure Key Vault'a erişmesi için bir kullanıcı tarafından atanan kimliği kullanın.

### <a name="create-an-identity"></a>Bir kimlik oluşturun

Öncelikle bir kimlik kullanarak abonelik oluşturma [az kimliği oluşturma](/cli/azure/identity?view=azure-cli-latest#az-identity-create) komutu. Anahtar kasası oluşturmak için kullanılan aynı kaynak grubunu kullanın veya farklı bir tane kullanın.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACIId
```

Aşağıdaki adımlarda kimlik kullanmak için [az kimlik show](/cli/azure/identity?view=azure-cli-latest#az-identity-show) kimliğin hizmet sorumlusu kimliği ve kaynak kimliği değişkenlerinde depolanacak komutu.

```azurecli-interactive
# Get service principal ID of the user-assigned identity
spID=$(az identity show --resource-group myResourceGroup --name myACIId --query principalId --output tsv)

# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACIId --query id --output tsv)
```

### <a name="enable-a-user-assigned-identity-on-a-container-group"></a>Bir kullanıcı tarafından atanan kimliği bir kapsayıcı grubunda Etkinleştir

Aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma](/cli/azure/container?view=azure-cli-latest#az-container-create) tabanlı Ubuntu Server bir kapsayıcı örneği oluşturmak için komutu. Bu örnek, etkileşimli olarak diğer Azure hizmetlerine erişmek için kullanabileceğiniz bir tek kapsayıcı grubu sağlar. `--assign-identity` Parametresi, kullanıcı tarafından atanan bir yönetilen kimlik gruba geçirir. Uzun süre çalışan komut kapsayıcıyı tutar. Bu örnek, anahtar kasası oluşturmak için kullanılan aynı kaynak grubunda kullanır, ancak farklı bir belirtebilirsiniz.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity $resourceID --command-line "tail -f /dev/null"
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. İle durumunu denetleyin [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show) komutu.

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

`identity` Çıkış bölümüne benzer şekilde görünür aşağıdaki kimlik gösteren kapsayıcı grubunda ayarlanır. `principalID` Altında `userAssignedIdentities` hizmet sorumlusu kimliğini Azure Active Directory'ye oluşturduğunuz:

```console
...
"identity": {
    "principalId": "null",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-0903-4b79-a55a-xxxxxxxxxxxx/resourcegroups/danlep1018/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACIId": {
        "clientId": "xxxxxxxx-5523-45fc-9f49-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-f25b-4895-b828-xxxxxxxxxxxx"
      }
    }
  },
...
```

### <a name="grant-user-assigned-identity-access-to-the-key-vault"></a>Anahtar Kasası'na kullanıcı tarafından atanan kimlikle erişimi verme

Aşağıdaki komutu çalıştırın [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest) Key Vault'a erişim ilkesi ayarlamak için komutu. Aşağıdaki örnekte, gizli dizileri anahtar Kasasından almak kullanıcı tarafından atanan kimlik sağlar:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-user-assigned-identity-to-get-secret-from-key-vault"></a>Key Vault'tan gizli dizisini alma kullanıcı tarafından atanan kimliği kullan

Artık çalışan bir kapsayıcı örneği içinde Key Vault'una erişmek için yönetilen kimlik kullanabilirsiniz. Bu örnekte, ilk kapsayıcı bir bash kabuğunu başlatın:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Kapsayıcı bash kabuğunda aşağıdaki komutları çalıştırın. Azure Active Directory için Key Vault kimlik doğrulaması için kullanılacak erişim belirteci almak için aşağıdaki komutu çalıştırın:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Çıkış:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Erişim belirteci kimliğini doğrulamak için sonraki komutlarda kullanılacak bir değişkende depolamak için aşağıdaki komutu çalıştırın:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Anahtar Kasası'na kimliğini doğrulamak ve gizli dizi okumak için artık erişim belirtecini kullanın. Anahtar kasanıza URL adı ile değiştirdiğinizden emin olun (*https://mykeyvault.vault.azure.net/...* ):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Aşağıdakine benzer bir yanıt gizli dizi gösteriliyor. Kodunuzda, gizli diziyi almak için bu çıkış ayrıştırma. Ardından, gizli dizi, başka bir Azure kaynak erişmek için bir sonraki işlemi kullanın.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="example-2-use-a-system-assigned-identity-to-access-azure-key-vault"></a>Örnek 2: Azure Key Vault'a erişmesi için sistem tarafından atanan bir kimlik kullanın.

### <a name="enable-a-system-assigned-identity-on-a-container-group"></a>Bir sistem tarafından atanan kimliği bir kapsayıcı grubunda Etkinleştir

Aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma](/cli/azure/container?view=azure-cli-latest#az-container-create) tabanlı Ubuntu Server bir kapsayıcı örneği oluşturmak için komutu. Bu örnek, etkileşimli olarak diğer Azure hizmetlerine erişmek için kullanabileceğiniz bir tek kapsayıcı grubu sağlar. `--assign-identity` Parametresi ek değer içermeyen bir sistem tarafından atanan yönetilen kimlik grubundaki sağlar. Uzun süre çalışan komut kapsayıcıyı tutar. Bu örnek, anahtar kasası oluşturmak için kullanılan aynı kaynak grubunda kullanır, ancak farklı bir belirtebilirsiniz.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/azure-cli --assign-identity --command-line "tail -f /dev/null"
```

Birkaç saniye içinde Azure CLI'den dağıtımın tamamlandığını belirten bir yanıt almanız gerekir. İle durumunu denetleyin [az container show](/cli/azure/container?view=azure-cli-latest#az-container-show) komutu.

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

`identity` Çıkış bölümünde görünür aşağıdakine benzer bir sistem tarafından atanan kimliği Azure Active Directory'ye oluşturduğunuz gösteriliyor:

```console
...
"identity": {
    "principalId": "xxxxxxxx-528d-7083-b74c-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-f292-4e60-9122-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
},
...
```

Bir değişken değerine ayarlanmasından `principalId` (hizmet sorumlusu kimliği) sonraki adımlarda kullanılacak kimlik.

```azurecli-interactive
spID=$(az container show --resource-group myResourceGroup --name mycontainer --query identity.principalId --out tsv)
```

### <a name="grant-container-group-access-to-the-key-vault"></a>Kapsayıcı grubu erişimi vermek için Key Vault

Aşağıdaki komutu çalıştırın [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest) Key Vault'a erişim ilkesi ayarlamak için komutu. Aşağıdaki örnekte, gizli dizileri anahtar Kasasından almak sistem tarafından yönetilen kimlik sağlar:

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $spID --secret-permissions get
```

### <a name="use-container-group-identity-to-get-secret-from-key-vault"></a>Key Vault'tan gizli almak için kapsayıcı grubu kimliği'ni kullanın

Artık çalışan bir kapsayıcı örneği içinde Key Vault'una erişmek için yönetilen kimlik kullanabilirsiniz. Bu örnekte, ilk kapsayıcı bir bash kabuğunu başlatın:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name mycontainer --exec-command "/bin/bash"
```

Kapsayıcı bash kabuğunda aşağıdaki komutları çalıştırın. Azure Active Directory için Key Vault kimlik doğrulaması için kullanılacak erişim belirteci almak için aşağıdaki komutu çalıştırın:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net%2F' -H Metadata:true -s
```

Çıkış:

```bash
{"access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9......xxxxxxxxxxxxxxxxx","refresh_token":"","expires_in":"28799","expires_on":"1539927532","not_before":"1539898432","resource":"https://vault.azure.net/","token_type":"Bearer"}
```

Erişim belirteci kimliğini doğrulamak için sonraki komutlarda kullanılacak bir değişkende depolamak için aşağıdaki komutu çalıştırın:

```bash
token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true | jq -r '.access_token')

```

Anahtar Kasası'na kimliğini doğrulamak ve gizli dizi okumak için artık erişim belirtecini kullanın. Anahtar kasanıza URL adı ile değiştirdiğinizden emin olun (*https:\//mykeyvault.vault.azure.net/...* ):

```bash
curl https://mykeyvault.vault.azure.net/secrets/SampleSecret/?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

Aşağıdakine benzer bir yanıt gizli dizi gösteriliyor. Kodunuzda, gizli diziyi almak için bu çıkış ayrıştırma. Ardından, gizli dizi, başka bir Azure kaynak erişmek için bir sonraki işlemi kullanın.

```bash
{"value":"Hello Container Instances!","contentType":"ACIsecret","id":"https://mykeyvault.vault.azure.net/secrets/SampleSecret/xxxxxxxxxxxxxxxxxxxx","attributes":{"enabled":true,"created":1539965967,"updated":1539965967,"recoveryLevel":"Purgeable"},"tags":{"file-encoding":"utf-8"}}
```

## <a name="enable-managed-identity-using-resource-manager-template"></a>Resource Manager şablonu kullanarak bir yönetilen kimlik etkinleştir

Bir yönetilen kimlik kullanarak bir kapsayıcı grubu etkinleştirmek için bir [Resource Manager şablonu](container-instances-multi-container-group.md)ayarlayın `identity` özelliği `Microsoft.ContainerInstance/containerGroups` nesnesi ile bir `ContainerGroupIdentity` nesne. Aşağıdaki kod parçacıkları Göster `identity` özelliği farklı senaryolar için yapılandırılmış. Bkz: [Resource Manager şablon başvurusu](/azure/templates/microsoft.containerinstance/containergroups). Belirtin bir `apiVersion` , `2018-10-01`.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Bir kullanıcı tarafından atanan kimliği formunun bir kaynak kimliği aşağıdaki gibidir:

```
"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}"
``` 

Bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```json
"identity": {
    "type": "UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
```

### <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

```json
"identity": {
    "type": "SystemAssigned"
    }
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve kullanıcı tarafından atanan kimlikleri

Bir kapsayıcı grubu üzerinde bir sistem tarafından atanan kimliği hem de bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```json
"identity": {
    "type": "System Assigned, UserAssigned",
    "userAssignedIdentities": {
        "myResourceID1": {
            }
        }
    }
...
```

## <a name="enable-managed-identity-using-yaml-file"></a>YAML dosyası kullanılarak yönetilen kimlik etkinleştir

Bir kapsayıcı grubundaki yönetilen bir kimlik etkinleştirmek için dağıtılan kullanarak bir [YAML dosyası](container-instances-multi-container-yaml.md), aşağıdaki YAML içerir.
Belirtin bir `apiVersion` , `2018-10-01`.

### <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik

Bir kaynak kimliği biçiminde bir kullanıcı tarafından atanan kimliği olan 

```
'/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'
```

Bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```YAML
identity:
  type: UserAssigned
  userAssignedIdentities:
    {'myResourceID1':{}}
```

### <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

```YAML
identity:
  type: SystemAssigned
```

### <a name="system--and-user-assigned-identities"></a>Sistem ve kullanıcı tarafından atanan kimlikleri

Bir kapsayıcı grubu üzerinde bir sistem tarafından atanan kimliği hem de bir veya daha fazla kullanıcı tarafından atanan kimlikleri etkinleştirebilirsiniz.

```YAML
identity:
  type: SystemAssigned, UserAssigned
  userAssignedIdentities:
   {'myResourceID1':{}}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Container ınstances'da yönetilen kimlikler hakkında öğrendiniz ve nasıl yapılır:

> [!div class="checklist"]
> * Bir kapsayıcı grubundaki bir kullanıcı tarafından atanan veya sistem tarafından atanan kimliği etkinleştirme
> * Bir Azure anahtar Kasası'na kimlik erişim
> * Çalışan bir kapsayıcıdan bir anahtar kasasına erişmek için yönetilen kimliği kullanma

* Daha fazla bilgi edinin [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/).

* Bkz: bir [Azure Go SDK örnek](https://medium.com/@samkreter/c98911206328) Azure Container Instances bir anahtar kasasına erişmek için bir yönetilen kimlik kullanarak.
