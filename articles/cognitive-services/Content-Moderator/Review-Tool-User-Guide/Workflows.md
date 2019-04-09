---
title: Tanımlama ve içerik iş akışları aracılığıyla - Content Moderator İnceleme aracını kullanma
titlesuffix: Azure Cognitive Services
description: Özel iş akışları ve içerik, ilkelere dayalı eşikleri tanımlamak için Azure Content Moderator iş akışı Tasarımcısı'nı kullanabilirsiniz.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 04/04/2019
ms.author: sajagtap
ms.openlocfilehash: 006f7d6691b8872aaa7ff8ccacff484585761d00
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263086"
---
# <a name="define-and-use-moderation-workflows"></a>Tanımlama ve denetleme iş akışlarını kullanma

Bu kılavuzda, ayarlama ve kullanma hakkında bilgi edineceksiniz [iş akışları](../review-api.md#workflows) üzerinde [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) Web sitesi. İçeriği daha verimli bir şekilde işlemek için kullanabileceğiniz bulut tabanlı özelleştirilmiş filtreler iş akışlarıdır. İş akışları çeşitli içerik farklı yollarla filtreleyebilir ve ardından uygun eylemi gerçekleştirin için hizmetler için bağlanabilirsiniz. Bu kılavuzun içeriği filtrelemek ve tipik denetimi senaryosunda incelemelere ayarlamak için (varsayılan olarak dahil edilir) Content Moderator bağlayıcısını kullanmayı gösterir.

## <a name="create-a-new-workflow"></a>Yeni bir iş akışı oluşturma

Git [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) ve oturum açın. Üzerinde **ayarları** sekmesinde **iş akışları**.

![İş Akışlarını ayarlama](images/2-workflows-0.png)

Sonraki ekranda seçin **iş akışı Ekle**.

![Bir iş akışı Ekle](images/2-workflows-1.png)

### <a name="assign-a-name-and-description"></a>Bir ad ve açıklama atayın

Akışınızı adlandırın, bir açıklama girin ve iş akışı, görüntü veya metin işleyeceğini seçin.

![İş akışı adı ve açıklaması](images/image-workflow-create.PNG)

### <a name="define-evaluation-criteria"></a>Değerlendirme ölçütleri tanımlayın

Sonraki ekranda, Git **varsa** bölümü. Üstteki açılan menüde seçin **koşul**. Bu iş akışı eylemi gerçekleştirir koşul yapılandırmanıza olanak tanır. Birden çok koşul kullanmak istiyorsanız, seçin **birleşimi** yerine. 

Ardından, bir bağlayıcı seçin. Bu örnekte **Content Moderator**. Seçtiğiniz bağlayıcı bağlı olarak, veri çıkışı için farklı seçenekler alırsınız. Bkz: [Bağlayıcılar](./configure.md#connectors) diğer bağlayıcılar hakkında bilgi edinmek için gözden geçirme aracı ayarları Kılavuzu'nun bölümü.

![İş akışı bağlayıcısını seçin](images/image-workflow-connect-to.PNG)

Kullanın ve bunu kontrol eden koşullar ayarlamak için istenen çıkış'ı seçin.

![İş akışı koşulunu tanımlama](images/image-workflow-condition.PNG)

### <a name="define-the-action"></a>Eylemi tanımlayın

Git **ardından** bölümünde, bir eylem burada seçersiniz. Aşağıdaki örnek, bir resim incelemesi oluşturur ve bir etiket atar. İsteğe bağlı olarak, bir alternatif (Else) yolu ekleyin ve bir eylem için de ayarlayın.

![İş akışı eylemi tanımlayın](images/image-workflow-action.PNG)

### <a name="save-the-workflow"></a>İş akışını kaydedin

İş akışı adını not edin; İş akışı API (aşağıya bakın) ile bir denetimi işi başlatmak için adı gerekir. Son olarak, iş akışını kullanarak kaydetmek **Kaydet** sayfanın üstünde düğme.

## <a name="test-the-workflow"></a>İş akışını test etme

Özel bir iş akışı tanımladığınıza göre örnek içerik ile test edin. Git **iş akışları** ve karşılık gelen **iş akışı yürütme** düğmesi.

![İş akışı test](images/image-workflow-execute.PNG)

Bunu kaydetmek [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg) yerel sürücünüze. Ardından **seçin dosyaları** ve görüntüyü karşıya yükleme iş akışı.

![Yansımaya koyulmuş bir teklif olan bir Çalıştırıcı](images/sample-text.jpg)

### <a name="track-progress"></a>İlerleme izleme

İş akışı ilerlemesini sonraki açılan pencerede görüntüleyebilirsiniz.

![İzleme iş akışı yürütme](images/image-workflow-job.PNG)

### <a name="verify-workflow-action"></a>İş akışı eylemi doğrula

Git **görüntü** sekmesinde altında **gözden** ve yeni oluşturulan resim incelemesi olduğunu doğrulayın.

![Görüntüleri inceleme](images/image-workflow-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, ayarlamak ve Content Moderator denetimi iş akışlarından kullanma hakkında bilgi edindiniz [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com). Ardından, bkz: [REST API Kılavuzu](../try-review-api-workflow.md) programlı olarak iş akışları oluşturmayı öğrenin.
