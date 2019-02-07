---
title: Azure CLI kullanarak Azure için açık kaynak FHIR sunucusu dağıtma
description: Bu hızlı başlangıçta, Azure için açık kaynak Microsoft FHIR sunucu dağıtma işlemi açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6bb2806aa1ba830f1ef3215a61db4d40f531bdd8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823529"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak açık kaynak FHIR sunucusu dağıtma

Bu hızlı başlangıçta bir açık kaynak FHIR dağıtmayı öğreneceksiniz&reg; Azure CLI kullanarak azure'da sunucusu.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Sağlanan kaynakları içeren ve oluşturulduğu kaynak grubu için bir ad seçin:

```azurecli-interactive
servicename="myfhirservice"
az group create --name $servicename --location westus2
```

## <a name="deploy-template"></a>Şablon dağıtma

Azure için Microsoft FHIR Server [GitHub deposu](https://github.com/Microsoft/fhir-server) tüm gerekli kaynakları dağıtacağınız bir şablonu içerir. Dağıtın:

```azurecli-interactive
az group deployment create -g $servicename --template-uri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json --parameters serviceName=$servicename
```

## <a name="verify-fhir-server-is-running"></a>FHIR sunucunun çalıştığını doğrulayın

Yetenek ifadesi ile FHIR sunucudan alın:

```azurecli-interactive
metadataurl="https://${servicename}.azurewebsites.net/metadata"
curl --url $metadataurl
```

Veya sunucunun ilk kez yanıt vermesi için bunu bir dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kaynak grubunu silin:

```azurecli-interactive
az group delete --name $servicename
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHIR sunucu aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)