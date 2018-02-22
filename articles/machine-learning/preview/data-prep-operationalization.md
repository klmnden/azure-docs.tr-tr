---
title: "Azure Machine Learning veriler hazırlıkları Operationalization kullanma hakkında ayrıntılı kılavuz | Microsoft Docs"
description: "Bu belgede daha önce yürütülen Ayrıntılar veri kaynakları ve veriler hazırlıkları paketleri tasarlanan sağlar"
services: machine-learning
author: hughz
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/13/2018
ms.openlocfilehash: 76bf850df5c8f4a93268dd06f7b9254bbd2cfce6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="data-preparation-operationalization"></a>Veri hazırlama Operationalization 

## <a name="operationalization-scenario"></a>Operationalization senaryosu

Bir kullanıcı gelebilir arasında farklı veri hazırlık Operationalization senaryolar aşağıda verilmiştir.

### <a name="develop-your-model-based-on-training-data"></a>Eğitim verileri esas modelinizin geliştirilmesine

Gerçek zamanlı bir Puanlama API hizmeti oluşturup aradığınızı varsayalım. Bazı eğitim veri kümesine göre Puanlama için bir modeli geliştirmek için ilk adımdır bakın. Veri hazırlığı eğitim verilerinizin bir örnek yeni bir veri kaynağı içeri aktarır. Veri hazır sonra DataPrep paket hazırlık adımlarını içerir. Bir AzureML deneme hesabı kullanarak, kullanıcı eğitim verileri her giriş için etiketleri oluşturmak için bir makine öğrenimi modeline yineleyebilirsiniz.

Güncelleştirilecek veri gereken özellikleri olarak kullanıcı verilerini yeni bir şekilde hazırlayın ve bu adımları kaydetmek için DataPrep paketi döndürür. Model doğru bir şekilde test verilerini puanlar kadar yeni özellikler oluşturmak ve bunların makine öğrenimi modeline uyguladıkça bu yineleme işlemi devam eder. 

### <a name="deploy-your-model-to-the-azure-model-management-service"></a>Modelinizi Azure Model yönetim hizmetine dağıtma

Şimdi gerçek zamanlı olarak puan istediğiniz müşteri veri içeriyor. Azure Model Yönetim hizmetini kullanarak, bu model AzureML Python clı'dan bir hizmet olarak dağıtabilirsiniz. Dağıtılan hizmet gerçek zamanlı olarak müşteri verilerini almak için bir REST uç noktasını kullanıma sunar ve karşılık gelen döndürme eğitilen model etiketlerden çıktı.

Kullanım kolaylığı için JSON biçiminde veri modeli uç nokta kabul eder. Giriş 2 boyutlu bir dizi ReadJSONLiteral dönüştürme geçirilen ve DataPrep veri kaynağına dönüştürülemeyen veri temsil eden JSON dize olmalıdır. Veri hazırlık paketi deneme aşaması can sırasında oluşturulan sonra karşı akış JSON verilerini yürütülebilir. Sonuçta elde edilen dataframe, eğitilen modeli Puanlama için sonra geçirilebilir.

## <a name="read-json-literal-transformation"></a>JSON değişmez değer dönüştürme okuma

### <a name="description"></a>Açıklama

Veri hazırlığı operationalization amacıyla içeren bir **ReadJsonLiteral** bir etkinliği yürütmek ve bir Pandas oluşturmak için dönüştürme veya Spark Dataframe. Bu dönüşüm benzersiz şekilde alır giriş olarak varolan bir veri prep paketi ve JSON veri kaynağı. Bu dönüştürme DataPrep Python CLI aracılığıyla sunulur.

### <a name="instructions"></a>Yönergeler

#### <a name="pythoncli"></a>PythonCLI

Azure Machine Learning çalışma ekranından komut satırı arabirimi açın (Dosya > komut istemi açın).

Bu örnekte, TrainingPreparationSteps veri hazırlık paketi verileri hazırlamak ve her giriş için Toplu özellik eklemek için kullanılır.

```
>>> import azureml.dataprep.package as azdp

>>> azdp.run_on_data.__doc__
"Generate a dataframe based on a selected dataflow in a package using in-memory data sources.
'user_config' is expected as a dictionary that maps the absolute path of a dsource file to an in-memory data source represented as a list of lists."

>>> real_time_customer_data = [[1.0, 1.5, 2.5], [2.5, 1.5, 3]]
>>> azdp.run_on_data({ "C:\\TrainingSampleData.dsource": real_time_customer_data}, "C:\\TrainingPreparationSteps.dprep")
    Height   Width   Depth  Volumne
0   1.0       1.5    2.5    3.75
1   2.5       1.5    3.0    11.25
```

`run_on_data()` Aşağıdaki isteğe bağlı parametreleri içerir
 - `dataflow_idx`: pakette yürütmek için istenen veri akışı dizinini belirtin
 - `secrets`: gizli deposu sözlük (anahtar: secretId, değer: depolanan gizli)
 - `spark`: spark yürütme için spark oturumu sağlayın

#### <a name="input"></a>Girdi

2 boyutlu bir dizi giriş ("dizi dizisi"). Python içinde giriş listelerinin bir listesi olmalıdır. JSON türü yerel tarih olmadığından, herhangi bir Python datetime.datetime nesne ISO biçimlendirilen tarih dizeleri dönüştürülecek. REST uç noktaları için giriş 2-D JSON dizisini temsil eden JSON sabit değerli bir dize olmalıdır.

#### <a name="output"></a>Çıktı

Bir Pandas veya Spark DataFrame. DataFrame türünü çalıştırma yapılandırmasında seçili framework tarafından belirlenir (`.runconfig`). Sonuçta elde edilen dataframe Puanlama eğitilen modeli giriş olarak geçirilebilir.
