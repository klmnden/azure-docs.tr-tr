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
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 80bb7af0f7ed20336ab08d4f3ca9639057b9c67f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65149764"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning hizmeti

Bu makalede, bulma ve hataları düzeltin veya Azure Machine Learning hizmeti kullanılırken karşılaşılan hatalar yardımcı olur.

## <a name="visual-interface-issues"></a>Görsel arabirimi sorunları

Machine learning hizmet sorunları için görsel arabirim.

### <a name="long-compute-preparation-time"></a>Uzun bir işlem hazırlık süresi

Yeni işlem oluşturmak veya işlem tarafından gerçekleştirilen işlemlerin zaman bırakma evoke, birkaç dakika veya daha uzun olabilir. Takım, iyileştirme için çalışmaktadır.


### <a name="cannot-run-an-experiment-only-contains-dataset"></a>Bir denemeyi çalıştırma yalnızca içeremez veri kümesi 

Çalıştırmak istediğiniz bir denemeyi yalnızca veri kümesini görselleştirmenin veri kümesi içerir. Ancak, çalışmasına izin olmayan bir denemeyi yalnızca veri kümesini hemen içeren. Biz bu sorunu düzeltmeye etkin.
 
Düzeltme önce herhangi bir veri dönüştürme Modülü (veri kümesi meta verileri düzenlemek, bölünmüş veri vb. Sütunları Seç) veri kümesine bağlanmak ve denemeyi çalıştırın. Ardından veri kümesini görselleştirebilirsiniz. 

Aşağıdaki görüntüde nasıl: ![visulize veri](./media/resource-known-issues/aml-visualize-data.png)

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

## <a name="fpgas"></a>FPGA

İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

Tensor Flow otomatik makine öğrenimi tensor flow sürümü 1.13 şu anda desteklemiyor. Bu sürümü yüklemeden Paket bağımlılıklarını çalışmayı durdurmasına neden olur. Gelecekteki bir sürümde bu sorunu düzeltmek için çalışıyoruz. 

### <a name="experiment-charts"></a>Deneme grafikleri

Otomatik ML deneme yinelemelerini gösterilen ikili sınıflandırma grafikleri (duyarlık geri çekme, ROC, elde eğri vb.) işleme corectly kullanıcı arabiriminde 4/12 beri değildir. Grafik çizimleri daha iyi performans gösteren modelleri ile alt sonuçlar burada gösterilen ters sonuçlar gösteriliyor, şu anda içindedir. Bir çözüm incelenmektedir.

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

### <a name="failure-when-installing-packages"></a>Paketler yüklenirken hata

Daha fazla paketleri yüklendiğinde azure Machine Learning SDK yüklemesi Azure Databricks üzerinde başarısız olur. Gibi bazı paketler `psutil`, çakışmaları neden olabilir. Yükleme hataları önlemek için kitaplığı sürüm dondurma tarafından paketleri yükleyin. Bu sorun, Databricks ve Azure Machine Learning hizmeti SDK'sını ilişkilidir. Çok diğer kitaplıkları, bu sorunla karşılaşabilirsiniz. Örnek:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Alternatif olarak, yükleme sorunlarını Python kitaplıkları karşılıklı tutmak, init komut dosyalarını kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için [küme kapsamlı init betikleri](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Bir otomatik makine öğrenme çalıştırması iptal et

Çalıştırma iptal edin ve çalıştırın, yeni bir deneme başlatın otomatik makine öğrenimi özellikleri Azure Databricks üzerinde kullandığınızda, Azure Databricks kümenizi yeniden başlatın.

### <a name="10-iterations-for-automated-machine-learning"></a>> 10 yinelemeden otomatik machine learning için

10'dan fazla yinelemeler, varsa ayarları, öğrenme otomatik makine kümesi `show_output` için `False` çalıştırmayı ne zaman gönderdiğiniz.

### <a name="widget-for-the-azure-machine-learning-sdkautomated-machine-learning"></a>Azure Machine Learning SDK/otomatik machine learning için pencere öğesi

Not defterlerini HTML pencere öğeleri ayrıştıramadığından, Azure Machine Learning SDK'sı pencere öğesi bir Databricks not defteri desteklenmez. Pencere öğesi, Azure Databricks not defteri hücresinde bu Python kodu kullanarak, portalda görüntüleyebilirsiniz:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>İçeri aktarma hatası: 'Pandas.core.indexes' adlı bir modül yok

Kullandığınızda, bu hatayı görürseniz, machine learning otomatik:

1. Azure Databricks kümenizde iki paketleri yüklemek için şu komutu çalıştırın: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Ayırma ve ardından, not defterinizin kümeye yeniden bağlayın. 

Bu adımlar sorunu gidermezse, kümeyi yeniden başlatmayı deneyin.

## <a name="azure-portal"></a>Azure portal

Doğrudan paylaşım bağlantısı SDK veya portalından çalışma alanınızda görüntülemeye giderseniz, uzantı normal genel bakış sayfası ile abonelik bilgilerini görüntülemek mümkün olmayacaktır. Siz de başka bir çalışma alanına geçmeniz mümkün olmayacaktır. Başka bir çalışma alanını görüntülemek gerekiyorsa, doğrudan gitmek için geçici çözüm olan [Azure portalında](https://portal.azure.com) ve çalışma alanı adı arayın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Bazı günlüklerini görmek için ziyaret edin [Azure portalında](https://portal.azure.com) ve seçin ve çalışma alanı **çalışma alanı > deneme > Çalıştır > günlükleri**.

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
