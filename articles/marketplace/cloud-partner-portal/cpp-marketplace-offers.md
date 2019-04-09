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
ms.date: 03/27/2019
ms.author: pbutlerm
ms.openlocfilehash: f537a43f5d4d0431e1659daa258e0c1453f2295b
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010592"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Azure ve AppSource Market teklifleri

Bu bölümde bu ilk kısmı oluşturup AppSource Marketlerden ve Azure için teklifleri yönetmek için kullanılan genel işlemler sunar.  Bu bölümü, tüm ortak teknik bilgileri yanı sıra belirli teklif türlerini yönetmek için anlamanız gereken arka plan türleri sunulur sağlar.  Bu bölümde çoğunu oluşturmak ve belirli teklif türlerini yönetmek ayrıntılı yönergeler içerir.  

Aşağıdaki videoda, çeşitli özellikleri ve Azure Market veya Appsource'ta kullanılabilir farklı teklife türlerini tanıtır.  Ayrıca, önemli teknik ve bu marketlerden içinde bir uygulama veya hizmet yayımlama iş yönlerini kapsar.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Yapı uygulamalar ve hizmetler için Azure Market ve AppSource - oluşturun 2018**

Bu Market hakkında daha fazla bilgi için bkz. [Azure Market'te ve Appsource'ta yayımlama Kılavuzu](../marketplace-publishers-guide.md).


## <a name="common-offer-operations"></a>Sık kullanılan teklif işlemler

Yeni bir teklif oluşturma işlemini büyük ölçüde teklif türleri arasında örneğin arasında farklı bir [Azure uygulaması teklif](./azure-applications/cpp-azure-app-offer.md) ve [danışmanlık hizmet teklifinin](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md).  Buna karşılık, diğer işlemlerinin birçoğu bir teklife gerçekleştirdiğiniz [bulut iş ortağı portalı](https://cloudpartner.azure.com) teklif türleri arasında oldukça standartlaştırılmıştır.  Bu yaygın işlemler — yayımlama, Görünüm durumu, güncelleştirme ve silme de dahil olmak üzere — bölümünde ele alınmıştır [teklifler yönetme](./manage-offers/cpp-manage-offers.md)


## <a name="test-drive"></a>Test Sürüşü

*Sürücü test* "satın almadan önce deneyin" tanıtım seçeneği etkin şekilde her teklif için müşteri sağlayan bir Market özelliğidir.  Teklif türleri aşağıdaki alt kümesi için Test Sürüşü yeteneği sınırlıdır: [Azure uygulamaları](./azure-applications/cpp-azure-app-offer.md), [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md), [Dynamics 365 müşteri katılımı için](./dyn365ce/cpp-customer-engagement-offer.md), [Finans ve operasyon için Dynamics 365](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md), [ SaaS uygulamaları](./saas-app/cpp-saas-offer.md), ve [sanal makineler](./virtual-machine/cpp-virtual-machine-offer.md).  Bu yetenek, yayımcı, teklif için özelleştirilmiş bir Test Sürüşü şablonu oluşturmak gereklidir.  Daha fazla bilgi için konudaki [Test Sürüşü](./test-drive/what-is-test-drive.md).

Test Sürüşü tanıtımlar uygulayarak sahip mevcut Market tekliflerini göz atabilirsiniz [test sürücü filtre](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?filters=test-drive). 


## <a name="azure-marketplace-and-appsource-offer-types"></a>Azure Market ve AppSource türleri sunulur.

Tarafından desteklenen geçerli teklif türleri aşağıdaki tabloda [bulut iş ortağı portalı](https://cloudpartner.azure.com).  Her bir teklif türü için teklif burada listelenebilir marketplace(s) yanı sıra, teklif çözümü teknolojiye genel bir açıklamasını listeler.

|                Teklif Türü                |  Market  |   Açıklama                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Azure uygulaması](./azure-applications/cpp-azure-app-offer.md) | Azure | Çözüm, bir veya daha fazla bir Azure Resource Manager şablonu aracılığıyla dağıtılan sanal makinelerin (VM'ler), isteğe bağlı özel Azure Kod oluşur.  Dağıtım veya yayımcı tarafından yönetilen bir çözüm şablonu müşteri tarafından ya da olabilir. Bu tür, sağlanan sanal makine teklif türüne göre daha fazla esneklik sağlamak için kullanılır.  |
| [Danışmanlık hizmeti](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Her ikisi de | Microsoft ile nitelenen danışmanlar özel etki alanı hizmetlerini Azure Market veya Appsource'ta listeleyebilirsiniz.  Uzmanlara sorunlarını değerlendirme oluşturma ve doğru çözümleri dağıtma müşterilerin kendi iş hedeflerinize ulaşmanızı yardımcı olur.  |
| [Kapsayıcı](./containers/cpp-containers-offer.md)  | Azure | Kubernetes tabanlı bir hizmet veya Azure Container Instances olarak sağlanan bir Docker kapsayıcı görüntüsü çözümüdür. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Bu Kurumsal Kaynak planlama (ERP) genişleten paketi ve iş yönetim sistemi. |
| [Müşteri Etkileşimi için Dynamics 365](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Bu müşteri genişleten bir paket kaynak yönetimi (CRM) sistemi, satış, hizmet, proje hizmeti ve alan hizmeti modülleri  |
| [Finans ve operasyon için Dynamics 365](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Bu Kurumsal Kaynak planlama (ERP) hizmeti bu Gelişmiş destekler Finans, işlemler, üretim ve tedarik zinciri yönetimi genişleten bir paket |
| [IoT Edge modülü](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Bir IOT Edge cihazında çalıştırılır ve Docker ile uyumlu kapsayıcısı.  Bu, özel kod, diğer Azure Hizmetleri ve 3. taraf hizmetleri kullanan küçük hesaplama modüller içerir. |
| [Power BI uygulaması](./power-bi/cpp-power-bi-offer.md) | AppSource | Veri kümeleri, raporlar ve panolar da dahil olmak üzere özelleştirilebilir Power BI içerik paketleri bir Power BI uygulaması |
| [SaaS uygulama](./saas-app/cpp-saas-offer.md) | Azure | Hangi kullanıcıların Azure Active Directory kullanan özelleştirilmiş bir arabirim oturum açın bir yayımcı tarafından yönetilen bir hizmet olarak yazılım abonelikte çözümüdür. |
| [Sanal makine](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Çözüm, müşterinin hizmet aboneliği dağıtılan tek bir sanal makine içinde yer alır.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Daha fazla bilgi için [yayımlama Kılavuzu'nu teklif türüne göre](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Sonraki adımlar

Market teklifleri ve ortak teknik öznitelikler ve varlıklar makaledeki gerçekleştirebileceğiniz genel işlemler hakkında bilgi edineceksiniz [Yönetme Teklifler](./manage-offers/cpp-manage-offers.md).
