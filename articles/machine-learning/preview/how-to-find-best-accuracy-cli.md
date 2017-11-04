---
title: "En iyi doğruluk ve Azure Machine Learning çalışma ekranındaki en düşük süre ile çalışır Bul | Microsoft Docs"
description: "Azure Machine Learning çalışma ekranı kullanarak en iyi doğruluğu CLI aracılığıyla bulmak için bir uçtan uca kullanım örneği"
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: aaadf526577b9b6c254204aae90200661d40f325
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>En iyi doğruluk ve en düşük süre Bul çalıştırır
Birden çok çalıştırır göz önüne alındığında, bir kullanım örneği çalıştırır ile en iyi doğruluğu bulmaktır. İle komut satırı arabirimi (CLI) kullanmak üzere bir yaklaşım ise bir [JMESPath](http://jmespath.org/) sorgu. Azure CLI JMESPath kullanma hakkında daha fazla bilgi için bkz: [Azure CLI 2.0 kullanmak JMESPath sorgularıyla](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). Aşağıdaki örnekte, dört çalıştırır 0, 0,98, 1 ve 1 doğruluğu değerlerle oluşturulur. Çalıştırır aralığında olmaları durumunda filtrelenir `[MaxAccuracy-Threshold, MaxAccuracy]` burada `Threshold = .03`.

## <a name="sample-data"></a>Örnek veriler
İle varolan çalışır yoksa bir `Accuracy` değeri, aşağıdaki adımları oluşturmak sorgulamak için çalışır.

İlk olarak, Azure Machine Learning çalışma ekranı bir Python dosyası oluşturun, adlandırın `log_accuracy.py`, aşağıdaki kodu yapıştırın:
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

Son olarak, çalışma ekranı aracılığıyla CLI açın ve şu komutu çalıştırın `python run.py` dört denemeler göndermek için. Komut dosyası tamamlandıktan sonra dört çalıştırmalar görmelisiniz `Run History` sekmesi.

## <a name="query-the-run-history"></a>Sorgu çalıştırma geçmişi
İlk komut max doğruluk değeri bulur.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Bu max doğruluk değeri kullanılarak `1` ve bir eşik değerini `0.03`, ikinci komutu filtreleri çalıştıran kullanarak `Accuracy` ve sıralar çalıştırır tarafından `duration` artan.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Kesin bir üst sınır onay istiyorsanız, sorgu biçimidir``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``

PowerShell'i kullanırsanız, aşağıdaki kod yerel değişkenler eşik ve max doğruluğu depolamak için kullanır:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Sonraki adımlar
Günlüğe kaydetme hakkında daha fazla bilgi için bkz: [çalıştırma geçmişi ve model ölçümleri Azure Machine Learning çalışma ekranı içinde nasıl kullanılacağını](how-to-use-run-history-model-metrics.md).    
