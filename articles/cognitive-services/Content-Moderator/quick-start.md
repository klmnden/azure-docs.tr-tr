---
title: "Hızlı Başlangıç: Web - Content Moderator Content Moderator'ı deneyin"
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme Aracı'nı kullanır.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: sajagtap
ms.openlocfilehash: 31bbc59b0587bb71999c4b10d273f75942737be2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604142"
---
# <a name="quickstart-try-content-moderator-on-the-web"></a>Hızlı Başlangıç: Web üzerinde Content Moderator'ı deneyin

Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme Aracı'nı kullanır. Diğer hızlı başlangıçlar, bu hizmet, daha hızlı bir şekilde uygulamanızla tümleştirmek istiyorsanız bkz [sonraki adımlar](#next-steps) bölümü.

## <a name="prerequisites"></a>Önkoşullar

- Bir web tarayıcısı

## <a name="set-up-the-review-tool"></a>Gözden geçirme Aracı'nı ayarlama
Content Moderator İnceleme aracını İnsan gözden geçirenlerin bilişsel hizmet kararları yapılmasına izin veren bir web tabanlı bir araçtır. Bu kılavuzda, Content Moderator Service'in nasıl çalıştığını görebilmeniz için gözden geçirme Aracı'nı ayarlama kısa sürecinde yönlendirilir. Git [Content Moderator İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) site ve kaydolun.

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

Uygulanan denetimi etiketleri gözden geçirin. İçeriğinize hangi etiketlerin uygulanan ve her kategoride puanı neydi görebilirsiniz. Bkz: [görüntü](image-moderation-api.md), [metin](text-moderation-api.md), ve [Video](video-moderation-api.md) ne farklı içerik etiketleri hakkında daha fazla bilgi edinmek için denetleme konuları belirtin.

![Sonuçları gözden geçirme](images/reviewresults_text.png)

Bir projede, veya gözden geçirme ekibinizle bu etiketleri değiştirebilir veya gerektiğinde daha fazla etiket ekleyebilir. Bu değişikliklerle göndermeniz **sonraki** düğmesi. Moderator API'leri iş uygulamanızı çağırır gibi etiketli içeriği burada kuyruk, insan tarafından İnceleme ekipleri tarafından incelenmesi için hazır. Hızlı bir şekilde içerik bu yaklaşımı kullanarak büyük hacimlerdeki gözden geçirebilirsiniz.

Bu noktada, Content Moderator hizmet neler yapabileceğinizi örnekler görmek için Content Moderator İnceleme aracını kullandınız. Ardından, gözden geçirme aracı ve gözden geçirme API'lerini kullanarak bir yazılım projesine tümleştirme hakkında daha fazla ya da bulabilir veya adımına atlayabilirsiniz [sonraki adımlar](#next-steps) bölümü yönetim API'leri kendilerini uygulamanızda kullanmayı öğrenin.

## <a name="learn-more-about-the-review-tool"></a>Gözden geçirme aracı hakkında daha fazla bilgi edinin

Content Moderator gözden geçirme Aracı'nı kullanma hakkında daha fazla bilgi için göz atın [gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md) yönlendirmesine ve gözden geçirme aracı API'lerini, insan tarafından İnceleme deneyimi ince ayar yapma hakkında bilgi edinmek için bkz:
- [İş API](try-review-api-job.md) yönetim API'lerini kullanarak içeriğinizi tarar ve gözden geçirmeleri gözden geçirme Aracı'nda oluşturur. 
- [Gözden geçirme API](try-review-api-review.md) doğrudan görüntü, metin veya görüntü incelemeleri İnsan Moderatörler için içerik taramadan oluşturur. 
- [İş akışı API](try-review-api-workflow.md) oluşturur, güncelleştirir ve ekibinizin oluşturduğu özel iş akışlarını ayrıntılarını alır.

Ya da kodunuzda yönetim API'leri kullanmaya başlamak için sonraki adımlar ile devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim API'leri kendilerini uygulamanızda kullanmayı öğrenin.
- Görüntü denetimi uygulayın. Kullanım [API Konsolu](try-image-api.md) veya [ C# hızlı](image-moderation-quickstart-dotnet.md) görüntülerini taramak ve olası yetişkinlere yönelik ve müstehcen içeriğin etiketleri, güven puanları ve diğer kullanarak algılamak için bilgi ayıklanır.
- Metin denetimi uygulayın. Kullanma [API Konsolu](try-text-api.md) veya [ C# Hızlı Başlangıç](text-moderation-quickstart-dotnet.md) metin içeriği olası küfürleri, makine destekli istenmeyen metin sınıflandırma (Önizleme) ile kişisel veriler için tarayın.
- Video denetimi uygulayın. İzleyin [Video denetimi nasıl yapılır Kılavuzu C# ](video-moderation-api.md) videoları taramak ve olası yetişkinlere yönelik ve müstehcen içerikleri algılama için. 
