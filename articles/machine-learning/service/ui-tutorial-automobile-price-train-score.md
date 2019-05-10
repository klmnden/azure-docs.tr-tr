---
title: 'Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin'
titleSuffix: Azure Machine Learning service
description: ', Eğitme, Puanlama ve sürükle ve bırak görsel bir arabirim kullanarak makine öğrenme modeli dağıtma konusunda bilgi edinin. Bu öğreticide doğrusal regresyon kullanarak otomobil fiyatlarını tahmin etme bulunan iki bölümden oluşan bir bölümüdür.'
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 352f695f186b656ad0f6471d8ede905d53ad5274
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234113"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin

Bu öğreticide, Azure Machine Learning hizmeti görsel arabirim Tahmine dayalı bir çözüm geliştirirken bir genişletilmiş göz atalım. Bu öğreticinin sonunda, gönderdiğiniz teknik belirtimlerinden yola çıkarak herhangi bir araba fiyatını tahmin edebilen bir çözüm gerekir.

Bu öğreticide [hızlı başlangıçtan devam](ui-quickstart-run-experiment.md) ve **iki kısımlı öğretici serisinin birinci kısmında**. Ancak, başlamadan önce hızlı başlangıcı tamamlamak zorunda değilsiniz.

Birinci öğretici serisinde şunların nasıl yapılır:

> [!div class="checklist"]
> * İçeri aktarma ve verileri temizleme (hızlı başlangıç aynı adımları)
> * Makine öğrenme modeli eğitme
> * Puanlama ve bir modeli değerlendirme

İçinde [bölüm iki](ui-tutorial-automobile-price-deploy.md) öğretici serisi, Tahmine dayalı model bir Azure web hizmeti olarak dağıtmayı öğreneceksiniz.

> [!NOTE]
> Bu öğretici tamamlanmış bir sürümünü, örnek deneme olarak kullanılabilir.
> Denemeleri sayfasından Git **yeni Ekle** > **örnek 1 - regresyon: Otomobil fiyat Prediction(Basic)**

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Build-zero-code-machine-learning-models-with-Azure-Machine-Learning-service/player]


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanı varsa, atlamak [sonraki bölümde](#open-the-visual-interface-webpage). Aksi takdirde, şimdi bir tane oluşturun.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Görsel arabirim Web sayfasını açın

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  

1. Çalışma alanınızı seçin **görsel arabirim**.  Ardından **başlatma görsel arabirim**.  

    ![Azure portalında bir Machine Learning hizmeti çalışma alanından görsel arabirim nasıl gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

    Yeni bir tarayıcı sayfada arabirimi Web sayfası açılır.  

## <a name="import-and-clean-your-data"></a>İçeri aktarma ve verilerinizi temizleme

İhtiyacınız olan ilk şey, temiz verilerdir. Hızlı Başlangıç tamamlandı, veri hazırlığı denemenizi buraya yeniden kullanabilirsiniz. Bu hızlı başlangıçta tamamlamadıysanız, sonraki bölüme atlayın ve [yeni bir deneme başlatın](#start-from-a-new-experiment).

### <a name="reuse-the-quickstart-experiment"></a>Hızlı Başlangıç denemeyi yeniden kullanma

1. Hızlı Başlangıç denemenizi açın.

1. Seçin **Kaydet** pencerenin alt kısmındaki.

1. Bu görünen açılan iletişim kutusunda yeni bir ad verin.

    ![Deneme "Öğretici – tahmin otomobil Fiyat" Yeniden Adlandır gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/save-a-copy.png)

1. Deneme şimdi aşağıdakine benzer görünmelidir:

    ![Denemeyi beklenen durumunu gösteren ekran görüntüsü. Otomobil veri kümesini eksik verileri temizleme için bağlanan Sütunları Seç modülü bağlanır](./media/ui-tutorial-automobile-price-train-score/save-copy-result.png)

Hızlı Başlangıç denemenizi başarıyla yeniden, başlamak için sonraki bölüme atlayın. [modelinizi eğitim](#train-the-model).

### <a name="start-from-a-new-experiment"></a>Yeni bir deneme başlatın

Bu hızlı başlangıçta tamamlanmadıysa, hızlı bir şekilde içeri aktarır ve otomobil veri kümesini temizler ve yeni bir deneme oluşturmak için aşağıdaki adımları izleyin.

1. Seçerek yeni bir deneme oluşturma **+ yeni** görsel arabirim pencerenin alt kısmındaki.

1. Seçin **deneme** >  **boş deneme**.

1. Varsayılan deneme adını seçin **"deneme oluşturulan üzerinde...** "tuvalin üst kısmındaki ve anlamlı bir şekilde yeniden adlandırın. Örneğin, **otomobil fiyat tahmini**. Adın benzersiz olması gerekmez.

1. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Modüller bulmak için modül paletinin en üstündeki arama kutusunu kullanın. Tür **otomobil** etiketli veri kümesini bulmak için arama kutusuna **otomobil fiyat verileri (ham)**. Bu veri kümesini deneme tuvaline sürükleyin.

    ![Ekran otomobil fiyat veri kümesi bulma](./media/ui-tutorial-automobile-price-train-score/automobile-dataset.png)

    Artık verileriniz olduğuna göre kaldıran bir modül ekleyebileceğiniz **normalized-losses** sütun tamamen. Ardından eksik veriler içeren satırları kaldıran başka bir modül ekleyin.

1. Tür **sütunları seçme** bulmak için arama kutusuna **kümesindeki sütunları seçme** modülü. Ardından bunu deneme tuvaline sürükleyin. Bu modül, hangi veri sütunlarını dahil etmek veya hariç modelde istediğiniz seçmenize olanak sağlar.

1. Çıkış bağlantı noktasına bağlanmak **otomobil fiyat verileri (ham)** veri kümesindeki sütunları seçme, giriş bağlantı noktasına kümesi.

    ![Animasyonlu GIF gösteren otomobil fiyat verileri modülünü Sütunları Seç modülüne bağlayın](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. Sütunları Seç veri kümesi modülünde'ı seçip **Sütun seçiciyi Başlat** içinde **özellikleri** bölmesi.

   1. Sol tarafta, seçin **kurallarla**

   1. Yanındaki **şununla Başla**seçin **tüm sütunları**. Bu kurallar doğrudan **kümesindeki sütunları seçme** (dışarıda bu sütunlar hariç) tüm sütunlardan geçmeye.

   1. Açılan menüden seçin **hariç** ve **sütun adları**, Anahtar'a tıklayın ve **normalized-losses** metin kutusu içinde.

   1. (Sağ alt köşede üzerinde) sütun seçiciyi kapatmak için Tamam düğmesini seçin.

     Artık **Select Columns in Dataset (Veri Kümesinde Sütun Seçme)** için özellikler bölmesi, **normalleştirilmiş kayıplar** dışındaki tüm veri kümelerindeki tüm sütunlardan geçeceğini belirtir.

1. Bir yorum eklemek **kümesindeki sütunları seçme** modüle çift ve "Dışlama normalleştirilmiş kayıplar." girerek modülü. Bu, modülün denemenizde ne yaptığını, bir bakışta görmenize yardımcı olabilir.

    ![Sütunları Seç modülünün ekran gösteren doğru yapılandırma](./media/ui-tutorial-automobile-price-train-score/select-columns.png)

1. Tür **temiz** bulmak için arama kutusuna **eksik verileri temizleme** modülü. Sürükleme **eksik verileri temizleme** modülünü deneme tuvaline ve buna bağlanmak **kümesindeki sütunları seçme** modülü.

1. **Özellikler** bölmesinde, **Temizleme modu** altında **Tüm satırı kaldır**’ı seçin. Bu seçenekleri doğrudan **eksik verileri temizleme** değer içeren satırları kaldırarak verileri temizlemesi için. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.

![Eksik verileri temizleme modülü gösteren doğru yapılandırma ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/clean-missing-data.png)

## <a name="train-the-model"></a>Modeli eğitme

Veriler hazır olduğuna göre Tahmine dayalı bir model oluşturabilirsiniz. Verilerinizi modeli eğitmek için kullanacaksınız. Sonra ne kadar yakın fiyatlarını tahmin etme olanağına sahip görmek üzere modeli test edeceğiz.

**Sınıflandırma** ve **regresyon**, denetimli makine öğrenimi algoritmasının iki türüdür. **Sınıflandırma** kategoriler, örneğin bir rengi (kırmızı, mavi veya yeşil) tanımlanmış bir kümesinden yanıt tahmin eder. **Regresyon** bir sayıyı tahmin etmek için kullanılır.

Bir sayı olan fiyatı tahmin etmek istediğimiz bir regresyon algoritmasıdır kullanabilirsiniz. Bu örnekte, bir doğrusal regresyon modeli kullanacağız.

Bir fiyatı da içeren bir veri kümesi vererek modeli eğitme. Model verileri tarar ve bir otomobilin özellikleri ile fiyatı arasındaki bağlantıları arar. Ardından, bir özellik kümesi alışkın olduğu otomobiller için vererek modeli test ve ne kadar yakın modelin bilinen fiyatı tahmin etmeye gelen bakın.

Verileriniz, hem modeli eğitmek ve verileri ayrı eğitim ve test bölerek test için kullanın.

1. Tür **verileri bölme** bulmak için arama kutusuna **verileri bölme** modülü ve sol bağlantı noktasına bağlayın **eksik verileri temizleme** modülü.

1. Seçin **verileri bölme** modülü, henüz bağlandığınız seçin. Özellikler bölmesinde 0,7 için ilk çıkış veri kümesinde satır kesiri ayarlayın. Bu şekilde, biz modeli eğitmek için verilerin yüzde 70'i kullanın ve test etmek için yüzde 30 ayıracağız.

    ![Özellikler bölmesi doğru yapılandırıldığını gösteren ekran görüntüsü. "Verileri bölme", "Bölünmüş satırları", 0,7, bölme, 0, False rastgele değerleri.](./media/ui-tutorial-automobile-price-train-score/split-data.png)

1. Çift **verileri bölme** ve "Bölme dataset set(0.7) ve test set(0.3) eğitim içine" yorum yazın

1. Öğrenme algoritmasını seçmek için modül palet arama kutusuna temizleyin.

1. Genişletin **Machine Learning** genişletin **modeli Başlat**. Böylece makine öğrenimi algoritmalarını başlatmak için kullanılabilecek çeşitli modül kategorileri görüntülenir.

1. Bu deneme için seçin **regresyon** > **doğrusal regresyon** ve bunu deneme tuvaline sürükleyin.

    ![Özellikler bölmesi doğru yapılandırıldığını gösteren ekran görüntüsü. "Verileri bölme", "Bölünmüş satırları", 0,7, bölme, 0, False rastgele değerleri.](./media/ui-tutorial-automobile-price-train-score/linear-regression-module.png)

1. Bulun ve sürükleyin **modeli eğitme** modülünü deneme tuvaline. Doğrusal regresyon modülün çıkışına Train Model modülünün sol girişine bağlayın ve eğitim verileri çıkışına (sol bağlantı noktası) bağlayın **verileri bölme** modülünün sağ girişine **modeli eğitme**modülü.

    ![Model eğitme modülünü doğru yapılandırıldığını gösteren ekran görüntüsü. Doğrusal regresyon modülü sol bağlantı noktası modeli eğitme modülünü bağlanır ve verileri bölme modülünün sağ bağlantı noktası, modeli eğitme için bağlanır.](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. Seçin **modeli eğitme** modülü. Sütun seçiciyi Başlat Özellikler bölmesinde seçin ve ardından yazın **fiyat** yanındaki **Include column names**. Fiyat, modelinizi tahmin etmek için gittiği değerdir

    ![Sütun Seçici modülü için doğru yapılandırmayı gösteren ekran görüntüsü. Kurallarla > Include column names > "price"](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    Şimdi deneme gibi görünmelidir.
    ![Model eğitme modülünü ekledikten sonra deneme doğru yapılandırıldığını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Eğitim denemesini çalıştırma

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Puanlama ve modeli değerlendirme

Verilerinizi yüzde 70'i kullanarak modeli eğittiğimize göre bunu diğer yüzde 30'ne kadar iyi görmek için verileri puanlamak için kullanabilirsiniz, model işlevleri.

1. Tür **model Puanlama** bulmak için arama kutusuna **Score Model** modülü ve modülünü deneme tuvaline sürükleyin. Çıkışını **modeli eğitme** modülünün sol giriş bağlantı noktasına **Score Model**. Test verileri çıkışını (sağ bağlantı noktası) **verileri bölme** modülünün sağ giriş bağlantı noktasına **Score Model**.

1. Tür **değerlendirmek** bulmak için arama kutusuna **Evaluate Model** ve BT sürükleyin modülünü deneme tuvaline. Çıkışını **Score Model** modülünün sol girişine **Evaluate Model**. Son deneme şuna benzer şekilde görünecektir:

    ![Denemeyi son doğru yapılandırıldığını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Daha önce kullanılan aynı işlem hedefi kullanarak denemeyi çalıştırın.

1. Çıkışı görüntülemek **Score Model** modülünün çıkış bağlantı noktasını seçerek **Score Model** seçip **Görselleştir**. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.

    !["Etiket Puanlanmış" sütununda vurgulama çıkış görselleştirme görüntüsü](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Evaluate Model modülünden çıkışı görüntülemek için çıkış bağlantı noktasına seçin ve ardından Görselleştir'i seçin.

    ![Son deneme için değerlendirme sonuçları gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

Modeliniz için aşağıdaki İstatistikler gösterilir:

* **Mean Absolute Error (MAE)**: (Hata tahmin edilen değer ile gerçek değer arasındaki farktır) mutlak hataların ortalaması.
* **Kök ortalama karesi alınmış hata (RMSE)**: Test veri kümesinde yapılan tahminlerin karesi Ortalama kare kökü.
* **Göreli mutlak hata**: Mutlak hataların gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki mutlak farka göreli ortalaması.
* **Göreli karesi alınmış hata**: Ortalama gerçek değerler ve tüm gerçek değerlerin ortalaması arasındaki karesi alınmış fark göreli karesi.
* **Katsayısı**: Olarak da bilinen R karesi alınmış değer, bu model verileri ne kadar iyi uyumlu olduğunu gösteren istatistik ölçümleridir.

Her bir hata istatistiği ne kadar küçük olursa o kadar iyidir. Daha küçük olan bir değer, tahminlerin gerçek değerlerle daha yakından eşleştiğini gösterir. Belirleme katsayısı değeri bire (1.0) daha yakındır iyi olur.

## <a name="manage-experiments-in-azure-machine-learning-service-workspace"></a>Azure Machine Learning hizmeti çalışma alanında denemeler yönetme

Görsel arabirim içinde oluşturduğunuz denemeleri, Azure Machine Learning hizmeti çalışma alanından yönetilebilir. Kişiler deneme çalıştırdığı gibi tanılama günlükleri, yürütme grafikler ve daha ayrıntılı bilgi görmek için çalışma alanını kullanın.

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  

1. Çalışma alanınızı seçin **denemeleri**. Ardından, oluşturduğunuz denemeyi seçin.

    ![Azure portalında denemeleri gidin gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/portal-experiments.png)

    Bu sayfada, deneme ve onun en son çalıştırmalar genel bakış görürsünüz.

    ![Azure portalında deneme istatistikleri ekran gösteren genel bakış](./media/ui-tutorial-automobile-price-train-score/experiment-overview.png)

1. Belirli bir yürütme hakkında daha fazla ayrıntı görmek için bir çalıştırma numarası seçin.

    ![Ekran ayrıntılı çalıştırma raporu](./media/ui-tutorial-automobile-price-train-score/run-details.png)

    Rapor Çalıştır, gerçek zamanlı olarak güncelleştirilir. Kullandıysanız bir **Python betiği yürütme** denemenizi modülünde çıkış için betik günlüklerini belirtebilirsiniz **günlükleri** sekmesi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Serinin Bu öğretici, bu adımlar tamamlandı:

* Bu hızlı başlangıçta oluşturulan denemeyi yeniden kullanma
* Verileri hazırlama
* Modeli eğitme
* Puanlama ve modeli değerlendirme

İkinci kısımda, modelinizi bir Azure web hizmeti olarak dağıtmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Modelleri dağıtmak için devam edin](ui-tutorial-automobile-price-deploy.md)
