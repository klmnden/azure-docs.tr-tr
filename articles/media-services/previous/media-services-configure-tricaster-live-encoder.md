---
title: Tek bit hızlı bir canlı akışı göndermeyi NewTek TriCaster Kodlayıcı yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar göndermek için Tricaster gerçek zamanlı Kodlayıcı yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: ''
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 8084f32ac8cc2184d93796468ad66fb73398e876
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı bir canlı akışı göndermek için NewTek TriCaster Kodlayıcı kullanın
> [!div class="op_single_selector"]
> * [tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Elemental dinamik](media-services-configure-elemental-live-encoder.md)
> * [wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Bu makalede nasıl yapılandırılacağı gösterilmektedir [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) AMS bir tek bit hızlı akışın kanallar göndermek için gerçek zamanlı Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetmek gösterilmiştir. Bu araç, yalnızca bir Windows Bilgisayarına çalışır. Mac veya Linux varsa, oluşturmak için Azure portalını kullanın [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Tricaster gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar akış katkı olarak göndermek için kullanılırken, olabilir video/ses hatalardan Canlı olayınızın hızlı akışları arasında kesme veya grafikten maskeleme görüntülerini değiştirme gibi Tricaster belirli özelliklerini kullanır. AMS takım o zamana kadar bu sorunları çözmeye çalışıyor, bu özellikleri kullanmak için önerilir değil.
>
>

## <a name="prerequisites"></a>Önkoşullar
* [Bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Çalıştıran bir akış uç olduğundan emin olun. Daha fazla bilgi için bkz: [akış uç noktalarını yönetme Media Services hesabı](media-services-portal-manage-streaming-endpoints.md)
* En son sürümünü yüklemek [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) aracı.
* Aracı'nı başlatın ve AMS hesabınıza bağlanın.

## <a name="tips"></a>İpuçları
* Mümkün olduğunda, bir sabit Internet bağlantısı kullanır.
* Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
* Yazılım tabanlı kodlayıcılar kullanırken, gereksiz tüm programları kapatın.

## <a name="create-a-channel"></a>Kanal oluşturma
1. AMSE aracını gidin **canlı** sekmesini tıklatın ve içinde kanal alanı sağ tıklatın. Seçin **kanal oluştur...** menüden.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Bir kanal adı belirtin açıklama alanı isteğe bağlıdır. Kanal ayarları altında seçin **standart** gerçek zamanlı kodlama seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin olun **yeni kanal Şimdi Başlat** seçilir.

3. Tıklatın **kanal oluşturmak**.

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Kanal başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [Kodlayıcı yapılandırma](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Kanal hazır bir durumuna geçtiğinde hemen faturalama başlar. Daha fazla bilgi için bkz: [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_tricaster_rtmp></a>NewTek TriCaster Kodlayıcı yapılandırın
Bu öğreticide, aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

**Video**:

* Codec: H.264
* Profil: Yüksek (düzeyi 4.0)
* Bit hızı: 5000 KB/sn
* Anahtar kare: 2 saniye (60 saniye)
* Çerçeve oranı: 30

**Ses**:

* Codec: AAC (LC)
* Bit hızı: 192 Kb/sn
* Örnek Hızı: 44,1 kHz

### <a name="configuration-steps"></a>Yapılandırma adımları
1. Yeni bir **NewTek TriCaster** hangi video giriş kaynağı kullanılan bağlı olarak proje.
2. Bu projede bir kez bulma **akış** düğmesine tıklayın ve akış yapılandırma menüsüne erişmek için yanındaki dişli simgesine tıklayın.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Menü açtıktan sonra tıklatın **yeni** bağlantı başlığı altında. Bağlantı türü için istendiğinde seçin **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. **Tamam**’a tıklayın.
5. Bir FMLE profili şimdi açılan ok altında tıklayarak içeri aktarılabilir **akış profili** ve giderek **Gözat**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Yapılandırılan FMLE profili kaydedildiği gidin.
7. Seçin ve basın **Tamam**.

    Profil yüklendikten sonra sonraki adıma geçebilirsiniz.
8. Get kanal girdisini URL Tricaster atamak için **RTMP Endpoint**.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durumu değiştiğinden sonra **başlangıç** için **çalıştıran**, giriş URL'yi elde edebilirsiniz.

    Kanal çalıştırırken, kanal adı sağ tıklayın ve ardından aşağıya doğru vurgulu üzerinden gidin **Panoya Kopyala giriş URL** ve ardından **birincil giriş URL**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Bu bilgileri **konumu** altında **Flash Server** Tricaster projedeki. Ayrıca bir Akış adı atamak **akış kimliği** alan.

    Akış bilgi FMLE profiline eklediyseniz, aynı zamanda bu bölüme tıklayarak alınabileceği **ayarları içeri**tıklanarak ve kaydedilmiş FMLE profil gezinme **Tamam**. FMLE bilgileriyle ilgili Flash Server alanları doldurmanız gerekir.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Tamamlandığında tıklatarak **Tamam** ekranın altındaki. Video ve ses girişleri Tricaster içine hazır olduğunuzda tıklayarak AMS için akış başlamadan **akış** düğmesi.

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Tıklamadan önce **akış**, size **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakın değil emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme
AMSE Aracı'na gidin ve sınanacak kanalı sağ tıklatın. Menüden, üzerine gelerek **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Ardından akış player görünürse, kodlayıcı düzgün için AMS bağlanmak için yapılandırıldı.

Bir hata alırsanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.  

## <a name="create-a-program"></a>Bir program oluşturun
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracının içinde programı alanı sağ tıklatın ve seçin **Yeni Program Oluştur**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Program adı ve gerekirse, ayarlamak **arşiv penceresi uzunluğu** (hangi varsayılan olarak dört saate kadar). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **Program Şimdi Başlat** kutusu.
4. Tıklatın **Program oluşturma**.  

    >[!NOTE]
    >Program oluşturma kanal oluşturma daha az zaman alır.
        
5. Program çalışmaya başladıktan sonra program sağ tıklayarak ve giderek kayıttan yürütme onaylayın **kayıttan yürütme edinin** seçilerek **Azure Media Player ile**.  
6. Onaylandıktan sonra programı tekrar sağ tıklayın ve seçin **çıkış URL'yi Panoya Kopyala** (veya bu bilgileri almak **Program bilgilerine ve ayarlarına** seçeneği menüsünde).

Akış bir oynatıcı katıştırılmış veya dinamik görüntülemek için bir izleyici için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.

## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
