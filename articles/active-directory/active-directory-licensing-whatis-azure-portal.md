---
title: Grup tabanlı Azure Active Directory lisanslaması nedir? | Microsoft Docs
description: Azure Active Directory grup tabanlı lisans, nasıl çalıştığı ve en iyi yöntemler açıklaması
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2018
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 1d4151d6c00da618cc008257bcdad4607f8fec49
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Azure Active Directory'de Grup tabanlı lisans temelleri

Microsoft Office 365, Enterprise Mobility + güvenlik, Dynamics CRM ve benzer diğer ürünler gibi bulut Hizmetleri Ücretli kullanma lisansı gerektirir. Bu lisanslar hizmetlerin erişmesi gereken her kullanıcıya atanır. Lisansları yönetmek için yöneticiler yönetim portallarını (Office veya Azure) ve PowerShell cmdlet'leri kullanın. Azure Active Directory (Azure AD) olan tüm Microsoft bulut Hizmetleri için Kimlik Yönetimi destekleyen altyapının. Azure AD kullanıcıları için lisans Atama durumları hakkında bilgi depolar.

Şimdiye kadar lisansları yalnızca büyük ölçekli yönetim zorlaştırabilir tek tek kullanıcı düzeyinde atanabilir. Örneğin, katılma veya kuruluş ya da bir bölüm bırakarak kullanıcılar gibi kuruluş değişikliklere göre kullanıcı lisans eklemek veya kaldırmak için yönetici genellikle karmaşık bir PowerShell komut dosyası yazmanız gerekir. Bu komut, bulut hizmetine tek tek çağrılar.

Bu sorunları çözmek için Grup tabanlı lisans artık Azure AD içerir. Bir gruba bir veya daha fazla ürün lisansları atayabilirsiniz. Azure AD lisans grubunun tüm üyeleri atanmasını sağlar. Gruba katılma herhangi bir yeni üyeler uygun lisansları atanır. Gruptan ayrılmak, bu lisansların kaldırılır. Bu, bir kullanıcı başına temelinde kuruluş ve departman yapısını değişiklikleri yansıtmak üzere PowerShell aracılığıyla Lisans Yönetimi otomatikleştirme gereğini ortadan kaldırır.

## <a name="features"></a>Özellikler

Grup tabanlı lisansı önemli özellikleri şunlardır:

- Lisansları Azure AD içinde herhangi bir güvenlik grubuna atanabilir. Güvenlik grupları, Azure AD Connect kullanarak eşitlenen şirket olabilir. Güvenlik grupları (yalnızca bulut grupları olarak da bilinir) doğrudan Azure AD'de veya otomatik olarak Azure AD dinamik grup özelliği aracılığıyla da oluşturabilirsiniz.

- Bir ürün lisans bir gruba atandığında, yönetici bir veya daha fazla hizmet planları üründeki devre dışı bırakabilirsiniz. Kuruluşunuz henüz bir ürüne Hizmeti'ni kullanmaya başlamak hazır değil, genellikle, bu yapılır. Örneğin, yönetici Office 365 bir bölüme atarsanız, ancak Yammer hizmet geçici olarak devre dışı.

- Kullanıcı düzeyinde lisans gerektiren tüm Microsoft bulut hizmetlerine desteklenir. Bu, tüm Office 365 ürünler, Enterprise Mobility + güvenlik ve Dynamics CRM içerir.

- Grup tabanlı lisans edinilebilir şu anda yalnızca [Azure portalı](https://portal.azure.com). Kullanıcı ve Grup Yönetimi, Office 365 portalı gibi diğer yönetim portalları öncelikle kullanıyorsanız, bunu yapmak devam edebilirsiniz. Ancak, lisans grubu düzeyinde yönetmek için Azure Portalı'nı kullanmanız gerekir.

- Azure AD, grup üyeliği değişikliklerden kaynaklanan lisans değişiklikler otomatik olarak yönetir. Genellikle, bir üyelik değişiklik dakika içinde lisans değişiklikler etkili olur.

- Bir kullanıcı belirtilen lisans ilkelerini sahip birden fazla grup üyesi olabilir. Bir kullanıcı, doğrudan, dışında herhangi bir grup atanmış bazı lisanslar da sağlayabilirsiniz. Sonuçta elde edilen kullanıcı durumunu, tüm atanan ürün ve hizmet lisansı birleşimidir.

- Bazı durumlarda, bir kullanıcıya lisans atanamaz. Örneğin, olmayabilir kullanılabilir yeterli lisans kiracısında veya çakışan Hizmetleri aynı anda atanmış. Yöneticileri, kendisi için Azure AD tam Grup lisansları işleyemedi kullanıcılar hakkındaki bilgilere erişebilir. Bunlar daha sonra bu bilgilere dayanarak düzeltme eylemi alabilir.

- Genel Önizleme sırasında grup tabanlı lisans yönetimi kullanmak için Kiracı ücretli veya deneme aboneliği Azure AD temel veya Premium sürümleri için gereklidir.

## <a name="your-feedback-is-welcome"></a>Geri bildiriminiz Hoş Geldiniz!

Geri bildirim ya da özellik isteğiniz varsa, Lütfen bunları kullanarak bizimle paylaşın [Bu forumda](https://feedback.azure.com/forums/169401-azure-active-directory/category/317677-group-based-licensing).

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisans aracılığıyla lisans yönetimi için diğer senaryolar hakkında daha fazla bilgi için bkz:

* [Azure Active Directory lisansları kullanmaya başlama](active-directory-licensing-get-started-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans sorunlarını tanımlama ve](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory'de Grup tabanlı lisans için tek tek lisanslı kullanıcıları geçirme](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory grup tabanlı ilave senaryolar lisanslama](active-directory-licensing-group-advanced.md)
