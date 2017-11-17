---
title: "Azure ödeme işleme şeması - gerekliliklerini"
description: PCI DSS gereksinim 7
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: ac3afee9-0471-465d-a115-67488a1635a6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 5a3c9eac552fb96309cfa791a2e72a7102662e60
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="access-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için erişim gereksinimleri 
## <a name="pci-dss-requirement-7"></a>PCI DSS gereksinim 7

**İş gerekenler tarafından kart sahibi verilere erişimi kısıtlama**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Kritik verileri yalnızca yetkili personel tarafından erişilebildiğinden emin olmak için sistemleri ve işlemleri temelinde gerekenler ve iş sorumluluklarını göre erişimi sınırlamak için bir yerde olması gerekir.

Yalnızca en az miktarda veri ve bir işi gerçekleştirmek için gereken ayrıcalık için erişim haklarına sahiptir "bilmeniz gereken" durumdur.

## <a name="pci-dss-requirement-71"></a>PCI DSS gereksinim 7.1

**7.1** sınırlamak sistemi bileşenlerine erişim ve işleri gibi erişim gerektiren kişiler kart sahibi verileri.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure zorlar Azure sistemi bileşenlerine, erişim denetim etkinliğini doğrulanması Azure Personel erişim ile ilgili, zaman içinde sadece sağlayarak mevcut ISMS ilkeleri yönetim erişimi, artık gerekli ve sağlamayı personel zaman erişimi iptal ediliyor Azure platformu ortamı erişme sahip bir iş gereksinimi. Müşteri ortamlara Azure erişim yüksek oranda kısıtlı ve yalnızca müşteri onay ile izin verilir.<br /><br />Yordamlar, veri merkezi yetkili çalışanlar, satıcılar, Yükleniciler ve ziyaretçiler için fiziksel erişimi kısıtlamak için oluşturulmuştur. Güvenlik doğrulaması ve iade personeli iç veri merkezi tesis geçici erişmesi için gereklidir. Fiziksel erişim günlüklerini her üç Azure ekipleri tarafından incelenen. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşterileri ve kart sahibi verilere yalnızca işleri gibi erişmesi kişiler için sistem bileşenleri erişimi sınırlamak için sorumludur. Bu sınırlama içerir ve Azure Yönetim Portalı'na erişimi kısıtlama hesapları veya rolleri oluşturmak için izni olan belirtme yanı sıra değiştirmek veya PaaS Hizmetleri silin.|



### <a name="pci-dss-requirement-711"></a>PCI DSS gereksinim 7.1.1

**7.1.1** tanımla erişmesi gereken her rol için de dahil olmak üzere:
- Her rol için kendi iş işlevi erişmesi gereken sistem bileşenleri ve veri kaynakları
- Kaynaklara erişim için gereken ayrıcalık (örneğin, kullanıcı, yönetici, vb.) düzeyi

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşterileri tanımlama ve bir kullanıcı kimliği onay işlemi belgeleme, en düşük ayrıcalık tanımlama, kart sahibi verilere erişimi kısıtlayarak, benzersiz kimlikler kullanarak, görevlerin ayrılmasını sağlayan ve artık gerekli olduğunda kullanıcı erişimini iptal etmek için sorumludur.|



### <a name="pci-dss-requirement-712"></a>PCI DSS gereksinim 7.1.2

**7.1.2** iş sorumluluklarını gerçekleştirmek gerekli en düşük ayrıcalık için ayrıcalıklı kullanıcı kimlikleri kısıtlayın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure geçerli şirket ve kuruluş güvenlik ilkeleri, bilgi güvenlik ilkesi de dahil olmak üzere benimsemiştir. İlkeleri alınan onaylanmış, yayımlanan ve Windows Azure için iletilen. Microsoft Azure bilgi Güvenlik İlkesi Microsoft Azure varlıklara erişimi verilen varlık sahibinin yetkilendirme ile iş gerekçesinin dayalı ve "gerek bilme" ve "en az ayrıcalıklı" ilkelerine dayalı olmasını gerektirir. İlkesi de erişim sağlama, erişim yetkilendirmesi, erişim hakları, kimlik doğrulama kaldırılmasını dahil olmak üzere erişim yönetimi yaşam döngüsü gereksinimlerini ele alır ve düzenli erişim inceler. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Kullanıcı rolleri belgelenen demo senaryoyu temel görevlerini sınırlıdır.|



### <a name="pci-dss-requirement-713"></a>PCI DSS gereksinim 7.1.3'ten

**7.1.3'ten** tek tek personel'ın iş sınıflandırması ve işlev bağlı erişimi atayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı sırasında üç hesapları oluşturur: yönetici, sqladmin ve edna (tanıtım yürütme sırasında web uygulamasına oturum varsayılan kullanıcı). Kullanıcı rolleri belgelenen demo senaryoyu temel görevlerini sınırlıdır.|



### <a name="pci-dss-requirement-714"></a>PCI DSS gereksinim 7.1.4

**7.1.4** belirtme yetkili tarafların belgelenen onay gereken ayrıcalıkları gerektirir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Müşterileri ve kart sahibi verilere yalnızca işleri gibi erişmesi kişiler için sistem bileşenleri erişimi sınırlamak için sorumludur. Bu sınırlama içerir ve Azure Yönetim Portalı'na erişimi kısıtlama hesapları veya rolleri oluşturmak için izni olan belirtme yanı sıra değiştirmek veya PaaS Hizmetleri silin.|



## <a name="pci-dss-requirement-72"></a>PCI DSS gereksinim 7.2

**7.2** kullanıcının göre erişimi kısıtlar sistemleri bileşenleri bilmeniz gereken için bir erişim denetimi sistemini kurmak ve "tümünü reddet" özellikle izin sürece ayarlanır.
Bu erişim denetimi sistem şunları içermelidir:
- 7.2.1 tüm sistem bileşenlerinin kapsamı.
- 7.2.2 bir iş sınıflandırması ve işlev göre bireylere ayrıcalık atama.
- 7.2.3 varsayılan "Reddet tüm" ayarı.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore, yalnızca belirtilen kullanıcılar için erişimi kısıtlamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).|



## <a name="pci-dss-requirement-73"></a>PCI DSS gereksinim 7.3

**7.3** güvenlik ilkeleri ve işletimsel yordamlarına kart sahibi verilere erişimi kısıtlayarak, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore belgelerine kullanım örneği ve CHD kullanan ve CHD nasıl kullanıldığını ile ilgili bir açıklama sağlar.|




