---
title: Dışarı aktarma veya özel görme, Azure Bilişsel hizmetler verilerinizi silme | Microsoft Docs
description: Dışarı aktarma veya özel görme verilerinizi silmek öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 49fc49b3181f56c23167cfbae18911e7db441f8c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355379"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Dışarı aktarma veya özel görme kullanıcı verilerini sil

Denetleyici hizmeti çalıştırmak için kullanıcı verilerini toplar, ancak müşterilerin özel görme hizmetini kullanarak kendi verileri silme görüntüleme ve dışarı aktarma üzerinde tam denetim sahip içerik [eğitim API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Dışarı aktarma ve özel görme kullanıcı verilerini silme hakkında daha fazla bilgi için aşağıdaki tabloya bakın.

| Veriler | Dışarı aktarma işlemi | İşlemi Siler |
| ---- | ---------------- | ---------------- |
| Hesap bilgileri (Abonelik anahtarları) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure portalını (Azure abonelikleri) kullanarak silin. Veya "Hesabınız Sil" düğmesini kullanarak CustomVision.ai Ayarları sayfası (Microsoft hesabının abonelikleri) |
| Yineleme ayrıntıları | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme performans ayrıntıları | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Yineleme listesi | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Proje ve proje ayrıntılarını | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) |
| Görüntü etiketleri | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) ve [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) |
| Görüntüler | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (URI görüntüsünü karşıdan sağlar) ve [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (URI görüntüsünü karşıdan sağlar) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) |
| Dışarı aktarılan modelleri | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Hesap silme silindi |
