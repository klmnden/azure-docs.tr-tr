---
title: Azure Machine Learning modeli veri toplama API Başvurusu | Microsoft Docs
description: Azure Machine Learning modeli veri toplama API'si başvurusu.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: db8718b60cdd934e3301479489f21fe45f5578cd
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Azure Machine Learning modeli veri toplama API'si başvurusu

Model veri koleksiyonu, model girişleri ve web hizmeti öğrenme bir makineden tahminleri arşivlemek olanak sağlar. Bkz: [model veri toplama nasıl yapılır Kılavuzu](how-to-use-model-data-collection.md) nasıl yükleneceği anlamak için `azureml.datacollector` , Windows ve Linux makinenizde.

Bu API başvuru kılavuzu modeli girişleri ve tahminleri web hizmeti öğrenme bir makineden toplamak konusunda adım adım bir yaklaşım kullanın.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Azure ML çalışma ekranı ortamında model veri toplamayı etkinleştir

 Conda için Ara\_dependencies.yml dosya aml_config klasörü altında projenizdeki ve, conda sahip\_dependencies.yml dosya PIP bölümünde azureml.datacollector modülü aşağıda gösterildiği gibi ekleyin. Bu yalnızca bir tam conda alt olduğuna dikkat edin\_dependencies.yml dosyası:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Şu anda, docker modunda çalıştırarak Azure ML çalışma ekranı veri toplayıcı modülünü kullanabilirsiniz. Yerel mod tüm ortamlar için çalışmayabilir.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Puanlama dosyasında model veri toplamayı etkinleştir

Operationalization için kullanılan Puanlama dosyasında ModelDataCollector sınıfı ve veri toplayıcı modülü içeri aktarın:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Model Veri Toplayıcı örnek oluşturma
Bir ModelDataCollector yeni bir örneğini örneği:

    dc = ModelDataCollector(model_name, identifier='default', feature_names=None, model_management_account_id='unknown', webservice_name='unknown', model_id='unknown', model_version='unknown')

Sınıf ve parametre ayrıntıları bakın:

### <a name="class"></a>Sınıf
| Ad | Açıklama |
|--------------------|--------------------|
| ModelDataCollector | Bir sınıf azureml.datacollector ad. Bu sınıf örneği, model verileri toplamak için kullanılır. Tek bir Puanlama dosyası birden çok ModelDataCollectors içerebilir. Her bir örnek toplanan verileri şeması tutarlı kalmayacak şekilde Puanlama dosyasındaki bir ayrık konumdaki verilerinin toplanması için kullanılması gereken (diğer bir deyişle, girişleri ve tahmin)|


### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
|-------------|------------|-------------------------|
| model_adı | string | hangi verilerin toplanan modelinin adı |
| tanımlayıcı | string | Bu veriler, yani tanımlayan kod konumu 'RawInput' veya 'Tahmin' |
| feature_names | dize listesi | sağlandığında csv başlığı hale özellik adlarının listesi |
| model_management_account_id | string | Bu model depolandığı model yönetim hesabı tanımlayıcısı. modelleri AML kullanıma hazır hale getirilmiş, bu otomatik olarak doldurulur |
| webservice_name | string | Bu model şu anda dağıtıldığı webservice adı. modelleri AML kullanıma hazır hale getirilmiş, bu otomatik olarak doldurulur |
| model_id | string | Bir model yönetim hesabı bağlamında bu model için benzersiz tanımlayıcı. modelleri AML kullanıma hazır hale getirilmiş, bu otomatik olarak doldurulur |
| model_version | string | Bu modelin model yönetim hesabı bağlamında sürüm numarası. modelleri AML kullanıma hazır hale getirilmiş, bu otomatik olarak doldurulur |



 

## <a name="collecting-the-model-data"></a>Model verileri toplama

Yukarıda oluşturduğunuz ModelDataCollector örneği kullanarak model verileri toplayabilir.

    dc.collect(input_data, user_correlation_id="")

Yöntem ve parametre ayrıntıları bakın:

### <a name="method"></a>Yöntem
| Ad | Açıklama |
|--------------------|--------------------|
| TOPLA | Bir model giriş veya tahmin verilerini toplamak için kullanılan|


### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
|-------------|------------|-------------------------|
| input_data | birden çok tür | Toplanacak veri (şu anda kabul türleri listesi, numpy.array pandas. DataFrame, pyspark.sql.DataFrame). Üstbilgi özellik adları ile varsa dataframe türleri için bu bilgiler veri hedef (açıkça özellik adları ModelDataCollector oluşturucuda geçmesi gerek kalmadan) dahil edilir |
| user_correlation_id | string | Bu tahmin ilişkilendirmek için kullanıcı tarafından sağlanan bir isteğe bağlı bağıntı kimliği |

