---
title: Dışarı aktarma veya verilerinizi - özel görüntü işleme hizmeti Sil
titlesuffix: Azure Cognitive Services
description: Dışarı aktarma veya özel görüntü işleme hizmeti verilerinizi silme öğrenin.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: pafarley
ms.openlocfilehash: e3932c27b7741f04dfeda2a64f88a890b1e908ad
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054988"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Dışarı aktarma veya özel görüntü işleme kullanıcı verilerini silme

Content Moderator, hizmeti çalıştırmak için kullanıcı bilgileri toplar, ancak müşteriler Custom Vision Service kullanarak görüntüleme, verme ve kendi verilerini silme üzerinde tam denetime sahip [eğitim API](https://go.microsoft.com/fwlink/?linkid=865446).

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
