---
title: Azure Güvenlik Merkezi'nde (olaylar) Akıllı uyarı bağıntısı bulut | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi'nde güvenlik olaylarını oluşturulacak fusion kullandığı bulut akıllı uyarı bağıntısı şeklini açıklanmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 2ab4dab8cb7729b0c2ca023f22066f7b5d166a02
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571783"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Azure Güvenlik Merkezi'nde (olaylar) bulut akıllı uyarı bağıntısı

Güvenlik Merkezi, kötü amaçlı etkinlikler hakkında sizi uyarmak için Gelişmiş analiz ve tehdit bilgilerini kullanarak hibrit bulut iş yükleri için sürekli olarak analiz eder.

Tehdit kapsama derecesini büyür ve hatta ufak göstergesi algılamak için gereken arttıkça tehlikeye gibi farklı uyarıları önceliklendirme ve gerçek bir saldırı tanımlamak güvenlik analistleri için zor olabilir. Güvenlik Merkezi yardımcı olur, uyarı yorulma ve ortaya çıktıkları saldırıları tanılama başa analistler tarafından güvenlik olaylara farklı uyarıların ve düşük güvenilirliğe sahip sinyalleri ilişkilendirme.

Fusion teknolojisidir ve analitik arka uç destek veren Güvenlik Merkezi olaylar, farklı uyarıların ve bağlamsal sinyalleri birlikte ilişkilendirmek için etkinleştiriliyor. Fusion çalışır farklı sinyaller bakarak bir abonelikte kaynaklarında bildirilen ve saldırı ilerleme gösterir veya gösteren bir birleşik yanıt yordam paylaşılan bağlamsal bilgilerle sinyalleri yaygın desenler bulma olmalıdır Bunlar için gerçekleştirilen.

Fusion analytics güvenlik etki alanı bilgisini oluşunca yeni saldırı düzenlerini keşfetmek uyarıları çözümlemek için yapay ZEKA ile birleştirin. 

Güvenlik Merkezi, güvenlik etki alanı bilgisini resmileştirin yardımcı uyarılar algılanan bunların amacı ile ilişkilendirilecek MITRE saldırı matris yararlanır. Ayrıca, bir saldırı her adımı için topladığınız bilgileri kullanarak, Güvenlik Merkezi, saldırının adımları gibi görünüyor, ancak değil faaliyeti çıkarabilirsiniz.  

Güvenlik Merkezi, saldırıları genellikle farklı kiracılarda gerçekleşmediği her abonelikte yalnızca bu arada her ilişkilendirilmiş yerine uyarı desenleri yaygın olarak belirlenebilmesi için bildirilen saldırı dizileri analiz etmek için yapay ZEKA algoritmalarının birleştirebilirsiniz Diğer.

Bir olayın araştırma sırasında analistlerin genelde azaltılacağı tehdit ve nasıl önlenebileceğini doğası hakkında bir sonuca ulaşabilmesi için ek bağlam gerekir. Bile bir ağ anomalisinin algılandığında, örneğin, başka ağ üzerinde veya hedeflenen kaynakla ilgili neler olduğunu anlama olmadan, gerçekleştirilecek bir sonraki eylemin ne olacağını anlamak zordur. Yardımcı olmak için bir güvenlik olayı yapıtlar, ilgili olaylar ve bilgileri içerebilir. Güvenlik olayları için ek bilgiler, algılanan tehdit türüne ve ortamınızın yapılandırmasına bağlı olarak değişir. 

![Güvenlik Olay Ayrıntıları](./media/security-center-alerts-cloud-smart/security-incident.png)

Güvenlik olaylarını daha iyi anlamak için bkz: [Azure Güvenlik Merkezi'nde güvenlik olaylarını işlemek nasıl](https://docs.microsoft.com/azure/security-center/security-center-incident).

