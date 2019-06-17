---
title: NewTek TriCaster Kodlayıcı, tek bit hızlı canlı akış göndermek için yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için Canlı Tricaster Kodlayıcı yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: cenkdin
manager: femila
editor: ''
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 6e09ce83296fccfbcb4a04913d55961e0da4de79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720793"
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a>NewTek TriCaster Kodlayıcı tek bit hızlı canlı akış göndermektir.  
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Bu makalede nasıl yapılacağı gösterilmektedir [NewTek TriCaster](https://newtek.com/products/tricaster-40.html) AMS tekli bit hızı akışına kanallar göndermek için gerçek zamanlı Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğreticide, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetilecek gösterilmektedir. Bu araç yalnızca Windows bilgisayarda çalışır. Mac veya Linux bilgisayarda ise oluşturmak için Azure portalını kullanma [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS akışındaki bir katkısı göndermek için Tricaster kullanırken olabilir görüntü/ses arızalardan Canlı etkinliğiniz Tricaster, hızlı akışları arasında kesme veya maskeleme görüntülerini gönderip buralardan değiştirme gibi belirli özelliklerini kullanıyorsanız. AMS takım o zamana kadar bu sorunları düzeltmek için çalışmaktadır, bunu bu özellikleri kullanmak için önerilmez.
>
>

## <a name="prerequisites"></a>Önkoşullar

* [Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Çalışan bir akış uç bulunduğundan emin olun. Daha fazla bilgi için [akış uç noktalarını yönetme Media Services hesabı](media-services-portal-manage-streaming-endpoints.md)
* En son sürümünü yükleyin [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) aracı.
* Aracı'nı başlatın ve AMS hesabınızı bağlayın.

## <a name="tips"></a>İpuçları

* Mümkün olduğunda, bir sabit internet bağlantısı kullanın.
* Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit hızlarına dönüştürme çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
* Yazılım tabanlı kodlayıcılar kullanırken, gereksiz tüm programları kapatın.

## <a name="create-a-channel"></a>Kanal oluşturma

1. AMSE Aracı'nda gidin **canlı** sekmesini tıklatıp içinde kanal alana sağ tıklayın. Seçin **kanal oluştur...** belirleyin.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Bir kanal adı belirtirseniz açıklama alanı isteğe bağlıdır. Kanal ayarları altında **standart** Live Encoding seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin **yeni kanal Şimdi Başlat** seçilir.

3. Tıklayın **kanal oluşturma**.

   ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> Kanalı başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [kodlayıcıyı Yapılandır](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Faturalandırma, kanalın bir hazır durumuna geçtiğinde hemen sonra başlar. Daha fazla bilgi için [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a name="a-idconfiguretricasterrtmpconfigure-the-newtek-tricaster-encoder"></a><a id="configure_tricaster_rtmp"/>NewTek TriCaster Kodlayıcı yapılandırın

Bu öğreticide, aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

**Video**:

* Codec: H.264
* Profil: Yüksek (düzeyi 4.0)
* Bit hızı: 5000 kbps
* Ana kare: 2 saniye (60 saniye)
* Kare hızı: 30

**Ses**:

* Codec: AAC (LC)
* Bit hızı: 192 kbps
* Örnek Hızı: 44,1 kHz

### <a name="configuration-steps"></a>Yapılandırma adımları

1. Yeni bir **NewTek TriCaster** proje bağlı olarak hangi video giriş kaynağı kullanılır.
2. Bu proje içinde bir kez bulmak **Stream** düğmesini ve yanında stream yapılandırma menüye erişmek için dişli simgesine tıklayın.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Menü açıldığında tıklayın **yeni** bağlantı başlığı. Bağlantı türü için sorulduğunda **Adobe Flash**.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. **Tamam**'ı tıklatın.
5. FMLE profili artık açılan ok altında tıklayarak aktarılabilen **akış profili** giderek **Gözat**.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Yapılandırılan FMLE profili kaydedildiği gidin.
7. Onu seçin ve basın **Tamam**.

    Profil karşıya yüklendikten sonra sonraki adıma geçin.
8. Kanal alma giriş URL'si için Tricaster atamak için **RTMP uç nokta**.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durum değiştiğinde **başlangıç** için **çalıştıran**, giriş URL'sini alabilirsiniz.

    Kanal çalıştırırken, kanal adına sağ tıklayın, üzerine gelindiğinde aşağı gidin **Panoya kopyalama giriş URL** seçip **birincil giriş URL**.  

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Bu bilgileri yapıştırın **konumu** altında **Flash Server** Tricaster projesi içinde. Ayrıca bir Akış adı atamak **Stream kimliği** alan.

    Akış bilgileri FMLE profiline eklediyseniz, bu da bu bölüme tıklayarak içeri aktarılabilir **içeri aktarma ayarları**, kaydedilmiş FMLE profiline gidin ve tıklayarak **Tamam**. FMLE bilgileriyle ilgili Flash Server alanları doldurmanız gerekir.

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. İşiniz bittiğinde tıklayın **Tamam** ekranın alt kısmındaki. Video ve ses girdiler Tricaster olarak hazır olduğunuzda AMS'ye tıklayarak akış başlamak **Stream** düğmesi.

     ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Tıklamadan önce **Stream**, size **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakmamaya emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE aracına gidin ve test edilecek kanal sağ tıklayın. Menüden üzerine **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Ardından akış Player'da görünüyorsa, kodlayıcı düzgün AMS'ye bağlanmak için yapılandırıldı.

Bir hata aldıysanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.  

## <a name="create-a-program"></a>Bir program oluşturma

1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracı içinde program alanı sağ tıklatın ve seçin **yeni bir Program oluşturma**.  

    ![Tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Program adı ve gerekirse ayarlamak **arşiv penceresi uzunluğu** (bunun varsayılan dört saate kadar). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **programı'nı şimdi başlatmak** kutusu.
4. Tıklayın **Program oluşturma**.  

    >[!NOTE]
    >Program oluşturma kanal oluşturmayı daha az zaman alır.
        
5. Program çalışmaya başladığında, kayıttan yürütme programa sağ tıklayıp giderek onaylayın **kayıttan yürütme programlarının** seçip **Azure Media Player ile**.  
6. Onaylandıktan sonra programı tekrar sağ tıklayıp **çıkış URL'sini Panoya Kopyala** (veya bu bilginin alındığı **Program bilgileri ve ayarları** menü seçeneğinden).

Akış bir oynatıcı içinde gömülü veya canlı görüntülemek için bir hedef kitle için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme

Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.

## <a name="next-step"></a>Sonraki adım

Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
