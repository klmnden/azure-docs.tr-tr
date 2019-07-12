---
title: Konuşma cihazları SDK Belgeleri
titleSuffix: Azure Cognitive Services
description: Sürüm Notları - en son sürümlerde nelerin değiştiğini
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: wellsi
ms.openlocfilehash: 1ca6b5265cf97ef551ec0b13b46ac934a372d2c7
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797921"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Bilişsel hizmetler konuşma cihazları SDK sürüm notları
Aşağıdaki bölümlerde listesi değişiklikleri en son sürümlerde.

## <a name="speech-devices-sdk-160"></a>Konuşma cihaz SDK'sı 1.6.0:

*   Destek [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) Windows ve Linux ile ortak [örnek uygulama](https://aka.ms/sdsdk-download)
*   Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.6.0 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>Konuşma cihaz SDK'sı 1.5.1:

*   Dahil [konuşma Transkripsiyonu](conversation-transcription-service.md) örnek uygulamada.
*   Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.5.1 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-150-2019-may-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 1.5.0: Mayıs 2019 sürüm

*   Konuşma cihaz SDK'sı GA ve artık Geçitli Önizleme sunulmuştur.
*   Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.5.0 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).
*   Yeni Uyandırma word teknoloji önemli kalitesi geliştirmeleri getirir, bozucu değişiklikleri görebilirsiniz.
*   Yeni ses işleme işlem hattı için geliştirilmiş alan uzak tanıma.

**Bozucu değişiklikler**

*   Yeni Uyandırma word teknolojisi nedeniyle tüm Uyandırma sözcükler bizim geliştirilmiş Uyandırma word portalında yeniden oluşturulmalıdır. Tamamen kaldırmak için cihaz eski sözcüklerden eski uygulamayı kaldırın.
    - adb com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp kaldırma

## <a name="cognitive-services-speech-devices-sdk-140-2019-apr-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 1.4.0: 2019 Apr sürüm

* Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.4.0 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 1.3.1: 2019 Mart sürüm

* Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.3.1 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).
*   Güncelleştirilmiş Uyandırma sözcük işleme, bozucu değişiklikleri bakın.
*   Örnek uygulama, konuşma tanıma hem çeviri için dil seçimi ekler.

**Bozucu değişiklikler**

*   [Uyandırma Word'ü yükleme](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) olmamıştı Basitleştirilmiş, şimdi uygulamanın parçası olduğundan ve cihazın ayrı yükleme gerektirmez.
*   Uyandırma sözcük tanıma değişti ve iki olay desteklenir.
    - RecognizingKeyword, gösterir (doğrulanmamış) anahtar sözcüğü metin konuşma sonucunu içerir.
    - RecognizedKeyword, belirtilen anahtar sözcüğün algılamayı tamamlandı, anahtar sözcüğü tanıma gösterir.


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 1.1.0: Kasım 2018 sürüm

* Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) sürüm 1.1.0 bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).
* Şimdiye kadar alan konuşma tanıma doğruluğunu sunduğumuz Gelişmiş ses işleme algoritması ile geliştirilmiştir.
* Örnek uygulama, Çince konuşma tanıma desteği eklendi.

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 1.0.1: Ekim 2018 sürüm

* Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) 1.0.1 sürümü bileşeni. Daha fazla bilgi için kendi [sürüm notları](https://aka.ms/csspeech/whatsnew).
* Konuşma tanıma doğruluğunu bizim ses gerçekleştirilen geliştirilmiş işlemi algoritmasıyla geliştirilecektir  
* Bir sürekli tanıma ses oturumu hatanın giderilmesidir.

**Bozucu değişiklikler**

* Bu sürümle birlikte birkaç önemli değişiklikler yapılmıştır. Lütfen denetleyin [bu sayfayı](https://aka.ms/csspeech/breakingchanges_1_0_0) API'leri için ilgili ayrıntılar için.
* KWS model dosyaları 1.0.1 konuşma cihazları SDK ile uyumlu değildir. Var olan Uyanma Word dosyaları yeni Uyandırma Word dosyaları cihaza yazıldıktan sonra silinir.

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sını buradan 0.5.0 sürümünü: 2018-Ağu sürüm

* Konuşma tanıma doğruluğunu ses işleme kodda bir hatayı düzeltirken tarafından geliştirildi.
* Güncelleştirilmiş [Speech SDK'sı](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) 0.5.0 sürümünü bileşeni. Daha fazla bilgi için kendi [sürüm notları](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma cihaz SDK'sı 0.2.12733: Mayıs 2018 sürüm

Bilişsel hizmetler konuşma cihaz SDK'sının ilk genel Önizleme sürümü.
