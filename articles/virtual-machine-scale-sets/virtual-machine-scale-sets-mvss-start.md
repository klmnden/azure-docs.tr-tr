---
title: Sanal makine ölçek kümesi şablonları hakkında bilgi edinin | Microsoft Docs
description: Sanal makine ölçek kümeleri için basit bir ölçek kümesi şablon oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: 8b6a6b78dc74572b22d397b5536efa1394401bbc
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868919"
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Sanal makine ölçek kümesi şablonları hakkında bilgi edinin
[Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment), ilgili kaynak gruplarını dağıtmanın harika bir yoludur. Bu öğretici serisinde, basit bir ölçek kümesi şablonunun nasıl oluşturulacağını ve çeşitli senaryolara uygun olarak bu şablonu nasıl değiştireceğiniz gösterilmektedir. Tüm örnekler buradan gelen [GitHub deposu](https://github.com/gatneil/mvss).

Bu şablon, basit olması amaçlanmıştır. Ölçeğin daha ayrıntılı örnekler için şablonlar, bakın [Azure hızlı başlangıç şablonları GitHub deposunda](https://github.com/Azure/azure-quickstart-templates) ve arama dizesini içeren klasörlere `vmss`.

Şablonları oluşturma konusunda bilginiz varsa, bu şablonu nasıl değiştireceğiniz görmek için "Sonraki adımlar" bölümüne atlayabilirsiniz.

## <a name="define-schema-and-contentversion"></a>Define $schema and contentVersion
Öncelikle, tanımlamanız `$schema` ve `contentVersion` şablondaki. `$schema` Öğe şablonu dil sürümünü tanımlar ve Visual Studio söz dizimi vurgulama ve benzer doğrulama özellikleri için kullanılır. `contentVersion` Öğesi, Azure tarafından kullanılmaz. Bunun yerine, şablon sürümünü izlemenize yardımcı olur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```

## <a name="define-parameters"></a>Parametreleri tanımlayın
Ardından, iki parametre tanımlama `adminUsername` ve `adminPassword`. Parametreleri, dağıtım sırasında belirttiğiniz değerlerdir. `adminUsername` Parametresi, yalnızca bir `string` türü, ancak `adminPassword` olduğundan bu tür bir gizli dizi, verin `securestring`. Daha sonra bu parametreleri ile ölçek kümesi yapılandırması geçirilir.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Değişkenleri tanımlama
Resource Manager şablonları, ayrıca daha sonra şablonunda kullanılacak değişkenleri tanımlayın olanak tanır. JSON nesnesi boş, bu nedenle, örnek tüm değişkenler kullanmaz.

```json
  "variables": {},
```

## <a name="define-resources"></a>Kaynakları tanımlama
Sonraki şablon kaynakları bölümünde bulunur. Burada, hangi gerçekten dağıtmak istediğiniz tanımlayın. Farklı `parameters` ve `variables` (JSON nesneleri olan), `resources` JSON nesnesi, JSON listesidir.

```json
   "resources": [
```

Tüm kaynakları gerektiren `type`, `name`, `apiVersion`, ve `location` özellikleri. Bu örneğin ilk kaynak türünde [Microsoft.Network/virtualNetwork](/azure/templates/microsoft.network/virtualnetworks), adı `myVnet`ve apiVersion `2018-11-01`. (Bir kaynak türü için en son API sürümü bulmak için bkz: [Azure Resource Manager şablon başvurusu](/azure/templates/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2018-11-01",
```

## <a name="specify-location"></a>Konumu belirtin
Sanal ağ konumunu belirtmek için kullanın. bir [Resource Manager şablon işlevi](../azure-resource-manager/resource-group-template-functions.md). Bu işlev teklifleri ve bunun gibi köşeli ayraç içine alınması gerekir: `"[<template-function>]"`. Bu durumda `resourceGroup` işlevi. Bu hiçbir bağımsız değişkeni alır ve bu dağıtım yapılmakta kaynak grubu hakkında meta veriler içeren bir JSON nesnesi döndürür. Kaynak grubu, dağıtım sırasında kullanıcı tarafından ayarlanır. Bu değer, bu ile JSON nesnesi olarak dizine alınır `.location` JSON nesnesinden konumu alınamıyor.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Sanal ağ özelliklerini belirtin
Her bir Resource Manager kaynak kendi bölümüne sahiptir `properties` kaynağa özgü yapılandırmalar için bölüm. Bu durumda, sanal ağ özel IP adresi aralığını kullanarak tek bir alt ağda olması gerektiğini belirtin `10.0.0.0/16`. Bir ölçek kümesi, her zaman bir alt ağ içinde yer alır. Bu alt ağlara yayılamaz.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>DependsOn listeye ekleyin
Ek olarak gerekli `type`, `name`, `apiVersion`, ve `location` özellikleri, her kaynak bir isteğe bağlı olabilir `dependsOn` dize listesi. Bu liste, bu kaynak dağıtmadan önce bu dağıtım diğer kaynaklardan bitmesi gereken belirtir.

Bu durumda, var. yalnızca bir öğe listesinde, önceki örnekte sanal ağ Herhangi bir VM oluşturmadan önce mevcut ağa kümesinin ölçeğini olmalıdır çünkü bu bağımlılık belirtin. Bu şekilde, Ölçek kümesi daha önce Ağ Özellikleri'nde belirtilen IP adresi aralığından bu VM'lerin özel IP adreslerini verin. DependsOn listesindeki her bir dizenin biçimi `<type>/<name>`. Aynı `type` ve `name` sanal ağın kaynak tanımı'nda önceden kullanılmış.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2019-03-01",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Ölçek kümesi özelliklerini belirtin
Ölçek kümeleri ölçek kümesindeki sanal makineler özelleştirmeye yönelik pek çok özellikleri vardır. Bu özelliklerin tam listesi için bkz. [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachinescalesets). Bu öğretici için yalnızca birkaç yaygın olarak kullanılan özellikler ayarlanır.
### <a name="supply-vm-size-and-capacity"></a>VM boyutu ve kapasite sağlayın
Ölçek kümesi ("sku adı") oluşturmak için VM boyutu bilmeniz gerekir ve böyle kaç VM ("sku kapasitesi") oluşturun. Hangi VM boyutlarının kullanılabilir olduğunu görmek için bkz: [VM boyutları belgeleri](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Güncelleştirmelerin türünü seçin
Ayrıca ölçek, Ölçek kümesindeki güncelleştirmelerin nasıl ele alınacağını bilmek ister. Şu anda üç seçenek vardır `Manual`, `Rolling` ve `Automatic`. İkisi arasındaki farklar hakkında daha fazla bilgi için belgelere bakın [bir ölçek kümesi yükseltme](./virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>VM işletim sistemini seçin
Yerleştirme Vm'lerde hangi işletim sistemi bilmeniz gerekir ölçek kümesi. Burada, sanal makineler ile tam olarak düzeltme eki uygulanmış bir Ubuntu 16.04 LTS görüntüsünü oluşturun.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>ComputerNamePrefix belirtin
Birden fazla VM ölçek kümesi dağıtır. Her bir VM adı belirtmek yerine belirtin `computerNamePrefix`. Böylece VM adları form ölçek kümesi bir dizin ön eki her VM için ekler `<computerNamePrefix>_<auto-generated-index>`.

Aşağıdaki kod parçacığında, Ölçek kümesindeki tüm sanal makineler için yönetici kullanıcı adı ve parola ayarlamak için önce parametrelerini kullanın. Bu işlemi kullanır `parameters` şablon işlevi. Bu işlev, başvurmak için hangi parametre belirtir ve bu parametre için değer veren bir dize alır.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM ağ yapılandırması belirtin
Son olarak, Ölçek kümesindeki sanal makineleri için ağ yapılandırması belirtin. Bu durumda, yalnızca daha önce oluşturduğunuz alt ağ Kimliğini belirtmeniz gerekir. Bu, bu alt ağda ağ arabirimlerine yerleştirin ölçek bildirir.

Kullanarak alt ağ içeren sanal ağ Kimliğini alabilirsiniz `resourceId` şablon işlevi. Bu işlev, bir kaynağın adını ve türünü alır ve bu kaynağın tam tanımlayıcısını döndürür. Bu kimliği biçime sahiptir: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Ancak, sanal ağ tanıtıcısı yeterli değildir. Ölçek kümesi Vm'lerine belirli alt olması belirtin. Bunu yapmak için birleştirme `/subnets/mySubnet` sanal ağ kimliği. Sonuç alt ağın tam kimliğidir. Bu birleştirme ile yapmak `concat` dizelerin sıralı alır ve kendi birleştirme döndüren işlev.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
