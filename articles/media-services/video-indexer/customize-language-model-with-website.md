---
title: Video Indexer Web dil modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, Video Indexer Web sitesiyle bir dil modelini özelleştirin gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: f98cdcab2d108f8dd9d40e3770498ad17b2a8a88
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799627"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir dil modelini özelleştirin

Video Indexer, konuşma tanıma uyarlama metinle, yani sözlük uyum sağlamak üzere altyapısı istediğiniz etki alanından karşıya yükleyerek özelleştirmek için özel dil modelleri oluşturmanıza olanak sağlar. Modelinizi eğitin sonra yeni sözcük uyarlama metinli tanınır. 

Ayrıntılı bir genel ve özel dil modelleri için en iyi yöntemler için bkz: [Video Indexer ile bir dil modelini özelleştirin](customize-language-model-overview.md).

Bu konuda açıklandığı gibi oluşturmak ve özel dil modelleri, hesabınızdaki düzenlemek için Video Indexer Web sitesi kullanabilirsiniz. Bölümünde anlatıldığı gibi API de kullanabilirsiniz [özelleştirme dil modeli API'lerini kullanarak](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Dil modeli oluşturma

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
2. Hesabınızdaki bir model Özelleştirme alanına tıklayarak **içerik modeli özelleştirmesi** sayfanın sağ üst köşesindeki düğmesine.

   ![İçerik modeli özelleştirme](./media/content-model-customization/content-model-customization.png)

3. Seçin **dil** sekmesi.

    Desteklenen dillerin listesini görürsünüz. 

    ![Dil modelini özelleştirin](./media/customize-language-model/customize-language-model.png)

4. İstediğiniz dil altında **Ekle modeli**.
5. Dil modeli ve isabet girmek için ad yazın.

    Bu modeli oluşturur ve modele metin dosyaları karşıya yükle seçeneği sunar.
6. Bir metin dosyası eklemek için tıklatın **Dosya Ekle**. Bu, dosya gezgini'nizi açar.

7. Gidin ve metin dosyasını seçin. Birden çok metin dosyaları için bir dil modeli ekleyebilirsiniz.

    Tıklayarak bir metin dosyasına ekleyebilirsiniz **...**  düğmesini dil modeli sağ tarafında ve seçerek **Dosya Ekle**.
8. İşiniz bittiğinde yeşil tıklayın metin dosyalarını karşıya yükleme **eğitme** seçeneği.

    ![Dil modeli eğitme](./media/customize-language-model/train-model.png)

Eğitim işlemini birkaç dakika sürebilir. Alıştırma tamamlandıktan sonra gördüğünüz **Trained** modelin yanında. Önizleme, indirin ve dosyayı modelden silin.

![Eğitilmiş dil modeli](./media/customize-language-model/preview-model.png)

### <a name="using-a-language-model-on-a-new-video"></a>Dil modeli kullanarak yeni bir video

Yeni bir video, dil modelini kullanmak için aşağıdakilerden birini yapın:

* Tıklayarak **karşıya** sayfanın üstündeki düğmesi 

    ![Karşıya Yükle](./media/customize-language-model/upload.png)
* Daire, ses veya video dosyası bırakın veya dosyanız için Gözat

    ![Karşıya Yükle](./media/customize-language-model/upload2.png)

Bu tercih yapma seçeneğine verecektir **Video kaynak dili**. Açılan tıklayın ve oluşturduğunuz listeden bir dil modeli seçin. Bu dil, dil modeli ve parantez içinde verdiğiniz ad yazması gerekir.

Tıklayın **karşıya** sayfasının ve yeni videonuzu alt seçeneği dizini dil modelinizi kullanarak.

### <a name="using-a-language-model-to-reindex"></a>Dil modeli yeniden kullanma

Dil modelinizin koleksiyonunuz bulunan bir videoyu yeniden kullanmak için Git, **hesap videoları** üzerinde [Video Indexer](https://www.videoindexer.ai/) giriş sayfası ve yeniden istediğiniz videoya adını üzerine gelin.

Video düzenleme, video, silme ve videonuzu yeniden dizin oluşturma için seçenekler görürsünüz. Videonuzu yeniden seçeneğine tıklayın.

![Arat](./media/customize-language-model/reindex1.png)

Bu belirleme seçeneği sağlar **Video kaynak dili** videonuzu yeniden. Açılan tıklayın ve oluşturduğunuz listeden bir dil modeli seçin. Bu dil, dil modeli ve parantez içinde verdiğiniz ad yazması gerekir.

![Arat](./media/customize-language-model/reindex.png)

Tıklayın **yeniden dizin** düğmesi ve videonuzu indekslenmeli dil modeli kullandığınız.

## <a name="edit-a-language-model"></a>Dil modeli Düzenle

Dil modeli adının değiştirilmesi, dosyaları eklemeyi ve dosyaları silmek düzenleyebilirsiniz.

Ekleyin veya dosyaları dil modelden silin, yeşil renkte tıklayarak modeli yeniden eğitme gerekecektir **eğitme** seçeneği.

### <a name="rename-the-language-model"></a>Dil modeli yeniden adlandır

Dil modeli adını tıklatarak değiştirebilirsiniz **...**  dil sağ tarafında model ve seçerek **Yeniden Adlandır**. 

Yeni tür adı ve isabet girin.

### <a name="add-files"></a>Dosyaları Ekle

Bir metin dosyası eklemek için tıklatın **Dosya Ekle**. Bu, dosya gezgini'nizi açar.

Gidin ve metin dosyasını seçin. Birden çok metin dosyaları için bir dil modeli ekleyebilirsiniz.

Tıklayarak bir metin dosyasına ekleyebilirsiniz **...**  düğmesini dil modeli sağ tarafında ve seçerek **Dosya Ekle**.

### <a name="delete-files"></a>Dosyaları silme

Dil modeli bir dosyayı silmek için tıklayın **...**  düğmesine tıklayın, metin dosyasının sağ tarafta ve belirleyin **Sil**. Bu silme işlemi geri alınamaz olduğunu belirten yeni bir pencere getirir. Tıklayın **Sil** yeni pencerede seçeneği.

Bu eylem dosya, dil modeli tamamen kaldırır.

## <a name="delete-a-language-model"></a>Dil modeli Sil

Dil modeli hesabınızdan silmek için tıklayın **...**  dil modeli sağ tarafında'düğmesine tıklayın ve belirleyin **Sil**.

Bu silme işlemi geri alınamaz olduğunu belirten yeni bir pencere getirir. Tıklayın **Sil** yeni pencerede seçeneği.

Dil modeli, bu eylem hesabınızdan tamamen kaldırır. Videoyu yeniden dizine kadar silinen dil modelini kullanan herhangi bir video aynı dizin tutar. Videoyu yeniden dizine eklerseniz, yeni bir dil modeli videoyu atayabilirsiniz. Aksi takdirde, Video Indexer videoyu yeniden dizine kendi varsayılan modelini kullanır. 

## <a name="customize-language-models-by-correcting-transcripts"></a>Dökümler düzelterek dil Modellerinizi özelleştirin

Video Indexer, videoların döküm için gerçek düzeltmeleri kullanıcı temelli modelleri olun dilinin otomatik özelleştirmeyi destekler.

1. Bir döküm yapmak için hesap videolarınızı düzenlemek istediğiniz videoyu açın. Seçin **zaman çizelgesi** sekmesi.

    ![Dil modelini özelleştirin](./media/customize-language-model/timeline.png)
1. Döküm transkripti düzenlemek için Kalem simgesine tıklayın. 

    ![Dil modelini özelleştirin](./media/customize-language-model/edits.png)

    Video Indexer, sizin tarafınızdan videonuzun transkripsiyonu düzeltilmiş ve bunları otomatik olarak ekler "döküm düzenlemeleri" adlı bir metin dosyasına tüm satırları yakalar. Bu düzenlemeler, bu video dizine eklemek için kullanılan belirli bir dil modeli yeniden eğitme için kullanılır. 
    
    Bu video dizine eklenirken, dil modeli belirtmediyseniz, bu video için tüm düzenlemeleri video içinde algılanan dilin hesabı uyarlamaları adlı varsayılan dil modeli içinde depolanır. 
    
    İçin aynı satırda birden çok düzenlemeler yapıldı durumunda, yalnızca son sürümü düzeltilmiş satır dil modeli güncelleştirmek için kullanılır.  
    
    > [!NOTE]
    > Yalnızca metin düzeltmeleri özelleştirme için kullanılır. Başka bir deyişle, gerçek sözcüklerin (örneğin, noktalama işaretleri veya alanları) içermeyen düzeltmeleri dahil edilmez. 
    
1. İçerik modeli Özelleştirme sayfasında dil sekmesinde göstermek döküm düzeltmeleri görürsünüz.

    ![Dil modelini özelleştirin](./media/customize-language-model/customize.png)

   Her dil Modellerinizi "Kimden"döküm düzenlemeleri dosyayı aramak için açmak için tıklayın. 

    ![Transkript düzenlemelerinden](./media/customize-language-model/from-transcript-edits.png)

## <a name="next-steps"></a>Sonraki adımlar

[Dil modeli API'lerini kullanarak özelleştirme](customize-language-model-with-api.md)
