---
title: 'Python modeli oluşturun: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Özel modelleme ve veri işleme modülünü oluşturmak için Azure Machine Learning hizmetinde bir Python modeli oluşturma modeli kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ea9b50cede3e2d264ca0476b6a987ca6896c3c79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029753"
---
# <a name="create-python-model"></a>Python Modeli Oluşturma

Bu makalede nasıl kullanılacağını **Python modeli oluşturma** modülü bir Python komut dosyasından deneyimsiz bir model oluşturmak için. 

Azure Machine Learning ortamında bir Python paket dahil tüm learner modeli temel alabilir. 

Modeli oluşturduktan sonra kullanabileceğiniz [modeli eğitme](train-model.md) gibi başka bir learner Azure Machine learning'de bir veri kümesinde modeli eğitmek için. Eğitilen modelin geçirilebilir [Score Model](score-model.md) tahminlerde bulunmak üzere modelini kullanmak için. Eğitilen modelin ardından kaydedilebilir ve iş akışı Puanlama web hizmeti olarak yayımlanabilir.

> [!WARNING]
> Şu anda Python modeline puanlanmış sonuçlarını geçirmek mümkün değildir [Evaluate Model](evaluate-model.md). Bir modeli değerlendirme gerekiyorsa, özel bir Python betiğini indirip çalıştırın kullanarak yazabilirsiniz [Python betiği yürütme](execute-python-script.md) modülü.  


## <a name="how-to-configure-create-python-model"></a>Python modeli oluşturma yapılandırma

Bu modül Python ara veya Uzman bilinmesini gerektirir. Modül, Azure Machine Learning'de yüklü Python paketlerini de dahil tüm learner kullanımını destekler. Önceden yüklenmiş Python paket listede [Python betiği yürütme](execute-python-script.md).
  

Bu makale, nasıl kullanılacağını gösterir **Python modeli oluşturma** ile basit bir deneme. Denemeyi grafiği aşağıda verilmiştir.

![oluşturma-python-model](./media/module/aml-create-python-model.png)

1.  Tıklayın **Python modeli oluşturma**, modelleme, uygulamak için betiği düzenleyin veya veri yönetimi işlemi. Azure Machine Learning ortamında bir Python paket dahil tüm learner modeli temel alabilir.


    Popüler kullanarak örnek bir kod iki sınıflı Naive Bayes sınıflandırıcı aşağıdadır *sklearn* paket.

```Python

# The script MUST define a class named AzureMLModel.
# This class MUST at least define the following three methods:
    # __init__: in which self.model must be assigned,
    # train: which trains self.model, the two input arguments must be pandas DataFrame,
    # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
# The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.


import pandas as pd
from sklearn.naive_bayes import GaussianNB


class AzureMLModel:
    def __init__(self):
        self.model = GaussianNB()
        self.feature_column_names = list()

    def train(self, df_train, df_label):
        self.feature_column_names = df_train.columns.tolist()
        self.model.fit(df_train, df_label)

    def predict(self, df):
        return pd.DataFrame(
            {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
             'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
        )


```


2. Bağlama **Python modeli oluşturma** yeni oluşturduğunuz için modülü bir **modeli eğitme** ve **Model Puanlama**

3. Modeli değerlendirme gerekiyorsa, ekleme bir [Python betiği yürütme](execute-python-script.md) ve değerlendirme uygulamak için Python betiğini düzenleyin.

Örnek değerlendirme kod aşağıda verilmiştir.

```Python


# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to 
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):
    
    from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
    import pandas as pd
    import numpy as np
    
    scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
    ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
    ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
    probabilities = scores["probabilities"]
    
    accuracy, precision, recall, auc = \
    accuracy_score(ytrue, ypred),\
    precision_score(ytrue, ypred),\
    recall_score(ytrue, ypred),\
    roc_auc_score(ytrue, probabilities)
    
    metrics = pd.DataFrame();
    metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
    metrics["Value"] = [accuracy, precision, recall, auc]
    
    return metrics,

```
