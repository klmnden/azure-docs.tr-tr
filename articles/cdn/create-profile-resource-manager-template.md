---
title: Hızlı Başlangıç - bir Azure Content Delivery Network profili ve Resource Manager şablonlarını kullanarak uç nokta oluştur | Microsoft Docs
description: Bir Azure içerik teslim ağı profili ve uç nokta Resource Manager şablonlarını kullanarak oluşturmayı öğrenin
services: cdn
documentationcenter: ''
author: senthuransivananthan
manager: danielgi
editor: ''
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: azure-cdn
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: cbde4c7fd568e6d9ff9a0d90332da96926e08077
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593145"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint-using-resource-manager-template"></a>Hızlı Başlangıç: Bir Azure CDN profili ve uç nokta Resource Manager şablonu kullanarak oluşturma

Bu hızlı başlangıçta, CLI kullanarak bir Azure Resource Manager şablonu dağıtın. Oluşturduğunuz şablonu, bir CDN profili ve CDN uç noktası, web uygulamanızın ön dağıtır.
Bu adımları tamamlamak için yaklaşık on dakika sürer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prequisites"></a>Önkoşulları işaretli

Bu hızlı başlangıç amaçları doğrultusunda, kaynağınıza kullanılacak bir Web uygulaması olmalıdır. Web uygulaması, bu hızlı başlangıçta kullanılan örnek şuraya dağıtıldı https://cdndemo.azurewebsites.net

Daha fazla bilgi için [Azure'da statik HTML web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-html).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm kaynaklar aynı kaynak grubunda dağıtılması gerekir.

Seçtiğiniz konumda kaynak grubu oluşturun. Bu örnek, Doğu ABD konumunda cdn'de adlı bir kaynak grubu oluşturmayı gösterir.

```bash
az group create --name cdn --location eastus
```

![Yeni kaynak grubu](./media/create-profile-resource-manager-template/cdn-create-resource-group.png)

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Bu adımda, kaynakları dağıtan bir şablon dosyası oluşturun.

Bu örnek bir genel Web sitesi hızlandırma senaryoyu adım kılavuzluk eder, ancak yapılandırılabilir diğer birçok ayarı vardır. Bu ayarlar Azure Resource Manager şablon Başvurusu'nda kullanılabilir. Lütfen başvuruları için bkz. [CDN profili](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles) ve [CDN profili bitiş noktasına](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints).

Microsoft CDN içerik türü listesinin değiştirmeyi desteklemediğini aklınızda bulundurun.

Şablon olarak Kaydet **resource manager cdn.json**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "cdnProfileSku": {
            "type": "string",
            "allowedValues": [
                "Standard_Microsoft",
                "Standard_Akamai",
                "Standard_Verizon",
                "Premium_Verizon"
            ]
        },
        "endpointOriginHostName": {
            "type": "string"
        }
    },
    "variables": {
        "profile": {
            "name": "[replace(toLower(parameters('cdnProfileSku')), '_', '-')]"
        },
        "endpoint": {
            "name": "[replace(toLower(parameters('endpointOriginHostName')), '.', '-')]",
            "originHostName": "[parameters('endpointOriginHostName')]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Cdn/profiles",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[variables('profile').name]",
            "sku": {
                "name": "[parameters('cdnProfileSku')]"
            }
        },
        {
            "dependsOn": [
                "[resourceId('Microsoft.Cdn/profiles', variables('profile').name)]"
            ],
            "type": "Microsoft.Cdn/profiles/endpoints",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[concat(variables('profile').name, '/', variables('endpoint').name)]",
            "properties": {
                "hostName": "[concat(variables('endpoint').name, '.azureedge.net')]",
                "originHostHeader": "[variables('endpoint').originHostName]",
                "isHttpAllowed": true,
                "isHttpsAllowed": true,
                "queryStringCachingBehavior": "IgnoreQueryString",
                "origins": [
                    {
                        "name": "[replace(variables('endpoint').originHostName, '.', '-')]",
                        "properties": {
                            "hostName": "[variables('endpoint').originHostName]",
                            "httpPort": 80,
                            "httpsPort": 443
                        }
                    }
                ],
                "contentTypesToCompress": [
                    "application/eot",
                    "application/font",
                    "application/font-sfnt",
                    "application/javascript",
                    "application/json",
                    "application/opentype",
                    "application/otf",
                    "application/pkcs7-mime",
                    "application/truetype",
                    "application/ttf",
                    "application/vnd.ms-fontobject",
                    "application/xhtml+xml",
                    "application/xml",
                    "application/xml+rss",
                    "application/x-font-opentype",
                    "application/x-font-truetype",
                    "application/x-font-ttf",
                    "application/x-httpd-cgi",
                    "application/x-javascript",
                    "application/x-mpegurl",
                    "application/x-opentype",
                    "application/x-otf",
                    "application/x-perl",
                    "application/x-ttf",
                    "font/eot",
                    "font/ttf",
                    "font/otf",
                    "font/opentype",
                    "image/svg+xml",
                    "text/css",
                    "text/csv",
                    "text/html",
                    "text/javascript",
                    "text/js",
                    "text/plain",
                    "text/richtext",
                    "text/tab-separated-values",
                    "text/xml",
                    "text/x-script",
                    "text/x-component",
                    "text/x-java-source"
                ],
                "isCompressionEnabled": true,
                "optimizationType": "GeneralWebDelivery"
            }
        }
    ],
    "outputs": {
        "cdnUrl": {
            "type": "string",
            "value": "[concat('https://', variables('endpoint').name, '.azureedge.net')]"
        }
    }
}
```

## <a name="create-the-resources"></a>Kaynakları oluşturma

Azure CLI kullanarak şablonu dağıtın. 2 girdiler için istenir:

**cdnProfileSku** -kullanmak istediğiniz CDN sağlayıcısı. Seçenekler şunlardır:

* Standard_Microsoft
* Standard_Akamai
* Standard_Verizon
* Premium_Verizon.

**endpointOriginHostName** -Örneğin, CDN cdndemo.azurewebsites.net sunulacak uç nokta.

```bash
az group deployment create --resource-group cdn --template-file arm-cdn.json
```

![Resource Manager şablonu dağıtma](./media/create-profile-resource-manager-template/cdn-deploy-resource-manager.png)

## <a name="view-the-cdn-profile"></a>CDN profili görüntüle

```bash
az cdn profile list --resource-group cdn -o table
```

![CDN profili görüntüle](./media/create-profile-resource-manager-template/cdn-view-profile.png)

## <a name="view-the-cdn-endpoint-for-the-profile-standard-microsoft"></a>CDN uç noktası için profil standart microsoft görüntüleyin

```bash
az cdn endpoint list --profile-name standard-microsoft --resource-group cdn -o table
```

![CDN uç noktayı görüntüle](./media/create-profile-resource-manager-template/cdn-view-endpoint.png)

İçeriği görüntülemek için ana bilgisayar adını kullanın. Örneğin, erişim https://cdndemo-azurewebsites-net.azureedge.net tarayıcınızı kullanarak.

## <a name="clean-up"></a>Temizleme

Kaynak grubu silindiğinde otomatik olarak dağıtılan kaynakların tümünü içinde kaldırılır.

```bash
az group delete --name cdn
```

![Kaynak grubunu sil](./media/create-profile-resource-manager-template/cdn-delete-resource-group.png)

## <a name="references"></a>Referanslar

* CDN profili - [Azure Resource Manager şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles)
* CDN uç noktası - [Azure Resource Manager şablon başvurusu belgeleri](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints)

## <a name="next-steps"></a>Sonraki adımlar

CDN uç noktanıza özel etki alanı ekleme hakkında bilgi edinmek için aşağıdaki öğreticiye bakın:

> [!div class="nextstepaction"]
> [Öğretici: Azure CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)
