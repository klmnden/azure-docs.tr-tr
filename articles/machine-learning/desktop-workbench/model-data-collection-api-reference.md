---
title: Azure Machine Learning Model veri koleksiyonu API Başvurusu | Microsoft Docs
description: Azure Machine Learning Model veri koleksiyonu API Başvurusu.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ROBOTS: NOINDEX
ms.openlocfilehash: c047555c30481f34b607197f71483197bc64620c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961476"
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Azure Machine Learning Model veri koleksiyonu API Başvurusu

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Model veri koleksiyonu, model girişlerini ve tahminlerini bir machine learning web hizmeti'den arşiv olanak tanır. Bkz: [model veri koleksiyonu yapılır kılavuzunda](how-to-use-model-data-collection.md) nasıl yükleneceğini anlamak için `azureml.datacollector` Windows ve Linux makinenizde.

Bu API Başvurusu Kılavuzu'nda bir machine learning web hizmetini model girişlerini ve tahminlerini toplamak konusunda adım adım bir yaklaşım kullanın.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Azure ML Workbench ortamında model verisi toplamayı etkinleştir

 Aramak için conda\_dependencies.yml dosya aml_config klasörü altında projenizdeki ve, conda sahip\_dependencies.yml dosya aşağıda gösterildiği gibi pip bölümünde azureml.datacollector modülü içerir. Bu, yalnızca tam conda alt olduğunu unutmayın\_dependencies.yml dosyası:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>Şu anda docker modunda çalıştırarak Azure ML Workbench uygulamasında veri toplayıcı modülü kullanabilirsiniz. Yerel mod için tüm ortamlarını çalışmayabilir.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Puanlama dosyası, model verisi toplamayı etkinleştir

Kullanıma hazır hale getirme için kullanılan Puanlama dosyasında veri toplayıcı modülü ve ModelDataCollector sınıfını içeri aktarın:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Model Veri Toplayıcı örneklemesi
Bir yeni bir ModelDataCollector örneği:

    dc = ModelDataCollector(model_name, identifier='default', feature_names=None, model_management_account_id='unknown', webservice_name='unknown', model_id='unknown', model_version='unknown')

Sınıf ve parametre ayrıntılarını bakın:

### <a name="class"></a>Sınıf
| Ad | Açıklama |
|--------------------|--------------------|
| ModelDataCollector | Bir sınıf içinde azureml.datacollector ad alanı. Bu sınıfın bir örneği, model verileri toplamak için kullanılır. Tek bir Puanlama dosyası, birden çok ModelDataCollectors içerebilir. Her örnek, toplanan veri şeması tutarlı kalması Puanlama dosyası farklı bir konumda verilerinin toplanması için kullanılmalıdır (yani, giriş ve tahmin)|


### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
|-------------|------------|-------------------------|
| model_adı | dize | hangi veri toplanan model adı |
| tanımlayıcı | dize | Bu veriler, yani tanımlayan kod konumu 'RawInput' veya 'Tahmin' |
| feature_names | dize listesi | sağlandığında csv üst bilgisi haline özellik adları listesi |
| model_management_account_id | dize | Bu model depolandığı model Yönetimi hesabı için tanımlayıcı. Modelleri AML Çalıştır duruma getirdiniz, bu otomatik olarak doldurulur |
| webservice_name | dize | Bu model şu anda dağıtılmış olan Web hizmeti adı. Modelleri AML Çalıştır duruma getirdiniz, bu otomatik olarak doldurulur |
| model_id | dize | Model Yönetimi hesabı bağlamında bu model için benzersiz tanımlayıcı. modelleri AML Çalıştır duruma getirdiniz, bu otomatik olarak doldurulur |
| model_version | dize | Bu modelin model Yönetimi hesabı bağlamında sürüm numarası. Modelleri AML Çalıştır duruma getirdiniz, bu otomatik olarak doldurulur |



 

## <a name="collecting-the-model-data"></a>Model verileri toplama

Yukarıda oluşturulan ModelDataCollector örneği kullanarak model verileri toplayabilir.

    dc.collect(input_data, user_correlation_id="")

Yöntem ve parametre ayrıntıları bakın:

### <a name="method"></a>Yöntem
| Ad | Açıklama |
|--------------------|--------------------|
| TOPLA | Bir model giriş veya tahmin verilerini toplamak için kullanılan|


### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
|-------------|------------|-------------------------|
| input_data | birden çok tür | Toplanacak verileri (şu anda kabul türleri listesi, numpy.array pandas. Veri çerçevesi, pyspark.sql.DataFrame). Özellik adları ile bir üstbilgi varsa, veri türleri için bu bilgiler verilerin hedef (açıkça özellik adları ModelDataCollector oluşturucuda geçirmek zorunda kalmadan) dahil edilir |
| user_correlation_id | dize | Bu tahmin ilişkilendirmek için kullanıcı tarafından sağlanan bir isteğe bağlı bir bağıntı kimliği |

