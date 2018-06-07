---
title: Azure depolama tablo verileri şifrelemek | Microsoft Docs
description: Tablo verileri şifreleme Azure depolama hakkında bilgi edinin.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/11/2018
ms.author: sngun
ms.openlocfilehash: 082e8a54cc8625a4bbdea2157f73874dbc86fde2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661041"
---
# <a name="encrypt-table-data"></a>Tablo verileri şifrele
.NET Azure Storage istemci kitaplığı dizesi varlık özellikleri şifrelenmesi için INSERT destekler ve değiştirme işlemlerini. Şifrelenmiş dizelerin hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra dizelere geri dönüştürülür.    

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenecek özelliklerini belirtmeniz gerekir. Bu, ya da bir [EncryptProperty] özniteliği (için TableEntity türetilen POCO varlıklar) veya bir şifreleme çözümleyici isteği seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarı, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilci ' dir. Şifreleme sırasında istemci kitaplığının bir özellik için kablo yazılırken şifrelemek karar vermek için bu bilgileri kullanır. Temsilci özellikler nasıl şifrelenir geçici mantığı olasılığı için de sağlar. (Örneğin, X ise ardından özelliği A şifrelemek; Aksi takdirde özellikleri A ve b şifreleme) Bu bilgileri okurken veya varlıkları sorgulamak gerekli değildir.

## <a name="merge-support"></a>Destek birleştirme

Birleştirme şu anda desteklenmiyor. Bir alt özellikler kümesini daha önce farklı bir anahtar kullanılarak şifrelenmiş olabilecek olduğundan, sadece yeni özellikleri birleştirme ve meta verilerini güncelleştirme veri kaybı ile sonuçlanır. Ya da birleştirme önceden var olan varlık hizmetinden okumak için fazladan hizmeti çağrıları yapma gerektirir veya yeni bir anahtar özellik başına kullanarak, her ikisi de performansı artırmak için uygun değildir.     

Tablo verisi şifreleme hakkında daha fazla bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Modelleme ilişkileri](table-storage-design-modeling.md)
- [Modelleme ilişkileri](table-storage-design-modeling.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)