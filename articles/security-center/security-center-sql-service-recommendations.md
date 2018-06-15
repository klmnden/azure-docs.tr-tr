---
title: Azure SQL Hizmeti ve Azure Güvenlik Merkezi'nde veri koruma | Microsoft Docs
description: Bu belge adresleri yardımcı olacak öneriler Azure Güvenlik Merkezi'nde veri ve Azure SQL Hizmeti korumak ve güvenlik ilkeleriyle uyumlu olmak.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 0c3a11e9a86767641533b16de1b96b4c59bfdf51
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866278"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Azure SQL Hizmeti ve Azure Güvenlik Merkezi'nde veri koruma
Azure Güvenlik Merkezi, ayrıca Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması sürecinde size rehberlik öneriler oluşturur.  Önerileri Azure kaynak türleri için geçerlidir: sanal ağ, SQL ve veri ve uygulamaları makineleri (VM'ler).

Bu makalede, Azure SQL Hizmeti ve verilere uygulanır önerileri giderir. Azure SQL sunucuları ve SQL veritabanları için şifreleme ve Azure depolama hesabınızın etkinleştirme şifreleme etkinleştirme veritabanları için denetimi etkinleştirme geçici önerileri Merkezi.  Aşağıdaki tabloda kullanılabilir SQL Hizmeti ve verileri önerileri ve onu uygularsanız ne her biri yaptığını anlamanıza yardımcı olması için bir başvuru olarak kullanın.

## <a name="available-sql-service-and-data-recommendations"></a>Kullanılabilir SQL Hizmeti ve verileri önerileri
| Öneri | Açıklama |
| --- | --- |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |Denetim ve tehdit algılama için (sanal makinelerde çalışan SQL Azure SQL Hizmeti yalnızca; içermeyen) Azure SQL sunucuları Aç önerir. |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |Denetim ve tehdit algılama (sanal makinelerde çalışan SQL Azure SQL Hizmeti yalnızca; içermeyen) Azure SQL veritabanları için Aç önerir. |
| [SQL veritabanlarını saydam veri şifreleme etkinleştir](security-center-enable-transparent-data-encryption.md) |SQL veritabanları (yalnızca Azure SQL Hizmeti) için şifrelemeyi etkinleştirmek önerir. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türleri için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde, sanal makineleri koruma](security-center-virtual-machine-recommendations.md)
* [Uygulamalarınızı Azure Güvenlik Merkezi'nde koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi, ağınızda koruma](security-center-network-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
