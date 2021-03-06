---
title: Yönetici desteği - Azure Batch işleme
description: Azure Batch işleme manager tümleştirmesi kullanılarak işlemeye yönelik Azure kullanma
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 6878d3b4fc8648db540d016389747eceb45d936a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436172"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Azure Batch işleme Çiftlik Yöneticileri ile kullanma

Mevcut bir kullanıyorsanız, şirket içi bir işleme Yöneticisi işleme grubu kapasite denetler ve işleme işleri büyük olasılıkla sonra Grup, işleyin.

Azure, popüler işleme yöneticileri için yerleşik destek ya da eklenti sağlar. Ardından, ekleme ve Azure Vm'leri Kullandıkça Öde uygulama lisansları ile Vm'leri ve düşük öncelikli VM'ler dahil olmak üzere, kaldırma.

Aşağıdaki işleme yöneticileri desteklenir:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Kraliyet işleme](https://www.royalrender.de/)
* [Thinkbox son tarihi](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Azure ile PipelineFX Qube kullanma

Betikleri ve Azure Batch etkinleştirme yönergeleri Qube çalışanları olduğu gibi kullanılacak VM havuzu [GitHub deposunu](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Azure ile Kraliyet işleme kullanma

Kraliyet oluşturma, VM'lerin Azure tabanlı bir işleme çiftliği genişletmenizi sağlayan yerleşik bir Azure ve Azure Batch tümleştirmeye sahiptir. Bir özeti için bkz: [Yardım dosyaları](https://www.royalrender.de/help8/index.html?Cloudrendering.html).

Azure tümleştirmesi kullanılarak Kraliyet işleme bir müşteri örneği için bkz [Jellyfish resimleri müşteri hikayesi](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Azure ile Thinkbox son kullanma

Betikleri ve Azure Batch etkinleştirme yönergeleri son kopyalanmış olduğu gibi kullanılacak VM havuzu [GitHub deposunu](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Sonraki adımlar

Azure Batch tümleştirme için uygun kullanarak işleme yöneticinize denemek eklenti ve GitHub'ı yönergeler uygunsa.