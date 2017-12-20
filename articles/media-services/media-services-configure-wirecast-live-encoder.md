---
title: "Tek bit hızlı bir canlı akışı göndermek için Telestream Wirecast Kodlayıcı yapılandırma | Microsoft Docs"
description: "Bu konuda, tek bit hızlı akış gerçek zamanlı kodlama için etkinleştirilmiş AMS kanallar göndermek için Wirecast gerçek zamanlı Kodlayıcı yapılandırma gösterilmektedir. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: c4df14f24650ce431dfb31cc774cab6d3cf3aef0
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Tek bit hızlı bir canlı akışı göndermek için Wirecast Kodlayıcı kullanın
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Elemental dinamik](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Bu konuda nasıl yapılandırılacağını göstermektedir [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) AMS bir tek bit hızlı akışın kanallar göndermek için gerçek zamanlı Kodlayıcı, gerçek zamanlı kodlama için etkinleştirilir.  Daha fazla bilgi için bkz. [Azure Media Services ile Gerçek Zamanlı Kodlama Gerçekleştirmek İçin Etkinleştirilmiş Kanallar ile Çalışma](media-services-manage-live-encoder-enabled-channels.md).

Bu öğretici, Azure Media Services Gezgini (AMSE) aracı ile Azure Media Services (AMS) yönetmek gösterilmiştir. Bu araç, yalnızca bir Windows Bilgisayarına çalışır. Mac veya Linux varsa, oluşturmak için Azure portalını kullanın [kanalları](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ve [programlar](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Ön koşullar
* [Bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)
* Çalıştıran bir akış uç olduğundan emin olun. Daha fazla bilgi için bkz: [akış uç noktalarını yönetme Media Services hesabı](media-services-portal-manage-streaming-endpoints.md)
* En son sürümünü yüklemek [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) aracı.
* Aracı'nı başlatın ve AMS hesabınıza bağlanın.

## <a name="tips"></a>İpuçları
* Mümkün olduğunda, bir sabit Internet bağlantısı kullanır.
* Bir iyi bant genişliği gereksinimlerini belirlerken için udur akış bit çift. Bu zorunlu bir gereksinim olmamasına karşın, Ağ Tıkanıklığı etkisini azaltmaya yardımcı olur.
* Kodlayıcılar tabanlı yazılım kullanarak, gereksiz tüm programları kapatın.

## <a name="create-a-channel"></a>Kanal oluşturma
1. AMSE aracını gidin **canlı** sekmesinde ve içinde kanal alanı sağ tıklatın. Seçin **kanal oluştur...** menüden.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Bir kanal adı belirtin açıklama alanı isteğe bağlıdır. Kanal ayarları altında seçin **standart** gerçek zamanlı kodlama seçeneğini ayarlamak giriş protokolü için **RTMP**. Olduğu gibi tüm diğer ayarlar bırakabilirsiniz.

    Emin olun **yeni kanal Şimdi Başlat** seçilir.

3. Tıklatın **kanal oluşturmak**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> Kanal başlatmak için 20 dakika sürebilir.
>
>

Kanal başlatılırken yapabilecekleriniz [Kodlayıcı yapılandırma](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Kanal hazır bir durumuna geçtiğinde hemen faturalama başladığını unutmayın. Daha fazla bilgi için bkz: [kanalın durumları](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>Telestream Wirecast Kodlayıcı yapılandırın
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

### <a name="configuration-steps"></a>Yapılandırma adımları
1. Kullanılan ve RTMP akış için ayarlanmış olan makine üzerinde Telestream Wirecast uygulamayı açın.
2. Çıktı giderek yapılandırmak **çıkış** sekmesi ve seçerek **çıktı ayarları...** .

    Emin olun **Çıkış hedefini** ayarlanır **RTMP sunucusu**.
3. **Tamam** düğmesine tıklayın.
4. Ayarlar sayfasında ayarlamak **hedef** olmasını alan **Azure Media Services**.

    Kodlama profili için önceden seçilmiş **Azure H.264 720 p 16:9 (1280 x 720)**. Bu ayarları özelleştirmek için aşağı açılan sağındaki dişli simgesini seçin ve ardından **yeni önceden**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Encoder hazır ayarlarını yapılandırın.

    Önceden ayarlanmış adlandırın ve için önerilen ayarları aşağıdakileri denetleyin:

    **Video**

   * Kodlayıcı: MainConcept H.264
   * Saniyedeki çerçeve sayısı: 30
   * Ortalama bit hızı: 5000 kbit/sn (ayarlanabilir ağ sınırlamalar tabanlı)
   * Profil: ana
   * Anahtar çerçevesi her: 60 çerçeveler

    **Ses**

   * Hedef bit hızı: 192 kbit/sn
   * Örnek Hızı: 44,100 kHz

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Tuşuna **kaydetmek**.

    Encoding alanı artık seçim için kullanılabilen yeni oluşturulan profil sahiptir.

    Yeni profili seçili olduğundan emin olun.
7. Get kanal girdisini URL için Wirecast atamak için **RTMP Endpoint**.

    AMSE aracına gidin ve kanal tamamlanma durumunu denetleyin. Durumu değiştiğinden sonra **başlangıç** için **çalıştıran**, giriş URL'yi elde edebilirsiniz.

    Kanal çalıştırırken, kanal adı sağ tıklayın ve ardından aşağıya doğru vurgulu üzerinden gidin **Panoya Kopyala giriş URL** ve ardından **birincil giriş URL**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. Wirecast içinde **çıkış ayarları** penceresinde, bu bilgileri **adresi** çıkış bölüm ve atama Akış adı alanı.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. **Tamam**’ı seçin.
2. Ana **Wirecast** hazır olduğunuzda, video ve ses giriş kaynağı onaylayın ve ardından isabet **akış** üst sol alt köşesindeki.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Tıklamadan önce **akış**, size **gerekir** kanal hazır olduğundan emin olun.
> Ayrıca, kanal hazır durumda > 15 dakikadan fazla akış bir giriş katkı olmadan bırakın değil emin olun.
>
>

## <a name="test-playback"></a>Testi kayıttan yürütme

AMSE Aracı'na gidin ve sınanacak kanal sağ tıklayın. Menüden, üzerine gelerek **kayıttan yürütme Önizleme** seçip **Azure Media Player ile**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Ardından akış player görünürse, kodlayıcı düzgün için AMS bağlanmak için yapılandırıldı.

Bir hata alırsanız, kanal sıfırlanması gerekir ve Kodlayıcı ayarları ayarlanır. Lütfen bakın [sorun giderme](media-services-troubleshooting-live-streaming.md) Kılavuzu konu.  

## <a name="create-a-program"></a>Bir program oluşturun
1. Kanal kayıttan yürütme onaylandıktan sonra bir program oluşturun. Altında **canlı** sekmesinde AMSE aracının içinde programı alanı sağ tıklatın ve seçin **Yeni Program Oluştur**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. Program adı ve gerekirse, ayarlamak **arşiv penceresi uzunluğu** (hangi varsayılan olarak 4 saat olarak). Ayrıca, bir depolama konumu belirtin veya varsayılan olarak bırakın.  
3. Denetleme **Program Şimdi Başlat** kutusu.
4. Tıklatın **Program oluşturma**.  

   >[!NOTE]
   >Program oluşturma kanal oluşturma daha az zaman alır.
       
5. Program çalışmaya başladıktan sonra program sağ tıklayarak ve giderek tarafından kayıttan yürütme onaylayın **kayıttan yürütme edinin** seçilerek **Azure Media Player ile**.  
6. Onaylandıktan sonra sağ program yeniden tıklatın ve seçin **çıkış URL'yi Panoya Kopyala** (veya bu bilgileri almak **Program bilgilerine ve ayarlarına** seçeneği menüsünde).

Akış bir oynatıcı katıştırılmış veya dinamik görüntülemek için bir izleyici için Dağıtılmış artık hazırdır.  

## <a name="troubleshooting"></a>Sorun giderme
Lütfen bakın [sorun giderme](media-services-troubleshooting-live-streaming.md) Kılavuzu konu.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
