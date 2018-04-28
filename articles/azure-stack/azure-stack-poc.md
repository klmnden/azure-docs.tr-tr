---
title: Azure Stack nedir? | Microsoft Docs
description: Azure yığını, Azure Hizmetleri, veri merkezinizde çalıştırmanızı sağlar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: d54f392eddfcca99e7fe0b037b9efd0a4e90433c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-azure-stack"></a>Azure Stack nedir?

Microsoft Azure yığın kuruluşunuzun veri merkezinden Azure Hizmetleri sunmanıza olanak sağlayan bir karma bulut platformudur.  Azure yığın kenar ve bağlantısı kesilmiş ortamlarda veya toplantı belirli güvenlik ve uyumluluk gereksinimleri gibi anahtar senaryolarda modern, uygulamalarınız için yeni senaryoları etkinleştirmek için tasarlanmıştır.  Azure yığın gereksinimlerinizi karşılamak için iki dağıtım seçeneği sunulur.

## <a name="azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemleri
Azure yığın tümleşik sistemleri, bir Microsoft ortaklık sunulur ve [donanım iş ortaklarından](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), bulut hızını belirleyebileceği yenilik sunan bir çözüm oluşturma dengeli Basitlik yönetim ile.  Azure yığın donanım ve yazılım tümleşik bir sistem olarak sunulan olduğundan hala bulutta yenilik benimsenmesi sırasında esneklik ve Denetim doğru miktarda sunulur.  Azure tümleşik yığını sistemleri boyutu 4-12 düğümlerinden aralığı ve ortaklaşa donanım iş ortakları ve Microsoft tarafından desteklenir.  Azure tümleşik yığını sistemleri, üretim iş yükleri için yeni senaryoları etkinleştirmek için kullanın.    

## <a name="azure-stack-development-kit"></a>Azure Stack Geliştirme Seti
Microsoft [Azure yığın Geliştirme Seti (ASDK)](.\asdk\asdk-what-is.md) değerlendirmek ve Azure yığın hakkında bilgi almak için kullanabileceğiniz Azure yığınının tek düğümlü dağıtımıdır.  Burada API'lerini kullanarak ve Azure ile tutarlı araç geliştirebilirsiniz Geliştirici ortamı ASDK de kullanabilirsiniz. ASDK bir üretim ortamına olarak kullanılmak üzere tasarlanmamıştır.

ASDK aşağıdaki sınırlamalara sahiptir:
* ASDK tek Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kimlik sağlayıcısı ile ilişkilidir. Bu dizinde birden çok kullanıcı oluşturun ve her kullanıcı için abonelikleri atayın.
* Tüm bileşenleri tek makineye dağıtılmış, Kiracı kaynaklarına için kullanılabilir sınırlı fiziksel kaynak yok. Bu yapılandırma, Ölçek veya performans değerlendirme için tasarlanmamıştır.
* Ağ senaryoları, tek konak NIC gereksinimi nedeniyle sınırlıdır.  

## <a name="next-steps"></a>Sonraki adımlar
[Önemli özellikler ve kavramlar](azure-stack-key-features.md)

[Azure yığını: Azure (pdf) uzantısıdır](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)

