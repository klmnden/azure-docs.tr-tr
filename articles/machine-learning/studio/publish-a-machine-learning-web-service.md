---
title: Bir Machine Learning Studio web hizmeti dağıtma
titleSuffix: Azure Machine Learning Studio
description: Bir eğitim denemesini öngörücü bir denemeye dönüştürme, dağıtım için hazırlamak ve ardından bir Azure Machine Learning Studio web hizmeti olarak dağıtma nasıl.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 01/06/2017
ms.openlocfilehash: 2ffc9055f23b8221a6f711f741b6146545ff0821
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60334038"
---
# <a name="deploy-an-azure-machine-learning-studio-web-service"></a>Bir Azure Machine Learning Studio web hizmetini dağıtma

Azure Machine Learning Studio Tahmine dayalı bir analiz çözümü test etmek ve oluşturmanıza olanak sağlar. Ardından, çözümü bir web hizmeti olarak dağıtabilirsiniz.

Machine Learning Studio web Hizmetleri, uygulama ile Machine Learning Studio iş akışı Puanlama modeli arasında bir arabirim sağlar. Bir dış uygulama, Machine Learning Studio'da bir iş akışı Puanlama modeli ile gerçek zamanlı iletişim kurabilir. Machine Learning Studio web hizmeti çağrısı bir dış uygulamaya tahmin sonuçlarını döndürür. Bir web hizmetine çağrı yapmak için web hizmeti dağıtılırken oluşturulan bir API anahtarını geçirirsiniz. Bir Machine Learning Studio web hizmeti bir web programlama projeleri için popüler bir mimari seçimi olan REST'i temel alır.

Azure Machine Learning Studio web hizmetleri iki tür vardır:

* İstek-yanıt hizmeti (RRS): Düşük gecikme süreli, bir tek veri kaydı puanlar yüksek oranda ölçeklenebilir bir hizmettir.
* Toplu yürütme hizmeti (BES): Zaman uyumsuz bir hizmettir, toplu veri kayıtlarının.

BES girişi, RRS’nin kullandığı veri girişi gibidir. Aralarındaki temel fark, BES'nin Azure Blob depolama, Azure Tablo depolama, Azure SQL Veritabanı, HDInsight (hive sorgusu) ve HTTP kaynakları gibi çeşitli kaynaklardan bir kayıt bloğunu okumasıdır.

Bir üst düzey noktası-ın-görünümünden modelinizi üç adımda dağıtın:

* **[Eğitim denemenizi oluşturma]**  -Studio'da, eğitin ve test çok sayıda yerleşik makine öğrenimi algoritmalarını kullanarak, sağladığınız eğitim verilerini kullanarak bir Tahmine dayalı analiz modeli.
* **[Öngörücü bir denemeye dönüştürme]**  -modelinizi mevcut verilerle eğitim almış ve yeni verileri puanlamak için kullanıma hazır sonra hazırlama ve tahminler elde etmek için denemenizin kolaylaştırın.
* **Dağıtma** olarak bir **[yeni web hizmeti]** veya **[Klasik web hizmeti]** - deneyiminizi Tahmine dayalı olarak dağıttığınızda bir Azure web hizmeti, kullanıcıları, modelinize veri göndermek ve modelinizin Öngörüler alırlar.

## <a name="create-a-training-experiment"></a>Eğitim denemenizi oluşturma

Tahmine dayalı analiz modeli eğitmek için Azure Machine Learning Studio eğitim denemesini oluşturmak için nereden eğitim veri yükleme, gerektiğinde verileri hazırlama, makine öğrenimi algoritmaları uygulamak ve sonuçları değerlendirmek için çeşitli modül dahil kullanın. Bir deney üzerinde yinelemek ve karşılaştırmak ve sonuçları değerlendirmek için farklı makine öğrenimi algoritmaları deneyin.

Oluşturma ve eğitim denemeleri yönetme işlemini başka bir yerde daha kapsamlı olarak ele alınmıştır. Daha fazla bilgi için şu makalelere bakın:

* [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md)
* [Azure Machine Learning Studio'da öngörülebilir bir çözüm geliştirin](tutorial-part1-credit-risk.md)
* [Eğitim verilerinizi Azure Machine Learning Studio'ya alma](import-data.md)
* [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme

Kullanarak modeli eğittiğimize sonra yeni verileri puanlamak için Tahmine dayalı bir denemenin eğitim denemenizi dönüştürmek hazır olursunuz.

Öngörücü bir denemeye dönüştürme tarafından eğitilen model Puanlama web hizmeti olarak dağıtılması hazırlandığınızı. Kullanıcılar web hizmeti giriş verileri modelinize gönderebilir ve modelinizi tahmin sonuçlarını geri gönderir. Öngörücü bir denemeye dönüştürme gibi başkaları tarafından kullanılmak üzere modelinizi nasıl beklediğiniz göz önünde bulundurun.

Eğitim denemesini öngörücü bir denemeye dönüştürme için tıklatın **çalıştırma** deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı**, ardından **Tahmine dayalı Web hizmeti**.

![Deneme puanlamaya dönüştürme](./media/publish-a-machine-learning-web-service/figure-1.png)

Bu dönüşüm gerçekleştirme hakkında daha fazla bilgi için bkz. [modelinizin Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

Aşağıdaki adımlar, bir Tahmine dayalı denemeye yeni bir web hizmeti olarak dağıtma açıklar. Bu gibi durumlarda, denemeler de klasik web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-it-as-a-new-web-service"></a>Yeni bir web hizmeti olarak dağıtma

Tahmine dayalı denemeye hazır olduktan sonra yeni (Resource Manager tabanlı), Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırma** deneme tuvalinin altındaki. Denemeyi çalıştırma tamamlandıktan sonra tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [yeni]**.  Machine Learning Studio Web hizmeti Portalı'nın dağıtım sayfası açılır.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi edinmek, [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md). 

### <a name="machine-learning-studio-web-service-portal-deploy-experiment-page"></a>Machine Learning Studio Web hizmeti Portalı'nı dağıtma deneme sayfası

Deneme dağıtma sayfasında, web hizmeti için bir ad girin.
Fiyatlandırma planı seçin. Aksi takdirde, seçebileceğiniz bir devrenin fiyatlandırma planını varsa, hizmet için yeni bir fiyat planı oluşturmanız gerekir.

1. İçinde **fiyat planı** açılan menü, var olan bir planı veya seçin **Select yeni plan** seçeneği.
2. İçinde **planı adı**, faturanızda planı tanımlayacak bir ad yazın.
3. Birini **aylık Plan katmanları**. Hizmetinizi varsayılan bölgeden ve web hizmetiniz için planlar plan katmanları varsayılan bu bölgeye dağıtılır.

Tıklayın **Dağıt** ve **hızlı** web hizmetiniz için sayfası açılır.

Web hizmeti hızlı başlangıç sayfası bir web hizmeti oluşturduktan sonra gerçekleştireceğiniz en yaygın görevleri erişim ve rehberlik sunar. Burada Test sayfası ve kullanım sayfası kolayca erişebilirsiniz.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-new-web-service"></a>Yeni web hizmetinizi test etme

Yeni web hizmetini test etmek için **Test web hizmeti** ortak görevleri altında. Test sayfasında, web hizmeti bir istek-yanıt hizmeti'olarak (RRS) veya toplu iş yürütme hizmeti (BES) test edebilirsiniz.

Girişler, çıkışlar ve deneme için tanımladığınız herhangi bir genel parametre RRS test sayfası görüntüler. Web hizmeti test etmek için el ile girişleri için uygun değerleri girebilir veya test değerleri içeren bir virgülle ayrılmış değer (CSV) biçimlendirilmiş bir dosya sağlayın.

RRS kullanarak test etmek için liste görünümü modundan girişleri için uygun değerleri girin ve **Test istek-yanıt**. Çıkış sütununu sola, tahmin sonuçlarını görüntüler.

![Web hizmetini test etmek için uygun değerleri girin](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

BES, test etmek için **Batch**. Toplu test sayfasında girişinizi altında Gözat'a tıklayın ve uygun örnek değerleri içeren bir CSV dosyası seçin. Bir CSV dosyası yoksa ve Machine Learning Studio'yu kullanarak Tahmine dayalı denemenizi oluşturduğunuz, Tahmine dayalı denemeniz için veri kümesini indirin ve kullanın.

Veri kümesi indirmek için Machine Learning Studio'da açın. Tahmine dayalı denemenizi açın ve denemeniz için giriş sağ tıklayın. Bağlam menüsünden seçin **veri kümesi** seçip **indirme**.

![Veri kümeniz Studio tuvalden indirin](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Tıklayın **Test**. Toplu iş yürütmeye ilişkin iş durumunu görüntüler altında sağındaki **Test toplu işleri**.

![Web hizmeti portalı ile toplu iş yürütme işinizi test etme](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

Üzerinde **yapılandırma** sayfası, başlığı açıklamayı değiştirmek, depolama hesabı anahtarını güncelleştirmek ve web hizmetiniz için örnek verileri etkinleştirin.

![Web hizmetinizi yapılandırın](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

### <a name="access-your-new-web-service"></a>Yeni, web hizmetine erişme

Machine Learning Studio'dan web hizmetini dağıttığınızda, hizmete veri gönderebilir ve program aracılığıyla yanıtlar almasına.

**Tüket** sayfası, web hizmetine erişmek için gereksinim duyduğunuz tüm bilgileri sağlar. Örneğin, hizmet yetkili erişime izin vermek için API anahtarı sağlanır.

Machine Learning Studio web hizmetine erişim hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Studio Web hizmetinin nasıl kullanılacağı hakkında](consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Yeni web hizmetinizi yönetin

Yeni web hizmetleri Machine Learning Studio Web Hizmetleri portalınızı yönetebilirsiniz. Gelen [ana portal sayfasına](https://services.azureml-test.net/), tıklayın **Web Hizmetleri**. Web Hizmetleri sayfasından silin veya hizmet kopyalayın. Belirli bir hizmeti izlemek için hizmete tıklayın ve ardından **Pano**. Web hizmeti ile ilişkilendirilen toplu işleri izlemek için tıklayın **Batch istek günlüğü**.

### <a id="multi-region"></a> Yeni web hizmetini birden fazla bölgeye dağıtın.

Yeni bir web hizmeti için birden çok bölgede birden fazla abonelik veya çalışma alanları gerek kalmadan kolayca dağıtabilirsiniz.

Web hizmeti dağıtacağınız her bölge için bir faturalama planını tanımlamanız gereken fiyatlandırma bölgeye özeldir, olduğundan.

#### <a name="create-a-plan-in-another-region"></a>Başka bir bölgede bir plan oluşturun

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/).
2. Tıklayın **planları** menü seçeneği.
3. Görünüm sayfası üzerinden planlarında tıklayın **yeni**.
4. Gelen **abonelik** açılır listesinde, yeni plan bulunacağı aboneliği seçin.
5. Gelen **bölge** açılır listesinde, yeni plan için bölge seçin. Seçilen bölge için Plan seçenekleri görüntüler **planlama seçenekleri** sayfasının bölümünde.
6. Gelen **kaynak grubu** bir kaynak grubu için plan açılır penceresinde seçin. Kaynak grupları hakkında daha fazla bilgi [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).
7. İçinde **planı adı** planın adını yazın.
8. Altında **planı seçenekleri**, fatura yeni plan için tıklayın.
9. **Oluştur**’a tıklayın.

#### <a name="deploy-the-web-service-to-another-region"></a>Başka bir bölgeye web hizmetini dağıtma

1. Microsoft Azure Machine Learning Web Hizmetleri sayfasında tıklayın **Web Hizmetleri** menü seçeneği.
2. Yeni bir bölgeye dağıttığınız Web hizmeti seçin.
3. Tıklayın **kopyalama**.
4. İçinde **Web hizmeti adı**, web hizmeti için yeni bir ad yazın.
5. İçinde **Web hizmeti açıklaması**, web hizmeti için bir açıklama yazın.
6. Gelen **abonelik** açılır listesinde, yeni bir web hizmeti bulunacağı aboneliği seçin.
7. Gelen **kaynak grubu** bir kaynak grubu web hizmeti için açılır penceresinde seçin. Kaynak grupları hakkında daha fazla bilgi [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).
8. Gelen **bölge** açılır listesinde, web hizmeti dağıtacağınız bölgeyi seçin.
9. Gelen **depolama hesabı** açılır listesinde, bir depolama hesabı web hizmeti depolamak için.
10. Gelen **fiyat planı** açılır listesinde, 8. adımda seçtiğiniz bölgede planı seçin.
11. Tıklayın **kopyalama**.

## <a name="deploy-it-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtma

Tahmine dayalı denemeye yeterince hazırlanan, Azure Klasik web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar, modelinize veri gönderebilir ve modeli, Öngörüler döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırma** denemeyi alt kısmındaki tuval ve ardından **Web hizmeti Dağıt**. Web hizmeti ayarlayın ve web hizmeti panosunda yer alır.

![Web hizmetinizi Studio'dan dağıtma](./media/publish-a-machine-learning-web-service/figure-2.png)

### <a name="test-your-classic-web-service"></a>Klasik web hizmetini test edin

Machine Learning Studio Web Hizmetleri portalından veya Machine Learning Studio web hizmeti test edebilirsiniz.

İstek yanıtı web hizmeti test etmek için **Test** web hizmeti panosundaki düğmesi. Hizmeti giriş verileri için soran bir iletişim kutusu açılır. Puanlama deney tarafından beklenen sütunlar şunlardır. Bir veri kümesini girin ve ardından **Tamam**. Web hizmeti tarafından oluşturulan sonuçlar, panonun alt kısmında görüntülenir.

Tıklayabilirsiniz **Test** Önizleme bağlantı, daha önce yeni web hizmeti bölümünde gösterildiği gibi Azure Machine Learning Studio Web Hizmetleri portalında, hizmeti test etmek için.

Toplu yürütme hizmeti test etmek için **Test** Önizleme bağlantı. Toplu test sayfasında girişinizi altında Gözat'a tıklayın ve uygun örnek değerleri içeren bir CSV dosyası seçin. Bir CSV dosyası yoksa ve Machine Learning Studio'yu kullanarak Tahmine dayalı denemenizi oluşturduğunuz, Tahmine dayalı denemeniz için veri kümesini indirin ve kullanın.

![Web hizmetini test edin](./media/publish-a-machine-learning-web-service/figure-3.png)

Üzerinde **yapılandırma** sayfasında, hizmet görünen adını değiştirme ve bir açıklama girin. Ad ve Açıklama bölümünde görüntülenir [Azure portalında](https://portal.azure.com/) web hizmetlerinizi yönettiği yerdir.

Bir açıklama için girdi verilerini, çıktı verilerini ve web hizmeti parametreleri altındaki her sütun için bir dize girerek sağlayabilirsiniz **giriş ŞEMASINI**, **çıkış ŞEMASI**, ve **Web hizmeti PARAMETRE**. Bu açıklamalar web hizmeti için sağlanan örnek kod belgelerinde kullanılır.

Web hizmetinizi erişildiğinde sizin gördüğünüz tüm hataları tanılamak günlük kaydını etkinleştirebilirsiniz. Daha fazla bilgi için [Machine Learning Studio web hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

![Web hizmetleri portalında oturum açmayı etkinleştir](./media/publish-a-machine-learning-web-service/figure-4.png)

Azure Machine Learning Web Hizmetleri portalında benzer şekilde yeni web hizmeti bölümünde daha önce gösterilen yordam, web hizmeti uç noktaları da yapılandırabilirsiniz. Seçenekler farklıdır, ekleyebilir veya hizmet açıklaması, günlüğü kaydetmeyi etkinleştir ve test etmek için örnek verileri etkinleştir değiştirin.

### <a name="access-your-classic-web-service"></a>Klasik web hizmeti erişim

Machine Learning Studio'dan web hizmetini dağıttığınızda, hizmete veri gönderebilir ve program aracılığıyla yanıtlar almasına.

Pano, web hizmetine erişmek gereken tüm bilgileri sağlar. Örneğin, API anahtarını hizmet yetkili erişime izin vermek için sağlanır ve API Yardım sayfaları yardımcı olması için sağlanmıştır, kod yazmaya başlayın.

Machine Learning Studio web hizmetine erişim hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Studio Web hizmetinin nasıl kullanılacağı hakkında](consume-web-services.md).

### <a name="manage-your-classic-web-service"></a>Klasik web hizmetini yönetme

Çeşitli eylemler gerçekleştirebilirsiniz vardır bir web hizmetini izlemek için. Güncelleştirin ve silin. Ayrıca, bir Klasik web hizmeti, dağıtım sırasında oluşturulan varsayılan uç nokta yanı sıra ek uç noktalar ekleyebilirsiniz.

Daha fazla bilgi için [bir Azure Machine Learning Studio çalışma alanını yönetme](manage-workspace.md) ve [Azure Machine Learning Studio Web Hizmetleri portalını kullanarak bir web hizmetini yönetme](manage-new-webservice.md).

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirmek
Ek eğitim verilerle bir modeli güncelleştirme gibi web hizmetini, değişiklik ve yeniden, özgün web hizmeti üzerine dağıtın.

Web hizmetini güncelleştirmek için web hizmetini dağıtma ve tıklayarak düzenlenebilir bir kopyasını oluşturmak için kullanılan özgün Tahmine dayalı denemeye açın **SAVE AS**. Değişikliklerinizi yapın ve ardından **Web hizmeti Dağıt**.

Bu deneyde önce dağıttınız olduğundan (Klasik Web hizmeti) üzerine veya (Yeni web hizmeti) hizmetini güncelleştirmek istiyorsanız istenir. Tıklayarak **Evet** veya **güncelleştirme** mevcut web hizmetini durdurur ve yeni dağıtır Tahmine dayalı denemeye, onun yerine dağıtılır.

> [!NOTE]
> Özgün web hizmetinde yapılan yapılandırma değişiklikleri, örneğin, bir yeni görünen adını veya açıklamasını girme, bu değerleri yeniden girmeniz gerekir.

Web hizmetini güncelleştirmek için bir model programlama yoluyla yeniden eğitme seçenektir. Daha fazla bilgi için [modellerini programlama aracılığıyla yeniden eğitme Machine Learning Studio](/azure/machine-learning/studio/retrain-machine-learning-model).

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım birlikte nasıl çalıştığı hakkında daha fazla teknik ayrıntılar için bkz [çalışır hale getirilen Web hizmetine bir denemeyi nasıl bir Machine Learning Studio'da model ilerler](model-progression-experiment-to-web-service.md).

* Modelinizi dağıtmaya hazırlanma konusunda daha fazla ayrıntı için bkz. [modelinizin Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

* REST API’sini kullanmanın ve web hizmetine erişmenin birkaç yolu vardır. Bkz: [bir Azure Machine Learning Studio web hizmetinin nasıl kullanılacağı hakkında](consume-web-services.md).

<!-- internal links -->
[Eğitim denemenizi oluşturma]: #create-a-training-experiment
[Öngörücü bir denemeye dönüştürme]: #convert-the-training-experiment-to-a-predictive-experiment
[Yeni web hizmeti]: #deploy-it-as-a-new-web-service
[Klasik web hizmeti]: #deploy-it-as-a-classic-web-service
[Yeni]: #deploy-it-as-a-new-web-service
[classic]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
