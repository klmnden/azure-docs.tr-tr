---
title: Bir Machine Learning Studio web hizmeti - Azure'ı dağıtma | Microsoft Docs
description: Bir eğitim denemesini öngörücü bir denemeye dönüştürme, dağıtım için hazırlamak ve ardından bir Azure Machine Learning Studio web hizmeti olarak dağıtma nasıl.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.openlocfilehash: d96755f00aa5023d57c9c4c2b2457902c337e29d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314277"
---
# <a name="deploy-an-azure-machine-learning-studio-web-service"></a>Bir Azure Machine Learning Studio web hizmetini dağıtma
Azure Machine Learning, derleme, test etme ve Tahmine dayalı analiz çözümlerini dağıtmak sağlar.

Bir üst düzey noktası-ın-görünümünden, bu üç adımda gerçekleştirilir:

* **[Eğitim denemenizi oluşturma]**  -Azure Machine Learning Studio olan eğitmek ve sağladığınız eğitim verilerini kullanarak bir Tahmine dayalı analiz modeli test etmek için kullandığınız bir işbirliğine dayalı visual geliştirme ortamı.
* **[Öngörücü bir denemeye dönüştürme]**  -modelinizi mevcut verilerle eğitim almış ve yeni verileri puanlamak için kullanıma hazır sonra hazırlama ve tahminler elde etmek için denemenizin kolaylaştırın.
* **[Bir web hizmeti olarak dağıtma]**  -deneyiminizi Tahmine dayalı olarak dağıtabileceğiniz bir [yeni] veya [Klasik] Azure web hizmeti. Kullanıcılar, modelinize veri göndermek ve modelinizin Öngörüler alırsınız.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Eğitim denemenizi oluşturma
Tahmine dayalı analiz modeli eğitmek için Azure Machine Learning Studio eğitim denemesini oluşturmak için nereden eğitim veri yükleme, gerektiğinde verileri hazırlama, makine öğrenimi algoritmaları uygulamak ve sonuçları değerlendirmek için çeşitli modül dahil kullanın. Bir deney üzerinde yinelemek ve karşılaştırmak ve sonuçları değerlendirmek için farklı makine öğrenimi algoritmaları deneyin.

Oluşturma ve eğitim denemeleri yönetme işlemini başka bir yerde daha kapsamlı olarak ele alınmıştır. Daha fazla bilgi için şu makalelere bakın:

* [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md)
* [Azure Machine Learning ile Tahmine dayalı bir çözüm geliştirin](walkthrough-develop-predictive-solution.md)
* [Eğitim verilerinizi Azure Machine Learning Studio'ya alma](import-data.md)
* [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme
Kullanarak modeli eğittiğimize sonra yeni verileri puanlamak için Tahmine dayalı bir denemenin eğitim denemenizi dönüştürmek hazır olursunuz.

Öngörücü bir denemeye dönüştürme tarafından eğitilen model Puanlama web hizmeti olarak dağıtılması hazırlandığınızı. Kullanıcılar web hizmeti giriş verileri modelinize gönderebilir ve modelinizi tahmin sonuçlarını geri gönderir. Öngörücü bir denemeye dönüştürme gibi başkaları tarafından kullanılmak üzere modelinizi nasıl beklediğiniz göz önünde bulundurun.

Eğitim denemesini öngörücü bir denemeye dönüştürme için tıklatın **çalıştırma** deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı**, ardından **Tahmine dayalı Web hizmeti**.

![Deneme puanlamaya dönüştürme](./media/publish-a-machine-learning-web-service/figure-1.png)

Bu dönüşüm gerçekleştirme hakkında daha fazla bilgi için bkz. [modelinizin Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

Aşağıdaki adımlar, bir Tahmine dayalı denemeye yeni bir web hizmeti olarak dağıtma açıklar. Bu gibi durumlarda, denemeler de klasik web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-it-as-a-web-service"></a>Bir web hizmeti olarak dağıtma

Tahmine dayalı denemeye yeni bir web hizmeti olarak veya bir Klasik web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Tahmine dayalı denemeye yeni bir web hizmeti olarak dağıtma
Tahmine dayalı denemeye hazır olduktan sonra yeni bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırma** deneme tuvalinin altındaki. Denemeyi çalıştırma tamamlandıktan sonra tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [yeni]**.  Machine Learning Web hizmeti Portalı'nın dağıtım sayfası açılır.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning Web hizmeti portalı Dağıt deneme sayfası
Deneme dağıtma sayfasında, web hizmeti için bir ad girin.
Fiyatlandırma planı seçin. Aksi takdirde, seçebileceğiniz bir devrenin fiyatlandırma planını varsa, hizmet için yeni bir fiyat planı oluşturmanız gerekir.

1. İçinde **fiyat planı** açılan menü, var olan bir planı veya seçin **Select yeni plan** seçeneği.
2. İçinde **planı adı**, faturanızda planı tanımlayacak bir ad yazın.
3. Birini **aylık Plan katmanları**. Hizmetinizi varsayılan bölgeden ve web hizmetiniz için planlar plan katmanları varsayılan bu bölgeye dağıtılır.

Tıklayın **Dağıt** ve **hızlı** web hizmetiniz için sayfası açılır.

Web hizmeti hızlı başlangıç sayfası bir web hizmeti oluşturduktan sonra gerçekleştireceğiniz en yaygın görevleri erişim ve rehberlik sunar. Burada Test sayfası ve kullanım sayfası kolayca erişebilirsiniz.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Yeni web hizmetinizi test etme
Yeni web hizmetini test etmek için **Test web hizmeti** ortak görevleri altında. Test sayfasında, web hizmeti bir istek-yanıt hizmeti'olarak (RRS) veya toplu iş yürütme hizmeti (BES) test edebilirsiniz.

Girişler, çıkışlar ve deneme için tanımladığınız herhangi bir genel parametre RRS test sayfası görüntüler. Web hizmeti test etmek için el ile girişleri için uygun değerleri girebilir veya test değerleri içeren bir virgülle ayrılmış değer (CSV) biçimlendirilmiş bir dosya sağlayın.

RRS kullanarak test etmek için liste görünümü modundan girişleri için uygun değerleri girin ve **Test istek-yanıt**. Çıkış sütununu sola, tahmin sonuçlarını görüntüler.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

BES, test etmek için **Batch**. Toplu test sayfasında girişinizi altında Gözat'a tıklayın ve uygun örnek değerleri içeren bir CSV dosyası seçin. Bir CSV dosyası yoksa ve Machine Learning Studio'yu kullanarak Tahmine dayalı denemenizi oluşturduğunuz, Tahmine dayalı denemeniz için veri kümesini indirin ve kullanın.

Veri kümesi indirmek için Machine Learning Studio'da açın. Tahmine dayalı denemenizi açın ve denemeniz için giriş sağ tıklayın. Bağlam menüsünden seçin **veri kümesi** seçip **indirme**.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Tıklayın **Test**. Toplu iş yürütmeye ilişkin iş durumunu görüntüler altında sağındaki **Test toplu işleri**.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

Üzerinde **yapılandırma** sayfası, başlığı açıklamayı değiştirmek, depolama hesabı anahtarını güncelleştirmek ve web hizmetiniz için örnek verileri etkinleştirin.

![Web hizmetini yapılandır](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Web hizmeti dağıttıktan sonra şunları yapabilirsiniz:

* **Erişim** web hizmeti API'si üzerinden.
* **Yönetme** , Azure Machine Learning web hizmetleri portalından.
* **Güncelleştirme** , modelinizi değişirse.

#### <a name="access-your-new-web-service"></a>Yeni, web hizmetine erişme
Machine Learning Studio'dan web hizmetini dağıttığınızda, hizmete veri gönderebilir ve program aracılığıyla yanıtlar almasına.

**Tüket** sayfası, web hizmetine erişmek için gereksinim duyduğunuz tüm bilgileri sağlar. Örneğin, hizmet yetkili erişime izin vermek için API anahtarı sağlanır.

Machine Learning web hizmetine erişim hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Yeni web hizmetinizi yönetin
Yeni web hizmetleri Machine Learning Web Hizmetleri portalınızı yönetebilirsiniz. Gelen [ana portal sayfasına](https://services.azureml-test.net/), tıklayın **Web Hizmetleri**. Web Hizmetleri sayfasından silin veya hizmet kopyalayın. Belirli bir hizmeti izlemek için hizmete tıklayın ve ardından **Pano**. Web hizmeti ile ilişkilendirilen toplu işleri izlemek için tıklayın **Batch istek günlüğü**.

### <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Tahmine dayalı denemeye Klasik web hizmeti olarak dağıtma

Tahmine dayalı denemeye yeterince hazırlanan, Azure Klasik web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırma** denemeyi alt kısmındaki tuval ve ardından **Web hizmeti Dağıt**. Web hizmeti ayarlayın ve web hizmeti panosunda yer alır.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Klasik web hizmetini test edin

Machine Learning Web Hizmetleri portalından veya Machine Learning Studio web hizmeti test edebilirsiniz.

İstek yanıtı web hizmeti test etmek için **Test** web hizmeti panosundaki düğmesi. Hizmeti giriş verileri için soran bir iletişim kutusu açılır. Puanlama deney tarafından beklenen sütunlar şunlardır. Bir veri kümesini girin ve ardından **Tamam**. Web hizmeti tarafından oluşturulan sonuçlar, panonun alt kısmında görüntülenir.

Tıklayabilirsiniz **Test** Önizleme bağlantı, daha önce yeni web hizmeti bölümünde gösterildiği gibi Azure Machine Learning Web Hizmetleri portalında, hizmeti test etmek için.

Toplu yürütme hizmeti test etmek için **Test** Önizleme bağlantı. Toplu test sayfasında girişinizi altında Gözat'a tıklayın ve uygun örnek değerleri içeren bir CSV dosyası seçin. Bir CSV dosyası yoksa ve Machine Learning Studio'yu kullanarak Tahmine dayalı denemenizi oluşturduğunuz, Tahmine dayalı denemeniz için veri kümesini indirin ve kullanın.

![Web hizmetini test edin](./media/publish-a-machine-learning-web-service/figure-3.png)

Üzerinde **yapılandırma** sayfasında, hizmet görünen adını değiştirme ve bir açıklama girin. Ad ve Açıklama bölümünde görüntülenir [Azure portalında](https://portal.azure.com/) web hizmetlerinizi yönettiği yerdir.

Bir açıklama için girdi verilerini, çıktı verilerini ve web hizmeti parametreleri altındaki her sütun için bir dize girerek sağlayabilirsiniz **giriş ŞEMASINI**, **çıkış ŞEMASI**, ve **Web hizmeti PARAMETRE**. Bu açıklamalar web hizmeti için sağlanan örnek kod belgelerinde kullanılır.

Web hizmetinizi erişildiğinde sizin gördüğünüz tüm hataları tanılamak günlük kaydını etkinleştirebilirsiniz. Daha fazla bilgi için [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

![Web hizmetini yapılandır](./media/publish-a-machine-learning-web-service/figure-4.png)

Azure Machine Learning Web Hizmetleri portalında benzer şekilde yeni web hizmeti bölümünde daha önce gösterilen yordam, web hizmeti uç noktaları da yapılandırabilirsiniz. Seçenekler farklıdır, ekleyebilir veya hizmet açıklaması, günlüğü kaydetmeyi etkinleştir ve test etmek için örnek verileri etkinleştir değiştirin.

#### <a name="access-your-classic-web-service"></a>Klasik web hizmeti erişim
Machine Learning Studio'dan web hizmetini dağıttığınızda, hizmete veri gönderebilir ve program aracılığıyla yanıtlar almasına.

Pano, web hizmetine erişmek gereken tüm bilgileri sağlar. Örneğin, API anahtarını hizmet yetkili erişime izin vermek için sağlanır ve API Yardım sayfaları yardımcı olması için sağlanmıştır, kod yazmaya başlayın.

Machine Learning web hizmetine erişim hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Klasik web hizmetini yönetme
Çeşitli eylemler gerçekleştirebilirsiniz vardır bir web hizmetini izlemek için. Güncelleştirin ve silin. Ayrıca, bir Klasik web hizmeti, dağıtım sırasında oluşturulan varsayılan uç nokta yanı sıra ek uç noktalar ekleyebilirsiniz.

Daha fazla bilgi için [bir Azure Machine Learning çalışma alanını yönetme](manage-workspace.md) ve [Azure Machine Learning Web Hizmetleri portalını kullanarak bir web hizmetini yönetme](manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek
Ek eğitim verilerle bir modeli güncelleştirme gibi web hizmetini, değişiklik ve yeniden, özgün web hizmeti üzerine dağıtın.

Web hizmetini güncelleştirmek için web hizmetini dağıtma ve tıklayarak düzenlenebilir bir kopyasını oluşturmak için kullanılan özgün Tahmine dayalı denemeye açın **SAVE AS**. Değişikliklerinizi yapın ve ardından **Web hizmeti Dağıt**.

Bu deneyde önce dağıttınız olduğundan (Klasik Web hizmeti) üzerine veya (Yeni web hizmeti) hizmetini güncelleştirmek istiyorsanız istenir. Tıklayarak **Evet** veya **güncelleştirme** mevcut web hizmetini durdurur ve yeni dağıtır Tahmine dayalı denemeye, onun yerine dağıtılır.

> [!NOTE]
> Özgün web hizmetinde yapılan yapılandırma değişiklikleri, örneğin, bir yeni görünen adını veya açıklamasını girme, bu değerleri yeniden girmeniz gerekir.
> 
> 

Web hizmetini güncelleştirmek için bir model programlama yoluyla yeniden eğitme seçenektir. Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](retrain-models-programmatically.md).

<!-- internal links -->
[Eğitim denemenizi oluşturma]: #create-a-training-experiment
[Öngörücü bir denemeye dönüştürme]: #convert-the-training-experiment-to-a-predictive-experiment
[Bir web hizmeti olarak dağıtma]: #deploy-it-as-a-web-service
[Yeni]: #deploy-the-predictive-experiment-as-a-new-web-service
[Klasik]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
