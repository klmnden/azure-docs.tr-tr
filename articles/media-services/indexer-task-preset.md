---
title: "Görev için Azure Media Indexer hazır"
description: "Bu konu Azure Media Indexer için önceden görev genel bir bakış sağlar."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: ae6c4da189cd6637b4e1fa9274473b62f6664e51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="task-preset-for-azure-media-indexer"></a>Görev için Azure Media Indexer hazır

Azure Media Indexer olan aşağıdaki görevleri gerçekleştirmek için kullandığınız medya işlemcisi: ortam dosyaları ve içerik aranabilir yapmanıza, kapalı açıklamalı alt yazı izler ve anahtar sözcükler oluşturmak, Varlığınızı parçası olan varlık dosyaları dizini.

Bu konu görevi açıklayan dizin işinizi geçmesi gerektiğini hazır. Tam örnek için bkz: [Azure Media Indexer ortam dosyalarıyla dizin](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer yapılandırma XML

Aşağıdaki tabloda, öğeleri ve yapılandırma XML öznitelikleri açıklanmaktadır.

|Ad|Gerektirme|Açıklama|
|---|---|---|
|Girdi|TRUE|Dizin istediğiniz varlık dosyaları.<br/>Azure Media Indexer aşağıdaki ortam dosya biçimleri destekler: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Dosya adı (s) belirleyebilirsiniz **adı** veya **listesi** özniteliği **giriş** (aşağıda gösterildiği gibi) öğesi. Hangi dizin varlık dosyasına belirtmezseniz, birincil dosya çekilir. Herhangi bir birincil varlık dosyası ayarlarsanız, ilk giriş varlık dosyasında dizine alınır.<br/><br/>Varlık dosya adı açıkça belirtmek için aşağıdakileri yapın:<br/>```<input name="TestFile.wmv" />```<br/><br/>Ayrıca, birden çok varlık aynı anda (dosyaları en fazla 10) dizin oluşturabilirsiniz. Bunu yapmak için:<br/>-Bir metin dosyası (bildirim dosyası) oluşturun ve bir .lst uzantısı verin.<br/>-Tüm varlık dosya adlarının bir listesini giriş Varlığınızı bildirim bu dosyaya ekleyin.<br/>-(Karşıya yükleme) thanifest dosyasını varlık için ekleyin.<br/>-Girdinin liste özniteliğinde bildirim dosyasının adını belirtin.<br/>```<input list="input.lst">```<br/><br/>**Not:** bildirim dosyası 10'dan fazla dosyaları eklerseniz, dizin oluşturma işi 2006 hata koduyla başarısız olur.|
|Meta veriler|False|Belirtilen varlık dosyaları için meta veriler.<br/>```<metadata key="..." value="..." />```<br/><br/>Önceden tanımlanmış anahtarlar için değer sağlayabilir. <br/><br/>Şu anda aşağıdaki anahtarları desteklenir:<br/><br/>**Başlık** ve **açıklama** - konuşma tanıma doğruluğunu artırmak için dil modeli güncelleştirmek için kullanılan.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**Kullanıcı adı** ve **parola** - http veya https üzerinden internet dosyaları yüklerken kimlik doğrulaması için kullanılır.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Kullanıcı adı ve parola değerleri giriş bildiriminde tüm medya URL'leri uygulayın.|
|SaaS Uygulamaları Geliştirme<br/><br/>Sürüm 1.2 eklendi. Şu anda, yalnızca desteklenen konuşma tanıma ("ASR") özelliğidir.|False|Konuşma tanıma özelliği şu ayarları anahtarlarını sahiptir:<br/><br/>Dil:<br/>-Multimedya dosyasında kabul edilecek doğal dili.<br/>-İngilizce, İspanyolca<br/><br/>CaptionFormats:<br/>-noktalı virgülle ayrılmış listesini istenen çıkış resim yazısı biçimleri (varsa)<br/>-ttml; sami; webvtt<br/><br/><br/>GenerateAIB:<br/>-Bir AIB dosyası (kullanmak için SQL Server ve müşteri dizin oluşturucu IFilter ile) gerekli olup olmadığını belirten boolean bir bayrak. Daha fazla bilgi için Azure Media Indexer ve SQL Server ile AIB dosyaları kullanma konusuna bakın.<br/>-True; False<br/><br/>GenerateKeywords:<br/>-Bir anahtar sözcük XML dosyasını gerekli olup olmadığını belirten boolean bir bayrak.<br/>-True; FALSE.|

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

Bkz: [Azure Media Indexer ortam dosyalarıyla dizin](media-services-index-content.md).

