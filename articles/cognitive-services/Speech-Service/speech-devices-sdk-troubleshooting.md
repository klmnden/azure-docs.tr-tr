---
title: Konuşma cihaz SDK'sı - konuşma Hizmetleri sorunlarını giderme
titleSuffix: Azure Cognitive Services
description: Bu makalede, konuşma cihaz SDK'sı kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlar.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wellsi
ms.openlocfilehash: 87fb35f06dcb1d1e3fb8c3ae3be64c7448162f14
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65026163"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sında sorun giderme

Bu makalede, konuşma cihaz SDK'sı kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olacak bilgiler sağlar.

## <a name="certificate-failures"></a>Sertifika hataları

Konuşma hizmetlerini kullanırken sertifika hataları alırsanız, cihazınız doğru tarih ve saat olduğundan emin olun:

1. Git **ayarları**. Altında **sistem**seçin **tarih ve saat**.

    ![Tarih ve saat ayarları altında seçin](media/speech-devices-sdk/qsg-12.png)

1. Tutun **otomatik tarih ve saat** seçeneği belirlenmiş. Altında **Select saat dilimi**, geçerli saat diliminizde seçin.

    ![Tarih ve saat dilimi seçenekleri belirleyin](media/speech-devices-sdk/qsg-13.png)

    Dev Seti'nın zaman zaman bilgisayarınızda eşleştiğini gördüğünüzde, dev Seti internet'e bağlı.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
