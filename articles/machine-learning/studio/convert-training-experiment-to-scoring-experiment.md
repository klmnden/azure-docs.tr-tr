---
title: Model dağıtımı için hazırlama
titleSuffix: Azure Machine Learning Studio
description: Machine Learning Studio'da eğitim denemesini öngörücü bir denemeye dönüştürme tarafından eğitilen modeli bir web hizmeti olarak dağıtım için hazırlamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 03/28/2017
ms.openlocfilehash: 2a318edada5cdc4124e221fdc8c441ab323a9289
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60752002"
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio"></a>Modelinizin Azure Machine Learning Studio'da dağıtımı için hazırlama

Azure Machine Learning Studio'da öngörülebilir bir analitik model geliştirmenize ve ardından bir Azure web hizmeti olarak dağıtarak faaliyete geçirmek için ihtiyacınız olan araçları sağlar.

Bunu yapmak için Studio adlı bir deneme - oluşturmak için kullandığınız bir *eğitim denemesini* - Burada, eğitme, Puanlama ve modelinizi Düzenle. Memnun olduğunuzda, modelinizi eğitim denemenizi dönüştürerek dağıtmaya hazırlanma bir *Tahmine dayalı denemeye* puanı kullanıcı verileri için yapılandırılır.

Bu işlemde örneği gördüğünüz [öğretici 1: Kredi riskini tahmin](tutorial-part1-credit-risk.md).

Bu makalede bir yakından eğitim denemesini öngörücü bir denemeye nasıl dönüştürüldüğünü ve bu Tahmine dayalı denemeye nasıl dağıtıldığını ayrıntılarını alır. Bu ayrıntılar anlayarak, dağıtılan modelinizin daha verimli hale getirmek için yapılandırma konusunda bilgi edinebilirsiniz.



## <a name="overview"></a>Genel Bakış 

Eğitim denemesini öngörücü bir denemeye dönüştürme işlemi üç adımdan oluşur:

1. Makine öğrenimi algoritması, eğitilen model modüllerle değiştirin.
2. Denemeyi Puanlama için gerekli olan modüller için kesim. Eğitim denemesini eğitim için gerekli olan ancak modeli eğitilir sonra gerekli olmayan modüller içerir.
3. Modelinizi web hizmeti kullanıcı verileri kabul nasıl ve hangi verilerin döndürülecek tanımlayın.

> [!TIP]
> Eğitim denemenizi eğitim ve puanlama kendi verilerinizi kullanarak modelinizi endişe başardım. Ancak uygulama dağıtıldıktan sonra kullanıcılar modelinize yeni veri göndermek ve tahmin sonuçlarını döndürür. Dağıtım için hazır almak için öngörücü bir denemeye eğitim denemenizi Dönüştür gibi bu nedenle, nasıl modelin başkaları tarafından kullanılacak göz önünde bulundurun.
> 
> 

## <a name="set-up-web-service-button"></a>Web hizmetini Ayarla düğmesi
Denemenizi çalıştırdıktan sonra (tıklayın **ÇALIŞTIRMA** deneme tuvalinin altındaki), tıklayın **Web hizmetinin ayarı** düğmesine (seçin **Tahmine dayalı Web hizmeti** seçeneği). **Web hizmetinin ayarı** sizin için eğitim denemesini öngörücü bir denemeye dönüştürme üç adımı gerçekleştirir:

1. Eğitilen modelinizde kaydeder **eğitilen modelleri** (deneme tuvalinin sol için) modül paletinin bölümü. Daha sonra bir makine öğrenme algoritmasını değiştirir ve [modeli eğitme] [ train-model] modülleri ile kaydedilmiş eğitilen modeli.
2. Denemenizi analiz eder ve yalnızca eğitim için açıkça kullanılmış ve artık gerekmeyen modülleri kaldırır.
3. Bunu ekler _Web hizmeti giriş_ ve _çıkış_ varsayılan konumda (Bu modülleri kabul edin ve dönüş kullanıcı verileri), denemenize modülleri.

Örneğin, aşağıdaki denemenin örnek görselleştirmenizdeki verilerin kullanarak iki sınıflı artırmalı karar ağacı modeli eğitir:

![Eğitim denemesini](./media/convert-training-experiment-to-scoring-experiment/figure1.png)

Bu deneyde modülleri temelde dört farklı işlevleri gerçekleştirir:

![Modül işlevleri](./media/convert-training-experiment-to-scoring-experiment/figure2.png)

Bu eğitim denemesini öngörücü bir denemeye dönüştürme yaptığınızda, bazı bu modüllerin artık gerekli olmayan veya artık farklı bir amaç sağladıkları:

* **Veri** -örnek veri kümesinde veri Puanlama sırasında kullanılmaz - web hizmeti kullanıcı verileri puanlanması sağlayacak. Ancak, veri türleri gibi bu veri kümesi meta verileri, eğitilen model tarafından kullanılır. Bu nedenle bu meta veriler sağlayabilmesi veri kümesi Tahmine dayalı denemeye tutmanız gerekir.

* **Hazırlığı** - Puanlama için bu modülleri olabilir veya gelen veriyi işlemek gerekli olmayabilir gönderilecek kullanıcı verilere bağlı olarak. **Web hizmetinin ayarı** düğmesi bu touch değil - nasıl, bunları işlemek istediğinize karar vermeniz gerekir.
  
    Örneğin, örnek veri kümesi olabilir eksik değerleri, bu nedenle bu örnekte bir [eksik verileri temizleme] [ clean-missing-data] modülü bunlarla işlem dahil. Ayrıca, örnek veri kümesi modeli eğitmek için gerekli olmayan sütunları içerir. Bu nedenle bir [kümesindeki sütunları seçme] [ select-columns] modülü dahil edilen veri akışından ek sütunlar dışlanacak. Web hizmeti aracılığıyla Puanlama için gönderilen veri eksik değerleri olmaz ve ardından, kaldırabilirsiniz biliyorsanız [eksik verileri temizleme] [ clean-missing-data] modülü. Ancak, bu yana [kümesindeki sütunları seçme] [ select-columns] modülü yardımcı olan eğitilen modelin veri sütunlarını tanımlar, bu modül kalması gerekir.

* **Eğitim** -Bu modüller modeli eğitmek için kullanılır. Tıkladığınızda **Web hizmetinin ayarı**, bu modüller, eğitilen modeli içeren tek bir modül ile değiştirilir. Bu yeni modül kaydedilir **eğitilen modelleri** modül paletinin bölümü.

* **Puan** - Bu örnekte, [verileri bölme] [ split] modülünün test verileri ve eğitim verilerini veri akışı bölmek için kullanılır. Tahmine dayalı deneme biz artık bunu Eğitim değil [verileri bölme] [ split] kaldırılabilir. Benzer şekilde, ikinci [Score Model] [ score-model] modülü ve [Evaluate Model] [ evaluate-model] modülü sonuçları test verileri, bu nedenle karşılaştırmak için kullanılır Bu modüller Tahmine dayalı denemeye gerekli değildir. Kalan [Score Model] [ score-model] modülü, ancak web hizmeti aracılığıyla bir puan sonuç döndürmek için gereklidir.

İşte tıklandıktan sonra Örneğimizdeki nasıl göründüğünü **Web hizmetinin ayarı**:

![Tahmine dayalı denemeye dönüştürüldü](./media/convert-training-experiment-to-scoring-experiment/figure3.png)

İşleri halletmek **Web hizmetinin ayarı** deneyiminizi bir web hizmeti olarak dağıtılması hazırlamak yeterli olabilir. Ancak, bazı ek işleri denemenizi için belirli yapmak isteyebilirsiniz.

### <a name="adjust-input-and-output-modules"></a>Giriş ve çıkış modülleri ayarlama
Eğitim denemenizi bir eğitim veri kümesi kullanılan ve daha sonra machine learning algoritmasını gerektiği ndaki bir forma veri almak için bazı işleme vermedi. Web hizmeti aracılığıyla almaya beklediğiniz verileri bu işlem gerekli değildir, atlayabilirsiniz: çıkışını **Web hizmeti giriş Modülü** denemenizi içinde farklı bir modül için. Kullanıcı verileri, artık bu konumda modelinde ulaşırsınız.

Örneğin, varsayılan olarak **Web hizmetinin ayarı** koyar **Web hizmeti giriş** Yukarıdaki şekilde gösterildiği gibi veri akışı üst kısmındaki modülü. Ancak biz elle konumlandırabilirsiniz **Web hizmeti giriş** veri işleme modülleri geçmiş:

![Web hizmeti girişini taşıma](./media/convert-training-experiment-to-scoring-experiment/figure4.png)

Tüm ön işleme olmadan web hizmeti aracılığıyla sağlanan giriş verilerini artık doğrudan Score Model modüle geçirin.

Benzer şekilde, varsayılan olarak **Web hizmetinin ayarı** Web Hizmetleri çıkış modülü, veri akışı alt kısmındaki koyar. Bu örnekte, web hizmetinin çıktısını kullanıcıya döndürür [Score Model] [ score-model] modülü eksiksiz bir giriş veri vektör artı Puanlama sonuçlarını içerir.
Farklı bir döndürülecek tercih ederseniz, ancak daha sonra önce ek modüller ekleyebilirsiniz **Web hizmeti çıkış** modülü. 

Örneğin, yalnızca Puanlama sonuçları ve giriş verileri değil tüm vektörü döndürmek için ekleme bir [kümesindeki sütunları seçme] [ select-columns] Puanlama sonuçları dışındaki tüm sütunları dışlamak için modülü. Ettirin **Web hizmeti çıkış** modülünün çıkışını [kümesindeki sütunları seçme] [ select-columns] modülü. Denemeyi şöyle görünür:

![Web hizmeti çıkış taşıma](./media/convert-training-experiment-to-scoring-experiment/figure5.png)

### <a name="add-or-remove-additional-data-processing-modules"></a>Ek veri işleme modülleri Ekle Kaldır
Puanlama sırasında ihtiyaç bildiğiniz denemenizi daha fazla modülleri varsa bunlar kaldırılabilir. Örneğin, geçtiğimizi çünkü **Web hizmeti giriş** modülü bir noktadan sonra veri işleme modüller için biz kaldırabilirsiniz [eksik verileri temizleme] [ clean-missing-data] modülünden Tahmine dayalı denemeye.

Bizim Tahmine dayalı denemeye artık şöyle görünür:

![Ek modülü kaldırılıyor](./media/convert-training-experiment-to-scoring-experiment/figure6.png)


### <a name="add-optional-web-service-parameters"></a>İsteğe bağlı Web hizmeti parametrelerini Ekle
Bazı durumlarda, kullanıcının web hizmetinizin hizmet erişildiğinde modülleri davranışını değiştirmesine izin ver isteyebilirsiniz. *Web hizmeti parametreleri* bunu yapmanıza olanak sağlar.

Yaygın olarak karşılaşılan örneklerden ayarlama bir [verileri içeri aktarma] [ import-data] web hizmeti erişim sağlandığında dağıtılan web hizmeti kullanıcı farklı bir veri kaynağına belirtebilmeniz modülü. Veya yapılandırma bir [verileri dışarı aktarma] [ export-data] modülü böylece farklı bir hedef belirtilebilir.

Web hizmeti parametrelerini tanımlayın ve bunları bir veya daha fazla modül parametrelerini ile ilişkilendirin ve bunlar gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz. Web hizmeti kullanıcı hizmete erişme ve modül işlemleri uygun şekilde değiştirilir Bu parametreler için değerler sağlar.

Web hizmeti parametrelerini nelerdir ve bunların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Machine Learning Web hizmeti parametrelerini kullanma][webserviceparameters].

[webserviceparameters]: web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Tahmine dayalı denemeye bir web hizmeti olarak dağıtma
Tahmine dayalı denemeye yeterince hazırlandığından, bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Tam dağıtım işlemi hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning web hizmetini dağıtma][deploy]

[deploy]: publish-a-machine-learning-web-service.md

<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
