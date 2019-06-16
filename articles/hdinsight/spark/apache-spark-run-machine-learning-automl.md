---
title: Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırın.
description: Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 5135de0fc87af227073f96c653d928ace1a50fd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64917050"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırın.

Azure Machine Learning basitleştirir ve yapı, eğitim ve makine öğrenimi modelleri dağıtımını hızlandırır. Otomatik makine öğrenimi (AutoML), tanımlanan target özelliği olan eğitim verileriyle başlayacağız ve algoritmaları ve otomatik olarak en iyi modeli için eğitim puanları temel alarak verilerinizi seçmek için özellik seçimleri birleşimlerini yinelemek. HDInsight ile yüzlerce düğüm kümeleri sağlama olanağı sunar. Bir HDInsight kümesinde Spark üzerinde çalıştırılan AutoML eğitim işleri genişleme biçimde çalıştırın ve paralel olarak birden fazla eğitim işleri çalıştırmak için işlem kapasitesi bu düğümlere kullanmasına olanak tanır. Bu işlem, diğer büyük veri iş yüklerini ile paylaşırken AutoML denemeleri çalıştırmak kullanıcıların sağlar.
 

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Azure Machine Learning bir HDInsight kümesine yükleme

Genel otomatik machine learning öğreticileri için bkz. [Öğreticisi: Otomatik makine öğrenimi, regresyon modeli derler](../../machine-learning/service/tutorial-auto-train-models.md).
Tüm yeni HDInsight Spark kümelerinde AzureML AutoML SDK'sı ile önceden yüklenmiş olarak gelir. HDInsight üzerinde AutoML ile bu oluşturabileceğinize dair [örnek Jupyter not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-hdi). Bu Jupyter not defteri sınıflandırıcı basit sınıflandırma sorunu için öğrenme otomatik bir makine nasıl kullanacağınızı gösterir.

> [!Note]
> Azure Machine Learning paketleri Python3 conda ortamına yüklenir. PySpark3 çekirdek kullanarak yüklü Jupyter not defteri çalıştırmanız gerekir.

Alternatif olarak, Zeppelin not defterlerini AutoML de kullanmak için de kullanabilirsiniz.

> [!Note]
> Zeppelin sahip bir [bilinen sorun](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html) burada PySpark3 değil çekme doğru Python sürümünü. Lütfen belgelenmiş geçici çözümünü kullan.

## <a name="authentication-for-workspace"></a>Çalışma alanı için kimlik doğrulaması

Çalışma alanı oluşturma ve deneme gönderme bir kimlik doğrulama belirteci gerektirir. Bu belirteci kullanılarak oluşturulabilir. bir [Azure AD uygulaması](../../active-directory/develop/app-objects-and-service-principals.md). Bir [Azure AD kullanıcı](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) çok faktörlü kimlik doğrulaması hesapta etkin değil, ayrıca gerekli kimlik doğrulama belirteci oluşturmak için kullanılabilir.  

Aşağıdaki kod parçacığını kullanarak bir kimlik doğrulama belirteci oluşturur. bir **Azure AD uygulaması**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                service_principal_id = '<Azure AD Application ID>',
                service_principal_password = '<Azure AD Application Key>'
                )
```
Aşağıdaki kod parçacığını kullanarak bir kimlik doğrulama belirteci oluşturur. bir **Azure AD kullanıcı**.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com','my_smart_password')
```

## <a name="loading-dataset"></a>Veri kümesi yükleniyor

Machine learning Spark kullanan otomatik **veri akışlarını**, veriler üzerinde işlem gevşek Değerlendirilmiş, sabit olan.  Bir veri akışı, genel okuma erişimi olan bir blobu veya blob URL bir SAS belirteci ile bir veri kümesi yükleyebilirsiniz.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

Veri deposu, ayrıca tek seferlik bir kayıt kullanarak çalışma alanı ile kaydedebilirsiniz.

## <a name="experiment-submission"></a>Denemeyi gönderme

İçinde [otomatik machine learning yapılandırma](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig), özellik `spark_context` dağıtılmış modu çalıştırmak bir paket için ayarlanması gerekir. Özellik `concurrent_iterations`, Spark uygulaması Yürütücü çekirdek daha düşük bir sayı olan en fazla paralel olarak yürütülen yineleme sayısını ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğrenimi arkasında motivasyon hakkında daha fazla bilgi için bkz. [machine learning Microsoft'un kullanarak adım yayın model otomatik!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/)
* Azure ML otomatik ML özelliklerini kullanarak daha fazla ayrıntı için bkz: [yeni Azure Machine Learning hizmetindeki makine öğrenimi özelliklerinden otomatik](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/)
* [Microsoft Research AutoML projeden](https://www.microsoft.com/research/project/automl/)
