---
title: Azure Key Vault ile yönetilen uygulamaları kullanma | Microsoft Docs
description: Yönetilen uygulamaları dağıtırken erişim gizli dizilerini Azure Key Vault'ta kullanma işlemini gösterir
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 55410250ccd4dfceac8ac9ae5b81d4736de0d91a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60588303"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Azure yönetilen uygulamaları dağıtırken access Key Vault gizli

(Parola gibi) güvenli bir değerle, dağıtım sırasında parametre olarak geçirmeniz gerektiğinde, değerini almak bir [Azure anahtar kasası](../key-vault/key-vault-whatis.md). Yönetilen uygulamaları dağıtırken Key Vault'a erişmek için erişim izni vermesi gerekir **Gereci kaynak sağlayıcısı** hizmet sorumlusu. Yönetilen uygulamaları hizmet işlemleri çalıştırmak için bu kimliği kullanır. Başarıyla bir değer dağıtım sırasında anahtar Kasasından almak için hizmet sorumlusu Key Vault erişebilir olması gerekir.

Bu makalede yönetilen uygulamalarla çalışmak için Key Vault yapılandırma açıklanır.

## <a name="enable-template-deployment"></a>Şablon dağıtımı etkinleştir

1. Portalda, anahtar kasanızı seçin.

1. **Erişim ilkeleri**'ni seçin.   

   ![Erişim ilkelerini seçin](./media/key-vault-access/select-access-policies.png)

1. **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**'ı seçin.

   ![Gelişmiş erişim ilkelerini görüntülemek](./media/key-vault-access/advanced.png)

1. Seçin **şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir**. Ardından **Kaydet**’i seçin.

   ![Şablon dağıtımı etkinleştir](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Katkıda bulunan olarak hizmet Ekle

1. Seçin **erişim denetimi (IAM)** .

   ![Erişim denetimi seçin](./media/key-vault-access/access-control.png)

1. Seçin **rol ataması Ekle**.

   ![Ekle'yi seçin](./media/key-vault-access/add-access-control.png)

1. Seçin **katkıda bulunan** rolü için. Arama **Gereci kaynak sağlayıcısı** ve kullanılabilir seçenekler arasından seçin.

   ![Arama sağlayıcısı](./media/key-vault-access/search-provider.png)

1. **Kaydet**’i seçin.

## <a name="reference-key-vault-secret"></a>Key Vault gizli başvurusu

Yönetilen uygulamanızın bir şablon için Key Vault'tan bir gizli dizi geçirmek için kullanmanız gerekir bir [bağlı şablon](../azure-resource-manager/resource-group-linked-templates.md) ve bağlantılı şablon parametrelerini Key Vault'ta başvuru. Key Vault kaynak kimliği ve gizli dizi adı sağlayın.

```json
"resources": [{
  "apiVersion": "2015-01-01",
  "name": "linkedTemplate",
  "type": "Microsoft.Resources/deployments",
  "properties": {
    "mode": "incremental",
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<key-vault-name>"
          },
          "secretName": "<secret-name>"
        }
      },
      "adminLogin": { "value": "[parameters('adminLogin')]" },
      "sqlServerName": {"value": "[parameters('sqlServerName')]"}
    }
  }
}],
```

## <a name="next-steps"></a>Sonraki adımlar

Key Vault'unuza yönetilen bir uygulama dağıtımı sırasında erişilebilir olacak şekilde yapılandırdınız.

* Şablon parametresi olarak bir anahtar Kasası'ndaki bir değer geçirme hakkında daha fazla bilgi için bkz: [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](../azure-resource-manager/resource-manager-keyvault-parameter.md).
* Yönetilen uygulama örnekleri için bkz. [örnek projeler için Azure yönetilen uygulamalar](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.