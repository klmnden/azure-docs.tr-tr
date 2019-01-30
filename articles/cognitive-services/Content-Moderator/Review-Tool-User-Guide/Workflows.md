---
title: Tanımlama ve içerik denetleme iş akışları - Content Moderator'ı kullanma
titlesuffix: Azure Cognitive Services
description: API ve Azure Content Moderator iş akışı Tasarımcısı'nı, özel iş akışları ve içerik, ilkelere dayalı eşikleri tanımlamak için kullanabilirsiniz.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 8fe380e3015e5b6929aebcb898eef44d6f6bceda
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213285"
---
# <a name="define-test-and-use-workflows"></a>Tanımlayın, test ve iş akışları kullanın

API ve Azure Content Moderator iş akışı Tasarımcısı'nı, özel iş akışları ve içerik, ilkelere dayalı eşikleri tanımlamak için kullanabilirsiniz.

İş akışları "Content Moderator API'si için bağlayıcı'yı kullanarak bağlan". Bu API için bir bağlayıcı varsa, diğer API'ler kullanabilirsiniz. Örnek, varsayılan olarak dahil edilir Content Moderator bağlayıcıyı kullanır.

## <a name="browse-to-the-workflows-section"></a>İş Akışları bölümüne göz atın

Üzerinde **ayarları** sekmesinde **iş akışları**.

  ![İş Akışlarını ayarlama](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Yeni bir iş akışını Başlat

Seçin **iş akışı Ekle**.

  ![Bir iş akışı Ekle](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Bir ad ve açıklama atayın

Akışınızı adlandırın, bir açıklama girin ve iş akışı görüntü veya metin işleme'yi seçin.

  ![İş akışı adı ve açıklaması](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Değerlendirme ölçütleri ("koşul") tanımlayın

Aşağıdaki ekran görüntüsünde, alanları ve iş akışlarınızı için tanımlamak için gereken If-Then-Else seçimleri bakın. Bir bağlayıcı seçin. Bu örnekte **Content Moderator**. Çıkış için kullanılabilir seçenekler seçtiğiniz bağlayıcı bağlı olarak değiştirin.

  ![İş akışı koşulunu tanımlama](images/ocr-workflow-step-2-condition.PNG)

Bağlayıcı ve istediğiniz çıktısını seçtikten sonra bir işleç ve değer koşulu seçin.

## <a name="define-the-action-to-take"></a>Gerçekleştirilecek eylemi tanımlayın

Gerçekleştirilecek eylemi ve karşılamak için koşulu seçin. Aşağıdaki örnek, bir resim incelemesi oluşturur, bir etiketi atar `a`ve gösterilen koşulunu vurgular. Ayrıca, istediğiniz sonuçları elde etmek için birden çok koşulu birleştirebilirsiniz. İsteğe bağlı olarak, bir alternatif (Else) yolunu ekleyin.

  ![İş akışı eylemi tanımlayın](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>Akışınızı kaydedin

Son olarak, iş akışını kaydedin ve iş akışı adını not edin. Gözden geçirme API'sini kullanarak bir denetimi işi başlatmak için adı gerekir.

## <a name="test-the-workflow"></a>İş akışını test etme

Özel bir iş akışında tanımlanan, örnek içerik ile test edin.

Buna karşılık gelen seçin **iş akışı yürütme** düğmesi.

  ![İş akışı test](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

Kaydet [örnek görüntü](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) yerel sürücünüze. İş akışı test etmek için seçin **seçin dosyaları** ve görüntüyü karşıya yükleme.

  ![Görüntü dosyasını yükleyin](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>İş akışı izleme

Yürütme sırasında iş akışını izleyin.

  ![İzleme iş akışı yürütme](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>İnsan tarafından denetim için işaretlenen herhangi bir görüntü gözden geçirin

Gözden geçirin, Git görüntüyü görmek için **görüntü** sekmesinde altında **gözden**.

  ![Görüntüleri inceleme](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Sonraki adımlar 

İş akışı koddan çağırmak için özel iş akışlarıyla kullanın [ `Job` API Konsolu hızlı](../try-review-api-job.md) ve [.NET SDK'sı hızlı](../moderation-jobs-quickstart-dotnet.md).
