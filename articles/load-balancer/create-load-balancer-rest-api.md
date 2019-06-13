---
title: REST API kullanarak Azure Load Balancer oluşturma
titlesuffix: Azure Load Balancer
description: REST API kullanarak bir Azure yük dengeleyici oluşturmayı öğrenin.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: load-balancer
ms.date: 06/06/2018
ms.author: kumud
ms.openlocfilehash: 159fe9d6a891858d8d2cc2315e9544b79eb44cff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884988"
---
# <a name="create-an-azure-basic-load-balancer-using-rest-api"></a>Bir Azure temel REST API kullanarak yük dengeleyici oluşturma

Azure Load Balancer arka uç havuzu örneklerine, kurallar ve sistem durumu araştırmaları göre load balancer'ın ön uç üzerinde geldiğinde yeni gelen akışlar dağıtır. Load Balancer iki Sku'da kullanılabilir: Temel ve Standart. İki SKU sürümü arasındaki farkı anlamak için [yük dengeleyici SKU karşılaştırmalar](load-balancer-overview.md#skus).
 
Bu nasıl yapılır kullanarak bir temel Azure Load Balancer oluşturulacağı gösterilmektedir [Azure REST API'si](/rest/api/azure/) bir Azure sanal ağ içindeki birden çok VM arasında dengeleme gelen istek yükünü dengeleyebilmek için. Eksiksiz başvuru belgeleri ve ek örnekleri kullanılabilir [Azure yük dengeleyici REST başvurusu](/rest/api/load-balancer/).
 
## <a name="build-the-request"></a>Derleme isteği
Yeni Azure temel yük dengeleyici oluşturmak için aşağıdaki HTTP PUT İsteği'ni kullanın.
 ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}?api-version=2018-02-01
  ```
### <a name="uri-parameters"></a>URI parametreleri

|Ad  |İçinde  |Gerekli |Tür |Açıklama |
|---------|---------|---------|---------|--------|
|subscriptionId   |  yol       |  True       |   string      |  Microsoft Azure aboneliği benzersiz olarak tanımlanabilmesi abonelik kimlik bilgileri. Abonelik kimliği, her hizmet çağrısı için URI parçası oluşturur.      |
|resourceGroupName     |     yol    | True        |  string       |   Kaynak grubunun adı.     |
|loadBalancerName     |  yol       |      True   |    string     |    Yük dengeleyicinin adı.    |
|API sürümü    |   sorgu     |  True       |     string    |  İstemci API sürümü.      |



### <a name="request-body"></a>İstek gövdesi

Tek gerekli parametresi `location`. Değil tanımlarsanız *SKU* sürümü, bir temel yük dengeleyici varsayılan olarak oluşturulur.  Kullanım [isteğe bağlı parametreler](https://docs.microsoft.com/rest/api/load-balancer/loadbalancers/createorupdate#request-body) yük dengeleyici özelleştirmek için.

| Ad | Tür | Açıklama |
| :--- | :--- | :---------- |
| location | string | Kaynak konumu. Konumları kullanarak geçerli bir listesini alın [List Locations](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations) işlemi. |


## <a name="example-create-and-update-a-basic-load-balancer"></a>Örnek: Oluşturma ve bir temel yük dengeleyici güncelleştirme

Bu örnekte, ilk kaynaklarıyla birlikte bir temel yük dengeleyici oluşturun. Ardından, bir ön uç IP yapılandırması, bir arka uç adres havuzu, bir Yük Dengeleme kuralı, bir durum araştırması ve gelen NAT kuralı içeren yük dengeleyici kaynaklarının yapılandırın.

Aşağıdaki örneği kullanarak bir yük dengeleyici oluşturmadan önce adlı bir sanal ağ oluşturma *vnetlb* adlı bir alt ağ ile *subnetlb* adlı bir kaynak grubu içinde *rg1* içinde **Doğu ABD** konumu.

### <a name="step-1-create-a-basic-load-balancer"></a>1. ADIMI. Temel Yük Dengeleyici oluşturma
Bu adımda, oluşturduğunuz adlı bir temel yük dengeleyici, *lb* adresindeki **Doğu ABD** içindeki konum *rg1* kaynak grubu.
#### <a name="sample-request"></a>Örnek istek

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>İstek gövdesi

  ```JSON
   {
    "location": "eastus",
   }
  ```
### <a name="step-2-configure-load-balancer-resources"></a>2. ADIMI. Yük Dengeleyici kaynakları yapılandırma
Bu adımda yük dengeleyici yapılandırma *lb* bir ön uç IP yapılandırması içeren kaynakları (*fe lb*), bir arka uç adres havuzu (*olması lb*), Yük Dengeleme kuralı ( *rulelb*), bir durum araştırması (*lb araştırma*) ve gelen NAT kuralı (*içinde-nat-rule*).
#### <a name="sample-request"></a>Örnek istek

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>İstek gövdesi

  ```JSON
{
  "properties": {
    "frontendIPConfigurations": [
      {
        "name": "fe-lb",
        "properties": {
          "subnet": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/vnetlb/subnets/subnetlb"
          },
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ],
          "inboundNatRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/inboundNatRules/in-nat-rule"
            }  ]  }  }  ],
    "backendAddressPools": [
      {
        "name": "be-lb",
        "properties": {
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]   }   }   ],
    "loadBalancingRules": [
      {
        "name": "rulelb",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 80,
          "backendPort": 80,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp",
          "loadDistribution": "Default",
          "backendAddressPool": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/be-lb"
          },
          "probe": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/probes/probe-lb"
          }  }  }  ],
    "probes": [
      {
        "name": "probe-lb",
        "properties": {
          "protocol": "Http",
          "port": 80,
          "requestPath": "healthcheck.aspx",
          "intervalInSeconds": 15,
          "numberOfProbes": 2,
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]  }  } ],
    "inboundNatRules": [
      {
        "name": "in-nat-rule",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 3389,
          "backendPort": 3389,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp"
        } } ],
    "inboundNatPools": [],
    "outboundNatRules": []
  }  }
```
