---
title: "Microsoft Azure kişisel verileri korumak | Microsoft Docs"
description: "Bu makalede Azure kişisel verileri korumak ve genel veri koruma düzenleme (GDPR) ile uyumlu kullanmanıza yardımcı olması"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 741fb17be315faacef6483cbaaa565136622cb45
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="protect-personal-data-in-microsoft-azure"></a>Microsoft Azure kişisel verileri korumak

Bu makalede, Yardım makaleleri bir dizi tanıtılır kişisel verilerinizi korumak için Azure güvenlik teknolojileri ve Hizmetleri kullanın. Çoğu Kurumsal Anahtar gereksinimini ve endüstri uyumluluk ve idare girişimleri budur. Örneğin, genel veri koruma düzenleme (GDPR) ile uyum sağlamak için bu makaleler dizide sağlanan bilgileri kullanabilirsiniz. Senaryo, sorun bildirimi ve şirket hedeflerini buraya dahil edilir.

## <a name="scenario-and-problem-statement"></a>Senaryo ve sorun bildirimi

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, masraflarını Akdeniz, Adriatic ve Baltık seas yanı sıra İngiliz Adaları arasında içinde sunmaya işlemlerini genişletmektedir. Bu çalışmalarını desteklemek için daha küçük birkaç seyahat satırlarını İtalya, Almanya, Danimarka ve İngiltere göre aldı

Şirket Microsoft Azure bulutta şirket verilerini depolamak için kullanır. Bu müşteri ve/veya çalışan bilgiler gibi şunlar olabilir:

- adresleri
- Telefon numaraları
- Vergi kimlik numaraları
- Kredi kartı bilgileri

Şirket verileri ihtiyaç bu Departmanlar için erişilebilir olmasını sağlarken müşteri ve çalışan verilerin gizliliği korumanız gerekir. (bordro ve ayırmaları Departmanlar gibi)

## <a name="company-goals"></a>Şirketin hedeflerine 

- Kişisel verileri içeren veri kaynakları, bulut depolama alanında bulunan zaman şifrelenir.

- Tek bir konumdan diğerine aktarılır kişisel veriler aktarım sırasında sırasında şifrelenir. Bu veriler sanal ağ üzerinden veya Internet üzerinden Azure Bulut ve kurumsal veri merkezi arasında seyahat ederken olduğunda true olur.

- Gizlilik ve kişisel verilerin bütünlüğünü güçlü kimlik yönetimi ve erişim denetimi teknolojileri tarafından yetkisiz erişimden korunur.

- Kişisel verileri Etkilenme Güvenlik Açıkları ve tehditleri için izleme aracılığıyla veri ihlali aracılığıyla gelen korunur.

- Depolama veya kişisel verilerin aktarılması Azure Hizmetleri güvenlik durumunu daha iyi kişisel verilerinizi korumak için fırsatlarını tanımlayabilmeniz için uygunluk.

## <a name="data-protection-guidance"></a>Veri koruma Kılavuzu

Aşağıdaki makaleler, yukarıda listelenen kişisel veri koruma hedeflerini elde yardımcı olacak teknik nasıl yapılır yönergeleri içerir:

- [Azure şifreleme teknolojileri](protect-personal-data-at-rest.md)

- [Azure şifreleme teknolojileri](protect-personal-data-in-transit-encryption.md)

- [Azure kimlik ve erişim teknolojileri](protect-personal-data-identity-access-controls.md)

- [Azure ağı güvenlik teknolojileri](protect-personal-data-network-security.md)

- [Azure Güvenlik Merkezi](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenlik bilgileri Site](https://aka.ms/AzureSecInfo)

- [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx)

- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)

- [Azure Güvenlik Ekibi Blogu](https://www.azuresecurityorg)

- [Azure.com Blog - güvenlik](https://azure.microsoft.com/blog/topics/security/)
