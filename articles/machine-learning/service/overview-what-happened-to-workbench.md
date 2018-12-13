---
title: Workbench'e ne oldu?
titleSuffix: Azure Machine Learning service
description: Uygulama, Azure Machine Learning hizmetindeki değişiklikler ve Destek zaman çizelgesi nedir, ne Workbench hakkında öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: c9559e07cc70cbd7adafd75c23b9e67d45bee48a
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53184314"
---
# <a name="what-is-happening-to-workbench-in-azure-machine-learning-service"></a>Workbench'i Azure Machine Learning hizmetinde neler oluyor?

Workbench uygulaması ve başka bir erken özellikleri kullanım dışı ve geliştirilmiş bir yol sağlamak için Eylül 2018 sürümünden yerine [mimarisi](concept-azure-machine-learning-architecture.md). Bu sürümde deneyimlerinizi iyileştirmek için, müşteri geri bildirimlerinden gelen pek çok önemli güncelleştirme vardır. Deneme çalıştırmalarından model dağıtımına uzanan temel işlevler değişmese de, artık makine öğrenimi görevlerinizi ve işlem hatlarınızı gerçekleştirmek için sağlam <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı ve [CLI](reference-azure-machine-learning-cli.md)’yı kullanabilirsiniz.  

Bu makalede, değişiklikler ve Azure Machine Learning Workbench ve API'lerini önceden mevcut olan iş etkilemesi öğreneceksiniz.

## <a name="what-changed"></a>Ne değişti?

Azure Machine Learning hizmetinin en son sürümünde şunlar bulunur:
+ [Basitleştirilmiş Azure kaynakları modeli](concept-azure-machine-learning-architecture.md)
+ Denemelerinizi ve işlem hedeflerinizi yönetmek için [yeni portal arabirimi](how-to-track-experiments.md)
+ Yeni, daha kapsamlı Python <a href="https://aka.ms/aml-sdk" target="_blank">SDK’sı</a>
+ Makine öğrenimi için yeni ve genişletilmiş [Azure CLI uzantısı](reference-azure-machine-learning-cli.md)

Kullanım kolaylığı göz önünde bulundurularak [mimari](concept-azure-machine-learning-architecture.md) yeniden tasarlandı. Birden çok Azure kaynağı ve hesabı yerine, size gereken yalnızca bir [Azure Machine Learning hizmeti Çalışma Alanı](concept-azure-machine-learning-architecture.md#workspace)'dır.  [Azure portalda](quickstart-get-started.md) hemen çalışma alanları oluşturabilirsiniz.  Çalışma alanını, eğitim ve dağıtım işlem hedeflerini, model denemelerini, Docker görüntülerini, dağıtılan modelleri, vb. depolamak için birden çok kullanıcı kullanabilir.

Geçerli sürümde yeni, geliştirilmiş CLI ve SDK istemcileri olsa da, masaüstü Workbench uygulamasının kendisi kullanım dışı bırakılmıştır. Artık denemelerinizi [Azure web portaldaki çalışma alanı panosunda](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal) izleyebilirsiniz. Deneme geçmişinizi almak, çalışma alanınıza bağlı işlem hedeflerini yönetmek, modellerinizi ve Docker görüntülerinizi yönetmek, hatta web hizmetlerini dağıtmak için panoyu kullanın.

## <a name="how-do-i-migrate"></a>Nasıl geçiş yaparım?

Azure Machine Learning hizmetinin önceki sürümünde oluşturulan yapıtların çoğu kendi yerel depolamanızda veya bulut depolama alanında depolanır. Bu yapıtlar hiçbir zaman kaybolmayacaktır. Geçiş için, yapıtları güncelleştirilmiş Azure Machine Learning hizmetine yeniden kaydetmeniz gerekir. Neyi nasıl geçirebileceğinizi bu [geçiş makalesinde](how-to-migrate.md) öğrenebilirsiniz.

<a name="timeline"></a>

## <a name="support-timeline"></a>Destek zaman çizelgesi

Eylül 2018’den sonra bir süre daha Workbench uygulamasının yanı sıra deneme ve model yönetimi hesaplarınızı kullanmaya devam edebilirsiniz. Aşağıdaki kaynaklar için destek, bu yayından sonraki 3-4 ay içinde aşamalı olarak kaldırılacaktır. İçindekiler’in alt tarafındaki [Kaynaklar bölümünde](../desktop-workbench/tutorial-classifying-iris-part-1.md) eski özelliklere ilişkin belgelere ulaşmaya devam edebilirsiniz.

|Kullanımdan kaldırma&nbsp;aşaması|Daha eski özellikler için destek ayrıntıları|
|:---:|----------------|
|4 Aralık 2018'e|Oluşturma olanağı _Azure Machine Learning denemesi hesabı_ ve _Model Yönetimi hesabı_ Azure portalında ve clı'dan sona erdi. ML işlem ortamları CLI'dan oluşturma olanağı da sona erdi. Bir hesabınız varsa, CLI ve Workbench masaüstü sürümü bu aşamada çalışmaya devam eder.|
|9 Ocak 2019|API'ler ve Masaüstü Workbench uçları bu tarihte kalan dahil olmak üzere diğer her şey için destek.|

[Geçişe hemen başlayın](how-to-migrate.md). En yeni özelliklerin tümüne <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, [CLI](reference-azure-machine-learning-cli.md) ve [portal](quickstart-get-started.md) üzerinden ulaşılabilir.

## <a name="what-about-run-histories"></a>Çalıştırma geçmişleri ne olacak?

Çalıştırma geçmişleri bir süre daha erişilebilir kalacaktır. Azure Machine Learning hizmetinin güncelleştirilmiş sürümüne geçmeye hazır olduğunuzda, bu çalıştırma geçmişlerinin bir kopyasını saklamak istiyorsanız bunları dışarı aktarabilirsiniz.

Geçerli yayında, çalıştırma geçmişleri artık _denemeler_ olarak adlandırılıyor. Modelinizin denemelerini toplayabilir ve SDK, CLI veya web portalını kullanarak bunları inceleyebilirsiniz.

Portalın çalışma alanı panosu yalnızca Edge, Chrome ve Firefox tarayıcılarında desteklenir.

[ ![Çevrimiçi portal](./media/overview-what-happened-to-workbench/image001.png) ] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Verileri yine hazırlayabilir miyim?

Artık Workbench’imiz olmadığından önceden var olan veri hazırlama dosyalarınız en son sürüme taşınmaz. Bununla birlikte, verilerinizi modelleme için yine de hazırlayabilirsiniz.  

Küçük veri kümeleri söz konusu olduğunda, modellemeden önce verilerinizi hızla hazırlamak için <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Veri Hazırlama SDK’sını</a> kullanabilirsiniz. 

Daha büyük veri kümeleri için aynı <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı kullanabileceğiniz gibi, Azure Databricks’i de kullanabilirsiniz. 

## <a name="will-projects-persist"></a>Projeler kalacak mı?

Hiçbir kodu veya çalışmayı kaybetmeyeceksiniz. Eski sürümde projeler yerel dizini olan bulut varlıklarıydı. En son sürümde, yerel bir yapılandırma dosyasını kullanarak yerel dizinleri Azure Machine Learning hizmeti Çalışma Alanı’na eklersiniz. [En son mimarinin diyagramına bakın](concept-azure-machine-learning-architecture.md).

Proje içeriğinin çoğu zaten yerel makinenizde olduğundan, bu dizinde bir yapılandırma dosyası oluşturmanız ve çalışma alanınıza bağlanmak için kodunuzda buna başvurmanız yeterlidir. [Var olan projelerinizin nasıl geçirileceğini öğrenin.](how-to-migrate.md#projects)

[Python’da ana SDK ile ](quickstart-get-started.md) nasıl başlangıç yapılacağını öğrenin.

## <a name="what-about-my-registers-models-and-images"></a>Kayıtlı modellerim ve görüntülerim ne olacak?
 
Eski model kayıt defterinizde kayıtlı modelleri, bunları kullanmaya devam etmek istiyorsanız yeni çalışma alanınıza geçirilmelidir. [Modelleri indirip yeni çalışma alanınıza kaydederek](how-to-migrate.md) bunu yapabilirsiniz. 

Eski görüntü kayıt defterinizde oluşturduğunuz görüntüleri kullanmaya devam etmek için, bunların yeni çalışma alanında yeniden oluşturulması gerekir. Bunu izleyerek yapabilirsiniz [yapılandırma ve görüntü oluşturma](how-to-deploy-and-where.md#configureimage) bölümler. 

## <a name="what-about-deployed-web-services"></a>Dağıtılan web hizmetlerine ne oldu?

Model Yönetimi hesabınızı kullanıp web hizmetleri olarak dağıttığınız modeller Azure Container Service (ACS) desteklendiği sürece çalışmaya devam edecektir. Bu web hizmetleri, Model Yönetimi hesapları için destek sona erdiğinde bile çalışacaktır. Öte yandan eski CLI için destek sona erdiğinde, bu web hizmetlerini yönetme beceriniz de sona erecektir.

Ardından yeni sürümü modelleri web hizmetleri için Azure Container Instances(ACI) veya Azure Kubernetes Service (AKS) kümesi olarak dağıtılır. Ayrıca, FPGA ve IOT edge için dağıtabilirsiniz. Daha fazla bilgi için [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge. Puanlama dosyalarınızda, bağımlılıklarınızda ve şemalarınızda hiçbir değişiklik yapmadan yeni SDK’yı veya CLI’yi kullanarak modellerinizi yeniden dağıtabilirsiniz. 

## <a name="what-about-the-old-sdk--cli"></a>Eski SDK ve CLI'ya ne oldu?

Evet, bunlar Ocak tarihine kadar çalışmaya devam edecektir (bkz [zaman çizelgesi](#timeline) yukarıda). Yeni denemelerinizi ve modellerinizi en son SDK ve/veya CLI ile oluşturmaya başlamanızı öneririz.

En son sürümde yeni Python SDK'sı, tüm Python ortamlarında Azure Machine Learning hizmetiyle etkileşimli çalışmanızı sağlar. En son <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı yüklemeyi öğrenin.  Ayrıca, Azure portal bulut kabuğu da dahil olmak üzere herhangi bir komut satırı ortamında hizmetle etkileşim kurmak için zengin `az ml` komut kümesiyle birlikte [güncelleştirilmiş Azure CLI makine öğrenme uzantısını](reference-azure-machine-learning-cli.md) da kullanabilirsiniz.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning hakkında neler diyeceksiniz?

Bu en son sürümle birlikte, Azure Machine Learning için Visual Studio (VS) kod olduğundan genişletilmiş ve yukarıdaki yeni özellikler ile çalışacak şekilde geliştirildi.

[ ![Visual Studio Code için azure Machine Learning](./media/overview-what-happened-to-workbench/vscode.png) ] (. / media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Etki alanı paketlerine ne oldu?

[Görüntü İşleme, Metin Analizi ve Tahmin](../desktop-workbench/reference-python-package-overview.md) için etki alanı paketleri, Azure Machine Learning’in en son sürümüyle kullanılamaz. Ancak yine de görüntü işleme, metin analizi ve tahmin modellerini en son Azure Machine Learning Python<a href="https://aka.ms/aml-sdk" target="_blank">SDK’sı</a> ile derleyebilir ve eğitebilirsiniz. Görüntü İşleme, Metin Analizi ve Tahmin paketleri kullanılarak derlenmiş, önceden var olan modellerin nasıl geçirileceğini öğrenmek için [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com) adresinden bize ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning hizmetinin en son mimarisi](concept-azure-machine-learning-architecture.md) hakkında bilgi edinin ve hızlı başlangıçlardan veya öğreticilerden birini deneyin:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Hızlı Başlangıç: Python ile bir çalışma alanı oluşturma](quickstart-get-started.md)
* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md)
