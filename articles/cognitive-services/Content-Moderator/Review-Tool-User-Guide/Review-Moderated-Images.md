---
title: Gözden geçirme aracı - Content Moderator içerik incelemeleri kullanın
titlesuffix: Azure Cognitive Services
description: İnceleme aracını görüntüleri bir web Portalı'nda gözden geçirmek İnsan Moderatörler nasıl olanak tanıdığını öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/15/2019
ms.author: sajagtap
ms.openlocfilehash: a482ecf4a0d321525ab7e392695d2c4c0eebeadc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60629058"
---
# <a name="create-human-reviews"></a>İncelemelere oluşturma

Bu kılavuzda, nasıl ayarlanacağını öğreneceksiniz [incelemeleri](../review-api.md#reviews) gözden geçirme Aracı Web sitesi. Gözden geçirmeler depolayın ve değerlendirmek İnsan Moderatörler içeriğini görüntüler. Moderatörler, uygulanan etiket alter ve uygun şekilde kendi özel etiketler. Bir kullanıcı bir gözden geçirme tamamlandığında, sonuçları bir belirtilen geri çağırma uç noktasına gönderilir ve içeriği siteden kaldırılır.

## <a name="prerequisites"></a>Önkoşullar

- Oturum açma veya hesap üzerinde Content Moderator oluşturmak [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="image-reviews"></a>Görüntü incelemeleri

1. Git [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/)seçin **deneyin** sekme ve gözden geçirmek için bazı görüntüleri karşıya yükleyin.
1. Karşıya yüklenen görüntüleri işleme tamamladıktan sonra Git **gözden geçirme** sekmenize **görüntü**.

    ![İnceleme aracını gözden görüntü seçeneğinin vurgulandığı gösteren chrome tarayıcısı](images/review-images-1.png)

    Otomatik denetleme işlemi tarafından atanmış olan tüm etiketleri görüntüler. Diğer gözden geçirenler, gözden geçirme aracı gönderdiğiniz görüntüleri görünmez.

1. İsteğe bağlı olarak, taşıma **görüntülemek için gözden geçirmeleri** ekranında görüntülenen görüntülerinin sayısını ayarlamak için kaydırıcıyı (1). Tıklayarak **etiketli** veya **etiketlenmemiş** görüntüleri buna göre sıralamak için düğmeler (2). Açıp geçiş yapmak için bir etiket paneli (3) tıklayın.

    ![İnceleme aracını gözden geçirme için etiketli görüntülerle gösteren chrome tarayıcısı](images/review-images-2.png)

1. Görüntünün üzerinde daha fazla bilgi için üç noktayı seçip küçük resme tıklayın **ayrıntıları görüntüle**. Görüntü ile bir alt atayabilirsiniz **Taşı** seçeneği (bkz [takımlar](./configure.md#manage-team-and-subteams) alt ekipler hakkında daha fazla bilgi için bölümü).

    ![Seçeneğinin vurgulandığı görüntü görünümü ayrıntıları](images/review-images-3.png)

1. Görüntü denetimi bilgilerin ayrıntıları sayfasındaki göz atın.

    ![Ayrı bir bölmesinde listelenen denetimi ayrıntıları içeren bir görüntü](images/review-images-4.png)

1. Gözden geçirdikten ve gerektiğinde etiketi atamaları güncelleştirildi bitince **sonraki** , incelemeleri göndermek için. Gönderdikten sonra yaklaşık beş saniyeden tıklayın zorunda **önceki** düğmesine önceki ekrana dönün ve resimleri yeniden gözden geçirin. Bundan sonra artık gönderme kuyrukta görüntüleridir ve **önceki** düğmesi kullanılabilir artık.

## <a name="text-reviews"></a>Metin incelemeleri

Metin, görüntü incelemeleri işlevine benzer şekilde inceler. İçeriği karşıya yükleme, yerine, yalnızca yazma veya metin (en fazla 1024 karakter) yapıştırın. Ardından, Content Moderator metin analiz eder ve etiketleri (küfür ve kişisel veriler gibi diğer denetimi bilgileri) ek olarak uygular. Metin incelemeleri siz uygulanan etiketler geçiş veya gözden geçirme göndermeden önce özel etiketler.

![Metin bir Chrome tarayıcı penceresinde gözden geçirme aracı gösteren ekran görüntüsü bayrağı](../images/reviewresults_text.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, ayarlamak ve Content Moderator gözden geçirmeleri kullanma hakkında bilgi edindiniz [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com). Ardından, bkz: [REST API Kılavuzu](../try-review-api-review.md) veya [.NET SDK'sı Kılavuzu](../moderation-reviews-quickstart-dotnet.md) incelemeleri programlı oluşturmayı öğrenin.