---
title: Azure CLI ile Azure yönetilen uygulaması oluşturma | Microsoft Docs
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.date: 04/13/2018
ms.author: tomfitz
ms.openlocfilehash: f84fdd421ec6857cd940108546a16eb47770c766
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/18/2018
ms.locfileid: "34305302"
---
# <a name="create-and-deploy-an-azure-managed-application-with-azure-cli"></a>Azure CLI ile Azure yönetilen uygulaması oluşturma ve dağıtma

Bu makalede yönetilen uygulamalarla çalışmaya giriş bilgileri verilmektedir. Kuruluşunuzdaki kullanıcılar için bir iç kataloğa yönetilen uygulama tanımı eklersiniz. Daha sonra, bu yönetilen uygulamayı aboneliğinize dağıtırsınız. Girişi basitleştirmek amacıyla, yönetilen uygulamanız için dosyaları daha önce derledik. Bir dosya, çözümünüzün altyapısını tanımlar. İkinci bir dosya ise portal üzerinden yönetilen uygulamayı dağıtmak için kullanıcı arabirimini tanımlar. Bu dosyalar GitHub’da bulunabilir. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md) öğreticisinde bu dosyaların nasıl derlendiğini öğrenebilirsiniz.

İşiniz bittiğinde, yönetilen uygulamanın farklı kısımlarını içeren üç kaynak grubunuz olur.

| Kaynak grubu | Contains | Açıklama |
| -------------- | -------- | ----------- |
| appDefinitionGroup | Yönetilen uygulama tanımı. | Yayımcı bu kaynak grubunu ve yönetilen uygulama tanımını oluşturur. Yönetilen uygulama tanımına erişimi olan herkes bu dağıtımı yapabilir. |
| applicationGroup | Yönetilen uygulama örneği. | Tüketici bu kaynak grubunu ve yönetilen uygulama örneğini oluşturur. Tüketici bu örnekle yönetilen uygulamayı güncelleştirebilir. |
| infrastructureGroup | Yönetilen uygulama için kaynaklar. | Yönetilen uygulama oluşturulduğunda bu kaynak grubu otomatik olarak oluşturulur. Yayımcı, uygulamayı yönetmek için bu kaynak grubuna erişebilir. Tüketici, kaynak grubuna sınırlı erişime sahiptir. |

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Tanımınız için kaynak grubu oluşturma

Yönetilen uygulama tanımınız bir kaynak grubunda mevcuttur. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

Kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Yönetilen uygulama tanımlarken bir kullanıcı, grup veya tüketici adına kaynaklarını yöneten bir uygulama seçersiniz. Bu kimlik, atanan role göre yönetilen kaynak grubu üzerinde izinlere sahiptir. Genellikle, kaynakları yönetmek için bir Azure Active Directory grubu oluşturursunuz. Ancak, bu makalede kendi kimliğinizi kullanırsınız.

Kimliğinizin nesne kimliğini almak için aşağıdaki komutta kullanıcı asıl adınızı sağlayın:

```azurecli-interactive
userid=$(az ad user show --upn-or-object-id example@contoso.org --query objectId --output tsv)
```

Bundan sonra, kullanıcıya erişim izni vermek istediğiniz RBAC yerleşik rolünün rol tanımı kimliği gerekir. Aşağıdaki komut, Sahip rolünün rol tanımı kimliğinin nasıl alınacağını gösterir:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Şimdi, yönetilen uygulama tanımı kaynağını oluşturun. Yönetilen uygulama yalnızca bir depolama hesabı içerir.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"
```

Komut tamamlandığında, kaynak grubunuzda bir yönetilen uygulamanız olur. 

Yukarıdaki örnekte kullanılan parametrelerden bazıları şunlardır:

* **resource-group**: Yönetilen uygulama tanımının oluşturulduğu kaynak grubunun adı.
* **lock-level**: Yönetilen kaynak grubuna yerleştirilen kilit türü. Müşterinin bu kaynak grubunda istenmeyen işlemler gerçekleştirmesini engeller. ReadOnly şu anda desteklenen tek kilit düzeyidir. ReadOnly belirtildiğinde müşteri yalnızca yönetilen kaynak grubunda mevcut olan kaynakları okuyabilir. Yönetilen kaynak grubuna erişim izni verilen yayımcı kimlikleri kilitli olmaz.
* **authorizations**: Yönetilen kaynak grubuna izin vermek için kullanılan sorumlu kimliğini ve rol tanımı kimliğini açıklar. `<principalId>:<roleDefinitionId>` biçiminde belirtilir. Bu özellik için birden çok değer de belirtilebilir. Birden çok değer gerekirse `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>` biçiminde belirtilmelidir. Çoklu değerler boşlukla ayrılır.
* **package-file-uri**: Gerekli dosyaları içeren .zip paketinin konumu. Paket en azından **mainTemplate.json** ve **createUiDefinition.json** dosyalarını içerir. **mainTemplate.json**, yönetilen uygulamanın bir parçası olarak sağlanan Azure kaynaklarını tanımlar. Şablon normal bir Resource Manager şablonundan farklı değildir. **createUiDefinition.json**, portal üzerinden yönetilen uygulamayı oluşturan kullanıcılar için kullanıcı arabirimi oluşturur.

## <a name="create-resource-group-for-managed-application"></a>Yönetilen uygulama için kaynak grubu oluşturma

Artık yönetilen uygulamayı dağıtmaya hazırsınız. 

Dağıtılmış yönetilen uygulamayı kapsamak için bir kaynak grubu gerekir. **westcentralus** konumunu kullanın.

```azurecli-interactive
az group create --name applicationGroup --location westcentralus
```

## <a name="deploy-the-managed-application"></a>Yönetilen uygulamayı dağıtma

Uygulamayı aşağıdaki komutlarla dağıtırsınız:

```azurecli-interactive
appid=$(az managedapp definition show --name ManagedStorage --resource-group appDefinitionGroup --query id --output tsv)
subid=$(az account show --query id --output tsv)
managedGroupId=/subscriptions/$subid/resourceGroups/infrastructureGroup

az managedapp create \
  --name storageApp \
  --location "westcentralus" \
  --kind "Servicecatalog" \
  --resource-group applicationGroup \
  --managedapp-definition-id $appid \
  --managed-rg-id $managedGroupId \
  --parameters "{\"storageAccountNamePrefix\": {\"value\": \"storage\"}, \"storageAccountType\": {\"value\": \"Standard_LRS\"}}"
```

Yukarıdaki örnekte kullanılan parametrelerden bazıları şunlardır:

* **managedapp-definition-id**: Bu makalenin başında oluşturduğunuz tanımın kimliği.
* **managed-rg-id**: Yönetilen uygulamayla ilişkili kaynaklar için kaynak grubu kimliği. Komut bu kaynak grubunu oluşturur. **Komutu çalıştırmadan önce mevcut olmamalıdır**. Bu kaynak grubu yayımcı tarafından yönetilir. 
* **resource-group**: Yönetilen uygulama kaynağının oluşturulduğu kaynak grubu.
* **parameters**: Yönetilen uygulamayla ilişkili kaynaklar için gereken parametreler.

Dağıtım başarıyla tamamlandıktan sonra yönetilen uygulamanın applicationGroup içinde oluşturulduğunu görürsünüz. storageAccount kaynağı infrastructureGroup içinde oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Dosyaların örnekleri için [Yönetilen uygulama örnekleri](https://github.com/Azure/azure-managedapp-samples/tree/master/samples) konusunu inceleyin.
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.
