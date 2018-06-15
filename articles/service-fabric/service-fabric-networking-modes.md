---
title: Azure Service Fabric kapsayıcı hizmetlerini ağ modlarını yapılandırma | Microsoft Docs
description: Azure Service Fabric tarafından desteklenen farklı ağ modlarını ayarlama öğrenin.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f831c046bcf8f633841f9dc4a0fce6d1e419e6c2
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205663"
---
# <a name="service-fabric-container-networking-modes"></a>Service Fabric kapsayıcı ağ modları

Kapsayıcı için bir Azure Service Fabric kümesi kullanan Hizmetleri **nat** varsayılan olarak ağ modu. Birden fazla kapsayıcı hizmeti aynı bağlantı noktasında dinleme ve nat modu kullanılan dağıtım hataları oluşabilir. Aynı bağlantı noktasında dinleme birden çok kapsayıcı hizmetlerini desteklemek için Service Fabric sunar **açık** ağ modu (5.7 ve sonraki sürümler). Açık modunda her kapsayıcı hizmeti bir iç, aynı bağlantı noktasında dinleme birden çok hizmetlerini destekleyen bir IP adresi dinamik olarak atanmış sahiptir.  

Hizmet bildiriminizi statik bir uç noktası ile bir kapsayıcı hizmeti varsa, oluşturabilir ve yeni hizmetler dağıtım hatasız açık modunu kullanarak silebilirsiniz. Aynı docker-compose.yml dosyası birden çok hizmet oluşturmak için statik bağlantı noktası eşlemelerini ile de kullanılabilir.

Bir kapsayıcı hizmetini yeniden başlatır veya kümedeki başka bir düğüme taşır, IP adresini değiştirir. Bu nedenle, kapsayıcı hizmetlerini bulmak için dinamik olarak atanmış IP adresini kullanarak önerilmez. Yalnızca Service Fabric adlandırma hizmeti veya DNS hizmeti, hizmet bulma için kullanılmalıdır. 

>[!WARNING]
>Azure sanal ağ başına 4.096 IP'leri toplam sağlar. Düğüm sayısını ve (açık modunu kullanarak) kapsayıcı hizmeti örneklerinin sayısını toplamı 4.096 IP'leri bir sanal ağ içindeki aşamaz. Yüksek yoğunlukta senaryoları için nat ağ modu öneririz.
>

## <a name="set-up-open-networking-mode"></a>Açık ağ Modu'nu ayarla

1. Azure Resource Manager şablonu ayarlayın. İçinde **fabricSettings** bölümünde, DNS hizmeti ve IP sağlayıcısı etkinleştir: 

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
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
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

2. Kümedeki her düğümde yapılandırılması birden fazla IP adresine izin vermek için ağ profili bölümünü ayarlamak. Aşağıdaki örnek Windows/Linux Service Fabric kümesi için düğüm başına beş IP adresini ayarlar. Her düğümde bağlantı noktasında dinleme beş hizmet örneği olabilir.

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
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
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
 
3. Yalnızca Windows kümeler için aşağıdaki değerlere sahip sanal ağ için UDP/53 numaralı bağlantı noktası açar bir Azure ağ güvenlik grubu (NSG) kuralı ayarlayın:

   |Ayar |Değer | |
   | --- | --- | --- |
   |Öncelik |2000 | |
   |name |Custom_Dns  | |
   |Kaynak |VirtualNetwork | |
   |Hedef | VirtualNetwork | |
   |Hizmet | DNS (UDP/53) | |
   |action | İzin ver  | |
   | | |

4. Her hizmet için uygulama bildiriminde ağ modunu belirtin: `<NetworkConfig NetworkType="Open">`. **Açık** modu sonuçları bir ayrılmış IP adresi alma hizmetindeki ağ. Hizmet bir mod belirtilmezse, varsayılan **nat** modu. Aşağıdaki örnekte bildirim, `NodeContainerServicePackage1` ve `NodeContainerServicePackage2` hizmetleri her aynı bağlantı noktasını dinler olabilir (her iki hizmetlerin dinlediği `Endpoint1`). Açık ağ modu belirtildiğinde, `PortBinding` yapılandırmaları belirtilemez.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
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

    Karışık ve hizmetlerde Windows kümesi için bir uygulama içinde farklı ağ modları eşleşmesi. Başkalarının nat modu kullanırken bazı hizmetler açık modunu kullanabilirsiniz. Bir hizmet nat modunu kullanacak şekilde yapılandırıldığında, üzerinde dinleyen bir hizmet bağlantı noktası benzersiz olmalıdır.

    >[!NOTE]
    >Linux kümelerinde farklı Hizmetleri için ağ modları karıştırılması desteklenmiyor. 
    >

5. Zaman **açık** modu seçildiğinde, **Endpoint** hizmet bildirimi tanımında açıkça noktasına uç noktasına karşılık gelen kod paketi hizmet paketi yalnızca bir kod olsa bile Bu paketi. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)
* [Service Fabric hizmet bildirimi kaynakları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Windows Server 2016 Service Fabric Windows kapsayıcı dağıtma](service-fabric-get-started-containers.md)
* [Service Fabric Linux'ta Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
