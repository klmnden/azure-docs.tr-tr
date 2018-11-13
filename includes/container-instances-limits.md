---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572841"
---
| Kaynak | Varsayılan Sınır |
| --- | :--- |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına kapsayıcı grubu | 100<sup>1</sup> |
| Kapsayıcı grubu başına kapsayıcı sayısı | 60 |
| Kapsayıcı grubu başına birim sayısı | 20 |
| IP başına bağlantı noktası | 5 |
| Saat başına kapsayıcı oluşturma sayısı |300<sup>1</sup> |
| 5 dakika bir kapsayıcı oluşturma sayısı | 100<sup>1</sup> |
| Saat başına kapsayıcı silme sayısı | 300<sup>1</sup> |
| 5 dakika bir kapsayıcı silme sayısı | 100<sup>1</sup> |
| Kapsayıcı grubu başına birden çok kapsayıcı | Yalnızca Linux<sup>2</sup> |
| Azure Dosyaları birimleri | Yalnızca Linux<sup>2</sup> |
| GitRepo birimleri | Yalnızca Linux<sup>2</sup> |
| Gizli birimler | Yalnızca Linux<sup>2</sup> |

<sup>1</sup> Sınır yükseltme isteği için bir [Azure destek isteği][azure-support] oluşturun.<br />
<sup>2</sup> Bu özellik için Windows desteği planlanmıştır.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
