---
title: Uygulama yaşam döngüsü yönetimi
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da en iyi uygulama yaşam döngüsü yönetimi uygulama
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 10/27/2016
ms.openlocfilehash: 046afaa0e83fa572d6cd43a3717707892b25af69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66171111"
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da uygulama yaşam döngüsü yönetimi
Azure Machine Learning Studio, Azure bulut platformunda Çalıştır duruma getirdiniz makine öğrenimi denemeleri geliştirmek için kullanılan bir araçtır. Tek bir platformda Visual Studio IDE ve ölçeklenebilir bir bulut hizmeti gibi birleştirilir. Azure Machine Learning Studio'ya çeşitli varlıklar otomatik yürütme ve dağıtım, sürüm oluşturma standart uygulama yaşam döngüsü yönetimi (ALM) yöntemleri birleştirebilirsiniz. Bu makalede bazı seçenekleri ve yaklaşımları açıklanmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="versioning-experiment"></a>Deneme sürümü oluşturma
Denemelerinizi sürümü için önerilen iki yol vardır. Yerleşik çalıştırma geçmişi kullanır veya denemeyi harici olarak yönetmek için bir JSON biçiminde dışarı aktarın. Her yaklaşımın kendi Artıları ve eksileri ile birlikte gelir.

### <a name="experiment-snapshots-using-run-history"></a>Çalıştırma Geçmişi'ni kullanarak deneme anlık görüntüleri
Tıkladığınızda Azure Machine Learning deneme öğrenme Studio yürütme modelinde iş Zamanlayıcı deneme değişmez bir anlık görüntüsünü gönderilen **çalıştırma** deneme Düzenleyicisi. Bu anlık görüntülerin listesini görüntülemek için tıklayın **çalıştırma geçmişi** deneme Düzenleyicisi görünümündeki komut çubuğunda.

![Çalıştırma geçmişi düğmesi](./media/version-control/runhistory.png)

Anlık görüntü tıklayarak denemeyi çalıştırın ve anlık görüntü gönderildiği zaman deneme adını kilitli modda açık alındığı sonra kullanabilirsiniz. Yalnızca geçerli denemeyi gösterir, listedeki ilk öğeyi düzenlenebilir bir durumda olduğuna dikkat edin. Ayrıca her anlık görüntü çeşitli durumu olabilir bildirimi de dahil olmak üzere tamamlandı (kısmi) çalıştırın, başarısız oldu, başarısız (kısmi) çalıştırın, durumları veya taslak.

![Çalıştırma geçmişi liste](./media/version-control/runhistorylist.png)

Açıldıktan sonra anlık görüntü deney yeni bir deneme kaydedebilir ve sonra değiştirebilirsiniz. Deneme anlık eğitilen modeller, dönüşümler veya güncelleştirilmiş sürümlerini veri kümeleri gibi varlıklar içeriyorsa, anlık görüntünün anlık görüntü alındığında özgün sürümle başvuruları korur. Kilitli anlık görüntü yeni bir deneme kaydederseniz, Azure Machine Learning Studio bu varlıkları daha yeni bir sürümü var olup olmadığını algılar ve bunları yeni bir deneme otomatik olarak güncelleştirir.

Denemeyi silerseniz, bu deneyde tüm anlık görüntüleri silinir.

### <a name="exportimport-experiment-in-json-format"></a>JSON biçiminde içeri/dışarı aktarma denemesi
Çalıştırılacak gönderilen her zaman çalıştırma geçmişi anlık görüntüleri Azure Machine Learning Studio'da deneme değişmez bir sürümünü tutun. Ayrıca denemenin yerel bir kopyasını kaydedin ve Team Foundation Server gibi sık kullandığınız kaynak denetimi sistemine iade ve daha sonra yeniden deneme oluşturma, yerel dosya oluşturun. Kullanabileceğiniz [Azure Machine Learning PowerShell](https://aka.ms/amlps) commandlet'ler [ *Export-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [  *Import-AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) bunları gerçekleştirebilirsiniz.

JSON değerinin metinsel bir gösterimini varlıkları için başvuru veri kümesi veya eğitilen bir modelin gibi çalışma alanında içerebilir deneme grafiğini dosyasıdır. Bu seri hale getirilmiş bir varlık sürümü içermiyor. JSON belgesini çalışma alanınıza içeri aktarmaya çalışırsanız, başvurulan varlıkları aynı varlık deneme başvurulan kimlikleri ile önceden var olmalıdır. Aksi takdirde içeri aktarılan denemeyi erişemez.

## <a name="versioning-trained-model"></a>Eğitilen modelin sürümü oluşturma
Eğitilen bir modeli Azure Machine Learning Studio'da bir iLearner dosya bilinen bir biçime serileştirilmiş (`.iLearner`) ve çalışma alanı ile ilişkili Azure Blob Depolama hesabında depolanır. Olan iLearner dosyasını bir kopyasını almak için bir API aracılığıyla yeniden eğitme yoludur. [Bu makalede](/azure/machine-learning/studio/retrain-machine-learning-model) yeniden eğitme API nasıl çalıştığı açıklanmaktadır. Üst düzey adımları şunlardır:

1. Eğitim denemenizi ayarlayın.
2. Model eğitme modülünü veya üreten modeli hiper parametre ayarlamak veya R modeli oluşturma gibi eğitilen model modülü için bir web hizmeti çıkış bağlantı noktasını ekleyin.
3. Eğitim denemenizi çalıştırın ve ardından bir model eğitim web hizmeti olarak dağıtın.
4. BES eğitim web hizmeti uç noktasını çağırmak ve istenen iLearner dosya adını ve Blob Depolama hesabı konumu depolanacağı yeri belirtin.
5. BES çağrı bittikten sonra üretilen iLearner dosyasını toplar.

PowerShell komutu olan iLearner dosyasını almak için başka bir yolu ise [ *indirme AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Bu model programlama yoluyla yeniden eğitme gerek kalmadan iLearner dosyasını bir kopyasını almak istiyorsanız, daha kolay olabilir.

Eğitim modeli içeren iLearner dosyasını oluşturduktan sonra ardından kendi sürüm oluşturma stratejisi tercih edebilirsiniz. Stratejisi öncesi/sonek olarak bir adlandırma kuralı uygulamak ve yalnızca Blob depolama alanındaki iLearner dosyasını bırakarak veya kopyalama/sürüm denetimi sisteminiz almadan olarak basit olabilir.

Kaydedilmiş iLearner dosyasını ardından Puanlama dağıtılan web hizmetleri için de kullanılabilir.

## <a name="versioning-web-service"></a>Web hizmeti sürümü oluşturma
İki tür web Hizmetleri'nden bir Azure Machine Learning Studio'da deneme dağıtabilirsiniz. Klasik web hizmeti, çalışma alanının yanı sıra deneme ile sıkı şekilde bağlı. Artık özgün denemeyi veya çalışma alanı ile bağlı ve Azure Resource Manager framework yeni bir web hizmeti kullanır.

### <a name="classic-web-service"></a>Klasik web hizmeti
Sürüme bir Klasik web hizmeti, web hizmeti uç noktası yapısının yararlanabilirsiniz. Tipik bir akışı şu şekildedir:

1. Tahmine dayalı bir deneyden varsayılan uç nokta içeren yeni bir Klasik web hizmeti dağıtın.
2. Deneme ve eğitilen model geçerli sürümü sunan ep2 adlı yeni bir uç noktası oluşturun.
3. Geri dönün ve Tahmine dayalı denemeye ve eğitilen modeli güncelleştirin.
4. Ardından varsayılan uç nokta güncelleştirecektir Tahmine dayalı denemeye yeniden dağıtın. Ancak bu ep2 değiştirmez.
5. Deneme ve eğitilen modelin yeni sürümünü sunan ep3 adlı ek bir uç nokta oluşturursunuz.
6. Gerekirse 3. adıma geri dönün.

Zaman içinde aynı web hizmetinde oluşturulan fazla uç nokta olabilir. Her uç nokta, eğitilen model zaman içinde nokta sürümünü içeren denemeyi zaman içinde nokta kopyasını temsil eder. Ardından, eğitilen model Puanlama çalışma için bir sürümünü seçerek etkili bir şekilde anlamına çağırmak için hangi uç noktaya belirlemek için dış mantıksal kullanabilirsiniz.

Birçok aynı web hizmeti uç noktalarını oluşturabilir ve farklı sürümleri olan iLearner dosyasını benzer etkiyi elde etmek için uç nokta için düzeltme eki uygulama. [Bu makalede](create-models-and-endpoints-with-powershell.md) bunu yapmaya yönelik daha ayrıntılı olarak açıklanmaktadır.

### <a name="new-web-service"></a>Yeni web hizmeti
Yeni bir Azure Resource Manager tabanlı web hizmeti oluşturursanız, uç nokta yapısı artık kullanılamıyor. Bunun yerine, web hizmeti tanımının (WSD) dosyaları, JSON biçiminde kullanarak Tahmine dayalı denemenizi gelen oluşturabileceğiniz [dışarı aktarma AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell komutunu kullanarak veya [ *Dışarı aktarma AzMlWebservice* ](https://docs.microsoft.com/powershell/module/az.machinelearning/export-azmlwebservice) dağıtılan Resource Manager tabanlı web hizmetinden PowerShell komutu.

Dışarı aktarılan WSD dosya ve sürüm denetlemesine oluşturduktan sonra ayrıca WSD yeni bir web hizmeti olarak farklı bir web hizmeti planı farklı bir Azure bölgesinde dağıtabilirsiniz. Yeni web hizmeti planı kimliği yanı sıra, uygun bir depolama hesabı yapılandırması sağladığınız emin olmanız yeterlidir Farklı iLearner dosyalarında yama yapma WSD dosyasını değiştirin ve eğitilen modelin konumu başvuru güncelleştirme ve yeni web hizmeti olarak dağıtın.

## <a name="automate-experiment-execution-and-deployment"></a>Deneme yürütme ve dağıtımını otomatikleştirin
ALM önemli bir yönüdür yürütme ve uygulamanın dağıtım işlemini otomatik hale getirebilmek sağlamaktır. Azure Machine Learning Studio'da bunu kullanarak gerçekleştirebilirsiniz [PowerShell Modülü](https://aka.ms/amlps). İşte bir örnek için standart bir ALM ilgili uçtan uca adımları kullanarak yürütme/dağıtım işlemi otomatik [Azure Machine Learning Studio PowerShell Modülü](https://aka.ms/amlps). Her adım, bu adımı tamamlamak için kullanabileceğiniz bir veya daha fazla PowerShell commandlet'lerini bağlıdır.

1. [Bir veri kümesi karşıya](https://github.com/hning86/azuremlps#upload-amldataset).
2. Çalışma alanından bir eğitim denemesini kopyalayarak bir [çalışma](https://github.com/hning86/azuremlps#copy-amlexperiment) veya [galeri](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), veya [alma](https://github.com/hning86/azuremlps#import-amlexperimentgraph) bir [dışarı](https://github.com/hning86/azuremlps#export-amlexperimentgraph) yerel denemeden disk.
3. [DataSet'i güncellemek](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) eğitim denemesini içinde.
4. [Eğitim denemesini çalıştırma](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Eğitim modeli yükseltmek](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Tahmine dayalı denemeye kopyalama](https://github.com/hning86/azuremlps#copy-amlexperiment) çalışma alanına.
7. [Eğitilen bir modeli güncelleştirme](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) Tahmine dayalı denemeye içinde.
8. [Tahmine dayalı denemeyi çalıştırma](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Bir web hizmetini dağıtma](https://github.com/hning86/azuremlps#new-amlwebservice) Tahmine dayalı denemeye öğesinden.
10. Web hizmetini test [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) veya [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) uç noktası.

## <a name="next-steps"></a>Sonraki adımlar
* İndirme [Azure Machine Learning Studio PowerShell](https://aka.ms/amlps) modülü ve baştan ALM görevlerinizi otomatikleştirin.
* Bilgi nasıl [oluşturma ve yalnızca tek bir denemede kullanarak ML modelleri çok sayıda yönetme](create-models-and-endpoints-with-powershell.md) PowerShell ve API yeniden eğitme aracılığıyla.
* Daha fazla bilgi edinin [Azure Machine Learning web hizmetleri dağıtma](publish-a-machine-learning-web-service.md).
