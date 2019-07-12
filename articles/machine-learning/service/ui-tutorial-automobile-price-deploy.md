---
title: 'Öğretici: Bir machine learning modeli görsel arabirim ile dağıtma'
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti visual arabiriminde bir Tahmine dayalı analiz çözümü oluşturmayı öğrenin. Eğitme, Puanlama ve sürükleme kullanarak makine öğrenme modeli dağıtma ve modülleri bırakın. Bu öğreticide doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etme bulunan iki bölümden oluşan bir bölümüdür.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/11/2019
ms.openlocfilehash: dd28fb51a4fc3fbf3dfc893f2f5f159ccafdb4b3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839297"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Öğretici: Bir machine learning modeli görsel arabirim ile dağıtma

Başkalarının içinde geliştirilen Tahmine dayalı bir model kullanmak için bir fırsat vermek [öğreticinin birinci kısmında](ui-tutorial-automobile-price-train-score.md), bir Azure web hizmeti olarak dağıtabilirsiniz. Şu ana kadar modelinizi eğitim ile denemeler. Artık, kullanıcı girişini temel alarak yeni tahminler üretmek için zamanı geldi. Öğreticinin bu bölümünde:

> [!div class="checklist"]
> * Bir model dağıtımına hazırlanma
> * Bir web hizmetini dağıtma
> * Bir web hizmetini test edin
> * Bir web hizmetini yönetme
> * Web hizmetini kullanma

## <a name="prerequisites"></a>Önkoşullar

Tam [öğreticinin birinci kısmında](ui-tutorial-automobile-price-train-score.md) eğitme ve görsel bir arabirim machine learning modeli Puanlama hakkında bilgi edinmek için.

## <a name="prepare-for-deployment"></a>Dağıtıma hazırlanma

Bir web hizmeti olarak denemenizi dağıtmadan önce öncelikle dönüştürülecek olması, *eğitim denemesini* içine bir *Tahmine dayalı denemeye*.

1. Seçin **Tahmine dayalı denemeler oluşturma*** deneme tuvalinin altındaki.

    ![Animasyonlu GIF gösteren bir eğitim denemesini öngörücü bir denemeye otomatik dönüştürme](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

    Seçtiğinizde, **oluşturma Tahmine dayalı denemeye**, olacaklar:
    
    * Eğitilen model olarak depolanan bir **eğitilen Model** modül paletindeki modülü. Bunun altında bulabilirsiniz **eğitilen modelleri**.
    * Eğitim için kullanılan modülleri kaldırılır; özellikle:
      * Model Eğitme
      * Verileri Bölme
      * Modeli Değerlendirme
    * Kaydedilmiş eğitilen modeli yeniden denemenin eklenir.
    * **Web hizmeti giriş** ve **Web hizmeti çıkış** modülleri eklendi. Bu modüller, model kullanıcı verilerinin burada girer ve nerede veriler döndürülür belirleyin.

    **Eğitim denemesini** deneme tuvalinin üst kısmındaki yeni sekmeler altında kaydedilir.

1. Denemeyi çalıştırmak için **Run (Çalıştır)** düğmesine basın.

1. Çıkışı seçin **Score Model** modülü ve select **sonuçlarını görüntüle** modelin hala çalıştığından emin olmak için. Özgün veriler görüntülenir, tahmin edilen Fiyat ("Puanlanmış etiketler") yanı sıra görebilirsiniz.

Denemenizi gibi görünmelidir:  

![Dağıtım için hazırlandıktan sonra deneme beklenen yapılandırmasını gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma

1. Seçin **Web hizmeti Dağıt** tuval aşağıda.

1. Seçin **hedef işlem** web hizmetini çalıştırmak istiyor.

    Şu anda görsel arabirim, yalnızca Azure Kubernetes Service (AKS) işlem hedefleri dağıtımı destekler. Görüntülenen iletişim kutusunda yer alan adımları kullanarak yeni bir AKS ortamını yapılandırmak ya da kullanılabilir AKS işlem hedeflerden durum da, machine learning hizmeti çalışma alanını seçin.

    ![Yeni işlem hedefi için bir olası yapılandırma gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Seçin **Web hizmetini dağıtma**. Dağıtım tamamlandığında, aşağıdaki bildirim görürsünüz. Dağıtım birkaç dakika sürebilir.

    ![Başarılı bir dağıtım için onay iletisi gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Web hizmetini test edin

Test edebilir ve giderek görsel arabirim web hizmetlerinizi yönetme **Web Hizmetleri** sekmesi.

1. Web hizmeti bölümüne gidin. Adı ile dağıtılan web hizmeti göreceğiniz **Öğreticisi - otomobil fiyatını tahmin [Tahmine dayalı ifade]** .

     ![Vurgulanan son oluşturulan web hizmeti ile web hizmeti sekmesini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Ek ayrıntıları görüntülemek için web hizmeti adı seçin.

     ![Web hizmetinde kullanılabilir ek ayrıntılarını gösteren ekran görüntüsü görüntüleyin](./media/ui-tutorial-automobile-price-deploy/web-service-details.png)

1. Seçin **Test**.

    ![Sayfasını test etme web hizmetini gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)

1. Giriş verileri test etme veya autofilled örnek verileri kullanarak ve seçin **Test**.

    Test amaçlı istek web hizmetine gönderilir ve sonuçları sayfasında gösterilir. Giriş verileri için fiyat değerini oluşturulmasına rağmen tahmin değer oluşturmak için kullanılmaz.

## <a name="consume-the-web-service"></a>Web hizmetini kullanma

Kullanıcılar, artık Azure web hizmetiniz için API istekleri göndermek ve kendi yeni otomobil fiyatını tahmin etmek için sonuçlar alabilirsiniz.

**İstek/yanıt** -kullanıcı bir HTTP protokolünü kullanarak bir veya daha fazla satır otomobil veri hizmetine gönderir. Hizmet bir veya daha fazla sonuç kümeleri yanıt verir.

REST çağrıları örnek bulabilirsiniz **Tüket** web hizmeti ayrıntıları sayfasının sekmesi.

   ![Örnek bir REST gösteren ekran görüntüsü kullanıcıları Tüket sekmesinde bulabilir çağırın](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Gidin **API belge** daha fazla API Ayrıntıları bulmak için sekmesinde.

  ![Kullanıcılar API belge sekmesinde bulabilirsiniz ek API ayrıntıları gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-deploy/web-service-api.png)

## <a name="manage-models-and-deployments"></a>Modelleri ve dağıtımları yönetin

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

Bu öğreticide, anahtar oluşturma, dağıtma ve makine öğrenme modeli visual arabiriminde kullanma adımları öğrendiniz. Diğer tür sorunları çözmek için görsel arabirim nasıl kullanabileceğiniz hakkında daha fazla bilgi için diğer bizim örnek denemeleri bakın.

> [!div class="nextstepaction"]
> [Kredi riski sınıflandırma örneği](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
