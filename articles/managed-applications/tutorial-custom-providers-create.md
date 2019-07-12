---
title: Azure özel sağlayıcısı oluşturma ve kullanma
description: Bu öğretici, özel bir sağlayıcı oluşturma ve kullanma nasıl üzerinden geçer.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 65a8e60d8216e1da16af987c9e699e24ecaec3ec
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800004"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Bir RESTful uç noktası için özel sağlayıcılar yazma

Özel sağlayıcılar azure'da iş akışlarını özelleştirmenizi sağlar. Özel bir sağlayıcı Azure arasında bir sözleşmedir ve `endpoint`. Bu öğreticide, özel bir sağlayıcı oluşturma işlemi boyunca geçer. Azure özel sağlayıcıları ile alışkın değilseniz bkz [genel bakış'ta özel kaynak sağlayıcıları](./custom-providers-overview.md).

Bu öğreticide aşağıdaki adımlar ayrılır:

- Özel bir sağlayıcı nedir
- Özel Eylemler ve kaynakları tanımlama
- Özel sağlayıcı dağıtma

Bu öğreticide, aşağıdaki öğreticiler oluşturacaksınız:

- [Bir RESTful uç noktası için özel sağlayıcılar yazma](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-a-custom-provider"></a>Özel bir sağlayıcı oluşturma

> [!NOTE]
> Bu öğreticide bir uç nokta yazma üzerinden Git değil. Lütfen izleyin [RESTful uç noktaları yazma öğretici](./tutorial-custom-providers-function-authoring.md) bir RESTful uç noktası yoksa.

Bir kez `endpoint` olan oluşturuldu, bunu arasında bir sözleşme oluşturmak için özel bir sağlayıcı oluşturma oluşturabilir ve `endpoint`. Özel bir sağlayıcı uç nokta tanımlarını listesini belirtmenizi sağlar.

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Özellik | Gerekli | Açıklama
---|---|---
name | *Evet* | Uç nokta tanımı adı. Azure kullanıma sunma bu adı altında kendi API aracılığıyla ' /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders / {resourceProviderName} / {endpointDefinitionName}'
routingType | *Yok* | Sözleşme türü ile belirler `endpoint`. Belirtilmezse, varsayılan olarak "Proxy'sine" olacaktır.
endpoint | *Evet* | İstekleri yönlendirmek için uç nokta. Bu isteğin tüm yan etkileri yanı sıra yanıt işler.

Bu durumda, `endpoint` Azure işlevini tetikleme URL'si. `<yourapp>`, `<funcname>`, Ve `<functionkey>` oluşturulan işleviniz için değerleri ile değiştirilmelidir.

## <a name="defining-custom-actions-and-resources"></a>Özel Eylemler ve kaynakları tanımlama

Özel sağlayıcı altında modellenmiş uç nokta tanımlarını listesini içeren `actions` ve `resourceTypes`. `actions` Özel Eylemler eşlemesine özel sağlayıcı tarafından sunulan, while `resourceTypes` özel kaynak olarak kullanılabilir. Bu öğreticide, özel bir sağlayıcı ile tanımlarız bir `action` adlı `myCustomAction` ve `resourceType` adlı `myCustomResources`.

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ],
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ]
  },
  "location": "eastus"
}
```

Değiştirin `endpoint` tetikleyici URL'yi önceki öğreticide daha önce oluşturulan işleve koyun.

## <a name="deploying-the-custom-provider"></a>Özel sağlayıcı dağıtma

> [!NOTE]
> `endpoint` İşlevi URL'si ile değiştirilmelidir.

Bir Azure Resource Manager şablonu kullanarak yukarıdaki özel sağlayıcı dağıtılabilir.

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders",
            "name": "myCustomProvider",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "actions": [
                    {
                        "name": "myCustomAction",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ],
                "resourceTypes": [
                    {
                        "name": "myCustomResources",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ]
            }
        }
    ]
}
```

## <a name="using-custom-actions-and-resources"></a>Özel Eylemler ve kaynakları kullanma

Özel bir sağlayıcı biz oluşturduktan sonra size yeni Azure API'leri kullanabilir. Özel bir sağlayıcı yazılımınız olması ve çağırmak nasıl sonraki bölümde açıklanmaktadır.

### <a name="custom-actions"></a>Özel Eylemler

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> `{subscriptionId}` Ve `{resourceGroupName}` özel bir sağlayıcı dağıtıldığı kaynak grubu ve abonelik ile değiştirilmelidir.

```azurecli-interactive
az resource invoke-action --action myCustomAction \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider \
                          --request-body
                            '{
                                "hello": "world"
                            }'
```

Parametre | Gerekli | Açıklama
---|---|---
Eylem | *Evet* | Oluşturulan özel sağlayıcıda tanımlanmış eylemin adı.
kimlikleri | *Evet* | Oluşturulan özel sağlayıcı kaynak kimliği.
istek gövdesi | *Yok* | Gönderilecek istek gövdesi `endpoint`.

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Yok.

---

### <a name="custom-resources"></a>Özel kaynaklar

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> `{subscriptionId}` Ve `{resourceGroupName}` özel bir sağlayıcı dağıtıldığı kaynak grubu ve abonelik ile değiştirilmelidir.

Özel bir kaynak oluşturun:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1 \
                   --properties
                    '{
                        "location": "eastus",
                        "properties": {
                            "hello" : "world"
                        }
                    }'
```

Parametre | Gerekli | Açıklama
---|---|---
tam-nesnesi | *Evet* | Özellikleri nesnesi konum, etiketler, sku ve/veya planı gibi diğer seçenekleri içerip içermediğini belirtir.
id | *Evet* | Özel kaynak kaynak kimliği. Bu, oluşturulan özel bir sağlayıcı dışına mevcut olması gerekir.
properties | *Evet* | Gönderilecek istek gövdesi `endpoint`.

Özel bir Azure kaynağı silin:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
id | *Evet* | Özel kaynak kaynak kimliği. Bu, oluşturulan özel bir sağlayıcı dışına mevcut olması gerekir.

Özel bir Azure kaynağı alın:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
id | *Evet* | Özel kaynak kaynak kimliği. Bu, oluşturulan özel bir sağlayıcı dışına mevcut olması gerekir.

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Azure Resource Manager şablonu örneği:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "name": "myCustomProvider/myTestResourceName1",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "hello": "world"
            }
        }
    ]
}
```

Parametre | Gerekli | Açıklama
---|---|---
resourceTypeName | *Evet* | `name` , *ResourceType* özel sağlayıcıda tanımlanmış.
resourceProviderName | *Evet* | Özel sağlayıcı örneği adı.
customResourceName | *Evet* | Özel kaynak adı.

---

> [!NOTE]
> Dağıtma ve özel bir sağlayıcı kullanarak tamamladıktan sonra Azure işlevi dahil olmak üzere oluşturulan tüm kaynakları temizleyin unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcıları hakkında bilgi edindiniz. Özel bir sağlayıcı oluşturmak için sonraki makaleye gidin.

- [Nasıl Yapılır: Azure REST API'si için özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl Yapılır: Azure REST API'si için özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
