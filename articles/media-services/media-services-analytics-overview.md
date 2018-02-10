---
title: Medya analizi medya Hizmetleri platformunda | Microsoft Docs
description: "Medya analizi, Kurumsal ölçek, uyumluluk, güvenlik ve genel ulaşma konuşma ve bilgisayar görme hizmetler koleksiyonu genel önizlemesi genel bakış"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 88c854a6a2bc98a6851246c0ac3481869bbd9c34
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="media-analytics-on-the-media-services-platform"></a>Media Services platformunda medya analizi
## <a name="overview"></a>Genel Bakış
Daha fazla kuruluş video çalışanlarını eğitmek, müşterilerinin gerçekleştirmesine ve işletme işlevlerinin belge için tercih edilen ortamı olarak kullanılmıştır. Bulut bilgi işlem depolamak, akış ve bu büyük medya dosyalarını erişmek için bir yol sağlar. Ancak bir şirketin kitaplığı video içeriğinin büyüdükçe Öngörüler içerikten ayıklanması, eşit etkili bir yol gerekiyor. 

Bu büyüyen gereksiniminin karşılanması amacıyla Azure medya analizi Azure Media Services sunar. Medya Analizi, kuruluş ve işletmelerin video dosyalarından eyleme dönüştürülebilir öngörüler türetmesini kolaylaştıran bir grup konuşma ve görme bileşenidir. Çekirdek Media Services platform bileşenleri kullanılarak oluşturulmuş, medya analizi medya gün bir ölçekte işleme işleyebilir.

Medya Analizi ile geliştiricilerin hızla Gelişmiş video işlevselliği uygulamalara kullanıma sunabilirsiniz. Kurumsal ortamlar tam ölçekli, uyumluluk, güvenlik ve büyük kuruluşlar tarafından gerekli genel erişim sağlar.

Aşağıdaki diyagramda, medya analizi ve Media Services platformunun diğer başlıca parçaları gösterilmektedir. 

![VoD iş akışı](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

Medya Analizi medya işlemcileri MP4 veya JSON dosyaları üretir. Medya işlemcisi bir MP4 dosyası oluşturursa, dosyayı aşamalı olarak indirebilirsiniz. Medya işlemcisi bir JSON dosyası oluşturursa, Azure Blob depolama biriminden dosyayı indirebilirsiniz. 

## <a name="media-analytics-services"></a>Medya analizi hizmetlerinden

### <a name="indexer"></a>Dizinleyici
Azure Media Indexer ile aranabilir içerik ve kapalı açıklamalı parçaları oluşturmak yapabilirsiniz. Önceki sürüme geri ile karşılaştırıldığında, Azure Media Indexer 2 Önizleme hızlı dizin oluşturma ve daha geniş dil desteği vardır. Desteklenen diller, İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince, Portekizce ve Arapça içerir. Ayrıntılı bilgi ve örnekler için bkz: [Azure Media Indexer 2 ile video işleme](media-services-process-content-with-indexer2.md).
### <a name="hyperlapse"></a>Hyperlapse
Microsoft Hyperlapse video sabitlemeyi ve hızlı, uzun formlu içeriğinizi tüketilebilir videoların oluşturma yeteneği zaman atlama birleştirir. Zaman atlama video oluşturmanın yanı sıra Hyperlapse kararlı videolar cep telefonları ve kameralar aracılığıyla yakalanan shaky videolar oluşturmak için kullanabilirsiniz. Ayrıntılı bilgi ve örnekler için bkz: [Hyperlapse Azure medya Hyperlapse ile medya dosyalarını](media-services-hyperlapse-content.md).
### <a name="motion-detector"></a>Hareket Algılayıcısı
Hareket algılayıcısı sabit arka plan ile video hareket algılamak için kullanabilirsiniz. İzleme kameralarını tarafından algılanan hareket olaylarına hatalı pozitif sonuç denetlemek mümkün kılar. Ayrıntılı bilgi ve örnekler için bkz: [hareket algılama Azure medya analizi için](media-services-motion-detection.md).
### <a name="face-detector"></a>Yüz Algılayıcısı
Yüz algılayıcısı kullanarak, kişilerin yüzeyleri ve mutluluk, sadness ve beklenmedik biçimde dahil olmak üzere kendi duygular algılayabilir. Bu, toplama ve analiz etme tepki olaya katılan kişilerin de dahil olmak üzere daha sonra açıklanan çeşitli yararlı endüstri uygulamalar, sahiptir. Ayrıntılı bilgi ve örnekler için bkz: [Azure medya analizi için yüz ve duygu algılama](media-services-face-and-emotion-detection.md).
### <a name="video-summarization"></a>Video özetleme
Video özetleme otomatik olarak kaynak video ilginç parçacıkları'i seçerek uzun videoları özetlerini oluşturmanıza yardımcı olabilir. Bu yetenek, uzun bir video beklenmesi gerekenler hızlı bir bakış sağlamak istediğinizde kullanışlıdır. Ayrıntılı bilgi ve örnekler için bkz: [kullanım Azure medya Video video özeti oluşturma küçük resimleri](media-services-video-summarization.md).
### <a name="optical-character-recognition"></a>Optik karakter tanıma
Azure Media ORC (optik karakter tanıma) ile video dosyaları metin içeriği düzenlenebilir, aranabilir dijital metne dönüştürebilirsiniz. Ardından, medya video sinyali anlamlı meta veri ayıklama otomatik hale getirebilirsiniz.
### <a name="scalable-face-redaction"></a>Ölçeklenebilir yüz Redaksiyon
Azure Media Redactor ölçeklenebilir yüz Redaksiyon bulutta sunan medya analizi medya işlemcisi ' dir. Yüz Redaksiyon kullanarak videonuzu seçili kişiler yüzeyleri ölçeklendirilmelidir değiştirebilirsiniz. Haber ortamda veya ortak güvenlik söz konusu olduğunda yüz Redaksiyon hizmetini kullanmak isteyebilirsiniz. Birden çok yüzeyleri içeren çekimi birkaç dakika el ile Redaksiyon saat sürebilir, ancak bu hizmetle yüz Redaksiyon birkaç basit adımla alır. Daha fazla bilgi için bkz: [Redaksiyon yüzeyleri Azure medya Analizi ile](media-services-face-redaction.md) makalesi.
### <a name="content-moderation"></a>İçerik Denetleme
Azure içerik denetleyici videolarınızı için makine destekli yönetimini kullanmanıza olanak sağlar. Örneğin, videoları olası yetişkin ve saldırganlardan içerikleri algılama ve işaretli içerik İnsan yönetimini ekipleriniz tarafından gözden geçirmek isteyebilirsiniz. El ile yönetme videolar istenmeyen içerik için bir zaman alabilir ve pahalı bir görevdir. Bu hizmeti ve ilişkili gözden geçirme araçları ile verimli ve uygun maliyetli bir şekilde en iyi sonuçlar için İnsan-içinde--döngü özellikleriyle makine destekli yönetimini birleştirin. Daha fazla bilgi için bkz: [Azure içerik Denetleyici ile kullanarak videolarınızı işleyin](media-services-content-moderation.md) makalesi.

## <a name="common-scenarios"></a>Genel senaryolar
Medya analizi kuruluşlara yardımcı olur ve işletmelerin video ilişkin yeni bilgiler glean ve daha etkili bir şekilde video içeriği büyük miktarda yönetin. Burada, çeşitli senaryolar vardır:

* **Çağrı merkezleri**. Hatta geliştirilirken sosyal medya ile müşteri çağrı merkezleri hala müşteri hizmetleri işlemleri büyük bir yüzdesini kolaylaştırır. Bu ses verilerde büyük miktarda yüksek müşteri memnuniyetini elde etmek için analiz müşteri bilgi kodlanır. Kuruluşlar, medya Dizin Oluşturucu kullanarak metin ayıklayın ve arama dizinlerini ve panolar oluşturmaya başlayın. Ardından bunlar yaygın şikayetlerinden, şikayetlerinden kaynaklarını ve diğer ilgili verileri çevresinde Intelligence ayıklayabilirsiniz.
* **Kullanıcı tarafından oluşturulmuş içerik yönetimini**. Polis departmanlara haber medya çıkışlar çoğu kuruluş, videolar ve resimler gibi kullanıcı tarafından oluşturulan medya kabul genel kullanıma yönelik portalları sahiptir. İçerik hacmi beklenmeyen olaylar nedeniyle ani değişiklik. Bu senaryolarda, site için içeriği etkili el ile incelemeler gerçekleştir zordur. Müşteriler, uygun olan içerik odaklanmak için içerik yönetimini hizmetine güvenir.
* **İzleme**. IP kamera kullanımda büyümesi ile izleme sistemine büyüyen bir sayım video gelir. El ile izleme video gözden geçirme yoğun ve İnsan hataya yatkın saattir. Medya analizi hareket algılama, yüz algılama ve gözden geçirme, yönetme ve türevleri daha kolay oluşturma işlemi yapmak için Hyperlapse gibi hizmetleri sağlar.

## <a name="media-analytics-media-processors"></a>Medya analizi medya işlemcileri
Bu bölümde, medya analizi medya işlemcileri listeler ve .NET veya REST medya işlemci (MP) nesnesini almak için nasıl kullanılacağını gösterir.

### <a name="mp-names"></a>MP adları
* Azure Media Indexer 2 Önizleme
* Azure Media Indexer
* Azure Media Hyperlapse
* Azure Media Face Detector
* Azure Media Motion Detector
* Azure Media Video Thumbnails
* Azure Media OCR
* Azure Media Content Moderator

### <a name="net"></a>.NET
Aşağıdaki işlevi, belirtilen MP adlarından birini alır ve bir MP nesnesi döndürür.

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
İsteği:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

Yanıtı:

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

## <a name="demos"></a>Gösterileri
Bkz: [Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
Bkz: [medya Hizmetleri analizi duyuru](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
