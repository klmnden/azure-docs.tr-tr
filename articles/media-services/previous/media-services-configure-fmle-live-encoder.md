---
title: FMLE Kodlayıcı, tek bit hızlı canlı akış göndermek için yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için Flash Media Live Encoder (FMLE) Kodlayıcı yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 01bb628a6520488dcebf49a1e868213b955abc31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61466026"
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>FMLE Kodlayıcı tek bit hızlı canlı akış göndermektir. 
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
>
>

Bu makalede nasıl yapılacağı gösterilmektedir [Flash Media Live Encoder](https://www.adobe.com/products/flash-media-encoder.html) AMS tekli bit hızı akışına kanallar göndermek için (FMLE) Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğreticide, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetilecek gösterilmektedir. Bu araç yalnızca Windows bilgisayarda çalışır. Mac veya Linux bilgisayarda ise oluşturmak için Azure portalını kullanma [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

Bu öğreticide, AAC kullanmayı açıklar. Ancak, FMLE değil destekler AAC varsayılan olarak. AAC gibi MainConcept kodlama için bir eklenti satın almanız gerekecektir: [AAC eklentisi](https://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

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

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Bir kanal adı belirtirseniz açıklama alanı isteğe bağlıdır. Kanal ayarları altında **standart** Live Encoding seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin **yeni kanal Şimdi Başlat** seçilir.

3. Tıklayın **kanal oluşturma**.

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> Kanalı başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [kodlayıcıyı Yapılandır](media-services-configure-fmle-live-encoder.md).

> [!IMPORTANT]
> Kanal bir hazır durumuna geçtiğinde hemen sonra faturalandırma başlatır. Daha fazla bilgi için [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_fmle_rtmp></a>FMLE Kodlayıcı yapılandırın
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
1. Gezinmek için kullanılan makine üzerinde Flash Media Live Encoder's (FMLE) arabirim.

    Bir ana sayfa ayarlarını arabirimidir. FMLE kullanarak akışa başlamak için önerilen ayarları aşağıdakileri not alın.

   * Biçim: H.264 kare oranı: 30.00
   * Giriş boyutu: 1280 x 720
   * Bit hızı: 5000 (ağ sınırlamalar göre ayarlanabilir) KB/sn  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     Onay işareti "Taramasız Hale Getir'i" seçeneğini kullanarak kaynakları aralıklı, lütfen
2. Biçim yanındaki İngiliz anahtarı simgesini seçin, bu ek ayarlar şöyle olmalıdır:

   * Profil: Ana
   * Düzey: 4.0
   * Ana kare sıklığı: 2 saniye

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. Aşağıdaki önemli ses ayarını ayarlayın:

   * Biçim: AAC
   * Örnek Hızı: 44100 Hz
   * Bit hızı: 192 Kbps

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. Kanal alma giriş URL FMLE için 's atamak için **RTMP uç nokta**.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durum değiştiğinde **başlangıç** için **çalıştıran**, giriş URL'sini alabilirsiniz.

    Kanal çalıştırırken, kanal adına sağ tıklayın, üzerine gelindiğinde aşağı gidin **Panoya kopyalama giriş URL** seçip **birincil giriş URL**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. Bu bilgileri yapıştırın **FMS URL** çıkış bölümüne ve ata Akış adı alanı.

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Ek artıklık için ikincil giriş URL'si ile bu adımları yineleyin.
6. **Bağlan**’ı seçin.

> [!IMPORTANT]
> Tıklamadan önce **Connect**, size **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakmamaya emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE aracına gidin ve test edilecek kanal sağ tıklayın. Menüden üzerine **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Ardından akış Player'da görünüyorsa, kodlayıcı düzgün AMS'ye bağlanmak için yapılandırıldı.

Bir hata aldıysanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.  

## <a name="create-a-program"></a>Bir program oluşturma
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracı içinde program alanı sağ tıklatın ve seçin **yeni bir Program oluşturma**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. Program adı ve gerekirse ayarlamak **arşiv penceresi uzunluğu** (bunun varsayılan 4 saat). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **programı'nı şimdi başlatmak** kutusu.
4. Tıklayın **Program oluşturma**.  

    >[!NOTE]
    >Program oluşturma kanal oluşturmayı daha az zaman alır.
        
5. Program çalışmaya başladığında, kayıttan yürütme programa sağ tıklayıp giderek onaylayın **kayıttan yürütme programlarının** seçip **Azure Media Player ile**.  
6. Onaylandıktan sonra programı tekrar sağ tıklayıp **çıkış URL'sini Panoya Kopyala** (veya bu bilginin alındığı **Program bilgileri ve ayarları** menü seçeneğinden).

Akış bir oynatıcı içinde gömülü veya canlı görüntülemek için bir hedef kitle için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Bkz: [sorun giderme](media-services-troubleshooting-live-streaming.md) makale Kılavuzu.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
