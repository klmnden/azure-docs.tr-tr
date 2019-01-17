---
title: Azure ve AppSource Market teklifleri | Microsoft Docs
description: Oluşturma ve yönetme, Azure ve AppSource Marketlerden sunar
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: 0d6879c6b9be06af4bb289761cc8f26b7e56d18e
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355851"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure ve AppSource Market teklifleri

Bu bölümde bu ilk kısmı oluşturup AppSource Marketlerden ve Azure için teklifleri yönetmek için kullanılan genel işlemler sunar.  Bu bölümü, tüm ortak teknik bilgileri yanı sıra belirli teklif türlerini yönetmek için anlamanız gereken arka plan türleri sunulur sağlar.  Bu bölümde çoğunu oluşturmak ve belirli teklif türlerini yönetmek ayrıntılı yönergeler içerir.  

Aşağıdaki videoda, çeşitli özellikleri ve Azure Market veya Appsource'ta kullanılabilir farklı teklife türlerini tanıtır.  Ayrıca, önemli teknik ve bu marketlerden içinde bir uygulama veya hizmet yayımlama iş yönlerini kapsar.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Yapı uygulamalar ve hizmetler için Azure Market ve AppSource - oluşturun 2018**

Bu Market hakkında daha fazla bilgi için bkz. [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](../marketplace-publishers-guide.md).


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Market ve AppSource türleri sunulur.

Tarafından desteklenen geçerli teklif türleri aşağıdaki tabloda [bulut iş ortağı portalı](https://cloudpartner.azure.com).  Her bir teklif türü için teklif burada listelenebilir marketplace(s) yanı sıra, teklif çözümü teknolojiye genel bir açıklamasını listeler.

|                Teklif Türü                |  Market  |   Açıklama                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure uygulaması](./azure-applications/cpp-azure-app-offer.md) | Azure | Çözüm, bir veya daha fazla bir Azure Resource Manager şablonu aracılığıyla dağıtılan sanal makinelerin (VM'ler), isteğe bağlı özel Azure Kod oluşur.  Dağıtım veya yayımcı tarafından yönetilen bir çözüm şablonu müşteri tarafından ya da olabilir. Bu tür, sağlanan sanal makine teklif türüne göre daha fazla esneklik sağlamak için kullanılır.  |
| [Danışmanlık hizmeti](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Her ikisi de | Microsoft ile nitelenen danışmanlar özel etki alanı hizmetlerini Azure Market veya Appsource'ta listeleyebilirsiniz.  Uzmanlara sorunlarını değerlendirme oluşturma ve doğru çözümleri dağıtma müşterilerin kendi iş hedeflerinize ulaşmanızı yardımcı olur.  |
| [Kapsayıcı](./containers/cpp-containers-offer.md)  | Azure | Kubernetes tabanlı bir hizmet veya Azure Container Instances olarak sağlanan bir Docker kapsayıcı görüntüsü çözümüdür. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Bu Kurumsal Kaynak planlama (ERP) genişleten paketi ve iş yönetim sistemi. |
| [Dynamics 365 müşteri katılımı için](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Bu müşteri genişleten bir paketi, satış, hizmet, proje hizmeti ve alan hizmet modülleri aracılığıyla kaynak yönetimi (CRM) sistemi.  |
| [Finans ve operasyon için Dynamics 365](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Bu Kurumsal Kaynak planlama (ERP) hizmeti bu Gelişmiş destekler Finans, işlemler, üretim ve tedarik zinciri yönetimi genişleten bir paket. |
| [IOT Edge Modülü](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Bir IOT Edge cihazında çalıştırılır ve Docker ile uyumlu kapsayıcısı.  Bu, özel kod, diğer Azure Hizmetleri ve 3. taraf hizmetleri kullanan küçük hesaplama modüller içerir. |
| [SaaS uygulama](./saas-app/cpp-saas-offer.md) | Azure | Hangi kullanıcıların Azure Active Directory'den yararlanır özelleştirilmiş bir arabirim oturum açın bir yayımcı tarafından yönetilen bir hizmet olarak yazılım abonelikte çözümüdür. |
| [Sanal makine](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Çözüm, müşterinin hizmet aboneliği dağıtılan tek bir sanal makine içinde yer alır.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Daha fazla bilgi için [yayımlama Kılavuzu'nu teklif türüne göre](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Sonraki adımlar

Market teklifleri ve ortak teknik öznitelikler ve varlıklar konusunda gerçekleştirebileceğiniz genel işlemler hakkında bilgi edineceksiniz [Yönetme Teklifler](./manage-offers/cpp-manage-offers.md).
