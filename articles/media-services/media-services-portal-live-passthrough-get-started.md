---
title: "Azure portalı kullanarak şirket içi kodlayıcılarda canlı akış gerçekleştirme | Microsoft Belgeleri"
description: "Bu öğretici, doğrudan teslimat için yapılandırılmış bir Kanal oluşturmaya ilişkin adımları anlatmaktadır."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ec6bb243872b3d4794050f735122f587a299e978


---
# <a name="how-to-perform-live-streaming-with-onpremise-encoders-using-the-azure-portal"></a>Azure portal kullanarak şirket içi kodlayıcılarda canlı akış gerçekleştirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://msdn.microsoft.com/library/azure/dn783458.aspx)
> 
> 

Bu öğretici, Azure portal kullanarak doğrudan teslimat için yapılandırılmış bir **Kanal** oluşturmaya ilişkin adımları anlatmaktadır. 

## <a name="prerequisites"></a>Ön koşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
* Bir Media Services hesabı.    Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-portal-create-account.md).
* Bir Web kamerası. Örneğin, [Telestream Wirecast kodlayıcı](http://www.telestream.net/wirecast/overview.htm).

Aşağıdaki makaleleri gözden geçirmeniz için önerilir:

* [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [Azure Media Services kullanarak Canlı Akış’a genel bakış](media-services-manage-channels-overview.md)
* [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md)

## <a name="a-idscenarioacommon-live-streaming-scenario"></a><a id="scenario"></a>Ortak canlı akış senaryosu
Aşağıdaki adımlar, doğrudan teslimat için yapılandırılan kanalları kullanan ortak canlı akış uygulamaları oluşturmaya dahil olan görevleri açıklamaktadır. Bu öğretici, doğrudan geçiş kanalı ve canlı olayları oluşturmayı ve yönetmeyi gösterir.

1. Bilgisayara bir video kamera bağlayın. Çoklu bit hızlı RTMP ya da Parçalı MP4 akışı çıktısı veren bir şirket içi gerçek zamanlı kodlayıcı çalıştırın ve yapılandırın. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
2. Geçiş Kanalı oluşturun ve başlatın.
3. Kanal alma URL’sini alın. 
   
    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
4. Kanal önizleme URL’sini alın. 
   
    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Canlı olay/program oluşturun. 
   
    Azure portalı kullanırken, canlı bir olay oluşturma bir varlık da oluşturur. 
   
   > [!NOTE]
   > İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.
   > 
   > 
6. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda, olayı/programı başlatın.
7. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
8. Olay akışını ve arşivlemeyi durdurmak istediğinizde, olayı/programı durdurun.
9. Olayı/programı silin (ve isteğe bağlı olarak varlığı silin).     

> [!IMPORTANT]
> Lütfen şirket içi kodlayıcılarda ve geçiş kanallarında canlı akışlar ilgili kavramları ve dikkate alınması gereken konuları öğrenmek için [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md) başlığını gözden geçirin.
> 
> 

## <a name="to-view-notifications-and-errors"></a>Bildirimleri ve hataları görüntülemek için
Azure portal tarafından oluşturulan bildirimleri ve hataları görüntülemek istiyorsanız, Bildirim simgesine tıklayın.

![Bildirimler](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="configure-streaming-endpoints"></a>Akış uç noktalarını yapılandırma
Media Services, MPEG DASH, HLS veya Kesintisiz Akış HDS akış biçimlerinde yeniden paketlemenize gerek kalmadan çoklu bit hızlı MP4’ler göndermenizi sağlayan dinamik paketleme olanağı verir. Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir; Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için, içeriğinizi teslim etmek istediğiniz akış uç noktası için en az bir akış birimi almanız gerekir.  

Akışa ayrılan birim sayısını oluşturmak ve değiştirmek için, aşağıdakileri yapın:

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. **Ayarlar** penceresinde, **Akış uç noktaları**’na tıklayın. 
3. Varsayılan akış uç noktasına tıklayın. 
   
    **VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILAR** penceresi görüntülenir.
4. Akış birimi sayısını belirtmek için, **Akış birimleri** kaydırıcısını kaydırın.
   
    ![Akış birimleri](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)
5. Yaptığınız değişiklikleri kaydetmek için **Kaydet** düğmesine tıklayın.
   
   > [!NOTE]
   > Yeni birimleri ayırmanın tamamlanması 20 dakika sürebilir.
   > 
   > 

## <a name="create-and-start-passthrough-channels-and-events"></a>Geçiş kanalları ve olayları oluşturma ve başlatma
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

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Nov16_HO2-->


