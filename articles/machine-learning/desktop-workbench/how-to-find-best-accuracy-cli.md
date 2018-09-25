---
title: En yüksek doğruluğa ve Azure Machine Learning Workbench uygulamasında en düşün süreye sahip çalıştırmaları bulun | Microsoft Docs
description: Azure Machine Learning Workbench'i kullanarak en yüksek doğruluğa CLI aracılığıyla bulmak için bir uçtan uca kullanım örneği
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs, jmartens, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 094fd6d8c6c6d647533cf5409d1a85283c71c80e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953251"
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>En yüksek doğruluğa ve en düşün süreye çalıştırmaları bulma
Birden çok çalıştırma göz önünde bulundurulduğunda, bir kullanım durumu en yüksek doğruluk oranıyla çalıştırmaları bulma olmaktır. İle komut satırı arabirimi (CLI) kullanmak üzere bir yaklaşım ise bir [JMESPath](http://jmespath.org/) sorgu. JMESPath Azure CLI kullanma hakkında daha fazla bilgi için bkz. [Azure CLI ile kullanma JMESPath sorgularını](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). Aşağıdaki örnekte, dört çalıştırmaları 0, 0,98, 1 ve 1 doğruluğu değerlerle oluşturulur. Çalıştırmaları aralığında olmaları durumunda filtrelenir `[MaxAccuracy-Threshold, MaxAccuracy]` nerede `Threshold = .03`.

## <a name="sample-data"></a>Örnek veriler
Mevcut çalıştırmalarla yoksa bir `Accuracy` değer, aşağıdaki adımları oluşturmak, sorgulamak için çalışır.

İlk olarak, Azure Machine Learning Workbench uygulamasında Python dosyası oluşturun, adlandırın `log_accuracy.py`, aşağıdaki kodu yapıştırın:
```python
from azureml.logging import get_azureml_logger

logger = get_azureml_logger()

accuracy_value = 0.5

if len(sys.argv) > 1:
     accuracy_value = float(sys.argv[1])

logger.log("Accuracy", accuracy_value)
```

Ardından, bir dosya oluşturun `run.py`, aşağıdaki kodu yapıştırın:
```python
import os

accuracy_values = [0, 0.98, 1.0, 1.0]
for value in accuracy_values:
    os.system('az ml experiment submit -c local ./log_accuracy.py {}'.format(value))
```

Son olarak, Workbench aracılığıyla CLI'yı açın ve şu komutu çalıştırın `python run.py` dört denemeleri göndermek için. Betik bittikten sonra dört çalıştırmalar görmelisiniz `Run History` sekmesi.

## <a name="query-the-run-history"></a>Sorgu çalıştırma geçmişi
İlk komut, en yüksek doğruluk değeri bulur.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Bu en yüksek doğruluk değeri kullanılarak `1` ve bir eşik değerini `0.03`, ikinci komut filtreler çalıştırır kullanarak `Accuracy` ve ardından sıralar çalıştırır `duration` artan.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Kesin bir üst sınır denetim istiyorsanız sorgu biçimidir ``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``

PowerShell kullanıyorsanız, aşağıdaki kod eşiği ve en yüksek doğruluk depolamak için yerel değişkenleri kullanır:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Sonraki adımlar
Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [çalıştırma geçmişini ve model ölçümlerini Azure Machine Learning Workbench'te kullanmayı](how-to-use-run-history-model-metrics.md).    
