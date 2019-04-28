---
title: Azure portalını kullanarak şirket içi kodlayıcılarda canlı akış | Microsoft Docs
description: Bu öğretici, doğrudan teslimat için yapılandırılmış bir Kanal oluşturmaya ilişkin adımları anlatmaktadır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 9a8ab024443744f50482dd2ca1cfb33db43359e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463381"
---
# <a name="perform-live-streaming-with-on-premises-encoders-using-azure-portal"></a>Azure portalını kullanarak şirket içi kodlayıcılarla canlı akış gerçekleştirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Bu öğretici, Azure portal kullanarak doğrudan teslimat için yapılandırılmış bir **Kanal** oluşturmaya ilişkin adımları anlatmaktadır. 

## <a name="prerequisites"></a>Önkoşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
* Bir Media Services hesabı. Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-portal-create-account.md).
* Bir Web kamerası. Örneğin, [Telestream Wirecast kodlayıcı](https://www.telestream.net/wirecast/overview.htm).

Aşağıdaki makaleleri gözden geçirmeniz için önerilir:

* [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [Azure Media Services kullanarak Canlı Akış’a genel bakış](media-services-manage-channels-overview.md)
* [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md)

## <a id="scenario"></a>Ortak canlı akış senaryosu

Aşağıdaki adımlar, doğrudan teslimat için yapılandırılan kanalları kullanan ortak canlı akış uygulamaları oluşturmaya dahil olan görevleri açıklamaktadır. Bu öğretici, doğrudan geçiş kanalı ve canlı olayları oluşturmayı ve yönetmeyi gösterir.

> [!NOTE]
> İçerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumunda olduğundan emin olun. 
    
1. Bilgisayara bir video kamera bağlayın. <br/>Kurulum fikir edinmek için kullanıma [basit ve taşınabilir olay video dişli Kurulum]( https://link.medium.com/KNTtiN6IeT).
1. Çoklu bit hızlı RTMP ya da Parçalı MP4 akışı çıktısı veren bir şirket içi gerçek zamanlı kodlayıcı çalıştırın ve yapılandırın. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://go.microsoft.com/fwlink/?LinkId=532824).<br/>Ayrıca, bu bloguna göz atın: [Üretim OBS ile canlı akış](https://link.medium.com/ttuwHpaJeT).
   
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
1. Geçiş Kanalı oluşturun ve başlatın.
1. Kanal alma URL’sini alın. 
   
    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
1. Kanal önizleme URL’sini alın. 
   
    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
1. Canlı olay/program oluşturun. 
   
    Azure portalı kullanırken, canlı bir olay oluşturma bir varlık da oluşturur. 

1. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda, olayı/programı başlatın.
1. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
1. Olay akışını ve arşivlemeyi durdurmak istediğinizde, olayı/programı durdurun.
1. Olayı/programı silin (ve isteğe bağlı olarak varlığı silin).     

> [!IMPORTANT]
> Lütfen şirket içi kodlayıcılarda ve geçiş kanallarında canlı akışlarla ilgili kavramları ve dikkate alınması gereken noktaları öğrenmek için [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md) başlığını gözden geçirin.
> 
> 

## <a name="to-view-notifications-and-errors"></a>Bildirimleri ve hataları görüntülemek için
Azure portal tarafından oluşturulan bildirimleri ve hataları görüntülemek istiyorsanız, Bildirim simgesine tıklayın.

![Bildirimler](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a>Geçiş kanalları ve olayları oluşturma ve başlatma
Bir kanal, canlı akıştaki kesimleri yayımlamanızı ve depolamanızı denetlemenizi sağlayan olaylar/programlarla ilişkilidir. Kanallar olayları yönetir. 

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarında yürütülür, ancak pencere uzunluğunu aşan içerik sürekli olarak iptal edilir. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her olay bir varlıkla ilişkilidir. Olayı yayımlamak için ilişkili varlığa yönelik bir OnDemand bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üç olaya kadar destekler, böylece aynı gelen akışta birden fazla arşiv oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Mevcut canlı olayları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir olay oluşturun ve başlatın.

Akışa ve arşivlemeye hazır olduğunuzda olayı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, olayı durdurun ve ardından ilişkili varlığı silin. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

### <a name="to-use-the-portal-to-create-a-channel"></a>Bir kanal oluşturmak amacıyla portalı kullanmak için
Bu bölüm bir geçiş kanalı oluşturmak için **Hızlı Oluştur** seçeneğinin nasıl kullanılacağını gösterir.

Geçiş kanalları hakkında daha fazla ayrıntı için bkz. [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** penceresinde, **Canlı Akış**’a tıklayın. 
   
    ![Başlarken](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    **Canlı akış** penceresi görüntülenir.
3. RTMP alma protokolüyle bir geçiş kanalı oluşturmak için **Hızlı Oluştur**’a tıklayın.
   
    **YENİ KANAL OLUŞTUR** penceresi görüntülenir.
4. Yeni kanala bir ad verin ve **Oluştur**’a tıklayın. 
   
    Bunun yapılması RTMP alma protokolüyle bir geçiş kanalı oluşturur.

## <a name="create-events"></a>Olay oluşturma
1. Olay eklemek istediğiniz bir kanal seçin.
2. **Canlı Olay** düğmesine basın.

![Olay](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a>Alma URL’leri alma
Kanal oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

![Oluşturulan](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-the-event"></a>Olayı izleme
Olay izlemek için, Azure portalda **İzle**’ye tıklayın veya akış URL'sini kopyalayın ve tercih ettiğiniz bir oynatıcı kullanın. 

![Oluşturulan](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Canlı olay durduğunda otomatik olarak isteğe bağlı içeriğe dönüştürülür.

## <a name="clean-up"></a>Temizleme
Geçiş kanalları hakkında daha fazla ayrıntı için bkz. [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).

* Bir kanal yalnızca kanaldaki tüm olaylar/programlar durdurulduğunda durdurulabilir.  Kanal durdurulduktan sonra herhangi bir ücret uygulanmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Bir kanal yalnızca kanaldaki tüm canlı olaylar silindiğinde silinebilir.

## <a name="view-archived-content"></a>Arşivlenen içeriği görüntüleme
Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

Varlıklarınızı yönetmek için, **Ayar**’ı seçin ve **Varlıklar**’a tıklayın.

![Varlıklar](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

