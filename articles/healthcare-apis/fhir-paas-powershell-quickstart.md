---
title: Azure API için FHIR PowerShell kullanarak dağıtma
description: Azure API, PowerShell kullanarak FHIR için dağıtın.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 0cea06da1815fa1128b16f1427690141823e82d8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823569"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Hızlı Başlangıç: Azure API için FHIR PowerShell kullanarak dağıtma

Bu hızlı başlangıçta, Azure API'si için PowerShell kullanarak FHIR dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Azure API FHIR kaynak sağlayıcısı kaydetme

Varsa `Microsoft.HealthcareApis` kaynak sağlayıcısı zaten kayıtlı değil, aboneliğiniz için ile kaydedebilirsiniz:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-manager-template"></a>Azure Resource Manager şablonu oluşturma

Bir Azure Resource Manager şablonu ile aşağıdaki içeriği oluşturun:

[!code-json[](samples/azuredeploy.json)]

Adla kaydet `azuredeploy.json`

## <a name="create-azure-resource-manager-parameter-file"></a>Azure Resource Manager parametre dosyası oluşturma

Aşağıdaki içeriğe bir Azure Resource Manager şablon parametre dosyası oluşturun:

[!code-json[](samples/azuredeploy.parameters.json)]

Adla kaydet `azuredeploy.parameters.json`

Nesne kimliği değerleri `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` ve `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` belirli Azure Active Directory kullanıcılarının kimliklerini nesnesine karşılık gelen veya hizmet sorumluları abonelikle ilişkili dizinde. Belirli bir kullanıcının nesne kimliği bilmek istiyorsanız, gibi bir komutla bulabilirsiniz:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

Nasıl yapılır Kılavuzu okuyun [kimlik nesne kimliklerini bulma](find-identity-object-ids.md) daha fazla ayrıntı için.

## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun

```azurepowershell-interactive
$rg = New-AzureRmResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-template"></a>Şablon dağıtma

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment -ResourceGroup $rg.ResourceGroupName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

## <a name="fetch-capability-statement"></a>Getirme yeteneği deyimi

Azure API FHIR hesabı için bir FHIR yetenek ifadesi yakalama tarafından çalıştığını doğrulamak mümkün olacaktır:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri $metadataUrl "https://nameOfFhirAccount.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kaynak grubunu silin:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure API FHIR için aboneliğinize dağıttınız. Postman kullanarak FHIR API'sine erişim öğrenmek için Postman Öğreticisine geçin.

>[!div class="nextstepaction"]
>[Postman kullanarak FHIR API'sine erişin](access-fhir-postman-tutorial.md)