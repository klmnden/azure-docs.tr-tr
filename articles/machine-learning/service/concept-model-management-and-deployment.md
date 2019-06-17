---
title: 'MLOps: Yönetme, dağıtma ve izleme ML modelleri'
titleSuffix: Azure Machine Learning service
description: 'Azure Machine Learning hizmeti için MLOps kullanmayı öğrenin: dağıtma, yönetme ve izleme Modellerinizi sürekli olarak iyileştirin. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 0eaf48f57c3011222b71a63d703e1ccec7aca001
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66692826"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, Modellerinizi ömrünü yönetmek için Azure Machine Learning hizmetini kullanma hakkında bilgi edinin. Azure Machine Learning, kalite ve makine öğrenimi çözümleri tutarlılığını artırır bir makine öğrenimi işlemleri (MLOps) yaklaşımı kullanır. Azure Machine Learning hizmeti aşağıdaki MLOps özellikleri sağlar:

* Azure işlem hatları ile tümleştirme. Sürekli tümleştirme ve dağıtım iş akışları için Modellerinizi tanımlayın.
* Eğitilen Modellerinizi birden çok sürümünü tutan bir model kayıt defteri.
* Model doğrulaması. Otomatik olarak eğitilen Modellerinizi doğrulamak ve onları üretim ortamına dağıtmak için en uygun yapılandırmayı seçin.
* Yerel olarak veya IOT Edge cihazları bulutta bir web hizmeti olarak Modellerinizi dağıtın.
* Model bir sonraki sürümünde iyileştirmeleri yönlendirebilirsiniz şekilde dağıtılan modelinizin performansı izleyin.

MLOps ve bunların Azure Machine Learning hizmeti ile nasıl uygulama kavramları hakkında daha fazla dinlemek için şu videoyu izleyin.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="integration-with-azure-pipelines"></a>Azure işlem hatları ile tümleştirme

Azure işlem hatları bir modeli eğitir bir sürekli tümleştirme işlem oluşturmak için kullanabilirsiniz. Bir veri Bilimcisi Git deposu için bir proje içinde bir değişikliği iade ederken tipik bir senaryoda, Azure işlem hattı bir eğitim çalıştırma başlatın. Eğitilen modelin performans özelliklerini görmek için çalıştırma sonuçlarını sonra inceledi. Ayrıca, bir web hizmeti olarak modeli dağıtan bir işlem hattı oluşturabilirsiniz.

[Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) Azure işlem hatları ile çalışmayı kolaylaştırır. Bu, Azure işlem hatlarına aşağıdaki geliştirmeleri sağlar:

* Çalışma alanı seçimi, bir hizmet bağlantısı tanımlarken sağlar.
* Etkinleştirir, işlem hatlarına eğitim hattında oluşturulmuş eğitilen modelleri tarafından harekete bırakın.

Azure işlem hatları ile Azure Machine Learning kullanma hakkında daha fazla bilgi için bkz. [sürekli tümleştirme ve dağıtım ML modelleri Azure işlem hatları ile](/azure/devops/pipelines/targets/azure-machine-learning) makale ve [Azure Machine Learning hizmeti MLOps](https://aka.ms/mlops) depo.

## <a name="convert-and-optimize-models"></a>Dönüştürme ve modelleri en iyi duruma getirme

Modelinize dönüştürme [açık sinir ağı Exchange](https://onnx.ai) (ONNX) performansı artırabilir. Ortalama olarak, dönüştürme için ONNX 2 x performans artışı sağlayabilir.

Azure Machine Learning hizmeti ile ONNX hakkında daha fazla bilgi için bkz: [oluştur ve ML modelleri hızlandırın](concept-onnx.md) makalesi.

## <a name="register-models"></a>Kayıt modelleri

Model kaydı, depolamanızı ve sürüm Modellerinizi çalışma alanınızda Azure bulutunda sağlar. Model kayıt defterini düzenlemek ve eğitilen Modellerinizi izlemek kolaylaştırır.

> [!TIP]
> Azure Machine Learning hizmetinin dışında eğitilen modelleri de kaydedebilirsiniz.
 
Kayıtlı modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz. Azure Machine Learning hizmeti yüklenen Python 3.5.2 kullanarak ya da daha yüksek olabilir herhangi bir modelini destekler.

Etkin bir dağıtımda kullanılmayan modelleri nelze odstranit.

Daha fazla bilgi için kayıt modeli bölümüne bakın. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel).

Bir model pickle biçiminde depolanan kaydetme ilişkin bir örnek için bkz [Öğreticisi: Bir görüntü sınıflandırma modeli eğitme](tutorial-deploy-models-with-aml.md).

## <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir model üretim ortamına dağıtmadan önce bir Docker görüntüsü halinde paketlenir. Çoğu durumda, görüntü oluşturmayı, arka planda otomatik olarak dağıtım sırasında gerçekleşir. Gelişmiş senaryolar için görüntü el ile belirtebilirsiniz.

Dağıtım sorunlarla karşılaşırsanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtabilirsiniz.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md#registermodel) ve [dağıtımıyla ilgili sorunları giderme](how-to-troubleshoot-deployment.md).

## <a name="validate-and-profile-models"></a>Modelleri profili ve doğrulama

Azure Machine Learning hizmeti, profil oluşturma modelinizi dağıtırken kullanmak için ideal CPU ve bellek ayarlarını belirlemek için kullanabilirsiniz. Model doğrulama için profil oluşturma işlemi, sağladığınız verileri kullanarak, bu işlemin bir parçası olarak gerçekleşir.

## <a name="use-models"></a>Modelleri kullanma

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

### <a name="web-service"></a>Web hizmeti

Modellerinizi içinde kullanabileceğiniz **web Hizmetleri** hedefleri ile aşağıdaki işlem:

* Azure Container Örneği
* Azure Kubernetes Service
* Yerel geliştirme ortamı

Bir web hizmeti olarak modeli dağıtacağız için aşağıdaki öğeleri belirtmeniz gerekir:

* Model veya modellerin topluluğu.
* Model kullanmak için gerekli bağımlılıkları. Örneğin, istekleri kabul eder ve çağıran conda bağımlılıklarını, modeli bir betik vb.
* Nasıl ve nerede açıklayan bir dağıtım yapılandırması model dağıtma.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>IOT Edge cihazları


Modeller IOT cihazları ile kullanabileceğiniz **Azure IOT Edge modülleri**. IOT Edge modülleri, çıkarım ya da, cihazda Puanlama modeli sağlayan bir donanım aygıtı için dağıtılır.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, verileri analiz için makine öğrenimi modelleri kullanarak destekler. Daha fazla bilgi için [Power BI (Önizleme) Azure Machine Learning tümleştirme](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="monitor-and-collect-data"></a>İzleme ve veri toplama

İzleme, hangi veri modeliniz ve döndürdüğü Öngörüler gönderildiğini anlamanıza olanak tanır.

Bu bilgiler, modelinizi nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş veri modelinin eğitim gelecekteki sürümlerinde yararlı olabilir.

Daha fazla bilgi için [model verileri toplamayı etkinleştirme](how-to-enable-data-collection.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile. Dağıtım örneği için bkz: [Öğreticisi: Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

Oluşturmayı [sürekli tümleştirme ve dağıtım ML modelleri Azure işlem hatları ile](/azure/devops/pipelines/targets/azure-machine-learning). 

İstemci uygulamaları oluşturmayı öğrenin ve Hizmetleri [bir web hizmeti olarak modeli kullanma](how-to-consume-web-service.md).
