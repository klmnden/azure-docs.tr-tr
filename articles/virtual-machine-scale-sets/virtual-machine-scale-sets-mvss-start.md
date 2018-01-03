---
title: "Sanal makine ölçek kümesi şablonları hakkında bilgi edinin | Microsoft Docs"
description: "Sanal makine ölçek kümeleri için minimum uygun ölçek kümesi şablon oluşturmayı öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: 5cd495d1332c71d7eae775f933b73d98826f10e4
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Sanal makine ölçek kümesi şablonları hakkında bilgi edinin
[Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment), ilgili kaynak gruplarını dağıtmanın harika bir yoludur. Bu öğretici serisi minimum uygun ölçek kümesi şablonunun nasıl oluşturulacağını ve çeşitli senaryoları uyacak şekilde bu şablonu nasıl değiştireceğiniz gösterilir. Tüm örnekler öğesinden gelen [GitHub deposunu](https://github.com/gatneil/mvss). 

Bu şablon, basit olması amaçlanmıştır. Ölçek daha kapsamlı örnekleri için şablonlar, bakın [Azure hızlı başlangıç şablonlarını GitHub deposunu](https://github.com/Azure/azure-quickstart-templates) ve arama dizesini içeren klasörler için `vmss`.

Şablonları oluşturma konusunda bilginiz varsa, bu şablonu nasıl değiştireceğiniz görmek için "Sonraki adımlar" bölümüne atlayabilirsiniz.

## <a name="review-the-template"></a>Şablon gözden geçirin

En düşük uygun ölçek kümesi şablon gözden geçirmek için GitHub kullanın [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

Bu öğreticide, fark inceleyelim (`git diff master minimum-viable-scale-set`) en düşük uygun ölçek oluşturmak için şablon tarafından parça parça ayarlayın.

## <a name="define-schema-and-contentversion"></a>$Schema ve contentVersion tanımlayın
Öncelikle, tanımlamanız `$schema` ve `contentVersion` şablondaki. `$schema` Öğesi şablonu dil sürümünü tanımlar ve Visual Studio söz dizimi vurgulama ve benzer doğrulama özellikleri için kullanılır. `contentVersion` Öğesi Azure tarafından kullanılmaz. Bunun yerine, şablonu sürümü izlemenize yardımcı olur.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>parametrelerini tanımlayın
Ardından, iki parametre tanımlayın `adminUsername` ve `adminPassword`. Parametreleri, dağıtım sırasında belirttiğiniz değerleridir. `adminUsername` Parametredir yalnızca bir `string` türü, ancak çünkü `adminPassword` yazın gizli, bir verin olduğu `securestring`. Daha sonra bu parametreler ölçek kümesi yapılandırmaya geçirilir.

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
Resource Manager şablonları aynı zamanda, daha sonra şablonunda kullanılacak değişkenler tanımlamanıza olanak sağlar. JSON nesnesi boş olması için örnek herhangi bir değişkeni kullanmaz.

```json
  "variables": {},
```

## <a name="define-resources"></a>Kaynakları tanımlayın
Sonraki şablon kaynakları bölümünde bulunur. Burada, aslında dağıtmak istediğinizi tanımlayabilirsiniz. Farklı `parameters` ve `variables` (JSON nesnelerinin olan), `resources` JSON nesnelerinin JSON listesidir.

```json
   "resources": [
```

Tüm kaynak gerektiren `type`, `name`, `apiVersion`, ve `location` özellikleri. Bu örneğin ilk kaynak türüne sahip `Microsft.Network/virtualNetwork`, adı `myVnet`ve apiVersion `2016-03-30`. (Kaynak türü için son API sürümü bulmak için bkz: [Azure REST API belgeleri](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Konumu belirtin
Sanal ağ konumunu belirtmek için kullanın bir [Resource Manager şablonu işlevi](../azure-resource-manager/resource-group-template-functions.md). Bu işlev teklifleri ve bu gibi köşeli ayraçlar içinde alınmalıdır: `"[<template-function>]"`. Bu durumda, kullanarak `resourceGroup` işlevi. İçinde bağımsız değişken almayan ve bu dağıtım dağıtıldığı kaynak grubu hakkındaki meta verileri içeren bir JSON nesnesi döndürür. Kaynak grubu dağıtım zamanında kullanıcı tarafından ayarlanır. Bu değer daha sonra bu JSON nesnesinin ile içine dizine `.location` JSON nesnesinden konumunu almak için.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Sanal ağ özelliklerini belirtin
Her bir Resource Manager kaynak kendi sahip `properties` kaynağa özgü yapılandırmalar için bölüm. Bu durumda, sanal ağ özel IP adres aralığını kullanarak tek bir alt ağda olması gerektiğini belirtirseniz `10.0.0.0/16`. Ölçek kümesi her zaman bir alt ağ içinde yer alır. Alt ağlar yayılamaz.

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
Ek olarak gerekli `type`, `name`, `apiVersion`, ve `location` özellikleri, her bir kaynağın isteğe bağlı bir olabilir `dependsOn` dize listesi. Bu liste, bu kaynak dağıtmadan önce bu dağıtım diğer kaynaklardan bitmesi gereken belirtir.

Bu durumda, var. yalnızca tek bir öğe listesinde, önceki örnekten sanal ağ Tüm sanal makineleri oluşturmadan önce mevcut ağa ölçek kümesinin çünkü bu bağımlılık belirtin. Bu şekilde, Ölçek kümesi Ağ Özellikleri'nde daha önce belirtilen IP adresi aralığından bu VM'ler özel IP adreslerini verin. DependsOn listedeki her dize biçimi `<type>/<name>`. Aynı `type` ve `name` sanal ağ kaynak tanımı'nda önceden kullanılmış.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Ölçek kümesi özelliklerini belirtin
Ölçek kümesi VM ölçek kümesindeki özelleştirmek için birçok özelliği vardır. Bu özelliklerin tam listesi için bkz: [ölçek kümesi REST API belgeleri](https://docs.microsoft.com/rest/api/virtualmachinescalesets/create-or-update-a-set). Bu öğretici için yalnızca birkaç yaygın olarak kullanılan özellikler ayarlanır.
### <a name="supply-vm-size-and-capacity"></a>VM boyutu ve kapasite sağlayın
Ölçek kümesi boyutu ("sku adı") oluşturmak için VM bilmeniz gerekir ve bu tür kaç VM'ler ("sku kapasitesi") oluşturun. Hangi VM boyutları kullanılabilir olduğunu görmek için bkz: [VM boyutları belgelerine](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Güncelleştirmelerin türünü seçin
Ayrıca ölçek ölçek kümesindeki güncelleştirmelerin nasıl ele alınacağını bilmek ister. Şu anda iki seçenek vardır `Manual` ve `Automatic`. İkisi arasındaki farklar hakkında daha fazla bilgi için belgelere bakın [ölçek kümesini yükseltme](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>VM işletim sistemini seçin
Ölçek sanal makinelerin koymak için hangi işletim sistemi bilmeniz gereksinimlerini ayarlayın. Burada, sanal makineleri bir tam olarak düzeltme eki Ubuntu 16.04 LTS görüntüsüyle oluşturun.

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
Birden çok VM ölçek kümesi dağıtır. Her bir VM adı belirtmek yerine belirtin `computerNamePrefix`. Formun VM adlara sahip şekilde ölçek kümesini dizin ön eki her bir VM ekler `<computerNamePrefix>_<auto-generated-index>`.

Aşağıdaki kod parçacığında, Parametreler önce ölçek kümesindeki tüm VM'ler için yönetici kullanıcı adı ve parolayı ayarlamak için kullanın. Bu işlem kullanır `parameters` şablon işlevi. Bu işlev başvurmak için hangi parametresi belirtir ve bu parametre için değer çıkaran bir dize olarak alır.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>VM ağ yapılandırması belirtin
Son olarak, Ölçek kümesindeki sanal makineleri için ağ yapılandırması belirtin. Bu durumda, yalnızca daha önce oluşturduğunuz alt ağ kimliği belirtmeniz gerekir. Bu alt ağda ağ arabirimleri yerleştirilecek kümesini söyler.

Alt ağ kullanarak içeren sanal ağ kimliği alabilirsiniz `resourceId` şablon işlevi. Bu işlev türü ve kaynağın adını alır ve bu kaynağın tam tanımlayıcısını döndürür. Bu kimliği biçime sahiptir:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Ancak, sanal ağ tanıtıcısı yeterli değildir. Ölçek VM'ler kümesi belirli alt olması gerektiğini belirtin. Bunu yapmak için birleştirme `/subnets/mySubnet` sanal ağ kimliği. Sonuç, alt ağın tam kimliğidir. Bu birleştirme ile yapmak `concat` dizeleri bir dizi alır ve kendi birleştirme döndüren işlev.

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
