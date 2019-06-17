---
title: Azure Media Indexer'ın hazır görev
description: Bu konu Azure Media Indexer'ın hazır görev genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Asolanki
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: adsolank;juliako;
ms.openlocfilehash: 8ce3ea3847e4c8c022f17375676d8415372dec85
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61466315"
---
# <a name="task-preset-for-azure-media-indexer"></a>Azure Media Indexer'ın hazır görev 

Azure Media Indexer, aşağıdaki görevleri gerçekleştirmek için kullandığınız Medya işleyicisi olduğu: ortam dosyaları ve içerik aranabilir hale getirin, kapalı açıklamalı alt yazı izleyen ve anahtar sözcükler üretmek, varlığınız bir parçası olan varlık dosyaları dizini.

Bu konuda görevi açıklayan önceden dizin oluşturma işinizle geçmeniz gerekir. Tam bir örnek için bkz. [Azure Media Indexer ile medya dosyalarının dizinini oluşturarak](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer yapılandırma XML'i

Aşağıdaki tablo, öğeleri ve yapılandırma XML özniteliklerini açıklar.

|Ad|Gerektirme|Açıklama|
|---|---|---|
|Girdi|true|Dizine eklemek istediğiniz varlık dosyaları.<br/>Azure Media Indexer, aşağıdaki ortam dosya biçimlerini destekler: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Dosya adı (s) belirleyebilirsiniz **adı** veya **listesi** özniteliği **giriş** (aşağıda gösterildiği gibi) öğesi. Birincil dosya dizini için hangi varlık dosyası belirtmezseniz çekilir. Hiçbir birincil varlık dosyası olarak ayarlanırsa ilk giriş varlığı dosyasında dizine alınır.<br/><br/>Varlık dosyası adı açıkça belirtmek için aşağıdakileri yapın:<br/>```<input name="TestFile.wmv" />```<br/><br/>Ayrıca, birden çok varlık dosyaları aynı anda (en fazla 10) de dizine ekleyebilir. Bunu yapmak için:<br/>-Bir metin dosyası (bildirim dosyasını) oluşturun ve .lst uzantısı verin.<br/>-Tüm varlık dosya adlarının bir listesini giriş varlığınız bu bildirim dosyasına ekleyin.<br/>-Varlık için (yükleme) bildirim dosyası ekleyin.<br/>-Girdinin listesi özniteliğini bildirim dosyasının adını belirtin.<br/>```<input list="input.lst">```<br/><br/>**Not:** Bildirim dosyası için 10'dan fazla dosyaları eklerseniz, dizin oluşturma işi 2006 hata kodu ile başarısız olur.|
|meta veriler|false|Belirtilen varlık dosyaları için meta veriler.<br/>```<metadata key="..." value="..." />```<br/><br/>Önceden tanımlı anahtarlar için değerleri sağlayabilirsiniz. <br/><br/>Şu anda aşağıdaki anahtarları desteklenmektedir:<br/><br/>**Başlık** ve **açıklama** - konuşma tanıma doğruluğunu artırmak için dil modeli güncelleştirmek için kullanılır.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**Kullanıcı adı** ve **parola** - http veya https aracılığıyla internet dosyaları indirirken kimlik doğrulaması için kullanılır.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Kullanıcı adı ve parola değerleri tüm medya URL'leri giriş bildiriminde uygulayın.|
|SaaS Uygulamaları Geliştirme<br/><br/>Sürüm 1.2 eklendi. Şu anda, yalnızca desteklenen konuşma tanıma ("ASR") özelliğidir.|false|Konuşma tanıma özelliği, aşağıdaki ayarları anahtarlarını sahiptir:<br/><br/>Dil:<br/>-Multimedya dosyasında tanınacak doğal dili.<br/>-İngilizce, İspanyolca<br/><br/>CaptionFormats:<br/>-noktalı virgülle ayrılmış listesini istenen çıkış başlığı biçimlendirir (varsa)<br/>- ttml;sami;webvtt<br/><br/><br/>GenerateAIB:<br/>-AIB dosyası (SQL Server ve müşteri dizin oluşturucu IFilter ile kullanın için) gerekli olup olmadığını belirleme bir Boole bayrağı. Daha fazla bilgi için Azure Media Indexer'ı ve SQL Server kullanarak AIB dosyaları bakın.<br/>-True; False<br/><br/>GenerateKeywords:<br/>-Bir anahtar sözcük XML dosyası gerekli olup olmadığını belirleme bir Boole bayrağı.<br/>-True; FALSE.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Azure Media Indexer yapılandırma XML örneği

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of the media file]" />  
    <metadata key="description" value="[Description of the media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="CaptionFormats" value="ttml;sami;webvtt"/>  
        <add key="GenerateAIB" value ="true" />  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure Media Indexer ile medya dosyalarının dizinini oluşturarak](media-services-index-content.md).

