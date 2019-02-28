---
title: Bulut karşıya yükleme ve Azure Media Services ile depolama | Microsoft Docs
description: Bu makalede bulut karşıya yükleme ve depolama kavramları.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: cda029dd11e8cb4cb07e9fce7eef95d6d4d78d7e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960233"
---
# <a name="cloud-upload-and-storage"></a>Bulutta karşıya yükleme ve depolama

Yönetmek, şifreleme, kodlama, çözümleme ve azure'da medya içeriği akışı başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. 

Media Services hesabı ve tüm ilişkili depolama hesapları aynı Azure aboneliğinde olması gerekir. Depolama hesapları Media Services hesabıyla aynı konumda ek gecikme süresi ve veri kullanım maliyetleri önlemek için önerilir

Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. Media Services, **General-purpose v2** (GPv2) veya **General-purpose v1** (GPv1) hesaplarını destekler. 

>[!NOTE]
> Yalnızca blob hesaplarının **Birincil** olmasına izin verilmez. 

Sık erişimli arasında seçim avantajlarından yararlanmak ve seyrek erişimli depolama katmanları, GPv2 kullanmanızı öneririz. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakışın](../../storage/common/storage-account-overview.md). 

Depolama hesabınız için seçebileceğiniz farklı SKU'ları vardır. Daha fazla bilgi için [depolama hesapları](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest). Depolama hesapları ile denemek istiyorsanız, kullanın `--sku Standard_LRS`. Ancak, üretim için bir SKU seçilmesi sırasında dikkate almanız gereken, `--sku Standard_RAGRS`, coğrafi çoğaltma için iş sürekliliği sağlar. 

## <a name="assets-in-a-storage-account"></a>Varlıkları bir depolama hesabı

Media Services v3 sürümünde depolama API'leri varlıklarına dosyaları karşıya yüklemek için kullanılır. Daha fazla bilgi için [varlıklar kavramı](assets-concept.md).

> [!Note]
> Media Services API'leri kullanmadan Media Services SDK'sı tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.
 
## <a name="next-steps"></a>Sonraki adımlar

Media Services hesabınızla bir depolama hesabı ekleme konusunda bilgi almak için bkz: [hesap oluşturma](create-account-cli-quickstart.md).
