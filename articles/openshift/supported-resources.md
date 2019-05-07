---
title: Kaynakları Azure Red Hat OpenShift için desteklenen | Microsoft Docs
description: Microsoft Azure Red Hat OpenShift ile hangi Azure bölgeleri ve sanal makine boyutları desteklenen anlayın.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 22cc195b7808ad4a9111aafcf883a68c51f3709f
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076113"
---
# <a name="azure-red-hat-openshift-resources"></a>Azure Red Hat OpenShift kaynakları

Bu konu, Azure bölgeleri ve Microsoft Azure Red Hat OpenShift hizmeti tarafından desteklenen sanal makine boyutlarını listeler.

## <a name="azure-regions"></a>Azure bölgeleri

Aşağıdaki Azure bölgeleri dünya çapındaki Azure Red Hat OpenShift kümeleri dağıtabilirsiniz:

|Bölge|CLI kod|
|-|-|
|🇦🇺 Avustralya Doğu|`australiaeast`|
|🇨🇦 Kanada Orta|`canadacentral`|
|🇨🇦 Kanada Doğu|`canadaeast`|
|🇺🇸 Doğu ABD|`eastus`|
|🇺🇸 Batı ABD|`westus`|
|🇪🇺 Batı Avrupa|`westeurope`|
|🇪🇺 Kuzey Avrupa|`northeurope`|

## <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Azure Red Hat OpenShift kümenizde işlem düğümleri için belirtebileceğiniz desteklenen sanal makine boyutları aşağıda verilmiştir.

> [!Important]
> Her VM eklenebilecek sürücüleri farklı sayıda sahiptir. Bu bellek veya CPU boyutu olarak olarak hemen açık olmayabilir.
> Tüm VM boyutları, tüm bölgelerde kullanılabilir. API, sizin belirlediğiniz boyuta destekler olsa bile, boyutu, belirttiğiniz bölgede kullanılabilir değilse, bir hata alabilirsiniz.
> Bkz: [güncel listesi aşağıda desteklenen VM boyutları bölge başına](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) daha fazla bilgi için.

## <a name="compute-node-sizes"></a>İşlem düğümü boyutları

Aşağıdaki işlem düğümü boyutları Azure Red Hat OpenShift REST API'si tarafından desteklenir:

|Boyut|Sanal işlemci|RAM|
|-|-|-|
|Standart D4s v3|4|16 GB|
|Standart D8s v3|8|32 GB|
|Standart D16s v3|16|64 GB|
|Standart D32s v3|32|128 GB|
|-|-|-|
|Standart E4s v3|4|32 GB|
|Standart E8s v3|8|64 GB|
|Standart E16s v3|16|128 GB|
|Standart E32s v3|32|256 GB|
|-|-|-|
|Standart F8s v2|8|16 GB|
|Standart F16s v2|16|32 GB|
|Standart F32s v2|32|64 GB|

## <a name="master-node-sizes"></a>Ana düğümü boyutları

Aşağıdaki ana / alt yapı düğümü boyutları, Azure Red Hat OpenShift REST API'si tarafından desteklenir:

|Boyut|Sanal işlemci|RAM|
|-|-|-|
|Standart D4s v3|4|16 GB|
|Standart D8s v3|8|32 GB|
|Standart D16s v3|16|64 GB|
|Standart D32s v3|32|128 GB|

## <a name="next-steps"></a>Sonraki adımlar

Deneyin [Azure Red Hat OpenShift küme oluşturma](tutorial-create-cluster.md) öğretici.