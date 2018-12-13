---
title: 'ML bulut: hüküm ve mimarisi'
titleSuffix: Azure Machine Learning service
description: Mimari, terminolojisi ve Azure Machine Learning hizmeti oluşturan kavramları hakkında bilgi edinin. Ayrıca genel iş akışı hizmeti ve Azure Machine Learning hizmeti tarafından kullanılan Azure hizmetlerini kullanma hakkında bilgi.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 3966d4b27f0e3d42f47d84fb5c9f5c8519a27b6c
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184738"
---
# <a name="how-the-azure-machine-learning-service-works-architecture-and-concepts"></a>Azure Machine Learning hizmetinin nasıl çalıştığı: Mimari ve kavramları

Bu belgede, Azure Machine Learning hizmeti kavramları ve mimarisi açıklanmaktadır. Aşağıdaki diyagramda, hizmet ana bileşenleri gösteren ve hizmeti kullanırken genel iş akışı gösterilmektedir: 

[![Azure Machine Learning hizmeti mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

İş akışı genellikle aşağıdaki adımları izleyin:

1. Makine öğrenimi betiklerini Eğitim geliştirme __Python__.
1. Oluşturma ve yapılandırma bir __hedef işlem__.
1. __Komut Gönderme__ ortamında çalıştırmak için yapılandırılmış işlem hedefine. Eğitim sırasında işlem hedef çalışma kayıtlara depolar. bir __veri deposu__. Kayıtlar var. kaydedilir bir __deneme__.
1. __Denemeyi sorgu__ için ölçümleri geçerli ve geçmiş çalıştırmalardan oturum. Ölçümler istenilen bir sonucu belirtmezseniz, geri adım 1 ve betiklerinizi üzerinde yineleme döngüsü.
1. Tatmin edici bir çalıştırma bulunduktan sonra kalıcı modelinde kaydetme __modeli kayıt defteri__.
1. Puanlama betiği geliştirin.
1. __Görüntü oluşturma__ ve içinde kaydetmek __görüntü kayıt__. 
1. __Görüntüyü dağıtmak__ olarak bir __web hizmetini__ azure'da.


> [!NOTE]
> Bu belgenin koşullarını ve Azure Machine Learning tarafından kullanılan kavramları tanımlar, ancak terimleri ve kavramları Azure platformu tanımlamıyor. Azure platformu terminolojisi hakkında daha fazla bilgi için bkz. [Microsoft Azure sözlüğünü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Çalışma alanı

Çalışma alanı, Azure Machine Learning hizmeti için en üst düzey kaynaktır. Azure Machine Learning hizmeti kullanırken oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerdir.

Çalışma alanı, modeli eğitmek için kullanılan işlem hedefleri listesini tutar. Ayrıca, günlükler, ölçümler, çıkış ve komut dosyalarınızın anlık görüntüsünü de dahil olmak üzere, bir eğitim çalıştırmalarının geçmişini tutar. Bu bilgiler, hangi eğitim çalıştırmanın en iyi modeli belirlemek için kullanılır.

Modelleri, çalışma alanı ile kaydedilir. Kayıtlı bir model ve puanlama betik, bir görüntü oluşturmak için kullanılır. Görüntü daha sonra Azure Container Instances, Azure Kubernetes hizmeti ya da bir alanda programlanabilir kapı dizileri (FPGA) bir REST tabanlı bir HTTP uç noktası olarak dağıtılabilir. Ayrıca bir modül olarak Azure IOT Edge cihazına olarak da dağıtılabilir.

Her bir çalışma alanı birden çok kişi tarafından paylaşılabilir ve birden çok çalışma alanı oluşturabilirsiniz. Bir çalışma alanı paylaşırken kullanıcılara aşağıdaki rolleri atayarak çalışma alanına erişim denetimi:

* Sahip
* Katılımcı
* Okuyucu

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından kullanılan bazı Azure kaynakları otomatik olarak oluşturur:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) -eğitim sırasında ve bir modeli dağıtırken kullanılan docker kapsayıcıları kaydeder.
* [Azure depolama](https://azure.microsoft.com/services/storage/) - çalışma alanı için varsayılan veri deposu olarak kullanılır.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) - izleme Modellerinizi ilgili bilgi depolar.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) - depoları gizli dizileri tarafından kullanılan işlem hedeflerini ve diğer hassas bilgiler çalışma alanı tarafından gerekli.

> [!NOTE]
> Yeni sürümlerini oluşturmak yerine var olan Azure hizmetleri de kullanabilirsiniz. 

Aşağıdaki diyagramda, çalışma alanının bir taksonomi şöyledir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.svg)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="model"></a>Model

En basit haliyle bir girdi alır ve çıktıyı üretir kod parçasını modelidir. Makine öğrenme modeli oluşturma, bir algoritma seçme, verilerle sağlama ve ayarlama hiperparametreleri içerir. Eğitim eğitim işlemi sırasında model öğrendikleriniz yalıtan eğitilen bir modelin üreten yinelemeli bir işlemdir.

Bir model, Azure Machine learning'de bir çalıştırma tarafından oluşturulur. Azure Machine Learning dışında eğitilmiş bir modeli de kullanabilirsiniz. Bir model, bir Azure Machine Learning hizmeti çalışma alanı altında kaydedilebilir.

Azure Machine Learning hizmeti framework belirsiz ' dir. Scikit gibi bir modeli oluştururken herhangi bir popüler machine learning çerçeveyi kullanabilirsiniz-xgboost, PyTorch, TensorFlow, Chainer ve CNTK öğrenin.

Modeli ilişkin bir örnek için bkz [hızlı başlangıç: Bir machine learning hizmeti çalışma alanını oluşturma](quickstart-get-started.md) belge.

### <a name="model-registry"></a>Model kayıt defteri

Model kayıt defteri, Azure Machine Learning hizmeti çalışma alanınızdaki tüm modelleri izler. 

Modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri yeni bir sürüm olduğunu varsayar. Sürümü artırılır ve yeni model adı altında kayıtlı.

Modeli kaydedin ve sonra bu etiketleri aramak modellerinde kullanma, ek meta veri etiketleri sağlayabilirsiniz.

Görüntü tarafından kullanılmakta olan modeller nelze odstranit.

Model kaydediliyor ilişkin bir örnek için bkz [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) belge.

## <a name="image"></a>Görüntü

Görüntüleri modelini kullanmak için gerekli tüm bileşenleri ile birlikte bir model güvenilir bir şekilde dağıtmak için bir yol sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Bir model.
* Bir Puanlama komut dosyası veya uygulama. Bu betik, giriş modeline geçirin ve model çıkışı döndürmek için kullanılır.
* Model veya Puanlama betik uygulaması tarafından gerekli bağımlılıkları.  Örneğin, Python Paket bağımlılıklarını listeleyen bir Conda ortam dosyası içerebilir.

Azure Machine Learning tarafından oluşturulan görüntüleri iki tür vardır:

* FPGA görüntüsü: Bir alanda programlanabilen geçit dizileri Azure bulutunda dağıtım yapılırken kullanılır.
* Docker görüntüsü: Dağıtırken FPGA dışında hedefleri hesaplamak için kullanılır. Örneğin, Azure Container Instances ve Azure Kubernetes hizmeti.

Görüntü oluşturma örneği için bkz: [bir Azure Container Instance görüntü sınıflandırma modelinde dağıtma](tutorial-deploy-models-with-aml.md) belge.

### <a name="image-registry"></a>Görüntü kayıt defteri

Görüntü kayıt defteri, görüntüleri Modellerinizi oluşturulan izler. Ek meta veri etiketleri, görüntü oluştururken sağlayabilir. Meta veri etiketleri görüntü kayıt depolanır ve görüntünüzü bulmak için sorgulanabilir.

## <a name="deployment"></a>Dağıtım

Görüntünüzü örneklemesi ya da bir Web bulutta barındırılan hizmet veya tümleşik cihaz dağıtımları için bir IOT modülü dağıtımıdır. 

### <a name="web-service"></a>Web hizmeti

Dağıtılmış bir web hizmetini Azure Container Instances, Azure Kubernetes hizmeti veya alanda programlanabilir kapı dizileri (FPGA) kullanabilirsiniz.
Hizmet, model, betik ve ilişkili dosyaları kapsülleyen bir görüntüden oluşturulur. Görüntü, web hizmetine gönderilen Puanlama istekleri alan yük dengeli bir HTTP uç vardır.

Azure, bu özelliği etkinleştirmek seçtiyseniz, Application Insight telemetri ve/veya modeli telemetri toplayarak Web hizmeti dağıtımınızı izlemenize yardımcı olur. Telemetri verilerini yalnızca size erişebilir ve depolama hesabı örnekleri ve Application Insights içinde depolanır.

Otomatik ölçeklendirme etkinleştirdiyseniz, Azure, dağıtımınızı otomatik olarak ölçeklendirin.

Bir web hizmeti olarak bir model dağıtımına ilişkin bir örnek için bkz [bir Azure Container Instance görüntü sınıflandırma modelinde dağıtma](tutorial-deploy-models-with-aml.md) belge.

### <a name="iot-module"></a>IOT Modülü

Dağıtılan bir IOT modülü modeliniz ve ilişkili komut dosyası veya uygulama ve herhangi ek bağımlılıklar içeren bir Docker kapsayıcıdır. Uç cihazlarda Azure IOT Edge'i kullanarak bu modülleri dağıtılır. 

İzleme etkinleştirdiyseniz, Azure, Azure IOT Edge modülü içinde modelinden telemetri verileri toplar. Telemetri verilerini yalnızca, erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IOT Edge, modülünüzde çalıştığından ve onu barındıran cihaz izleme sağlayacaktır.

## <a name="datastore"></a>Veri deposu

Bir veri deposu depolama soyutlama bir Azure depolama hesabıdır. Veri deposu Azure blob kapsayıcısı veya bir Azure dosya paylaşımı, arka uç depolama kullanabilirsiniz. Varsayılan veri deposu her çalışma alanına sahiptir ve ek veri depoları kaydedebilir. 

Python SDK API'si veya Azure Machine Learning CLI depolamak ve deposundan dosyaları almak için kullanın. 

## <a name="run"></a>Çalıştırın

Aşağıdaki bilgileri içeren bir kaydı bir çalıştırmadır:

* Meta verileri çalıştırmayı (zaman damgası, süresi vb.) hakkında
* Betiğinizi tarafından günlüğe ölçümleri
* Çıktı tarafından denemeyi autocollected dosyaları veya sizin tarafınızdan açıkça karşıya.
* Bir anlık görüntüsünü çalıştırma önce komut dosyalarınızı içeren dizine

Bir modeli eğitmek için bir betik gönderdiğinizde çalıştırma oluşturulur. Bir çalıştırma, sıfır veya daha fazla alt çalıştırma olabilir. Kendi alt çalışır, böylece en üst düzey çalışma, her biri olabilir, iki alt çalışma olabilir.

Bir model eğitip geleceği görüntüleme örneği üretilen çalıştırmaları için bkz. [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

## <a name="experiment"></a>Deneme

Bir deney, belirli bir betik birçok çalıştırmalardan gruplandırmasıdır. Her zaman, bir çalışma alanına aittir. Bir farklı çalıştır gönderdiğinizde bir deney adı sağlayın. Çalıştırma için bilgileri, bu deneme altında depolanır. Bir farklı çalıştır gönderin ve mevcut olmayan bir deney adı belirtin, bu ada sahip yeni bir deneme otomatik olarak oluşturulur.

Bir deney kullanma örneği için bkz. [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-get-started.md) belge.

## <a name="pipeline"></a>İşlem hattı

Machine learning işlem hatlarını oluşturmak ve birleştirmek iş akışları yönetmek için kullanılan makine öğrenimi aşamaları. Örneğin, bir işlem hattı, veri hazırlama, model eğitiminin, model dağıtımı ve çıkarım aşamaları içerebilir. Her aşamada, her biri çeşitli işlem hedeflerini katılımsız çalışabilir, birden çok adım kapsayabilir.

Machine learning işlem hatlarını bu hizmeti hakkında daha fazla bilgi için bkz [işlem hatları ve Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>Hedef işlem

Bir işlem hedefine eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan işlem kaynağıdır. Desteklenen işlem hedefleri şunlardır: 

| Hedef işlem | Eğitim | Dağıtım |
| ---- |:----:|:----:|
| Yerel bilgisayarınıza | ✓ | &nbsp; |
| Azure Machine Learning işlem | ✓ | &nbsp; |
| Azure'da bir Linux sanal makinesi</br>(veri bilimi sanal makinesi gibi) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; | &nbsp; |
| Azure Data Lake Analytics | ✓ | &nbsp; |
| HDInsight için Apache Spark | ✓ | &nbsp; |
| Azure Container Örneği | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(Alanda programlanabilen geçit dizileri) | &nbsp; | ✓ |

İşlem hedefleri, bir çalışma alanına eklenir. İşlem yerel makine dışındaki hedefleri çalışma alanının kullanıcılar tarafından paylaşılır.

### <a name="managed-and-unmanaged-compute-targets"></a>Yönetilen ve yönetilmeyen işlem hedefleri

**Yönetilen** işlem hedefleri oluşturulan ve Azure Machine Learning hizmeti tarafından yönetilir. Bu işlem, hedefleri ML iş yükleri için iyileştirilmiştir. __Azure Machine Learning işlem__ tek (4 Aralık 2018) şu anda hedef yönetilen bilgi işlem. Ek yönetilen bir işlem hedefleri gelecekte eklenebilir. ML bilgi işlem örnekleri, Azure portalı, Azure Machine Learning SDK veya Azure CLI kullanarak doğrudan çalışma oluşturulabilir. Diğer tüm işlem hedefleri çalışma alanı dışında oluşturulmalı ve ardından bağlı.

**Yönetilmeyen** hedefleri olmayan işlem yönetilen Azure Machine Learning hizmeti. Bunları Azure Machine Learning dışında oluşturup kullanmadan önce çalışma alanınıza eklemek gerekebilir. Bu işlem, hedefleri korumak için ek adımlar gerektirmez veya ML iş yükleri için performansı geliştirin.

Eğitim için işlem hedef seçme hakkında daha fazla bilgi için bkz: [seçin ve modelinizi eğitmek için bir işlem hedefine](how-to-set-up-training-targets.md) belge.

Dağıtım için bir işlem hedef seçme hakkında daha fazla bilgi için bkz: [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md) belge.

## <a name="run-configuration"></a>Çalıştırma yapılandırma

Bir çalıştırma yapılandırma komut dosyası içinde belirtilen işlem hedefi nasıl çalıştırılması gerektiğini tanımlayan bir yönerge kümesidir. Bu, çok sayıda bir Python ortamı kullanın veya belirtiminden yerleşik bir Conda ortam kullanmayı gibi davranış tanımları içerir.

Bir çalıştırma yapılandırma eğitim komut dosyanızı içeren veya bir bellek içi nesne olarak oluşturulur ve farklı çalıştır göndermek için kullanılan dizini içinde bir dosyaya kalıcı.

Çalışma yapılandırmaları, bkz: [seçin ve modelinizi eğitmek için bir işlem hedefine](how-to-set-up-training-targets.md) belge.

## <a name="training-script"></a>Eğitim betiği

Bir modeli eğitmek için eğitim betiğini ve ilişkili dosyaları içeren dizini belirtin. Siz de eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deney adı belirtin. Eğitimi sırasında tüm dizinde eğitim ortama (işlem hedefi) kopyalanır ve çalışma yapılandırması tarafından belirtilen kodun başlatılır. Dizinin bir anlık görüntü, ayrıca çalışma alanında denemeyi altında depolanır.

Bir örnek için bkz [Python ile bir çalışma alanı oluşturma](quickstart-get-started.md)

## <a name="logging"></a>Günlüğe kaydetme

Azure Machine Learning Python SDK'sı Python betiğinizde Çözümünüzü geliştirirken rastgele ölçümleri oturum için kullanın. Sonrasında çalıştırma, çalıştırmayı dağıtmak istediğiniz model üretilen varsa belirlemek için ölçümleri sorgulayın. 

## <a name="snapshot"></a>Anlık Görüntü

Bir çalıştırma gönderirken, Azure Machine Learning komut dosyasına bir zip dosyası olarak içeren ve işlem hedefe gönderir dizini sıkıştırır. Zip genişletilmiştir ve var. bir komut dosyasını çalıştırın. Azure Machine Learning, zip dosyası da çalıştırma kaydı bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydı göz atabilir ve anlık görüntü indirin.

## <a name="activity"></a>Etkinlik

Bir etkinlik, uzun süre çalışan bir işlemi temsil eder. Aşağıdaki işlemleri etkinlikleri örnekleri şunlardır:

* Oluşturma ya da işlem hedefi silme
* Bir işlem hedefte betik çalıştırma

Bu işlemlerin ilerleme durumunu kolayca izleyebilmek etkinlikleri SDK veya Web UI aracılığıyla bildirimleri sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmaya başlamak için aşağıdaki bağlantıları kullanın:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Hızlı Başlangıç: Python ile bir çalışma alanı oluşturma](quickstart-get-started.md)
* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
