---
title: Azure Deniz nedir?
description: Azure Deniz hizmet sağlayıcıları, yönetilen hizmetler, müşterilerinin daha yüksek bir otomasyon ve verimlilik için uygun ölçekte sunun olanak tanır.
author: JnHs
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
ms.service: lighthouse
manager: carmonm
ms.openlocfilehash: eb55af5a1121eb193bb76efc9f9e0b833f4b5a1f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809823"
---
# <a name="what-is-azure-lighthouse"></a>Azure Deniz nedir?

Hizmet sağlayıcıları, Azure Deniz görüntülemek ve Azure tüm müşterilerine daha yüksek bir Otomasyon, ölçek ve geliştirilmiş idare üzerinden yönetmek için bir tek denetim düzlemi sunar. Azure Deniz ile hizmet sağlayıcıları, Azure platformundaki yerleşik kapsamlı ve güçlü Yönetim Araçları'nı kullanarak yönetilen hizmetler sunabilirsiniz. Bu teklif ayrıca kurumsal BT avantaj elde edebileceği kuruluşlar birden fazla kiracıda kaynaklarını yönetme.

![Azure Deniz genel bakış diyagramı](media/azure-lighthouse-overview.jpg)

## <a name="benefits"></a>Avantajlar

Azure Deniz ederken ve verimli bir şekilde oluşturmanızı ve müşterileriniz için yönetilen Hizmetleri sunmanıza yardımcı olur. Avantajları şunlardır:

- **Uygun ölçekte Yönetim**: Müşteri kaynakları yönetmek için müşteri katılımı ve yaşam döngüsü işlemlerinin daha kolay ve daha ölçeklenebilir.
- **Daha fazla görünürlük ve müşteriler için duyarlık**: IP'niz korunur ancak müşterilerin yönettiğiniz kaynakları Eylemler ve kapsam üzerinde kesin denetim büyük görünürlüğe sahip, yönetimi için temsilci.
- **Kapsamlı ve birleşik platform araçlarını**: Araç deneyimimizi EA, CSP ve Kullandıkça Öde gibi birden çok lisans modelleri de dahil olmak üzere anahtar hizmet sağlayıcısının senaryoları ele alır. Yeni özellikler mevcut araç ve API'leri, lisans modelleri ile çalışır ve iş ortağı programları gibi [bulut çözümü sağlayıcısı programı (CSP)](https://docs.microsoft.com/partner-center/csp-overview). Seçtiğiniz Azure Deniz seçenekleri, mevcut iş akışları ve uygulamalara tümleştirilebilir ve müşterilerle yaşadığımız tarafından etkisi izleyebilirsiniz [, iş ortağı Kimliğini bağlama](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started).

Müşterilerinizin Azure kaynaklarını yönetmek için Azure Deniz kullanmayla ilişkili hiçbir ek bir maliyeti yoktur.

## <a name="capabilities"></a>Özellikler

Azure Deniz müşteri katılımı ve Yönetimi basitleştirin yardımcı olmak için birden çok yol içerir:

- **Azure kaynak yönetimi temsilcisi**: Müşterilerinizin Azure kaynaklarına güvenli bir şekilde kendi Kiracı içindeki bağlamını ve denetim düzlemi geçiş yapmak zorunda kalmadan yönetin. Daha fazla bilgi için bkz. [Azure kaynak yönetimi temsilcisi](./concepts/azure-delegated-resource-management.md).
- **Yeni Azure portalına deneyimleri**: Kiracılar arası bilgileri yeni görüntülemek getirin **müşterilerimin** sayfasını [Azure portalında](https://portal.azure.com). Karşılık gelen **hizmeti sağlayıcıları** dikey sağlar, müşterilerinizin görüntüleyin ve hizmet sağlayıcısına erişimi yönetin. Daha fazla bilgi için bkz. [görünümü ve müşteriyi yönetiyorsanız](./how-to/view-manage-customers.md) ve [görünümü ve hizmet sağlayıcılarını Yönet](./how-to/view-manage-service-providers.md).
- **Azure Resource Manager şablonları**: Yönetim görevlerini daha kolay gerçekleştirmek, Azure müşteri sayısını artırma dahil olmak üzere kaynak yönetimi temsilcisi. Daha fazla bilgi için bkz. bizim [depo örnekleri](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates) ve [yerleşik bir müşteri Azure kaynak yönetimi için temsilci](how-to/onboard-customer.md).
- **Azure Marketi'ndeki teklif Hizmetleri yönetilen**: Özel veya genel teklifler aracılığıyla müşterilere hizmetlerinizi sunmak ve otomatik olarak eklenen Azure Resource Manager şablonlarını kullanarak ekleme alternatif olarak temsil edilen Azure kaynak yönetimi sağlayın. Daha fazla bilgi için bkz. [yönetilen hizmetler Azure Marketi'ndeki teklif](./concepts/managed-services-offers.md).
- **Azure yönetilen uygulamalar**: Müşterilerinizin, kendi aboneliklerinizde dağıtılacağı ve kullanılacağı paket ve sevk uygulamalar. Uygulama hizmeti genel Azure Deniz deneyiminin bir parçası yönetmenize izin vererek kiracınızdan erişim bir kaynak grubuna dağıtılır. Daha fazla bilgi için bkz. [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).

> [!NOTE]
> Yukarıda açıklanan özellikler genel Bulutlar şu anda kullanılabilir. Tek başına hizmetlerinden bölgesel kullanılabilirliği için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/).