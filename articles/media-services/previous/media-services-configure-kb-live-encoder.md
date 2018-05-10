---
title: Tek bit hızlı bir canlı akışı Azure'a göndermek için Haivision KB Kodlayıcı yapılandırma | Microsoft Docs
description: Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar göndermek için Haivision KB gerçek zamanlı Kodlayıcı yapılandırma gösterilmektedir.
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
ms.date: 02/02/2018
ms.author: juliako;dbgeorge
ms.openlocfilehash: 25077cd9338a2764c6dff9e755812033685f6641
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı bir canlı akışı göndermek için Haivision KB gerçek zamanlı Kodlayıcı kullanın
> [!div class="op_single_selector"]
> * [Elemental dinamik](media-services-configure-elemental-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [tricaster](media-services-configure-tricaster-live-encoder.md)
> * [wirecast](media-services-configure-wirecast-live-encoder.md)

Bu konuda nasıl yapılandırılacağını göstermektedir [Havision KB gerçek zamanlı Kodlayıcı](https://www.haivision.com/products/kb-series/) AMS bir tek bit hızlı akışın kanallar göndermek için Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir. Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetmek gösterilmiştir. Bu araç, yalnızca bir Windows Bilgisayarına çalışır. Mac veya Linux varsa, oluşturmak için Azure portalını kullanın [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Önkoşullar
*   SW v5.01 çalıştıran bir Haivision KB Kodlayıcısı erişimi veya daha büyük.
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
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Bir kanal adı belirtin açıklama alanı isteğe bağlıdır. Kanal ayarları altında seçin **standart** gerçek zamanlı kodlama seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz. Emin olun **yeni kanal Şimdi Başlat** seçilir.
3. Tıklatın **kanal oluşturmak**.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> Kanal başlatmak için 20 dakika sürebilir.

## <a name="configure-the-haivision-kb-encoder"></a>Haivision KB Kodlayıcı yapılandırın
Bu öğreticide, aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

Video:
-   Codec: H.264
-   Profil: Yüksek (düzeyi 4.0)
-   Bit hızı: 5000 KB/sn
-   Anahtar kare: 2 saniye (60 saniye)
-   Çerçeve oranı: 30

Ses:
-   Codec: AAC (LC)
-   Bit hızı: 192 Kb/sn
-   Örnek Hızı: 44,1 kHz

## <a name="configuration-steps"></a>Yapılandırma adımları
1.  Haivision KB kullanıcı arabirimine oturum açın.
2.  Tıklayın **menü düğmesi** kanal denetim merkezi seçip **Ekle kanal**  
    ![Ekran görüntüsü 2017-08-14 9.15.09 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Tür **kanal adı** alan adı ve sonraki tıklayın.  
    ![Ekran görüntüsü 2017-08-14 9.19.07 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Seçin **kanal giriş kaynağı** gelen **giriş kaynağı** açılır ve sonraki tıklayın.
    ![Ekran görüntüsü 2017-08-14 9.20.44 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Gelen **Kodlayıcı şablonu** açılan seçin **H264-720-AAC-192** tıkladıktan sonra.
    ![Ekran görüntüsü 2017-08-14 9.23.15 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Gelen **yeni çıkış seçin** açılan seçin **RTMP** ve sonraki tıklayın.  
    ![Ekran görüntüsü 2017-08-14 9.27.51 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Gelen **kanal çıktı** penceresinde, Azure akış bilgileri doldurun. Yapıştır **RTMP** ilk kanal kurulumunda bağlantısından **Server** alanı. İçinde **çıktı adı** kanal adını alanı yazın. Akış adı şablonu alanında şablon RTMPStreamName_ video_bitrate % akış adlandırmak için kullanın.
    ![Ekran görüntüsü 2017-08-14 9.33.17 adresindeki AM.png](./media/media-services-configure-kb-live-encoder/step7.png)
8.  İleri'yi ve sonra Bitti'ye tıklayın tıklatın.
9.  Tıklatın **Yürüt düğmesini** Kodlayıcı kanal başlatmak için.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Testi kayıttan yürütme
AMSE Aracı'na gidin ve sınanacak kanalı sağ tıklatın. Menüsünde Önizleme kayıttan yürütme getirin ve Azure Media Player ile seçin.

Ardından akış player görünürse, kodlayıcı düzgün için AMS bağlanmak için yapılandırıldı.

Bir hata alırsanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Yönergeler için sorun giderme makalesine bakın.

## <a name="create-a-program"></a>Bir program oluşturun
1.  Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. AMSE aracının Canlı sekmesindeki içinde programı alanı sağ tıklatın ve Yeni Program Oluştur seçin.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  Program adı ve gerekirse, arşiv penceresi uzunluğu (hangi varsayılan olarak dört saat) ayarlayın. Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.
2.  Başlangıç programı şimdi kutuyu işaretleyin.
3.  Program Oluştur'u tıklatın.
4.  Program çalışmaya başladıktan sonra kayıttan yürütme program sağ tıklayarak onaylayın ve giderek edinin kayıttan ve Azure Media Player ile seçme.
5.  Onaylandıktan sonra programı tekrar sağ tıklayın ve çıkış URL'yi Panoya Kopyala'yı seçin (veya Program bilgilerini ve ayarlar seçeneğini menüsünde bu bilgileri almak).

Akış bir oynatıcı katıştırılmış veya dinamik görüntülemek için bir izleyici için Dağıtılmış artık hazırdır.

> [!NOTE]
> Program oluşturma kanal oluşturma daha az zaman alır.