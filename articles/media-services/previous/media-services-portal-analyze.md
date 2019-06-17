---
title: Azure portalını kullanarak medyanızı analiz etme | Microsoft Docs
description: Bu konuda, Azure portalını kullanarak medya analizi medya işlemcileri (MP'ler) ile medyanızı işlemek nasıl ele alınmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: d3917f65d8be08d6355013393f6c6675ea6c7fc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61131832"
---
# <a name="analyze-your-media-using-the-azure-portal"></a>Azure portalını kullanarak medyanızı analiz etme 
> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

## <a name="overview"></a>Genel Bakış
Azure medya Hizmetleri analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir Öngörüler türetmesini kolaylaştıran konuşma ve görüntü tanıma bileşenlerinin (en, Kurumsal ölçekte, uyumluluk, güvenlik ve küresel erişim) koleksiyonudur. Daha ayrıntılı Azure Media Services Analytics'e genel bakış için bkz. [bu](media-services-analytics-overview.md) konu. 

Bu konuda, Azure portalını kullanarak medya analizi medya işlemcileri (MP'ler) ile medyanızı işlemek nasıl ele alınmaktadır. Media Analytics Mp'leri MP4 veya JSON dosyaları üretir. Medya işlemcisi bir MP4 dosyası oluşturduysa dosyayı aşamalı olarak yükleyin. Medya işlemcisi bir JSON dosyası oluşturduysa dosyayı Azure blob Depolama'yı yükleyin. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Çözümlemek istediğiniz bir varlık seçin
1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** penceresinde **Varlıklar**’ı seçin.  
   
    ![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze001.png)
3. Tuşuna basın ve analiz etmek istediğiniz varlığı seçin **Çözümle** düğmesi.
   
    ![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. İçinde **Media Analytics ile medya varlığını işle** penceresinde işleyicisini seçin. 
   
    Bu makalenin geri kalanında nedenini açıklar ve her işlemci kullanma. 
5. Tuşuna **Oluştur** iş başlatma.

## <a name="azure-media-indexer"></a>Azure Media Indexer
**Azure Media Indexer'ın** medya işlemci kapalı açıklamalı alt yazı parçaları oluşturmak yanı sıra medya dosyalarını ve içerik aranabilir yapmanıza olanak tanır. Bu bölümde, bu MP için belirttiğiniz seçenekleri hakkında bazı ayrıntılar sağlar.

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Dil
Multimedya dosyasında tanınacak doğal dili. Örneğin, İngilizce veya İspanyolca. 

### <a name="captions"></a>açıklamalı alt yazılar
İçeriğinizi oluşturulan açıklamalı alt yazı biçimi seçebilirsiniz. Bir dizin oluşturma işi kapalı açıklamalı alt yazı dosyaları aşağıdaki biçimlerde oluşturabilirsiniz:  

* **SAMI**
* **TTML**
* **WebVTT**

Bu biçimler dosyalarında ses ve video dosyaları işitme engelli kişiler için erişilebilir hale getirmek için kullanılan açıklamalı alt yazı (CC) kapatıldı.

### <a name="aib-file"></a>AIB dosyası
Özel SQL Server IFilter ile kullanılacak ses dizini Blob dosyasını oluşturmak istiyorsanız bu seçeneği belirleyin. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.

### <a name="keywords"></a>anahtar sözcükler
Bir anahtar sözcük XML dosyası oluşturmak istiyorsanız bu seçeneği belirleyin. Bu dosya, sıklık ve uzaklık bilgilerini konuşma içeriğinden ayıklanan anahtar sözcükler içerir.

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan bir kolay ad. [Bu](media-services-portal-check-job-progress.md) makalede nasıl bir işin ilerlemesini izleyebilirsiniz. 

### <a name="output-file"></a>Çıktı dosyası
Çıkış içeriği belirlemenize olanak sağlayan bir kolay ad. 

### <a name="speed"></a>Hız
Hızını giriş videosunu hızlandırma katsayısını belirtin. Titreşimsiz ve zaman aralıklı bir işleme giriş video çıktıdır.

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan bir kolay ad. [Bu](media-services-portal-check-job-progress.md) makalede nasıl bir işin ilerlemesini izleyebilirsiniz. 

### <a name="output-file"></a>Çıktı dosyası
Çıkış içeriği belirlemenize olanak sağlayan bir kolay ad. 

## <a name="azure-media-face-detector"></a>Azure Media Face Detector
**Azure medya yüz algılayıcısı** medya işlemci (MP) sayısı, hareketleri izlemek ve hatta İzleyici katılım ve yüz ifadelerini aracılığıyla tepki ölçer olanak sağlar. Bu hizmet, iki özellik içerir: 

* **Yüz algılama**
  
    Yüz algılama bulur ve video İnsan yüzlerini izler. Birden fazla yüzeye algılanabilir ve bunlar JSON dosyasında döndürülen zaman ve konum meta verileriyle hareket ettirmek sonradan izlenmesi. İzleme sırasında bu obstructed veya kısaca çerçeve bırakın bile kişi ekranında gezinmek sırada tutarlı kimliği için aynı yüz vermek dener.
  
  > [!NOTE]
  > Bu hizmetler yüz tanıma gerçekleştirmez. Çerçeve ayrıldığında ya da için obstructed olur bireysel bunlar döndüğünüzde yeni bir kimliği çok uzun sunulur.
  > 
  > 
* **Duygu algılama**
  
    Duygu algılama, birden çok duygusal özniteliklerinde mutluluk, üzüntü, Korku, kızgınlık ve daha fazlası dahil olmak üzere algılanan yüzleri analiz döndüren yüz algılama medya işlemcisi isteğe bağlı bir bileşendir. 

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Algılama modu
Şu modlardan biri işlemci tarafından kullanılabilir:

* Yüz algılama
* yüze göre duygu algılama
* Toplu duygu algılama

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan bir kolay ad. [Bu](media-services-portal-check-job-progress.md) makalede nasıl bir işin ilerlemesini izleyebilirsiniz. 

### <a name="output-file"></a>Çıktı dosyası
Çıkış içeriği belirlemenize olanak sağlayan bir kolay ad. 

## <a name="azure-media-motion-detector"></a>Azure Media Motion Detector
**Azure ortam hareket algılayıcısı** medya işlemci (MP) faiz yoksa uzun ve olaysız bir video içinde bölümlerini verimli bir şekilde belirlemenizi sağlar. Hareket algılama üzerinde statik kamerası kayıtları, video bölümlerini hareket nerede oluştuğunu belirlemek için kullanılabilir. Bu zaman damgaları ve olayın gerçekleştiği sınırlayıcı bölge ile bir meta veri içeren bir JSON dosyası oluşturur.

Doğru güvenlik video akışları hedeflenen, ilgili olaylar ve hatalı pozitif sonuçları shadows ve ışık değişiklikleri gibi hareket kategorilere ayırmak bu teknoloji kuramıyor. Bu güvenlik uyarıları kamera akışlarından sonsuz ilgisiz olayları ile son derece uzun gözetim videolardan ilgi dakika ayıklayamayabilir olmanın yanı sıra adresinize gerek kalmadan oluşturmanıza olanak sağlar.

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video Thumbnails
Bu işlemci otomatik olarak kaynak videodaki ilginç parçacıkları'i seçerek uzun videoların özetlerini oluşturmanıza yardımcı olabilir. Uzun bir videoda beklenmesi gerekenler hızlı bir bakış sağlamak istediğinizde bu kullanışlıdır. Ayrıntılı bilgi ve örnekler için bkz. [kullanımı Azure medya Video küçük'bir Video özeti oluşturma](media-services-video-summarization.md)

![Videoları analiz etme](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>İş adı
İşi tanımlamanızı sağlayan bir kolay ad. [Bu](media-services-portal-check-job-progress.md) makalede nasıl bir işin ilerlemesini izleyebilirsiniz. 

### <a name="output-file"></a>Çıktı dosyası
Çıkış içeriği belirlemenize olanak sağlayan bir kolay ad. 

## <a name="azure-media-content-moderator"></a>Azure medya Content Moderator
Bu işlemci videolarda yetişkinlere yönelik ve müstehcen içerik olası algılamanıza yardımcı olur. İşlemci, videoda görüntüleri ve ana kareleri otomatik olarak algılar. Olası yetişkinlere yönelik veya müstehcen içeriği için ana kareler puanlar ve varsayılan eşiklere dayanarak incelemeleri önerir. Ayrıntılı bilgi ve örnekler için bkz. [kullanımı Azure medya içerik videoları Orta geçirilmesini](media-services-content-moderation.md)

![Orta videoları](./media/media-services-portal-analyze/media-services-portal-analyze-content-moderator.PNG)

### <a name="version"></a>Version 
"2.0" kullanın.

### <a name="mode"></a>Mod
2\.0 sürümünü Yoksay `Mode` ayarı.

## <a name="next-steps"></a>Sonraki adımlar
Görünüm Media Services'i öğrenme yolları.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
