<properties 
    pageTitle="Azure portal kullanarak şirket içi kodlayıcılarda canlı akış gerçekleştirme | Microsoft Azure" 
    description="Bu öğretici, doğrudan teslimat için yapılandırılmış bir Kanal oluşturmaya ilişkin adımları anlatmaktadır." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="08/30/2016" 
    ms.author="juliako"/>


#Azure portal kullanarak şirket içi kodlayıcılarda canlı akış gerçekleştirme

Bu öğretici, Azure portal kullanarak doğrudan teslimat için yapılandırılmış bir **Kanal** oluşturmaya ilişkin adımları anlatmaktadır. 


##Ön koşullar

Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

- Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-create-account.md).
- Bir Web kamerası. Örneğin, [Telestream Wirecast kodlayıcı](http://www.telestream.net/wirecast/overview.htm).

Aşağıdaki makaleleri gözden geçirmeniz için önerilir:

- [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Azure Media Services kullanarak Canlı Akış’a genel bakış](media-services-manage-channels-overview.md)
- [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Ortak canlı akış senaryosu

Aşağıdaki adımlar, doğrudan teslimat için yapılandırılan kanalları kullanan ortak canlı akış uygulamaları oluşturmaya dahil olan görevleri açıklamaktadır. Bu öğretici, doğrudan geçiş kanalı ve canlı olayları oluşturmayı ve yönetmeyi gösterir.

1. Bilgisayara bir video kamera bağlayın. Çoklu bit hızlı RTMP ya da Parçalı MP4 akışı çıktısı veren bir şirket içi gerçek zamanlı kodlayıcı çalıştırın ve yapılandırın. Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.

1. Geçiş Kanalı oluşturun ve başlatın.
1. Kanal alma URL’sini alın. 

    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
1. Kanal önizleme URL’sini alın. 

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.

3. Canlı olay/program oluşturun. 

    Azure portalı kullanırken, canlı bir olay oluşturma bir varlık da oluşturur. 
      
    >[AZURE.NOTE]İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.
1. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda, olayı/programı başlatın.
2. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
1. Olay akışını ve arşivlemeyi durdurmak istediğinizde, olayı/programı durdurun.
1. Olayı/programı silin (ve isteğe bağlı olarak varlığı silin).     

>[AZURE.IMPORTANT] Lütfen şirket içi kodlayıcılarda ve geçiş kanallarında canlı akışlar ilgili kavramları ve dikkate alınması gereken konuları öğrenmek için [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md) başlığını gözden geçirin.

##Bildirimleri ve hataları görüntülemek için

Azure portal tarafından oluşturulan bildirimleri ve hataları görüntülemek istiyorsanız, Bildirim simgesine tıklayın.

![Bildirimler](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##Akış uç noktalarını yapılandırma 

Media Services, şu akış biçimlerine yeniden paketlemenize gerekle kalmadan, çoklu bit hızlı MP4’leri göndermenizi sağlayan dinamik paketleme olanağı verir: MPEG DASH, HLS veya Kesintisiz Akış HDS. Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için, içeriğinizi teslim etmek istediğiniz akış uç noktası için en az bir akış birimi almanız gerekir.  

Akışa ayrılan birim sayısını oluşturmak ve değiştirmek için, aşağıdakileri yapın:

1. **Ayarlar** penceresinde, **Akış uç noktaları**’na tıklayın. 

2. Varsayılan akış uç noktasına tıklayın. 

    **VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILAR** penceresi görüntülenir.

3. Akış birimi sayısını belirtmek için, **Akış birimleri** kaydırıcısını kaydırın.

    ![Akış birimleri](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Yaptığınız değişiklikleri kaydetmek için **Kaydet** düğmesine tıklayın.

    >[AZURE.NOTE]Yeni birimleri ayırmanın tamamlanması 20 dakika sürebilir.
    
##Geçiş kanalları ve olayları oluşturma ve başlatma

Bir kanal, canlı akıştaki kesimleri yayımlamanızı ve depolamanızı denetlemenizi sağlayan olaylar/programlarla ilişkilidir. Kanallar olayları yönetir. 
    
Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarında yürütülür, ancak pencere uzunluğunu aşan içerik sürekli olarak iptal edilir. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her olay bir varlıkla ilişkilidir. Olayı yayımlamak için ilişkili varlığa yönelik bir OnDemand bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üç olaya kadar destekler, böylece aynı gelen akışta birden fazla arşiv oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Mevcut canlı olayları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir olay oluşturun ve başlatın.

Akışa ve arşivlemeye hazır olduğunuzda olayı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, olayı durdurun ve ardından ilişkili varlığı silin. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

###Bir kanal oluşturmak amacıyla portalı kullanmak için 

Bu bölümler bir geçiş kanalı oluşturmak için **Hızlı Oluştur** seçeneğinin nasıl kullanılacağını gösterir.

Geçiş kanalları hakkında daha fazla ayrıntı için bkz. [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).

1. **Ayarlar** penceresinde, **Canlı Akış**’a tıklayın. 

    ![Başlarken](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    **Canlı akış** penceresi görüntülenir.

3. RTMP alma protokolüyle bir geçiş kanalı oluşturmak için **Hızlı Oluştur**’a tıklayın.

    **YENİ KANAL OLUŞTUR** penceresi görüntülenir.
4. Yeni kanala bir ad verin ve **Oluştur**’a tıklayın. 

    Bu, RTMP alma protokolüyle bir geçiş kanalı oluşturur.

    Kanal ayrıca varsayılan bir canlı olay/program ekler, başlatır ve yayımlar. Bu olay 8 saatlik arşiv penceresine sahip olacak şekilde yapılandırılır. 

    Daha fazla olay eklemek için, **Canlı Olay** düğmesine basın.

##Alma URL’leri alma

Kanal oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

![Oluşturulan](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##Bir olay izleme

Olay izlemek için, Azure portalda **İzle**’ye tıklayın veya akış URL'sini kopyalayın ve tercih ettiğiniz bir oynatıcı kullanın. 
 
![Oluşturulan](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Durduğunda, canlı olay otomatik olarak isteğe bağlı içeriğe dönüşür.

##Temizleme

Geçiş kanalları hakkında daha fazla ayrıntı için bkz. [Çoklu bit hızı akışları oluşturan şirket içi kodlayıcılarla canlı akış](media-services-live-streaming-with-onprem-encoders.md).

- Bir kanal yalnızca kanaldaki tüm olaylar/programlar durdurulduğunda durdurulabilir.  Kanal durdurulduktan sonra herhangi bir ücret uygulanmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
- Bir kanal yalnızca kanaldaki tüm canlı olaylar silindiğinde silinebilir.

##Arşivlenen içeriği görüntüleme

Olayı durdurduktan ve sildikten sonra dahi, varlığı silmeniz sürece, kullanıcılar arşivlenen içeriğinizin isteğe bağlı içerik olarak akışını gerçekleştirebilir. Bir olay tarafından kullanılıyorsa varlık silinemez; önce olayın silinmesi gerekir. 

Varlıklarınızı yönetmek için, **Ayar**’ı seçin ve **Varlıklar**’a tıklayın.

![Varlıklar](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##Media Services’i öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!--HONumber=ago16_HO5-->


