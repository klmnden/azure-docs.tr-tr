---
title: Azure yönetilen uygulaması tanımı oluşturma | Microsoft Docs
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 1f80d7e63d994f0e3eb3733b99afaa1b056f4686
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60252409"
---
# <a name="publish-an-azure-managed-application-definition"></a>Azure yönetilen uygulaması tanımı yayımlama

Bu hızlı başlangıçta yönetilen uygulamalarla çalışmaya giriş bilgileri verilmektedir. Kuruluşunuzdaki kullanıcılar için bir iç kataloğa yönetilen uygulama tanımı eklersiniz. Girişi basitleştirmek amacıyla, yönetilen uygulamanız için dosyaları daha önce derledik. Bu dosyalar GitHub’da bulunabilir. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md) öğreticisinde bu dosyaların nasıl derlendiğini öğrenebilirsiniz.

İşlemi tamamladığınızda yönetilen uygulama tanımına sahip olacak **appDefinitionGroup** adlı bir kaynak grubuna sahip olacaksınız.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Tanımınız için kaynak grubu oluşturma

Yönetilen uygulama tanımınız bir kaynak grubunda mevcuttur. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

Kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Yönetilen uygulama tanımlarken bir kullanıcı, grup veya tüketici için kaynakları yöneten bir uygulama seçersiniz. Bu kimlik, atanan role göre yönetilen kaynak grubu üzerinde izinlere sahiptir. Genellikle, kaynakları yönetmek için bir Azure Active Directory grubu oluşturursunuz. Ancak, bu makalede kendi kimliğinizi kullanırsınız.

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

* **kaynak grubu**: Yönetilen uygulama tanımının oluşturulduğu kaynak grubunun adı.
* **Kilit düzeyi**: Yönetilen kaynak grubuna yerleştirilen kilit türü. Müşterinin bu kaynak grubunda istenmeyen işlemler gerçekleştirmesini engeller. ReadOnly şu anda desteklenen tek kilit düzeyidir. ReadOnly belirtildiğinde müşteri yalnızca yönetilen kaynak grubunda mevcut olan kaynakları okuyabilir. Yönetilen kaynak grubuna erişim izni verilen yayımcı kimlikleri kilitli olmaz.
* **yetkilendirmeleri**: Sorumlu Kimliği ve yönetilen kaynak grubuna izin vermek için kullanılan rol tanımı Kimliğini açıklar. `<principalId>:<roleDefinitionId>` biçiminde belirtilir. Birden fazla değer kullanacaksanız `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>` biçiminde belirtin. Değerler boşlukla ayrılır.
* **Paket dosya URI'si**: Gerekli dosyaları içeren .zip paketinin konumu. Paket **mainTemplate.json** ve **createUiDefinition.json** dosyalarını içermelidir. **mainTemplate.json**, yönetilen uygulamanın bir parçası olarak oluşturulan Azure kaynaklarını tanımlar. Şablon normal bir Resource Manager şablonundan farklı değildir. **createUiDefinition.json**, portal üzerinden yönetilen uygulamayı oluşturan kullanıcılar için kullanıcı arabirimi oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen uygulama tanımını yayımladınız. Şimdi bu tanımın bir örneğini nasıl dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Hizmet Kataloğu uygulaması dağıtma](deploy-service-catalog-quickstart.md)