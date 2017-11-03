---
title: "Azure AD Connect: Önizleme özellikleri | Microsoft Docs"
description: "Bu konuda, Azure AD Connect önizlemede olan daha fazla ayrıntı özellikleri açıklanmaktadır."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: cbf8f729d0ebfb271bb0d8702ac043442b42c262
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="more-details-about-features-in-preview"></a>Önizleme özellikleri hakkında daha fazla ayrıntı
Bu konuda özelliklerinin şu anda önizlemede nasıl kullanılacağını açıklar.

## <a name="group-writeback"></a>Grup geri yazma
Geri yazma için sağlayan isteğe bağlı özellikler grup geri yazma için seçeneği **Office 365 grupları** yüklü olan Exchange ile orman için. Bu, her zaman bulutta yönetilir grubudur. Şirket içi Exchange varsa, böylece kullanıcılar bir şirket içi Exchange posta ile göndermek ve bu gruplardan e-postaları sonra geri bu gruplar şirket içi yazabilirsiniz.

Office 365 grupları ve bunların nasıl kullanılacağını hakkında daha fazla bilgi bulunabilir [burada](http://aka.ms/O365g).

Bir Office 365 grup şirket içi bir dağıtım grubu olarak temsil edilen AD DS. Şirket içi Exchange server, Exchange 2013 toplu güncelleştirme (Mart 2015'te yayımlanan) 8 veya bu yeni Grup türünü tanımak için Exchange 2016 olması gerekir.

**Önizleme sırasında notları**

* Adres Defteri özniteliği şu anda önizlemede doldurulmamış. Bu öznitelik olmadan, Grup GAL görünür değil. Bu öznitelik doldurmak için en kolay yolu Exchange PowerShell cmdlet'ini kullanmaktır `update-recipient`.
* Yalnızca Exchange şema ormanlarla gruplar için geçerli hedefleri ' dir. Hiçbir Exchange algılandı, grup geri yazma etkinleştirmek mümkün değildir.
* Şu anda yalnızca tek orman Exchange kuruluşu dağıtımlar desteklenir. Birden fazla kuruluşun şirket içi Exchange varsa, bu gruplar, diğer ormanlara görünmesi için bir şirket içi GALSync çözümü gerekir.
* Grup geri yazma özelliği, güvenlik grubu veya dağıtım grubu işlemez.

> [!NOTE]
> Azure AD Premium aboneliği grup geri yazma için gereklidir.
> 
>

## <a name="user-writeback"></a>Kullanıcı geri yazma
> [!IMPORTANT]
> Kullanıcı geri yazma önizleme özelliği kaldırıldı Azure AD Connect Ağustos 2015 güncelleştirmesinde. Ardından etkinleştirdiyseniz, bu özellik devre dışı bırakmalısınız.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Devam etmek, [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
