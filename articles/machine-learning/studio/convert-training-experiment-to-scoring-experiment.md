---
title: Modelinizi Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl | Microsoft Docs
description: Tahmine dayalı bir deneme Machine Learning Studio'da eğitim denemenizi dönüştürerek eğitilen modelinizi bir web hizmeti olarak dağıtımına hazırlamak nasıl.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: 4bfbe22ba04f154c9f24daa13231d18e73316f9c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833851"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Modelinizi Azure Machine Learning Studio'daki dağıtımı için hazırlama

Azure Machine Learning Studio'da Tahmine dayalı bir analiz modeli geliştirmek ve bir Azure web hizmeti olarak dağıtarak faaliyete için ihtiyacınız olan araçları sağlar.

Bunu yapmak için Studio adlı bir denemeyi - oluşturmak için kullandığınız bir *eğitim denemenizi* - burada eğitme, Puanlama ve modelinizi Düzenle. Memnun kaldıktan sonra model eğitim denemenizi dönüştürerek dağıtmaya hazırlanırken bir *Tahmine dayalı denemeye* puan kullanıcı verilerini yapılandırılır.

Bu işlemde örneği görebilirsiniz [izlenecek yol: Azure Machine Learning kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md).

Bu makalede derinlemesine eğitim denemenizi Tahmine dayalı bir deneme nasıl dönüştürüldüğü ve bu Tahmine dayalı denemeye nasıl dağıtıldığını ayrıntılarını içine alır. Bu ayrıntılar anlayarak, dağıtılan modelinizi daha etkili olması için yapılandırmak nasıl öğrenebilirsiniz.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a>Genel Bakış 

Tahmine dayalı bir deneme eğitim denemenizi dönüştürme işlemi üç adımdan oluşur:

1. Makine öğrenimi algoritması modüller, eğitilen model ile değiştirin.
2. Puanlama için gerekli olan modülleri denemede kesim. Eğitim denemenizi eğitim için gerekli olan, ancak model eğitildi sonra gerekli olmayan modülleri sayısını içerir.
3. Modelinizi web hizmeti kullanıcı verileri nasıl kabul ettiği ve hangi verilerin döndürülür tanımlayın.

> [!TIP]
> Eğitim denemenizi eğitim ve puanlama kendi verilerinizi kullanarak modelinizi ilgilenen. Ancak uygulama dağıtıldıktan sonra kullanıcılar modelinize yeni veri göndermek ve tahmin sonuçlarını döndürür. Dağıtım için hazır hale getirmek için Tahmine dayalı bir deneme eğitim denemenizi dönüştürmek gibi bu nedenle, nasıl model başkaları tarafından kullanılacak göz önünde bulundurun.
> 
> 

## <a name="set-up-web-service-button"></a>Web hizmetinin ayarı düğmesi
Denemenizi çalıştırdıktan sonra (tıklatın **çalıştırmak** deneme tuvalinin altındaki), tıklatın **Web hizmetinin ayarı** düğmesini (seçin **Tahmine dayalı Web hizmeti** seçeneği). **Web hizmetinin ayarı** sizin için Tahmine dayalı bir deneme eğitim denemenizi dönüştürme üç adımları gerçekleştirir:

1. Eğitilmiş modelinizi kaydeder **eğitilmiş modeller** modül paletindeki (deneme tuvalinin sol) bölümü. Ardından makine öğrenme algoritmasının yerine geçer ve [Train Model] [ train-model] kaydedilmiş eğitilen model modüllerle.
2. Denemenizi analiz eder ve yalnızca eğitim için açıkça kullanılmış ve artık gerekmeyen modülleri kaldırır.
3. Bunu ekler _Web hizmeti girişi_ ve _çıkış_ denemenizi (Bu modülleri kabul etmek ve kullanıcı verilerini dönmek) varsayılan konumlarda modüllerine.

Örneğin, aşağıdaki deneme örnek census verileri kullanarak iki sınıflı artırılmış karar ağacı modeli eğitir:

![Eğitim denemenizi][figure1]

Bu deneme modülleri temelde dört farklı işlevleri gerçekleştirir:

![Modül işlevleri][figure2]

Bu eğitim denemenizi Tahmine dayalı bir deneme dönüştürürken bazı bu modüllerin artık gerekli olmayan veya farklı bir amaç şimdi verdikleri:

* **Veri** -Bu örnek veri kümesindeki veriler Puanlama sırasında kullanılmaz - web hizmeti kullanıcı belirtmek için veri sağlayacak. Ancak, veri türleri gibi bu veri kümesi meta verileri eğitilen modeli tarafından kullanılır. Bu nedenle böylece bu meta verileri sağlayabilir Tahmine dayalı denemeye dataset tutmanız gerekir.

* **Hazırlığı** - Puanlama için bu modülleri olabilir veya gelen verileri işlemek gerekli olmayabilir gönderilecek kullanıcı verilere bağlı olarak. **Web hizmetinin ayarı** düğmesi bu touch değil - nasıl bunları işlemek istediğinize karar vermeniz gerekir.
  
    Örneğin, örnek veri kümesi olabilir eksik değerleri, bu nedenle bu örnekte bir [Clean Missing Data] [ clean-missing-data] modülü bunlarla dağıtılacak dahil. Ayrıca, örnek veri kümesi modeli eğitmek için gerekli olmayan sütunları içerir. Bu nedenle bir [Select Columns in Dataset sütun] [ select-columns] modülü veri akışından bu ek sütunları hariç tutulacak dahil. Web hizmeti aracılığıyla Puanlama için gönderilen veri eksik değerleri olmaz ve ardından, kaldırabilirsiniz biliyorsanız [Clean Missing Data] [ clean-missing-data] modülü. Ancak, bu yana [Select Columns in Dataset sütun] [ select-columns] modülü yardımcı olan eğitilen model bekliyor veri sütunlarının tanımlamak, bu modül kalması gerekir.

* **Eğitmek** -Bu modüller modeli eğitmek için kullanılır. Tıkladığınızda **Web hizmetinin ayarı**, bu modüller, eğitilmiş model içeren tek bir modülü ile değiştirilir. Bu yeni modül kaydedilir **eğitilmiş modeller** modül paleti bölümü.

* **Puan** - Bu örnekte, [bölünmüş veri] [ split] modülü, veri akışı test verileri ve eğitim veri bölmek için kullanılır. Tahmine dayalı denemesinde biz artık, bunu Eğitim değil [bölünmüş veri] [ split] kaldırılabilir. Benzer şekilde, ikinci [Score Model] [ score-model] modülü ve [Evaluate Model] [ evaluate-model] modülü, bu nedenle test verileri sonuçları karşılaştırmak için kullanılır Bu modüller Tahmine dayalı denemeye gerekli değildir. Kalan [Score Model] [ score-model] modülü, ancak, web hizmeti aracılığıyla bir puan sonuca dönmek için gereklidir.

İşte tıkladıktan sonra örneğimizde nasıl göründüğünü **Web hizmetinin ayarı**:

![Dönüştürülen tahmini deneme][figure3]

Çalışmanın **Web hizmetinin ayarı** denemenizi bir web hizmeti olarak dağıtılması için hazırlamak yeterli olabilir. Ancak, bazı ek iş denemenizi için belirli yapmak isteyebilirsiniz.

### <a name="adjust-input-and-output-modules"></a>Giriş ve çıkış modülleri Ayarla
Eğitim denemenizi bir eğitim veri kümesi kullanılan ve makine öğrenme algoritmasını gerekli bir formda veri almak için bazı işleme vermedi. Web hizmeti aracılığıyla almaya beklediğiniz verileri bu işlem gerekmez, atlayabilirsiniz: çıkışına bağlayın **Web hizmeti giriş Modülü** denemenizi farklı bir modüle için. Kullanıcının verileri artık bu konumda modelindeki ulaşırsınız.

Örneğin, varsayılan olarak **Web hizmetinin ayarı** koyar **Web hizmeti girişi** Yukarıdaki şekilde gösterildiği gibi veri akışı üstündeki modülü. Ancak biz el ile yerleştirebilirsiniz **Web hizmeti girişi** veri işleme modülleri geçmiş:

![Web hizmeti girişi taşıma][figure4]

Web hizmeti aracılığıyla sağlanan giriş verileri, herhangi bir önişleme olmadan doğrudan Score Model modüle şimdi geçer.

Benzer şekilde, varsayılan olarak **Web hizmetinin ayarı** Web hizmeti çıkış modülü, veri akışı sonundaki koyar. Bu örnekte, web hizmeti çıktısını kullanıcıya döndürülecek [Score Model] [ score-model] tam giriş verisi vektör artı Puanlama sonuçları içeren modülü.
Farklı bir şey döndürmek tercih ediyorsanız, ancak daha sonra önce ek modüller ekleyebilirsiniz **Web hizmeti çıkış** modülü. 

Örneğin, yalnızca Puanlama sonuçları ve giriş verileri değil tüm vektörü döndürmek için ekleyin bir [Select Columns in Dataset sütun] [ select-columns] Puanlama sonuçları hariç tüm sütunlar hariç tutulacak modülü. Ardından taşıma **Web hizmeti çıkış** çıktısını modülüne [Select Columns in Dataset sütun] [ select-columns] modülü. Denemeyi şöyle görünür:

![Web hizmeti çıkış taşıma][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Ek veri işleme modülleri Ekle Kaldır
Puanlama sırasında ihtiyaç bildiğiniz denemenizi daha fazla modülleri varsa bunlar kaldırılabilir. Örneğin, biz taşınmış olduğundan **Web hizmeti girişi** noktasında veri işleme modülleri sonra modülü, biz kaldırabilirsiniz [Clean Missing Data] [ clean-missing-data] Modülü Tahmine dayalı denemeye.

Bizim Tahmine dayalı denemeye şimdi şöyle görünür:

![Ek modülü kaldırma][figure6]


### <a name="add-optional-web-service-parameters"></a>İsteğe bağlı Web hizmeti parametreleri ekleme
Bazı durumlarda, hizmet erişildiğinde modülleri davranışını değiştirmek kullanıcı web hizmetinizin izin vermek isteyebilirsiniz. *Web hizmeti parametreleri* bu yapmanıza olanak sağlar.

Yaygın bir örnek ayarlama bir [veri içeri aktarma] [ import-data] web hizmeti erişildiğinde dağıtılan web hizmeti kullanıcı farklı bir veri kaynağına belirtebilmeniz modülü. Veya yapılandırma bir [verileri dışa aktar] [ export-data] modülü böylece farklı bir hedef belirtilebilir.

Web hizmeti parametreleri tanımlamak ve bir veya daha fazla modülü parametreleri ile ilişkilendirmek ve gerekli veya isteğe bağlı oldukları belirtebilirsiniz. Web hizmeti kullanıcı hizmete erişme ve modül Eylemler uygun şekilde değiştirilir Bu parametreler için değerler sağlar.

Web hizmeti parametreleri nedir ve bunların nasıl kullanılacağını hakkında daha fazla bilgi için bkz: [kullanarak Azure Machine Learning Web hizmeti parametreleri][webserviceparameters].

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Tahmine dayalı denemeye bir web hizmeti olarak dağıtma
Tahmine dayalı denemeye yeterince hazırlandı, bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model kendi tahminleri döndürür.

Tam dağıtım işlemi hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma][deploy]

[deploy]: publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
