---
title: 'MLOps: Yönetme, dağıtma ve izleme ML modelleri'
titleSuffix: Azure Machine Learning service
description: 'Azure Machine Learning hizmeti için MLOps kullanmayı öğrenin: dağıtma, yönetme ve izleme Modellerinizi sürekli olarak iyileştirin. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 11a4a17d7816d2302b6549cffb9517e10ad1258d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442340"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, Modellerinizi ömrünü yönetmek için Azure Machine Learning hizmetini kullanma hakkında bilgi edinin. Azure Machine Learning, kalite ve makine öğrenimi çözümleri tutarlılığını artırır bir makine öğrenimi işlemleri (MLOps) yaklaşımı kullanır. 

Azure Machine Learning hizmeti aşağıdaki MLOps özellikleri sağlar:

- **Her yerden ML projeleri dağıtma**
- **ML uygulamaları için izleme işletimsel ve ML ile ilgili sorunlar** - model girişlerini eğitim ve çıkarım arasında karşılaştırma, model özgü ölçümleri keşfedin ve ML altyapınızı izleme ve uyarılar sağlar.
- **ML yaşam döngüsünün bir uçtan uca denetim kaydı oluşturmak için gerekli verileri yakalama**modelleri yayımlama neden değişiklikler yapılmıştır ve modelleri dağıtılan veya üretim ortamında kullanılan zaman dahil olmak üzere.
- **Azure Machine Learning ve Azure DevOps ile uçtan uca ML yaşam döngüsünü otomatikleştirin** sık modelleri güncelleştirmek için yeni modeller test ve sürekli olarak yeni ML modelleri, diğer uygulama ve hizmetlerin yanı sıra kullanıma alma.

MLOps ve bunların Azure Machine Learning hizmeti ile nasıl uygulama kavramları hakkında daha fazla dinlemek için şu videoyu izleyin.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="deploy-ml-projects-from-anywhere"></a>Her yerden ML projeleri dağıtma

### <a name="turn-your-training-process-into-a-reproducible-pipeline"></a>Eğitim işleminizi yeniden üretilebilen bir işlem hattı'nda Aç
Azure Machine learning'in ML işlem hatları birleştirmek için kullanın tüm hiper parametre modeli değerlendirmeye ayarlama için veri hazırlama özelliği ayıklama için model eğitim işlemindeki adımlar.

Daha fazla bilgi için [ML işlem hatları](concept-ml-pipelines.md).

### <a name="register-and-track-ml-models"></a>Kaydolun ve ML modelleri izleyin

Model kaydı, depolamanızı ve sürüm Modellerinizi çalışma alanınızda Azure bulutunda sağlar. Model kayıt defterini düzenlemek ve eğitilen Modellerinizi izlemek kolaylaştırır.

> [!TIP]
> Kayıtlı bir model, modeli bir veya daha fazla dosyaların mantıksal bir kapsayıcıdır. Birden çok dosyasında depolanan bir model varsa, örneğin, bunları tek bir model Azure Machine Learning çalışma alanınızda kaydedebilirsiniz. Kayıt sonrasında sonra indirin veya kayıtlı modeli dağıtabilir ve kaydedilmiş tüm dosyalar alırsınız.
 
Kayıtlı modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz. Azure Machine Learning hizmeti yüklenen Python 3.5.2 kullanarak ya da daha yüksek olabilir herhangi bir modelini destekler.

> [!TIP]
> Azure Machine Learning hizmetinin dışında eğitilen modelleri de kaydedebilirsiniz.

Etkin bir dağıtımında kullanılan kayıtlı bir modeli silinemiyor.
Daha fazla bilgi için kayıt modeli bölümüne bakın. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel).

### <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir model üretim ortamına dağıtmadan önce bir Docker görüntüsü halinde paketlenir. Çoğu durumda, görüntü oluşturmayı, arka planda otomatik olarak dağıtım sırasında gerçekleşir. Gelişmiş senaryolar için görüntü el ile belirtebilirsiniz.

Dağıtım sorunlarla karşılaşırsanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtabilirsiniz.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md#registermodel) ve [dağıtımıyla ilgili sorunları giderme](how-to-troubleshoot-deployment.md).

### <a name="validate-and-profile-models"></a>Modelleri profili ve doğrulama

Azure Machine Learning hizmeti, profil oluşturma modelinizi dağıtırken kullanmak için ideal CPU ve bellek ayarlarını belirlemek için kullanabilirsiniz. Model doğrulama için profil oluşturma işlemi, sağladığınız verileri kullanarak, bu işlemin bir parçası olarak gerçekleşir.

### <a name="convert-and-optimize-models"></a>Dönüştürme ve modelleri en iyi duruma getirme

Modelinize dönüştürme [açık sinir ağı Exchange](https://onnx.ai) (ONNX) performansı artırabilir. Ortalama olarak, dönüştürme için ONNX 2 x performans artışı sağlayabilir.

Azure Machine Learning hizmeti ile ONNX hakkında daha fazla bilgi için bkz: [oluştur ve ML modelleri hızlandırın](concept-onnx.md) makalesi.

### <a name="use-models"></a>Modelleri kullanma

Eğitilen makine öğrenimi modellerini web hizmetleri bulutta veya yerel olarak geliştirme ortamınıza bağlı olarak dağıtılabilir. Ayrıca, modellerini Azure IOT Edge cihazlarına dağıtabilirsiniz. Dağıtımlar için çıkarım CPU, GPU ve alanda programlanabilen geçit dizileri (FPGA) kullanabilirsiniz. Power bı'dan modelleri de kullanabilirsiniz.

Bir modeli bir web hizmeti veya IOT Edge cihaz olarak kullanırken, aşağıdakileri sağlar:

* Hizmet/cihaza gönderilen verileri puanlamak için kullanılan bir model.
* Bir giriş betiğine girildi. Bu betik, istekleri kabul eder, verilerinizi puanlamada ve bir yanıt döndüreceğini modellerini kullanır.
* Modellere ve giriş komut dosyası tarafından gerekli olan bağımlılıkları açıklayarak conda ortam dosyası.
* Modellere ve giriş komut dosyası için gereken tüm ek varlıkları metin, veri, vb. gibi.

Bu varlıklar bir Docker görüntüsü halinde paketlenir ve bir web hizmeti veya IOT Edge modülü olarak dağıtılabilir.

İsteğe bağlı olarak, daha fazla dağıtım ayarlamak için aşağıdaki parametreleri kullanabilirsiniz:

* GPU etkinleştir: Docker görüntüsünü GPU desteğini etkinleştirmek için kullanılır. Görüntüyü Azure Container Instances, Azure Kubernetes hizmeti, Azure Machine Learning işlem veya Azure sanal makineler gibi Microsoft Azure Hizmetleri kullanılmalıdır.
* Ek bir docker dosyası adımlar: Docker görüntüsü oluşturulurken çalıştırmak için ek Docker adımları içeren bir dosya.
* Temel görüntü: Temel görüntü olarak kullanılacak özel bir görüntü. Özel görüntü kullanma, temel görüntü Azure Machine Learning hizmeti tarafından sağlanır.

Ayrıca, hedef dağıtım platform yapılandırmasını de sağlar. Örneğin, VM ailesi türünün kullanılabilir bellek ve Azure Kubernetes Service'e dağıtırken çekirdek sayısı.

Görüntü oluşturulduğunda, Azure Machine Learning hizmeti tarafından gerekli bileşenleri de eklenir. Örneğin, web hizmetini çalıştırmak ve IOT Edge ile etkileşim kurmak için gereken varlıklar.

> [!NOTE]
> Değiştiremez veya web sunucusu veya Docker görüntüsünü kullanılan IOT Edge bileşenleri değiştirin. Azure Machine Learning hizmeti web sunucusu yapılandırma ve test edilmiş ve Microsoft tarafından desteklenen IOT Edge bileşenlerini kullanır.

#### <a name="web-service"></a>Web hizmeti

Modellerinizi içinde kullanabileceğiniz **web Hizmetleri** hedefleri ile aşağıdaki işlem:

* Azure Container Örneği
* Azure Kubernetes Service
* Yerel geliştirme ortamı

Bir web hizmeti olarak modeli dağıtacağız için aşağıdaki öğeleri belirtmeniz gerekir:

* Model veya modellerin topluluğu.
* Model kullanmak için gerekli bağımlılıkları. Örneğin, istekleri kabul eder ve çağıran conda bağımlılıklarını, modeli bir betik vb.
* Nasıl ve nerede açıklayan bir dağıtım yapılandırması model dağıtma.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

#### <a name="iot-edge-devices"></a>IOT Edge cihazları

Modeller IOT cihazları ile kullanabileceğiniz **Azure IOT Edge modülleri**. IOT Edge modülleri, çıkarım ya da, cihazda Puanlama modeli sağlayan bir donanım aygıtı için dağıtılır.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, verileri analiz için makine öğrenimi modelleri kullanarak destekler. Daha fazla bilgi için [Power BI (Önizleme) Azure Machine Learning tümleştirme](https://docs.microsoft.com/power-bi/service-machine-learning-integration).


## <a name="monitor-ml-applications-for-operational-and-ml-related-issues"></a>ML uygulamaları için izleme işletimsel ve ML ile ilgili sorunlar

İzleme, hangi veri modeliniz ve döndürdüğü Öngörüler gönderildiğini anlamanıza olanak tanır.

Bu bilgiler, modelinizi nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş veri modelinin eğitim gelecekteki sürümlerinde yararlı olabilir.

Daha fazla bilgi için [model verileri toplamayı etkinleştirme](how-to-enable-data-collection.md).


## <a name="capture-an-end-to-end-audit-trail-of-the-ml-lifecycle"></a>ML yaşam döngüsünün bir uçtan uca denetim kaydı yakalama

Azure ML, ML varlıklarınızı tüm uçtan uca denetim izi izleme olanağı sağlar. Bu avantajlar şunlardır:

- Azure ML, depoyu bilgileri izlemek / dal / tamamlama, kod gelen gelen Git ile tümleştirilir.
- Azure ML veri kümeleri Yardım izlemenize ve sürüm verileri.
- Azure ML çalıştırma geçmişi, kod, veri ve bir modeli eğitmek için kullanılan işlem yönetir.
- Azure ML Model kayıt defteri modelinizi (dağıtımları sağlıklı olduğunu, hangi deney, burada dağıtılacağı, eğitilmiş) ile ilişkili meta verilerin tümünün yakalar.

## <a name="automate-the-end-to-end-ml-lifecycle"></a>Uçtan uca ML yaşam döngüsünü otomatikleştirin 

Bir modeli eğitir bir sürekli tümleştirme işlem oluşturmak için Azure işlem hatlarını ve GitHub'ı kullanabilirsiniz. Bir veri Bilimcisi Git deposu için bir proje içinde bir değişikliği iade ederken tipik bir senaryoda, Azure işlem hattı bir eğitim çalıştırma başlatın. Eğitilen modelin performans özelliklerini görmek için çalıştırma sonuçlarını sonra inceledi. Ayrıca, bir web hizmeti olarak modeli dağıtan bir işlem hattı oluşturabilirsiniz.

[Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) Azure işlem hatları ile çalışmayı kolaylaştırır. Bu, Azure işlem hatlarına aşağıdaki geliştirmeleri sağlar:

* Çalışma alanı seçimi, bir hizmet bağlantısı tanımlarken sağlar.
* Etkinleştirir, işlem hatlarına eğitim hattında oluşturulmuş eğitilen modelleri tarafından harekete bırakın.

Azure işlem hatları ile Azure Machine Learning kullanma hakkında daha fazla bilgi için bkz. [sürekli tümleştirme ve dağıtım ML modelleri Azure işlem hatları ile](/azure/devops/pipelines/targets/azure-machine-learning) makale ve [Azure Machine Learning hizmeti MLOps](https://aka.ms/mlops) depo.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile. Dağıtım örneği için bkz: [Öğreticisi: Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

Oluşturmayı [sürekli tümleştirme ve dağıtım ML modelleri Azure işlem hatları ile](/azure/devops/pipelines/targets/azure-machine-learning). 

İstemci uygulamaları oluşturmayı öğrenin ve Hizmetleri [bir web hizmeti olarak modeli kullanma](how-to-consume-web-service.md).
