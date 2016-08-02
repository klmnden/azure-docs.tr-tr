<properties 
    pageTitle="Klasik Azure Portalı ile çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme" 
    description="Bu öğreticide, tek bit hızında bir canlı akışı alıp Klasik Azure Portalı’nı kullanarak çoklu bit hızında akışa kodlayan bir Kanal oluşturulması adım adım anlatılmaktadır." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="05/05/2016" 
    ms.author="juliako"/>


#Klasik Azure Portalı ile çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Bu öğreticide, tek bit hızında bir canlı akışı alıp çoklu bit hızında akışa kodlayan bir **Kanal** oluşturulması adım adım anlatılmaktadır.

>[AZURE.NOTE]Gerçek zamanlı kodlama için etkinleştirilmiş Kanallar ile ilgili daha fazla kavramsal bilgi için bkz. [Çoklu bit hızı akışları oluşturmak için Azure Media Services kullanarak canlı akış](media-services-manage-live-encoder-enabled-channels.md).

##Ortak Canlı Akış Senaryosu

Yaygın canlı akış uygulamaları oluşturmak için gerekli olan genel adımlar aşağıdadır.

>[AZURE.NOTE] Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.

1. Bilgisayara bir video kamera bağlayın. Şu protokollerin birinde tek bit hızlı bir akış çıkışı sağlayabilecek şirket içi bir gerçek zamanlı kodlayıcı başlatıp bunu yapılandırın: RTMP, Kesintisiz Akış veya RTP (MPEG-TS). Daha fazla bilgi için bkz. [Azure Media Services RTMP Desteği ve Gerçek Zamanlı Kodlayıcılar](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Bu adım, Kanalınızı oluşturduktan sonra da gerçekleştirilebilir.

1. Bir Kanal oluşturup başlatın. 

1. Kanal alma URL’sini alın. 

    Alma URL’si gerçek zamanlı kodlayıcı tarafından akışı Kanala göndermek için kullanılır.
1. Kanal önizleme URL’sini alın. 

    Kanalınızın canlı akışı düzgün şekilde aldığını doğrulamak için bu URL’yi kullanın.

3. Bir program oluşturun (ayrıca bir varlık oluşturur). 
1. Programı yayımlayın (ilişkili varlığa yönelik bir OnDemand bulucu oluşturulur).  

    İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.
1. Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın.
2. İsteğe bağlı olarak, gerçek zamanlı kodlayıcıya bir reklam başlatması bildirilebilir. Reklam, çıktı akışına eklenir.
1. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun.
1. Programı silin (ve isteğe bağlı olarak varlığı da silin).   

##Bu öğreticide

Bu öğreticide, Klasik Azure Portalı aşağıdaki görevleri gerçekleştirmek için kullanılır: 

2.  Akış uç noktalarını yapılandırma.
3.  Gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş bir kanal oluşturma.
1.  Gerçek zamanlı kodlayıcıya sağlamak üzere alım URL'sini alma. Gerçek zamanlı kodlayıcı, bu URL’yi kullanarak akışı Kanala alır. .
1.  Bir program (ve bir varlık) oluşturma
1.  Varlığı yayımlama ve akış URL'lerini alma  
1.  İçeriğinizi oynatma 
2.  Temizleme

##Ön koşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir.

- Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/).
- Bir Media Services hesabı. Media Services hesabı oluşturma konusunda bilgi edinmek için bkz. [Hesap Oluşturma](media-services-create-account.md).
- Bir web kamerası ve tek bit hızlı bir canlı akış gönderebilen bir kodlayıcı.

##Portalı kullanarak akış uç noktasını yapılandırma

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Bit hızı uyarlamalı akış ile video geçerli ağ bant genişliği, CPU kullanımı ve diğer etkenlere bağlı olarak görüntülendiğinden istemci, daha yüksek veya daha düşük bit hızlı bir akışa geçebilir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH ve HDS (yalnızca Adobe PrimeTime/Erişim lisans sahipleri için). 

Canlı akış ile çalışırken, şirket içi bir gerçek zamanlı kodlayıcı (bizim için Wirecast) çoklu bit hızlı bir canlı akışı kanalınıza alır. Akış bir kullanıcı tarafından istendiğinde Media Services, dinamik paketleme kullanarak kaynak akışı istenen bit hızı uyarlamalı akışa (HLS, DASH veya Kesintisiz) yeniden paketler. 

Dinamik paketlemeden yararlanmak için, içeriğinizi teslim etmek istediğiniz **akış uç noktası** için en az bir akış birimi almanız gerekir.

Akışa ayrılan birim sayısını değiştirmek için aşağıdakileri yapın:

1. [Klasik Azure Portalı](https://manage.windowsazure.com/)’nda **Media Services**’e tıklayın. Ardından, medya hizmetinin adına tıklayın.

2. AKIŞ UÇ NOKTALARI sayfasını seçin. Değiştirmek istediğiniz akış uç noktasına tıklayın.

3. Akış birim sayısını belirtmek için, ÖLÇEK sekmesini seçin ve **ayrılan kapasite** kaydırıcısını hareket ettirin.

    ![Ölçek sayfası](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-origin-scale.png)

4. Yaptığınız değişiklikleri kaydetmek için KAYDET düğmesine basın.

    Yeni birimlerin ayrılması yaklaşık 20 dakikada tamamlanır. 

     
    >[AZURE.NOTE] Şu anda, herhangi bir pozitif sayıda akış birimi bulunan bir durumdan hiçbir akış birimi bulunmamasına dönmek, akışı bir saate varan bir süre boyunca devre dışı bırakabilir.
    >
    > Maliyet hesaplamasında, 24 saatlik dönem için belirtilen en yüksek birim sayısı kullanılır. Fiyatlandırma ayrıntıları hakkında daha fazla bilgi için bkz. [Media Services Fiyatlandırma Ayrıntıları](http://go.microsoft.com/fwlink/?LinkId=275107).

 
##KANAL oluşturma

1.  [Klasik Azure Portalı](http://manage.windowsazure.com/)’nda Media Services’e ve ardından Media Services hesap adına tıklayın.
2.  KANALLAR sayfasını seçin.
3.  Yeni bir kanal eklemek için Ekle+ seçeneğine tıklayın.

**Standart** kodlama türlerini seçin. Bu tür, gerçek zamanlı kodlama için etkinleştirilmiş bir Kanal oluşturmak istediğinizi belirtir. Bu durumda, gelen tek bit hızlı akış Kanala gönderilir ve belirlenmiş gerçek zamanlı kodlayıcı ayarları kullanılarak çoklu bit hızına sahip bir akışa kodlanır. Daha fazla bilgi için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).

![standard0][standard0]

**Standart** kodlama türüne ait geçerli alma protokolü seçenekleri şunlardır:

- Tek bit hızlı Parçalanmış MP4 (Kesintisiz Akış)
- Tek bit hızlı RTMP
- RTP (MPEG-TS): RTP üzerinden MPEG-2 Aktarım Akışı.

Her protokole ait detaylı bilgiler için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).

![standard1][standard1]

Kanal veya ilişkili programları çalışıyorken giriş protokolünü değiştiremezsiniz. Farklı protokollere ihtiyacınız varsa her bir giriş protokolü için farklı bir kanal oluşturmalısınız.  

**Reklam Yapılandırma** sayfasında reklam işaretçi sinyallerinin kaynağını belirtebilirsiniz. Portal kullanırken yalnızca, Kanal içindeki gerçek zamanlı kodlayıcının zaman uyumsuz bir Reklam İşaretçisi API’yi dinlemesi gerektiğini gösteren API’yi seçebilirsiniz. Portal kullanırken yalnızca API’yi seçebilirsiniz.

Daha fazla bilgi için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).

![standard2][standard2]

**Kodlama Önayarı** sayfasında sistem önayarlarını seçebilirsiniz. Şu anda seçebileceğiniz tek sistem önayarı **Varsayılan 720p** seçeneğidir.

![standard3][standard3]

**Kanal Oluşturma** sayfasında, bu kanala video yayımlamasına izin verilen IP adreslerini tanımlayabilirsiniz. İzin verilen IP adresleri, tek bir IP adresi (ör. '10.0.0.1'), bir IP adresi ve CIDR alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1/22’) veya bir IP adresi ve noktalı ondalık alt ağ maskesi kullanarak bir IP aralığı (ör. ‘10.0.0.1(255.255.252.0)’) şeklinde belirtilebilir.

Herhangi bir IP adresi belirtilmezse ve bir kural tanımı yoksa hiçbir IP adresine izin verilmez. Tüm IP adreslerine izin vermek için, bir kural oluşturun ve 0.0.0.0/0 olarak ayarlayın.


![standard4][standard4]

>[AZURE.NOTE] Şu anda, bir Kanalın başlaması 30 dakika kadar sürebilir. Kanal sıfırlanması 5 dakika kadar sürebilir.

Kanalı oluşturduktan sonra, kanal yapılandırmalarınızı görüntüleyebileceğiniz **KODLAYICI** sekmesini seçebilirsiniz. Reklamları ve maskeleme görüntülerini de yönetebilirsiniz. 

![standard5][standard5]

Daha fazla bilgi için bkz. [Çoklu bit hızına sahip akışlar oluşturmak üzere Azure Media Services’i kullanarak canlı akış gerçekleştirme](media-services-manage-live-encoder-enabled-channels.md).


##Alma URL’leri alma

Kanal oluşturulduktan sonra, gerçek zamanlı kodlayıcıya sağlayacağınız alma URL’lerini alabilirsiniz. Kodlayıcı bu URL'leri canlı akış girişi için kullanır.

![readychannel](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ready-channel.png)


![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##Program oluşturma ve yönetme

###Genel Bakış

Kanal, bir canlı akıştaki segmentlerin yayımlanması ve depolanmasını denetlemenizi sağlayan programlarla ilişkilidir. Kanallar, Programları yönetir. Kanal ve Program arasındaki ilişki, kanalın sürekli bir içerik akışının bulunduğu ve programın bu kanalda zamanlanmış bir olayı kapsadığı geleneksel medyadaki ilişkiye benzer.

Program için kaydedilen içeriği kaç saat tutmak istediğinizi **Arşiv Penceresi** uzunluğunu ayarlayarak belirleyebilirsiniz. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir. Arşiv penceresi uzunluğu, istemcilerin geçerli canlı konumdan zamanda geri gidebilecekleri en uzun süre miktarını da belirler. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her program bir Varlık ile ilişkilidir. Programı yayımlamak için ilişkili varlığa yönelik bir OnDemand bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Yeni olaylar için mevcut programları yeniden kullanmamalısınız. Bunun yerine, her olay için yeni bir program oluşturup başlatın.

Akışı ve arşivlemeyi başlatmaya hazır olduğunuzda programı başlatın. Olay için akışı ve arşivlemeyi durdurmak istediğinizde programı durdurun. 

Arşivlenen içeriği silmek için, programı durdurup silin ve ardından ilişkili varlığı silin. Bir program tarafından kullanılıyorsa varlık silinemez; önce programın silinmesi gerekir. 

Programı durdurup sildikten sonra bile, varlığı silmediğiniz sürece kullanıcılar, arşivlenen içeriğinizin isteğe bağlı video olarak akışını gerçekleştirebilir.

Arşivlenen içeriği tutmak istiyor ancak bu içeriğin akış için kullanılmasını istemiyorsanız, akış bulucuyu silin.

###Program Oluşturma/Başlatma/Durdurma

Akışın Kanala akması sağlandıktan sonra bir Varlık, Program ve Akış Bulucu oluşturarak akış olayını başlatabilirsiniz. Bu olay, akışı arşivler ve akışın Akış Uç Noktası aracılığıyla izleyiciler tarafından kullanılabilmesini sağlar. 

Olayı başlatmanın iki yolu vardır: 

1. **KANAL** sayfasında **EKLE**’ye basarak yeni bir program ekleyin.

    Şunları belirleyin: program adı, varlık adı, arşiv penceresi ve şifreleme seçeneği.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    **Bu programı şimdi yayımla** seçeneğini işaretli bıraktıysanız YAYIMLAMA URL'leri oluşturulur.
    
    Programı akışla aktarmaya hazır olduğunuzda **BAŞLAT** tuşuna basabilirsiniz.

    Programı başlattıktan sonra, içeriği oynatmayı başlatmak için OYNAT’a basabilirsiniz.


    ![createdprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-created-program.png)

2. Alternatif olarak, bir kısayol kullanıp **KANAL** sayfasındaki **AKIŞI BAŞLAT** düğmesine basabilirsiniz. Böyle yapmak bir Varlık, Program ve Akış Bulucu oluşturur.

    Programa DefaultProgram adı verilir ve arşiv penceresi 1 saat olarak ayarlanır.

    Yayımlanan programı KANAL sayfasından oynatabilirsiniz. 

    ![channelpublish](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-channel-play.png)


**KANAL** sayfasında **AKIŞI DURDUR**’a tıklarsanız varsayılan program durdurulup silinir. Varlık, yerinde kalmaya devam eder ve **İÇERİK** sayfasından yayımlanabilir veya yayımdan kaldırılabilir.

**İÇERİK** sayfasına geçiş yaparsanız programlarınız için oluşturulan varlıkları görürsünüz.

![contentasset](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-content-assets.png)


##İçerik oynatma

Kullanıcınıza içeriğinizin akışını yapmak amacıyla kullanılabilecek bir URL sağlamak için, önce bir bulucu oluşturarak (Portalı kullanarak bir varlık yayımladığınızda bulucular sizin için oluşturulur) varlığınızı “yayımlamanız” (önceki bölümde açıklandığı şekilde) gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. 

İçeriğinizi kayıttan yürütmek üzere kullanmak istediğiniz akış protokolüne bağlı olarak, kanalın\programın **YAYIMLAMA URL'si** bağlantısından aldığınız URL'yi değiştirmeniz gerekebilir.

Dinamik paketleme, canlı akışın belirtilen protokole paketlenmesini halleder. 

Varsayılan olarak, akış URL’si aşağıdaki biçime sahiptir ve Kesintisiz Akış varlıklarını oynatmak için kullanılabilir:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS akış URL’si oluşturmak için, URL’ye (format=m3u8-aapl) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH akış URL’si oluşturmak için, URL’ye (format=mpd-time-csf) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

İçeriğinizi teslim etmek hakkında daha fazla bilgi için bkz. [İçerik teslim etme](media-services-deliver-content-overview.md).

[AMS Oynatıcısı](http://amsplayer.azurewebsites.net/azuremediaplayer.html)’nı kullanarak Kesintisiz Akışı kayıttan yürütebilir ya da iOS ve Android cihazlarını kullanarak HLS sürüm 3’ü oynatabilirsiniz.

##Temizleme

Olayların akışla aktarılmasını tamamlayıp önceden sağlanan kaynakları temizlemek istediğinizde aşağıdaki yordamı izleyin.

- Kodlayıcıdan akışı göndermeyi durdurun.
- Kanalı durdurun. Kanal durdurulduktan sonra herhangi bir ücret uygulanmaz. Tekrar başlatmanız gerektiğinde, aynı alma URL’sine sahip olacağından kodlayıcıyı yeniden yapılandırmanız gerekmez.
- Canlı olayınızın arşivini isteğe bağlı bir akış olarak sunmaya devam etmek istemiyorsanız Akış Uç Noktanızı durdurabilirsiniz. Kanalın durumu, durdurulmuş ise herhangi bir ücret uygulanmaz.
  

##Dikkat edilmesi gerekenler

- Canlı bir etkinlik için önerilen en uzun süre şu anda 8 saattir. Daha uzun bir süre için bir Kanal çalıştırmanız gerekiyorsa lütfen amslived@microsoft.com adresine başvurun.
- İçerik akışını gerçekleştirmek istediğiniz akış uç noktasında akışa ayrılan en az bir birim olduğundan emin olun.


##Media Services’i öğrenme yolları

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Geri bildirimde bulunma

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



[standard0]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard0.png
[standard1]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard1.png
[standard2]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard2.png
[standard3]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard3.png
[standard4]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard4.png
[standard5]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard_encode.png 


<!---HONumber=Jun16_HO2-->


