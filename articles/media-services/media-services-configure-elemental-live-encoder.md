---
title: "Tek bit hızlı bir canlı akışı göndermek için Elemental Canlı Kodlayıcı yapılandırma | Microsoft Docs"
description: "Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar göndermek için Elemental Canlı Kodlayıcı yapılandırma gösterilmektedir."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 668a3ab46a70c0ee25fa87031d27c0f4333ec89c
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı bir canlı akışı göndermek için Elemental Canlı Kodlayıcı kullanın
> [!div class="op_single_selector"]
> * [Elemental dinamik](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Bu konuda nasıl yapılandırılacağını göstermektedir [Elemental canlı](http://www.elementaltechnologies.com/products/elemental-live) AMS bir tek bit hızlı akışın kanallar göndermek için Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir.  Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetmek gösterilmiştir. Bu araç, yalnızca bir Windows Bilgisayarına çalışır. Mac veya Linux varsa, oluşturmak için Azure portalını kullanın [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Ön koşullar
* Bir çalışma Elemental canlı web arabirimi Canlı olayları oluşturmak üzere kullanma hakkında bilginiz olması gerekir.
* [Bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Çalıştıran bir akış uç olduğundan emin olun. Daha fazla bilgi için bkz: [akış uç noktalarını yönetme Media Services hesabı](media-services-portal-manage-streaming-endpoints.md).
* En son sürümünü yüklemek [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) aracı.
* Aracı'nı başlatın ve AMS hesabınıza bağlanın.

## <a name="tips"></a>İpuçları
* Mümkün olduğunda, bir sabit Internet bağlantısı kullanır.
* Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
* Kodlayıcılar tabanlı yazılım kullanarak, gereksiz tüm programları kapatın.

## <a name="elemental-live-with-rtp-ingest"></a>RTP ile elemental Canlı alma
Bu bölümde, tek bit hızlı bir canlı akışı RTP gönderir Elemental Canlı Kodlayıcı yapılandırma gösterilmektedir.  Daha fazla bilgi için bkz: [RTP üzerinden MPEG-TS akış](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Kanal oluşturma

1. AMSE aracını gidin **canlı** sekmesinde ve içinde kanal alanı sağ tıklatın. Seçin **kanal oluştur...** menüden.

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Bir kanal adı belirtin açıklama alanı isteğe bağlıdır. Kanal ayarları altında seçin **standart** gerçek zamanlı kodlama seçeneğini ayarlamak giriş protokolü için **RTP (MPEG-TS)**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin olun **yeni kanal Şimdi Başlat** seçilir.

3. Tıklatın **kanal oluşturmak**.

   ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> Kanal başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [Kodlayıcı yapılandırma](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

> [!IMPORTANT]
> Kanal hazır bir durumuna geçtiğinde hemen faturalama başladığını unutmayın. Daha fazla bilgi için bkz: [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

### <a id=configure_elemental_rtp></a>Elemental Canlı Kodlayıcı yapılandırın
Bu öğreticide aşağıdaki çıkış ayarları kullanılır. Bu bölümün geri kalanında daha ayrıntılı yapılandırma adımlarını açıklar.

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

#### <a name="configuration-steps"></a>Yapılandırma adımları
1. Gidin **Elemental canlı** web arabirimi ve kodlayıcıdan ayarlama **UDP/TS** akış.
2. Yeni bir olay oluşturulduğunda, çıktı grupları aşağıya doğru kaydırın ve ekleme **UDP/TS** çıktı grubu.
3. Seçerek yeni bir çıktı oluşturmak **yeni akış** ve ardından **Çıktı Ekle**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > Elemental olay akışı hatası olması durumunda yeniden Kodlayıcı yardımcı olmak için "Sistem saati için" ayarlayın zaman kodu sahip olması önerilir.
   >
   >
4. Çıktı oluşturuldu, tıklatın **akış eklemek**. Çıkış ayarları artık yapılandırılabilir.
5. "Yeni oluşturduğunuz akış aşağı kaydırın 1"'e gidin,'ı tıklatın **Video** sekmesinde solda ve genişletin **Gelişmiş** ayarları bölümü.

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Elemental Canlı kullanılabilir özelleştirme çeşitli sahipken, aşağıdaki ayarları AMS için akış ile çalışmaya başlama için önerilir.

   * Çözünürlük: 1280 x 720
   * Kare hızı: 30
   * GOP boyutu: 60 çerçeveler
   * Mod Taramasız hale getirme: aşamalı
   * Bit hızı: 5000000 bit/sn (Bu ağ sınırlamaları göre ayarlanabilir)

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. Kanalın giriş URL'sini alma.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durumu değiştiğinden sonra **başlangıç** için **çalıştıran**, giriş URL'yi elde edebilirsiniz.

    Kanal çalıştırırken, kanal adı sağ tıklayın ve ardından aşağıya doğru vurgulu üzerinden gidin **Panoya Kopyala giriş URL** ve ardından **birincil giriş URL**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. Bu bilgileri **birincil hedef** Elemental alanı. Diğer tüm ayarları varsayılan kalabilir.

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Ek artıklık için UDP/TS akış için ayrı bir "Çıktı" sekmesinde oluşturarak ikincil giriş URL ile bu adımları yineleyin.
3. Tıklatın **oluşturma** (yeni bir olay oluşturduysanız) veya **güncelleştirme** (önceden var olan bir olay düzenleme varsa) ve Kodlayıcı başlatmak için devam edin.

> [!IMPORTANT]
> Tıklamadan önce **Başlat** Elemental canlı web arabirimde, **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal > 15 dakikadan daha uzun bir olay olmadan hazır durumda bırakın değil emin olun.
>
>

Akış 30 saniye yürütüldükten sonra geri AMSE aracını ve test kayıttan yürütme için gidin.  

### <a name="test-playback"></a>Testi kayıttan yürütme

AMSE Aracı'na gidin ve sınanacak kanal sağ tıklayın. Menüden, üzerine gelerek **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Ardından akış player görünürse, kodlayıcı düzgün için AMS bağlanmak için yapılandırıldı.

Bir hata alırsanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Lütfen bakın [sorun giderme](media-services-troubleshooting-live-streaming.md) Kılavuzu konu.   

### <a name="create-a-program"></a>Bir program oluşturun
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracının içinde programı alanı sağ tıklatın ve seçin **Yeni Program Oluştur**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. Program adı ve gerekirse, ayarlamak **arşiv penceresi uzunluğu** (hangi varsayılan olarak 4 saat olarak). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **Program Şimdi Başlat** kutusu.
4. Tıklatın **Program oluşturma**.  

    >[!NOTE]
    > Program oluşturma kanal oluşturma daha az zaman alır.   
      
5. Program çalışmaya başladıktan sonra program sağ tıklayarak ve giderek tarafından kayıttan yürütme onaylayın **kayıttan yürütme edinin** seçilerek **Azure Media Player ile**.  
6. Onaylandıktan sonra sağ program yeniden tıklatın ve seçin **çıkış URL'yi Panoya Kopyala** (veya bu bilgileri almak **Program bilgilerine ve ayarlarına** seçeneği menüsünde).

Akış bir oynatıcı katıştırılmış veya dinamik görüntülemek için bir izleyici için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Lütfen bakın [sorun giderme](media-services-troubleshooting-live-streaming.md) Kılavuzu konu.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
