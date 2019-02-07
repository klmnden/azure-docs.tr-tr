---
title: Azure API için FHIR Azure CLI kullanarak dağıtma
description: Azure API için FHIR Azure CLI kullanarak dağıtın.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 880f9d67f87d32fbc03bc04877267b5b26022381
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823583"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>Hızlı Başlangıç: Azure API için FHIR Azure CLI kullanarak dağıtma

Bu hızlı başlangıçta, Azure CLI kullanarak azure'da FHIR için Azure API dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-azure-resource-manager-template"></a>Azure Resource Manager şablonu oluşturma

Bir Azure Resource Manager şablonu ile aşağıdaki içeriği oluşturun:

[!code-json[](samples/azuredeploy.json)]

Adla kaydet `azuredeploy.json`

## <a name="create-azure-resource-manager-parameter-file"></a>Azure Resource Manager parametre dosyası oluşturma

Aşağıdaki içeriğe bir Azure Resource Manager şablon parametre dosyası oluşturun:

[!code-json[](samples/azuredeploy.parameters.json)]

Adla kaydet `azuredeploy.parameters.json`

Nesne kimliği değerleri `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` ve `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` belirli Azure Active Directory kullanıcılarının kimliklerini nesnesine karşılık gelen veya hizmet sorumluları abonelikle ilişkili dizinde. Belirli bir kullanıcının nesne kimliği bilmek istiyorsanız, gibi bir komutla bulabilirsiniz:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

Nasıl yapılır Kılavuzu okuyun [kimlik nesne kimliklerini bulma](find-identity-object-ids.md) daha fazla ayrıntı için.

# <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturma

Azure API için FHIR içerir ve oluşturulduğu kaynak grubu için bir ad seçin:

```azurecli-interactive
az group create --name "myResourceGroup" --location westus2
```

## <a name="deploy-the-azure-api-for-fhir-account"></a>Azure API FHIR hesabı için dağıtma

Şablonu (`azuredeploy.json`) ve şablon parametre dosyasını (`azuredeploy.parameters.json`) FHIR için Azure API dağıtmak için:

```azurecli-interactive
az group deployment create -g "myResourceGroup" --template-file azuredeploy.json --parameters @{azuredeploy.parameters.json}
```

## <a name="fetch-fhir-api-capability-statement"></a>FHIR API yetenek ifadesi getirilemedi

Yetenek ifadesi ile FHIR API'sinden elde:

```azurecli-interactive
curl --url "https://nameOfFhirAccount.azurehealthcareapis.com/fhir/metadata"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kaynak grubunu silin:

```azurecli-interactive
az group delete --name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure API FHIR için aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.

>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)