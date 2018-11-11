---
title: Azure Machine Learning Workbench’te neler oluyor? | Microsoft Docs
description: Workbench uygulamasına ne olduğu, Azure Machine Learning’de nelerin değiştiği ve destek zaman çizelgesinin ne olduğu hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: b8263c399f287be79590860cce7036207ef2e3f7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243752"
---
# <a name="what-is-happening-to-workbench-in-azure-machine-learning-preview"></a>Azure Machine Learning’de (önizleme) Workbench’e neler oluyor?

Workbench uygulaması ve bazı eski özellikler, gelişmiş bir [mimarinin](concept-azure-machine-learning-architecture.md) önünü açmak için Eylül 2018’de yeni sistemlerle değiştirildi. Bu sürümde deneyimlerinizi iyileştirmek için, müşteri geri bildirimlerinden gelen pek çok önemli güncelleştirme vardır. Deneme çalıştırmalarından model dağıtımına uzanan temel işlevler değişmese de, artık makine öğrenimi görevlerinizi ve işlem hatlarınızı gerçekleştirmek için sağlam <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı ve [CLI](reference-azure-machine-learning-cli.md)’yı kullanabilirsiniz.  

Bu makalede Azure Machine Learning hizmetinde nelerin değiştiğini ve zaten var olan işlerinizin bu değişikliklerden nasıl etkilendiğini göreceksiniz.

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

|Aşama|Daha eski özellikler için destek ayrıntıları|
|:---:|----------------|
|1|Azure portalda ve CLI’den _Azure Machine Learning Denemesi hesabı_ ve _Model Yönetimi hesabı_ oluşturma özelliği sonlandırılıyor. CLI’dan ML İşlem Ortamı oluşturma özelliği de sonlandırılıyor. Bir hesabınız varsa, CLI ve Workbench masaüstü sürümü bu aşamada çalışmaya devam eder.|
|2|Geri kalan API’ler ve Workbench masaüstü sürümü de dahil olmak üzere diğer tüm hizmetlerin desteği bu aşamada sonlandırılmaktadır.|

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
 
Eski model kayıt defterine kaydettiğiniz modelleri kullanmak istiyorsanız, bunlar yeni çalışma alanınıza geçirilmelidir. [Modelleri indirip yeni çalışma alanınıza kaydederek](how-to-migrate.md) bunu yapabilirsiniz. 

Eski görüntü kayıt defterinizde oluşturduğunuz görüntüleri kullanmaya devam etmek için, bunların yeni çalışma alanında yeniden oluşturulması gerekir. Bu işlemi, [docker görüntüsü oluşturma](how-to-deploy-to-aci.md#configure-an-image) bölümünü uygulayarak gerçekleştirebilirsiniz. 

## <a name="what-about-deployed-web-services"></a>Dağıtılan web hizmetlerine ne oldu?

Model Yönetimi hesabınızı kullanıp web hizmetleri olarak dağıttığınız modeller Azure Container Service (ACS) desteklendiği sürece çalışmaya devam edecektir. Bu web hizmetleri, Model Yönetimi hesapları için destek sona erdiğinde bile çalışacaktır. Öte yandan eski CLI için destek sona erdiğinde, bu web hizmetlerini yönetme beceriniz de sona erecektir.

Yeni sürümde, modeller web hizmetleri olarak [Azure Container Instances](how-to-deploy-to-aci.md) (ACI) veya [Azure Kubernetes Service](how-to-deploy-to-aks.md) (AKS) kümelerine dağıtılır. Ayrıca [FPGA'lara ve IoT Edge’e de dağıtabilirsiniz](how-to-deploy-and-where.md). Puanlama dosyalarınızda, bağımlılıklarınızda ve şemalarınızda hiçbir değişiklik yapmadan yeni SDK’yı veya CLI’yi kullanarak modellerinizi yeniden dağıtabilirsiniz. 

## <a name="what-about-the-old-sdk--cli"></a>Eski SDK ve CLI'ya ne oldu?

Evet, bir süre daha çalışmaya devam edecekler (yukarıdaki [zaman çizelgesine](#timeline) bakın). Yeni denemelerinizi ve modellerinizi en son SDK ve/veya CLI ile oluşturmaya başlamanızı öneririz.

En son sürümde yeni Python SDK'sı, tüm Python ortamlarında Azure Machine Learning hizmetiyle etkileşimli çalışmanızı sağlar. En son <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’yı yüklemeyi öğrenin.  Ayrıca, Azure portal bulut kabuğu da dahil olmak üzere herhangi bir komut satırı ortamında hizmetle etkileşim kurmak için zengin `az ml` komut kümesiyle birlikte [güncelleştirilmiş Azure CLI makine öğrenme uzantısını](reference-azure-machine-learning-cli.md) da kullanabilirsiniz.

## <a name="what-about-vs-code-tools-for-ai"></a>VS Code Tools for AI uzantısına ne oldu?

Ben en son sürümle birlikte, Visual Studio (VS) Code Tools for AI uzantısı, yukarıdaki yeni özelliklerle çalışacak şekilde genişletildi ve geliştirildi.

[ ![Visual Studio Code Tools for AI](./media/overview-what-happened-to-workbench/vscode.png) ] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Etki alanı paketlerine ne oldu?

[Görüntü İşleme, Metin Analizi ve Tahmin](../desktop-workbench/reference-python-package-overview.md) için etki alanı paketleri, Azure Machine Learning’in en son sürümüyle kullanılamaz. Ancak yine de görüntü işleme, metin analizi ve tahmin modellerini en son Azure Machine Learning Python<a href="https://aka.ms/aml-sdk" target="_blank">SDK’sı</a> ile derleyebilir ve eğitebilirsiniz. Görüntü İşleme, Metin Analizi ve Tahmin paketleri kullanılarak derlenmiş, önceden var olan modellerin nasıl geçirileceğini öğrenmek için [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com) adresinden bize ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning hizmetinin en son mimarisi](concept-azure-machine-learning-architecture.md) hakkında bilgi edinin ve hızlı başlangıçlardan veya öğreticilerden birini deneyin:

* [Azure Machine Learning hizmeti nedir?](overview-what-is-azure-ml.md)
* [Hızlı Başlangıç: Python ile çalışma alanı oluşturma](quickstart-get-started.md)
* [Öğretici: Modeli eğitme](tutorial-train-models-with-aml.md)
