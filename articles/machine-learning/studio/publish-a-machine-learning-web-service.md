---
title: "Machine Learning web hizmetini dağıtma | Microsoft Docs"
description: "Tahmine dayalı bir deneme eğitim denemenizi dönüştürmek, dağıtım için hazırlanma, sonra bir Azure Machine Learning web hizmeti olarak dağıtabilir nasıl."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 73a3e9c6-00d0-41d4-8cf1-2ec87713867e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: bdf0bd54130521a7178af3a28731f4c0e21e3e0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-azure-machine-learning-web-service"></a>Azure Machine Learning web hizmeti dağıtma
Azure Machine Learning, derleme, test ve Tahmine dayalı analitik çözümleri dağıtmanızı sağlar.

Bir üst düzey noktası-in-görünümden, bu üç adımda gerçekleştirilir:

* **[Eğitim denemenizi oluşturma]**  -Azure Machine Learning Studio olduğu eğitmek ve sağladığınız eğitim verileri kullanarak bir Tahmine dayalı bir analiz modeli test etmek için kullandığınız bir işbirliğine dayalı görsel geliştirme ortamı.
* **[Tahmine dayalı bir deneme Dönüştür]**  -modelinizi var olan verilerle eğitilmiş ve yeni verilerinizi puanlamada için kullanıma hazır sonra hazırlamak ve denemenizi Öngörüler için kolaylaştırır.
* **[Web hizmeti olarak dağıtabilir]**  -Tahmine dayalı denemenizi olarak dağıttığınız bir [yeni] veya [Klasik] Azure web hizmeti. Kullanıcıların modelinize veri gönderebilir ve modelinizin tahminleri alabilirsiniz.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Eğitim denemenizi oluşturma
Tahmine dayalı bir analiz modeli eğitmek için Azure Machine Learning Studio'da eğitim denemenizi oluşturmak için eğitim verileri yüklemek, verileri gereken şekilde hazırlama, machine learning algoritmaları uygulamak ve sonuçları değerlendirin çeşitli modülleri dahil olduğu kullanın. Bir deneme üzerinde yinelemek ve karşılaştırmak ve sonuçları değerlendirmek için başka bir makine öğrenimi algoritmalarını deneyin.

Oluşturma ve eğitim denemeler yönetme sürecini daha kapsamlı başka bir yere ele alınmıştır. Daha fazla bilgi için bu makalelere bakın:

* [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md)
* [Azure Machine Learning ile Tahmine dayalı bir çözüm geliştirme](walkthrough-develop-predictive-solution.md)
* [Azure Machine Learning Studio'ya eğitim verilerinizi alın](import-data.md)
* [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Tahmine dayalı bir deneme eğitim denemenizi Dönüştür
Modelinizi eğittiğimize sonra yeni verilerinizi puanlamada için Tahmine dayalı bir deneme eğitim denemenizi dönüştürmek hazırsınız.

Tahmine dayalı bir deneme dönüştürerek, eğitilen modeli Puanlama bir web hizmeti olarak dağıtılmaya hazır alıyorsunuz. Kullanıcılar web hizmetinin modelinize giriş veri gönderebilir ve modelinizi tahmin sonuçlarını geri gönderir. Tahmine dayalı bir deneme dönüştürme gibi modelinizin başkaları tarafından kullanılmak üzere nasıl beklediğiniz göz önünde bulundurun.

Tahmine dayalı bir deneme eğitim denemenizi dönüştürmek için tıklatın **çalıştırmak** deneme tuvalinin altındaki tıklatın **Web hizmetinin ayarı**seçeneğini belirleyip **Tahmine dayalı Web hizmeti**.

![Deneme Puanlama için Dönüştür](./media/publish-a-machine-learning-web-service/figure-1.png)

Bu dönüştürme gerçekleştirme hakkında daha fazla bilgi için bkz: [modelinizi Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

Aşağıdaki adımlar, Tahmine dayalı bir deneme yeni bir web hizmeti olarak dağıtma açıklamaktadır. Ayrıca, denemeler Klasik web hizmeti olarak dağıtabilirsiniz.

## <a name="deploy-it-as-a-web-service"></a>Bir web hizmeti olarak dağıtma

Tahmine dayalı denemeye yeni bir web hizmeti veya bir Klasik web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Tahmine dayalı denemeye yeni bir web hizmeti olarak dağıtma
Tahmine dayalı denemeye hazır, yeni bir Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model kendi tahminleri döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırmak** deneme tuvalinin altındaki. Denemeyi çalışması bittikten sonra tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [yeni]**.  Machine Learning Web hizmeti portal'ın dağıtım sayfası açılır.

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

#### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning Web hizmeti portal dağıtma deneme sayfası
Deneme dağıtma sayfasında web hizmeti için bir ad girin.
Fiyatlandırma planı seçin. Aksi takdirde, seçebilirsiniz varolan bir fiyatlandırma planı varsa, hizmet için yeni bir fiyat planı oluşturmanız gerekir.

1. İçinde **fiyat planı** açılan, var olan bir planı veya seçin **Select yeni plan** seçeneği.
2. İçinde **Plan adı**, plan faturanızı üzerinde tanımlayacak bir ad yazın.
3. Aşağıdakilerden birini seçin **aylık Plan katmanlarını**. Varsayılan bölgeniz ve web hizmetiniz için planlara planı katmanları varsayılan bu bölgeye dağıtılır.

Tıklatın **dağıtma** ve **Hızlı Başlangıç** web hizmetiniz için sayfası açılır.

Web hizmeti hızlı başlangıç sayfası bir web hizmeti oluşturduktan sonra gerçekleştirecek en yaygın görevleri erişim ve rehberlik sağlar. Buradan, Test sayfası ve Tüket sayfa kolayca erişebilirsiniz.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

#### <a name="test-your-new-web-service"></a>Yeni web hizmetinizi test
Yeni web hizmetinizi sınamak için **Test web hizmeti** Ortak Görevler'in altında. Test sayfası üzerinde web hizmetiniz bir istek-yanıt hizmeti'olarak (RR) veya toplu iş yürütme hizmeti (BES) test edebilirsiniz.

RRS test sayfası girişleri ve çıkışları deneme için tanımladığınız genel parametreleri görüntüler. Web hizmeti sınamak için el ile girişleri için uygun değerleri girebilir veya test değerlerini içeren bir virgülle ayrılmış değer (CSV) biçimlendirilmiş bir dosya belirtin.

RRS kullanarak test etmek için liste görünümü modundan girişleri için uygun değerleri girin ve **Test istek-yanıt**. Sol çıkış sütununda tahmin sonuçlarını görüntüler.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

BES sınamak için **toplu**. Toplu test sayfasında girişinizi altında Gözat'ı tıklatın ve uygun örnek değerler içeren bir CSV dosyası seçin. Bir CSV dosyası yok ve Machine Learning Studio kullanarak Tahmine dayalı denemenizi oluşturulan veri kümesi için Tahmine dayalı denemenizi indirin ve kullanın.

Veri kümesi indirmek için Machine Learning Studio'da açın. Tahmine dayalı denemenizi açın ve denemeniz için giriş sağ tıklayın. Bağlam menüsünden seçin **dataset** ve ardından **karşıdan**.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Tıklatın **Test**. Toplu iş yürütme iş durumunu görüntüler altında sağındaki **Test toplu işlerini**.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

Üzerinde **yapılandırma** sayfası, başlığı açıklamasını değiştirmek, depolama hesabı anahtarı güncelleştirmek ve web hizmetiniz için örnek verileri etkinleştir.

![Web hizmetini yapılandır](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Web hizmeti dağıttıktan sonra sonra şunları yapabilirsiniz:

* **Erişim** web hizmeti API'si üzerinden.
* **Yönetme** Azure Machine Learning web hizmetleri portalı veya Klasik Azure portalı üzerinden.
* **Güncelleştirme** , modelinizi değişirse.

#### <a name="access-your-new-web-service"></a>Yeni web hizmetine erişim
Machine Learning Studio'dan web hizmetini dağıtma sonra hizmete veri göndermek ve yanıtları program aracılığıyla alırsınız.

**Tüket** sayfası, web hizmetinize erişmek gereken tüm bilgileri sağlar. Örneğin, API anahtarı, hizmet yetkili erişmesine izin vermek için sağlanır.

Machine Learning web hizmetine erişim hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmeti kullanmak nasıl](consume-web-services.md).

#### <a name="manage-your-new-web-service"></a>Yeni web hizmetinizi yönetme
Yeni web hizmetleri Machine Learning Web Hizmetleri portalı yönetebilirsiniz. Gelen [ana portal sayfası](https://services.azureml-test.net/), tıklatın **Web Hizmetleri**. Web Hizmetleri sayfasından silin veya bir hizmet kopyalayın. Belirli bir hizmeti izlemek için hizmete tıklayın ve ardından **Pano**. Web hizmeti ile ilişkili toplu işleri izlemek için tıklatın **toplu isteği günlük**.

### <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Tahmine dayalı denemeye Klasik web hizmeti olarak dağıtma

Tahmine dayalı denemeye yeterince hazırlandı, Klasik Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model kendi tahminleri döndürür.

Tahmine dayalı denemenizi dağıtmak için **çalıştırmak** alt kısmındaki deneme tuvaline ve ardından **Web hizmeti Dağıt**. Web hizmeti ayarlama ve web hizmeti panosunda yerleştirilir.

![Web hizmetini dağıtma](./media/publish-a-machine-learning-web-service/figure-2.png)

#### <a name="test-your-classic-web-service"></a>Klasik web hizmeti test etme

Machine Learning Web Hizmetleri portalı veya Machine Learning Studio web hizmeti test edebilirsiniz.

İstek yanıtı web hizmeti sınamak için **Test** web hizmeti panosundaki düğmesi. Hizmet için bir giriş verisi sormak için bir iletişim kutusu açılır. Puanlama deney tarafından beklenen sütun bunlar. Bir veri kümesi girin ve ardından **Tamam**. Web hizmeti tarafından oluşturulan sonuçların Pano altında görüntülenir.

Tıklayabilirsiniz **Test** yeni web hizmeti bölümünde daha önce gösterildiği gibi Azure Machine Learning Web Hizmetleri portalında hizmetinizi test etmek için Önizleme bağlantı.

Toplu yürütme hizmeti sınamak için **Test** Önizleme bağlantı. Toplu test sayfasında girişinizi altında Gözat'ı tıklatın ve uygun örnek değerler içeren bir CSV dosyası seçin. Bir CSV dosyası yok ve Machine Learning Studio kullanarak Tahmine dayalı denemenizi oluşturulan veri kümesi için Tahmine dayalı denemenizi indirin ve kullanın.

![Web hizmetini sınama](./media/publish-a-machine-learning-web-service/figure-3.png)

Üzerinde **yapılandırma** sayfası, hizmetin görünen adını değiştirebilir ve bir açıklama girin. Ad ve açıklama görüntülenir [Klasik Azure portalı](http://manage.windowsazure.com/) web hizmetlerinizi yöneteceğiniz.

Bir açıklama giriş verileri, çıktı verilerini ve web hizmeti parametreleri altında her sütun için bir dize girerek sağlayabilirsiniz **giriş ŞEMASINI**, **çıkış ŞEMASI**, ve **Web hizmeti PARAMETRE**. Bu açıklamalar, web hizmeti için sağlanan örnek kodu belgelerinde kullanılır.

Web hizmetiniz erişildiğinde, görüyorsunuz hataları tanılamak günlük kaydını etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirmek](web-services-logging.md).

![Web hizmetini yapılandır](./media/publish-a-machine-learning-web-service/figure-4.png)

Yeni web hizmeti bölümünde daha önce gösterilen yordama benzer Azure Machine Learning Web Hizmetleri portalında web hizmeti uç noktalarını da yapılandırabilirsiniz. Seçenekler farklıdır, ekleyebilir veya hizmet açıklaması, etkinleştir günlüğe kaydetme ve test etmek için etkinleştir örnek verileri değiştirebilirsiniz.

#### <a name="access-your-classic-web-service"></a>Klasik web hizmetine erişim
Machine Learning Studio'dan web hizmetini dağıtma sonra hizmete veri göndermek ve yanıtları program aracılığıyla alırsınız.

Pano web hizmetinize erişmek gereken tüm bilgileri sağlar. Örneğin, API anahtarını hizmet yetkili erişmesine izin vermek için sağlanır ve API yardım sayfalarına sağlanan yardımcı olmak için kodunuzu yazmaya başlayın.

Machine Learning web hizmetine erişim hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmeti kullanmak nasıl](consume-web-services.md).

#### <a name="manage-your-classic-web-service"></a>Klasik web hizmetinizi yönetme
Çeşitli gerçekleştirebileceğiniz eylemler vardır bir web hizmeti izlemek için. Güncelleştirin ve silin. Klasik web hizmeti, dağıttığınızda oluşturulan varsayılan uç nokta yanı sıra ek uç noktalar ekleyebilirsiniz.

Daha fazla bilgi için bkz: [bir Azure Machine Learning çalışma alanını yönetme](manage-workspace.md) ve [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir web hizmetini yönetmek](manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Web hizmeti güncelleştir
Model ek eğitim verilerle güncelleştirme gibi web hizmetiniz için değişiklik ve tekrar, özgün web hizmeti üzerine dağıtın.

Web hizmetini güncelleştirmek için web hizmetini dağıtma ve tıklayarak düzenlenebilir bir kopyasını oluşturmak için kullanılan özgün Tahmine dayalı denemeye açın **SAVE AS**. İstediğiniz değişiklikleri yapın ve ardından **Web hizmeti Dağıt**.

Bu deneme önce dağıttıktan sonra olduğundan, (Klasik Web hizmeti) üzerine veya (Yeni web hizmeti) hizmetini güncelleştirmek isteyip istemediğinizi istenir. Tıklatarak **Evet** veya **güncelleştirme** varolan web hizmetini durdurur ve yeni dağıtır Tahmine dayalı denemeye onun yerine dağıtılır.

> [!NOTE]
> Yapılandırma değişiklikleri özgün web hizmetinde yaptıysanız, örneğin, yeni bir görünen ad veya açıklama girme, bu değerleri yeniden girin gerekir.
> 
> 

Web hizmeti güncelleştirmek için bir model program aracılığıyla yeniden eğitme seçenektir. Daha fazla bilgi için bkz. [Machine Learning modellerini programlama aracılığıyla yeniden eğitme](retrain-models-programmatically.md).

<!-- internal links -->
[Eğitim denemenizi oluşturma]: #create-a-training-experiment
[Tahmine dayalı bir deneme Dönüştür]: #convert-the-training-experiment-to-a-predictive-experiment
[Web hizmeti olarak dağıtabilir]: #deploy-it-as-a-web-service
[yeni]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Klasik]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
