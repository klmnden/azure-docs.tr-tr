---
title: Azure Media Services, depolama hesapları | Microsoft Docs
description: Bu makalede, Media Services depolama hesaplarını nasıl kullandığı açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: f5fc1385bff92851db81cd4ed397d66cb8a832f2
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54350940"
---
# <a name="storage-accounts"></a>Depolama hesapları

Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Media Services hesabı ve ilişkili depolama hesabı aynı veri merkezinde ve aynı kaynak grubunun parçası olması gerekir.

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. 

>[!NOTE]
> Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. 

Sık erişimli arasında seçim avantajlarından yararlanmak ve seyrek erişimli depolama katmanları, GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakışın](../../storage/common/storage-account-overview.md). 

## <a name="assets-in-a-storage-account"></a>Varlıkları bir depolama hesabı

Media Services v3 sürümünde depolama API'leri, dosyaları karşıya yüklemek için kullanılır. Depolama API'leri, giriş dosyalarınızı karşıya yüklemek için Media Services ile kullanma hakkında bilgi için kullanıma [yerel bir dosyadan iş girdisi oluşturma](job-input-from-local-file-how-to.md). 

> [!Note]
> Media Services API'leri kullanmadan Media Services SDK'sı tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınızla bir depolama hesabı ekleme konusunda bilgi almak için bkz: [hesap oluşturma](create-account-cli-quickstart.md).
