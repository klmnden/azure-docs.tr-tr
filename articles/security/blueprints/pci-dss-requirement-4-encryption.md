---
title: Azure ödeme işleme şeması - şifreleme gereksinimleri
description: PCI DSS gereksinim 4
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 43f75ba9-cb4e-49ab-b3f4-09e48310bc18
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 6de3290fc2147e3c8ed63642b6e8470093898ef6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33896430"
---
# <a name="encryption-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için şifreleme gereksinimleri 
## <a name="pci-dss-requirement-4"></a>PCI DSS gereksinim 4

**Kart sahibi veri iletimini açık, ortak ağlarda şifrele**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Hassas bilgileri kötü niyetli kişilere kolayca erişilebilen ağlar üzerinden iletim sırasında şifrelenmelidir. Yanlış yapılandırılmış kablosuz ağlar ve eski şifreleme ve kimlik doğrulama protokolleri güvenlik açıkları kart sahibi veri ortamları ayrıcalıklı erişim kazanmak için bu güvenlik açıklarından yararlanma kötü niyetli kişilere hedefleri devam etmektedir.

## <a name="pci-dss-requirement-41"></a>PCI DSS gereksinim 4.1

**4.1** hassas kart sahibi veri aktarım sırasında aşağıdakiler de dahil olmak üzere açık, ortak ağlarla, korumak için (örneğin, TLS, IPSec, SSH, vb.) güçlü şifreleme ve güvenlik protokolleri kullanır:
- Yalnızca güvenilir anahtarlar ve sertifikalar kabul edilir.
- Kullanımda olan protokole yalnızca güvenli sürümleri veya yapılandırmaları destekler.
- Şifreleme düzeyi şifreleme yöntemi kullanmak için uygundur. 

> [!NOTE]
> SSL/erken TLS kullanıldığı ek A2 gereksinimleri tamamlanması gerekir.
>
> Açık, ortak ağlara örnekleri dahil ancak bunlarla sınırlı değildir:
> - Internet
> - 802.11 ve Bluetooth dahil olmak üzere kablosuz teknolojileri
> - Cep telefonu teknolojileri, örneğin, genel sistem Mobile communications (GSM) için birden çok erişim (CDMA) bölme kod
> - Genel paket radyo hizmeti (GPRS)
> - Uydu iletişimleri


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı için güçlü şifreleme gibi uygulayan bir PaaS çözümüdür:<br /><br />Şifrelenmiş veri adresindeki rest gereksinimlerini karşılamak için [Azure Storage](https://azure.microsoft.com/services/storage/) şunları kullanır:<br /><br /><ul><li>[Rest verileri için Azure Storage hizmeti şifreleme (SSE)](/azure/storage/storage-service-encryption)</li><li>SQL veritabanı: PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).</li><li>[Azure Disk şifrelemesi (Bitlocker)](/azure/security/azure-security-disk-encryption)</li></ul>Azure anahtar kasası kullanarak Azure kamu, PCI DSS ve HIPAA gereksinimleri ile hizalar.|



### <a name="pci-dss-requirement-411"></a>PCI DSS gereksinim 4.1.1

**4.1.1** kart sahibi veri aktarırken kablosuz ağlar sağlayın ya da kart sahibi veri ortamına bağlı, kimlik doğrulama ve iletim için güçlü şifreleme uygulamak için endüstrideki en iyi uygulamaları (örneğin, IEEE 802.11i) kullanın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Kablosuz ve SNMP çözümde uygulanmadı.|



## <a name="pci-dss-requirement-42"></a>PCI DSS gereksinim 4.2

**4.2** korumasız planlar son kullanıcı Mesajlaşma teknolojiler (örneğin, e-posta, anlık ileti, SMS, sohbet, vb.) tarafından hiçbir zaman gönderme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore korumasız birincil hesap numarası (PAN) veri gönderebilir uygulanan tüm Mesajlaşma çözümleri yok.|



## <a name="pci-dss-requirement-43"></a>PCI DSS gereksinim 4.3

**4.3** güvenlik ilkeleri ve kart sahibi veri aktarımları şifrelemek için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşterileri belgeleme ve kart sahibi verileri içeren iletimleri şifrelemek için sorumludur.|




