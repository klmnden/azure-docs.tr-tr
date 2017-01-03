---
title: "Azure portalı ile çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’ı kullanarak canlı akış gerçekleştirme | Microsoft Belgeleri"
description: "Bu öğreticide, tek bit hızında bir canlı akışı alıp Azure portalını kullanarak çoklu bit hızında akışa kodlayan bir Kanal oluşturulması adım adım anlatılmaktadır."
services: media-services
documentationcenter: 
author: anilmur
manager: erikre
editor: 
ms.assetid: 504f74c2-3103-42a0-897b-9ff52f279e23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: ff663f40507547ba561053b5c9a7a8ce93fbf213
ms.openlocfilehash: 99dfabcfcfcef69a43b45994cb4c729bd7faecff


---
# <a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Azure portalı ile çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)
> 
> 

Bu öğreticide, tek bit hızında bir canlı akışı alıp çoklu bit hızında akışa kodlayan bir **Kanal** oluşturulması adım adım anlatılmaktadır.

> [!NOTE]
> Gerçek zamanlı kodlama için etkinleştirilmiş Kanallar ile ilgili daha fazla kavramsal bilgi için bkz. [Çoklu bit hızı akışları oluşturmak için Azure Media Services kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).
> 
> 

## <a name="common-live-streaming-scenario"></a>Ortak Canlı Akış Senaryosu
Yaygın canlı akış uygulamaları oluşturmak için gerekli olan genel adımlar aşağıdadır.

> [!NOTE]
> Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
> 
> 

1. Bilgisayara bir video kamera bağlayın. Şu protokollerin birinde tek bit hızlı bir akış çıkışı sağlayabilecek şirket içi bir gerçek zamanlı kodlayıcı başlatıp bunu yapılandırın: RTMP, Kesintisiz Akış veya RTP (MPEG-TS). Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.
2. Bir Kanal oluşturup başlatın. 
3. Kanal alma URL’sini alın. 
   
    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
4. Kanal önizleme URL’sini alın. 
   
    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.
5. Bir olay/program oluşturun (ayrıca bir varlık oluşturur). 
6. Olayı yayımlayın (ilişkili varlığa yönelik bir OnDemand bulucu oluşturulur).  
   
    İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.
7. Akışa ve arşivlemeye hazır olduğunuzda olayı başlatın.
8. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
9. Olay akışını ve arşivlemeyi durdurmak istediğinizde, olayı durdurun.
10. Olayı silin (ve isteğe bağlı olarak varlığı silin).   

## <a name="in-this-tutorial"></a>Bu öğreticide
Bu öğreticide, Azure portalı aşağıdaki görevleri gerçekleştirmek için kullanılır: 

1. Akış uç noktalarını yapılandırma.
2. Gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş bir kanal oluşturma.
3. Gerçek zamanlı kodlayıcıya sağlamak üzere alım URL'sini alma. Gerçek zamanlı kodlayıcı, bu URL’yi kullanarak akışı Kanala alır. .
4. Bir olay/program (ve bir varlık) oluşturma
5. Varlığı yayımlama ve akış URL'lerini alma  
6. İçeriğinizi oynatma 
7. Temizleme

## <a name="prerequisites"></a>Ön koşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

* Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. 
  Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir Media Services hesabı. Media Services hesabı oluşturma konusunda bilgi edinmek için bkz. [Hesap Oluşturma](media-services-portal-create-account.md).
* Bir web kamerası ve tek bit hızlı bir canlı akış gönderebilen bir kodlayıcı.

## <a name="configure-streaming-endpoints"></a>Akış uç noktalarını yapılandırma
Media Services, çoklu bit hızlı MP4'leri şu akış biçimlerinde yeniden paketlemenize gerek kalmadan göndermenizi sağlayan dinamik paketleme olanağı sağlar: MPEG DASH, HLS, Kesintisiz Akış. Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için, içeriğinizi teslim etmek istediğiniz akış uç noktası için en az bir akış birimi almanız gerekir.  

Akışa ayrılan birim sayısını oluşturmak ve değiştirmek için, aşağıdakileri yapın:

1. [Azure portalında](https://portal.azure.com/) oturum açın ve AMS hesabınızı seçin.
2. **Ayarlar** penceresinde, **Akış uç noktaları**’na tıklayın. 
3. Varsayılan akış uç noktasına tıklayın. 
   
    **VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILAR** penceresi görüntülenir.
4. Akış birimi sayısını belirtmek için, **Akış birimleri** kaydırıcısını kaydırın.
   
    ![Akış birimleri](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)
5. Yaptığınız değişiklikleri kaydetmek için **Kaydet** düğmesine tıklayın.
   
   > [!NOTE]
   > Yeni birimleri ayırmanın tamamlanması 20 dakika sürebilir.
   > 
   > 

## <a name="create-a-channel"></a>KANAL oluşturma
1. [Azure portalında](https://portal.azure.com/), Media Services'i seçin ve ardından Media Services hesabınızın adına tıklayın.
2. **Canlı Akış**’ı seçin.
3. **Özel oluştur**’u seçin. Bu seçenek canlı kodlama için etkinleştirilmiş bir kanal oluşturmanızı sağlar.
   
    ![Kanal oluşturma](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
4. **Ayarlar**’a tıklayın.
   
   1. **Live Encoding** kanal türünü seçin. Bu tür, gerçek zamanlı kodlama için etkinleştirilmiş bir Kanal oluşturmak istediğinizi belirtir. Bu durumda, gelen tek bit hızlı akış Kanala gönderilir ve belirlenmiş gerçek zamanlı kodlayıcı ayarları kullanılarak çoklu bit hızına sahip bir akışa kodlanır. Daha fazla bilgi için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md). Tamam'a tıklayın.
   2. Bir kanalın adını belirtin.
   3. Ekranın alt kısmındaki Tamam’a tıklayın.
5. **Al** sekmesini seçin.
   
   1. Bu sayfada bir akış protokolü seçebilirsiniz. **Live Encoding** kanal türü için geçerli protokol seçenekleri şunlardır:
      
      * Tek bit hızlı Parçalanmış MP4 (Kesintisiz Akış)
      * Tek bit hızlı RTMP
      * RTP (MPEG-TS): RTP üzerinden MPEG-2 Aktarım Akışı.
        
        Her protokole ait detaylı bilgiler için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).
        
        Kanal veya ilişkili olayları/programları çalışıyorken protokol seçeneğini değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir akış protokolü için farklı bir kanal oluşturmalısınız.  
   2. Alma işlemine IP kısıtlaması uygulayabilirsiniz. 
      
       Bu kanala video almasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri, tek bir IP adresi (ör. '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1/22’) veya bir IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı (ör. '10.0.0.1(255.255.252.0)').
      
       Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.
6. **Önizleme** sekmesinde önizlemeye IP kısıtlaması uygulayın.
7. **Kodlama** sekmesinde kodlama ön ayarını belirtin. 
   
    Şu anda seçebileceğiniz tek sistem önayarı **Varsayılan 720p** seçeneğidir. Özel bir ön ayar belirtmek için bir Microsoft destek bileti açın. Ardından, sizin için oluşturulan ön ayarın adını girin. 

> [!NOTE]
> Şu anda, bir Kanalın başlaması 30 dakika kadar sürebilir. Kanal sıfırlanması 5 dakika kadar sürebilir.
> 
> 

Kanalı oluşturduktan sonr, Kanal’a tıklayıp **Ayarlar**’ı seçerek kanal yapılandırmalarınızı görüntüleyebilirsiniz. 

Daha fazla bilgi için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).

## <a name="get-ingest-urls"></a>Alma URL’leri alma
Kanal oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)

## <a name="create-and-manage-events"></a>Olay oluşturma ve yönetme
### <a name="overview"></a>Genel Bakış
Bir kanal, canlı akıştaki kesimleri yayımlamanızı ve depolamanızı denetlemenizi sağlayan olaylar/programlarla ilişkilidir. Olayları/programları kanallar yönetir. Kanal ve Program arasındaki ilişki, kanalın sürekli bir içerik akışının bulunduğu ve programın bu kanalda zamanlanmış bir olayı kapsadığı geleneksel medyadaki ilişkiye benzer.

Olay için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarında yürütülür, ancak pencere uzunluğunu aşan içerik sürekli olarak iptal edilir. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her olay bir Varlıkla ilişkilidir. Olayı yayımlamak için ilişkili varlığa yönelik bir OnDemand bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üç olaya kadar destekler, böylece aynı gelen akışta birden fazla arşiv oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, işiniz gereği bir olayın 6 saatini arşivlerken yalnızca son 10 dakikasını yayınlamanız gerekebilir. Bunu yapmak için, eşzamanlı olarak çalışan iki olay oluşturmanız gerekir. 6 saatlik olayı arşivlemek için bir olay ayarlanır ancak program yayımlanmaz. Diğer olay 10 dakika arşivlenecek şekilde ayarlanır ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir program oluşturup başlatın.

Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda bir olay/program başlatın. Olay akışını ve arşivlemeyi durdurmak istediğinizde, olayı durdurun. 

Arşivlenen içeriği silmek için, olayı durdurun ve ardından ilişkili varlığı silin. Olay tarafından kullanılan bir varlık silinemez; önce olayın silinmesi gerekir. 

Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

### <a name="createstartstop-events"></a>Olay oluşturma/başlatma/durdurma
Akışın Kanala akması sağlandıktan sonra bir Varlık, Program ve Akış Bulucu oluşturarak akış olayını başlatabilirsiniz. Bu olay, akışı arşivler ve akışın Akış Uç Noktası aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar. 

Olayı başlatmanın iki yolu vardır: 

1. **Kanal** sayfasında **Canlı Olay**’a basarak yeni bir olay ekleyin.
   
    Şunları belirleyin: olay adı, varlık adı, arşiv penceresi ve şifreleme seçeneği.
   
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
   
    **Bu canlı olayı şimdi yayımla** seçeneğini işaretli bıraktıysanız URL'leri YAYIMLAMA olayı oluşturulacaktır.
   
    Olayı yayınlamaya hazır olduğunuzda **Başlat** tuşuna basabilirsiniz.
   
    Olayı başlattıktan sonra, içeriği oynatmayı başlatmak için **İzle**’ye basabilirsiniz.
2. Alternatif olarak, bir kısayol kullanıp **Kanal** sayfasındaki **Yayını Başlat** düğmesine basabilirsiniz. Bunun yapılması varsayılan bir Varlık, Program ve Akış Bulucu oluşturur.
   
    Olaya **default** adı verilir ve arşiv penceresi 8 saat olarak ayarlanır.

Yayımlanmış olayı **Canlı olay** sayfasından izleyebilirsiniz. 

**Çevrimdışı** seçeneğine tıklarsanız tüm canlı olaylar durdurulur. 

## <a name="watch-the-event"></a>Olayı izleme
Olay izlemek için, Azure portalda **İzle**’ye tıklayın veya akış URL'sini kopyalayın ve tercih ettiğiniz bir oynatıcı kullanın. 

![Oluşturulan](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Canlı olay durduğunda otomatik olarak isteğe bağlı içeriğe dönüşür.

## <a name="clean-up"></a>Temizleme
Olayların akışla aktarılmasını tamamlayıp önceden sağlanan kaynakları temizlemek istediğinizde aşağıdaki yordamı izleyin.

* Kodlayıcıdan akışı göndermeyi durdurun.
* Kanalı durdurun. Kanal durdurulduktan sonra herhangi bir ücret uygulanmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
* Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Kanalın durumu, durdurulmuş ise herhangi bir ücret uygulanmaz.

## <a name="view-archived-content"></a>Arşivlenen içeriği görüntüleme
Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

Varlıklarınızı yönetmek için, **Ayar**’ı seçin ve **Varlıklar**’a tıklayın.

![Varlıklar](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
* İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Dec16_HO2-->


