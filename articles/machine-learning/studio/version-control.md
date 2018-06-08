---
title: Azure Machine learning'de ALM | Microsoft Docs
description: Azure Machine Learning Studio uygulama yaşam döngüsü yönetimi en iyi yöntemleri uygulayın
keywords: ALM, AML, Azure ML, uygulama yaşam döngüsü yönetimi, sürüm denetimi
services: machine-learning
documentationcenter: ''
author: hning86
ms.author: haining
manager: mwinkle
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.openlocfilehash: 49d1a228132cc220b30091481bb542623b1e222d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835874"
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da uygulama yaşam döngüsü yönetimi
Azure Machine Learning Studio, Azure bulut platform kullanıma hazır hale getirilmiş, machine learning denemelerini geliştirmek için kullanılan bir araçtır. Tek bir platformuna Visual Studio IDE ve ölçeklenebilir bulut hizmeti gibi birleştirilir. Sürüm oluşturma standart uygulama yaşam döngüsü yönetimi (ALM) uygulamaları otomatik yürütme ve dağıtım için çeşitli varlıklar Azure Machine Learning Studio'ya dahil edebilirsiniz. Bu makalede seçenekleri ve yaklaşımlar bazıları açıklanmaktadır.

## <a name="versioning-experiment"></a>Sürüm oluşturma deneme
Denemelerinizi sürüm için önerilen iki yolu vardır. Yerleşik çalıştırma geçmişi kullanır veya deneme dışarıdan yönetmek amacıyla bir JSON biçiminde dışarı aktarma. Her iki yaklaşımın kendi Artıları ve eksileri ile gelir.

### <a name="experiment-snapshots-using-run-history"></a>Çalıştırma geçmişi kullanarak deneme anlık görüntüler
Tıkladığınızda Azure Machine Learning öğrenimi denemesinin Studio'da yürütme modelinde İş Zamanlayıcısı değişmez bir anlık görüntüsünü deneme gönderilen **çalıştırmak** deneme düzenleyicisinde. Bu anlık görüntülerin listesini görüntülemek için tıklatın **çalıştırma geçmişi** deneme Düzenleyicisi görünümündeki komut çubuğunda.

![Çalıştırma geçmişi düğmesi](./media/version-control/runhistory.png)

Açık denemeyi çalıştırma ve anlık görüntü gönderilen zaman deneme adını tıklatarak kilitli modunda anlık görüntü alındıktan sonra kullanabilirsiniz. Yalnızca geçerli deneme temsil eden, listedeki ilk öğeyi düzenlenebilir bir durumda olduğuna dikkat edin. Ayrıca her anlık görüntü çeşitli durumunda olabilir bildirim de dahil olmak üzere tamamlandı (kısmi) çalıştırın, başarısız, başarısız (kısmi) çalıştırmak, durumları ya da taslak.

![Geçmiş listesini çalıştırın.](./media/version-control/runhistorylist.png)

Açıldıktan sonra anlık görüntü deney yeni bir deneme kaydedin ve sonra değiştirebilirsiniz. Deneme anlık görüntünüz eğitilmiş modeller, Dönüşümlerin veya güncelleştirilmiş sürümlerini veri kümeleri gibi varlıklar içeriyorsa, anlık görüntü alındıktan olduğunda anlık görüntüyü özgün sürümü başvuruları korur. Kilitli anlık görüntü yeni bir deneme kaydederseniz, Azure Machine Learning Studio'da bir sürümü bu varlıkları varlığını algılar ve bunları yeni bir deneme otomatik olarak güncelleştirir.

Denemeyi silerseniz, bu deneme tüm anlık görüntüleri silinir.

### <a name="exportimport-experiment-in-json-format"></a>JSON biçiminde içeri/dışarı aktarma deneme
Çalıştırmak için gönderilen her zaman çalıştırma geçmişi anlık görüntüleri Azure Machine Learning Studio'da deneme değişmez bir sürümünü tutun. Ayrıca deneme yerel bir kopyasını kaydedin ve Team Foundation Server gibi sık kullanılan kaynak denetim sistemi iade ve daha sonra bu yerel dosyadan bir denemeyi yeniden oluşturun. Kullanabileceğiniz [Azure Machine Learning PowerShell](http://aka.ms/amlps) cmdlet'leri [ *verme AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [ *alma AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) bunu yapmaya yönelik.

Bir veri kümesi veya bir modeli gibi çalışma alanındaki varlıklar için bir başvuru içerebilir deneme grafiği metinsel gösterimini JSON dosyasıdır. Varlığın seri hale getirilmiş bir sürüm içermiyor. Çalışma alanına JSON belgesini içeri aktarmaya çalışırsanız, başvurulan varlıklar denemesinde başvurulan kimlikleri aynı varlık ile zaten mevcut olmalıdır. Aksi takdirde içeri aktarılan deneme erişemiyor.

## <a name="versioning-trained-model"></a>Sürüm oluşturma eğitilen modeli
Bir Azure Machine Learning eğitilen model iLearner dosya olarak bilinen bir biçime serileştirilmiş (`.iLearner`) ve çalışma alanıyla ilişkilendirilmiş Azure Blob Depolama hesabında depolanır. İLearner dosyasının bir kopyasını almak için bir yeniden eğitme API aracılığıyla yoludur. [Bu makalede](retrain-models-programmatically.md) yeniden eğitme API nasıl çalıştığını açıklar. Üst düzey adımları şunlardır:

1. Eğitim denemenizi ayarlayın.
2. Train Model modülü veya Model Hyperparameter ayarlamak veya R modeli oluşturma gibi eğitilen model üreten modülü için web hizmeti çıkış bağlantı noktasına ekleyin.
3. Eğitim denemenizi çalıştırın ve ardından bir model eğitim web hizmeti olarak dağıtabilirsiniz.
4. Eğitim web hizmetinin BES uç noktasını çağırmak ve istenen iLearner dosya adı ve Blob Depolama hesabı konumu depolanacağı yeri belirtin.
5. BES çağrısı bittikten sonra üretilen iLearner dosya elde etme.

İLearner dosyasını almak için başka bir PowerShell komutunu yoludur [ *indirme AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Bu model program aracılığıyla yeniden eğitme gerek kalmadan iLearner dosyasının bir kopyasını almak istiyorsanız, daha kolay olabilir.

Eğitim modeli içeren iLearner dosyasını oluşturduktan sonra daha sonra kendi sürüm oluşturma stratejisini tercih edebilirsiniz. Stratejisi öncesi/sonek bir adlandırma kuralı olarak uygulama ve yalnızca Blob depolama alanındaki iLearner dosya bırakarak ya da kopyalama/sürüm denetimi sisteminize almadan olarak kadar basit olabilir.

Kaydedilen iLearner dosya sonra dağıtılmış web Hizmetleri aracılığıyla Puanlama için kullanılabilir.

## <a name="versioning-web-service"></a>Sürüm oluşturma web hizmeti
Bir Azure Machine Learning deneme web hizmetlerinden iki tür dağıtabilirsiniz. Klasik web hizmeti çalışma yanı sıra deneme ile sıkı şekilde bağlı. Artık özgün deneme veya çalışma ile bağlı ve yeni web hizmeti Azure Resource Manager framework kullanır.

### <a name="classic-web-service"></a>Klasik web hizmeti
Klasik web hizmeti sürümü için web hizmeti uç noktası yapının avantajından yararlanabilirsiniz. Tipik bir akışı şöyledir:

1. Tahmine dayalı denemenizi varsayılan bir uç içeren yeni bir Klasik web hizmeti dağıtın.
2. Deneme ve eğitilmiş model geçerli sürümü kullanıma sunan ep2 adlı yeni bir uç noktası oluşturun.
3. Geri dönün ve Tahmine dayalı denemeye ve eğitilen model güncelleştirin.
4. Varsayılan uç sonra güncelleştirecektir Tahmine dayalı denemeye yeniden dağıtın. Ancak bu ep2 değiştirmez.
5. Yeni sürümünü deneme ve eğitilen modelini gösterir ep3 adlı başka bir uç nokta oluşturun.
6. Gerekirse 3. adıma geri dönün.

Zaman içinde çok sayıda uç nokta aynı web hizmetinde oluşturuldu olabilir. Her uç nokta eğitilen model zaman içinde nokta sürümünü içeren deneme zaman noktası kopyasını temsil eder. Daha sonra yani etkili bir şekilde eğitilen modeli Puanlama çalıştırma için bir sürümünü seçerek çağırmak için hangi uç noktaya belirlemek için dış mantığı kullanabilirsiniz.

Birçok aynı web hizmeti uç noktalarını da oluşturabilir ve farklı sürümlerini benzer efekti elde etmek için uç nokta iLearner dosyasına düzeltme eki. [Bu makalede](create-models-and-endpoints-with-powershell.md) bunu yapmaya yönelik daha ayrıntılı olarak açıklanmaktadır.

### <a name="new-web-service"></a>Yeni web hizmeti
Yeni Azure Resource Manager tabanlı web hizmeti oluşturursanız, uç nokta yapı artık mevcut değil. Bunun yerine, web hizmeti tanımının (WSD) dosyaları, JSON biçiminde kullanarak Tahmine dayalı denemenizi oluşturabilirsiniz [verme AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell komutunu kullanarak veya [ *verme AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) PowerShell komutunu dağıtılmış Resource Manager tabanlı web hizmeti.

Dışarı aktarılan WSD dosya ve denetlemesine sürüm oluşturduktan sonra da WSD yeni bir web hizmeti olarak farklı bir Azure bölgesindeki farklı bir web hizmeti planındaki dağıtabilirsiniz. Yalnızca doğru depolama hesabı yapılandırmanın yanı sıra yeni web hizmeti planı kimliği sağladığınız emin olun Farklı iLearner dosyalarında düzeltme eki için WSD dosyasını değiştirmek ve konum başvurusu eğitilen modeli güncelleştirmek ve yeni bir web hizmeti olarak dağıtabilir.

## <a name="automate-experiment-execution-and-deployment"></a>Deneme yürütme ve dağıtımı otomatik hale getirme
Bir önemli ALM yürütme ve uygulamanın dağıtım işlemini otomatikleştirmek için yönüdür. Azure Machine Learning ile bunu kullanarak gerçekleştirebilirsiniz [PowerShell Modülü](http://aka.ms/amlps). İşte standart ALM için uygun olan uçtan uca adımları örneği kullanarak yürütme/dağıtım işlemi otomatik [Azure Machine Learning Studio PowerShell Modülü](http://aka.ms/amlps). Her adım, bu adımı gerçekleştirmek için kullanabileceğiniz bir veya daha fazla PowerShell cmdlet'leri bağlanır.

1. [Bir veri kümesi karşıya](https://github.com/hning86/azuremlps#upload-amldataset).
2. Çalışma alanından bir eğitim denemenizi kopyalayın bir [çalışma](https://github.com/hning86/azuremlps#copy-amlexperiment) veya [galeri](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), veya [alma](https://github.com/hning86/azuremlps#import-amlexperimentgraph) bir [dışarı](https://github.com/hning86/azuremlps#export-amlexperimentgraph) yerel diskten deneyin.
3. [Veri kümesi güncelleştirmesi](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) eğitim denemede.
4. [Eğitim denemeyi çalıştırın](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Eğitim modeli yükseltmek](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Tahmine dayalı denemeye kopyalama](https://github.com/hning86/azuremlps#copy-amlexperiment) çalışma alanına.
7. [Eğitim modeli güncelleştirmek](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) Tahmine dayalı denemede.
8. [Tahmine dayalı denemeyi çalıştırın](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Bir web hizmetini dağıtma](https://github.com/hning86/azuremlps#new-amlwebservice) Tahmine dayalı denemeye gelen.
10. Web hizmetini sınama [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) veya [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) uç noktası.

## <a name="next-steps"></a>Sonraki adımlar
* Karşıdan [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) modülü ve ALM görevleri otomatikleştirmek için Başlat.
* Bilgi nasıl [oluşturmak ve yalnızca tek bir deneme kullanarak ML modelleri çok sayıda yönetmek](create-models-and-endpoints-with-powershell.md) PowerShell ve API yeniden eğitme aracılığıyla.
* Daha fazla bilgi edinmek [Azure Machine Learning web hizmetlerini dağıtma](publish-a-machine-learning-web-service.md).
