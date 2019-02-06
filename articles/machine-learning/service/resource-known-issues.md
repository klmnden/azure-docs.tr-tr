---
title: Bilinen sorunlar ve sorun giderme
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için sorun giderme ve bilinen sorunlar çözümleriyle birlikte bir listesini alın.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: b10e434aece0ac214a0fd397ea94cbeccca4e44a
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746499"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti

Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur.

## <a name="sdk-installation-issues"></a>SDK yükleme sorunları

**Hata iletisi: 'PyYAML' kaldırılamıyor**

Azure Machine için Python SDK'sı öğrenme: PyYAML bir distutils yüklü projesidir. Bu nedenle, biz kısmi kaldırma ise hangi dosyaların kendisine ait doğru bir şekilde belirlenemiyor. Bu hatayı yoksayma sırasında SDK'sı yüklemeye devam etmek için kullanın:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Azure Machine Learning işlem oluştururken sorun

GA sürümü önce Azure portalından, Azure Machine Learning çalışma alanı oluşturan bazı kullanıcıların bu çalışma alanında Azure Machine Learning işlem oluşturmak mümkün olmayabilir nadir bir fırsat yoktur. Bir destek isteği hizmetinde yükseltmek veya Portal veya SDK'yı kendiniz hemen engelini kaldırmak için yeni bir çalışma alanı oluşturun.

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmeti dağıtılırken hata oluşturma görüntüsü. Geçici çözüm olan eklemek için "pynacl 1.2.1 ==" Conda dosyasına görüntü yapılandırması için pip bağımlılık olarak.

## <a name="deployment-failure"></a>Dağıtım hatası

Gözlemlerseniz, `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`, daha fazla belleğe sahip bir dağıtımda kullanılan VM'ler için SKU değişimi.

## <a name="fpgas"></a>FPGA'lar
İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

1. Daha fazla paketleri yüklendiğinde Databricks üzerinde bir Azure Machine Learning SDK yükleme hataları.

   Gibi bazı paketler `psutil`, çakışmaları neden olabilir. Yükleme hataları önlemek için paketleri dondurma LIB sürümüyle yükleyin. Bu sorun, Databricks ve Azure Machine Learning hizmeti SDK'sı - değil ilgili, diğer kitaplıklar ile çok karşılaşacağınız. Örnek:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   Alternatif olarak, yükleme sorunlarını Python kitaplıklar ile yan yana tutmak, init komut dosyalarını kullanabilirsiniz. Bu yaklaşım, resmi olarak desteklenen bir yaklaşım değildir. Başvurabilirsiniz [bu belge](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

2. Machine Learning otomatik Databricks üzerinde bir çalışmayı iptal edip yeni bir deneme çalıştırma başlatmak istiyorsanız kullanırken, Azure Databricks kümenizi yeniden başlatın.

3. 10'dan fazla yineleme varsa, otomatik ml ayarlarında belirlenen `show_output` için `False` çalıştırmayı ne zaman gönderdiğiniz.


## <a name="azure-portal"></a>Azure portal
Doğrudan paylaşım bağlantısı SDK veya portalından çalışma alanınızda görüntülemeye giderseniz, uzantı normal genel bakış sayfası ile abonelik bilgilerini görüntülemek mümkün olmayacaktır. Siz de başka bir çalışma alanına geçmeniz mümkün olmayacaktır. Başka bir çalışma alanını görüntülemek gerekiyorsa, doğrudan gitmek için geçici çözüm olan [Azure portalında](https://portal.azure.com) ve çalışma alanı adı arayın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir.
Günlük dosyaları burada Canlı aşağıda verilmiştir:

## <a name="resource-quotas"></a>Kaynak kotaları

Hakkında bilgi edinin [kaynak kotaları](how-to-manage-quotas.md) Azure Machine Learning'i kullanmaya çalışırken hatalarla karşılaşabilirsiniz.

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir işten işlem hedefi bir yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, oluşturma veya uzaktan yürütme için gönderilen bir ML ardışık işlem hedefi iliştirilmeye çalışırsanız hata alırsınız.

## <a name="get-more-support"></a>Daha fazla destek alın

Destek isteği gönderin ve teknik destek, forumlar ve diğer Yardım alın. [Daha fazla bilgi edinin...](support-for-aml-services.md)
