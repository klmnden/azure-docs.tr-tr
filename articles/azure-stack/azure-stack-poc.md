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
ms.date: 06/04/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 848df59b01cc0c03b9a5f3dae2644d469c8651bb
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234895"
---
# <a name="what-is-azure-stack"></a>Azure Stack nedir?

Microsoft Azure Stack, veri merkezinizde Azure Hizmetleri sunmanıza olanak sağlayan bir karma bulut platformudur. Bu platform değişen iş gereksinimlerinizi destekleyecek şekilde tasarlanmıştır. Azure Stack kenarı ve bağlantısı kesilmiş ortamlarda gibi modern uygulamalarınız için yeni senaryolar etkinleştirebilir veya belirli güvenlik ve uyumluluk gereksinimlerini karşılamak.

Azure Stack gereksinimlerinizi karşılamak için iki dağıtım seçeneği sunulur.

## <a name="azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemleri
Azure Stack tümleşik sistemleri, bir Microsoft ortaklık sunulur ve [donanım iş ortaklarından](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), bulut hızını belirleyebileceği yenilik sunan bir çözümü oluşturma ve yönetim Basitlik bilgi işlem. Azure Stack bir tümleşik donanım ve yazılım sistemi sunulan olduğundan, esneklik ve buluttan yenilik olanağı yanı sıra, gereken denetim sahip. Azure Stack tümleşik sistemleri boyutu 4-12 düğümlerinden aralığı ve ortaklaşa donanım iş ortakları ve Microsoft tarafından desteklenir.  Yeni senaryoları oluşturabilir ve üretim iş yükleri için yeni çözümlerini dağıtmak için Azure Stack tümleşik sistemleri kullanın.

## <a name="azure-stack-development-kit"></a>Azure Stack Geliştirme Seti

Microsoft [Azure Stack Geliştirme Seti (ASDK)](.\asdk\asdk-what-is.md) değerlendirmek ve Azure Stack hakkında bilgi almak için kullanabileceğiniz Azure Stack’in tek düğümlü dağıtımıdır.  Azure ile tutarlı olan araç ve API'leri kullanarak uygulamalar oluşturmak için ASDK Geliştirici ortamı olarak da kullanabilirsiniz.

>[!Note]
>ASDK bir üretim ortamında kullanılmaya yönelik değildir.

ASDK aşağıdaki sınırlamalara sahiptir:

* ASDK tek Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kimlik sağlayıcısı ile ilişkilidir. Bu dizinde birden çok kullanıcı oluşturun ve her kullanıcı için abonelikleri atayın.
* Azure Stack bileşenleri bir ana bilgisayara dağıtılan olduğundan, Kiracı kaynaklarına için sınırlı fiziksel kaynak yok. Bu yapılandırma, Ölçek veya performans değerlendirme için tasarlanmamıştır.
* Ağ senaryoları tek ana bilgisayar ve NIC dağıtım gereksinimleri nedeniyle sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Önemli özellikler ve kavramlar](azure-stack-key-features.md)
- [Azure Stack: Azure (pdf) uzantısıdır](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
