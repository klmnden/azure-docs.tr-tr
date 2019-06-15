---
title: Haivision KB Kodlayıcı, tek bit hızlı canlı akış'ı Azure'a göndermek için yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş kanallar AMS göndermek için Canlı Haivision KB Kodlayıcı yapılandırma gösterilmektedir.
services: media-services
documentationcenter: ''
author: dbgeorge
manager: vsood
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;dbgeorge
ms.openlocfilehash: 058a1f964eb14d89628c92cbadd80511b7a27bae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61230580"
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Haivision KB gerçek zamanlı Kodlayıcı, tek bit hızlı canlı akış göndermek için kullanın  
> [!div class="op_single_selector"]
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)

Bu konu nasıl yapılandırılacağı gösterilmektedir [Havision KB gerçek zamanlı Kodlayıcı](https://www.haivision.com/products/kb-series/) AMS tekli bit hızı akışına kanallar göndermek için Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğreticide, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetilecek gösterilmektedir. Bu araç yalnızca Windows bilgisayarda çalışır. Mac veya Linux bilgisayarda ise oluşturmak için Azure portalını kullanma [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Önkoşullar
*   Çalışıyor SW v5.01 bir Haivision KB Kodlayıcı erişimi veya daha büyük.
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
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Bir kanal adı belirtirseniz açıklama alanı isteğe bağlıdır. Kanal ayarları altında **standart** Live Encoding seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz. Emin **yeni kanal Şimdi Başlat** seçilir.
3. Tıklayın **kanal oluşturma**.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> Kanalı başlatmak için 20 dakika sürebilir.

## <a name="configure-the-haivision-kb-encoder"></a>Haivision KB Kodlayıcı yapılandırın
Bu öğreticide, aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

Video:
-   Codec: H.264
-   Profil: Yüksek (düzeyi 4.0)
-   Bit hızı: 5000 kbps
-   Ana kare: 2 saniye (60 çerçeve)
-   Kare hızı: 30

Ses:
-   Codec: AAC (LC)
-   Bit hızı: 192 kbps
-   Örnek Hızı: 44,1 kHz

## <a name="configuration-steps"></a>Yapılandırma adımları
1.  Haivision KB kullanıcı arabirimine oturum açın.
2.  Tıklayarak **menü düğmesine** kanal denetim merkezi seçip **Kanal Ekle**  
    ![Ekran 2017-08-14 9.15.09 AM](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Tür **kanal adı** ad alanı ve sonraki tıklayın.  
    ![Ekran 2017-08-14 9.19.07 AM](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Seçin **kanal giriş kaynağı** gelen **giriş kaynağı** açılır ve sonraki tıklayın.
    ![Ekran 2017-08-14 9.20.44 AM](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Gelen **Encoder şablonunu** açılan seçin **720 AAC 192 H264** ve sonraki tıklayın.
    ![Ekran 2017-08-14 9.23.15 AM](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Gelen **seçin yeni çıkış** açılan seçin **RTMP** ve sonraki tıklayın.  
    ![Ekran 2017-08-14 9.27.51 AM](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Gelen **kanal çıkış** penceresinde Azure akış bilgileri doldurun. Yapıştırma **RTMP** ilk kanal kurulumunda bağlantıdan **sunucu** alan. İçinde **çıkış adı** kanal adını alan yazın. Stream adı şablon bölümünde, şablonu RTMPStreamName_ video_bitrate % Akış adı için kullanın.
    ![Ekran 2017-08-14 9.33.17 AM](./media/media-services-configure-kb-live-encoder/step7.png)
8.  İleri'yi ve sonra Bitti'ye tıklayın tıklatın.
9.  Tıklayın **YÜRÜT düğmesine** Kodlayıcı kanalı başlatmak.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Testi kayıttan yürütme
AMSE aracına gidin ve test edilecek kanal sağ tıklayın. Menüsünden Önizleme kayıttan yürütme gelin ve Azure Media Player'ı seçin.

Ardından akış Player'da görünüyorsa, kodlayıcı düzgün AMS'ye bağlanmak için yapılandırıldı.

Bir hata aldıysanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Yönergeler için sorun giderme makalesine bakın.

## <a name="create-a-program"></a>Bir program oluşturma
1.  Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. AMSE Aracı'nda Canlı sekmesindeki içinde program alana sağ tıklayın ve yeni bir Program oluşturma seçin.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Program adı ve gerekirse, arşiv penceresi uzunluğu (Bu varsayılan olarak dört saat) ayarlayın. Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.
2.  Başlangıç programı şimdi kutuyu işaretleyin.
3.  Program Oluştur'a tıklayın.
4.  Program çalışmaya başladığında, kayıttan yürütme programa sağ tıklayarak onaylayın ve giderek programların kayıttan yürütme ve Azure Media Player'ı seçerek.
5.  Onaylandıktan sonra programı tekrar sağ tıklayın ve çıkış URL'sini Panoya Kopyala'yı seçin (veya menü seçeneğinden ayarları ve Program bilgilerini bu bilgileri almak).

Akış bir oynatıcı içinde gömülü veya canlı görüntülemek için bir hedef kitle için Dağıtılmış artık hazırdır.

> [!NOTE]
> Program oluşturma kanal oluşturmayı daha az zaman alır.
