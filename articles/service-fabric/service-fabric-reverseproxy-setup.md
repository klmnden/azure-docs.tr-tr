---
title: Azure Service Fabric kümesi ters Proxy'yi | Microsoft Docs
description: Ayarlama ve Service Fabric'in ters proxy ayarlarını yapılandırma hakkında bilgi edinin.
services: service-fabric
documentationcenter: na
author: jimacoMS2
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/13/2018
ms.author: v-jamebr
ms.openlocfilehash: 7f1b6f955dd3f59f6c17403b536cf99d666aab08
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60393019"
---
# <a name="set-up-and-configure-reverse-proxy-in-azure-service-fabric"></a>Ayarlama ve Azure Service Fabric ters proxy ayarlarını yapılandırma
Ters proxy, bulmak ve http uç noktaları olan diğer hizmetlerle iletişim kurma bir Service Fabric kümesinde çalışan mikro hizmetler yardımcı olan isteğe bağlı bir Azure Service Fabric hizmetidir. Daha fazla bilgi için bkz. [ters proxy Azure Service fabric'te](service-fabric-reverseproxy.md). Bu makalede ayarlama ve kümedeki ters Ara sunucu yapılandırma gösterilmektedir. 

## <a name="enable-reverse-proxy-using-azure-portal"></a>Azure portalını kullanarak ters Proxy'yi Etkinleştir

Azure portalında yeni bir Service Fabric kümesi oluşturduğunuzda, ters proxy etkinleştirmek için bir seçenek sunar. Ters proxy portalı üzerinden kullanmak için mevcut kümesi yükseltemezsiniz. 

Ters Proxy'yi yapılandırmak için olduğunda, [Azure portalını kullanarak küme oluşturma](./service-fabric-cluster-creation-via-portal.md), aşağıdakileri yaptığınızdan emin olun:

1. İçinde **2. adım: Küme Yapılandırması**altında **düğüm türü yapılandırması**seçin **etkinleştir ters proxy**.

   ![Portal ters Proxy'yi Etkinleştir](./media/service-fabric-reverseproxy-setup/enable-rp-portal.png)
2. (İsteğe bağlı) Güvenli ters Proxy'yi yapılandırmak için bir SSL sertifikası'nı yapılandırmanız gerekir. İçinde **3. adım: Güvenlik**, **küme güvenlik ayarlarını yapılandırın**altında **yapılandırma türü**seçin **özel**. Ardından, altında **Ters Proxy SSL sertifikası**seçin **bir ters proxy SSL sertifikası dahil** ve sertifika ayrıntılarınızı girin.

   ![Portalda güvenli ters proxy ayarlarını yapılandırma](./media/service-fabric-reverseproxy-setup/configure-rp-certificate-portal.png)

   Kümeyi oluşturduğunuzda ters proxy ile bir sertifika yapılandırmak isterseniz, bu nedenle daha sonra küme kaynak grubu için Resource Manager şablonu aracılığıyla yapabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager şablonları aracılığıyla ters Proxy'yi Etkinleştir](#enable-reverse-proxy-via-azure-resource-manager-templates).

## <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Azure Resource Manager şablonları aracılığıyla ters Proxy'yi Etkinleştir

Azure'da kümeler için Service Fabric ters proxy etkinleştirmek için Azure Resource Manager şablonu kullanabilirsiniz. Kümeyi oluşturduğunuzda veya daha sonraki bir zamanda küme güncelleştirerek etkinleştirmek, ters proxy etkinleştirebilirsiniz. 

Yeni bir küme için yapabilecekleriniz [özel bir Resource Manager şablonu oluşturma](service-fabric-cluster-creation-via-arm.md) veya örnek şablonu kullanabilirsiniz. 

Bir Azure kümesinde için güvenli ters proxy ayarlarını yapılandırmanıza yardımcı olabilecek örnek Resource Manager şablonları bulabilirsiniz [güvenli Ters Proxy örnek şablonları](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample) GitHub üzerinde. Başvurmak [HTTPS Ters Proxy Yapılandırma güvenli bir küme içinde](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample/README.md#configure-https-reverse-proxy-in-a-secure-cluster) ve benioku dosyasındaki yönergeleri ve şablonları bir sertifikayla güvenli ters proxy ayarlarını yapılandırma ve sertifika geçişi işlemek için kullanılacak.

Var olan bir küme için küme kaynağı için Resource Manager şablonu dışarı aktarabilirsiniz kullanarak grup [Azure portalında](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell), veya [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli).

Resource Manager şablonu oluşturduktan sonra aşağıdaki adımlarla ters proxy etkinleştirebilirsiniz:

1. Ters proxy için bir bağlantı noktasını tanımlamak [parametreler bölümü](../azure-resource-manager/resource-group-authoring-templates.md) şablonu.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Her bir nodetype nesneleri için bağlantı noktasını belirtin [ **Microsoft.ServiceFabric/clusters** ](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) [kaynak türü bölümüne](../azure-resource-manager/resource-group-authoring-templates.md).

    Bağlantı noktası, parametre adı, reverseProxyEndpointPort tanımlanır.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. SSL sertifikaları için ters proxy bağlantı noktası üzerinde yapılandırmak için sertifikaya eklemek ***reverseProxyCertificate*** özelliğinde **Microsoft.ServiceFabric/clusters** [kaynak türü bölüm](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Küme sertifikası farklı bir ters proxy sertifikasını destekleme
 Ters proxy sertifikasını kümenin güvenliğini sertifikasından farklıysa, ardından daha önce belirtilen sertifika sanal makine üzerinde yüklenmeli ve Service Fabric erişebilmesi erişim denetim listesine (ACL) eklendi. Bu yapılabilir [ **Microsoft.Compute/virtualMachineScaleSets** ](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachinescalesets) [kaynak türü bölümüne](../resource-group-authoring-templates.md). Yükleme için sertifika için osProfile ekleyin. ACL sertifika şablonu uzantısı bölümünü güncelleştirebilirsiniz.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Var olan bir kümede ters proxy etkinleştirmek için küme sertifikası farklı sertifikaları kullandığınızda, ters proxy sertifikasını yükleyin ve ters proxy etkinleştirmeden önce küme üzerinde bir ACL güncelleştirin. Tamamlamak [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md) belirtilen ayarları kullanarak dağıtım daha önce ters proxy etkinleştirmek için bir dağıtım başlamadan önce adımları 1-3.

## <a name="enable-reverse-proxy-on-standalone-clusters"></a>Tek başına kümeler üzerinde ters Proxy'yi Etkinleştir

Tek başına kümeler için ters proxy ClusterConfig.json dosyasında etkinleştirin. Ters proxy küme oluşturma veya var olan bir küme yapılandırmasını yükselterek etkinleştirebilirsiniz. ClusterConfig.json dosyalarında ayarları hakkında daha fazla bilgi edinmek için [tek başına küme ayarlarını](./service-fabric-cluster-manifest.md).

Aşağıdaki adımlar ters proxy etkinleştirmek için kullanılacak ayarları gösterir ve isteğe bağlı olarak, ters proxy X.509 sertifikası ile güvenli hale getirmek için. 

1. Ters proxy etkinleştirmek için **reverseProxyEndpointPort** altında düğüm türü için değer **özellikleri** küme yapılandırmasını. Ters proxy 19081 "NodeType0" türüne sahip düğümleri için uç nokta bağlantı noktası ayarı aşağıdaki JSON'u göstermektedir:

   ```json
       "properties": {
          ... 
           "nodeTypes": [
               {
                   "name": "NodeType0",
                   ...
                   "reverseProxyEndpointPort": "19081",
                   ...
               }
           ],
          ...
       }
   ```
2. (İsteğe bağlı) Güvenli bir ters proxy için bir sertifikaya yapılandırma **güvenlik** bölümüne **özellikleri**. 
   - Geliştirme veya test ortamı için kullanabileceğiniz **ReverseProxyCertificate** ayarı:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificate": {
                          "Thumbprint": "[Thumbprint]",
                          "ThumbprintSecondary": "[Thumbprint]",
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```
   - Bir üretim ortamı için **ReverseProxyCertificateCommonNames** ayarı önerilir:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificateCommonNames": {
                        "CommonNames": [
                            {
                              "CertificateCommonName": "[CertificateCommonName]"
                            }
                          ],
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```

   Ve hakkında daha fazla yapılandırma bir tek başına küme yanı sıra ters proxy güvenliğini sağlamak için kullanılan sertifikaları yapılandırma hakkında daha fazla ayrıntı için sertifikaları yönetme bilgi edinmek için [X509 sertifika tabanlı güvenlik](./service-fabric-windows-cluster-x509-security.md).

Ters proxy etkinleştirmek için ClusterConfig.json dosyanızı değiştirdiğiniz sonra yönergeleri izleyin [küme yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md) kümenize değişiklikleri göndermek için.


## <a name="expose-reverse-proxy-on-a-public-port-through-azure-load-balancer"></a>Ters proxy Azure Load Balancer üzerinden bir genel bağlantı noktasını kullanıma sunma

Ters dışından Ara sunucuya Azure kümesine yönelik olarak, Azure Load Balancer kuralları ve bir Azure sistem durumu araştırması için ters proxy bağlantı noktası ayarlayın. Bu adımlar, kümeyi oluşturduktan sonra herhangi bir zamanda Azure portalı ya da Resource Manager şablonu kullanarak gerçekleştirilebilir. 

> [!WARNING]
> Yük Dengeleyici ters proxy bağlantı noktası yapılandırdığınızda, bir HTTP uç noktasını açığa tüm mikro Hizmetleri kümedeki küme dışında'ten adreslenebilir. Başka bir deyişle, mikro hizmetler iç olacak şekilde tasarlanmış belirlenen kötü niyetli bir kullanıcı tarafından keşfedilebilir olabilir. Bu, potansiyel olarak yararlanılabilir ciddi güvenlik açıklarını sunar; Örneğin:
>
> * Kötü niyetli bir kullanıcı, sürekli olarak yeterli saldırı yüzeyini yok. bir iç hizmet çağırarak bir hizmet reddi saldırısı başlatabilir.
> * Kötü niyetli bir kullanıcının istenmeyen davranışla bir iç hizmet için hatalı biçimlendirilmiş paketlerini teslim edebilir.
> * İç olacak şekilde tasarlanmış bir hizmet, böylece kötü niyetli bir kullanıcı için bu hassas bilgileri gösterme, küme dışındaki hizmetlere açığa amaçlanmaz özel veya hassas bilgiler döndürebilir. 
>
> Tam olarak anlamak ve kümenizi ve ters proxy bağlantı noktası genel yapmadan önce bunun üzerinde çalışan uygulamalar için olası güvenlik sonuçları azaltmak emin olun. 
>

Ters proxy tek başına küme için genel olarak kullanıma sunmak istiyorsanız, bunu bir şekilde kümeyi barındıran sistemde bağlıdır ve bu makalenin kapsamı dışındadır. Ancak, genel olarak, ters proxy gösterme hakkında yukarıdaki uyarı hala geçerlidir.

### <a name="expose-the-reverse-proxy-using-azure-portal"></a>Azure portalını kullanarak bir ters proxy kullanıma sunma 

1. Azure portalında kümenizin kaynak grubuna tıklayın ve ardından kümenizin yük dengeleyiciye tıklayın.
2. Sistem durumu araştırması için ters proxy bağlantı noktası yük dengeleyici penceresinin sol bölmede altında eklemek için **ayarları**, tıklayın **sistem durumu araştırmalarının**. Ardından **Ekle** pencere durumu üst kısmında araştırmaları ve ters proxy bağlantı noktası ayrıntılarını girin ve ardından tıklayın **Tamam**. Kümeyi oluştururken değiştirmediğiniz sürece varsayılan olarak, 19081, ters proxy bağlantı noktasıdır.

   ![Ters proxy sistem durumu araştırmasını yapılandırma](./media/service-fabric-reverseproxy-setup/lb-rp-probe.png)
3. Ters proxy bağlantı noktası, yük dengeleyici penceresinin sol bölmede altında ortaya çıkarmak için yük dengeleyici kuralını eklemek için **ayarları**, tıklayın **Yük Dengeleme kuralları**. Ardından **Ekle** üst kısmında, Yük Dengeleme kuralları penceresi ve ters proxy bağlantı noktası ayrıntılarını girin. Ayarladığınızdan emin olun **bağlantı noktası** , istediğiniz, kullanıma sunulan ters proxy bağlantı noktası değerine **arka uç bağlantı noktası** ters proxy, etkinleştirildiğinde ayarladığınız bağlantı noktasının değerini ve **Durumaraştırması** önceki adımda yapılandırdığınız durum araştırması için değer. Uygun bir şekilde tıklayın diğer alanları **Tamam**.

   ![Ters proxy için yük dengeleyici kuralı yapılandırma](./media/service-fabric-reverseproxy-setup/lb-rp-rule.png)

### <a name="expose-the-reverse-proxy-via-resource-manager-templates"></a>Ters proxy Resource Manager şablonları aracılığıyla kullanıma sunma

Aşağıdaki JSON içinde kullanılan aynı şablonu başvuran [Azure Resource Manager şablonları aracılığıyla ters Proxy'yi Etkinleştir](#enable-reverse-proxy-via-azure-resource-manager-templates). Resource Manager şablonu oluşturma veya var olan bir küme için bir şablonu dışarı aktarma hakkında bilgi edinmek için belgenin, bölümüne bakın.  Değişiklik [ **Microsoft.Network/loadBalancers** ](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers) [kaynak türü bölümüne](../resource-group-authoring-templates.md).

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```


## <a name="customize-reverse-proxy-behavior-using-fabric-settings"></a>Yapı ayarları kullanarak ters proxy davranışını özelleştirme

Ters proxy aracılığıyla Azure veya tek başına kümeler için ClusterConfig.json dosyasında barındırılan kümeleri için Resource Manager şablonunda yapı ayarları davranışını özelleştirebilirsiniz. Ters proxy davranışını denetleyen ayarları yerleştirilir [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) konusundaki **fabricSettings** Kümebölümünde**özellikleri** bölümü. 

Örneğin, değerini ayarlayabilirsiniz **DefaultHttpRequestTimeout** aşağıdaki JSON olduğu gibi 180 saniye için ters proxy istekleri için zaman aşımını ayarlamak için:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "DefaultHttpRequestTimeout",
                   "value": "180"
                 }
               ]
             }
           ],
           ...
   }
   ``` 

Yapı ayarları Azure kümeleri için güncelleştirme hakkında daha fazla bilgi için bkz. [Resource Manager şablonlarını kullanarak küme ayarlarını özelleştirme](service-fabric-cluster-config-upgrade-azure.md). Tek başına kümeler için bkz: [tek başına kümeler için küme ayarlarını Özelleştir](service-fabric-cluster-config-upgrade-windows-server.md). 

Birkaç yapı ayarları ters proxy ve hizmetler arasında güvenli iletişim kurmak amacıyla kullanılır. Bu ayarlar hakkında ayrıntılı bilgi için bkz. [güvenli hizmet ters proxy ile bağlanma](service-fabric-reverseproxy-configure-secure-communication.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenli HTTP hizmetine ileten ters proxy ile ayarlama](service-fabric-reverseproxy-configure-secure-communication.md)
* Ters proxy yapılandırma seçenekleri için bkz. [Applicationgateway'inin/Http özelleştirme Service Fabric küme ayarları bölümünde](service-fabric-cluster-fabric-settings.md#applicationgatewayhttp).
