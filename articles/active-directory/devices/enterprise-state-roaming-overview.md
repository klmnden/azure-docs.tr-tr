---
title: Azure Active Directory'de Kurumsal durumda Dolaşım olduğu ne? | Microsoft Docs
description: Windows cihazlarında Kurumsal durumda Dolaşım ayarları hakkında bilgi sağlar. Kurumsal durumda dolaşım, kullanıcılar Windows cihazlarını arasında birleşik bir deneyim sağlar ve yeni bir cihaz yapılandırmak için gereken süreyi azaltır.
services: active-directory
keywords: windows bulut Kurumsal durumda dolaşım, Kurumsal eşitleme nedir
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3a2a81bd8aa3fc99d033564e8a8782c79261305
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353151"
---
# <a name="what-is-enterprise-state-roaming"></a>Enterprise State Roaming nedir?

Windows 10 ile [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) kullanıcılar, kullanıcı ayarlarını ve uygulama ayarlarını verileri buluta güvenli bir şekilde eşitleme olanağı sağlar. Kurumsal durumda dolaşım, kullanıcılar Windows cihazlarını arasında birleşik bir deneyim sağlar ve yeni bir cihaz yapılandırmak için gereken süreyi azaltır. Kurumsal durumda Dolaşım standart için benzer çalışır [tüketici ayarları eşitleme](https://go.microsoft.com/fwlink/?linkid=2015135) , ilk kez sunulmuştur Windows 8'de. Ayrıca, Kurumsal durumda Dolaşım sunar:

* **Ve ayrılması şirket verilerinin tüketici** – kuruluşların kendi veri denetiminde olan ve hiçbir kurumsal veri tüketici bulut hesabındaki ya da bir kurumsal bulut hesabındaki Tüketici verileri karıştırma yoktur.
* **Gelişmiş Güvenlik** : veriler, Azure Rights Management (Azure RMS) kullanarak kullanıcının Windows 10 cihaz ayrılmadan önce otomatik olarak şifrelenir ve verileri bulutta bekleme sırasında şifrelenmiş kalır. Tüm içerik ayarları ve Windows uygulama adları gibi ad alanlarını dışında bulutta, bekleme sırasında şifrelenmiş olarak kalır.  
* **Daha iyi yönetim ve izleme** – sağlar denetim ve görünürlüğü ayarları hangi cihazların Azure AD portalı tümleştirmesi ve kuruluşunuzda kimin eşitler üzerinden. 

Kurumsal durumda Dolaşım birden çok Azure bölgelerinde kullanılabilir. Kullanılabilir bölgelerin güncelleştirilmiş listesini bulabilirsiniz [bölgeleri göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services) sayfasında Azure Active Directory altında.

| Makale | Açıklama |
| --- | --- |
| [Azure Active Directory'de Dolaşım Kurumsal durumunu etkinleştir](enterprise-state-roaming-enable.md) |Kurumsal durumda dolaşım, tüm kuruluşa bir Premium Azure Active Directory (Azure AD) aboneliğiniz ile kullanılabilir. Azure AD aboneliğiniz alma hakkında daha fazla ayrıntı için [Azure AD ürünü](https://azure.microsoft.com/services/active-directory) sayfası. |
| [Ayarlar ve veri dolaşımı hakkında SSS](enterprise-state-roaming-faqs.md) |Bu konu, BT yöneticileri, ayarları ve uygulama verilerini eşitleme hakkında olabilir. bazı sorular yanıtlanmaktadır. |
| [Grup İlkesi ve MDM ayarları için ayarları eşitleme](enterprise-state-roaming-group-policy-settings.md) |Windows 10, Grup İlkesi ve mobil cihaz Yönetimi (MDM) ilkesi ayarları ayarları eşitleme sınırlamak için sağlar. |
| [Windows 10 Dolaşım ayarları başvurusu](enterprise-state-roaming-windows-settings-reference.md) |Dolaşıma açıldı ve/veya Windows 10'da, yedeklenen tüm ayarları tam bir listesi verilmiştir. |
| [Sorun giderme](enterprise-state-roaming-troubleshooting.md) |Bu konu, sorun giderme için bazı temel adımları geçer ve bilinen sorunların bir listesini içerir. |

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal durumda Dolaşım etkinleştirme hakkında daha fazla bilgi için bkz: [Kurumsal durumda Dolaşım etkinleştirme](enterprise-state-roaming-enable.md).
