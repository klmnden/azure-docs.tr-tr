---
title: PowerShell - Microsoft sağlık API'lerini kullanarak Azure için açık kaynak FHIR sunucusu dağıtma
description: Bu Hızlı Başlangıç, PowerShell kullanarak Microsoft açık kaynak FHIR sunucusu dağıtma işlemi açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6118c9371df7aa0af685390dae28bceb496145d4
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823560"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-powershell"></a>Hızlı Başlangıç: PowerShell kullanarak açık kaynak FHIR sunucusu dağıtma

Bu hızlı başlangıçta, Azure PowerShell kullanarak açık kaynak Microsoft FHIR sunucusu dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Sağlanan kaynakları içeren ve oluşturulduğu kaynak grubu için bir ad seçin:

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = New-AzureRmResourceGroup -Name $fhirServiceName -Location westus2
```

## <a name="deploy-the-fhir-server-template"></a>FHIR sunucusu şablonu dağıtma

Azure için Microsoft FHIR Server [GitHub deposu](https://github.com/Microsoft/fhir-server) tüm gerekli kaynakları dağıtacağınız bir şablonu içerir. Dağıtın:

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName `
```

## <a name="verify-fhir-server-is-running"></a>FHIR sunucunun çalıştığını doğrulayın

```azurepowershell-interactive
$metadataUrl = "https://" + $fhirServiceName + ".azurewebsites.net/metadata" 
$metadata = Invoke-WebRequest -Uri $metadataUrl
$metadata.RawContent
```

Veya sunucunun ilk kez yanıt vermesi için bunu bir dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kaynak grubunu silin:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHIR sunucu aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)