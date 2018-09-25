---
title: Azure Machine Learning veri hazırlıkları yürütme API kullanma hakkında ayrıntılı kılavuz | Microsoft Docs
description: Bu belge, daha önce yürütülen ayrıntı veri kaynakları ve veri hazırlıkları paketleri tasarlanmış sağlar.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 5280ed4143ef2ee980300cebe234de7f2020a14f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968110"
---
# <a name="execute-data-sources-and-data-preparations-packages-from-python"></a>Python'dan veri kaynakları ve veri hazırlıkları paketleri yürütün

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Bir Azure Machine Learning veri kaynakları veya Azure Machine Learning veri hazırlıkları paket Python içinde kullanmak için:

1. Git **veri** projenizin sekmesi.

2. Uygun kaynak sağ tıklayın.

3. Seçin **veri erişim kodu dosyası oluştur.**

Bu eylem, paketin yürütür ve bir dataframe döndüren kısa bir Python betiğini oluşturur.

## <a name="data-sources"></a>Veri Kaynakları

`azureml.dataprep.datasource` Modülü bir veri kaynağı yürütmek ve bir dataframe döndürmek için tek bir işlevi içerir: `load_datasource(path, secrets=None, spark=None)`.
- `path` veri kaynağı (.dsource dosyası) yoludur.
- `secrets` için gizli anahtarlarını eşleyen bir isteğe bağlı bir sözlük var.
- `spark` Spark dataframe veya Pandas dataframe döndürülüp döndürülmeyeceğini belirleyen bir isteğe bağlı bool olur. Varsayılan olarak, Azure Machine Learning Workbench, bağlama göre çalışma zamanında döndürülecek dataframe türünü belirler.

## <a name="data-preparations-packages"></a>Veri hazırlıkları paketleri

`azureml.dataprep.package` Modülü bir veri hazırlıkları paketinden bir veri akışını yürütecek üç işlev içerir.

### <a name="execution-functions"></a>Yürütme işlevleri

- `submit(package_path, dataflow_idx=0, secrets=None, spark=None)` yürütme için belirtilen veri akışı gönderir, ancak bir dataframe döndürmez.
- `run(package_path, dataflow_idx=0, secrets=None, spark=None)` Belirtilen veri akışını çalıştırır ve sonuçları bir veri çerçevesi döndürür.
- `run_on_data(user_config, package_path, dataflow_idx=0, secrets=None, spark=None)` bir bellek içi veri kaynağını temel alan belirtilen veri akışını çalıştırır ve sonuçları bir veri çerçevesi döndürür. `user_config` Bağımsız değişkeni bir liste temsil edilen bir bellek içi veri kaynağı mutlak yol (.dsource dosyası) veri kaynağının eşleyen sözlük etmektir.

### <a name="common-arguments"></a>Genel bağımsız değişkenleri

- `package_path` Veri hazırlıkları paket (.dprep dosyası) yoludur.
- `dataflow_idx` hangi veri akışının yürütmek amacıyla pakete sıfır tabanlı dizindir. Belirtilen veri akışı, başka bir veri akışı veya veri kaynakları başvuruyorsa, bunlar da yürütülür.
- `secrets` için gizli anahtarlarını eşleyen bir isteğe bağlı bir sözlük var.
- `spark` Spark dataframe veya Pandas dataframe döndürülüp döndürülmeyeceğini belirleyen bir isteğe bağlı bool olur. Varsayılan olarak, Workbench, bağlama göre çalışma zamanında döndürülecek dataframe türünü belirler.
