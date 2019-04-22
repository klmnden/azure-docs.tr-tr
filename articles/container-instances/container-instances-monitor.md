---
title: Azure Container Instances’taki kapsayıcıları izleme
description: Azure Container Instances'taki kapsayıcılarınızın CPU ve bellek gibi bilgi işlem kaynakları kullanımını izleme işleminin ayrıntıları.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 04/24/2018
ms.author: danlep
ms.openlocfilehash: 950d8b4b5ec1a55e2054039a01d6807915b5c714
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784082"
---
# <a name="monitor-container-resources-in-azure-container-instances"></a>Azure Container Instances’taki kapsayıcı kaynaklarını izleme

Azure İzleyici, kapsayıcı örnekleriniz tarafından kullanılan bilgi işlem kaynaklarıyla ilgili içgörü sağlar. Kapsayıcı gruplarının ve bunların kapsayıcılarının CPU ve bellek kullanımını izlemek için Azure İzleyici'den yararlanın. Bu kaynak kullanımı verileri, kapsayıcı gruplarınız için en iyi CPU ve bellek ayarlarını saptamanıza yardımcı olur.

Bu belgede, hem Azure portalını hem de Azure CLI'yi kullanarak kapsayıcı örneklerinin CPU ve bellek kullanım bilgilerini toplama işleminin ayrıntıları açıklanır.

> [!IMPORTANT]
> Şu anda, yalnızca Linux kapsayıcıları için kaynak kullanım ölçümleri sağlanmaktadır.
>

## <a name="available-metrics"></a>Mevcut ölçümler

Azure İzleyici, Azure Container Instances için hem **CPU** hem de **bellek** kullanım ölçümleri sağlar. Kapsayıcı grubu ve tek tek kapsayıcılar için her iki ölçüm de sağlanır.

CPU ölçümleri **millicore** cinsinden gösterilir. Bir millicore, CPU çekirdeğinin 1/1000'idir, dolayısıyla 500 millicore (veya 500 m) CPU çekirdeğinin %50 kullanımına işaret eder.

Bellek ölçümleri **bayt** cinsinden gösterilir.

## <a name="get-metrics---azure-portal"></a>Ölçümleri alma - Azure portal

Kapsayıcı grubu oluşturulduğunda, Azure portalda Azure İzleyici verileri sağlanır. Kapsayıcı grubunun ölçümlerini görmek için, kaynak grubunu ve ardından kapsayıcı grubunu seçin. Burada, hem CPU hem de bellek kullanımına ilişkin olarak önceden oluşturulmuş grafikleri görebilirsiniz.

![çift grafik][dual-chart]

Birden çok kapsayıcı içeren bir kapsayıcı grubunuz varsa, tek tek her kapsayıcının ölçümlerinin gösterilmesi için [boyut][monitor-dimension] kullanın. Tek bir kapsayıcının ölçümlerinin yer aldığı bir grafik oluşturmak için aşağıdaki adımları izleyin:

1. Sol taraftaki gezinti menüsünde **İzleyici**'yi seçin.
2. Bir kapsayıcı grubu ve ölçüm (CPU veya Bellek) seçin.
3. Yeşil boyut düğmesini seçin ve sonra da **Kapsayıcı Adı**'nı seçin.

![boyut][dimension]

## <a name="get-metrics---azure-cli"></a>Ölçümleri alma - Azure CLI

Kapsayıcı örnekleri CPU ve bellek kullanımı, Azure CLI kullanılarak da toplanabilir. Önce, aşağıdaki komutu kullanarak kapsayıcı grubunun kimliğini alın. `<resource-group>` değerini kaynak grubunuzun adıyla ve `<container-group>` değerini kapsayıcı grubunuzun adıyla değiştirin.


```console
CONTAINER_GROUP=$(az container show --resource-group <resource-group> --name <container-group> --query id --output tsv)
```

Aşağıdaki komutu kullanarak **CPU** kullanım ölçümlerini alın.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:39:00  CPU Usage
2018-04-22 04:40:00  CPU Usage
2018-04-22 04:41:00  CPU Usage
2018-04-22 04:42:00  CPU Usage
2018-04-22 04:43:00  CPU Usage      0.375
2018-04-22 04:44:00  CPU Usage      0.875
2018-04-22 04:45:00  CPU Usage      1
2018-04-22 04:46:00  CPU Usage      3.625
2018-04-22 04:47:00  CPU Usage      1.5
2018-04-22 04:48:00  CPU Usage      2.75
2018-04-22 04:49:00  CPU Usage      1.625
2018-04-22 04:50:00  CPU Usage      0.625
2018-04-22 04:51:00  CPU Usage      0.5
2018-04-22 04:52:00  CPU Usage      0.5
2018-04-22 04:53:00  CPU Usage      0.5
```

**Bellek** kullanım ölçümlerini almak için de aşağıdaki komutu kullanın.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric MemoryUsage --output table

Timestamp            Name              Average
-------------------  ------------  -----------
2018-04-22 04:38:00  Memory Usage
2018-04-22 04:39:00  Memory Usage
2018-04-22 04:40:00  Memory Usage
2018-04-22 04:41:00  Memory Usage
2018-04-22 04:42:00  Memory Usage  6.76915e+06
2018-04-22 04:43:00  Memory Usage  9.22061e+06
2018-04-22 04:44:00  Memory Usage  9.83552e+06
2018-04-22 04:45:00  Memory Usage  8.42906e+06
2018-04-22 04:46:00  Memory Usage  8.39526e+06
2018-04-22 04:47:00  Memory Usage  8.88013e+06
2018-04-22 04:48:00  Memory Usage  8.89293e+06
2018-04-22 04:49:00  Memory Usage  9.2073e+06
2018-04-22 04:50:00  Memory Usage  9.36243e+06
2018-04-22 04:51:00  Memory Usage  9.30509e+06
2018-04-22 04:52:00  Memory Usage  9.2416e+06
2018-04-22 04:53:00  Memory Usage  9.1008e+06
```

Birden çok kapsayıcılı bir grup söz konusu olduğunda, kapsayıcı başına bu verileri döndürmek için `containerName` boyutu eklenebilir.

```console
$ az monitor metrics list --resource $CONTAINER_GROUP --metric CPUUsage --dimension containerName --output table

Timestamp            Name          Containername             Average
-------------------  ------------  --------------------  -----------
2018-04-22 17:03:00  Memory Usage  aci-tutorial-app      1.95338e+07
2018-04-22 17:04:00  Memory Usage  aci-tutorial-app      1.93096e+07
2018-04-22 17:05:00  Memory Usage  aci-tutorial-app      1.91488e+07
2018-04-22 17:06:00  Memory Usage  aci-tutorial-app      1.94335e+07
2018-04-22 17:07:00  Memory Usage  aci-tutorial-app      1.97714e+07
2018-04-22 17:08:00  Memory Usage  aci-tutorial-app      1.96178e+07
2018-04-22 17:09:00  Memory Usage  aci-tutorial-app      1.93434e+07
2018-04-22 17:10:00  Memory Usage  aci-tutorial-app      1.92614e+07
2018-04-22 17:11:00  Memory Usage  aci-tutorial-app      1.90659e+07
2018-04-22 16:12:00  Memory Usage  aci-tutorial-sidecar  1.35373e+06
2018-04-22 16:13:00  Memory Usage  aci-tutorial-sidecar  1.28614e+06
2018-04-22 16:14:00  Memory Usage  aci-tutorial-sidecar  1.31379e+06
2018-04-22 16:15:00  Memory Usage  aci-tutorial-sidecar  1.29536e+06
2018-04-22 16:16:00  Memory Usage  aci-tutorial-sidecar  1.38138e+06
2018-04-22 16:17:00  Memory Usage  aci-tutorial-sidecar  1.41312e+06
2018-04-22 16:18:00  Memory Usage  aci-tutorial-sidecar  1.49914e+06
2018-04-22 16:19:00  Memory Usage  aci-tutorial-sidecar  1.43565e+06
2018-04-22 16:20:00  Memory Usage  aci-tutorial-sidecar  1.408e+06
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure İzlemesi'ne genel bakış][azure-monitoring] bölümünde Azure İzlemesi hakkında daha fazla bilgi edinebilirsiniz.

<!-- IMAGES -->
[cpu-chart]: ./media/container-instances-monitor/cpu-multi.png
[dimension]: ./media/container-instances-monitor/dimension.png
[dual-chart]: ./media/container-instances-monitor/metrics.png
[memory-chart]: ./media/container-instances-monitor/memory-multi.png

<!-- LINKS - Internal -->
[azure-monitoring]: ../monitoring-and-diagnostics/monitoring-overview.md
[monitor-dimension]: ../azure-monitor/platform/data-platform-metrics.md#multi-dimensional-metrics
