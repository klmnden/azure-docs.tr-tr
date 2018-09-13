---
title: Azure Machine Learning Önizleme özelliklerinde kavramsal genel bakış | Microsoft Docs
description: Azure Machine Learning Önizleme özellikleri abonelikler, hesaplar, çalışma alanları, projeler, vb. gibi kavramsal genel bakış.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: f63b9c077e64b642adfd8c7eed5026563eb6319a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647202"
---
# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning - kavramları

Bu makalede, tanımlar ve Azure Machine Learning kullanmak için bilmeniz gereken kavramlar açıklanır. 

![Hiyerarşi kavramları](media/overview-general-concepts/hierarchy.png)

- **Abonelik:** Azure aboneliğinin Azure kaynaklarına erişim verir. Azure Machine Learning işlem, depolama ve birçok diğer Azure kaynaklarını ve Hizmetleri ile tamamen tümleşik olduğundan, Workbench, her kullanıcının geçerli bir Azure aboneliğine erişiminiz olduğunu gerektirir. Kullanıcıların, bu Abonelikteki kaynakları oluşturmak için yeterli izinlere de olmalıdır.


- **Deneme hesabı:** deneme hesabı, Azure ML ve fatura araç tarafından gereken bir Azure kaynağı. Bu sırayla projeleri içeren, çalışma alanlarını içerir. Olarak adlandırılan birden fazla kullanıcı ekleyebilirsiniz _kişilik_, bir deneme hesabına. Denemeleri çalıştırmak için Azure ML Workbench'i kullanabilmeniz için bir deneme hesabına erişiminiz olması gerekir. 


- **Model Yönetimi hesabı** model Yönetimi hesabı da tarafından Azure ML modelleri yönetmek için gereken bir Azure kaynağıdır. Kaydetme modelleri ve bildirimleri, kapsayıcılı web hizmetleri oluşturmak ve yerel olarak veya bulutta dağıtmak için kullanabilirsiniz. Azure ML, fatura diğer araç da sağlar.


- **Çalışma alanı:** birincil bileşen paylaşım ve işbirliği Azure ML için bir çalışma alanıdır. Projeleri, bir çalışma alanı içinde gruplandırılır. Bir çalışma alanı deneme hesabına eklenen birden çok kullanıcıya sahip ardından paylaşılabilir.


- **Proje:** Azure Machine Learning'de proje bir sorunu çözmek için yapılan tüm çalışmanın mantıksal kapsayıcısıdır. Yerel diskinizde tek bir dosya klasörüne eşlenir ve buna istediğiniz tüm dosyaları veya alt klasörleri ekleyebilirsiniz. İsteğe bağlı olarak bir proje kaynak denetimi ve işbirliği için bir Git deposu ile ilişkili olabilir.  

- **Deneme:** Azure ML bir tek giriş noktası yürütülebilir bir veya daha fazla kaynak kodu dosyaları bir deneme olduğunu. Veri alımı, özellik Mühendisliği, model eğitiminin ve model değerlendirme gibi görevleri içerebilir. Şu anda, Azure ML, Python destekler veya PySpark yalnızca denemeleri görüntüleyebilir.


- **Modeli:** Azure Machine Learning modelleri çarpımını bir makine öğrenimi denemesi için başvurun. Tarif olduklarından, doğru verilere uygulandığında, tahmin edilen değerler oluşturur. Modeller test veya üretim ortamları dağıtılan ve yeni veri Puanlama için kullanılır. Bir kez üretimde modelleri için performans ve veri değişikliklerini izlenir ve gerektiği şekilde retrained. 

- **İşlem Hedefi:** işlem hedefi denemelerinizi yürütmek için yapılandırdığınız işlem kaynağıdır. Yerel bilgisayar (Windows veya Mac OS x), yerel bilgisayarınızda veya bir Linux VM, Azure veya bir HDInsight Spark kümesinde çalışan Docker kapsayıcısını olabilir.


- **Çalıştır:** işlem hedefi bir deneme yürütme ömrünü olarak bir deneme hizmeti tanımlar. Azure ML, her çalıştırma bilgileri otomatik olarak yakalar ve belirli bir deneme çalıştırma geçmişi biçiminde tüm geçmişini gösterir.

- **Ortam:** Azure Machine Learning'de bir ortama dağıtma ve Modellerinizi yönetmek için kullanılan belirli bir bilgisayar kaynağına gösterir. Yerel bilgisayarınızda, azure'da bir Linux VM veya içerik ve yapılandırma bağlı olarak Azure Container Service'te çalışan bir Kubernetes kümesi olabilir. Modelinizi bu ortamlarda çalışan bir Docker kapsayıcısında barındırılan ve kullanıma sunulan bir REST API uç noktası olarak.


- **Yönetilen model:** Model Yönetimi Modellerinizi web Hizmetleri olarak dağıtmanıza, modellerin çeşitli sürümlerini yönetmek ve performans ve ölçümleri izleme olanak sağlar. Yönetilen modeller bir Azure Machine Learning Model Yönetimi hesabı ile kaydedilir.

- **Bildirimleri:** Model yönetimi sistemi üretime model dağıttığında, bu model, bağımlılıkları, Puanlama betik, örnek veri ve şema kapsayabilir bir bildirim içerir. Bir Docker kapsayıcı görüntüsü oluşturmak için kullanılan tarif bildirimidir. Model Yönetimi'ni kullanarak bildirimler otomatik olarak oluşturmak, farklı sürümler oluşturmayı ve bu bildirimleri yönetme. 


- **Resimler:** bildirimleri oluşturmak (ve yeniden oluşturmak için) kullanabileceğiniz Docker görüntüleri. Bulutta, IOT cihaz veya yerel makine üzerinde çalışması için esneklik kapsayıcılı Docker görüntüleri oluşturun. Görüntüleri müstakil ve modelleri yeni verilerle Puanlama için gereken tüm bağımlılıkları içerir. 

- **Hizmetler:** Model yönetimi, Modellerinizi web Hizmetleri olarak dağıtmanızı sağlar. Web hizmeti mantık ve bağımlılıkları görüntüye kapsüllenir. Her Web hizmeti, kapsayıcı görüntüsü hazır hizmet isteklerini belirli bir URL'ye göre kümesidir. Bir web hizmeti, tek bir dağıtım olarak kabul edilir.
