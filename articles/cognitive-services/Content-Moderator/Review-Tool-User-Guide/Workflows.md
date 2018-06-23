---
title: Tanımlama ve Azure içerik Aracı alanında iş akışlarını kullanma | Microsoft Docs
description: İçerik ilkelerinizi tabanlı özel iş akışları oluşturmayı öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/07/2018
ms.author: sajagtap
ms.openlocfilehash: dfe3ba8a2ef1bcbc69ef585b504a9367d9420bf0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351598"
---
# <a name="define-test-and-use-workflows"></a>Tanımlama, test ve iş akışlarını kullanma

Özel iş akışları ve içerik ilkelerinizi dayanarak eşikleri tanımlamak için API ve Azure içeriği yönetici iş akışı Tasarımcısı'nı kullanabilirsiniz.

İş akışları "içerik yönetici API'sine bağlayıcıları kullanarak bağlanma". Bu API için bir bağlayıcı varsa, diğer API'leri kullanabilirsiniz. Örneği burada, varsayılan olarak dahil içerik denetleyici bağlayıcı kullanır.

## <a name="browse-to-the-workflows-section"></a>İş Akışları bölümüne göz atın

Üzerinde **ayarları** sekmesine **iş akışları**.

  ![İş akışları ayarlama](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Yeni bir iş akışı Başlat

Seçin **iş akışını Ekle**.

  ![Bir iş akışını Ekle](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Bir ad ve açıklama atayın

İş akışınızı adı, bir açıklama girin ve iş akışı görüntü veya metin işleme seçin.

  ![İş akışı adı ve açıklaması](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>("Koşul") değerlendirme ölçütleri tanımlayın

Aşağıdaki ekran görüntüsünde, alanları ve tanımlamak için iş akışları için ihtiyacınız If-Then-Else seçimleri bakın. Bir bağlayıcı seçin. Bu örnekte **içerik denetleyici**. Seçtiğiniz Bağlayıcısı bağlı olarak, çıktı için kullanılabilir seçeneklerini değiştirin.

  ![İş akışı koşulu tanımla](images/ocr-workflow-step-2-condition.PNG)

Bağlayıcı ve istediğiniz çıktısını seçtikten sonra bir işleç ve değer koşulu için seçin.

## <a name="define-the-action-to-take"></a>Gerçekleştirilecek eylemi tanımlayın

Gerçekleştirilecek eylemi ve karşılamak için koşulu seçin. Aşağıdaki örnekte bir görüntü gözden geçirme oluşturur, bir etiket atar `a`ve gösterilen koşulunu vurgular. Ayrıca, istediğiniz sonuçları almak için birden çok koşul birleştirebilirsiniz. İsteğe bağlı olarak, bir alternatif (Else) yolunu ekleyin.

  ![İş akışı eylemi tanımlayın](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>İş akışınızı Kaydet

Son olarak, iş akışını kaydedin ve iş akışı adını not edin. Gözden geçirme API'sini kullanarak bir denetleme işi başlatmak için adı gerekir.

## <a name="test-the-workflow"></a>İş akışını test etme

Özel bir iş akışında tanımlanan, örnek içerikle sınayın.

Karşılık gelen seçin **iş akışı yürütme** düğmesi.

  ![İş akışı sınama](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

Kaydet [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) yerel diskinize. İş akışı test etme seçeneğini belirleyin **seçin dosyaları** ve görüntüyü karşıya yükleme.

  ![Görüntü dosyasını karşıya yükle](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>İş akışı izleme

Yürütülmeden gibi iş akışı izler.

  ![İzleme iş akışı yürütme](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>İnsan yönetimi için işaretlenen tüm görüntüleri gözden geçirin

Görüntünün gözden geçirin, Git görmek için **görüntü** altında sekmesinde **gözden**.

  ![Görüntüleri inceleme](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Sonraki adımlar 

Koddan iş akışının çağırmak için özel iş akışlarıyla kullanmak [ `Job` API konsol quickstart](../try-review-api-job.md) ve [.NET SDK'sı quickstart](../moderation-jobs-quickstart-dotnet.md).
