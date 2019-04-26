---
title: Grup İlkesi ve MDM ayarları | Microsoft Docs
description: Şirkete ait cihazlarda kullanılması gereken Yönetimi (MDM) ayarları Grup İlkesi ve mobil cihaz hakkında bilgi sağlar. Bu ilkeler, kullanıcının cihazın tamamını uygulanır.
services: active-directory
keywords: Grup İlkesi ve MDM ayarları Kurumsal durumda dolaşım, Kurumsal durumda dolaşım, windows bulut nedir
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f1b23862edeabbf8e03d4589fa3f22a3fab9f72
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60353168"
---
# <a name="group-policy-and-mdm-settings"></a>Grup İlkesi ve MDM ayarları
Bu ilkeler, kullanıcının cihazın tamamını uygulandığından yalnızca şirkete ait cihazlarda bu Grup İlkesi ve mobil cihaz Yönetimi (MDM) ayarlarını kullanın. Cihaz kullanıcıya ait kişisel için ayarları eşitleme devre dışı bırakmak için bir MDM ilkesi uygulayarak, cihazın kullanımını olumsuz etkiler. Ek olarak, diğer cihazda kullanıcı hesaplarını ilkeden etkilenir.

(Yönetilmeyen) kişisel cihazlar için Dolaşım yönetmek istediğiniz kuruluşların etkinleştirmek veya Grup İlkesi veya MDM kullanmak yerine dolaşımı devre dışı bırakmak için Azure portalını kullanabilirsiniz
Aşağıdaki tablolar kullanılabilir ilke ayarlarını açıklar.

## <a name="mdm-settings"></a>MDM ayarları
MDM ilke ayarları, Windows 10 ve Windows 10 Mobile için geçerlidir.  Windows 10 Mobile destek kullanıcının OneDrive hesabına Dolaşım tabanlı yalnızca Microsoft hesabı yok.  Lütfen [cihazları ve uç noktaları](enterprise-state-roaming-windows-settings-reference.md) ayrıntılı Azure AD tabanlı eşitleme için hangi cihazlar desteklenir.

| Ad | Açıklama |
| --- | --- |
| Microsoft hesabı bağlantısına izin ver |Kullanıcıların cihazda bir Microsoft hesabı kullanarak kimlik doğrulaması yapmasına izin verir |
| Ayarlarımı eşitlemesine izin ver |Windows ayarları ve uygulama verilerini Dolaşımda olabilen kullanıcılara; Bu ilke devre dışı bırakma eşitleme, hem de mobil cihazlarda yedeklemeleri devre dışı bırakır |

## <a name="group-policy-settings"></a>Grup ilkesi ayarları
Grup İlkesi ayarları, bir Active Directory etki alanına katılmış Windows 10 cihazları için geçerlidir. Tablo, ayrıca 'tanımı kullanmayın ile' belirtilmiştir, eşitleme ayarlarını yönetmek için görünür, ancak Kurumsal durumu Dolaşım için Windows 10, çalışmaz eski ayarları içerir.

Bu ayarlar şu adreste bulunabilir: `Computer Configuration > Administrative Templates > Windows Components > Sync your settings` 

| Ad | Açıklama |
| --- | --- |
| Hesaplar: Blok Microsoft hesapları |Bu ilke ayarı kullanıcıların bu bilgisayarda yeni Microsoft hesapları eklemesini engeller. |
| Eşitleme değil |Windows ayarları ve uygulama verilerini gerekmeden kullanıcıların engeller |
| Eşitleme kişiselleştirin |Devre dışı bırakır Temalar grubunun eşitleniyor |
| Tarayıcı ayarlarını eşitliyor musunuz |Devre dışı bırakır Internet Explorer grubunun eşitleniyor |
| Parola Eşitleme yok |Parolaları grubu devre dışı bırakır eşitleniyor |
| Diğer Windows ayarları eşitleme değil |Diğer Windows Ayarları grubu devre dışı bırakır eşitleniyor |
| Masaüstü kişiselleştirme eşitliyor musunuz |Kullanmayın; hiçbir etkisi yoktur |
| Tarifeli bağlantılarda eşitliyor musunuz |Dolaşım devre dışı bırakır, bağlantılar, cep telefonu 3 G gibi ölçülür. |
| Uygulamaları eşitliyor musunuz |Kullanmayın; hiçbir etkisi yoktur |
| Uygulama ayarları eşitleme değil |Devre dışı bırakır uygulama verileri dolaşımı |
| Başlangıç ayarları eşitleme değil |Kullanmayın; hiçbir etkisi yoktur |

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için bkz. [Kurumsal durumda dolaşıma genel bakış](enterprise-state-roaming-overview.md).


