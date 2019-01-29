---
title: Lisans Azure PIM - kullanmak için gereksinimler | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) kullanmaya yönelik lisans gereksinimlerini açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/16/2019
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ad64fba0288aa5663e008484d6f0d2cbdff0bca6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187301"
---
# <a name="license-requirements-to-use-pim"></a>PIM'i kullanmak için lisans gereksinimleri

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullanmak için bir dizin geçerli bir lisansı olması gerekir. Ayrıca, ilgili kullanıcıları ve yöneticileri lisansı atanmalıdır. Bu makalede PIM'i kullanmak için lisans gereksinimleri açıklanır.

## <a name="prerequisites"></a>Önkoşullar

PIM'i kullanmak için dizin aşağıdaki ücretli veya deneme sürümü lisansları birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md).

## <a name="which-users-must-have-licenses"></a>Hangi kullanıcıların lisansına sahip olması gerekir?

Her yönetici veya etkileşimde veya bir avantajı PIM aldığında kullanıcının bir lisansı olması gerekir. Örneklere şunlar dahildir:

- PIM kullanarak yöneticiler Azure AD rolleri ile yönetilen
- PIM kullanarak yönetilen bir Azure kaynağı rolleri olan yöneticiler
- Ayrıcalıklı rol yöneticisi rolüne atanan yöneticileri
- PIM kullanarak yönetilen Dizin rolleri için uygun olarak atanan kullanıcılar
- Kullanıcılar için PIM istekleri Onayla/Reddet
- Just-ın-time ya da doğrudan atamaları (zaman tabanlı) ile bir Azure kaynak rolüne atanan kullanıcılar  
- Erişim gözden geçirmesi için atanan kullanıcılar
- Erişim gözden geçirmeleri gerçekleştiren kullanıcı

İçin kullanılan lisansları atama hakkında daha fazla bilgi için bkz: [atamayı veya kaldırmayı lisanslarını kullanarak Azure Active Directory portalında](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Bir lisansı süresi dolduğunda ne olur?

Bir Azure AD Premium P2, EMS E5 veya deneme sürümü lisansına dolarsa, PIM özellikleri artık dizininizde kullanılabilir olacak:

- Azure AD rollerine kalıcı rol atamaları etkilenmez.
- Azure portalı, yanı sıra Graph API cmdlet'leri ve PowerShell arabirimleri PIM, PIM hizmetinde artık kullanıcıların ayrıcalıklı rolleri etkinleştirmesine, ayrıcalıklı erişim yönetmek veya erişim gözden geçirmeleri ayrıcalıklı rolleri gerçekleştirmek için kullanılabilir.
- Bu kullanıcılar ayrıcalıklı rolleri etkinleştirmesine mümkün Azure AD rolleri, uygun rol atamaları kaldırılacak.
- Azure AD rolleri tüm devam eden bir erişim gözden geçirmelerini sona erecek ve PIM yapılandırma ayarları kaldırılır.
- PIM, e-postaları artık rol atama değişiklikleri gönderir.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM kullanmaya başlama](pim-getting-started.md)
- [Rolleri PIM'de yönetemez.](pim-roles.md)
