---
title: Standart Kodlayıcı Azure Media Services kullanarak bir otomatik olarak oluşturulan bit hızı Merdiveni video kodlamak için kullanın. | Microsoft Docs
description: Bu konuda, video giriş çözünürlük ve bit hızı dayalı bir otomatik olarak oluşturulan hızı Merdivenini otomatik ile standart Kodlayıcı giriş kodlama için Media Services kullanma gösterilmektedir. Giriş çözünürlük ve bit hızı hiçbir zaman aşacak. Örneğin, giriş ise 3 MB/sn, çıkış, 720p 720 p en iyi şekilde kalır ve 3 MB/sn düşük fiyatları başlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: ec1b4b88e5b9639c3ee9debbd8ac7d48544344dc
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378967"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Bir otomatik olarak oluşturulan bit hızı Merdiveni ile kodlayın

## <a name="overview"></a>Genel Bakış

Bu makalede standart Kodlayıcı Media Services giriş çözünürlük ve bit hızı dayalı bir otomatik olarak oluşturulan bit hızı Merdiveni (bit hızı çözümleme çiftleri) bir giriş video kodlayın için nasıl kullanılacağını açıklar. Bu ayarı, yerleşik Kodlayıcı veya hazır, hiçbir zaman giriş çözünürlük ve bit hızı aşacak. Örneğin, giriş 3 MB/sn hızında 720 p, çıkış 720 p en iyi şekilde kalır ve 3 MB/sn düşük fiyatları başlar.

### <a name="encoding-for-streaming"></a>Akış için kodlama

Kullanırken **AdaptiveStreaming** içinde hazır **dönüştürme**, HLS ve DASH gibi akış ile teslim için uygun olan bir çıkış alırsınız. Bu hazır kullanırken, hizmet akıllı bir şekilde oluşturmak için kaç video katmanları belirler ve hangi bit hızı ve çözümleme. Çıkış içeriği burada AAC kodlu ses ve video H.264 kodlamalı değil aralıklı MP4 dosyalarını içerir.

Bu hazır nasıl kullanıldığına dair bir örnek için bkz: [dosya Stream](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Çıktı

Bu bölümde kodlama ile sonucunda medya Hizmetleri Kodlayıcı tarafından üretilen çıkış video katmanları, üç örnek gösterir **AdaptiveStreaming** hazır. Her durumda, çıkış stereo sesli 128 Kb/sn ile kodlanmış bir yalnızca ses MP4 dosyası içerir.

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve "29.970" kare hızını kaynağıyla 6 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yükseklik "720" ve "23.970" kare hızını kaynağıyla 5 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve "29.970" kare hızını kaynağıyla 3 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
