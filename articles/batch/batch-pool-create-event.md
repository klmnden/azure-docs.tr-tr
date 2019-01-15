---
title: Azure Batch havuz oluşturma olayı | Microsoft Docs
description: Başvuru için Batch havuzu oluşturma olayı.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: 794b3c83ff58967ef8169bed98f7b369335029ae
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259855"
---
# <a name="pool-create-event"></a>Havuz oluşturma olayı

 Bu olay, bir havuzu oluşturulduktan sonra yayınlanır. Günlük içeriği havuzu hakkındaki genel bilgileri açığa çıkarır. Hedef boyutu havuzun işlem düğümleri 0 değerinden ise bir havuz yeniden boyutlandırma başlangıç olayı hemen bu olaydan sonra takip edeceğini unutmayın.

 Aşağıdaki örnek, gövdesi bir havuz oluşturma olayı CloudServiceConfiguration özelliği kullanılarak oluşturulan bir havuz için gösterir.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuz kimliği.|
|displayName|Dize|Havuzun görünen adı.|
|vmSize|Dize|Havuzundaki sanal makinelerin boyutu. Bir havuzdaki tüm sanal makineler aynı boyuttadır. <br/><br/> Havuzları (cloudServiceConfiguration ile oluşturulan havuzlar), bilgi kullanılabilir boyutları hakkında sanal makineler için bulut Hizmetleri için bkz [Cloud Services boyutları](https://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch dışında tüm Cloud Services sanal makine boyutlarını destekler `ExtraSmall`.<br/><br/> Kullanılabilir VM hakkında bilgi için bkz: boyutları (virtualMachineConfiguration ile oluşturulan havuzlar) sanal makineler Market görüntüleri kullanarak havuzlar için [sanal makine boyutları](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) veya [sanal boyutları Makineleri](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Batch `STANDARD_A0` ve premium depolama alanına sahip olanlar (`STANDARD_GS`, `STANDARD_DS` ve `STANDARD_DSV2` serisi) dışında tüm Azure sanal makinelerini destekler.|
|[cloudServiceConfiguration](#bk_csconf)|Karmaşık Tür|Havuz için bulut hizmeti yapılandırması.|
|[virtualMachineConfiguration](#bk_vmconf)|Karmaşık Tür|Sanal Makine Yapılandırması havuzu için.|
|[networkConfiguration](#bk_netconf)|Karmaşık Tür|Havuz ağ yapılandırması.|
|resizeTimeout|Zaman|Havuz üzerindeki son yeniden boyutlandırma işlemi için belirtilen havuz işlem düğümlerinin ayrılması için zaman aşımı.  (Havuz oluşturulduğunda ilk boyutlandırma bir yeniden boyutlandırma sayar.)|
|targetDedicated|Int32|Havuz için istenen işlem düğümleri sayısı.|
|enableAutoScale|bool|Havuz boyutunu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|enableInterNodeCommunication|bool|Havuzun düğümler arası doğrudan iletişime için ayarlanan olup olmadığını belirtir.|
|isAutoPool|bool|Havuza bir işin AutoPool mekanizması oluşturulup oluşturulmadığını belirtir.|
|maxTasksPerNode|Int32|Bir havuzun tek bir işlem düğümünde eşzamanlı olarak çalışan görevler maksimum sayısı.|
|vmFillType|Dize|Batch hizmetinin görevleri havuzdaki işlem düğümleri arasında nasıl dağıtacağını tanımlar. Geçerli değerler yayılır veya paketi.|

###  <a name="bk_csconf"></a> cloudServiceConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|osFamily|Dize|Azure konuk işletim sistemi ailesi'havuzundaki sanal makinelere yüklenecek.<br /><br /> Olası değerler şunlardır:<br /><br /> **2** – işletim sistemi ailesi 2, Windows Server 2008 R2 SP1'e eşdeğer.<br /><br /> **3** – işletim sistemi aile 3, Windows Server 2012'ye denk.<br /><br /> **4** – işletim sistemi ailesi 4, Windows Server 2012 R2'ye denk.<br /><br /> Daha fazla bilgi için [Azure konuk işletim sistemi sürümleri](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Dize|Havuzundaki sanal makinelere yüklenecek Azure konuk işletim sistemi sürümü.<br /><br /> Varsayılan değer **\*** belirtilen ürün ailesi için en son işletim sistemi sürümünü belirtir.<br /><br /> Diğer izin verilen değerler için bkz. [Azure konuk işletim sistemi sürümleri](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a> virtualMachineConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Karmaşık Tür|Platform veya Market görüntüsü kullanmak hakkındaki bilgileri belirtir.|
|Nodeagentskuıd|Dize|İşlem düğümü üzerinde sağlanan Batch düğüm Aracısı SKU.|
|[windowsConfiguration](#bk_winconf)|Karmaşık Tür|Sanal makine üzerinde Windows işletim sistemi ayarlarını belirtir. Bu özellik olmamalıdır Imagereference bir bir Linux işletim sistemi görüntüsü başvurulduğu belirtildi.|

###  <a name="bk_imgref"></a> Imagereference

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|Yayımcı|Dize|Görüntünün yayımcısı.|
|Teklif|Dize|Görüntünün teklifi.|
|sku|Dize|Görüntünün SKU'su.|
|version|Dize|Görüntü sürümü.|

###  <a name="bk_winconf"></a> windowsConfiguration

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|enableAutomaticUpdates|Boole|Sanal makine için Otomatik Güncelleştirmeler etkin olup olmadığını gösterir. Bu özellik belirtilmezse, varsayılan değer True'dur.|

###  <a name="bk_netconf"></a> networkConfiguration

|Öğe adı|Tür|Notlar|
|------------------|--------------|----------|
|subnetId|Dize|Havuzun işlem düğümleri oluşturulduğu alt kaynak tanımlayıcısını belirtir.|
