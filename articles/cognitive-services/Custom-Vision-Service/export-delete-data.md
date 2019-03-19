---
title: Dışarı aktarma veya verilerinizi - özel görüntü işleme hizmeti Sil
titlesuffix: Azure Cognitive Services
description: Dışarı aktarma veya özel görüntü işleme hizmeti verilerinizi silme öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.openlocfilehash: 4c395a062b1132710f888cc5a315529db082a805
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850035"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Dışarı aktarma veya özel görüntü işleme kullanıcı verilerini silme

Özel görüntü işleme hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak imajlarını görüntüleme, dışarı aktarma ve özel görüntü kullanarak verileri silme üzerinde tam denetim [eğitim API'leri](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve özel görüntü işleme kullanıcı verilerini silmek öğrenmek için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure portal (Azure abonelikleri) kullanarak silin. Veya "Hesabınızı Sil" düğmesini kullanarak CustomVision.ai Ayarları sayfasından (Microsoft hesabı abonelikleri) | 
| Yineleme ayrıntılarını | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme performans ayrıntıları | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Yinelemeler listesini | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projeleri ve Proje Ayrıntıları | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Görüntü etiketleri | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Görüntüler | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (görüntü yükleme için URI sağlar) ve [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (görüntü yükleme için URI sağlar) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Dışarı aktarılan modelleri | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Hesabını silme işlemi silindi |
