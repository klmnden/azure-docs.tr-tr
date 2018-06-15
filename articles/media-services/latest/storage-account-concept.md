---
title: Azure Media Services depolama hesaplarında | Microsoft Docs
description: Bu makalede, Media Services depolama hesapları nasıl kullandığı açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 6d4c21867b0b46508f348300ae2b9553a75d23b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788070"
---
# <a name="storage-accounts"></a>Depolama hesapları

Bir Media Services hesabı oluştururken, bir Azure depolama hesabı kaynağın adı sağlamanız gerekir. Belirtilen depolama hesabının Media Services hesabınıza eklenir. 

Bir olmalıdır **birincil** depolama hesabı ve herhangi bir sayıda olabilir **ikincil** Media Services hesabınızla ilişkili depolama hesapları. Media Services destekler **genel amaçlı v2** (GPv2) veya **genel amaçlı v1** (GPv1) hesaplar. 

>[!NOTE]
> BLOB yalnızca hesapları olarak izin verilmez **birincil**. 

Böylece arasında hot seçme avantajlarından yararlanmak ve seyrek erişimli depolama katmanları GPv2, kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz: [Azure depolama hesabı seçenekleri](../../storage/common/storage-account-options.md). 

## <a name="assets-in-a-storage-account"></a>Bir depolama hesabı varlıkları

Media Services v3 depolama API'leri dosyaları karşıya yüklemek için kullanılır. Depolama API'leri ile Media Services, giriş dosyalarınızın karşıya yüklemek için nasıl kullanılacağını görmek için kullanıma [yerel bir dosyadan bir iş girişi oluşturma](job-input-from-local-file-how-to.md). 

> [!Note]
> Media Services API'ları kullanmadan Media Services SDK'sı tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınıza bir depolama hesabı ekleme konusunda bilgi almak için bkz: [bir hesap oluşturmanız](create-account-cli-quickstart.md).
