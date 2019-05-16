---
title: 'Hızlı Başlangıç: Hazırlama ve kod yazmaya gerek kalmadan verileri görselleştirin'
titleSuffix: Azure Machine Learning service
description: Bir machine learning denemesinden hazırlamak ve bir Sürükle ve bırak kullanıcı arabirimi ile verilerinizi görselleştirmek için oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 05/02/2019
ms.openlocfilehash: 0c492424e67853f7cb4a017fb4215d38a555a8a4
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545074"
---
# <a name="quickstart-prepare-and-visualize-data-without-writing-code-in-azure-machine-learning"></a>Hızlı Başlangıç: Hazırlama ve Azure Machine Learning'de kod yazmaya gerek kalmadan verileri görselleştirin

Hazırlama ve Azure Machine Learning için görsel sürükle ve bırak arabirimi (Önizleme) verilerinizi görselleştirin. Kullanacağınız veri üreticisi, model, teknik belirtimler ve fiyat gibi bilgiler dahil olmak üzere çeşitli ayrı otomobil için giriş içerir.  

Bu hızlı başlangıçta keşfedin ve verileri hazırlama:

- Ekleme ve verilerin önizlemesini ilk denemenizi oluşturma
- Eksik değerleri kaldırarak verileri hazırlama
- Denemeyi çalıştırma
- Sonuçta elde edilen verileri görselleştirin

Machine learning, video serisi için yeni [yeni başlayanlar için veri bilimi](https://docs.microsoft.com/azure/machine-learning/studio/data-science-for-beginners-the-5-questions-data-science-answers) makine öğrenimine harika bir giriş niteliğindedir.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

### <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir Azure Machine Learning hizmeti çalışma alanı varsa, atlamak [sonraki bölümde](#start). Aksi takdirde, şimdi bir tane oluşturun.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="start"></a> Görsel arabirim Web sayfasını açın

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  

1. Çalışma alanınızı seçin **görsel arabirim**.  Ardından **başlatma görsel arabirim**.  
 
    ![Görsel arabirim başlatın](./media/ui-quickstart-run-experiment/launch-ui.png)

    Yeni bir tarayıcı sayfada arabirimi Web sayfası açılır.  

## <a name="create-your-first-experiment"></a>İlk denemenizi oluşturma

Görsel arabirim araç kolayca oluşturun, test ve Tahmine dayalı bir modeli üzerinde yinelemek için etkileşimli ve görsel bir yer sağlar. Veri kümeleri ve analiz sürükle ve bırak modüllerini bunları birbirine bağlayarak etkileşimli bir tuvale bir _deneme_.  Şimdi ilk denemenizi oluşturun.

1. Sol alt köşedeki seçin **yeni Ekle**.
![Yeni bir deneme Ekle](./media/ui-quickstart-run-experiment/add-new.png)

1. Seçin **boş deneme**.

1. Denemenizi bir varsayılan ad verilir. Bu metin seçin ve yeniden adlandırmak "Hızlı Başlangıç için keşfetmek veri." Bu ad benzersiz olması gerekmez.

1. **Mini harita** ekranın alt kısmında, büyük denemeleri görüntülemek için yararlıdır.  Bu hızlı başlangıçta gerekmez bu nedenle, en aza indirmek için üst oku tıklatın.  

    ![Denemeyi yeniden adlandırma](./media/ui-quickstart-run-experiment/rename.png)

## <a name="add-data"></a>Veri ekle

Machine learning için gereken ilk şey verilerdir. Bu arabirimde kullanabileceğiniz birçok örnek veri kümesi vardır ve birçok kaynaktan verileri içeri aktarabilirsiniz. Bu örnekte, örnek veri kümesini kullanırsınız **otomobil fiyat verileri (ham)**. 

1. Deneme tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. Seçin **kaydedilmiş veri kümeleri** seçip **örnekleri** kullanılabilir örnek veri kümelerini görüntülemek için.

1. Veri kümesi seçin **otomobil fiyat verileri (ham)** ve tuvale sürükleyin.

   ![Veri tuvale sürükleyin](./media/ui-quickstart-run-experiment/drag-data.png)


## <a name="select-columns"></a>Sütun seçme

Çalışmak için veri sütunları seçin.  İle başlamak modülü, tüm kullanılabilir sütunları gösterecek şekilde yapılandırın.

> [!TIP]
> Veri veya istediğiniz modül adını biliyorsanız Arama çubuğuna paleti üst kısmında hızla bulmak için kullanın.  Hızlı başlangıcın geri kalanını bu kısayol kullanır.

1. Tür **seçin** bulmak için arama kutusuna **kümesindeki sütunları seçme** modülü.

1. Tıklayın ve sürükleyin **kümesindeki sütunları seçme** tuvale. Modülü daha önce eklediğiniz veri kümesi altına bırakın.

1. Veri kümesine bağlanmak **kümesindeki sütunları seçme**: veri kümesinin çıkış bağlantı noktasına tıklayın, giriş bağlantı noktasına sürükleyin **kümesindeki sütunları seçme**, ardından fare düğmesini bırakın. Veri kümesi ve modül, ya da tuvalde yerleri bile bağlı kalır.

    > [!TIP]
    > Veri kümeleri ve modülleri küçük dairelerle gösterilen giriş ve çıkış bağlantı noktalarına sahiptir. Giriş bağlantı noktaları yukarıda, çıkış bağlantı noktaları aşağıdadır. Bir modülün çıkış bağlantı noktasını bir diğerinin giriş bağlantı noktasına bağlandığında denemeniz veri bir akış oluşturun.
    >
    > Modüller bağlanırken sorun varsa, bağlandığınız düğümü tamamen sürüklemeyi deneyin.

    ![Modüller bağlanma](./media/ui-quickstart-run-experiment/connect-modules.gif) 

    Kırmızı ünlem işareti modülü için özellikleri henüz ayarlamadıysanız gösterir. Şimdi yaparsınız.
   
1. Seçin **kümesindeki sütunları seçme** modülü.

1. İçinde **özellikleri** select tuvalinin sağ bölmeye **sütunları Düzenle**.

    İçinde **sütunları seçme** iletişim kutusunda, **tüm sütunları** ve **tüm özellikleri**. İletişim kutusu şu şekilde görünmelidir:

     ![Sütun Seçici](./media/ui-quickstart-run-experiment/select-all.png)

1. Alt sağ tarafta seçin **Tamam** Sütun seçiciyi kapatmak için.

## <a name="run-the-experiment"></a>Denemeyi çalıştırma

Herhangi bir zamanda bir veri kümesi veya modül verileri bu noktada, veri akışında nasıl göründüğünü görmek için çıkış bağlantı noktasına tıklayın.  Varsa **Görselleştir** seçeneği devre dışıdır, ilk deneme çalıştırmanız gerekir.  Şimdi yaparsınız.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

İşlem hedefi kullanılabilir olduktan sonra deneme çalıştırır. Çalıştırma tamamlandığında her modülü, yeşil bir onay işareti görünür.

![Durumu görüntüle](./media/ui-quickstart-run-experiment/status1.png)

## <a name="preview-the-data"></a>Veri önizlemesi

İlk denemenizi çalıştırmanız, verileri ile çalışmak zorunda bilgiler hakkında daha fazla bilgi edinin görselleştirebilirsiniz.

1. Alt kısmındaki çıkış bağlantı noktasına seçin **kümesindeki sütunları seçme** seçip **Görselleştir**.

1. Farklı sütunların veri penceresinde, bu sütunu hakkındaki bilgileri görüntülemek için tıklayın.  

    Bu veri kümesi her satır bir otomobili temsil eder ve her Otomobille ilişkili değişkenler sütun olarak görünür.    205 satırları ve bu kümesindeki 26 sütunları vardır.

     Bir veri sütununun her tıkladığınızda **istatistikleri** bilgi ve **görselleştirme** söz konusu sütunu görüntüsü soldaki bölmede görünür.  Örneğin, tıkladığınızda **kapılar num** sahip 2 benzersiz ve 2 eksik değerleri görürsünüz.  Değerleri görmek için aşağı kaydırın: iki ve dört kapılar.

     ![Veri önizlemesi](./media/ui-quickstart-run-experiment/preview-data.gif)

1. Veri kümenizde daha iyi anlamak için her bir sütunun tıklayın.

## <a name="prepare-data"></a>Verileri hazırlama

Genellikle bir veri kümesi analiz edilmeden önce biraz ön işleme gerekir. Çeşitli satırların sütunlarında bulunan eksik değerleri fark etmiş olabilirsiniz. Modelin verileri doğru şekilde analiz edebilmesi için bu eksik değerlerin temizlenmesi gerekir. Eksik değerleri olan satırları kaldıracağız. Ayrıca, **normalized-losses** sütununun büyük kısmı eksik değerleri, bu sütunu modelin tamamen dışında şekilde.

> [!TIP]
> Giriş verilerinden eksik değerleri temizleme, çoğu modülü kullanmanın bir önkoşuludur.  

### <a name="remove-column"></a>Sütunu kaldırma

İlk olarak, kaldırma **normalized-losses** sütun tamamen.

1. Seçin **kümesindeki sütunları seçme** modülü.

1. İçinde **özellikleri** select tuvalinin sağ bölmeye **sütunları Düzenle**.

    * Bırakın **kurallarla** ve **tüm sütunları** seçili.

    * Açılan menülerden **Hariç Tut** ve **sütun adlarını** seçerek metin kutusuna tıklayın. Tür **normalized-losses**.

    * Alt sağ tarafta seçin **Tamam** Sütun seçiciyi kapatmak için.

    ![Bir sütununu hariç tutun](./media/ui-quickstart-run-experiment/exclude-column.png)
        
    Şimdi bunu tüm sütunları veri kümesinden geçer kümesindeki sütunları seçme için Özellikler bölmesi gösterir **normalized-losses**.
        
    Özellikler bölmesi gösteren **normalized-losses** sütun çıkarılır.
        
    ![Özellik bölmesi](./media/ui-quickstart-run-experiment/property-pane.png)
        
    Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. 

1. Çift **kümesindeki sütunları seçme** modülü ve yorumunu yazın "normalleştirilmiş kayıpları dışarıda." 
    
    Yorum yazın sonra modülü tıklayın.  Modül bir açıklamayı içeren göstermek için aşağı ok görünür.

1. Aşağı açıklamayı görüntülemek için oka tıklayın.

    Modülü artık yorum gizlemek için bir yukarı ok gösterir.
        
    ![Açıklamalar](./media/ui-quickstart-run-experiment/comments.png)

### <a name="clean-missing-data"></a>Eksik verileri temizleme

Eksik verileri olan kalan satırları kaldıran başka bir modül. Şimdi ekleyin.

1. Tür **temiz** bulmak için arama kutusuna **eksik verileri temizleme** modülü.

1. Sürükleme **eksik verileri temizleme** modülünü deneme tuvaline ve buna bağlanmak **kümesindeki sütunları seçme** modülü. 

1. Özellikler bölmesinde seçin **tüm satırı Kaldır** altında **temizleme modu**.

    Bu seçenekleri doğrudan **eksik verileri temizleme** değer içeren satırları kaldırarak verileri temizlemesi için.

1. Modüle çift tıklayın ve "Eksik değerli satırları kaldır" yorumunu yazın.
 
    ![Satırları Kaldır](./media/ui-quickstart-run-experiment/remove-rows.png)

    Denemenizi şöyle görünmelidir:
    
    ![sütun seçin](./media/ui-quickstart-run-experiment/experiment-clean.png)

## <a name="visualize-the-results"></a>Sonuçlarını Görselleştirme

Değişiklikleri denemenizde modüllerine yapılan sonra durum "İçinde Taslak" durumuna değişti.  Yeni temiz verileri görselleştirmek için ilk kez denemeyi tekrar çalıştırabilir gerekir.

1. Seçin **çalıştırma** kısımdaki denemeyi çalıştırın.

    Bu süre, daha önce oluşturduğunuz işlem hedef yeniden kullanabilirsiniz.  

1. Seçin **çalıştırma** iletişim.

   ![Denemeyi çalıştırma](./media/ui-quickstart-run-experiment/select-compute.png)

1. Çalıştırma tamamlandığında sağ **eksik verileri temizleme** yeni temiz verileri görselleştirmek için modülü.  

    ![Temiz verileri görselleştirin](./media/ui-quickstart-run-experiment/visualize-cleaned.png)

1. Farklı sütunlar Temizlenen veri penceresinde verileri nasıl değiştiğini görmek için tıklayın.  

    ![Temiz verileri görselleştirin](media/ui-quickstart-run-experiment/visualize-result.png)

    Artık olduğuna 193 satırları ve sütunları 25.

    Tıkladığınızda **kapılar num** hala 2 benzersiz değerlere sahip ancak artık 0 eksik değerleri olan görürsünüz.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

- Ekleme ve verilerin önizlemesini ilk denemenizi oluşturma
- Eksik değerleri kaldırarak verileri hazırlama
- Sonuçta elde edilen verileri görselleştirin

Bu verileri bir otomobilin fiyatını tahmin etmek için kullanılacak Öğreticisine geçin.

> [!div class="nextstepaction"]
> [Öğretici: Görsel arabirim ile otomobil fiyatını tahmin edin](ui-tutorial-automobile-price-train-score.md)
