---
title: 'Öğretici: Bir machine learning modeli görsel arabirim ile dağıtma'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti visual arabiriminde bir Tahmine dayalı analiz çözümü oluşturmayı öğrenin. Eğitme, Puanlama ve sürükleme kullanarak makine öğrenme modeli dağıtma ve modülleri bırakın. Bu öğreticide doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etme bulunan iki bölümden oluşan bir bölümüdür.
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 8512ca2fe01c772d7e4c21a5cb09303b9804899c
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389214"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Öğretici: Bir machine learning modeli görsel arabirim ile dağıtma

Bu öğreticide, Azure Machine Learning hizmeti görsel arabirim Tahmine dayalı bir çözüm geliştirirken bir genişletilmiş göz atalım. Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. İçinde [öğreticinin birinci kısmında](ui-tutorial-automobile-price-train-score.md), eğitim, puanlanmış ve otomobil fiyatlarını tahmin etmek için bir model değerlendirilir. Öğreticinin bu bölümünde:

> [!div class="checklist"]
> * Bir model dağıtımına hazırlanma
> * Bir web hizmetini dağıtma
> * Bir web hizmetini test edin
> * Bir web hizmetini yönetme
> * Web hizmetini kullanma

## <a name="prerequisites"></a>Önkoşullar

Tam [öğreticinin birinci kısmında](ui-tutorial-automobile-price-train-score.md).

## <a name="prepare-for-deployment"></a>Dağıtıma hazırlanma

Bu öğreticide geliştirilen Tahmine dayalı model kullanmak için bir şans verin başkaları için bir Azure web hizmeti olarak dağıtabilirsiniz.

Şu ana kadar modelinizi eğitim ile denemeler. Artık, kullanıcı girişini temel alarak yeni tahminler üretmek için zamanı geldi.

Dağıtım için iki adımlı bir işlemdir:  

1. Dönüştürme *eğitim denemesini* içinde oluşturduğunuz bir *Tahmine dayalı denemeye*
1. Tahmine dayalı denemeye bir web hizmeti olarak dağıtma

Denemenin bir kopyasını, öncelikle seçerek yapmak isteyebileceğiniz **Kaydet** deneme tuvalinin altındaki.

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme

Bu model dağıtım için hazır hale getirmek için bu eğitim denemesini öngörücü bir denemeye dönüştürme. Bu, genellikle üç adımdan oluşur:

1. Modeli eğittiğimize ve eğitim modüllerinizi değiştirin kaydedin
1. Eğitim için yalnızca gerekli olan modülleri kaldırmak için denemeyi Kırp
1. Burada, web hizmeti giriş verileri kabul eder ve burada Bu çıktıyı oluşturur tanımlayın

Bu adımları el ile yapabilirsiniz veya seçebilirsiniz **Web hizmetinin ayarı** bunları otomatik olarak yapılır için deneme tuvalinin altındaki.

![Animasyonlu GIF gösteren bir eğitim denemesini öngörücü bir denemeye otomatik dönüştürme](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

Seçtiğinizde, **Web hizmetinin ayarı**, olacaklar:

* Tek bir eğitim modeli dönüştürülür **eğitilen Model** modülü. Deneme tuvaline solundaki modül paletindeki depolanır. Bunun altında bulabilirsiniz **eğitilen modelleri**.
* Eğitim için kullanılan modülleri kaldırılır; özellikle:
  * Model Eğitme
  * Verileri Bölme
  * Modeli Değerlendirme
* Kaydedilmiş eğitilen modeli yeniden denemenin eklenir
* **Web hizmeti giriş** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller, kullanıcının veri modeli nerede girer ve burada veriler döndürülür belirleyin.

Deneme tuvalinin üst kısmındaki yeni sekmeler altında iki bölümden denemeyi kaydedilir görebilirsiniz. Özgün eğitim denemesini sekmesi altında olduğunu **eğitim denemesini**, ve yeni oluşturulan Tahmine dayalı denemeye altındadır **Tahmine dayalı denemeye**. Tahmine dayalı denemeye bir web hizmeti olarak dağıtacaksınız olur.

Denemenizi gibi görünmelidir:  

![Dağıtım için hazırlandıktan sonra deneme beklenen yapılandırmasını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

Son bir kez denemeyi çalıştırın (seçin **çalıştırma**). Açılan iletişim kutusunda çalıştırılacak deney istediğiniz işlem hedefini seçin. Modelin hala çalıştığından emin olmak için Model Puanlama modülü bir çıkış seçin ve seçin **sonuçlarını görüntüle**. Özgün veriler görüntülenir, tahmin edilen Fiyat ("Puanlanmış etiketler") yanı sıra görebilirsiniz.

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma

Yeni bir web hizmetini dağıtmak için denemenizi türetilmiş:

1. Seçin **Web hizmeti Dağıt** tuval aşağıda.
1. Seçin **hedef işlem** web hizmetini çalıştırmak istiyor.

    Şu anda görsel arabirim, yalnızca Azure Kubernetes Service (AKS) işlem hedefleri dağıtımı destekler. Görüntülenen iletişim kutusunda yer alan adımları kullanarak yeni bir AKS ortamını yapılandırmak ya da kullanılabilir AKS işlem hedeflerden durum da, machine learning hizmeti çalışma alanını seçin.

    ![Yeni işlem hedefi için bir olası yapılandırma gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Seçin **Web hizmetini dağıtma**. Dağıtım tamamlandığında, aşağıdaki bildirim görürsünüz. Dağıtım birkaç dakika sürebilir.

    ![Başarılı bir dağıtım için onay iletisi gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Web hizmetini test edin

Kullanıcı giriş verilerini girer, dağıtılan modeliyle **Web hizmeti giriş** modülü. Giriş olarak puanlanır **Score Model** modülü. Tahmine dayalı denemeye belirlediğiniz şekilde, özgün otomobil fiyat veri kümesi ile aynı biçimde veri modeli bekliyor. Son olarak kullanıcının üzerinden sonuçları döndüren **Web hizmeti çıkış** modülü.

Web hizmeti sekmesinde görsel arabirim bir web hizmetini test edebilirsiniz.

1. Web hizmeti bölümüne gidin. Adı ile dağıtılan web hizmeti göreceğiniz **Öğreticisi - otomobil fiyatını tahmin [Tahmine dayalı ifade]** .

     ![Vurgulanan son oluşturulan web hizmeti ile web hizmeti sekmesini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Ek ayrıntıları görüntülemek için web hizmeti adı seçin.

     ![Web hizmetinde kullanılabilir ek ayrıntılarını gösteren ekran görüntüsü görüntüleyin](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Seçin **Test**.

    ![Sayfasını test etme web hizmetini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Giriş verileri test etme veya autofilled örnek verileri kullanarak ve seçin **Test** altındaki. Test amaçlı istek web hizmetine gönderilir ve sonuçları sayfasında gösterilir. Giriş verileri için fiyat değerini oluşturulmasına rağmen tahmin değer oluşturmak için kullanılmaz.

## <a name="manage-the-web-service"></a>Web hizmetini yönetme

Web hizmetinizi dağıttıktan sonra buradan yönetebilirsiniz **Web Hizmetleri** görsel arabirim sekmesindedir.

Bir web hizmeti seçerek silebilirsiniz **Sil** web hizmeti ayrıntı sayfasında.

   ![Pencerenin alt kısmındaki silme web hizmeti düğmenin konumu gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-delete.png)

## <a name="consume-the-web-service"></a>Web hizmetini kullanma

Bu öğreticinin önceki adımlarda bir otomobil tahmin modeli bir Azure web hizmeti olarak dağıtılabilir. Artık kullanıcıların veri göndermek ve REST API aracılığıyla sonuçları alırsınız.

**İstek/yanıt** -kullanıcı bir HTTP protokolünü kullanarak bir veya daha fazla satır otomobil veri hizmetine gönderir. Hizmet bir veya daha fazla sonuç kümeleri yanıt verir.

REST çağrıları örnek bulabilirsiniz **Tüket** web hizmeti ayrıntıları sayfasının sekmesi.

   ![Örnek bir REST gösteren ekran görüntüsü kullanıcıları Tüket sekmesinde bulabilir çağırın](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Gidin **API belge** daha fazla API Ayrıntıları bulmak için sekmesinde.

  ![Kullanıcılar API belge sekmesinde bulabilirsiniz ek API ayrıntıları gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments-in-azure-machine-learning-service-workspace"></a>Modelleri ve Azure Machine Learning hizmeti çalışma alanında dağıtımlar'ı yönetme

Modeller ve görsel arabirim içinde oluşturduğunuz web hizmeti dağıtımları da Azure Machine Learning hizmeti çalışma alanından yönetilebilir.

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  

1. Çalışma alanınızı seçin **modelleri**. Ardından, oluşturduğunuz denemeyi seçin.

    ![Azure portalında denemeleri gidin gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/portal-models.png)

    Bu sayfada, model hakkında ek ayrıntılar görürsünüz.

    ![Azure portalında deneme istatistikleri ekran gösteren genel bakış](./media/ui-tutorial-automobile-price-deploy/model-details.png)

1. Seçin **dağıtımları**, modeli kullanan tüm web hizmetleri listelenir. Web hizmeti adı seçin, web hizmeti ayrıntıları sayfasına geçer. Bu sayfada, web hizmeti daha ayrıntılı bilgi edinebilirsiniz.

    ![Ekran ayrıntılı çalıştırma raporu](./media/ui-tutorial-automobile-price-deploy/deployment-details.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, anahtar oluşturma, dağıtma ve makine öğrenme modeli visual arabiriminde kullanma adımları öğrendiniz. Diğer tür sorunları çözmek için görsel arabirim nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için örnek denemeleri denetleyin.

> [!div class="nextstepaction"]
> [Kredi riski sınıflandırma örneği](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
