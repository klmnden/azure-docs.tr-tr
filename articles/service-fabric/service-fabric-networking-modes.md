---
title: Azure Service Fabric kapsayıcı Hizmetleri için ağ modları yapılandırma | Microsoft Docs
description: Azure Service Fabric tarafından desteklenen farklı ağ modları ayarlama konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: 6f14b3184cabd1dfd84f04260f6b8c831037cbcf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60718164"
---
# <a name="service-fabric-container-networking-modes"></a>Service Fabric kapsayıcı ağ modları

Bir Azure Service Fabric kümesi için kapsayıcı hizmetleri kullanan **nat** varsayılan olarak ağ modu. Birden fazla kapsayıcı hizmeti, aynı bağlantı noktasında dinleme ve nat modu kullanılan dağıtım hataları oluşabilir. Birden çok kapsayıcı Hizmetleri aynı bağlantı noktasında dinleme desteklemek için Service Fabric sunar **açık** ağ modu (5.7 ve sonraki sürümler). Açık modda, aynı bağlantı noktasında dinleme birden çok hizmeti destekleyen bir IP adresi dinamik olarak atanan her kapsayıcı hizmeti bir iç, sahiptir.  

Statik bir uç noktası ile bir kapsayıcı hizmeti, hizmet bildiriminde varsa, oluşturun ve dağıtım hata olmadan açık modunu kullanarak yeni hizmetler silin. Aynı docker-compose.yml dosyası birden çok hizmeti oluşturmak için statik bağlantı noktası eşlemelerini ile de kullanılabilir.

Bir kapsayıcı hizmeti yeniden başlatılıyor veya kümedeki başka bir düğüme taşır, IP adresi değişir. Bu nedenle, kapsayıcı Hizmetleri bulmak için dinamik olarak atanan IP adresini kullanarak önermemekteyiz. Yalnızca Service Fabric adlandırma ağ geçidi veya DNS hizmeti için hizmet bulma kullanılmalıdır. 

>[!WARNING]
>Azure sanal ağ başına 65.356 IP'lerin toplam sağlar. Düğüm sayısını ve kapsayıcı hizmeti (kullandığınız modunu açın) bir örnek sayısı toplamı, bir sanal ağ içindeki 65.356 IP'ler aşamaz. Yüksek yoğunluklu senaryoları için nat ağ modu öneririz. Ayrıca, yük dengeleyici gibi diğer bağımlılıklara diğer olacaktır [sınırlamaları](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits) değerlendirilecek. Şu anda en çok 50 IP'ler düğüm başına test ve kararlı kanıtlanmış. 
>

## <a name="set-up-open-networking-mode"></a>Açık ağ Modu'nu ayarla

1. Azure Resource Manager şablonu ayarlayın. İçinde **fabricSettings** bölümüne küme kaynağı, DNS hizmeti ve IP sağlayıcısı etkinleştir: 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```
    
2. Sanal makine ölçek kümesi kaynak ağ profili bölümünü ayarlayın. Bu, kümedeki her düğümde yapılandırılması birden çok IP adresi sağlar. Aşağıdaki örnek bir Windows/Linux Service Fabric kümesi için düğüm başına beş adet IP adresi ayarlar. Her düğümde bağlantı noktasını dinleyen beş hizmet örnekleri olabilir. Beş Azure yük Dengeleyiciden erişilebilir IP sağlamak için aşağıda gösterildiği gibi beş IP'ler Azure yük dengeleyici arka uç adres havuzunu kaydedin.  Değişkenler bölümünde şablonunuzda üstüne değişkenleri eklemek gerekir.

    Bu bölümde, değişkenleri ekleyin:

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    ```
    
    Bu bölümde, sanal makine ölçek kümesi kaynak ekleyin:

    ```json   
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 
3. Yalnızca Windows kümeleri için aşağıdaki değerlerle sanal ağ için bağlantı noktası UDP/53'kurmak açılır bir Azure ağ güvenlik grubu (NSG) kuralı ayarlayın:

   |Ayar |Değer | |
   | --- | --- | --- |
   |Öncelik |2000 | |
   |Ad |Custom_Dns  | |
   |Kaynak |VirtualNetwork | |
   |Hedef | VirtualNetwork | |
   |Hizmet | DNS (UDP/53) | |
   |Eylem | İzin Ver  | |
   | | |

4. Her hizmet için uygulama bildiriminde ağ modu belirtin: `<NetworkConfig NetworkType="Open">`. **Açık** modu sonuçları bir ayrılmış IP adresini alma hizmetinde ağ oluşturma. Hizmet bir modu belirtilmezse, varsayılan **nat** modu. Aşağıdaki örnekte liste, `NodeContainerServicePackage1` ve `NodeContainerServicePackage2` hizmetleri her aynı bağlantı noktasını dinler kullanabilirsiniz (her iki hizmet de dinlemede `Endpoint1`). Ağ modunu açın belirtildiğinde `PortBinding` yapılandırmaları belirtilemez.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```

    Karışık ve uygulama için bir Windows kümesi içindeki hizmetler arasında farklı ağ modları eşleşmesi. Bazı hizmetler, kullanılırken diğerlerinde nat modu açık modunu kullanabilirsiniz. Bir hizmet, nat modunu kullanacak şekilde yapılandırıldığında, hizmet dinleme yaptığı bağlantı noktası benzersiz olmalıdır.

    >[!NOTE]
    >Farklı Hizmetleri için ağ modları karıştırma Linux kümelerinde desteklenmez. 
    >

5. Zaman **açık** modu seçilidir ve **uç nokta** hizmet bildirimindeki tanım açıkça işaret etmelidir uç noktasına karşılık gelen kod paketi için hizmet paketi tek bir kod olsa bile Bu paketi. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```
   
6. Windows için VM yeniden başlatma, yeniden oluşturulması açık ağ neden olur. Bu, ağ yığınını, temel alınan bir sorunu azaltmak içindir. Ağ yeniden oluşturmak için varsayılan davranıştır. Bu davranışı devre dışı bırakılması gerekiyorsa, bir yapılandırma yükseltmenin ardından aşağıdaki yapılandırma kullanılabilir.

```json
"fabricSettings": [
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "SkipContainerNetworkResetOnReboot",
                            "value": "true"
                    }
                    ]
                }
            ],          
 ``` 
 
## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)
* [Service Fabric hizmet bildirimi kaynakları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Service fabric'e Windows Server 2016 üzerinde bir Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux üzerinde Service Fabric için bir Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
