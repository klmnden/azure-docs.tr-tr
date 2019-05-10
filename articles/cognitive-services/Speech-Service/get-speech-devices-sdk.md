---
title: Konuşma Cihazları SDK’sını edinme
titleSuffix: Azure Cognitive Services
description: Konuşma Hizmetleri çok çeşitli cihazları ve ses kaynaklar ile çalışın. Şimdi, konuşma uygulamalarınızın eşleşen donanım ve yazılım ile bir sonraki düzeye alabilir. Bu makalede, konuşma cihaz SDK'sı erişin ve geliştirmeye başlayın öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b9a0890000cda0b3663ac29bee61fc1c702f6254
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410702"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Bilişsel hizmetler konuşma cihaz SDK'sı Al

Konuşma cihaz SDK'sı, amaca yönelik olarak tasarlanmış bir geliştirme setleri ve değişen mikrofon dizi yapılandırmaları ile çalışmak üzere tasarlanmış pretuned bir kitaplıktır.

## <a name="choose-a-development-kit"></a>Geliştirme Seti'ni seçin

|Cihazlar|Belirtimi|Açıklama|Senaryolar|
|--|--|--|--|
|[Roobo akıllı ses Dev Seti](https://ddk.roobo.com)</br>[Kurulum](speech-devices-sdk-roobo-v1.md) / [hızlı](speech-devices-sdk-android-quickstart.md)![Roobo akıllı ses Dev Seti](media/speech-devices-sdk/device-roobo-v1.jpg)|7 MIC dizisi, ARM SOC WIFI, ses çıkış, g/ç. </br>Android|Microsoft Mic Array ve yüksek kaliteli tanıma ve konuşma senaryoları geliştirmek için SDK, işleme ön uyarlamak için ilk konuşma cihaz SDK'sı|Konuşma Transkripsiyonu, akıllı konuşmacının Ses Aracısı üstte taşınır|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)![Azure Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|MIC dizisi RGB ve derinlik kameralar 7. </br>Windows/Linux|Gelişmiş bilgisayar görme ve konuşma modelleri oluşturmak için Gelişmiş yapay zeka (AI) sensör ile bir Geliştirme Seti. Bir video kamera ve yönlendirmesini algılayıcısı ile sınıfının en iyisi uzamsal mikrofon dizi ve derinlik kamera birleştirir — birden çok modları, seçeneklerini ve SDK'ları bir dizi uyum sağlamak için küçük bir cihazla içindeki tüm işlem türleri.|Konuşma Transkripsiyonu, robotlara ilişkin, akıllı oluşturma|
|Roobo Smart Audio Dev Kit 2![Roobo Smart Audio Dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|7 MIC dizisi, ARM SOC WIFI, Bluetooth, g/ç. </br>Linux|2. nesil konuşma cihaz SDK'sı, diğer işletim sistemi ve başvuru uygun maliyetli tasarımla daha fazla özellik sağlar.|Konuşma Transkripsiyonu, akıllı konuşmacının Ses Aracısı üstte taşınır|
|URbetter T11 geliştirme Panosu![URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|7 MIC dizisi, ARM SOC WIFI, Ethernet, HDMI, USB Kamera. </br>Linux|Bir sektör düzeyi Speech cihaz SDK'sı Microsoft Mic dizi uyum sağlar ve destekleyen g/ç HDMI veya Ethernet ve daha fazla USB çevre birimleri gibi genişletilmiş|Konuşma Transkripsiyonu, eğitim, hastaneler, Robotlar, OTT kutusu, aracı, sesli den sürücü|

## <a name="download-the-speech-devices-sdk"></a>Konuşma cihaz SDK'sını indirin

İndirme [konuşma cihaz SDK'sı](https://aka.ms/sdsdk-download).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihaz SDK'sı ile çalışmaya başlama](https://aka.ms/sdsdk-quickstart)
