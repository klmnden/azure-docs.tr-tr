---
title: Video Indexer Web sitesi markaları modeli - Azure'ı özelleştirmek için kullanın
titlesuffix: Azure Media Services
description: Bu makalede, bir Video Indexer Web sitesiyle markaları model özelleştirme gösterilmektedir.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 2522ede85c290fa238c0d5a5604d2dfbab984cdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60535563"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Video Indexer Web sitesiyle bir markaları modeli özelleştirme

Video Indexer, video ve ses içeriği ölçeklemek ve dizin oluşturma sırasında markaya algılama konuşma ve görsel metin destekler. Bahsetme ürünleri, hizmetleri, marka algılama özelliğini tanımlar ve şirketler Bing'in markaları veritabanı tarafından önerilen. Örneğin, Microsoft bir video veya ses içeriğini belirtilen veya bir video visual metinde görünür gerekiyorsa, Video Indexer, bir marka içerik olarak algılar. Özel bir markaları modeli markaları veritabanı, dışlama markaları engeller (aslında markaları, bloke liste oluşturmaya) algılanan belirli markaları Bing, Video Indexer algılar kullanılıp kullanılmayacağını seçmenize olanak sağlar ve modelinizi bir parçası olmalıdır markaları dahil et Bu, (temelde markaları beyaz listesi oluşturarak) Bing'e markaları veritabanında olmayabilir.

Ayrıntılı bir genel bakış için bkz. [genel bakış](customize-brands-model-overview.md).

Video Indexer Web sitesi oluşturun, kullanın ve bu konuda açıklandığı gibi bir videoda, algılanan özel markaları modelleri düzenlemek için kullanabilirsiniz. Bölümünde anlatıldığı gibi API de kullanabilirsiniz [markaları özelleştirin modeli API'lerini kullanarak](customize-brands-model-with-api.md).

## <a name="edit-the-settings-of-the-brands-model"></a>Markaları modeli ayarlarını düzenleyin  

Markaları algılanamayacak kadar Bing markaları veritabanından istediğiniz olup olmadığını belirleme seçeneğiniz var. Bu, markalar modelinizi ayarlarını düzenlemeniz gerekir.

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
2. Hesabınızdaki bir model Özelleştirme alanına tıklayarak **içerik modeli özelleştirmesi** sayfanın sağ üst köşesindeki düğmesine.
 
   ![İçerik modeli özelleştirme](./media/content-model-customization/content-model-customization.png) 
3. Markaları düzenlemek için seçin **markaları** sekmesi.

    ![Markaları modeli özelleştirme](./media/customize-brand-model/customize-brand-model.png)
4. Denetleme **Bing tarafından önerilen markaları Göster** seçenek Video Indexer'ı Bing tarafından önerilen markaları dahil etmek istiyorsanız. Video Indexer'ın, içeriğinizi Bing tarafından önerilen markaları algılamak için istemiyorsanız seçeneği işaretsiz bırakın. 

## <a name="include-brands-in-the-model"></a>Modelde markaları dahil et

**Markaları dahil** bölümü, Video Indexer'ı Bing tarafından önerilen değil olsa bile algılamak için kullanmak istediğiniz özel markaları temsil eder.  

### <a name="add-a-brand"></a>Bir marka Ekle

1. Tıklayın "+ marka Ekle".

    ![Markaları modeli özelleştirme](./media/customize-brand-model/add-brand.png)

    Bir ad (gerekli), kategori (isteğe bağlı) ve Açıklama (isteğe bağlı) ve URL'si (isteğe bağlı) başvuru.
    Kategori alanı, markalar etiketi yardımcı olmak için tasarlanmıştır. Bu alan marka ın gösterilir *etiketleri* Video Indexer API kullanırken. Örneğin, "Azure" marka etiketli veya "Bulut" olarak sınıflandırılmış.

    Başvuru URL'si, Wikipedia sayfasında bir bağlantı gibi bir marka için herhangi bir başvuru Web sitesinde olabilir.
2. "Marka Ekle" ye tıklayın ve bir marka için eklendiğini görürsünüz **markaları dahil** listesi.

### <a name="edit-a-brand"></a>Bir marka Düzenle

1. Düzenlemek istediğiniz marka yanındaki kalem simgesine tıklayın.

    Kategori, açıklama ve başvuru URL'si marka güncelleştirebilirsiniz. Markaları adlarının benzersiz olduğundan, marka adı değiştiremezsiniz. Marka adı değiştirmeniz gerekirse, tüm marka Sil (sonraki bölüme bakın) ve yeni adlı yeni bir marka oluşturun.
2. Tıklayın **güncelleştirme** düğmesini marka yeni bilgilerle güncelleştirin.

### <a name="delete-a-brand"></a>Bir marka Sil

1. Silmek istediğiniz marka yanındaki çöp kutusu simgesine tıklayın.
2. "Sil" e tıklayın ve marka artık görünür, *markaları dahil* listesi.

## <a name="exclude-brands-from-the-model"></a>Modelden markaları hariç tut

**Markaları hariç** bölümü, Video Indexer'ın algılama istediğiniz markalar temsil eder.

### <a name="add-a-brand"></a>Bir marka Ekle

1. Tıklayın "+ marka Ekle".

    Bir ad (gerekli), kategori (isteğe bağlı) girin.
2. "Marka Ekle" ye tıklayın ve bir marka için eklendiğini görürsünüz *markaları hariç* listesi.

### <a name="edit-a-brand"></a>Bir marka Düzenle

1. Düzenlemek istediğiniz marka yanındaki kalem simgesine tıklayın.

    Yalnızca bir marka kategorisini güncelleştirebilirsiniz. Markaları adlarının benzersiz olduğundan, marka adı değiştiremezsiniz. Marka adı değiştirmeniz gerekirse, tüm marka Sil (sonraki bölüme bakın) ve yeni adlı yeni bir marka oluşturun.
2. Tıklayın **güncelleştirme** düğmesini marka yeni bilgilerle güncelleştirin.

### <a name="delete-a-brand"></a>Bir marka Sil

1. Silmek istediğiniz marka yanındaki çöp kutusu simgesine tıklayın.
2. "Sil" e tıklayın ve marka artık görünür, *markaları hariç* listesi.

## <a name="next-steps"></a>Sonraki adımlar

[Markaları modeli API'lerini kullanarak özelleştirme](customize-brands-model-with-api.md)
