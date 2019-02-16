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
ms.date: 05/25/2018
ms.author: pafarley
ms.openlocfilehash: 01273ca241769c5e3bb7b7222355d32b29fd51b9
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308511"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Dışarı aktarma veya özel görüntü işleme kullanıcı verilerini silme

Özel görüntü işleme hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak müşteriler Custom Vision Service kullanarak görüntüleme, verme ve kendi verilerini silme üzerinde tam denetime sahip [eğitim API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve özel görüntü işleme kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

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
