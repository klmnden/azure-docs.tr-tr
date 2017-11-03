---
title: "Azure Service Fabric kapsayıcı hizmetlerini ağ modlarını yapılandırma | Microsoft Docs"
description: "Kurulum, Azure Service Fabric destekleyen farklı ağ modları öğrenin."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 1ecded3af6396f50e67dc5d2a9ef8337699046ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-container-networking-modes"></a>Service Fabric kapsayıcı ağ modları

Ağ modu sunulan kapsayıcı hizmetlerini Service Fabric kümesindeki varsayılan `nat` ağ modu. İle `nat` ağ modu, birden fazla kapsayıcı hizmeti dağıtım hataları aynı bağlantı noktası sonuçları dinlemek sahip. Bu aynı bağlantı noktasını dinler birkaç çalışan hizmetleri için Service Fabric destekler `open` ağ modu (sürüm 5.7 veya daha yüksek). İle `open` modu ağ, her kapsayıcı hizmeti dahili olarak aynı bağlantı noktasını dinlemek üzere birden çok hizmet veren dinamik olarak atanmış bir IP adresi alır.   

Bu nedenle, hizmet bildiriminde tanımlanan statik bir uç noktası ile tek hizmet türü ile yeni hizmetleri oluşturulabilir ve kullanarak dağıtım hatasız silinmiş `open` ağ modu. Benzer şekilde, aynı kullanabilirsiniz `docker-compose.yml` birden fazla hizmet oluşturmak için dosya statik bağlantı noktası eşlemesi.

Dinamik olarak atanmış IP Hizmetleri değil önerilir itibaren IP adresi değişiklikleri hizmeti yeniden başlatılır veya başka bir düğüme taşır keşfetmek için kullanma. Yalnızca **Service Fabric adlandırma hizmeti** veya **DNS hizmeti** hizmet bulmayı için. 


> [!WARNING]
> Yalnızca 4096 IP'leri toplam Azure vNET başına izin verilir. Bu nedenle, düğüm sayısını ve kapsayıcı hizmeti örneklerinin sayısını toplamı (ile `open` ağ) bir sanal ağ içindeki 4096 aşamaz. Bu tür yüksek yoğunluklu senaryoları için `nat` ağ modu önerilir.
>

## <a name="setting-up-open-networking-mode"></a>Açık ağ modunu ayarlama

1. DNS hizmeti ve IP sağlayıcısı altında etkinleştirerek Azure Resource Manager şablonu ayarlayın `fabricSettings`. 

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

2. Kümedeki her düğümde yapılandırılması birden fazla IP adresine izin vermek için ağ profili bölümünü ayarlamak. Düğüm başına beş IP adreslerini aşağıdaki örnekte kurar (Bu nedenle bağlantı noktası her düğüm üzerinde dinleme beş hizmet örnekleri olabilir) için bir Windows/Linux Service Fabric kümesi.

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
 

3. Yalnızca Windows kümeler için aşağıdaki değerlere sahip vnet UDP/53 numaralı bağlantı noktası açma bir NSG kuralı ayarlayın:

   | Öncelik |    Ad    |    Kaynak      |  Hedef   |   Hizmet    | Eylem |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | İzin Ver  |


4. Her hizmet için uygulama bildiriminde ağ modunu belirtin `<NetworkConfig NetworkType="open">`.  Mod `open` ayrılmış bir IP adresi alma hizmetinde neden olur. Bir mod belirtilmezse, basic varsayılanları `nat` modu. Bu nedenle, aşağıdaki örnekte bildirim, `NodeContainerServicePackage1` ve `NodeContainerServicePackage2` her dinleme bağlantı noktasına olabilir (her iki hizmetlerin dinlediği `Endpoint1`).

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
           <NetworkConfig NetworkType="open"/>
           <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="open"/>
            <PortBinding ContainerPort="8910" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
Karışık ve hizmetlerde Windows kümesi için bir uygulama içinde farklı ağ modları eşleşmesi. Bu nedenle, bazı hizmetler sahip `open` modu ve bazı üzerinde `nat` ağ modu. İle bir hizmeti yapılandırıldığında `nat`, dinleme yaptığı bağlantı noktası benzersiz olmalıdır. Farklı Hizmetleri için ağ modlarını karıştırma Linux kümeleri üzerinde desteklenmiyor. 


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Service Fabric tarafından sunulan modları ağ hakkında öğrendiniz.  

* [Service Fabric uygulama modeli](service-fabric-application-model.md)
* [Service Fabric hizmet bildirimi kaynakları](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources)
* [Windows Server 2016 Service Fabric Windows kapsayıcı dağıtma](service-fabric-get-started-containers.md)
* [Service Fabric Linux'ta Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
