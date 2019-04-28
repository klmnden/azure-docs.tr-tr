---
title: Media Services platformunda medya analizi | Microsoft Docs
description: Medya analizi Kurumsal ölçekte, uyumluluk, güvenlik ve küresel erişim konuşma ve bilgisayar işleme Hizmetleri koleksiyonunu genel Önizlemesi'ne genel bakış
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: aac9719f8d74c4b7bc283745ee2b8e01365a81f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61245269"
---
# <a name="media-analytics-on-the-media-services-platform"></a>Media Services platformunda medya analizi 

## <a name="overview"></a>Genel Bakış
Kuruluşların çalışanlarına eğitmek, kendi müşterilerle etkileşim kurun ve iş işlevleri belge için tercih edilen ortam video kullanmaktadır. Bulut bilgi işlem depolamak, akış ve bu büyük medya dosyalarını erişmek için bir yol sağlar. Ancak, video içeriğinin bir şirketin kitaplığı büyüdükçe ihtiyaç duyduğu ınsights içeriğini ayıklanması, eşit oranda etkili bir yöntem. 

Bu artan ihtiyacı karşılamak için Azure Media Services, Azure medya analizi sunar. Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Medya analizi, temel Media Services platform bileşenleri kullanılarak, medya işleme günde bir ölçekte işleyebilir.

Medya Analizi ile geliştiriciler uygulamalarına Gelişmiş video işlevleri hızlı bir şekilde sunabiliyoruz. Kurumsal ortamlar tam ölçekli, uyumluluk, güvenlik ve küresel erişim büyük kuruluşlar tarafından gerekli olan sağlar.

Aşağıdaki diyagramda, medya analizi ve Media Services platformunda diğer başlıca parçaları gösterilmektedir. 

![VoD iş akışı](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Medya Analizi medya işlemcileri MP4 veya JSON dosyaları üretir. Medya işlemcisi bir MP4 dosyası oluşturur, dosyayı aşamalı olarak indirebilirsiniz. Medya işlemcisi bir JSON dosyası oluşturur, dosyayı Azure Blob depolamadan indirebilirsiniz. 

## <a name="media-analytics-services"></a>Medya analizi Hizmetleri

### <a name="indexer"></a>Dizinleyici
Azure Media Indexer ile aranabilir içerik ve Kapalı Açıklamalı Altyazı parçaları oluşturma yapabilirsiniz. Azure Media Indexer 2 Önizleme önceki sürüme kıyasla daha hızlı dizinleme ve daha geniş dil desteği vardır. İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince, Portekizce ve Arapça desteklenen diller şunlardır. Ayrıntılı bilgi ve örnekler için bkz. [Azure Media Indexer 2 ile video işleme](media-services-process-content-with-indexer2.md).
### <a name="motion-detector"></a>Hareket Algılayıcısı
Hareket algılayıcısı, sabit bir arka plan ile bir video hareket algılama için kullanabilirsiniz. Bu hatalı pozitif sonuç ekleme işlemlerini kaydedecek tarafından algılanan hareket olayları denetlemek mümkün kılar. Ayrıntılı bilgi ve örnekler için bkz. [hareket algılama için Azure medya analizi](media-services-motion-detection.md).
### <a name="face-detector"></a>Yüz Algılayıcısı
Yüz algılayıcısı kullanarak, kişilerin yüzlerini ve duygularına mutluluk, üzüntü ve şaşkınlık gibi algılayabilir. Bu, toplamak ve bir olay katılan kişilerin tepkiler çözümleme dahil olmak üzere daha sonra açıklanan birkaç faydalı sektör uygulamaları içerir. Ayrıntılı bilgi ve örnekler için bkz. [Azure medya analizi için yüz ve duygu algılama](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Video özetleme
Video özetleme otomatik olarak kaynak videodaki ilginç parçacıkları'i seçerek uzun videoların özetlerini oluşturmanıza yardımcı olabilir. Bu özellik, uzun bir videoda beklenmesi gerekenler hızlı bir bakış sağlamak istediğinizde yararlıdır. Ayrıntılı bilgi ve örnekler için bkz. [kullanımı Azure medya Video video özeti oluşturma küçük resimleri](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optik karakter tanıma
Azure medya OCR (optik karakter tanıma) ile metin içerikli videoları düzenlenebilir, aranabilir dijital metne dönüştürebilirsiniz. Ardından, anlamlı meta verileri ayıklama medyanızın video sinyalinden otomatik hale getirebilirsiniz.
### <a name="scalable-face-redaction"></a>Ölçeklenebilir yüz flulaştırma
Azure Media Redactor, bulutta ölçeklenebilir yüz flulaştırma sunan medya analizi Medya işleyicisi ' dir. Yüz flulaştırma kullanarak seçilen kişilerin yüzlerini bulanıklaştıran için videonuzu değiştirebilirsiniz. Yüz flulaştırma hizmet haber medya veya kamu güvenliği söz konusu olduğunda kullanmak isteyebilirsiniz. El ile özgürlüğü saat birden fazla yüzeye içeren görüntülerini, birkaç dakika sürebilir, ancak bu hizmet ile yalnızca birkaç basit adımda yüz flulaştırma alır. Daha fazla bilgi için [özgürlüğü Azure medya Analizi ile yüzleri](media-services-face-redaction.md) makalesi.
### <a name="content-moderation"></a>İçerik Denetleme
Azure Content Moderator, videolarınız için makine Yardımlı resim denetimi kullanmanıza olanak sağlar. Örneğin videolardaki yetişkinlere yönelik veya müstehcen içerikleri tespit edip belirlenen içeriklerin moderasyon ekibiniz tarafından gözden geçirilmesini isteyebilirsiniz. El ile videolar için istenmeyen içeriği yönetme bir zaman alabilir ve pahalı bir görevdir. Bu hizmet ve ilişkili İnceleme araçları ile verimli ve hesaplı bir şekilde en iyi sonuçlar için İnsan içinde--döngüsü özelliği olan makine Yardımlı resim denetimi birleştirin. Daha fazla bilgi için bkz. [videolarınızı Azure Content Moderator ile işleme](media-services-content-moderation.md) makalesi.

## <a name="common-scenarios"></a>Genel senaryolar
Medya analizi, kuruluşların yardımcı olur ve işletmelerin video ait yeni öngörüleri elde ve daha etkili bir şekilde video içeriğinin büyük birimleri yönetin. Çeşitli senaryolar şunlardır:

* **Çağrı merkezleri**. Gelişinden bile sosyal medya ile müşteri çağrı merkezi müşteri hizmetlerine işlemleri büyük bir yüzdesini yine de kolaylaştırır. Bu ses verilerinde daha yüksek müşteri memnuniyeti elde etmek için analiz müşteri bilgileri, büyük bir miktarını kodlanır. Media Indexer'ı kullanarak, kuruluşlar, metni ayıklayın ve arama dizinleri ve panolar oluşturun. Ardından bunlar yaygın şikayetlerinden, şikayetlerinin kaynakları ve diğer ilgili verileri zeka ayıklayabilirsiniz.
* **Kullanıcı tarafından oluşturulan içerik denetleme**. Polis bölümlerine haber medya kuruluşlarının birçok kuruluşun kullanıcı tarafından oluşturulan medya video ve görüntü gibi kabul genel kullanıma yönelik portallar vardır. İçerik hacmi nedeniyle beklenmedik olaylar artırılabilir. Bu senaryolarda, site için içerik etkili el ile incelemeleri yürütmek zordur. Müşteriler, uygun olan içerik odaklanmak için içerik denetimi hizmette güvenebilirsiniz.
* **Gözetim**. IP kamera kullanımda büyümesi ile gözetim büyüyen bir envanterini video gelir. El ile gözetim video gözden geçirme, yoğun ve İnsan hatası potansiyeli zamanı geldi. Medya analizi için hareket algılama, yüz algılama ve gözden geçirme, yönetme ve türevleri daha kolay oluşturma işlemini kolaylaştırmak için Hyperlapse gibi hizmetleri sağlar.

## <a name="media-analytics-media-processors"></a>Medya analizi medya işlemcileri
Bu bölümde, medya analizi medya işlemcileri listeler ve .NET veya REST medya işlemci (MP) nesnesini almak için nasıl kullanılacağını gösterir.

### <a name="mp-names"></a>MP adı
* Azure Media Indexer 2-Önizleme
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR
* Azure medya Content Moderator

### <a name="net"></a>.NET
Aşağıdaki işlev belirtilen MP adlarını alır ve bir MP nesnesi döndürür.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


### <a name="rest"></a>REST
İstek:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Yanıt:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a>Demolar
Bkz: [Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
Bkz: [medya Hizmetleri analizi duyuru](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
