---
title: Python – Azure Machine Learning veri hazırlığı SDK'sı ile verileri hazırlama
description: Python için Azure Machine Learning veri hazırlığı SDK çeşitli biçimlerden veri yükleme, daha kullanışlı olması için dönüştürmek ve Modellerinizi erişmek için bir konum verileri yazmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 673b8ddf3ed07507cbce4b54e9b8a488cb3ac689
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166614"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Azure Machine Learning ile model için verileri hazırlama
 
Veri hazırlama, machine learning iş akışı önemli bir parçasıdır. Modellerinizi daha doğru ve verimli kullanmak daha kolay bir biçimde verileri temizlemek için erişime sahip oldukları olacaktır. 

Python kullanarak veri hazırlayabilir [Azure Machine Learning veri hazırlığı SDK'sı](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py). 

## <a name="data-preparation-pipeline"></a>Veri hazırlama işlem hattı

Ana veri hazırlama adımları şunlardır:

1. [Veri yükleme](how-to-load-data.md), çeşitli biçimlerde olabilir
2. [Dönüştürme](how-to-transform-data.md) içine daha kullanışlı bir yapısı
3. [Yazma](how-to-write-data.md) söz konusu veri modellerinize erişilebilir bir konuma

![Veri hazırlama işlemine](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme bir [örneğin Not Defteri](https://github.com/Microsoft/PendletonDocs/blob/master/Scenarios/GettingStarted/getting-started.ipynb) , Azure Machine Learning veri hazırlığı SDK'sını kullanarak veri hazırlama.
