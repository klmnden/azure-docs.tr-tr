---
title: "Bir Azure tüketen yönetilen uygulama | Microsoft Docs"
description: "Bir müşteri Azure nasıl oluşturduğunu açıklar yayımlanan dosyalarından yönetilen uygulama."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a>Bir iç yönetilen uygulama kullanma

Azure tüketebileceği [yönetilen uygulamaları](managed-application-overview.md) kuruluşunuzun üyeleri için yöneliktir. Örneğin, kuruluş standartlarıyla uyumluluğu sağlamak BT departmanınızın yönetilen uygulamaları seçin. Bu yönetilen uygulamaların Azure Marketi hizmet Kataloğu kullanılabilir.

Bu makale ile devam etmeden önce aboneliğiniz için bir yönetilen uygulamayı hizmet Kataloğu'nda kullanılabilir olmalıdır. Birisi, kuruluşunuzdaki yönetilen bir uygulamaya oluşturulmamış olup [dahili tüketim için yönetilen bir uygulama yayımlama](managed-application-publishing.md).

Şu anda bir yönetilen uygulamayı kullanmak için Azure CLI veya Azure portalını kullanabilirsiniz.

## <a name="create-the-managed-application-by-using-the-portal"></a>Portalı kullanarak yönetilen uygulama oluşturma

Portal üzerinden yönetilen bir uygulamayı dağıtmak için aşağıdaki adımları izleyin:

1. Azure portalına gidin. Arama **hizmet Kataloğu yönetilen uygulama**.

   ![Hizmet Kataloğu yönetilen uygulama](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Kullanılabilir çözümleri listesinden oluşturmak istediğiniz yönetilen uygulamayı seçin. **Oluştur**’u seçin.

   ![Yönetilen uygulama seçimi](./media/managed-application-consumption/select-offer.png)

1. Kaynakları sağlamak üzere gerekli parametreler için değerler sağlayın. Seçin **Batı Orta ABD** konumu. **Tamam**’ı seçin.

   ![Yönetilen uygulama parametreleri](./media/managed-application-consumption/input-parameters.png)

1. Şablon, sağlanan değerler doğrular. Doğrulama başarılı olursa seçin **Tamam** dağıtımı başlatmak için.

   ![Yönetilen uygulama doğrulama](./media/managed-application-consumption/validation.png)

Dağıtım tamamlandıktan sonra şablonda tanımlanan uygun kaynaklara sağladığınız yönetilen kaynak grubunda sağlanır.

## <a name="create-the-managed-application-by-using-azure-cli"></a>Azure CLI kullanarak yönetilen uygulama oluşturma

Azure CLI kullanarak bir yönetilen uygulamayı oluşturmanın iki yolu vardır:

* Komut, yönetilen uygulamaları oluşturmak için kullanın.
* Normal şablon dağıtımı komutunu kullanın.

### <a name="use-the-template-deployment-command"></a>Şablon dağıtımı komutunu kullanın

Satıcı oluşturulan applianceMainTemplate.json dosyasını dağıtın.

Daha sonra iki kaynak grubu oluşturun. Yönetilen uygulama kaynağı oluşturulduğu ilk kaynak grubu olduğundan: Microsoft.Solutions/appliances. İkinci kaynak grubu mainTemplate.json içinde tanımlanan tüm kaynaklar içeriyor. Bu kaynak grubu ISV tarafından yönetilir.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Kullanım `westcentralus` kaynak grubu konumu olarak.
>

MainResourceGroup applianceMainTemplate.json dağıtmak için aşağıdaki komutu kullanın:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Yukarıdaki şablonu çalıştıktan sonra şablonda tanımlanan parametre değerlerini ister. Bir şablona kaynakları sağlamak üzere gerekli parametreleri ek olarak, iki anahtar parametre değerlerini gerekir:

- **managedResourceGroupId**: applianceMainTemplate.json içinde tanımlanan kaynakları içeren kaynak grubunun kimliği. Kimliği biçimidir `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. İsteğe bağlı olarak önceki örnekte kimliği olan `managedResourceGroup`.
- **applianceDefinitionId**: yönetilen uygulama tanımı kaynak kimliği. Bu değer, ISV tarafından sağlanır.

> [!NOTE]
> Yayımcı, yönetilen uygulama tanımını içeren kaynak grubuna erişim vermeniz gerekir. Tanımı kaynak yayımcı abonelikte oluşturulur. Bu nedenle, bir kullanıcı, kullanıcı grubu veya müşteri Kiracı uygulamada bu kaynak için okuma erişimi gerekir.

Dağıtım başarıyla tamamlandıktan sonra yönetilen uygulama mainResourceGroup içinde oluşturulur bakın. StorageAccount kaynak managedResourceGroup içinde oluşturulur.

### <a name="use-the-create-command"></a>Oluştur komutunu kullanın

Kullanabileceğiniz `az managedapp create` yönetilen uygulamayı tanımından yönetilen bir uygulama oluşturmak için komutu.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **Gereci tanımı kimliği**: yönetilen uygulama tanımının önceki adımda oluşturduğunuz kaynak kimliği. Bu kimliği edinmek için aşağıdaki komutu çalıştırın:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Bu komut, yönetilen uygulama tanımının döndürür. ID özelliği değeri gerekir.

* **Yönetilen-rg-ID**: applianceMainTemplate.json içinde tanımlanan kaynakları içeren kaynak grubunun adı. Bu kaynak grubu yönetilen kaynak grubudur. Yayımcı tarafından yönetilir. Yoksa, sizin için oluşturulur.
* **kaynak grubu**: yönetilen uygulama kaynağı oluşturulduğu kaynak grubu. Bu kaynak grubunda Microsoft.Solutions/appliance kaynak yaşar.
* **parametreleri**: applianceMainTemplate.json içinde tanımlanan kaynaklar için gerekli olan parametreleri.

## <a name="known-issues"></a>Bilinen sorunlar

Bu önizleme sürümü, aşağıdaki konuları içerir:

* Yönetilen uygulama oluşturma sırasında 500 İç sunucu hatası görüntülenir. Bu sorunu yaşayıp çalıştırırsanız, aralıklı olması olasıdır. İşlemi yeniden deneyin.
* Yeni bir kaynak grubu yönetilen kaynak grubu için gereklidir. Varolan bir kaynak grubu kullanıyorsanız, dağıtım başarısız olur.
* Microsoft.Solutions/appliances kaynak içeren kaynak grubunu oluşturulmalıdır **westcentralus** konumu.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [yönetilen uygulama genel bakış](managed-application-overview.md).
* Hizmet Kataloğu yönetilen uygulama yayımlama hakkında daha fazla bilgi için bkz: [oluşturma ve bir hizmet Kataloğu yönetilen uygulamayı yayımlayın](managed-application-publishing.md).
* Azure Marketi'nde yayımlama yönetilen uygulamalar hakkında daha fazla bilgi için bkz: [Azure Market uygulamalarda yönetilen](managed-application-author-marketplace.md).
* Marketten bir yönetilen uygulamayı kullanma hakkında daha fazla bilgi için bkz: [tüketen Azure Market uygulamalarda yönetilen](managed-application-consume-marketplace.md).
