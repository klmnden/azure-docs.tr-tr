---
title: Azure CLI kullanarak Azure blok zinciri hizmeti yönetme
description: Oluşturma ve Azure CLI ile Azure blok zinciri hizmetini yönetme
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: d078ca181b2eed4b80d4f12f1c03b42f4e242194
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65154449"
---
# <a name="manage-azure-blockchain-service-with-azure-cli"></a>Azure CLI ile Azure blok zinciri hizmetini yönetme

Azure portalına ek olarak hızla oluşturup Azure blok zinciri hizmetiniz için blok zinciri üyeleri ve işlem düğümleri yönetmek için Azure CLI'yı kullanabilirsiniz.

En son yüklediğinizden emin olun [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ile bir Azure hesabında oturum açtığınızdan `az login`.

Aşağıdaki örneklerde, örnek değiştirin `<parameter names>` kendi değerlerinizle.

## <a name="create-blockchain-member"></a>Blok zinciri üye oluştur

Örnek bir blok zinciri üye çekirdek muhasebe Protokolü yeni Konsorsiyumu içinde çalışan Azure blok zinciri hizmeti oluşturur.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties "{ \"location\": \"<myBlockchainLocation>\", \"properties\": {\"password\": \"<myStrongPassword>\", \"protocol\": \"Quorum\", \"consortium\": \"<myConsortiumName>\", \"consortiumManagementAccountPassword\": \"<myConsortiumManagementAccountPassword>\", \"firewallRules\": [ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ] }, \"sku\": { \"name\": \"<skuName>\" } }"
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **name** | Azure Blockchain hizmet blok zinciri üyelik tanımlayan benzersiz bir ad. Ad için genel bir uç nokta adresi kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`. |
| **konum** | Blok zinciri üye oluşturulduğu azure bölgesi. Örneğin, `eastus`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin. |
| **Parola** | Üye hesabı parolası. Üye hesabı parolası, temel kimlik doğrulaması kullanarak blok zinciri üyenin genel uç kimlik doğrulaması için kullanılır. Parola aşağıdaki dört gereksinimleri üçünü karşılamalıdır: uzunluğu 12 & 72 karakter, 1 küçük harf, 1 büyük harf karakter, 1 sayı ve sayı değil sign(#), percent(%), virgül (,), star(*), 1 özel karakter arasında olması gerekiyor geri teklifi (\`), çift quote("), tek tırnak işareti, Tireli ve semicolumn(;)|
| **Protokolü** | Genel Önizleme çekirdek destekler. |
| **Consortium** | Katılma veya oluşturma consortium adı. |
| **consortiumManagementAccountPassword** | Consortium yönetim parolası. Parola Konsorsiyumu birleştirmek için kullanılır. |
| **RuleName** | Kural adı izin verilenler listesine bir IP adresi aralığı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **startIpAddress** | Beyaz listeye ekleme için IP adresi aralığı başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **Değerini** | Beyaz listeye ekleme için IP adresi aralığı sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **skuName** | Katman türü. S0 standart ve B0 için temel için kullanın. |

## <a name="change-blockchain-member-password"></a>Blok zinciri üye parolasını değiştirme

Örnek, bir blok zinciri üyesinin parolayı değiştirir.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.password="<myStrongPassword>" --remove properties.consortiumManagementAccountAddress
```
| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **name** | Azure Blockchain Service üyelik tanımlayan ad. |
| **Parola** | Üye hesabı parolası. Parola aşağıdaki dört gereksinimleri üçünü karşılamalıdır: uzunluğu 12 & 72 karakter, 1 küçük harf, 1 büyük harf karakter, 1 sayı ve sayı değil sign(#), percent(%), virgül (,), star(*), 1 özel karakter arasında olması gerekiyor geri teklifi (\`), çift quote("), tek tırnak işareti, Tireli ve semicolon(;). |


## <a name="create-transaction-node"></a>İşlem düğümü oluşturma

Bir işlem düğümü varolan bir blok zinciri üye içinde oluşturun. İşlem düğümleri ekleyerek yükünü dağıtmak ve güvenlik yalıtımı artırın. Örneğin, farklı istemci uygulamaları için bir işlem düğümü uç noktası olabilir.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --is-full-object --properties "{ \"location\": \"<myRegion>\", \"properties\": { \"password\": \"<myStrongPassword>\", \"firewallRules\": [ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ] } }"
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **name** | Yeni işlem düğüm adını da içeren Azure blok zinciri hizmet blockchain üyesinin adı. |
| **konum** | Blok zinciri üye oluşturulduğu azure bölgesi. Örneğin, `eastus`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin. |
| **Parola** | İşlem düğümü parolası. Parola aşağıdaki dört gereksinimleri üçünü karşılamalıdır: uzunluğu 12 & 72 karakter, 1 küçük harf, 1 büyük harf karakter, 1 sayı ve sayı değil sign(#), percent(%), virgül (,), star(*), 1 özel karakter arasında olması gerekiyor geri teklifi (\`), çift quote("), tek tırnak işareti, Tireli ve semicolon(;). |
| **RuleName** | Kural adı izin verilenler listesine bir IP adresi aralığı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **startIpAddress** | Beyaz listeye ekleme için IP adresi aralığı başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre. |
| **Değerini** | Beyaz listeye ekleme için IP adresi aralığı sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre.|

## <a name="change-transaction-node-password"></a>İşlem düğümü parolasını değiştirme

Örnek, bir işlem düğümü parolayı değiştirir.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --set properties.password="<myStrongPassword>"
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Yeni işlem düğüm adını da içeren Azure blok zinciri hizmet blockchain üyesinin adı. |
| **Parola** | İşlem düğümü parolası. Parola aşağıdaki dört gereksinimleri üçünü karşılamalıdır: uzunluğu 12 & 72 karakter, 1 küçük harf, 1 büyük harf karakter, 1 sayı ve sayı değil sign(#), percent(%), virgül (,), star(*), 1 özel karakter arasında olması gerekiyor geri teklifi (\`), çift quote("), tek tırnak işareti, Tireli ve semicolon(;). |

## <a name="change-consortium-management-account-password"></a>Consortium yönetim hesabı parolasını değiştirme

Consortium yönetim hesabı consortium Üyelik yönetimi için kullanılır. Her üye consortium yönetim hesabı tarafından benzersiz şekilde tanımlanır ve şu komutu ile bu hesabın parolasını değiştirebilirsiniz.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.consortiumManagementAccountPassword="<myConsortiumManagementAccountPassword>" --remove properties.consortiumManagementAccountAddress
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının oluşturulduğu kaynak grubu adı. |
| **name** | Azure Blockchain Service üyelik tanımlayan ad. |
| **consortiumManagementAccountPassword** | Consortium yönetim hesabının parolası. Parola aşağıdaki dört gereksinimleri üçünü karşılamalıdır: uzunluğu 12 & 72 karakter, 1 küçük harf, 1 büyük harf karakter, 1 sayı ve sayı değil sign(#), percent(%), virgül (,), star(*), 1 özel karakter arasında olması gerekiyor geri teklifi (\`), çift quote("), tek tırnak işareti, Tireli ve semicolon(;). |
  
## <a name="update-firewall-rules"></a>Güvenlik duvarı kurallarını güncelleştir

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.firewallRules="[ { \"ruleName\": \"<myRuleName>\", \"startIpAddress\": \"<myStartIpAddress>\", \"endIpAddress\": \"<myEndIpAddress>\" } ]" --remove properties.consortiumManagementAccountAddress
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Azure Blockchain hizmet blok zinciri üyesinin adı. |
| **RuleName** | Kural adı izin verilenler listesine bir IP adresi aralığı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **startIpAddress** | Beyaz listeye ekleme için IP adresi aralığı başlangıcı. Güvenlik duvarı kuralları için isteğe bağlı parametre.|
| **Değerini** | Beyaz listeye ekleme için IP adresi aralığı sonu. Güvenlik duvarı kuralları için isteğe bağlı parametre.|

## <a name="list-api-keys"></a>Anahtarları Listele API

API anahtarları, kullanıcı adı ve parola için benzer düğümü erişimi için kullanılabilir. Anahtar döndürme desteklemek için iki API anahtar vardır. API anahtarlarınızı listelemek için aşağıdaki komutu kullanın.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "listApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Yeni işlem düğüm adını da içeren Azure blok zinciri hizmet blockchain üyesinin adı. |

## <a name="regenerate-api-keys"></a>API anahtarları yeniden oluştur

API anahtarlarınızı yeniden oluşturmak için aşağıdaki komutu kullanın.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "regenerateApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers --request-body '{"keyName":"<keyValue>"}'
```


| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Yeni işlem düğüm adını da içeren Azure blok zinciri hizmet blockchain üyesinin adı. |
| **anahtar adı** | Değiştirin \<keyValue\> key1 veya key2. |

## <a name="delete-a-transaction-node"></a>İşlem düğümü Sil

Blok zinciri üyesi işlem düğümü örnek siler.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Silinecek yeni işlem düğüm adını da içeren Azure blok zinciri hizmet blockchain üyesinin adı. |

## <a name="delete-a-blockchain-member"></a>Blok zinciri üyeyi sil

Örneğin, blok zinciri üyesi siler.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure Blockchain hizmet kaynaklarının bulunduğu kaynak grubu adı. |
| **name** | Silinmek üzere Azure blok zinciri hizmet blockchain üyesinin adı. |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>Azure AD kullanıcısı için erişim izni ver

```azurecli
az role assignment create --role <role> --assignee <assignee> --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| Parametre | Açıklama |
|---------|-------------|
| **Rol** | Azure AD rolün adı. |
| **atanan** | Azure AD kullanıcı kimliği. Örneğin, `user@contoso.com` |
| **Kapsam** | Rol atama kapsamı. İşlem düğümün bir blok zinciri üyesi olabilir. |

**Örnek:**

Azure AD kullanıcı blok zinciri için düğüm erişim ver **üye**:

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**Örnek:**

Azure AD kullanıcı blok zinciri için düğüm erişim ver **işlem düğümü**:

```azurecli
az role assignment create \
  --role "MyRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>Azure AD grubu ya da uygulama düğümü erişimi vermek rolü

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id>
```
| Parametre | Açıklama |
|---------|-------------|
| **Rol** | Azure AD rolün adı. |
| **atanan nesne kimliği** | Azure AD grubu kimliği veya uygulama kimliği |
| **Kapsam** | Rol atama kapsamı. İşlem düğümün bir blok zinciri üyesi olabilir. |

**Örnek:**

Düğüm erişim için ver **uygulama rolü**

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee-object-id 22222222-2222-2222-2222-222222222222 \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>Azure AD düğümü erişimi Kaldır

```azurecli
az role assignment delete --role <myRole> --assignee <assignee> --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| Parametre | Açıklama |
|---------|-------------|
| **Rol** | Azure AD rolün adı. |
| **atanan** | Azure AD kullanıcı kimliği. Örneğin, `user@contoso.com` |
| **Kapsam** | Rol atama kapsamı. İşlem düğümün bir blok zinciri üyesi olabilir. |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure portalı ile Azure blok zinciri hizmeti işlem düğümleri yapılandırın](configure-transaction-nodes.md)
