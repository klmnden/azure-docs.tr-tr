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
ms.openlocfilehash: cb716e0d9f97d3ea2e9584a9fc3d7a6f57da9179
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65502089"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Azure Machine Learning hizmetinin nasıl çalıştığı: Mimari ve kavramları

Mimari, kavramlar ve Azure Machine Learning hizmeti için iş akışı hakkında bilgi edinin. Ana bileşenleri ve hizmet bu hizmeti kullanmak için genel iş akışı aşağıdaki diyagramda gösterilmiştir:

[![Azure Machine Learning hizmeti mimarisi ve iş akışı](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

## <a name="workflow"></a>İş Akışı

Machine learning iş akışı genellikle bu sırayı takip eder:

1. Makine öğrenimi betiklerini Eğitim geliştirme **Python**.
1. Oluşturma ve yapılandırma bir **hedef işlem**.
1. **Komut Gönderme** ortamında çalıştırmak için yapılandırılmış işlem hedefine. Eğitim sırasında komut dosyaları okuyabilir ya da yazma **veri deposu**. Ve olarak yürütme kayıtlarını kaydedilir **çalıştıran** içinde **çalışma** ve altında gruplandırılmış **denemeleri**.
1. **Denemeyi sorgu** için ölçümleri geçerli ve geçmiş çalıştırmalardan oturum. Ölçümler istenilen bir sonucu göstermediği, geri adım 1 ve betiklerinizi üzerinde yineleme döngüsü.
1. Tatmin edici bir çalıştırma bulunduktan sonra kalıcı modelde kaydetme **modeli kayıt defteri**.
1. Geliştirme modelini kullanan Puanlama betiğine ve **model dağıtma** olarak bir **web hizmetini** azure'da yenilemek için bir **IOT Edge cihazı**.


> [!NOTE]
> Hüküm ve Azure Machine Learning hizmeti tarafından kullanılan kavramları bu makalede tanımlasa da terimleri ve kavramları Azure platformu tanımlamıyor. Azure platformu hakkında daha fazla bilgi için bkz. [Microsoft Azure sözlüğünü](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Çalışma alanı

Çalışma alanı, Azure Machine Learning hizmeti için en üst düzey kaynaktır. Azure Machine Learning hizmeti kullanırken oluşturduğunuz tüm yapıları ile çalışma için merkezi bir yerdir.

Çalışma alanı, modeli eğitmek için kullanabileceğiniz işlem hedefleri listesini tutar. Ayrıca, günlükler, ölçümler, çıkış ve komut dosyalarınızın anlık görüntüsünü de dahil olmak üzere, bir eğitim çalıştırmalarının geçmişini tutar. Hangi eğitim çalıştırmanın en iyi modeli belirlemek için bu bilgileri kullanın.

Model çalışma alanı ile kaydedin. Azure Container Instances, Azure Kubernetes hizmeti veya bir REST tabanlı bir HTTP uç noktası olarak bir alanda programlanabilir kapı dizileri (FPGA) için model dağıtma için kayıtlı bir model ve puanlama komut dosyaları'nı kullanın. Ayrıca, görüntüyü Azure IOT Edge cihazına bir modül olarak dağıtabilirsiniz. Dahili olarak, dağıtılan görüntüye barındırmak için bir docker görüntüsü oluşturulur. Gerekirse, kendi görüntünüzü belirtebilirsiniz.

Her bir çalışma alanı birden çok kişi tarafından paylaşılabilir ve birden çok çalışma alanı oluşturabilirsiniz. Bir çalışma alanı paylaştığınızda, kullanıcılar için aşağıdaki rolleri atayarak erişim iznini kontrol edebilirsiniz:

* Sahibi
* Katılımcı
* Okuyucu

Bu roller hakkında daha fazla bilgi için bkz. [yönetmek için bir Azure Machine Learning çalışma alanına erişim](how-to-assign-roles.md) makalesi.

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından kullanılan bazı Azure kaynakları otomatik olarak oluşturur:

* [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/): Eğitim sırasında ve bir modeli dağıtırken kullandığınız docker kapsayıcıları kaydeder.
* [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): Çalışma alanı için varsayılan veri deposu olarak kullanılır.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): İzleme, modelleri hakkında bilgi depolar.
* [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/): Hedefleri ve gerekli olan diğer hassas bilgiler depolar gizli dizileri tarafından kullanılan işlem tarafından çalışma alanı.

> [!NOTE]
> Yeni sürümler oluşturmaya ek olarak, var olan Azure hizmetleri de kullanabilirsiniz.

Çalışma alanının bir taksonomi, aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="experiment"></a>Deneme

Bir deney, belirtilen betiği çok sayıda çalıştırmalardan gruplandırmasıdır. Her zaman, bir çalışma alanına aittir. Bir farklı çalıştır gönderdiğinizde bir deney adı sağlayın. Çalıştırma için bilgileri, bu deneme altında depolanır. Bir farklı çalıştır gönderin ve mevcut olmayan bir deney adı belirtin, bu yeni belirtilen ada sahip yeni bir deneme otomatik olarak oluşturulur.

Bir deney kullanma örneği için bkz. [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-run-cloud-notebook.md).

## <a name="model"></a>Model

En basit haliyle bir girdi alır ve çıktıyı üretir kod parçasını modelidir. Makine öğrenme modeli oluşturma, bir algoritma seçme, verilerle sağlama ve ayarlama hiperparametreleri içerir. Eğitim eğitim işlemi sırasında model öğrendikleriniz yalıtan eğitilen bir modelin üreten yinelemeli bir işlemdir.

Bir model, Azure Machine learning'de bir çalıştırma tarafından oluşturulur. Azure Machine Learning dışında eğitilmiş bir modeli de kullanabilirsiniz. Bir Azure Machine Learning hizmeti çalışma alanında bir model kaydedebilirsiniz.

Azure Machine Learning framework bağımsız bir hizmettir. Bir modeli oluşturduğunuzda, Scikit-öğrenme, XGBoost, PyTorch, TensorFlow ve bağlayıcı gibi tüm popüler makine öğrenmesi çerçeveleri kullanabilirsiniz.

Modeli ilişkin bir örnek için bkz [Öğreticisi: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

### <a name="model-registry"></a>Model kayıt defteri

Model kayıt defteri, Azure Machine Learning hizmeti çalışma alanınızdaki tüm modelleri izler.

Modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri yeni bir sürüm olduğunu varsayar. Sürümü artırılır ve yeni modeli aynı adla kaydedilir.

Modeli kaydettiğinizde, ek meta veri etiketleri sağlar ve modelleri için arama yaptığınızda etiketleri kullanın.

Etkin bir dağıtım tarafından kullanılan modelleri nelze odstranit.

Model kaydediliyor ilişkin bir örnek için bkz [bir Azure Machine Learning ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

## <a name="run-configuration"></a>Çalıştırma yapılandırma

Bir çalıştırma yapılandırma komut dosyası içinde belirtilen işlem hedefi nasıl çalıştırılması gerektiğini tanımlayan bir yönerge kümesidir. Yapılandırma çok sayıda bir Python ortamı kullanmak mı, yoksa bir belirtiminden yerleşik bir Conda ortamı kullanacağınızı gibi davranış tanımları içerir.

İçinde bir eğitim betiğini içeren dizine bir dosya içine bir çalıştırma yapılandırma kalıcı ya da bir bellek içi nesne olarak oluşturulur ve farklı çalıştır göndermek için kullanılır.

Çalışma yapılandırmaları, bkz: [seçin ve modelinizi eğitmek için bir işlem hedefine](how-to-set-up-training-targets.md).

## <a name="dataset"></a>Veri kümesi

Azure Machine Learning veri kümeleri (Önizleme) erişip ile çalışmanızı kolaylaştırır. Veri kümeleri, veri modeli eğitimi gibi çeşitli senaryolarda yönetin ve işlem hattı oluşturma. Azure Machine Learning SDK'sını kullanarak, temel alınan depolama alanına erişmek, keşfedin ve verileri hazırlama, farklı veri kümesi tanımları yaşam döngüsünü yönetme ve eğitim hem de üretim kullanılan veri kümeleri arasında karşılaştırma.

Veri kümelerini kullanma gibi popüler biçimlerde verilerle çalışmak için yöntemler sağlar `from_delimited_files()` veya `to_pandas_dataframe()`.

Daha fazla bilgi için [oluşturma ve Azure Machine Learning veri kümeleri kayıt](how-to-create-register-datasets.md).

Veri kümelerini kullanan bir örnek için bkz: [örnek not defterleri](https://aka.ms/dataset-tutorial).

## <a name="datastore"></a>Veri deposu

Bir veri deposu depolama soyutlama, bir Azure depolama hesabıdır. Veri deposu Azure blob kapsayıcısı veya bir Azure dosya paylaşımı, arka uç depolama kullanabilirsiniz. Varsayılan veri deposu her çalışma alanına sahiptir ve ek veri depoları kaydedebilirsiniz.

Python SDK API'si veya Azure Machine Learning CLI depolamak ve deposundan dosyaları almak için kullanın.

## <a name="compute-target"></a>Hedef işlem

İşlem hedefi eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullandığınız işlem kaynağıdır. Desteklenen işlem hedefleri şunlardır:

| Hedef işlem | Eğitim | Dağıtım |
| ---- |:----:|:----:|
| Yerel bilgisayarınıza | ✓ | &nbsp; |
| Azure Machine Learning işlem | ✓ | &nbsp; |
| Azure'da bir Linux sanal makinesi</br>(veri bilimi sanal makinesi gibi) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; |
| Azure Data Lake Analytics | ✓ | &nbsp; |
| HDInsight için Apache Spark | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Alanda programlanabilir kapı dizileri (FPGA) | &nbsp; | ✓ |

İşlem hedefleri, bir çalışma alanına eklenir. İşlem yerel makine dışındaki hedefleri çalışma alanının kullanıcılar tarafından paylaşılır.

### <a name="managed-and-unmanaged-compute-targets"></a>Yönetilen ve yönetilmeyen işlem hedefleri

* **Yönetilen**: Oluşturulan ve Azure Machine Learning hizmeti tarafından yönetilen hedef işlem. Bu işlem, hedef makine öğrenimi iş yükleri için iyileştirilmiştir. Azure Machine Learning işlem olduğundan, yalnızca 4 Aralık 2018'den itibaren hedef yönetilen bilgi işlem. Ek yönetilen bir işlem hedefleri gelecekte eklenebilir.

    Machine learning oluşturabilirsiniz işlem örnekleri çalışma aracılığıyla doğrudan Azure portalı, Azure Machine Learning SDK veya Azure CLI kullanarak. Diğer tüm işlem hedefleri çalışma alanı dışında oluşturdunuz ve ona bağlı.

* **Yönetilmeyen**: Olan işlem hedefleri *değil* Azure Machine Learning hizmeti tarafından yönetilir. Bunları Azure Machine Learning dışında oluşturup kullanmadan önce çalışma alanınıza eklemek gerekebilir. Yönetilmeyen işlem hedefleri, korumak veya makine öğrenimi iş yükleri için performansı artırmak için ek adımlar gerektirebilir.

Eğitim için işlem hedef seçme hakkında daha fazla bilgi için bkz: [seçin ve modelinizi eğitmek için bir işlem hedefine](how-to-set-up-training-targets.md).

Dağıtım için bir işlem hedef seçme hakkında daha fazla bilgi için bkz: [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

## <a name="training-script"></a>Eğitim betiği

Bir modeli eğitmek için eğitim betiğini ve ilişkili dosyaları içeren dizini belirtin. Siz de eğitim sırasında toplanan bilgileri depolamak için kullanılan bir deney adı belirtin. Eğitimi sırasında tüm dizinde eğitim ortama (işlem hedefi) kopyalanır ve çalışma yapılandırması tarafından belirtilen kodun başlatılır. Dizinin bir anlık görüntü, ayrıca çalışma alanında denemeyi altında depolanır.

Bir örnek için bkz [Öğreticisi: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).

## <a name="run"></a>Çalıştır

Aşağıdaki bilgileri içeren bir kaydı bir çalıştırmadır:

* Çalıştırma (zaman damgası, süresi ve benzeri) hakkındaki meta verileri
* Betiğinizi tarafından günlüğe kaydedilen ölçümleri
* Sizin tarafınızdan autocollected denemeyi tarafından veya açıkça karşıya çıktı dosyaları
* Bir anlık görüntüsünü çalıştırma önce komut dosyalarınızı içeren dizine

Bir modeli eğitmek için bir betik gönderdiğinizde çalıştırma üretir. Bir çalıştırma, sıfır veya daha fazla alt çalıştırma olabilir. Örneğin, üst düzey çalışma, her biri kendi alt çalıştırma olabilir, iki alt çalıştırma olabilir.

Bir model eğitip geleceği üretilen çalıştırmalarını görüntüleme ilişkin bir örnek için bkz [hızlı başlangıç: Azure Machine Learning hizmeti ile çalışmaya başlama](quickstart-run-cloud-notebook.md).

## <a name="snapshot"></a>Anlık görüntü

Bir farklı çalıştır gönderdiğinizde, Azure Machine Learning komut dosyasına bir zip dosyası olarak içeren ve işlem hedefe gönderir dizini sıkıştırır. Zip dosyası ardından ayıklanır ve komut dosyası var. çalıştırın. Azure Machine Learning, zip dosyası da çalıştırma kaydı bir parçası olarak bir anlık görüntü olarak depolar. Çalışma alanına erişimi olan herkes bir çalıştırma kaydı göz atabilir ve anlık görüntü indirin.

## <a name="activity"></a>Etkinlik

Bir etkinlik, uzun süre çalışan bir işlemi temsil eder. Aşağıdaki işlemleri etkinlikleri örnekleri şunlardır:

* Oluşturma ya da işlem hedefi silme
* Bir işlem hedefte betik çalıştırma

Bu işlemlerin ilerleme durumunu kolayca izleyebilir böylece etkinlikleri SDK veya web kullanıcı Arabirimi aracılığıyla bildirimleri sağlar.

## <a name="image"></a>Image

Görüntüleri modelini kullanmak için gereken tüm bileşenleri ile birlikte bir model güvenilir bir şekilde dağıtmak için bir yol sağlar. Görüntü, aşağıdaki öğeleri içerir:

* Bir model.
* Bir Puanlama komut dosyası veya uygulama. Giriş modeline geçirin ve model çıkışı döndürmek için komut dosyasını kullanın.
* Model veya Puanlama betik veya uygulama tarafından gereken bağımlılıklar. Örneğin, Python Paket bağımlılıklarını listeleyen bir Conda ortam dosyası içerebilir.

Azure Machine Learning iki tür görüntü oluşturabilirsiniz:

* **FPGA görüntü**: Azure'da bir alanda programlanabilen geçit dizileri dağıttığınızda kullanılır.
* **Docker görüntüsü**: FPGA dışındaki hedef işlem dağıttığınızda kullanılır. Azure Container Instances ve Azure Kubernetes Service verilebilir.

Azure Machine Learning hizmeti varsayılan olarak kullanılan bir temel görüntü sağlar. Kendi özel görüntülerinizi de sağlayabilirsiniz.

Görüntü oluşturma örneği için bkz: [Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Görüntü kayıt defteri

Görüntü kayıt defteri, Modellerinizi oluşturulan görüntüleri izler. Görüntü oluşturduğunuzda, ek meta veri etiketleri sağlayabilirsiniz. Meta veri etiketleri görüntü kayıt depolanır ve görüntünüzü bulmak için bunları sorgulayabilirsiniz.

## <a name="deployment"></a>Dağıtım

Bulutta barındırılan bir web hizmeti ya da veya bir IOT modülü tümleşik cihaz dağıtımları için model örneklemesi dağıtımıdır.

### <a name="web-service"></a>Web hizmeti

Azure Container Instances, Azure Kubernetes hizmeti veya FPGA dağıtılmış bir web hizmetini kullanabilirsiniz. Hizmet, model, betik ve ilişkili dosyalar oluşturun. Bu web hizmeti için çalışma zamanı ortamı sağlayan bir görüntü içinde kapsüllenir. Görüntü, web hizmetine gönderilen Puanlama istekleri alan yük dengeli bir HTTP uç vardır.

Azure Application Insights telemetrisini veya model telemetri, bu özelliği etkinleştirmek seçtiyseniz toplayarak web hizmeti dağıtımınızı izlemenize yardımcı olur. Telemetri verilerini yalnızca, erişilebilir ve depolama hesabı örnekleri ve Application Insights içinde depolanır.

Otomatik ölçeklendirme etkinleştirdiyseniz, Azure, dağıtımınızı otomatik olarak ölçeklendirir.

Bir web hizmeti olarak bir model dağıtımına ilişkin bir örnek için bkz [Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>IOT Modülü

Dağıtılan bir IOT modülü modeliniz ve ilişkili komut dosyası veya uygulama ve herhangi ek bağımlılıklar içeren bir Docker kapsayıcıdır. Bu modüller, uç cihazlarda Azure IOT Edge'i kullanarak dağıtın.

İzleme etkinleştirdiyseniz, Azure, Azure IOT Edge modülü içinde modelinden telemetri verileri toplar. Telemetri verilerini yalnızca, erişilebilir ve depolama hesabı Örneğinizde depolanır.

Azure IOT Edge, modülünü çalıştıran ve onu barındıran cihaz izler sağlar.

## <a name="pipeline"></a>İşlem hattı

Makine öğrenmesini kullanma oluşturup birleştirmek iş akışları yönetmek için işlem hatları machine learning aşamaları. Örneğin, bir işlem hattı, veri hazırlama, model eğitiminin, model dağıtımı ve çıkarım aşamaları içerebilir. Her aşamada, her biri çeşitli işlem hedeflerini katılımsız çalışabilir, birden çok adım kapsayabilir.

Machine learning işlem hatlarını bu hizmeti hakkında daha fazla bilgi için bkz: [işlem hatları ve Azure Machine Learning](concept-ml-pipelines.md).

## <a name="logging"></a>Günlüğe kaydetme

Çözümünüzü geliştirirken, Azure Machine Learning Python SDK'sı Python betiğinizde rastgele ölçümleri oturum için kullanın. Sonrasında çalıştırma, çalıştırmayı dağıtmak istediğiniz model üretilen olup olmadığını belirlemek için ölçümleri sorgulayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti ile çalışmaya başlamak için bkz:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md)
* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
* [Resource Manager şablonu ile bir çalışma alanı oluşturma](how-to-create-workspace-template.md)
