---
title: Machine Learning Workbench'i ne oldu?
titleSuffix: Azure Machine Learning service
description: Machine Learning Workbench'i ne hakkında uygulama, Azure Machine Learning hizmetindeki değişiklikler ve Destek zaman çizelgesi ne olduğunu öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ff6b61874363bbc869bd509174e58640a2487f56
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811316"
---
# <a name="whats-happening-to-machine-learning-workbench-in-azure-machine-learning-service"></a>Machine Learning Workbench'i Azure Machine Learning hizmetinde neler oluyor?

Azure Machine Learning Workbench uygulamasını ve bazı diğer erken özellikler kullanım dışı ve geliştirilmiş bir yol sağlamak için Eylül 2018 sürümünden yerine [mimarisi](concept-azure-machine-learning-architecture.md). Deneyiminizi iyileştirmek için sürüm tarafından müşteri geri bildirimi istenir birçok önemli güncelleştirme içerir. Model dağıtımı için temel işlevleri deneme çalıştırmalardan değişmedi. Ancak artık sağlam kullanabilir <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> ve [Azure CLI](reference-azure-machine-learning-cli.md) , makine öğrenimi görevlerini ve işlem hatlarını gerçekleştirmek için.  

Bu makalede, değişiklikler ve Azure Machine Learning Workbench ve API'lerini önceden mevcut olan iş nasıl etkilediği hakkında bilgi edinin.

## <a name="what-changed"></a>Ne değişti?

Azure Machine Learning hizmetinin en son sürüm, aşağıdaki özellikleri içerir:
+ A [Basitleştirilmiş Azure kaynaklarını modeli](concept-azure-machine-learning-architecture.md).
+ A [yeni portalı kullanıcı arabirimini](how-to-track-experiments.md) denemelerinizi yönetmek ve hedef işlem.
+ Yeni, daha kapsamlı bir Python <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ Yeni Genişletilmiş [Azure CLI uzantısı](reference-azure-machine-learning-cli.md) machine learning için.

[Mimarisi](concept-azure-machine-learning-architecture.md) kullanım kolaylığı için tasarlanmıştır. Birden çok Azure kaynağı ve hesabı yerine, size gereken yalnızca bir [Azure Machine Learning hizmeti Çalışma Alanı](concept-azure-machine-learning-architecture.md#workspace)'dır. [Azure portalda](quickstart-get-started.md) hemen çalışma alanları oluşturabilirsiniz. Bir çalışma alanı kullanarak, birden çok kullanıcı eğitimi depolayabilir ve dağıtım işlem hedefleri, model denemeleri, Docker görüntülerini, dağıtılan modellerinde ve benzeri.

Geçerli sürümde geliştirilmiş yeni CLI ve SDK'sı istemciler olsa da, Masaüstü workbench uygulaması kullanım dışı bırakılmıştır. Denemelerinizi içinde izleyebilirsiniz artık [Azure portalında çalışma alanı Pano](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Deneme geçmişinizi almak, çalışma alanınıza bağlı işlem hedeflerini yönetmek, modellerinizi ve Docker görüntülerinizi yönetmek, hatta web hizmetlerini dağıtmak için panoyu kullanın.

## <a name="how-do-i-migrate"></a>Nasıl geçiş yaparım?

Azure Machine Learning hizmeti önceki sürümünde oluşturulmuştur yapıtları çoğunu, kendi yerel depolanır veya Bulut depolama. Bu yapıtlar hiçbir zaman kaybolmayacaktır. Geçiş için, yapıtları güncelleştirilmiş Azure Machine Learning hizmetine yeniden kaydetmeniz gerekir. Neyi nasıl geçirebileceğinizi bu [geçiş makalesinde](how-to-migrate.md) öğrenebilirsiniz.

<a name="timeline"></a>

## <a name="support-timeline"></a>Destek zaman çizelgesi

Machine Learning denemesi ve Model Yönetimi hesaplarınızı ve Eylül 2018'den sonra Machine Learning Workbench uygulamasını kullanmaya devam edebilirsiniz. Aşağıdaki kaynaklar için destek indirildiğinde üç ila dört, yayımlandıktan sonra ay içinde kaldırılacak. İçindekiler’in alt tarafındaki [Kaynaklar bölümünde](../desktop-workbench/tutorial-classifying-iris-part-1.md) eski özelliklere ilişkin belgelere ulaşmaya devam edebilirsiniz.

|Kullanımdan kaldırma&nbsp;aşaması|Daha eski özellikler için destek ayrıntıları|
|:---:|----------------|
|4 Aralık 2018'e|Azure portalında ve clı'dan Azure Machine Learning denemesi ve Model Yönetimi hesapları oluşturma olanağına sona erdi. Machine Learning işlem ortamları CLI'dan oluşturma olanağı da sona erdi. Bir hesabınız varsa, CLI ve Machine Learning Workbench Masaüstü bu aşamada çalışmaya devam.|
|9 Ocak 2019|Diğer her şey için destek, bu tarihte sona erer. Kalan API ve Machine Learning Workbench Masaüstü verilebilir.|

[Geçişe hemen başlayın](how-to-migrate.md). Yeni kullanarak en son özellikleri tüm kullanılabilir <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, [CLI](reference-azure-machine-learning-cli.md)ve [portalı](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>Çalıştırma geçmişleri ne olacak?

Çalıştırma geçmişleri biraz erişilebilir. Azure Machine Learning hizmeti güncelleştirilmiş sürümüne geçmeye hazır olduğunuzda, bir kopyasını tutmak istiyorsanız bu çalıştırma geçmişleri dışarı aktarabilirsiniz.

Çalıştırma geçmişleri çağrılır **denemeleri** geçerli sürümde. Modelinizin denemeleri toplamak ve SDK'sı, CLI veya Azure portalını kullanarak keşfedin.

Portal'ın çalışma Pano yalnızca Microsoft Edge, Chrome ve Firefox tarayıcılarda desteklenir:

[ ![Çevrimiçi portalı](./media/overview-what-happened-to-workbench/image001.png)] (. / media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Verileri yine hazırlayabilir miyim?

Machine Learning Workbench artık sahip değilseniz çünkü önceden var olan veri hazırlama dosyalarınızı en son sürüme taşınabilir değildir. Bununla birlikte, verilerinizi modelleme için yine de hazırlayabilirsiniz.  

Her boyuttaki veri kümeleri ile kullanabileceğiniz [Azure Machine Learning veri hazırlığı SDK'sı](http://aka.ms/data-prep-sdk) hızla verilerinizi modelleme önce Python kod yazarak hazırlamak için. 

İzleyebileceğiniz [Bu öğreticide](tutorial-data-prep.md) Azure Machine Learning veri hazırlığı SDK'sı kullanma hakkında daha fazla bilgi için.

## <a name="will-projects-persist"></a>Projeler kalacak mı?

Hiçbir kodu veya çalışmayı kaybetmeyeceksiniz. Eski sürümde projeler yerel dizini olan bulut varlıklarıydı. En son sürümü, yerel yapılandırma dosyası kullanarak Azure Machine Learning hizmeti için çalışma alanı yerel dizin ekleyin. Bkz: bir [son mimarisi diyagramı](concept-azure-machine-learning-architecture.md).

Proje içeriğin yerel makinenizde zaten oluştu. Bu nedenle bu dizinde bir yapılandırma dosyası oluşturma ve kod, çalışma alanına bağlamak için referans yeterlidir. Bilgi edinmek için nasıl [mevcut projelerinizi geçirmenin](how-to-migrate.md#projects).

Kullanmaya başlamak öğrenin [ana SDK'sı Python](quickstart-create-workspace-with-python.md) veya bu adı kullanıyor [Azure portalında](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>My kayıtlı modelleri ve görüntüleri hakkında neler diyeceksiniz?
 
Eski model kayıt defterinizde kayıtlı modelleri, bunları kullanmaya devam etmek istiyorsanız yeni çalışma alanınıza geçirilmelidir. Modellerinizi, geçirilecek [modelleri indirin ve yeniden kaydedin](how-to-migrate.md) yeni çalışma alanınızdaki. 

Eski görüntü kayıt defterinizde oluşturduğunuz görüntüleri kullanmaya devam etmek için, bunların yeni çalışma alanında yeniden oluşturulması gerekir. Bu görüntüler aşağıdaki kullanarak yeniden oluşturabilirsiniz [yapılandırma ve görüntü oluşturma](how-to-deploy-and-where.md#configureimage) bölümler. 

## <a name="what-about-deployed-web-services"></a>Dağıtılan web hizmetlerine ne oldu?

Machine Learning Model Yönetimi hesabınızı kullanarak web Hizmetleri olarak dağıtılan modelleri, Azure Container Service desteklenen sürece çalışmaz. Bu web Hizmetleri, hatta Machine Learning Model Yönetimi hesapları için destek sona erdikten sonra çalışır. Öte yandan eski CLI için destek sona erdiğinde, bu web hizmetlerini yönetme beceriniz de sona erecektir.

Ardından yeni sürümü modelleri web hizmetleri için Azure Container Instances'ı veya Azure Kubernetes Service (AKS) kümesi olarak dağıtılır. Ayrıca, FPGA ve Azure IOT Edge için dağıtabilirsiniz. Daha fazla bilgi için makaleye bakın [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md). Puanlama dosyaları, bağımlılıklar ve şemaları değiştirmek zorunda kalmadan yeni SDK veya CLI kullanarak Modellerinizi yeniden dağıtabilirsiniz. 

## <a name="what-about-the-old-sdk-and-cli"></a>CLI ve eski SDK'sı hakkında neler diyeceksiniz?

Evet, bunlar Ocak tarihine kadar çalışmaya devam edeceğiz. Önceki bkz [zaman çizelgesi](#timeline). En son SDK'sı veya CLI ile yeni denemeler ve modelleri oluşturmaya başlamanızı öneririz.

En son sürümdeki yeni Python SDK'sını kullanarak Azure Machine Learning hizmetindeki herhangi bir Python ortamı etkileşim kurabilirsiniz. En son <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı yüklemeyi öğrenin. Güncelleştirilmiş kullanabilirsiniz [Azure Machine Learning CLI uzantısı](reference-azure-machine-learning-cli.md) ile zengin `az ml` Azure Cloud Shell dahil olmak üzere herhangi bir komut satırı ortamında hizmetiyle etkileşim kurmak için komutları.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning hakkında neler diyeceksiniz?

Bu en son sürümde, Visual Studio Code için Azure Machine Learning olduğundan genişletilmiş ve önceki yeni özellikler ile çalışacak şekilde geliştirildi.

[ ![Visual Studio Code için azure Machine Learning](./media/overview-what-happened-to-workbench/vscode.png)] (. / media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Etki alanı paketlerine ne oldu?

Etki alanı paketler için [görüntü işleme, metin analizi ve tahmin](../desktop-workbench/reference-python-package-overview.md) en son sürümü Azure Machine Learning ile kullanılamaz. Ancak, yine de oluşturabilir ve görüntü işleme, metin ve tahmin modellerinin en son Azure Machine Learning Python ile eğitme <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>. Metin analizi, görüntü işleme kullanılarak oluşturulmuş mevcut modellerde geçirmeyi öğrenin ve paketler, tahmin başvurun [ AML-Packages@microsoft.com ](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [Azure Machine Learning hizmeti için en son mimarisi](concept-azure-machine-learning-architecture.md). Hızlı başlangıçlarını veya öğreticilerini birini deneyin:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Hızlı Başlangıç: Python ile bir çalışma alanı oluşturma](quickstart-get-started.md)
* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
