---
title: "Hızlı Başlangıç: Content Moderator'ı kullanmaya başlama"
titlesuffix: Azure Cognitive Services
description: Content Moderator'ı kullanmaya başlamak nasıl.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: sajagtap
ms.openlocfilehash: f25434814a7fb3d0f49cab539b394970c9bcfb3b
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023449"
---
# <a name="quickstart-get-familiar-with-content-moderator"></a>Hızlı Başlangıç: Content Moderator ile tanıdık Al

Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator İnceleme aracını kullanır. Diğer hızlı başlangıçlar, bu hizmet, daha hızlı bir şekilde uygulamanızla tümleştirmek istiyorsanız bkz [sonraki adımlar](#next-steps) bölümü.

## <a name="prerequisites"></a>Önkoşullar

- Bir web tarayıcısı

## <a name="set-up-the-review-tool"></a>Gözden geçirme Aracı'nı ayarlama
Content Moderator İnceleme aracını İnsan gözden geçirenlerin bilişsel hizmet kararları yapılmasına izin veren bir web tabanlı bir araçtır. Bu kılavuzda, Content Moderator Service'in nasıl çalıştığını görebilmeniz için gözden geçirme Aracı'nı ayarlama kısa sürecinde yönlendirilir. Git [Content Moderator İnceleme aracı](http://contentmoderator.cognitive.microsoft.com/) site ve kaydolun.

![İçerik Moderator giriş sayfası](images/homepage.PNG)

## <a name="create-a-review-team"></a>Bir gözden geçirme takım oluştur

Ardından, gözden geçirme ekibi oluşturun. Bir çalışma senaryosunda, bu hizmetin denetimi kararları el ile gözden geçireceğiz kişi grubu olacaktır. Şu an için yalnızca bir takım adı oluşturmanız gerekir. Takım iş arkadaşlarınızı davet isterseniz, buraya e-posta adreslerini girerek bunu yapabilirsiniz.

![Takım üyesi davet et](images/QuickStart-2-small.png)

## <a name="upload-sample-content"></a>Örnek içerik yükleme

Örnek içerik yüklemek artık hazırsınız. Seçin **deneyin > Görüntü**, **deneyin > metin**, veya **deneyin > Video**.

![Görüntü veya metin denetimi deneyin](images/tryimagesortext.png)

İçerik denetleme için gönderin. İnceleme aracını yönetim API'lerini içeriğinizi taramak için dahili olarak çağırır. Tarama tamamlandığında, sonuçları gözden geçirmeniz için bekleyen olduğunu bildiren bir ileti görürsünüz.

![Orta dosyaları](images/submitted.png)

## <a name="review-moderation-tags"></a>Moderation etiketleri gözden geçirin

Uygulanan denetimi etiketleri gözden geçirin. İçeriğinize hangi etiketlerin uygulanan ve her kategoride puanı neydi görebilirsiniz.

![Sonuçları gözden geçirme](images/reviewresults_text.png)

Bir projede, veya gözden geçirme ekibinizle bu etiketleri değiştirebilir veya gerektiğinde daha fazla etiket ekleyebilir. Bu değişikliklerle göndermeniz **sonraki** düğmesi. Moderator API'leri iş uygulamanızı çağırır gibi etiketli içeriği burada kuyruk, insan tarafından İnceleme ekipleri tarafından incelenmesi için hazır. Hızlı bir şekilde içerik bu yaklaşımı kullanarak büyük hacimlerdeki gözden geçirebilirsiniz.

Bu noktada, Content Moderator hizmet neler yapabileceğinizi örnekler görmek için Content Moderator İnceleme aracını kullandınız. Ardından, gözden geçirme aracı ve gözden geçirme API'lerini kullanarak bir yazılım projesine tümleştirme hakkında daha fazla ya da bulabilir veya adımına atlayabilirsiniz [sonraki adımlar](#next-steps) bölümü yönetim API'leri kendilerini uygulamanızda kullanmayı öğrenin.

## <a name="learn-more-about-the-review-tool"></a>Gözden geçirme aracı hakkında daha fazla bilgi edinin

Content Moderator gözden geçirin Aracı'nı kullanma hakkında daha fazla bilgi için göz atın [İnsan içinde--döngüsü](Review-Tool-User-Guide/human-in-the-loop.md) yönlendirmesine ve gözden geçirme aracı API'leri insan tarafından İnceleme deneyimi ince ayar yapma hakkında bilgi edinmek için bkz:
- [İş API](try-review-api-job.md) yönetim API'lerini kullanarak içeriğinizi tarar ve gözden geçirmeleri gözden geçirme Aracı'nda oluşturur. 
- [Gözden geçirme API](try-review-api-review.md) doğrudan görüntü, metin veya görüntü incelemeleri İnsan Moderatörler için içerik taramadan oluşturur. 
- [İş akışı API](try-review-api-workflow.md) oluşturur, güncelleştirir ve ekibinizin oluşturduğu özel iş akışlarını ayrıntılarını alır.

Ya da kodunuzda yönetim API'leri kullanmaya başlamak için sonraki adımlar ile devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim API'leri kendilerini uygulamanızda kullanmayı öğrenin.
- Görüntü denetimi uygulayın. Kullanım [API Konsolu](try-image-api.md) veya [ C# hızlı](image-moderation-quickstart-dotnet.md) görüntülerini taramak ve olası yetişkinlere yönelik ve müstehcen içeriğin etiketleri, güven puanları ve diğer kullanarak algılamak için bilgi ayıklanır.
- Metin denetimi uygulayın. Kullanma [API Konsolu](try-text-api.md) veya [ C# Hızlı Başlangıç](text-moderation-quickstart-dotnet.md) olası küfür makine destekli istenmeyen metin sınıflandırma (Önizleme), metin içeriğini taramak ve kişisel bilgileri (PII). 
- Video denetimi uygulayın. Kullanım [ C# hızlı](video-moderation-api.md) videoları taramak ve olası yetişkinlere yönelik ve müstehcen içerikleri algılama için. 
