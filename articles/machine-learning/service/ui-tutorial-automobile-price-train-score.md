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
ms.openlocfilehash: 21f5a2d93b708e93f124bd44177bb7852dfbd86a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720550"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin

Bu öğreticide, Azure Machine Learning hizmeti görsel arabirim Tahmine dayalı bir çözüm geliştirirken bir genişletilmiş göz atalım. Bu öğreticinin sonunda, gönderdiğiniz teknik belirtimlerinden yola çıkarak herhangi bir araba fiyatını tahmin edebilen bir çözüm gerekir.

Birinci öğretici şunların nasıl yapılır:

> [!div class="checklist"]
> * İçeri aktarma ve verileri temizleme
> * Makine öğrenme modeli eğitme
> * Puanlama ve bir modeli değerlendirme

İçinde [bölüm iki](ui-tutorial-automobile-price-deploy.md) Öğreticisi Tahmine dayalı model bir Azure web hizmeti olarak dağıtmayı öğreneceksiniz.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

Bu öğretici tamamlanmış bir sürümünü, örnek deneme olarak kullanılabilir.

Öğesinden bulmak için **denemeleri sayfa**seçin **yeni Ekle**, ardından **örnek 1 - regresyon: Otomobil fiyat Prediction(Basic)** denemeler yapın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanı varsa, atlamak [sonraki bölümde](#open-the-visual-interface-webpage).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Görsel arabirim Web sayfasını açın

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).

1. Çalışma alanınızı seçin **görsel arabirim**. Ardından **başlatma görsel arabirim**. 

    ![Azure portalında bir Machine Learning hizmeti çalışma alanından görsel arabirim nasıl gösteren ekran görüntüsü](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

## <a name="create-your-first-experiment"></a>İlk denemenizi oluşturma

Görsel arabirim aracı, Tahmine dayalı analiz modelleri oluşturmak için etkileşimli ve görsel bir yer sağlar. Sürükle ve bırak veri kümeleri ve analiz modülleri etkileşimli bir tuvale ve bunları oluşturmak için bir araya bağlayın bir *deneme*.

1. Seçerek yeni bir deneme oluşturma **+ yeni** görsel arabirim pencerenin alt kısmındaki.

    ![Yeni bir deneme Ekle](./media/ui-tutorial-automobile-price-train-score/add-new.png)

1. Seçin **boş deneme**.

1. Varsayılan deneme adını seçin **"deneme oluşturulan üzerinde...** "tuvalin üst kısmındaki ve anlamlı bir şekilde yeniden adlandırın. Örneğin, **otomobil fiyat tahmini**. Adın benzersiz olması gerekmez.

## <a name="add-data"></a>Veri ekleme

Machine learning için gereken ilk şey verilerdir. Bu arabirimde kullanabileceğiniz birçok örnek veri kümesi vardır. Ayrıca, var olan kaynaklardan gelen verileri içeri aktarabilirsiniz. Bu öğreticide, örnek veri kümesini kullanmak **otomobil fiyat verileri (ham)** . 

1. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Seçin **kaydedilmiş veri kümeleri** seçip **örnekleri** kullanılabilir örnek veri kümelerini görüntülemek için.

1. Veri kümesi seçin **otomobil fiyat verileri (ham)** ve tuvale sürükleyin.

   ![Veri tuvale sürükleyin](./media/ui-tutorial-automobile-price-train-score/drag-data.png)

## <a name="select-columns"></a>Sütun seçme

Çalışmak için veri sütunları seçin. İle başlamak modülü, tüm kullanılabilir sütunları gösterecek şekilde yapılandırın.

> [!TIP]
> Veri veya istediğiniz modül adını biliyorsanız Arama çubuğuna paleti üst kısmında hızla bulmak için kullanın. Bu öğreticinin geri kalanını bu kısayol kullanır.


1. Tür **seçin** bulmak için arama kutusuna **kümesindeki sütunları seçme** modülü.

1. Tıklayın ve sürükleyin **kümesindeki sütunları seçme** tuvale. Modülü daha önce eklediğiniz veri kümesi altına bırakın.

1. Veri kümesine bağlanmak **kümesindeki sütunları seçme**: veri kümesinin çıkış bağlantı noktasına tıklayın, giriş bağlantı noktasına sürükleyin **kümesindeki sütunları seçme**, ardından fare düğmesini bırakın. Veri kümesi ve modül, ya da tuvalde yerleri bile bağlı kalır.

    > [!TIP]
    > Veri kümeleri ve modülleri küçük dairelerle gösterilen giriş ve çıkış bağlantı noktalarına sahiptir. Giriş bağlantı noktaları yukarıda, çıkış bağlantı noktaları aşağıdadır. Bir modülün çıkış bağlantı noktasını bir diğerinin giriş bağlantı noktasına bağlandığında denemeniz veri bir akış oluşturun.
    >

    ![Modüller bağlanma](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    Kırmızı ünlem işareti modülü için özellikleri henüz ayarlamadıysanız gösterir.

1. Seçin **kümesindeki sütunları seçme** modülü.

1. İçinde **özellikleri** select tuvalinin sağ bölmeye **sütunları Düzenle**.

    İçinde **sütunları seçme** iletişim kutusunda, **tüm sütunları** ve **tüm özellikleri**. İletişim kutusu şu şekilde görünmelidir:

     ![Sütun Seçici](./media/ui-tutorial-automobile-price-train-score/select-all.png)

1. Alt sağ tarafta seçin **Tamam** Sütun seçiciyi kapatmak için.

## <a name="run-the-experiment"></a>Denemeyi çalıştırma

Herhangi bir zamanda bir veri kümesi veya modül verileri bu noktada, veri akışında nasıl göründüğünü görmek için çıkış bağlantı noktasına tıklayın. Varsa **Görselleştir** seçeneği devre dışıdır, ilk deneme çalıştırmanız gerekir.

Bir deney çalışma alanınıza bağlı bir işlem kaynağı olan bir işlem hedefine çalışır. İşlem hedefi oluşturduktan sonra sonraki çalıştırmalar için yeniden kullanabilirsiniz.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

İşlem hedefi kullanılabilir olduktan sonra deneme çalıştırır. Çalıştırma tamamlandığında her modülü, yeşil bir onay işareti görünür.


## <a name="preview-the-data"></a>Veri önizlemesi

İlk denemenizi çalıştırdığınızda, veri kümesi ile çalışmak zorunda daha iyi anlamak için verileri görselleştirebilirsiniz.

1. Alt kısmındaki çıkış bağlantı noktasına seçin **kümesindeki sütunları seçme** seçip **Görselleştir**.

1. Farklı sütunların veri penceresinde, bu sütunu hakkındaki bilgileri görüntülemek için tıklayın.

    Bu veri kümesi her satır bir otomobili temsil eder ve her Otomobille ilişkili değişkenler sütun olarak görünür. 205 satırları ve bu kümesindeki 26 sütunları vardır.

     Bir veri sütununun her tıkladığınızda **istatistikleri** bilgi ve **görselleştirme** söz konusu sütunu görüntüsü soldaki bölmede görünür. Örneğin, tıkladığınızda **kapılar num** sahip 2 benzersiz ve 2 eksik değerleri görürsünüz. Değerleri görmek için aşağı kaydırın: iki ve dört kapılar.

     ![Veri önizlemesi](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)

1. Her sütun, veri kümesi hakkında daha fazla bilgi edinin ve olup bu sütunlar bir otomobilin fiyatını tahmin etmek yararlı olacak hakkında düşünmek için tıklayın.

## <a name="prepare-data"></a>Verileri hazırlama

Genellikle bir veri kümesi analiz edilmeden önce biraz ön işleme gerekir. Veri kümesi görselleştirirken kullanılacak bazı eksik değerleri fark olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Eksik değerleri olan satırları kaldıracağız. Ayrıca, **normalized-losses** sütununun büyük kısmı eksik değerleri, bu sütunu modelin tamamen dışında şekilde.

> [!TIP]
> Giriş verilerinden eksik değerleri temizleme, çoğu modülü kullanmanın bir önkoşuludur.

### <a name="remove-column"></a>Sütunu kaldırma

İlk olarak, kaldırma **normalized-losses** sütun tamamen.

1. Seçin **kümesindeki sütunları seçme** modülü.

1. İçinde **özellikleri** select tuvalinin sağ bölmeye **sütunları Düzenle**.

    * Bırakın **kurallarla** ve **tüm sütunları** seçili.

    * Açılan menülerden **Hariç Tut** ve **sütun adlarını** seçerek metin kutusuna tıklayın. Tür **normalized-losses**.

    * Alt sağ tarafta seçin **Tamam** Sütun seçiciyi kapatmak için.

    ![Bir sütununu hariç tutun](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Şimdi bunu tüm sütunları veri kümesinden geçer kümesindeki sütunları seçme için Özellikler bölmesi gösterir **normalized-losses**.
        
    Özellikler bölmesi gösteren **normalized-losses** sütun çıkarılır.
        
    ![Özellik bölmesi](./media/ui-tutorial-automobile-price-train-score/property-pane.png)
        
    Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. 

1. Çift **kümesindeki sütunları seçme** modülü ve yorumunu yazın "normalleştirilmiş kayıpları dışarıda." 
    
    Yorum yazın sonra modülü tıklayın. Modül bir açıklamayı içeren göstermek için aşağı ok görünür.

1. Aşağı açıklamayı görüntülemek için oka tıklayın.

    Modülü artık yorum gizlemek için bir yukarı ok gösterir.
        
    ![Açıklamalar](./media/ui-tutorial-automobile-price-train-score/comments.png)

### <a name="clean-missing-data"></a>Eksik verileri temizleme

Bir model eğitip, eksik veriler hakkında bir şey yapmanız gerekir. Bu durumda, bir modül eksik verileri olan kalan tüm satırı Kaldır ekleyeceksiniz.

1. Tür **temiz** bulmak için arama kutusuna **eksik verileri temizleme** modülü.

1. Sürükleme **eksik verileri temizleme** modülünü deneme tuvaline ve buna bağlanmak **kümesindeki sütunları seçme** modülü. 

1. Özellikler bölmesinde seçin **tüm satırı Kaldır** altında **temizleme modu**.

    Bu seçenekleri doğrudan **eksik verileri temizleme** değer içeren satırları kaldırarak verileri temizlemesi için.

1. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.
 
    ![Satırları Kaldır](./media/ui-tutorial-automobile-price-train-score/remove-rows.png)

    Denemenizi şöyle görünmelidir:
    
    ![sütun seçin](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="visualize-the-results"></a>Sonuçlarını Görselleştirme

Değişiklikleri denemenizde modüllerine yapılan sonra durum "İçinde Taslak" durumuna değişti.  Yeni temiz verileri görselleştirmek için ilk kez denemeyi tekrar çalıştırabilir gerekir.

1. Seçin **çalıştırma** kısımdaki denemeyi çalıştırın.

    Bu süre, daha önce oluşturduğunuz işlem hedef yeniden kullanabilirsiniz.

1. Seçin **çalıştırma** iletişim.

   ![Denemeyi çalıştırma](./media/ui-tutorial-automobile-price-train-score/select-compute.png)

1. Çalıştırma tamamlandığında sağ **eksik verileri temizleme** yeni temiz verileri görselleştirmek için modülü.

    ![Temiz verileri görselleştirin](./media/ui-tutorial-automobile-price-train-score/visualize-cleaned.png)

1. Farklı sütunlar Temizlenen veri penceresinde verileri nasıl değiştiğini görmek için tıklayın.

    ![Temiz verileri görselleştirin](media/ui-tutorial-automobile-price-train-score/visualize-result.png)

    Artık olduğuna 193 satırları ve sütunları 25.

    Tıkladığınızda **kapılar num** hala 2 benzersiz değerlere sahip ancak artık 0 eksik değerleri olan görürsünüz. Sütun kümesinde eksik değerlerin yok olduğuna bakın kalanında tıklayın. 

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

    Denemenizi şu şekilde görünmelidir:

    ![Model eğitme modülünü ekledikten sonra deneme doğru yapılandırıldığını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Eğitim denemesini çalıştırma

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Puanlama ve modeli değerlendirme

Verilerinizi yüzde 70'i kullanarak modeli eğittiğimize göre bunu diğer yüzde 30'ne kadar iyi görmek için verileri puanlamak için kullanabilirsiniz, model işlevleri.

1. Tür **model Puanlama** bulmak için arama kutusuna **Score Model** modülü ve modülünü deneme tuvaline sürükleyin. Çıkışını **modeli eğitme** modülünün sol giriş bağlantı noktasına **Score Model**. Test verileri çıkışını (sağ bağlantı noktası) **verileri bölme** modülünün sağ giriş bağlantı noktasına **Score Model**.

1. Tür **değerlendirmek** bulmak için arama kutusuna **Evaluate Model** modülünü deneme tuvaline sürükleyin. Çıkışını **Score Model** modülünün sol girişine **Evaluate Model**. Son deneme şuna benzer şekilde görünecektir:

    ![Denemeyi son doğru yapılandırıldığını gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Daha önce kullanılan aynı işlem hedefi kullanarak denemeyi çalıştırın.

1. Çıkışı görüntülemek **Score Model** modülünün çıkış bağlantı noktasını seçerek **Score Model** seçip **Görselleştir**. Çıkış, fiyat için tahmin edilen değerleri ve test verileri için bilinen değerleri gösterir.

    !["Etiket Puanlanmış" sütununda vurgulama çıkış görselleştirme görüntüsü](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Evaluate Model modülünden çıkışı görüntülemek için çıkış bağlantı noktasına seçin ve ardından Görselleştir'i seçin.

    ![Son deneme için değerlendirme sonuçları gösteren ekran görüntüsü.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

Modeliniz için aşağıdaki İstatistikler gösterilir:

* **Mean Absolute Error (MAE)** : (Hata tahmin edilen değer ile gerçek değer arasındaki farktır) mutlak hataların ortalaması.
* **Kök ortalama karesi alınmış hata (RMSE)** : Test veri kümesinde yapılan tahminlerin karesi Ortalama kare kökü.
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

* Bir deney oluşturuldu
* Verileri hazırlama
* Modeli eğitme
* Puanlama ve modeli değerlendirme

İkinci kısımda, modelinizi bir Azure web hizmeti olarak dağıtmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Modelleri dağıtmak için devam edin](ui-tutorial-automobile-price-deploy.md)
