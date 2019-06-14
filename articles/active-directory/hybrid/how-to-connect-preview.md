---
title: 'Azure AD Connect: Önizlemedeki Özellikler | Microsoft Docs'
description: Bu konuda, Azure AD Connect ön izleme aşamasındalar daha fazla ayrıntı özellikler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7def733a80aea1be77825bb9069217f5f43e003
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60347813"
---
# <a name="more-details-about-features-in-preview"></a>Önizlemedeki özellikler hakkında daha fazla ayrıntı
Bu konuda özelliklerinin şu anda Önizleme aşamasında nasıl kullanılacağını açıklar.

## <a name="group-writeback"></a>Grup geri yazma
Geri yazma için isteğe bağlı özellikler grup geri yazma için bir seçenek sağlar **Office 365 grupları** yüklü olan Exchange ile orman için. Bu, her zaman bir bulutta yönetilir bir gruptur. Şirket içi Exchange varsa, kullanıcılar şirket içi Exchange posta ile gönderin ve bu gruplardan e-posta almak için daha sonra geri bu grupları şirket içi yazabilirsiniz.

Office 365 grupları ve bunların nasıl kullanılacağı hakkında daha fazla bilgi bulunabilir [burada](https://aka.ms/O365g).

Bir Office 365 grubu, şirket içi bir dağıtım grubu olarak temsil edilen AD DS. Şirket içi Exchange server, Exchange 2013 toplu güncelleştirme (Mart 2015'te yayımlanan) 8 ya da bu yeni Grup türünü tanımak için Exchange 2016 olması gerekir.

**Önizleme sırasında notları**

* Adres Defteri özniteliği şu anda önizlemede doldurulmamış. Bu öznitelik olmadan, grubun içinde GAL görünür değil. Bu öznitelik doldurmak için en kolay yolu, Exchange PowerShell cmdlet'ini kullanmaktır `update-recipient`.
* Yalnızca Exchange şema ormanlarla grupları için geçerli hedeflerdir. Hiçbir Exchange algılanırsa grup geri yazma etkinleştirmek mümkün değildir.
* Şu anda, yalnızca tek ormanlı Exchange kuruluşu dağıtımlar desteklenir. Ardından Exchange şirket içi kuruluşa birden fazla varsa, bu grupları, diğer ormanlardaki görünmesi için bir şirket içi GALSync çözümüne ihtiyaç duyarsınız.
* Grup geri yazma özelliği, güvenlik grupları veya dağıtım grupları işlemez.

> [!NOTE]
> Grup geri yazma için Azure AD Premium aboneliği gereklidir.
> 
>

## <a name="user-writeback"></a>Kullanıcı geri yazma
> [!IMPORTANT]
> Kullanıcı geri yazma önizleme özelliğini kaldırıldıktan sonra Azure AD Connect Ağustos 2015 güncelleştirmesi içinde. Ardından etkinleştirdiyseniz, bu özelliği devre dışı bırakmalısınız.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Devam etmek, [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
