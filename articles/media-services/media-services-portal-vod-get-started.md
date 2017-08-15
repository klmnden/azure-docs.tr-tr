---
title: "Azure portalını kullanarak VoD göndermeye başlama | Microsoft Docs"
description: "Bu öğretici, Azure portalı kullanarak Azure Media Services (AMS) uygulaması ile temel bir İsteğe Bağlı Video (VoD) içerik teslim hizmeti uygulamanın adımlarını açıklar."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: cbb67ef92386a6288b3317bf77ebb67f15ce7fb2
ms.contentlocale: tr-tr
ms.lasthandoff: 08/08/2017

---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Azure portal kullanarak isteğe bağlı içerik göndermeye başlama
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Bu öğretici, Azure portalı kullanarak Azure Media Services (AMS) uygulaması ile temel bir İsteğe Bağlı Video (VoD) içerik teslim hizmeti uygulamanın adımlarını açıklar.

## <a name="prerequisites"></a>Ön koşullar
Öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
* Bir Media Services hesabı. Bir Media Services hesabı oluşturmak için bkz. [Media Services hesabı oluşturma](media-services-portal-create-account.md).

Bu öğretici aşağıdaki görevleri içerir:

1. Akış uç noktasını başlatın.
2. Bir video dosyası yükleyin.
3. Kaynak dosyayı uyarlamalı bit hızlı bir MP4 dosyaları grubuna kodlayın.
4. Varlığı yayımlayın, akış ve aşamalı indirme URL’lerini alın.  
5. İçeriğinizi oynatın.

## <a name="start-streaming-endpoints"></a>Akış uç noktalarını başlatma 

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri bit hızı uyarlamalı akış iletmektir. Media Services, bu akış biçimlerinin her birinin önceden paketlenmiş sürümlerini depolamanıza gerek kalmadan, uyarlamalı bit hızı MP4 ile kodlanmış içeriğinizi Media Services tarafından desteklenen akış biçimlerinde (MPEG DASH, HLS, Kesintisiz Akış) tam vaktinde göndermenize olanak tanıyan dinamik paketleme özelliğine sahiptir.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Akış uç noktasını başlatmak için aşağıdakileri yapın:

1. [Azure portal](https://portal.azure.com/)’da oturum açın.
2. Ayarlar penceresinde, Akış uç noktaları'na tıklayın. 
3. Varsayılan akış uç noktasına tıklayın. 

    VARSAYILAN AKIŞ UÇ NOKTASI AYRINTILARI penceresi görüntülenir.

4. Başlat simgesine tıklayın.
5. Yaptığınız değişiklikleri kaydetmek için Kaydet düğmesine tıklayın.

## <a name="upload-files"></a>Dosyaları karşıya yükleme
Azure Media Services kullanarak video akışı sağlamak için kaynak videoları karşıya yüklemeniz, bunları çoklu bit hızlarında kodlamanız ve sonucu yayımlamanız gerekir. İlk adım, bu bölümde ele alınmıştır. 

1. **Ayar** penceresinde **Varlıklar**’a tıklayın.
   
    ![Dosyaları karşıya yükleme](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. **Karşıya Yükle** düğmesine tıklayın.
   
    **Varlığı karşıya yükle** penceresi görüntülenir.
   
   > [!NOTE]
   > Dosya boyutu sınırlaması yoktur.
   > 
   > 
3. Bilgisayarınızda istediğiniz videoyu bulun, seçin ve Tamam'a tıklayın.  
   
    Karşıya yükleme başlar ve dosya adı altında ilerleme durumunu görebilirsiniz.  

Karşıya yükleme işlemi tamamlandıktan sonra, yeni varlık **Varlıklar** penceresinde listelenir. 

## <a name="encode-assets"></a>Varlıkları kodlama

Azure Media Services ile çalışırken en sık karşılaşılan senaryolardan biri, istemcilerinize bit hızı uyarlamalı akış iletmektir. Media Services şu bit hızı uyarlamalı akış teknolojilerini destekler: HTTP Canlı Akışı (HLS), Kesintisiz Akış, MPEG DASH. Videolarınızı uyarlamalı bit hızı akışına hazırlamak için, kaynak videonuzu çoklu bit hızı dosyalarına kodlamanız gerekir. Videolarınızı kodlamak için **Medya Kodlayıcısı Standart** kodlayıcıyı kullanmalısınız.  

Media Services, çoklu bit hızlı MP4’leri şu akış biçimlerinde yeniden paketlemenize gerek kalmadan göndermenizi sağlayan dinamik paketleme olanağı da sağlar: MPEG DASH, HLS, Kesintisiz Akış. Dinamik paketleme ile dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Dinamik paketlemeden yararlanmak için kaynak dosyanızı çoklu bit hızı MP4 dosyaları (kodlama adımları, bu bölümün devamında gösterilmiştir) grubu olarak kodlamanız gerekir.

### <a name="to-use-the-portal-to-encode"></a>Kodlama için portalı kullanmak üzere
Bu bölüm, içeriğinizi Medya Kodlayıcısı Standart ile kodlamak için atabileceğiniz adımları açıklar.

1. **Ayarlar** penceresinde **Varlıklar**’ı seçin.  
2. **Varlıklar** penceresinde kodlamak istediğiniz varlığı seçin.
3. **Kodla** düğmesine basın.
4. **Bir varlık kodla** penceresinde, seçin "Medya Kodlayıcısı Standart" işlemcisini ve bir ön ayarı seçin. Hazır ayarlar hakkında daha fazla bilgi için bkz. [Bit hızı merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) ve [MES Görev Ön Ayarları](media-services-mes-presets-overview.md). Hangi kodlama ön ayarlarının kullanıldığını denetlemeyi planlıyorsanız, şunu göz önünde bulundurun: Girdi videonuza en uygun olan ön ayarları seçmeniz önemlidir. Örneğin, girdi videonuzun 1920 x 1080 piksel çözünürlüğü olduğunu biliyorsanız, "H264 Çoklu Bit hızı 1080p" ön ayarını kullanabilirsiniz. Düşük çözünürlüklü (640 x 360) bir videonuz olması durumunda "H264 Çoklu Bit hızı 1080p" ön ayarını kullanmamalısınız.
   
   Daha kolay yönetim için, çıktı varlık adını ve işin adını düzenleme seçeneğine sahipsiniz.
   
   ![Varlıkları kodlama](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. **Oluştur**’a basın.

### <a name="monitor-encoding-job-progress"></a>Kodlama işi ilerleme durumunu izleme
Kodlama işinin ilerleme durumunu izlemek için, **Ayarlar** (sayfanın üst kısmında) ve ardından **İşler**’e tıklayın.

![İşler](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>İçerik yayımlama
Kullanıcınıza içeriğinizin akışını sağlamak veya indirmek için kullanılabilecek bir URL sağlamak üzere, önce bulucu oluşturarak varlığınızı “yayımlamanız” gerekir. Bulucular varlıkta bulunan dosyalara erişim imkanı sağlar. Media Services iki tür bulucuyu destekler: 

* Akış (OnDemandOrigin) bulucuları, uyarlamalı akış için kullanılır (örneğin, MPEG DASH, HLS ya da Kesintisiz Akış için). Akış bulucusu oluşturmak için varlığınız bir .ism dosyası içermelidir. 
* Aşamalı indirme aracılığıyla video teslimi için kullanılan aşamalı (SAS) bulucular.

Akış URL’si aşağıdaki biçime sahiptir ve bunu Kesintisiz Akış varlıklarını oynatmak için kullanabilirsiniz.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS akış URL’si oluşturmak için, URL’ye (format=m3u8-aapl) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH akış URL’si oluşturmak için, URL’ye (format=mpd-time-csf) ekleyin.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


SAS URL'leri aşağıdaki biçimdedir.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Mart 2015 öncesinde portalı kullanarak bulucu oluşturduysanız, iki yıllık bir sona erme tarihi olan bulucular oluşturulmuştur.  
> 
> 

Bir bulucunun sona erme tarihini güncelleştirmek için [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) ya da [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API’lerini kullanın. SAS bulucunun sona erme tarihini güncelleştirdiğinizde URL değişir.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Bir varlık yayımlamak için portal kullanmak üzere
Portalı kullanarak bir varlık yayımlamak için aşağıdakileri yapın:

1. **Ayarlar** > **Varlıklar**’ı seçin.
2. Yayımlamak istediğiniz varlığı seçin.
3. **Yayımla** düğmesine tıklayın.
4. Bulucu türünü seçin.
5. **Ekle**’ye basın.
   
    ![Yayımlama](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL **Yayımlanan URL’ler** listesine eklenir.

## <a name="play-content-from-the-portal"></a>Portaldan içerik oynatma
Azure portal, videonuzu test etmek için kullanabileceğiniz bir içerik oynatıcı sağlar.

İstediğiniz videoya tıklayın ve ardından **Oynat** düğmesine tıklayın.

![Yayımlama](./media/media-services-portal-vod-get-started/media-services-play.png)

Bazı dikkate alınması gereken noktalar vardır:

* Akışı başlatmak için **varsayılan** akış uç noktasını çalıştırmaya başlayın
* Videonun yayımlandığından emin olun.
* **Medya oynatıcı** varsayılan akış uç noktasından oynatılır. Varsayılan olmayan bir akış uç noktasından oynatmak istiyorsanız, URL'yi kopyalamak için tıklayın ve başka bir oynatıcı kullanın. Örneğin, [Azure Media Services Oynatıcı](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


