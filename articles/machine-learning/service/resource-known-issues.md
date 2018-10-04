---
title: Azure Machine Learning hizmeti için bilinen sorunlar ve sorun giderme
description: Geçici çözümler, bilinen sorunların bir listesini almak ve sorun giderme
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 02cee5a3e088c919ec94aee6f46ef6f428b9bb48
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249426"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti
 
Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur. 

## <a name="sdk-installation-issues"></a>SDK yükleme sorunları

**Hata iletisi: 'PyYAML' kaldırılamıyor** 

PyYAML bir distutils yüklü projesidir. Bu nedenle, biz durumunda kısmi bir kaldırma için hangi dosyaların ait doğru bir şekilde belirlenemiyor. Bu hatayı yoksayma sırasında SDK'sı yüklemeye devam etmek için kullanın:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmeti dağıtılırken hata oluşturma görüntüsü. Geçici çözüm olan eklemek için "pynacl 1.2.1 ==" Conda dosyasına görüntü yapılandırması için pip bağımlılık olarak.  

## <a name="pipelines"></a>İşlem hatları
PythonScriptStep birden çok kez betik ya da parametreler değiştirmeden bir satır çağrılırken bir hata oluşur. Geçici çözüm, PipelineData nesne yeniden oluşturmaktır.

## <a name="fpgas"></a>FPGA
İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

1. Databricks kümesini öneri:
   
   Python 3 v4.x olarak Azure Databricks kümenizi oluşturun. Yüksek eşzamanlılık küme öneririz.
 
1. AML SDK yükleme hatası Databricks üzerinde daha fazla paketleri yüklendiğinde.

   Gibi bazı paketler `psutil upgrade libs`, çakışmaları neden olabilir. Yükleme hataları önlemek için paketleri dondurma LIB sürümüyle yükleyin. Bu sorun için Databricks ilgili ve AML SDK ilişkili değil. Örnek:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="diagnostic-logs"></a>Tanılama günlükleri
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyaları burada Canlı aşağıda verilmiştir:

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="get-more-support"></a>Daha fazla destek alın

Destek isteği gönderin ve teknik destek, forumlar ve diğer Yardım alın. [Daha fazla bilgi edinin...](support-for-aml-services.md)
