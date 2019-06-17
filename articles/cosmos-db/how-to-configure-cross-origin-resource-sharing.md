---
title: Çıkış noktaları arası kaynak paylaşımı (CORS) Azure Cosmos DB
description: Bu makalede, Azure portalı ve Azure Resource Manager şablonları kullanarak Azure Cosmos DB'de çıkış noktaları arası kaynak paylaşımı (CORS) yapılandırma açıklanır.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 1269c4c2405e9b906b63c8a29c0de1ac217da1d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241901"
---
# <a name="configure-cross-origin-resource-sharing-cors"></a>Çıkış noktaları arası kaynak paylaşımı (CORS) yapılandırma 

Çıkış noktaları arası kaynak paylaşımı (CORS), başka bir etki alanındaki kaynaklara erişmek bir etki alanı altında çalışan bir web uygulamasını etkinleştiren bir HTTP özelliğidir. Web tarayıcısı arama API'leri farklı etki alanındaki bir web sayfasından engelleyen aynı çıkış noktası ilkesi olarak bilinen bir güvenlik kısıtlaması uygular. Ancak, CORS, başka bir etki alanındaki API'leri çağırmak kaynak etki alanının izin vermek için güvenli bir yol sağlar. Azure Cosmos DB SQL API çekirdek artık "allowedOrigins" üst bilgisini kullanarak çıkış noktaları arası kaynak paylaşımı (CORS) destekliyor. CORS desteği, Azure Cosmos hesabınız için etkinleştirdikten sonra yalnızca kimliği doğrulanmış istekler, belirlediğiniz kurallara göre izin verilip verilmeyeceğini belirlemek için değerlendirilir.

Çıkış noktaları arası kaynak paylaşımı (CORS) ayarı Azure portalından veya bir Azure Resource Manager şablonundan yapılandırabilirsiniz. Azure Cosmos DB SQL API Core, Node.js ve tarayıcı tabanlı ortamlar çalışır bir JavaScript kitaplığı destekler. Bu kitaplık, artık ağ geçidi modunu kullanırken CORS desteği avantajlarından yararlanabilirsiniz. Bu özelliği kullanmak için gereken istemci tarafı yapılandırma yoktur. CORS desteğiyle Azure Cosmos DB ile doğrudan bir tarayıcıdan kaynaklara erişebilir [JavaScript Kitaplığı](https://www.npmjs.com/package/@azure/cosmos) veya doğrudan [REST API](https://docs.microsoft.com/rest/api/cosmos-db/) basit işlemler için. 

## <a name="enable-cors-support-from-azure-portal"></a>Azure portalından CORS desteğini etkinleştirme

Azure portalını kullanarak çıkış noktaları arası kaynak paylaşımını etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure cosmos DB hesabınıza gidin. Açık **CORS** dikey penceresi.

2. Azure Cosmos DB hesabınıza çıkış noktaları arası aramaları kaynakları virgülle ayrılmış bir listesini belirtin. Örneğin, `https://www.mydomain.com`, `https://mydomain.com`, `https://api.mydomain.com`. Joker karakter kullanabilirsiniz "\*" tüm kaynaklara izin ve seçmek için **Gönder**. 

   > [!NOTE]
   > Şu anda, joker karakter etki alanı adının bir parçası olarak kullanamazsınız. Örneğin `https://*.mydomain.net` biçimi henüz desteklenmiyor. 
   
   ![Azure portalını kullanarak başlangıç noktaları arası kaynak paylaşımını etkinleştir](./media/how-to-configure-cross-origin-resource-sharing/enable-cross-origin-resource-sharing-using-azure-portal.png)
 
## <a name="enable-cors-support-from-resource-manager-template"></a>Resource Manager şablonundan CORS desteğini etkinleştirme

Resource Manager şablonu kullanarak CORS'yi etkinleştirmek için varolan bir şablonu için "allowedOrigins" özelliği "cors" bölümünü ekleyin. Aşağıdaki JSON CORS'yi etkinleştirerek yeni bir Azure Cosmos hesabı oluşturan bir şablonu örneğidir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "name": "test",
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "apiVersion": "2015-04-08",
            "location": "East US 2",
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "Session",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "test-eastus2",
                        "failoverPriority": 0,
                        "locationName": "East US 2"
                    }
                ],
                "cors": [
                    {
                        "allowedOrigins": "*"
                    }
                ]
            },
            "dependsOn": [
            ]
        }
    ]
}
```

## <a name="using-the-azure-cosmos-db-javascript-library-from-a-browser"></a>Bir tarayıcıdan Azure Cosmos DB JavaScript kitaplığı kullanma

Bugün, Azure Cosmos DB JavaScript kitaplığı, yalnızca kendi paketiyle birlikte kitaplığı CommonJS sürümü vardır. Bu kitaplık tarayıcıdan kullanmak için tarayıcı uyumlu bir kitaplık oluşturmak için toplaması veya Web gibi bir araç kullanmak zorunda. Belirli bir Node.js kitaplıkları, bunlar için tarayıcı mocks olması gerekir. Sahte gerekli ayarları içeren bir Web yapılandırma dosyası örneği verilmiştir.

```javascript
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  node: {
    net: "mock",
    tls: "mock"
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
```
 
İşte bir [kod örneği](https://github.com/christopheranderson/cosmos-browser-sample) kullanan TypeScript ve Web ile Azure Cosmos DB JavaScript SDK'sı kitaplığı yeni öğe oluşturulduğunda, gerçek zamanlı güncelleştirmeleri gönderen bir Todo uygulaması oluşturun.
Tarayıcıdan Azure Cosmos DB ile iletişim kurmak için en iyi uygulama, herhangi bir birincil anahtar kullanmayın. Bunun yerine, kaynak belirteçlerine iletişim kurmak için kullanın. Kaynak belirteçleri hakkında daha fazla bilgi için bkz: [Azure Cosmos DB erişimi güvenli hale getirme](secure-access-to-data.md#resource-tokens) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızın güvenliğini sağlamak için diğer yollar hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB için güvenlik duvarını yapılandırma](how-to-configure-firewall.md) makalesi.

* [Sanal ağ ve alt ağ tabanlı erişim Azure Cosmos DB hesabınız için yapılandırma](how-to-configure-vnet-service-endpoint.md)
    

