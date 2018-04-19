---
title: "Ağ kaynak sağlayıcısı genel bakış | Microsoft Docs"
description: "Yeni ağ kaynak sağlayıcısı Azure Kaynak Yöneticisi'nde hakkında bilgi edinin"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 79bf09da-4809-45cb-8d21-705616ef24dc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 2428c707ddeed281fddd1e57bc5574603f0b9b1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="network-resource-provider"></a>Ağ Kaynağı Sağlayıcı
Günümüzün iş success, bir underpinning ihtiyaç yeteneğidir yapı ve büyük ölçekli ağ kullanan uygulamalar Çevik, esnek, güvenli ve tekrarlanabilir bir yolla yönetin. Azure Resource Manager kaynakların kaynak grupları tek bir koleksiyon olarak bu tür uygulamalar oluşturmanıza olanak sağlar. Bu tür kaynakları çeşitli kaynak sağlayıcıları altında Kaynak Yöneticisi üzerinden yönetilir.

Azure Resource Manager kaynaklarınıza erişim sağlamak için farklı kaynak sağlayıcıları kullanır. Üç ana kaynak sağlayıcıları vardır: ağ, depolama ve işlem. Bu belge ağ kaynak sağlayıcısı, avantajları ve özelliklerini açıklar dahil olmak üzere:

* **Meta veri** – etiketleri kullanarak kaynaklarına bilgi ekleyebilirsiniz. Bu etiketler, kaynak grupları ve abonelikler arasında kaynak kullanımını izlemek için kullanılabilir.
* **Ağınızın daha geniş bir denetim** - ağ kaynaklarına gevşek ve daha ayrıntılı bir şekilde kontrol edebilirsiniz. Başka bir deyişle, ağ kaynaklarını yönetme daha fazla esneklik bulunur.
* **Daha hızlı yapılandırma** -ağ kaynaklarına birbirine sıkı şekilde bağlı olduğundan, oluşturabilir ve ağ kaynakları paralel düzenlemek. Bu yapılandırma zamanı önemli ölçüde düşürdü.
* **Rol tabanlı erişim denetimi** -RBAC güvenli yönetimi için özel rollerin oluşturulmasını sağlayan yanı sıra belirli güvenlik kapsamıyla, varsayılan rolleri sağlar.
* **Daha kolay yönetim ve dağıtım** -dağıtmak ve tüm uygulama yığınını tek bir kaynak grubunda kaynaklar koleksiyonu olarak oluşturabileceğiniz beri uygulamaları yönetmek daha kolay olur. Ve daha hızlı bir şablon JSON yükü sağlayarak dağıtabilirsiniz beri dağıtılacak.
* **Hızlı özelleştirme** -dağıtımlarının yinelenebilir ve hızlı özelleştirmeyi etkinleştirmek için stil bildirim temelli şablonlar kullanabilirsiniz.
* **Yinelenebilir özelleştirme** -dağıtımlarının yinelenebilir ve hızlı özelleştirmeyi etkinleştirmek için stil bildirim temelli şablonlar kullanabilirsiniz.
* **Yönetim arabirimleri** -kaynaklarınızı yönetmek için aşağıdaki arabirimlerden birini kullanabilirsiniz:
  * REST tabanlı bir API
  * PowerShell
  * .NET SDK
  * Node.JS SDK'sı
  * Java SDK
  * Azure CLI
  * Önizleme Portalı
  * Resource Manager şablonu dili

## <a name="network-resources"></a>Ağ kaynakları
Tümünü tek işlem kaynağı (bir sanal makine) yönetilen sahip olmak yerine, ağ kaynaklarını bağımsız olarak, artık yönetebilirsiniz. Bu esneklik ve çeviklik bir karmaşık ve büyük ölçekli altyapısında bir kaynak grubu oluşturma, daha yüksek bir düzeyde sağlar.

Çok katmanlı bir uygulama içeren bir örnek dağıtım kavramsal görünümünü aşağıda sunulmuştur. NIC, ortak IP adresleri ve sanal makineleri, gibi görmek her bir kaynağın bağımsız olarak yönetilebilir.

![Ağ kaynak modeli](./media/resource-groups-networking/Figure2.png)

Her kaynak ortak bir özellikler kümesini içerir ve tek tek kendi özelliğini ayarlayın. Ortak özellikleri şunlardır:

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **adı** |Benzersiz kaynak adı. Her kaynak türünün kendi adlandırma kısıtlamaları vardır. |PIP01, VM01, NIC01 |
| **konum** |Kaynağın bulunduğu azure bölgesi |westus, eastus |
| **Kimliği** |Benzersiz bir URI göre tanımlama |/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP |

Aşağıdaki bölümler kaynaklarında tek tek özelliklerini kontrol edebilirsiniz.

[!INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[!INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[!INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[!INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[!INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[!INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[!INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[!INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[!INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[!INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Yönetim arabirimleri
Farklı arabirimleri kullanarak Azure ağ kaynaklarınızı yönetebilir. Bu belgede Biz bu arabirimleri satırındaki odaklanacaktır: REST API ve şablonları.

### <a name="rest-api"></a>REST API
Daha önce belirtildiği gibi ağ kaynaklarına arabirimleri, REST API'si, .NET SDK'sı, Node.JS SDK'sı, Java SDK'sı, PowerShell, CLI, Azure Portal ve şablonları dahil olmak üzere çeşitli yönetilebilir.

Rest API'nin HTTP 1.1 protokolünü belirtimine uygun. API Genel URI yapısı aşağıda sunulmuştur:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

Ve köşeli parantez içindeki parametreler aşağıdaki öğeleri temsil eder:

* **Abonelik kimliği** -Azure abonelik kimliğinizi.
* **Kaynak sağlayıcısı ad** -ad alanı için kullanılan sağlayıcı. Ağ kaynak sağlayıcısı için değer *Microsoft.Network*.
* **bölge adı** -Azure bölge adı

Aşağıdaki HTTP yöntemleri REST API çağrıları yapılırken desteklenir:

* **PUT** - belirli bir türde bir kaynak oluşturmak, bir kaynak özelliği değiştirmek veya kaynakları arasında bir ilişki değiştirmek için kullanılan.
* **Alma** - sağlanan kaynak bilgilerini almak için kullanılan.
* **SİLME** - mevcut bir kaynağı silmek için kullanılan.

İstek ve yanıt bir JSON yükü biçimine uygun. Daha fazla ayrıntı için bkz: [Azure Resource Management API'leri](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="resource-manager-template-language"></a>Resource Manager şablonu dili
İmperatively (aracılığıyla kaynaklara API'leri veya SDK) yönetmeye ek olarak, bildirim temelli bir programlama stili oluşturmak ve Resource Manager şablonu dili kullanarak ağ kaynaklarını yönetmek için de kullanabilirsiniz.

Bir şablon örnek gösterimini aşağıda verilmiştir:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Öncelikle bir JSON kaynakları ve açıklama parametreleri aracılığıyla eklenen örnek değerleri şablonudur. Aşağıdaki örnek, bir sanal ağ ile 2 alt ağlar oluşturmak için kullanılabilir.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Bir şablon kullanırken parametre değerlerini el ile sağlama seçeneğiniz veya bir parametre dosyası kullanabilirsiniz. Aşağıdaki örnek, yukarıdaki şablonuyla kullanılacak parametre değerlerini olası bir dizi gösterir:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Şablonları kullanarak ana avantajları şunlardır:

* Bildirim temelli bir stil kaynak grubunda karmaşık bir altyapı oluşturabilir. Bağımlılık yönetimi gibi kaynakları oluşturma orchestration kaynak yöneticisi tarafından işlenir.
* Altyapı tekrarlanabilir bir biçimde çeşitli bölgelere ve bir bölge içinde parametreleri değiştirerek oluşturulabilir.
* Bildirim temelli stili şablonları oluşturma ve altyapıyı çalışırken daha kısa sağlama süresine neden olmaktadır.

Örnek şablonları için bkz: [Azure hızlı başlangıç şablonlarını](https://github.com/Azure/azure-quickstart-templates).

Resource Manager şablonu dili hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonu dili](../azure-resource-manager/resource-group-authoring-templates.md).

Yukarıdaki örnek şablonu sanal ağ ve alt ağ kaynaklarını kullanır. Aşağıda listelenen kullanabileceğiniz diğer ağ kaynaklarına vardır:

### <a name="using-a-template"></a>Bir şablon kullanarak
Hizmetler için Azure bir şablondan Github'dan dağıtmak için bir tıklatma gerçekleştirerek veya AzureCLI, PowerShell kullanarak dağıtabilirsiniz. Github'da bir şablondan Hizmetleri dağıtmak için aşağıdaki adımları yürütün:

1. Github'dan template3 dosyasını açın. Örnek olarak, açık [iki alt ağ ile sanal ağ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Tıklayın **Azure'a Dağıt**ve ardından Azure portalına kimlik bilgilerinizle oturum açın.
3. Şablon doğrulayın ve ardından **kaydetmek**.
4. Tıklatın **Düzenle parametreleri** ve gibi bir konum seçin *Batı ABD*, vnet ve alt ağları için.
5. Gerekirse, değiştirmek **ADDRESSPREFIX** ve **SUBNETPREFIX** parametreleri ve ardından **Tamam**.
6. Tıklatın **bir kaynak grubu seçin** vnet ve alt ağları eklemek istediğiniz kaynak grubunu tıklayın. Alternatif olarak, tıklatarak yeni bir kaynak grubu oluşturabilirsiniz **veya Yeni Oluştur**.
7. **Oluştur**'a tıklayın. Döşeme görüntüleme fark **sağlama şablon dağıtımı**. Dağıtımını gerçekleştirdikten sonra aşağıdaki benzer bir ekran görürsünüz.

![Örnek şablon dağıtımı](./media/resource-groups-networking/Figure6.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Resource Manager şablonu dili](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure ağı – şablonları kullanılan](https://github.com/Azure/azure-quickstart-templates)

[Azure Resource Manager ve klasik dağıtım](../azure-resource-manager/resource-manager-deployment-model.md)

[Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md)

