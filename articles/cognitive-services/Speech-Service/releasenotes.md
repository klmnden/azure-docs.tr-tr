---
title: Bilişsel hizmetler konuşma SDK Belgeleri | Microsoft Docs
description: Sürüm Notları - en son sürümlerde nelerin değiştiğini
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0b1559d288380cf3d0c180a225278cc13d22a5d0
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356301"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: 2018 Haziran yayını

**İşlev değişiklikleri**

- AudioInputStream

  Bir tanıyıcı, artık bir akış ses kaynağı olarak kullanabilir. Ayrıntılı bilgi için ilgili bkz [nasıl yapılır Kılavuzu](how-to-use-audio-input-streams.md).

- Ayrıntılı çıktı biçimi

  Oluşturma sırasında bir `SpeechRecognizer`, talep edebilir `Detailed` veya `Simple` çıktı biçimi. `DetailedSpeechRecognitionResult` Güvenirlik puanı, tanınan metni, ham sözcük biçiminde, normalleştirilmiş formunu ve normalleştirilmiş form ile maskelenmiş uygunsuz metin içerir.

**Değişiklik kesiliyor**

- Dönüştür `SpeechRecognitionResult.Text` gelen `SpeechRecognitionResult.RecognizedText` C#.

**Hata düzeltmeleri**

- Olası geri çağırma sorunu USP katmanda kapatma sırasında düzeltin.

- Ses giriş dosyası bir tanıyıcı tüketilen, onu gerekenden daha uzun dosya işlenecek bulunduran.

- İleti Pompalama tanıyıcı arasındaki birkaç kilitlenmeleri kaldırıldı.

- Yangın bir `NoMatch` neden hizmetinden gelen yanıt zaman aşımına uğradığında.

- Gecikmeli yüklenen Windows media foundation kitaplıkları. Bu kitaplık Only'dir mikrofon giriş için gerekli.

- Ses verilerini karşıya yükleme hızınızı iki kez özgün ses hızı hakkında sınırlıdır.

- Windows, C# .NET derlemelerini şimdi güçlü-olarak adlandırılır.

- Belge düzeltmesi: `Region` bir tanıyıcınızı oluşturmak için gerekli bir bilgidir.

Daha fazla örnekler eklenmiştir ve sürekli güncelleştirilen. En son örnekleri için bkz [konuşma SDK örnek GitHub deposunu](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma SDK 0.2.12733: 2018 May sürüm

Bilişsel hizmetler konuşma SDK'sının ilk genel Önizleme sürümü.
