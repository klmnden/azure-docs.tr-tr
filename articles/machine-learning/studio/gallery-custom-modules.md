---
title: Azure AI Gallery özel modüller - Azure Machine Learning Studio | Microsoft Docs
description: Azure AI Gallery'de özel machine learning modüllerinin keşfedin. Özel modüller, Azure Machine Learning Studio'da özelliklerini genişletin.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.openlocfilehash: 92067a93a1f67711df0312f4daf484a577ff14d1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273213"
---
# <a name="machine-learning-studio-discover-custom-machine-learning-modules-in-azure-ai-gallery"></a>Machine Learning Studio: Azure AI Gallery'de özel machine learning modüllerinin keşfedin

[Azure AI Gallery](http://gallery.cortanaintelligence.com) birkaç farklı [özel modüller](https://gallery.cortanaintelligence.com/customModules) Azure Machine Learning Studio'da özelliklerini genişletin. Daha da Gelişmiş Tahmine dayalı analiz çözümleri geliştirebilirsiniz denemelerinizi içinde kullanmak için modülleri içeri aktarabilirsiniz.

Şu anda üzerinde modüller Galerisi sunar *zaman serisi analizi*, *ilişkilendirme kuralları*, *algoritmaları Kümeleme* (ötesinde k-ortalamaları) ve  *görselleştirmeler*ve diğer workhorse yardımcı programı modüller.


## <a name="discover"></a>Keşif
Özel modüller göz atmak için [galerideki](http://gallery.cortanaintelligence.com)altında **daha fazla**seçin **özel modüller**.

![Özel modüller Galerisi giriş sayfasında seçin](./media/gallery-custom-modules/select-custom-modules-in-gallery.png)

**[Özel modüller](https://gallery.cortanaintelligence.com/customModules)** sayfası en son eklenen ve popüler modülleri listesini görüntüler. Tüm özel modüller görüntülemek için seçin **tümünü gör** düğmesi. Belirli bir özel modül için aranacak seçin **tümünü gör**ve ardından filtre ölçütleri. Arama terimlerini de girebilirsiniz **arama** Galerisi'nde sayfanın üst kısmındaki kutusu.

!["Tüm özel modüller göz atmak için select bakın tüm"](./media/gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Anlama

Yayımlanan bir özel modül nasıl çalıştığını anlamak için özel modül modül Ayrıntılar sayfasını açmak için seçin. Ayrıntılar sayfası, tutarlı ve bilgilendirici öğrenme deneyimini sunar. Örneğin, modülün amacı Ayrıntılar sayfası vurgular ve beklenen girişler, çıkışlar ve parametreleri listeler. Ayrıntılar sayfasını inceleyin ve özelleştirme, temel alınan kaynak koda bir bağlantı da vardır.

### <a name="comment-and-share"></a>Açıklama ve paylaşma
Ayrıntılar sayfasında, özel bir modülde **açıklamaları** bölümünde yapabilir yorum, geribildirim sağlamak veya modülü hakkında soru sorun. Arkadaş veya iş arkadaşlarınıza Twitter veya LinkedIn modülü bile paylaşabilirsiniz. Siz de sayfasını görüntülemek için diğer kullanıcıları davet etmek için modül ayrıntıları sayfasına bağlantı e-posta gönderebilirsiniz.

![Bu öğe arkadaşlarınızla paylaşın](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Kendi açıklamalar ekleme](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>İçeri Aktarma
İçin kendi deneyimlerinizi Galeriden herhangi özel bir modülü içeri aktarabilirsiniz.

Azure AI Gallery bir kopyasını bir modülü içeri aktarmak için iki yol sunar:

* **Galeriden**. Galeriden özel bir modülü içeri aktardığınızda, ayrıca modülü kullanmaya ilişkin bir örnek sağlar bir örnek denemeyi alın.
* **Machine Learning Studio'da içinden**. Machine Learning Studio'da çalışırken herhangi özel bir modülü içeri aktarabilirsiniz (Bu durumda, örnek deneme uygulanmaz).

### <a name="from-the-gallery"></a>Galeriden

1. Galeride modülü Ayrıntılar sayfasını açın. 
2. Seçin **Studio'da Aç**.
   
    ![Açık özel modül Galerisi](./media/gallery-custom-modules/open-custom-module-from-gallery.png)
   
Her özel modülü, modül kullanmayı gösteren bir örnek denemeyi içerir. Seçtiğinizde, **Studio'da Aç**, örnek deneme, Machine Learning Studio çalışma alanınızda açılır. (Studio'ya henüz oturum açmadıysanız, ilk oturum açma için Microsoft hesabınızı kullanarak sorulur.)

Örnek deneme ek olarak, çalışma alanınıza Özel Modül kopyalanır. Ayrıca, tüm, yerleşik veya özel Machine Learning Studio modülleri, modül paletindeki yerleştirilir. Çalışma alanınızdaki başka bir modül gibi kendi deneyimlerinizi içinde artık kullanabilirsiniz.

### <a name="from-within-machine-learning-studio"></a>Machine Learning Studio'da içinden

1. Machine Learning Studio'da seçin **yeni**.
2. Seçin **Modülü**. Bir galeri modülleri listesinden seçim yapın veya belirli bir modül kullanarak bulma **arama** kutusu.
3. Farenizi bir modülü üzerine getirin ve ardından **içeri aktarma modülü**. (Modülü hakkında bilgi almak için seçin **galeri görünümünde**. Bu, Galeri modülü ayrıntıları sayfasına götürür.)
   
    ![Özel Modül Machine Learning Studio'ya alma](./media/gallery-custom-modules/add-custom-module-in-studio.png)

Özel Modül çalışma alanınıza kopyalanır ve, yerleşik veya özel Machine Learning Studio modülleri, modül paletinin yerleştirilir. Çalışma alanınızdaki başka bir modül gibi kendi deneyimlerinizi içinde artık kullanabilirsiniz.

## <a name="use"></a>Kullanım

Modül içeri aktardığınızda, özel bir modülü içeri aktarmak için seçtiğiniz hangi yöntemin ne olursa olsun, modül, modül paletinin Machine Learning Studio'da yerleştirilir. Modül paletinden başka bir modül gibi çalışma alanınızdaki tüm deneme özel modülünü kullanabilirsiniz.

Bir içeri aktarılan modül kullanmak için:

1. Bir deney oluşturma veya var olan bir denemeyi açın.
2. Modül paletindeki çalışma alanınızdaki özel modüller listesini genişletmek için seçin **özel**. Deneme tuvalinin sol için modül paletinin olur.
   
    ![Özel Modül listesinde Studio paleti](./media/gallery-custom-modules/custom-module-in-studio-palette.png)
3. İçeri aktardığınız modülü seçin ve denemeniz için sürükleyin.


**[Galeriye Dön](http://gallery.cortanaintelligence.com)**



