---
title: "Azure AI galeri özel modüller | Microsoft Docs"
description: "Azure AI Galerisi'ndeki özel makine öğrenme modülleri bulur."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: roopalik;garye
ms.openlocfilehash: 278a24c44b76e6df097355b91d94a146be4aa9a9
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="discover-custom-machine-learning-modules-in-azure-ai-gallery"></a>Özel machine learning modülleri Azure AI galerisinde Bul
[!INCLUDE [machine-learning-gallery-item-selector](../../../includes/machine-learning-gallery-item-selector.md)]

## <a name="custom-modules-for-machine-learning-studio"></a>Machine Learning Studio için özel modüller
Azure AI Galerisi sunar birkaç [özel modüller](https://gallery.cortanaintelligence.com/customModules) Azure Machine Learning Studio özelliklerini genişletin. Denemelerinizi, daha gelişmiş Tahmine dayalı analiz çözümleri geliştirmek için kullanmak üzere modülleri içeri aktarabilirsiniz.

Şu anda, Galeri modülleri teklifleri *zaman serisi analytics*, *ilişkilendirme kuralları*, *algoritmaları Kümeleme* (ötesinde k-ortalamaları), ve  *görselleştirmeleri*ve diğer workhorse yardımcı programı modüller.


## <a name="discover"></a>Keşif
Özel modüller göz atmak için [galerisinde](http://gallery.cortanaintelligence.com)altında **daha fazla**seçin **özel modüller**.

![Galeri giriş sayfasında özel modüller seçin](./media/gallery-custom-modules/select-custom-modules-in-gallery.png)

 **[Özel modüller](https://gallery.cortanaintelligence.com/customModules)**  sayfa yakın zamanda eklenen ve popüler modülleri listesini görüntüler. Tüm özel modüller görüntülemek için seçin **tümünü görmek** düğmesi. Belirli bir özel modülü için aranacak seçin **tümünü görmek**ve ardından filtre ölçütlerini. Arama terimlerini da girebilirsiniz **arama** kutusunu sayfanın üst kısmındaki Galerisi.

!["Tüm özel modüller göz atmak için select bkz: tüm"](./media/gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Anlama

Özel bir yayımlanan modülü nasıl çalıştığını anlamak için modül Ayrıntılar sayfası açmak için özel modülü seçin. Ayrıntılar sayfası tutarlı ve bilgilendirici öğrenme deneyimini sunar. Örneğin, modülün amacı Ayrıntılar sayfası vurgular ve beklenen giriş, çıkış ve parametreleri listeler. Ayrıntılar sayfası de inceleyin ve özelleştirme temel alınan kaynak kodu için bir bağlantı vardır.

### <a name="comment-and-share"></a>Açıklama ve paylaşma
Ayrıntılar sayfası, özel bir modül **açıklamaları** bölümünde, açıklama, geri bildirim sağlamak veya için modülü hakkında sorular sorun. Arkadaş veya iş arkadaşlarınızı Twitter ya da LinkedIn modülü bile paylaşabilirsiniz. Ayrıca sayfasını görüntülemek için diğer kullanıcıların davet etmek için modül ayrıntıları sayfasına bağlantı e-posta gönderebilirsiniz.

![Bu öğe arkadaşlarınızla paylaşın](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Kendi açıklamaları ekleme](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>İçeri Aktarma
Kendi denemeler herhangi bir özel modülü Galeriden içeri aktarabilirsiniz.

Azure AI galeri modülün bir kopyasını almak için iki yol sunar:

* **Galeriden**. Galeriden özel bir modülü içeri aktardığınızda, ayrıca modülü kullanmak nasıl bir örnek sağlar bir örnek denemeyi alırsınız.
* **Studio içinden makine**. Machine Learning Studio'da çalışırken herhangi bir özel modülü içeri aktarabilirsiniz (Bu durumda, örnek deneme alma).

### <a name="from-the-gallery"></a>Galeriden

1. Galeride modülü Ayrıntılar sayfasını açın. 
2. Seçin **Studio'da Aç**.
   
    ![Galeriden açık özel Modülü](./media/gallery-custom-modules/open-custom-module-from-gallery.png)
   
Her özel modülü modülü kullanmak nasıl oluşturulduğunu gösteren bir örnek denemeyi içerir. Seçtiğinizde, **Studio'da Aç**, bir örnek denemeyi Machine Learning Studio çalışma alanınızda açar. (Studio'ya zaten oturum açtınız, ilk oturum açma için Microsoft hesabınızı kullanarak sorulur.)

Örnek deneme ek olarak, özel modülü, çalışma alanına kopyalanır. Ayrıca, tüm, yerleşik veya özel Machine Learning Studio modülleri, modül paletindeki yerleştirilir. Başka bir modül çalışma alanınızdaki gibi kendi denemeler içinde artık kullanabilirsiniz.

### <a name="from-within-machine-learning-studio"></a>Studio içinden makine

1. Machine Learning Studio'da seçin **yeni**.
2. Seçin **Modülü**. Galeri modülleri listesinden seçin veya kullanarak belirli bir modülü bulmak **arama** kutusu.
3. Farenizi bir modülü üzerine getirin ve ardından **alma Modülü**. (Modülü hakkında bilgi almak için seçin **galeri görünümünde**. Bu sizi galerisinde modülü ayrıntıları sayfasına götürür.)
   
    ![Machine Learning Studio'ya özel modülünü içeri aktarın](./media/gallery-custom-modules/add-custom-module-in-studio.png)

Özel modülü, çalışma alanına kopyalanır ve, yerleşik veya özel Machine Learning Studio modülleri, modül paleti yerleştirilir. Başka bir modül çalışma alanınızdaki gibi kendi denemeler içinde artık kullanabilirsiniz.

## <a name="use"></a>Kullanım

Ne olursa olsun, modülü içeri aktardığınızda özel bir modülü içeri aktarmak için seçtiğiniz yöntem, modül, modül paleti Machine Learning Studio'da yerleştirilir. Modül paletinden başka bir modül gibi çalışma alanınızda hiçbir denemesinde özel modülünü kullanabilirsiniz.

İçeri aktarılan modül kullanmak için:

1. Bir deneme oluşturmak veya var olan bir denemeyi açın.
2. Modül paletindeki alanınızdaki özel modüller listesini genişletmek için seçin **özel**. Deneme tuvalinin sol için modül paletinin olur.
   
    ![Studio paletindeki Özel Modül listesi](./media/gallery-custom-modules/custom-module-in-studio-palette.png)
3. İçeri aktardığınız modülü seçin ve denemenizi için sürükleyin.


**[Galerisine gidin](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

