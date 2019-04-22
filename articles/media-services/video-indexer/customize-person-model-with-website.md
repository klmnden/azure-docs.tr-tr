---
title: Video Indexer Web sitesi, bir kişi modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer Web sitesiyle bir kişi model özelleştirme gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 03/19/2019
ms.author: anzaman
ms.openlocfilehash: 8dd535d97e40fe1dd4358d782db60940af1dd95d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58892849"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir kişi modeli özelleştirme

Video Indexer, video içeriği için ünlü tanıma destekler. Ünlü tanıma özelliği, yaklaşık IMDB Wikipedia ve üst LinkedIn öğrenilenler gibi sık istenen bir veri kaynağına göre bir milyon yüzleri kapsar. Ayrıntılı bir genel bakış için bkz. [Video Indexer bir kişi model özelleştirme](customize-person-model-overview.md).

Video Indexer Web sitesi, bu konuda açıklandığı bir video, algılanan yüzeylere düzenlemek için kullanabilirsiniz. Bölümünde anlatıldığı gibi API de kullanabilirsiniz [API'leri kullanarak bir kişi model özelleştirme](customize-person-model-with-api.md).

## <a name="central-management-of-person-models-in-your-account"></a>Merkezi Yönetim hesabınızda kişi model

1. Görüntülemek için düzenlemek ve hesabınızda kişi model silmek, Video Indexer Web sitesine gidin ve oturum açın.
2. Sayfanın sağ üst köşesindeki içerik modeli özelleştirme düğmesine tıklayın.

    ![İçerik modeli özelleştirme](./media/customize-face-model/content-model-customization.png)
3. Kişiler sekmesini seçin.

    Hesabınızdaki varsayılan Kişi modeli görürsünüz. Varsayılan kişi modeli düzenlenemez veya içgörüler videolarınızı olduğu için özel bir kişi model dizin oluşturma sırasında belirtmedi sürümündekilerle olabilirsiniz tüm yüzleri tutar. 

    Diğer kişi modelleri oluşturduysanız, bu sayfada listelenir. 

    ![İçerik modeli özelleştirme](./media/customize-face-model/content-model-customization-people-tab.png)

## <a name="create-a-new-person-model"></a>Yeni bir kişi model oluşturma

1. Tıklayın **+ Ekle modeli** düğmesi.

    ![Yeni bir kişi ekleyin](./media/customize-face-model/add-new-person.png)
2. Modelin adını girin ve adının yanındaki onay düğmesine tıklayın.

    ![Yeni bir kişi ekleyin](./media/customize-face-model/add-new-person2.png)

    Yeni bir kişi modeli oluşturdunuz. Bu gibi durumlarda, yüzleri artık yeni kişi modeline ekleyebilirsiniz.
3. Liste menü düğmesine tıklayıp seçin **+ Ekle kişi**.

    ![Yeni bir kişi ekleyin](./media/customize-face-model/add-new-person3.png)
    
## <a name="add-a-new-person-to-a-person-model"></a>Yeni bir kişi için kişi model ekleme

> [!NOTE]
> Video Indexer birden çok kişinin aynı ada sahip bir kişi model eklemenizi sağlar. Ancak, unque adları her kişi için modelinizde kullanılabilirliğini ve açıklık için vermeniz önerilir.

1. Yeni bir yüz kişinin modele eklemek için yüz için eklemek istediğiniz kişi model yanındaki listeyi menü düğmesine tıklayın.
1. Tıklayın **+ Ekle kişi** menüsünde.

    ![Yeni bir yüz kişinin Ekle](./media/customize-face-model/add-new-face.png)
 
    Bir açılır pencere kişinin ayrıntıları bölümünü doldurun isteyip istemediğinizi sorar. Kişinin adını yazın ve onay düğmesine tıklayın.

    ![Yeni bir yüz kişinin Ekle](./media/customize-face-model/add-new-face2.png)
 
Ardından, dosya Gezgini'nde seçin ya da sürükleyin ve yüz tanıma yüz tanıma görüntülerini bırakın. Video Indexer, tüm standart görüntü dosya türleri alır (örn: JPG, PNG ve daha fazlası).

Video Indexer, bu kişi oluşumlarını gelecekteki videolarda, bu, dizin ve, zaten, bu yeni bir yüz için eklenen kişi modeli kullanılarak dizine geçerli videoları algılayabilir. olmalıdır. Bir toplu işlem olarak geçerli videolarınızı kişinin tanıma, etkili olması için biraz zaman alabilir.


## <a name="rename-a-person-model"></a>Bir kişi modeli yeniden adlandır

Varsayılan kişi modeli de dahil olmak üzere, hesabınızdaki herhangi bir kişi modeli yeniden adlandırabilirsiniz. Varsayılan kişi modelinizi yeniden adlandır olsa bile, yine de hesabınızdaki varsayılan Kişi modeli olarak kullanılacak.

1. Yeniden adlandırmak istediğiniz kişi model yanındaki listeyi menü düğmesine tıklayın.
2. Tıklayın **Yeniden Adlandır** menüsünde.

    ![Bir kişi yeniden adlandır](./media/customize-face-model/rename-person.png)
3. Modelin geçerli adına tıklayın ve yeni adı yazın.

    ![Bir kişi yeniden adlandır](./media/customize-face-model/rename-person2.png)
4. Yeniden adlandırılacak modeliniz için onay düğmesine tıklayın.

## <a name="delete-a-person-model"></a>Bir kişi modeli Sil

Hesabınızda oluşturduğunuz herhangi bir kişi model silebilirsiniz. Ancak, varsayılan Kişi modelinizi silemezsiniz.

1. Tıklayın **Sil** menüsünde.

    ![Bir kişi Sil](./media/customize-face-model/delete-person.png)
    
    Bir açılır pencere görünür ve bu eylem kişi modeli ve tüm kişiler ve içerdiği dosyaları silecek bildirir. Bu eylem geri alınamaz. 

    ![Bir kişi Sil](./media/customize-face-model/delete-person2.png)
1. Emin değilseniz silmeyi tekrar tıklayın.

> [!NOTE]
> (Artık silinen) bu kişi modeli kullanarak sıralanan mevcut videoları videoda görünen yüzleri adlarını güncelleştirmek olanağını desteklemez. Bu videolarda yüzleri adlarını yalnızca başka bir kişi modeli kullanarak bunları yeniden sonra düzenlemeniz mümkün olacaktır. Bir kişi modeli belirtmeden yeniden dizin oluşturma, varsayılan modeli kullanılır. 

## <a name="manage-existing-people-in-a-person-model"></a>Var olan bir kişi modeli kişiler yönetme

Herhangi bir kişi Modellerinizi içeriğini aramak için kişi model adının yanındaki oka tıklayın.
Aşağı açılan, tüm kişiler, belirli bir kişi modelinde gösterir. Her kişilerin yanındaki listeyi menü düğmesine tıklarsanız, gördüğünüz yönetmek, yeniden adlandırma ve silme seçenekleri.  

![Yeni bir yüz kişinin Ekle](./media/customize-face-model/manage-people.png)

### <a name="rename-a-person"></a>Bir kişi yeniden adlandır

1. Bir kişi kişi modelinizdeki yeniden adlandırmak için liste menüsü düğmesini tıklatın ve seçin **Yeniden Adlandır** listesi menüsünde.
1. Yeni adı yazın ve kişi geçerli adına tıklayın.
1. Onay düğmesine tıklayın ve kişi olarak yeniden adlandırılacaktır.

### <a name="delete-a-person"></a>Bir kişi Sil

1. Bir kişi kişi modelinizden silmek için liste menüsü düğmesini tıklatın ve seçin **Sil** listesi menüsünde.
1. Bir açılır pencere, bu eylem kişiyi siler ve bu eylem geri alınamaz olduğunu bildirir.
1. Silme yeniden tıklayın ve bu kişinin kişi modelden kaldırır.

### <a name="manage-a-person"></a>Bir kişi yönetme

Tıklarsanız **Yönet**, bu kişi modeli gelen eğitilir tüm yüzleri görürsünüz. Bu yüzeyleri söz konusu kişinin bu kişi modeli kullandığınız videoları, oluşumunu veya el ile karşıya yüklediğiniz görüntüleri gelir. 

Ekle görüntüleri tıklayarak daha fazla yüzleri kişiye ekleyebilirsiniz.

Kişinin yeniden adlandırın ve kişi kişi modelden Sil, Yönet bölmesinde kullanabilirsiniz.

## <a name="use-a-person-model-to-index-a-video"></a>Bir video dizini oluşturmak için bir kişi modeli kullanır

Videonun karşıya yüklenmesi sırasında kişi modeli atayarak yeni videonuzu dizinlemeyi için bir kişi modeli kullanabilirsiniz.

Kişi model üzerinde yeni bir video kullanmak için aşağıdakileri yapın:

1. Tıklayın **karşıya** sayfanın üstündeki düğmesi.

    ![Karşıya Yükle](./media/customize-face-model/upload.png)
1. Video dosyanızın daire bırakın veya dosyanız için göz atın.
1. Gelişmiş Seçenekleri oka tıklayın.

    ![Karşıya Yükle](./media/customize-face-model/upload2.png)
1. Aşağı açılan tıklayın ve oluşturduğunuz kişi modeli seçin.

    ![Karşıya Yükle](./media/customize-face-model/upload3.png)
1. Sayfanın alt kısmındaki karşıya yükleme seçeneğe tıklayın ve yeni videonuzu kişi model kullanarak dizinlenir.

Video Indexer, karşıya yükleme sırasında bir kişi modeli belirtmezseniz varsayılan Kişi modeli hesabınızı kullanarak video dizinini oluşturacak.

## <a name="use-a-person-model-to-reindex-a-video"></a>Kişi model video yeniden kullanın 

Koleksiyonunuzdaki bir videoyu yeniden bir kişi modelini kullanmak için yeniden istediğiniz videoya adını Video Indexer giriş sayfası ve üzerine gelindiğinde kullanılacak hesabı videolarınızı gidin.

Düzenleme, silme ve videonuzu yeniden dizin oluşturma için seçenekler görürsünüz. 

1. Videonuzu yeniden dizin oluşturma seçeneğine tıklayın.

    ![Arat](./media/customize-face-model/reindex.png)

    Artık, videonuzu yeniden kişi modeli de seçebilirsiniz.
1. Açılan listeyi tıklatın ve kullanmak istediğiniz kişi modeli seçin. 

    ![Arat](./media/customize-face-model/reindex2.png)

1. Tıklayın **yeniden** düğmesi ve videonuzu indekslenmeli kişi model kullanarak.

Yüz algılandı ve yalnızca reindexed videoda tanınan yaptığınız herhangi bir yeni düzenleme videoyu yeniden dizin oluşturma için kullanılan kişi modelde kaydedilir.

## <a name="managing-people-in-your-videos"></a>Videolarınızı kişiler yönetme

Videoları düzenleme ve silme yüzleri dizin tanındı kişiler ve algılanan yüzeylere yönetebilirsiniz.

Bir yüz silinmesi, belirli bir yüzün video ınsights'tan kaldırır.

Bir yüzü düzenleme, algılanan ve büyük olasılıkla videonuza tanınan bir yüz yeniden adlandırır. Videoda bir yüzün düzenlediğinizde, bu adı video dizin oluşturma ve karşıya yükleme sırasında atanan kişi modelinde bir kişi girişi olarak kaydedilir.

Bir kişi modeli videoyu karşıya yüklenmesi sırasında atadığınız değil, hesabınızın varsayılan Kişi modelinde düzenlemeniz kaydedilir.

### <a name="edit-a-face"></a>Bir yüzü Düzenle


> [!NOTE]
> İki veya daha fazla farklı kişilerin aynı ada sahip bir kişi modeli varsa bu ada bu kişi modeli kullanarak videolardaki etiketlemek mümkün olmayacaktır. Yalnızca paylaşım kişiler sekmesine Video Indexer içerik modeli Özelleştirme sayfasında, bu adı kişiler değişiklik yapmak mümkün olacaktır. Bu nedenle, bu kişi modelinizde her kişi için benzersiz adlar verin önerilir.

1. Video Indexer Web sitesine gidin ve oturum açın.
1. Hesabınızdaki görüntülemek ve düzenlemek için istediğiniz bir video arayın.
1. Video yüz düzenlemek için Insights sekmesine gidin ve pencerenin sağ üst köşedeki kalem simgesine tıklayın.

    ![Video yüz Düzenle](./media/customize-face-model/edit-face.png)
1. Algılanan yüzeylere birini tıklatın ve "Bilinmeyen #X" (veya yüz için daha önce atanan adı) adlarını değiştirin. 
1. Yeni adını yazdıktan sonra yeni adının yanındaki onay simgesine tıklayın. Bu yeni adı kaydeder ve tanır ve bu diğer geçerli videolarınızı ve gelecekte, karşıya video yüz tüm oluşumlarını adları. Bu bir toplu işlem olduğu için geçerli diğer videolarınızı bir yüz tanıma, etkili olması için biraz zaman alabilir.

Video kullanan kişi modelinde var olan bir kişi adı ile bir yazıtipi adı, bu o kişinin ile ne modelde zaten var. Bu videoyu algılanan yüz görüntülerden birleştirecektir. Tamamen yeni bir adla bir yazıtipi adı, bu yeni bir kişi giriş video kullanan kişi modeli oluşturur. 

    ![Edit a face in your video](./media/customize-face-model/edit-face2.png)

### <a name="delete-a-face"></a>Bir yüzü Sil

Algılanan bir yüz video silmek için Insights bölmesine gidin ve bölmenin sağ üst köşedeki kalem simgesine tıklayın. Yüz tanıma, adı altında silme seçeneğine tıklayın. Bu video yüz algılanan kaldırır. Kişinin yüz hala göründüğü diğer videoları algılanır ancak dizine sonra bu videolardan yüzü silebilirsiniz. Adlı, kişi de özel kişi kişi modelden silmediğiniz sürece, yüz tanıma sildiğiniz video dizini oluşturmak için kullanılan kişi modelinde var olmaya devam eder.

![Video yüz Sil](./media/customize-face-model/delete-face.png)


## <a name="next-steps"></a>Sonraki adımlar

[Kişi modeli API'lerini kullanarak özelleştirme](customize-person-model-with-api.md)
