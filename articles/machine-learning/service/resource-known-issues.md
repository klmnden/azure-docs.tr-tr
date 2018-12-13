---
title: Bilinen sorunlar ve sorun giderme
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için sorun giderme ve bilinen sorunlar çözümleriyle birlikte bir listesini alın.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 45eb24bb8a49fb59ae44533e59b2760940ee5c1a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097718"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti
 
Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur. 

## <a name="sdk-installation-issues"></a>SDK yükleme sorunları

**Hata iletisi: 'PyYAML' kaldırılamıyor** 

Python için Azure Machine Learning SDK: PyYAML olan yüklü distutils proje. Bu nedenle, biz durumunda kısmi bir kaldırma için hangi dosyaların ait doğru bir şekilde belirlenemiyor. Bu hatayı yoksayma sırasında SDK'sı yüklemeye devam etmek için kullanın:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Azure Machine Learning işlem oluştururken sorun
GA sürümü önce Azure portalından, Azure Machine Learning çalışma alanı oluşturan bazı kullanıcıların bu çalışma alanında Azure Machine Learning işlem oluşturmak mümkün olmayabilir nadir bir fırsat yoktur. Bir destek isteği hizmetinde yükseltmek veya Portal veya SDK'yı kendiniz hemen engelini kaldırmak için yeni bir çalışma alanı oluşturun. 

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmeti dağıtılırken hata oluşturma görüntüsü. Geçici çözüm olan eklemek için "pynacl 1.2.1 ==" Conda dosyasına görüntü yapılandırması için pip bağımlılık olarak.  

## <a name="fpgas"></a>FPGA
İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

1. Databricks kümesini öneri:
   
   Python 3 v4.x olarak Azure Databricks kümenizi oluşturun. Yüksek eşzamanlılık küme öneririz.
 
2. AML SDK yükleme hatası Databricks üzerinde daha fazla paketleri yüklendiğinde.

   Gibi bazı paketler `psutil`, çakışmaları neden olabilir. Yükleme hataları önlemek için paketleri dondurma LIB sürümüyle yükleyin. Bu sorun için Databricks ilgili ve Azure ML SDK ilgili olmayan - bunu diğer kitaplıklar ile çok karşılaşabilecekleri. Örnek:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Alternatif olarak, yükleme sorunlarını Python kitaplıklar ile yan yana tutmak, init komut dosyalarını kullanabilirsiniz. Bu yaklaşım, resmi olarak desteklenen bir yaklaşım değildir. Başvurabilirsiniz [bu belge](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

3. Machine Learning otomatik Databricks üzerinde kullanırken görürseniz `Import error: numpy.core.multiarray failed to import`

   Geçici çözüm: Python kitaplığı içeri aktarma `numpy==1.14.5` , Databricks için kitaplığa oluşturma kümesi kullanarak [yükleme ve ekleme](https://docs.databricks.com/user-guide/libraries.html#create-a-library).

## <a name="azure-portal"></a>Azure portal
Doğrudan paylaşım bağlantısı SDK veya portalından çalışma alanınızda görüntülemeye giderseniz, uzantı normal genel bakış sayfası ile abonelik bilgilerini görüntülemek mümkün olmayacaktır. Siz de başka bir çalışma alanına geçmeniz mümkün olmayacaktır. Başka bir çalışma alanı görmeniz gerekiyorsa, doğrudan gitmek için geçici çözüm olan [Azure portalında](https://portal.azure.com) ve çalışma alanı adı arayın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Günlük dosyaları burada Canlı aşağıda verilmiştir:

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="get-more-support"></a>Daha fazla destek alın

Destek isteği gönderin ve teknik destek, forumlar ve diğer Yardım alın. [Daha fazla bilgi edinin...](support-for-aml-services.md)
