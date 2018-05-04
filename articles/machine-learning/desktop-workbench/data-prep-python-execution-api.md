---
title: Azure Machine Learning veriler hazırlıkları yürütme API kullanımı hakkında ayrıntılı kılavuz | Microsoft Docs
description: Bu belgede daha önce yürütülen Ayrıntılar veri kaynakları ve veriler hazırlıkları paketleri tasarlanan sağlar
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 36814d238aabd12e7cc6947809c135130002eb46
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Veri kaynakları ve veriler hazırlıkları paketleri Python yürütme

Bir Azure Machine Learning veri kaynakları veya Azure Machine Learning veriler hazırlıkları paket Python içinde kullanmak için:

1. Git **veri** projenizin sekmesi.

2. Uygun kaynak sağ tıklayın.

3. Seçin **veri erişim kodu dosyası oluşturun.**

Bu eylem, paketin yürütür ve bir dataframe döndüren kısa bir Python betiği oluşturur.

## <a name="data-sources"></a>Veri Kaynakları

`azureml.dataprep.datasource` Modülü bir veri kaynağı yürütmek ve bir dataframe dönmek için tek bir işlevi içeriyor: `load_datasource(path, secrets=None, spark=None)`.
- `path` veri kaynağı (.dsource dosyası) yoludur.
- `secrets` için gizli anahtarlarını eşleyen isteğe bağlı bir sözlük olur.
- `spark` Spark dataframe veya Pandas dataframe döndürülmeyeceğini belirten isteğe bağlı bir bool olur. Varsayılan olarak, Azure Machine Learning çalışma ekranı bağlamına dayalı çalışma zamanında döndürmek için ne tür bir dataframe belirler.

## <a name="data-preparations-packages"></a>Veriler hazırlıkları paketleri

`azureml.dataprep.package` Modülü veriler hazırlıkları paketinden bir veri akışını yürütecek üç işlevler içerir.

### <a name="execution-functions"></a>Yürütme işlevleri

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)` yürütme için belirtilen veri akışı gönderir, ancak bir dataframe döndürmüyor.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)` Belirtilen veri akışı çalıştırır ve sonuçları bir dataframe döndürür.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)` bir bellek içi veri kaynağına göre belirtilen veri akışı çalıştırır ve sonuçları bir dataframe döndürür. `user_config` Bağımsız değişken listeleri oluşan bir liste olarak temsil edilen bir bellek içi veri kaynağı bir veri kaynağı (.dsource dosyası) mutlak yolu eşleyen bir sözlük etmektir.

### <a name="common-arguments"></a>Ortak bağımsız değişkenler

- `package_path` Veriler hazırlıkları paketi (.dprep dosyası) yoludur.
- `dataflow_idx` hangi veri akışını yürütmek için paketindeki sıfır tabanlı dizini var. Belirtilen veri akışı veri kaynakları veya diğer veri akışları başvurursa, bunlar da yürütülür.
- `secrets` için gizli anahtarlarını eşleyen isteğe bağlı bir sözlük olur.
- `spark` Spark dataframe veya Pandas dataframe döndürülmeyeceğini belirten isteğe bağlı bir bool olur. Varsayılan olarak, çalışma ekranı bağlamına dayalı çalışma zamanında döndürmek için ne tür bir dataframe belirler.
