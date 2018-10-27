---
title: Azure Stack nedir? | Microsoft Docs
description: Azure Stack Azure Hizmetleri, veri merkezinizde çalıştırmanıza olanak sağlar.
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
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 041d679cfc428278737a5668887e73f13ac694b1
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50137062"
---
# <a name="what-is-azure-stack"></a>Azure Stack nedir?

Microsoft Azure Stack, veri merkezinizde Azure Hizmetleri sunmanıza olanak sağlayan bir karma bulut platformudur. Bu platform, gelişen iş gereksinimlerinizi desteklemek için tasarlanmıştır. Azure Stack kenarı ve bağlantısı kesilmiş ortamlarda gibi modern uygulamalarınız için yeni senaryolar etkinleştirebilir veya belirli güvenlik ve uyumluluk gereksinimlerini karşılamak.

Azure Stack gereksinimlerinizi karşılamak için iki dağıtım seçeneği sunulur.

## <a name="azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemleri
Azure Stack tümleşik sistemleri, bir Microsoft ortaklık sunulur ve [donanım iş ortaklarından](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), bulut hızını belirleyebileceği yenilik sunan bir çözümü oluşturma ve yönetim Basitlik bilgi işlem. Azure Stack bir tümleşik donanım ve yazılım sistemi sunulan olduğundan, esneklik ve buluttan yenilik olanağı yanı sıra, gereken denetim sahip. Azure Stack tümleşik sistemleri boyutu 4-16 düğümlerden aralığı ve tüm dünyada donanım iş ortağı ve Microsoft tarafından desteklenir.  Yeni senaryoları oluşturabilir ve üretim iş yükleri için yeni çözümlerini dağıtmak için Azure Stack tümleşik sistemleri kullanın.

## <a name="azure-stack-development-kit"></a>Azure Stack Geliştirme Seti

Microsoft [Azure Stack Geliştirme Seti (ASDK)](.\asdk\asdk-what-is.md) değerlendirmek ve Azure Stack hakkında bilgi almak için kullanabileceğiniz Azure Stack’in tek düğümlü dağıtımıdır.  Azure ile tutarlı olan araç ve API'leri kullanarak uygulamalar oluşturmak için ASDK bir geliştirme ortamı olarak da kullanabilirsiniz.

>[!Note]
>ASDK üretim ortamı olarak kullanılmak üzere tasarlanmamıştır.

ASDK aşağıdaki sınırlamalara sahiptir:

* ASDK tek Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kimlik sağlayıcısı ile ilişkilidir. Bu dizinde birden çok kullanıcı oluşturabilir ve her kullanıcıya abonelikleri atayabilir.
* Azure Stack bileşenleri bir ana bilgisayara dağıtılan olduğundan, Kiracı kaynaklarına için sınırlı fiziksel kaynak yok. Bu yapılandırma, Ölçek veya performans değerlendirmesi için tasarlanmamıştır.
* Ağ senaryoları NIC dağıtım gereksinimleri ve tek bir konak nedeniyle sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Önemli özellikler ve kavramlar](azure-stack-key-features.md)

[Azure Stack: Azure (pdf) uzantısıdır](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
