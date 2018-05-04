---
title: Azure Machine Learning Önizleme özellikleri kavramsal genel bakış | Microsoft Docs
description: Azure Machine Learning, Önizleme özellikleri abonelikler, hesaplar, çalışma alanları, projeler, vb. gibi kavramsal genel bakış.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: ea9da6f23fd08c09f9e805519487648480816f35
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning - kavramları

Bu makalede tanımlar ve Azure Machine Learning kullanmak için bilmeniz gereken kavramlar açıklanmaktadır. 

![Hiyerarşi kavramları](media/overview-general-concepts/hierarchy.png)

- **Abonelik:** bir Azure aboneliği kaynaklara erişim verir. Azure Machine Learning iç işlem, depolama ve birçok diğer Azure kaynaklarını ve Hizmetleri ile tümleşik olduğundan, çalışma ekranı her kullanıcı erişimi geçerli bir Azure aboneliğinizin olmasını gerektirir. Kullanıcılar ayrıca bu abonelik içindeki kaynaklara oluşturmak için yeterli izinlere sahip olmalıdır.


- **Deneme hesabı:** deneme hesabıdır Azure ML ve fatura araç tarafından gerekli bir Azure kaynağı. Sırayla projeleri içeren, çalışma alanlarını içerir. Olarak adlandırılan birden fazla kullanıcı ekleyebilirsiniz _kişilik_, bir deneme hesabı. Azure ML çalışma ekranı denemeleri çalıştırmak için kullanmak üzere bir deneme hesabına erişiminiz olması gerekir. 


- **Model yönetim hesabı** bir model yönetim ayrıca Azure ML tarafından modelleri yönetmek için gerekli bir Azure kaynağı hesabıdır. Modelleri ve bildirimleri kaydetme, kapsayıcılı web hizmetleri oluşturmak ve bunları yerel olarak veya bulutta dağıtmak için kullanabilirsiniz. Bu ayrıca diğer fatura Azure ML, araçtır.


- **Çalışma alanı:** bir çalışma alanı paylaşımı ve Azure ML işbirliği için birincil bir bileşendir. Projeleri bir çalışma alanı içinde gruplandırılır. Bir çalışma alanı sonra deneme hesabı için eklenene birden çok kullanıcıyla paylaşılabilir.


- **Proje:** Azure Machine Learning ile bir proje mantıksal bir sorunu çözmek için gerçekleştirilen tüm iş için bir kapsayıcısıdır. Yerel diskinizde tek bir dosya klasörüne eşlenir ve buna istediğiniz tüm dosyaları veya alt klasörleri ekleyebilirsiniz. Bir proje isteğe bağlı olarak kaynak denetimi ve işbirliği için bir Git deposu ile ilişkili olabilir.  

- **Deneme:** Azure ML içinde bir denemeyi tek giriş noktasından yürütülen bir veya daha fazla kaynak kodu dosyaları değil. Veri alımı, özellik Mühendisliği, model eğitim veya model değerlendirme gibi görevleri içerebilir. Şu anda Python Azure ML destekler veya PySpark yalnızca denemelerini.


- **Model:** Azure Machine Learning'de modelleri bir makine öğrenimi denemesinin ürününe bakın. Tarif olduklarından, doğru verilere uygulandığında tahmin edilen değerler oluşturur. Modelleri olabilir test veya üretim ortamları dağıtılan ve yeni veri Puanlama için kullanılır. Bir kez üretimde modelleri için performans ve veri kayması izlenir ve gerektiği gibi retrained. 

- **İşlem Hedefi:** işlem hedef denemelerinizi yürütmek için yapılandırdığınız işlem kaynaktır. Bu, yerel bilgisayar (Windows veya macOS), yerel bilgisayarınızda veya bir Linux VM Azure veya Hdınsight Spark kümesinde çalışan Docker kapsayıcısı olabilir.


- **Çalıştır:** işlem hedef bir deneme yürütme süresi olarak çalıştır deneme hizmetini tanımlar. Azure ML her çalıştırmayı bilgilerini otomatik olarak yakalar ve belirli bir deneme çalıştırma geçmişi biçiminde tüm geçmişini gösterir.

- **Ortam:** Azure Machine Learning ile bir ortamda dağıtmak ve Modellerinizi yönetmek için kullanılan belirli bir bilgisayar kaynağına gösterir. Yerel bilgisayarınızda, azure'da bir Linux VM veya içerik ve yapılandırma bağlı olarak Azure kapsayıcı Hizmeti'nde çalışan Kubernetes küme olabilir. Modelinizi bu ortamlarda çalışan bir Docker kapsayıcısı içinde barındırılan ve REST API uç noktası olarak gösteriliyor.


- **Yönetilen modeli:** Model yönetim modelleri web Hizmetleri olarak dağıtmak, Modellerinizi çeşitli sürümlerini yönetmek ve kendi performansını ve ölçümleri izleme olanak sağlar. Yönetilen modelleri bir Azure Machine Learning modeli yönetim hesabı ile kaydedilir.

- **Bildirimleri:** Model yönetim sistemi üretime bir model dağıtırken, model, bağımlılıkları, Puanlama komut dosyası, örnek veri ve şema kapsayan bir bildirimi içerir. Docker kapsayıcısı görüntüsünü oluşturmak için kullanılan tarif bildirimidir. Model Yönetimi'ni kullanarak bildirimleri otomatik oluşturma, farklı sürümlerini oluşturun ve bu bildirimleri yönetin. 


- **Resimler:** oluştur (ve yeniden oluşturmak için) bildirimleri kullanabilirsiniz Docker görüntüler. Bunları bulutta, IOT cihaz veya yerel makine üzerinde çalıştırmak için esneklik kapsayıcılı Docker yansımaları oluşturun. Görüntüleri kendi içinde bulunan ve yeni veri modelleri ile Puanlama için gerekli tüm bağımlılıkları içerir. 

- **Hizmetleri:** Model yönetim modelleri web Hizmetleri olarak dağıtmanızı sağlar. Web hizmeti mantığı ve bağımlılıkları görüntüye kapsüllenir. Her Web hizmeti, hizmet isteklerini belirli bir URL için görüntü hazır göre kapsayıcıları kümesidir. Bir web hizmeti tek bir dağıtım kabul edilir.
