---
title: Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırın.
description: Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 896cae9b7fc43765e340ba3b92351e04b5512efd
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762560"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Azure Machine Learning iş yükleri, Azure HDInsight, Apache Spark üzerinde otomatik makine öğrenimi (AutoML) çalıştırın.

Azure Machine Learning derleme, test etme ve verilerinizde Tahmine dayalı analiz çözümleri dağıtmak için kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır. Azure Machine Learning modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web Hizmetleri olarak yayımlar. Otomatik machine learning (AutoML), yüksek kaliteli makine öğrenimi modellerini akıllı otomasyonla ve iyileştirme'yi kullanarak oluşturma yardımcı olur. AutoML doğru algoritması ve hiper parametreler belirli sorun türleri için kullanılacak karar verir.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Azure Machine Learning bir HDInsight kümesine yükleme

> [!Note]
> Azure ML çalışma alanı aşağıdaki bölgelerde kullanılabilir: eastus2 ve westcentralus eastus. HDInsight kümesi, ayrıca bu bölgelerden birinde oluşturulmalıdır.

Azure Machine Learning ve otomatik makine öğrenimi genel öğreticiler için bkz. [Öğreticisi: Azure Machine Learning Studio'da ilk veri bilimi denemenizi oluşturma](../../machine-learning/studio/create-experiment.md) ve [Öğreticisi: Otomatik makine öğrenimi, regresyon modeli derler](../../machine-learning/service/tutorial-auto-train-models.md).
Betik eylemi - çalıştırın, Azure HDInsight kümesinde AzureML yüklenecek [install_aml](https://commonartifacts.blob.core.windows.net/automl/install_aml.sh) - baş düğümü ve bir HDInsight 3.6 Spark 2.3.0 kümesinin (önerilen) çalışan düğümü üzerinde. Bu betik eylemi Azure portalı üzerinden ya da var olan bir kümede veya küme oluşturma işlemi, bir parçası olarak çalıştırılabilir.

Betik eylemleri hakkında daha fazla bilgi için okuma [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemlerini kullanarak](../hdinsight-hadoop-customize-cluster-linux.md). Azure Machine Learning paketleri ve bağımlılıklarını yükleme yanı sıra komut dosyası ayrıca örnek bir Jupyter not defteri indirir (yoluna `HdiNotebooks/PySpark` varsayılan depolama). Bu Jupyter not defteri sınıflandırıcı basit sınıflandırma sorunu için öğrenme otomatik bir makine nasıl kullanacağınızı gösterir.

> [!Note]
> Azure Machine Learning paketleri Python3 conda ortamına yüklenir. PySpark3 çekirdek kullanarak yüklü Jupyter not defteri çalıştırmanız gerekir.

## <a name="authentication-for-workspace"></a>Çalışma alanı için kimlik doğrulaması

Çalışma alanı oluşturma ve deneme gönderme bir kimlik doğrulama belirteci gerektirir. Bu belirteci kullanılarak oluşturulabilir. bir [Azure AD uygulaması](../../active-directory/develop/app-objects-and-service-principals.md). Bir [Azure AD kullanıcı](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) çok faktörlü kimlik doğrulaması hesapta etkin değil, ayrıca gerekli kimlik doğrulama belirteci oluşturmak için kullanılabilir.  

Aşağıdaki kod parçacığını kullanarak bir kimlik doğrulama belirteci oluşturur. bir **Azure AD uygulaması**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                username = '<Azure AD Application ID>',
                password = '<Azure AD Application Key>'
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

Otomatik machine learning yapılandırmasında, özellik `spark_context` dağıtılmış modu çalıştırmak bir paket için ayarlanması gerekir. Özellik `concurrent_iterations`, Spark uygulaması Yürütücü çekirdek daha düşük bir sayı olan en fazla paralel olarak yürütülen yineleme sayısını ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Otomatik makine öğrenimi arkasında motivasyon hakkında daha fazla bilgi için bkz. [machine learning Microsoft'un kullanarak adım yayın model otomatik!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/).
* [Microsoft Research AutoML projeden](https://www.microsoft.com/research/project/automl/)