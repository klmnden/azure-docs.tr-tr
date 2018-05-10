---
title: Tek bit hızlı bir canlı akışı göndermek için FMLE Kodlayıcı yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar göndermek için Flash medya Canlı Kodlayıcı (FMLE) Kodlayıcı yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 055decd689e260a69651c5a3e1ce3f3f78b67340
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı bir canlı akışı göndermek için FMLE Kodlayıcı kullanın
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Elemental dinamik](media-services-configure-elemental-live-encoder.md)
> * [tricaster](media-services-configure-tricaster-live-encoder.md)
> * [wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Bu makalede nasıl yapılandırılacağı gösterilmektedir [Flash medya Canlı Kodlayıcı](http://www.adobe.com/products/flash-media-encoder.html) AMS bir tek bit hızlı akışın kanallar göndermek için (FMLE) Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetmek gösterilmiştir. Bu araç, yalnızca bir Windows Bilgisayarına çalışır. Mac veya Linux varsa, oluşturmak için Azure portalını kullanın [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

Bu öğretici, AAC kullanmayı açıklar. Ancak, FMLE değil destekler AAC varsayılan olarak. AAC uğradıysa MainConcept böyle kodlama için bir eklenti satın almanız gerekir: [AAC eklentisi](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

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

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Bir kanal adı belirtin açıklama alanı isteğe bağlıdır. Kanal ayarları altında seçin **standart** gerçek zamanlı kodlama seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin olun **yeni kanal Şimdi Başlat** seçilir.

3. Tıklatın **kanal oluşturmak**.

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> Kanal başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [Kodlayıcı yapılandırma](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

> [!IMPORTANT]
> Kanal hazır bir durumuna geçtiğinde hemen faturalama başladığını unutmayın. Daha fazla bilgi için bkz: [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_fmle_rtmp></a>FMLE Kodlayıcı yapılandırın
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
1. Gidin kullanılan makinede Flash medya Canlı Kodlayıcı 's (FMLE) arabirim.

    Bir ana sayfa ayarlarını arabirimidir. Önerilen ayarları FMLE kullanarak akış ile çalışmaya başlamak için aşağıdakileri not edin.

   * Biçimi: H.264 kare hızı: 30,00
   * Giriş boyutu: 1280 x 720
   * Bit hızı: 5000 (ağ sınırlamalar göre ayarlanabilir) KB/sn  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     Onay işareti "Taramasız Hale Getir'i" seçeneğini kullanarak kaynakları aralıklı, lütfen
2. İngiliz anahtarı simgesi biçimi yanındaki seçin, bu ek ayarlar olması gerekir:

   * Profil: ana
   * Düzeyi: 4.0
   * Ana kare sıklığı: 2 saniye cinsinden

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. Aşağıdaki önemli ses ayarını ayarlayın:

   * Biçim: AAC
   * Örnek Hızı: 44100 Hz
   * Bit hızı: 192 Kb/sn

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. Get kanal girdisini URL FMLE için 's atamak için **RTMP Endpoint**.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durumu değiştiğinden sonra **başlangıç** için **çalıştıran**, giriş URL'yi elde edebilirsiniz.

    Kanal çalıştırırken, kanal adına sağ tıklayın, üzerinden vurgulu aşağıya doğru gidin **Panoya Kopyala giriş URL** ve ardından **birincil giriş URL**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. Bu bilgileri **FMS URL** çıkış bölüm ve atama Akış adı alanı.

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Ek artıklık için ikincil giriş URL ile bu adımları yineleyin.
6. **Bağlan**’ı seçin.

> [!IMPORTANT]
> Tıklamadan önce **Bağlan**, size **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakın değil emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE Aracı'na gidin ve sınanacak kanalı sağ tıklatın. Menüden, üzerine gelerek **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Ardından akış player görünürse, kodlayıcı düzgün için AMS bağlanmak için yapılandırıldı.

Bir hata alırsanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.  

## <a name="create-a-program"></a>Bir program oluşturun
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracının içinde programı alanı sağ tıklatın ve seçin **Yeni Program Oluştur**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. Program adı ve gerekirse, ayarlamak **arşiv penceresi uzunluğu** (hangi varsayılan olarak 4 saat olarak). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **Program Şimdi Başlat** kutusu.
4. Tıklatın **Program oluşturma**.  

    >[!NOTE]
    >Program oluşturma kanal oluşturma daha az zaman alır.
        
5. Program çalışmaya başladıktan sonra program sağ tıklayarak ve giderek kayıttan yürütme onaylayın **kayıttan yürütme edinin** seçilerek **Azure Media Player ile**.  
6. Onaylandıktan sonra programı tekrar sağ tıklayın ve seçin **çıkış URL'yi Panoya Kopyala** (veya bu bilgileri almak **Program bilgilerine ve ayarlarına** seçeneği menüsünde).

Akış bir oynatıcı katıştırılmış veya dinamik görüntülemek için bir izleyici için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
