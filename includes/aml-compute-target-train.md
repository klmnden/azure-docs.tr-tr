---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: 50905eb987defac612f1055b450b682726f0a56f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752965"
---
|Eğitim &nbsp;hedefleri| GPU desteği |[Otomatik ML](../articles/machine-learning/service/concept-automated-ml.md) | [ML işlem hatları](../articles/machine-learning/service/concept-ml-pipelines.md) | [Görsel arabirim](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Belki de | evet | &nbsp; | &nbsp; |
|[Azure Machine Learning işlem](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| evet | Evet & <br/>Hiper parametre&nbsp;ayarlama | evet | evet |
|[Uzak VM](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |evet | Evet & <br/>Hiper parametre ayarı | evet | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | evet | evet | &nbsp; |
|[Azure Data Lake Analytics'i](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | evet | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | evet | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | evet | &nbsp; |

**Tüm hedefler için birden fazla eğitim işleri yeniden kullanılabilir işlem**. Örneğin, uzak bir VM çalışma alanınıza eklediğiniz sonra birden çok iş için kullanabilirsiniz.