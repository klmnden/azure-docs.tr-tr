---
title: Mimari ve temel kavramlar
titleSuffix: Azure Machine Learning service
description: Mimari, koşulları, kavramlar ve Azure Machine Learning hizmeti oluşturan iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/15/2019
ms.custom: seodec18
ms.openlocfilehash: 2196e375db582202997b838d05c902db95b3a3ad
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461488"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Azure Machine Learning hizmetinin nasıl çalıştığı: Mimari ve kavramları

Mimari, kavramlar ve Azure Machine Learning hizmeti için iş akışı hakkında bilgi edinin. Ana bileşenleri ve hizmet bu hizmeti kullanmak için genel iş akışı aşağıdaki diyagramda gösterilmiştir:

![Azure Machine Learning hizmeti mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>İş akışı

Makine model iş akışı genellikle öğrenimi bu sırayı takip eder:

1. **Eğitme**
    + Makine öğrenimi betiklerini Eğitim geliştirme **Python** veya görsel arabirim.
    + Oluşturma ve yapılandırma bir **hedef işlem**.
    + **Komut Gönderme** ortamında çalıştırmak için yapılandırılmış işlem hedefine. Eğitim sırasında komut dosyaları okuyabilir ya da yazma **veri deposu**. Ve olarak yürütme kayıtlarını kaydedilir **çalıştıran** içinde **çalışma** ve altında gruplandırılmış **denemeleri**.

1. **Paket** - tatmin edici bir çalıştırma bulunduktan sonra kalıcı modelde kaydetme **modeli kayıt defteri**.

1. **Doğrulama** - **denemeyi sorgu** için ölçümleri geçerli ve geçmiş çalıştırmalardan oturum. Ölçümler istenilen bir sonucu göstermediği, geri adım 1 ve betiklerinizi üzerinde yineleme döngüsü.

1. **Dağıt** -geliştirme modelini kullanan Puanlama betiğine ve **model dağıtma** olarak bir **web hizmetini** azure'da yenilemek için bir **IOT Edge cihazı**.

1. **İzleyici** -izlemek **veri değişikliklerini** dağıtılmış bir modelinin bir eğitim veri kümesi ve çıkarım verileri arasında. Gerekli olduğunda tekrar yeni bir eğitim veri modeli yeniden eğitme için 1. adım döngüsü.

## <a name="tools-for-azure-machine-learning"></a>Azure Machine Learning için Araçlar 

Azure Machine Learning için bu araçları kullanın:

+  Hizmet ile herhangi bir Python ortamda etkileşim [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
+ Makine öğrenimi etkinliklerle otomatikleştirmek [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli).
+ Visual Studio Code ile kod yazma [Azure Machine Learning VS Code uzantısı](how-to-vscode-tools.md) 
+ Kullanım [görsel arabirim (Önizleme) Azure Machine Learning hizmeti için](ui-concept-visual-interface.md) kod yazmadan iş akışı adımları gerçekleştirmek için.

## <a name="glossary-of-concepts"></a>Sözlük kavramları

+ <a href="#workspaces">Çalışma alanı</a>
+ <a href="#experiments">Denemeler</a>
+ <a href="#models">Modelleri</a>
+ <a href="#run-configurations">Çalışma yapılandırması</a>
+ <a href="#datasets-and-datastores">Veri kümesi ve veri depoları</a>
+ <a href="#compute-targets">Hedef işlem</a>
+ <a href="#training-scripts">Eğitim betiği</a>
+ <a href="#runs">Çalıştırma</a>
+ <a href="#github-tracking-and-integration">Git izleme</a>
+ <a href="#snapshots">Anlık görüntü</a>
+ <a href="#activities">Etkinlik</a>
+ <a href="#images">Görüntü</a>
+ <a href="#deployment">Dağıtım</a>
+ <a href="#web-service-deployments">Web Hizmetleri</a>
+ <a href="#iot-module-deployments">IOT modülleri</a>
+ <a href="#ml-pipelines">ML işlem hatları</a>
+ <a href="#logging">Logging</a>

> [!NOTE]
> Hüküm ve Azure Machine Learning hizmeti tarafından kullanılan kavramları bu makalede tanımlasa da terimleri ve kavramları Azure platformu tanımlamıyor. Azure platformu hakkında daha fazla bilgi için bkz. [Microsoft Azure sözlüğünü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).


### <a name="workspaces"></a>Çalışma Alanları

[Çalışma alanı](concept-workspace.md) Azure Machine Learning hizmeti için en üst düzey bir kaynaktır. Azure Machine Learning hizmeti kullanırken oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerdir.

Çalışma alanının bir taksonomi, aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Çalışma alanları hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning çalışma alanı nedir?](concept-workspace.md).

### <a name="experiments"></a>Denemeler

Bir deney, belirtilen betiği çok sayıda çalıştırmalardan gruplandırmasıdır. Her zaman, bir çalışma alanına aittir. Bir farklı çalıştır gönderdiğinizde bir deney adı sağlayın. Çalıştırma için bilgileri, bu deneme altında depolanır. Bir farklı çalıştır gönderin ve mevcut olmayan bir deney adı belirtin, bu yeni belirtilen ada sahip yeni bir deneme otomatik olarak oluşturulur.

Bir deney kullanma örneği için bkz. [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-run-cloud-notebook.md).

### <a name="models"></a>Modeller

En basit haliyle bir girdi alır ve çıktıyı üretir kod parçasını modelidir. Makine öğrenme modeli oluşturma, bir algoritma seçme, verilerle sağlama ve ayarlama hiperparametreleri içerir. Eğitim eğitim işlemi sırasında model öğrendikleriniz yalıtan eğitilen bir modelin üreten yinelemeli bir işlemdir.

Bir model, Azure Machine learning'de bir çalıştırma tarafından oluşturulur. Azure Machine Learning dışında eğitilmiş bir modeli de kullanabilirsiniz. Bir Azure Machine Learning hizmeti çalışma alanında bir model kaydedebilirsiniz.

Azure Machine Learning framework bağımsız bir hizmettir. Bir modeli oluşturduğunuzda, Scikit-öğrenme, XGBoost, PyTorch, TensorFlow ve bağlayıcı gibi tüm popüler makine öğrenmesi çerçeveleri kullanabilirsiniz.

Modeli ilişkin bir örnek için bkz [Öğreticisi: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

**Modeli kayıt defteri** Azure Machine Learning hizmeti çalışma alanınızdaki tüm modelleri izler.

Modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri yeni bir sürüm olduğunu varsayar. Sürümü artırılır ve yeni modeli aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlar ve modelleri için arama yaptığınızda etiketleri kullanın.

> [!TIP]
> Kayıtlı bir model, modeli bir veya daha fazla dosyaların mantıksal bir kapsayıcıdır. Birden çok dosyasında depolanan bir model varsa, örneğin, bunları tek bir model Azure Machine Learning çalışma alanınızda kaydedebilirsiniz. Kayıt sonrasında sonra indirin veya kayıtlı modeli dağıtabilir ve kaydedilmiş tüm dosyalar alırsınız.

Etkin bir dağıtım tarafından kullanılan bir kayıtlı modeli silinemiyor.

Model kaydediliyor ilişkin bir örnek için bkz [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

### <a name="run-configurations"></a>Çalışma yapılandırmaları

Bir çalıştırma yapılandırma komut dosyası içinde belirtilen işlem hedefi nasıl çalıştırılması gerektiğini tanımlayan bir yönerge kümesidir. Yapılandırma çok sayıda bir Python ortamı kullanmak mı, yoksa bir belirtiminden yerleşik bir Conda ortamı kullanacağınızı gibi davranış tanımları içerir.

İçinde bir eğitim betiğini içeren dizine bir dosya içine bir çalıştırma yapılandırma kalıcı ya da bir bellek içi nesne olarak oluşturulur ve farklı çalıştır göndermek için kullanılır.

Çalışma yapılandırmaları, bkz: [seçin ve modelinizi eğitmek için bir işlem hedefine](how-to-set-up-training-targets.md).

### <a name="datasets-and-datastores"></a>Veri kümeleri ve veri depoları

**Azure Machine Learning veri kümeleri** (Önizleme) erişimi daha kolay hale getirmek ve verilerinizle çalışır. Veri kümeleri, veri modeli eğitimi gibi çeşitli senaryolarda yönetin ve işlem hattı oluşturma. Azure Machine Learning SDK'sını kullanarak, temel alınan depolama alanına erişmek, keşfedin ve verileri hazırlama, farklı veri kümesi tanımları yaşam döngüsünü yönetme ve eğitim hem de üretim kullanılan veri kümeleri arasında karşılaştırma.

Veri kümelerini kullanma gibi popüler biçimlerde verilerle çalışmak için yöntemler sağlar `from_delimited_files()` veya `to_pandas_dataframe()`.

Daha fazla bilgi için [oluşturma ve Azure Machine Learning veri kümeleri kayıt](how-to-create-register-datasets.md).  Veri kümeleri aracılığıyla daha fazla örnek için bkz. [örnek not defterleri](https://aka.ms/dataset-tutorial).

A **veri deposu** Azure depolama hesabınız depolama soyutlamadır. Veri deposu Azure blob kapsayıcısı veya bir Azure dosya paylaşımı, arka uç depolama kullanabilirsiniz. Varsayılan veri deposu her çalışma alanına sahiptir ve ek veri depoları kaydedebilirsiniz. Python SDK API'si veya Azure Machine Learning CLI depolamak ve deposundan dosyaları almak için kullanın.

### <a name="compute-targets"></a>Hedef işlem

A [hedef işlem](concept-compute-target.md) çalıştırdığınız eğitim betiği veya ana bilgisayar hizmeti dağıtımınız işlem kaynağı belirtmenize olanak sağlar. Bu konum, yerel makinenizde veya bir bulut tabanlı işlem kaynağı olabilir. İşlem hedefleri, bilgi işlem ortamınızı kodunuzu değiştirmeden değiştirmek kolaylaştırır. 

Daha fazla bilgi edinin [eğitim ve dağıtım için kullanılabilir işlem hedefleri](concept-compute-target.md). 

### <a name="training-scripts"></a>Eğitim betikleriniz

Bir modeli eğitmek için eğitim betiğini ve ilişkili dosyaları içeren dizini belirtin. Siz de eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deney adı belirtin. Eğitimi sırasında tüm dizinde eğitim ortama (işlem hedefi) kopyalanır ve çalışma yapılandırması tarafından belirtilen kodun başlatılır. Dizinin bir anlık görüntü, ayrıca çalışma alanında denemeyi altında depolanır.

Bir örnek için bkz [Öğreticisi: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

### <a name="runs"></a>Çalıştırmalar

Aşağıdaki bilgileri içeren bir kaydı bir çalıştırmadır:

* Çalıştırma (zaman damgası, süresi ve benzeri) hakkındaki meta verileri
* Betiğinizi tarafından günlüğe kaydedilen ölçümleri
* Sizin tarafınızdan autocollected denemeyi tarafından veya açıkça karşıya çıktı dosyaları
* Bir anlık görüntüsünü çalıştırma önce komut dosyalarınızı içeren dizine

Bir modeli eğitmek için bir betik gönderdiğinizde çalıştırma üretir. Bir çalıştırma, sıfır veya daha fazla alt çalıştırma olabilir. Örneğin, üst düzey çalışma, her biri kendi alt çalıştırma olabilir, iki alt çalıştırma olabilir.

Bir model eğitip geleceği üretilen çalıştırmalarını görüntüleme ilişkin bir örnek için bkz [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-run-cloud-notebook.md).

### <a name="github-tracking-and-integration"></a>GitHub izleme ve tümleştirme

Kaynak dizini yerel bir Git deposu olduğu çalıştırma eğitim başlattığınızda, depo bilgilerini çalıştırma geçmişinde depolanır. Örneğin, geçerli işleme kimliği depo için geçmiş bir parçası olarak günlüğe kaydedilir. Bu tahmin, ML işlem hattı ya da komut dosyasını çalıştır kullanılarak gönderilen çalıştırmaları ile çalışır. Ayrıca SDK ya da Machine Learning CLI gönderilen çalıştırmalar için çalışır.

### <a name="snapshots"></a>Anlık Görüntüler

Bir farklı çalıştır gönderdiğinizde, Azure Machine Learning komut dosyasına bir zip dosyası olarak içeren ve işlem hedefe gönderir dizini sıkıştırır. Zip dosyası ardından ayıklanır ve komut dosyası var. çalıştırın. Azure Machine Learning, zip dosyası da çalıştırma kaydı bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydı göz atabilir ve anlık görüntü indirin.

> [!NOTE]
> Gereksiz dosyaları anlık eklenmesini önlemek için yoksayma dosyası (.gitignore veya .amlignore) olun. Bu dosya anlık görüntü dizinine yerleştirin ve bunu yoksaymak için dosya adlarını ekleyin. Aynı .amlignore dosyasını kullanan [söz dizimi ve desenler .gitignore dosyası olarak](https://git-scm.com/docs/gitignore). Her iki dosya varsa, .amlignore dosya önceliklidir.

### <a name="activities"></a>Etkinlikler

Bir etkinlik, uzun süre çalışan bir işlemi temsil eder. Aşağıdaki işlemleri etkinlikleri örnekleri şunlardır:

* Oluşturma ya da işlem hedefi silme
* Bir işlem hedefte betik çalıştırma

Bu işlemlerin ilerleme durumunu kolayca izleyebilir böylece etkinlikleri SDK veya web kullanıcı Arabirimi aracılığıyla bildirimleri sağlar.

### <a name="images"></a>Görüntüler

Görüntüleri modelini kullanmak için gereken tüm bileşenleri ile birlikte bir model güvenilir bir şekilde dağıtmak için bir yol sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Bir model.
* Bir Puanlama komut dosyası veya uygulama. Giriş modeline geçirin ve model çıkışı döndürmek için komut dosyasını kullanın.
* Model veya Puanlama betik veya uygulama tarafından gereken bağımlılıklar. Örneğin, Python Paket bağımlılıklarını listeleyen bir Conda ortam dosyası içerebilir.

Azure Machine Learning iki tür görüntü oluşturabilirsiniz:

* **FPGA görüntü**: Azure'da bir alanda programlanabilen geçit dizileri dağıttığınızda kullanılır.
* **Docker görüntüsü**: FPGA dışındaki hedef işlem dağıttığınızda kullanılır. Azure Container Instances ve Azure Kubernetes Service verilebilir.

Azure Machine Learning hizmeti varsayılan olarak kullanılan bir temel görüntü sağlar. Kendi özel görüntülerinizi de sağlayabilirsiniz.

### <a name="image-registry"></a>Görüntü kayıt defteri

Görüntüleri katalog içinde **görüntü kayıt** çalışma alanınızdaki. Görüntü oluşturduğunuzda, böylece görüntünüzü bulabilmek için bunları sorgulayabilirsiniz ek meta veri etiketleri sağlayabilir.

Görüntü oluşturma örneği için bkz: [Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

Özel bir görüntü kullanarak bir model dağıtımına ilişkin bir örnek için bkz [özel Docker görüntüsü kullanarak bir model dağıtma](how-to-deploy-custom-docker-image.md).

### <a name="deployment"></a>Dağıtım

Bulutta barındırılan bir web hizmeti ya da veya bir IOT modülü tümleşik cihaz dağıtımları için model örneklemesi dağıtımıdır.

#### <a name="web-service-deployments"></a>Web hizmeti dağıtımları

Azure Container Instances, Azure Kubernetes hizmeti veya FPGA dağıtılmış bir web hizmetini kullanabilirsiniz. Hizmet, model, betik ve ilişkili dosyalar oluşturun. Bu web hizmeti için çalışma zamanı ortamı sağlayan bir görüntü içinde kapsüllenir. Görüntü, web hizmetine gönderilen Puanlama istekleri alan yük dengeli bir HTTP uç vardır.

Azure Application Insights telemetrisini veya model telemetri, bu özelliği etkinleştirmek seçtiyseniz toplayarak web hizmeti dağıtımınızı izlemenize yardımcı olur. Telemetri verilerini yalnızca, erişilebilir ve depolama hesabı örnekleri ve Application Insights içinde depolanır.

Otomatik ölçeklendirme etkinleştirdiyseniz, Azure, dağıtımınızı otomatik olarak ölçeklendirir.

Bir web hizmeti olarak bir model dağıtımına ilişkin bir örnek için bkz [Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-deployments"></a>IOT modülü dağıtımı

Dağıtılan bir IOT modülü modeliniz ve ilişkili komut dosyası veya uygulama ve herhangi ek bağımlılıklar içeren bir Docker kapsayıcıdır. Bu modüller, uç cihazlarda Azure IOT Edge'i kullanarak dağıtın.

İzleme etkinleştirdiyseniz, Azure, Azure IOT Edge modülü içinde modelinden telemetri verileri toplar. Telemetri verilerini yalnızca, erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IOT Edge, modülünü çalıştıran ve onu barındıran cihaz izler sağlar.

### <a name="ml-pipelines"></a>ML işlem hatları

Makine öğrenmesini kullanma oluşturup birleştirmek iş akışları yönetmek için işlem hatları machine learning aşamaları. Örneğin, bir işlem hattı, veri hazırlama, model eğitiminin, model dağıtımı ve çıkarım ve puanlama aşamaları içerebilir. Her aşamada, her biri çeşitli işlem hedeflerini katılımsız çalışabilir, birden çok adım kapsayabilir.

Machine learning işlem hatlarını bu hizmeti hakkında daha fazla bilgi için bkz: [işlem hatları ve Azure Machine Learning](concept-ml-pipelines.md).

### <a name="logging"></a>Günlüğe kaydetme

Çözümünüzü geliştirirken, Azure Machine Learning Python SDK'sı Python betiğinizde rastgele ölçümleri oturum için kullanın. Sonrasında çalıştırma, çalıştırmayı dağıtmak istediğiniz model üretilen olup olmadığını belirlemek için ölçümleri sorgulayın.

### <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti ile çalışmaya başlamak için bkz:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md)
* [Öğretici (Kısım 1): Bir model eğitip](tutorial-train-models-with-aml.md)
