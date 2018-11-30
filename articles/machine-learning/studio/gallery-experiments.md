---
title: Azure AI Gallery denemeleri - Azure Machine Learning Studio | Microsoft Docs
description: Bul ve Azure AI Gallery'de denemeleri paylaşın.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: f4248922-c961-4d3a-9e1b-aec743210166
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.openlocfilehash: 66b5e30a26c57676bdd65b4861d34ca90c0c25af
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317475"
---
# <a name="discover-experiments-in-azure-ai-gallery"></a>Azure AI Gallery'de denemeleri bulma

[Azure AI Gallery](http://gallery.cortanaintelligence.com) çeşitli sahip [denemeleri](https://gallery.cortanaintelligence.com/experiments) de geliştirilmiştir [Azure Machine Learning Studio](https://studio.azureml.net). Belirli bir makine öğrenme, karmaşık makine öğrenimi sorunlar için tam olarak geliştirilen çözümleri için teknik gösteren hızlı kavram kanıtı denemeleri denemeleri çeşitler barındırır.

> [!NOTE]
> Bir ***deneme*** Machine Learning Studio'da Tahmine dayalı analiz modeli oluşturmak için kullanabileceğiniz bir tuvaldir. Model, ile çeşitli analitik modüllere veri bağlanarak oluşturun. Farklı fikirleri deneyin, deneme çalıştırmaları yapın ve sonunda azure'da bir web hizmeti olarak modelinizi dağıtma. Temel bir deneme oluşturmak nasıl bir örnek için bkz [makine öğrenimi Öğreticisi: Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md). Tahmine dayalı analiz çözümü oluşturmak nasıl daha kapsamlı bir kılavuz için bkz: [izlenecek yol: bir Azure Machine learning'de kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md).
>
>

## <a name="discover"></a>Keşif
Denemelere göz atmak için [galerideki](http://gallery.cortanaintelligence.com), galeri giriş sayfasının en üstünde seçin **denemeleri**.

**[Denemeleri](https://gallery.cortanaintelligence.com/experiments)** sayfası en son eklenen ve popüler denemeleri bir listesini görüntüler. Tüm denemeleri görmek için seçin **tümünü gör** düğmesi. Belirli bir deneme için aranacak seçin **tümünü gör**ve ardından filtre ölçütleri. Arama terimlerini de girebilirsiniz **arama** Galerisi'nde sayfanın üst kısmındaki kutusu.

Deneme ayrıntıları sayfasındaki bir deneme hakkında daha fazla bilgi alabilirsiniz. Bir deney Ayrıntılar sayfasını açmak için denemeyi seçin. Bir deney üzerinde ayrıntıları sayfasındaki **açıklamaları** bölümünde, yorum, geribildirim sağlamak veya kullanabilirsiniz denemeler hakkında soru sorun. Arkadaş veya iş arkadaşlarınıza Twitter veya LinkedIn ile deneme bile paylaşabilirsiniz. Ayrıca, sayfayı görüntülemek için diğer kullanıcıları davet etmek için deneme ayrıntıları sayfasına bağlantı posta.

![Bu öğe arkadaşlarınızla paylaşın](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Kendi açıklamalar ekleme](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="download"></a>İndirme
Machine Learning Studio çalışma alanınıza galerideki herhangi bir denemenin bir kopyasını indirebilirsiniz. Ardından, kendi çözümlerinizi oluşturmaya kopyanızı değiştirebilirsiniz.

Azure AI Gallery bir denemenin bir kopyasını almak için iki yol sunar:

* **Galeriden**. Galeride istediğiniz bir denemeyi bulursanız, bir kopyasını indirin ve Machine Learning Studio çalışma alanınızda açın.
* **Machine Learning Studio'da içinden**. Machine Learning Studio'da bir denemeyi galeride bir şablon olarak yeni bir deneme oluşturmak için kullanabilirsiniz.

### <a name="from-the-gallery"></a>Galeriden

1. Galeride deneme Ayrıntılar sayfasını açın.
2. Seçin **Studio'da Aç**.

    ![Galerisi'nden açık deneme](./media/gallery-experiments/open-experiment-from-gallery.png)

Seçtiğinizde, **Studio'da Aç**, denemeyi Machine Learning Studio çalışma alanınızda açılır. (Machine Learning Studio'ya henüz oturum açmadıysanız, ilk oturum açma için Microsoft hesabınızı kullanarak sorulur.)

### <a name="from-within-machine-learning-studio"></a>Machine Learning Studio'da içinden

1. Machine Learning Studio'da seçin **yeni**.
2. Seçin **deneme**. Bir galeri denemeleri listesinden seçim yapın veya kullanarak belirli bir deneme bulma **arama** kutusu.
3. Farenizi bir deneme sırasında üzerine getirin ve ardından **Studio'da Aç**. (Denemeler hakkında bilgi için seçin **galeri görünümünde**. Bu, galerideki deneme ayrıntıları sayfasına götürür.)

    ![Açık galeri deneme gelen Machine Learning Studio içinde](./media/gallery-experiments/open-experiment-from-studio.png)

Özelleştirme, yineleme ve indirilen bir denemeyi Machine Learning Studio'da oluşturduğunuz herhangi bir denemeyi gibi dağıtın.

![Studio'da açtığınız deneme](./media/gallery-experiments/experiment-open-in-studio.png)

## <a name="contribute"></a>Katkıda bulunma
Galeride oturum açtığınızda, Galeri topluluğun bir üyesi haline gelir. Topluluğun bir üyesi, diğer kullanıcıların keşfettiğinize göre çözümlerden yararlanabilmesi kendi deneyimlerinizi katkıda bulunabilir.

### <a name="publish-your-experiment-to-the-gallery"></a>Denemenizi galeride yayımladıkça

1. Machine Learning Studio için Microsoft hesabınızı kullanarak oturum açın.
2. Deneyiminizi oluşturun ve çalıştırın.
3. Deneme tuvalinin altındaki eylemler listesinden galeri'deki denemenizi yayımlamaya hazır olduğunuzda seçin **Galerisi Yayımla**.

    !["Galeride Yayımladıkça" seçin](./media/gallery-experiments/publish-experiment-to-gallery.png)
4. Üzerinde **deneme açıklama** sayfasında, bir başlık ve etiket girin. Başlık ve etiket açıklayıcı olun. Kullanılan teknikleri ya da gerçek sorun çözme vurgulayın. Bir tanımlayıcı deneme başlık örneğidir "ikili sınıflandırma: Twitter yaklaşım analizi."

    ![Yayımlama için başlık ve etiketleri girin](./media/gallery-experiments/experiment-description.png)
5. İçinde **Özet** kutusuna, denemenizi özetini girin. Denemeyi çözen sorun ve nasıl yaklaşıldığında kısaca açıklayın.
6. İçinde **ayrıntılı bir açıklama** kutusunda, her bir parçası denemenizi gerçekleştirdiğiniz adımları tanımlayın. Dahil etmek için bazı yararlı konular şunlardır:
   * Deneme grafiği ekran görüntüsü
   * Veri kaynakları ve açıklaması
   * Veri işleme
   * Özellik mühendisliği
   * Model açıklaması
   * Sonuçları ve model performansını değerlendirme

   Açıklamanızı biçimlendirmek için markdown'ı kullanabilirsiniz. Denemeyi yayımlandığında girişlerinizi deneme açıklama sayfasında nasıl görüneceğine bakmak için seçin **Önizleme**.

   ![Metninizi nasıl görüneceğini görmek için "Önizleme" seçin](./media/gallery-experiments/preview-markdown-text.png)

   > [!TIP]
   > Markdown düzenleme ve Önizleme küçük için sağlanan metin kutuları. Bir markdown Düzenleyicisi'nde deneme belgelerinize yazma kopyalayın ve Tamamlanan belgeler galerideki metin kutusuna yapıştırın öneririz. Denemenizi yayımladıktan sonra düzeltmeleri standart web tabanlı araçlar kullanarak düzenleme ve önizleme için markdown kullanma yapabilirsiniz.

7. Üzerinde **görüntü seçimi** denemeniz için bir küçük resim görüntüsünü seçin. Deneme ayrıntıları sayfasının ve deneme kutucuğunda en küçük resim görüntüsü görünür. Kullanıcıların galeri göz diğer kullanıcıların resmine görürsünüz. Bilgisayarınızdan bir resim yükleyin ya da Galeriden stok bir görüntü seçin.
    </br>
    ![Karşıya yükleme veya galeri için bir görüntü seçin](./media/gallery-experiments/select-gallery-image.png)
8. Üzerinde **ayarları** sayfasındaki **görünürlük**, içeriğinizi herkese açık şekilde yayımlamak isteyip istemediğinizi seçin (**genel**) mi, yoksa bir bağlantı sayfası (sahip kişiler için erişilebilir **Listelenmemiş**).

    ![Herkese açık şekilde veya olarak listelenmemiş yayımlamak isteyip istemediğinizi seçin](./media/gallery-experiments/choose-public-or-unlisted.png)

    <!-- -->

   > [!TIP]
   > Genel olarak yayınlamak önce belgelerinize doğru göründüğünden emin olmak istiyorsanız, ilk olarak denemeyi yayımlayabilirsiniz **Unlisted**. Daha sonra görünürlük ayarını değiştirebilirsiniz **genel** deneme Ayrıntıları sayfasında.
   >
   >
9. Denemeyi galerisinde yayımlamak için seçin **Tamam** işaretleyin.

    ![Denemeyi yayımlamak için Tamam onay işaretine seçin](./media/gallery-experiments/ok-checkmark.png)

Yüksek kaliteli galeri deneme yayımlama konusunda ipuçları için bkz: [belgeleme ve denemenizi yayımlama için ipuçları](#tips-for-documenting-and-publishing-your-experiment).

--Olan bitirdiniz.

Artık, galeride denemenizi görüntüleyebilir ve bağlantısını başkalarıyla paylaşın. Denemenizi kullanarak yayımladıysanız **genel** görünürlük ayarlama, denemeyi galeri göz atma ve arama sonuçlarında görünür. Deneme belgelerinize deneme ayrıntıları sayfasındaki Galerisine açtınız dilediğiniz zaman düzenleyebilirsiniz.

Katkılarınız listesini görmek için herhangi bir galeri sayfanın sağ üst köşedeki görüntünüzü seçin. Ardından, hesap sayfasını açmak için adınızı seçin.

![Hesabınızın adını seçin](./media/gallery-experiments/click-account-name.png)

### <a name="update-your-experiment"></a>Denemenizi güncelleştir
Gerekirse, galeride yayımlanmış bir denemeyi akışında (modüller, parametreleri vb.) için değişiklik yapabilirsiniz. Machine Learning Studio'da denemeye olun ve yeniden yayımlamak istediğiniz değişiklikleri yapın. Yayımlanan denemenizi yaptığınız değişikliklerle güncelleştirilir.

Denemenizde doğrudan galeri için aşağıdaki bilgileri değiştirebilirsiniz:

* Deney adı
* Özet ya da açıklaması
* Etiketler
* Görüntü
* Görünürlük ayarı (**genel** veya **Unlisted**)

Galeriden denemeler de silebilirsiniz.

Bu değişiklikleri yapmak veya deneme Ayrıntıları sayfasında veya galeride profil sayfanızdan denemeyi silin.


#### <a name="from-the-experiment-details-page"></a>Deneme Ayrıntılar sayfasından
Denemenizi ayrıntılarını değiştirmek için deneme Ayrıntıları sayfasında seçin **Düzenle**.

![Denemenizi düzenlemek için Düzenle'yi seçin](./media/gallery-experiments/edit-button.png)

Ayrıntılar sayfası, düzenleme moduna girer. Değişiklik yapmak için seçin **Düzenle** deney adı, Özet veya etiketleri yanında. Değişiklik yapmadan tamamladığınızda seçin **Bitti**.

![Ayrıntıları düzenlemek için "Düzenle" ve "bittiğinde bitti" seçin](./media/gallery-experiments/edit-details-page.png)

Denemeyi görünürlük ayarlarını değiştirmek için (**genel** veya **Unlisted**), veya denemeyi Galeriden silmek için işaretleyin **ayarları** simgesi.

!["Ayarlar" görünürlüğünü değiştirmek veya denemeyi silmek için işaretleyin](./media/gallery-experiments/settings-button.png)

#### <a name="from-your-profile-page"></a>Profil sayfanızdan
Profil sayfanızdan, deneme için aşağı oku seçin ve ardından **Düzenle**. Bu sizi düzenleme modunda denemeniz için ayrıntıları sayfasına götürür. Değişiklikleri yapmayı tamamladığınızda, seçin **Bitti**.

Galeriden denemeyi silmek için işaretleyin **Sil**.

!["Düzenle" veya "Sil" seçin](./media/gallery-experiments/edit-delete-buttons.png)

### <a name="tips-for-documenting-and-publishing-your-experiment"></a>Belgeleme ve denemenizi yayımlama için ipuçları
* Önceki veri bilimi deneyimi okuyucu var, ancak basit bir dil kullanmak üzere yararlı olabilir varsayabilirsiniz. Mümkün olduğunca ayrıntılı şeyler açıklanmaktadır.
* Cortana Intelligence Suite, nispeten yeni bir özelliktir. Tüm okuyucular ile nasıl kullanacağınızı biliyorsunuzdur. Yeterli bilgi ve denemenizi gidin okuyucular yardımcı olmak için adım adım açıklamalar sağlar.
* Görselleri yorumlamak ve deneme belgelerinize doğru şekilde kullanmak, okuyucular için yararlı olabilir. Görselleri deneme grafikleri ve veri ekran görüntüleri içerir. Deneme belgelerinize görüntüleri ekleme hakkında daha fazla bilgi için bkz. [yayımlama kılavuzları ve örnekler koleksiyonu](https://gallery.cortanaintelligence.com/Collection/Publishing-Guidelines-and-Examples-1).
* Denemenizde bir veri kümesi eklerseniz (diğer bir deyişle, verileri içeri aktarma modülü aracılığıyla veri kümesini aldığınız değil), veri kümesi denemenizin bir parçasıdır ve galerisinde yayımlanır. Lisanslama paylaşımı ve herkes tarafından indirme sağlayan koşulları yayımladığınız veri kümesi olduğundan emin olun. Gallery Katkıları altında Azure kapsamında [kullanım](https://azure.microsoft.com/support/legal/website-terms-of-use/).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Gönderme veya my deneme için görüntüyü düzenleme gereksinimleri nelerdir?**

Denemenizi ile gönderdiğiniz görüntüler katkılarınız için bir deneme kutucuğu oluşturmak için kullanılır. Görüntüleri bir aspect ratio 3: 2'in ve 960 çözünürlüğü 500 KB değerinden küçük olmasını öneririz &#215; 640.

**Deneme kullandım veri kümesinin ne olur? Veri kümesi, ayrıca galerisinde yayımlandı mı?**

Veri kümeniz denemenizi bir parçasıdır ve verileri içeri aktarma modülü alındığı değil, veri kümesi denemenizi bir parçası olarak galerisinde yayımlanır. Denemenizi ile yayımlama veri kümesi lisanslama koşullarına uygun olduğundan emin olun. Lisans koşullarını herkes paylaşmak ve verileri indirmek izin vermelidir.

**Azure HDInsight ya da SQL Server veri çekmek için verileri içeri aktarma modülü kullanan deneme var. Verileri almak üzere kimlik bilgilerimi kullanır. Bu tür bir deneme yayımlayabilirim? Nasıl miyim kimlik bilgilerimi paylaşılmaz olabilirsiniz?**

Şu anda galeride kimlik bilgilerini kullanan bir denemeyi yayımlanamıyor.

**Birden çok etiketi nasıl katılabilirim?**

Başka bir etiket girmek için bir etiket girdikten sonra SEKME tuşuna basın.

**[Galeriye Dön](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
