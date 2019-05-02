---
title: Azure Container Instances’taki kapsayıcıları izleme
description: CPU ve bellek içinde Azure Container Instances, kapsayıcılarınızın tarafından gibi işlem kaynaklarının kullanımını izlemeyi öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 04/24/2019
ms.author: danlep
ms.openlocfilehash: 7b46ea0518038eeb908591b8438acc2a9095242c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64570910"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Azure Container Instances’taki kapsayıcı kaynaklarını izleme

[Azure İzleyici] [ azure-monitoring] kapsayıcı örneklerinizin tarafından kullanılan işlem kaynakları hakkında Öngörüler sağlar. Bu kaynak kullanım verilerini, kapsayıcı grupları için en iyi kaynak ayarlarını belirlemenize yardımcı olur. Azure İzleyici ayrıca, kapsayıcı örneklerinizin ağ etkinliği izleyen ölçümleri sağlar.

Bu belge Azure portalı ve Azure CLI kullanarak kapsayıcı örnekleri için Azure İzleyici ölçümleri toplama ayrıntıları.

> [!IMPORTANT]
> Azure Container ınstances'da Azure İzleyici ölçümleri şu anda önizlemededir ve bazı [sınırlamalar uygulanır](#preview-limitations). Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Şu anda Azure İzleyici ölçümleri yalnızca Linux kapsayıcıları için kullanılabilir.

## <a name="available-metrics"></a>Mevcut ölçümler

Azure İzleyici, aşağıdakileri sağlar [Azure Container Instances için ölçümleri][supported-metrics]. Bu ölçümler, bir kapsayıcı grubu ve tek tek kapsayıcılar için kullanılabilir.

* **CPU kullanımı** - ölçülen **millicores**. Bir millicore 1'dir / 500 millicores (veya 500 milyon) % 50 CPU Çekirdek kullanımını temsil eder. Bu nedenle bir CPU çekirdeği 1000th. Toplu olarak **ortalama kullanım** tüm çekirdekler üzerinde.

* **Bellek kullanımı** - toplu olarak **ortalama bayt**.

* **Saniyede alınan bayt ağ** ve **ağ bayt gönderilen / saniye** - toplu olarak **ortalama bayt / saniye**. 

## <a name="get-metrics---azure-portal"></a>Ölçümleri alma - Azure portal

Kapsayıcı grubu oluşturulduğunda, Azure portalda Azure İzleyici verileri sağlanır. Kapsayıcı grubu için ölçümleri görmek için Git **genel bakış** kapsayıcı grubu için sayfa. Burada her kullanılabilir ölçümler için önceden oluşturulmuş grafiklerini görebilirsiniz.

![çift grafik][dual-chart]

Birden çok kapsayıcı içeren bir kapsayıcı grubundaki bir [boyut] [ monitor-dimension] kapsayıcı tarafından ölçümleri sunmak için. Tek bir kapsayıcının ölçümlerinin yer aldığı bir grafik oluşturmak için aşağıdaki adımları izleyin:

1. İçinde **genel bakış** sayfasında, aşağıdaki gibi ölçüm grafikleri birini seçin **CPU**. 
1. Seçin **uygulamak bölme** düğmesini ve **kapsayıcı adı**.

![boyut][dimension]

## <a name="get-metrics---azure-cli"></a>Ölçümleri alma - Azure CLI

Container Instances için ölçümler, Azure CLI kullanarak da toplanabilir. Önce, aşağıdaki komutu kullanarak kapsayıcı grubunun kimliğini alın. `<resource-group>` değerini kaynak grubunuzun adıyla ve `<container-group>` değerini kapsayıcı grubunuzun adıyla değiştirin.


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Aşağıdaki komutu kullanarak **CPU** kullanım ölçümlerini alın.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table

Timestamp            Name       Average
-------------------  ---------  ---------
2019-04-23 22:59:00  CPU Usage
2019-04-23 23:00:00  CPU Usage
2019-04-23 23:01:00  CPU Usage  0.0
2019-04-23 23:02:00  CPU Usage  0.0
2019-04-23 23:03:00  CPU Usage  0.5
2019-04-23 23:04:00  CPU Usage  0.5
2019-04-23 23:05:00  CPU Usage  0.5
2019-04-23 23:06:00  CPU Usage  1.0
2019-04-23 23:07:00  CPU Usage  0.5
2019-04-23 23:08:00  CPU Usage  0.5
2019-04-23 23:09:00  CPU Usage  1.0
2019-04-23 23:10:00  CPU Usage  0.5
```

Değiştirin `--metric` diğer almak için komut parametresi [ölçümleri desteklenen][supported-metrics]. Örneğin, almak için aşağıdaki komutu kullanın **bellek** kullanım ölçümleri. 

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table

Timestamp            Name          Average
-------------------  ------------  ----------
2019-04-23 22:59:00  Memory Usage
2019-04-23 23:00:00  Memory Usage
2019-04-23 23:01:00  Memory Usage  0.0
2019-04-23 23:02:00  Memory Usage  8859648.0
2019-04-23 23:03:00  Memory Usage  9181184.0
2019-04-23 23:04:00  Memory Usage  9580544.0
2019-04-23 23:05:00  Memory Usage  10280960.0
2019-04-23 23:06:00  Memory Usage  7815168.0
2019-04-23 23:07:00  Memory Usage  7739392.0
2019-04-23 23:08:00  Memory Usage  8212480.0
2019-04-23 23:09:00  Memory Usage  8159232.0
2019-04-23 23:10:00  Memory Usage  8093696.0
```

Çok kapsayıcılı bir grup için `containerName` boyut, kapsayıcı başına ölçümler döndürülecek eklenebilir.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --dimension containerName --output table

Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2019-04-23 22:59:00  Memory Usage  aci-tutorial-app
2019-04-23 23:00:00  Memory Usage  aci-tutorial-app
2019-04-23 23:01:00  Memory Usage  aci-tutorial-app      0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-app      16834560.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-app      17534976.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-app      18329600.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-app      19742720.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-app      14786560.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-app      14651392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-app      15470592.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-app      15450112.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-app      15339520.0
2019-04-23 22:59:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:00:00  Memory Usage  aci-tutorial-sidecar
2019-04-23 23:01:00  Memory Usage  aci-tutorial-sidecar  0.0
2019-04-23 23:02:00  Memory Usage  aci-tutorial-sidecar  884736.0
2019-04-23 23:03:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:04:00  Memory Usage  aci-tutorial-sidecar  831488.0
2019-04-23 23:05:00  Memory Usage  aci-tutorial-sidecar  819200.0
2019-04-23 23:06:00  Memory Usage  aci-tutorial-sidecar  843776.0
2019-04-23 23:07:00  Memory Usage  aci-tutorial-sidecar  827392.0
2019-04-23 23:08:00  Memory Usage  aci-tutorial-sidecar  954368.0
2019-04-23 23:09:00  Memory Usage  aci-tutorial-sidecar  868352.0
2019-04-23 23:10:00  Memory Usage  aci-tutorial-sidecar  847872.0
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure İzlemesi'ne genel bakış][azure-monitoring] bölümünde Azure İzlemesi hakkında daha fazla bilgi edinebilirsiniz.

Oluşturmayı [ölçüm uyarıları] [ metric-alert] Azure Container Instances için bir ölçüm eşiği aştığında, bildirim almak için.

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-monitoring]: ../azure-monitor/overview.md
[metric-alert]: ..//azure-monitor/platform/alerts-metric.md
[monitor-dimension]: ../azure-monitor/platform/data-platform-metrics.md#multi-dimensional-metrics
[supported-metrics]: ../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups
