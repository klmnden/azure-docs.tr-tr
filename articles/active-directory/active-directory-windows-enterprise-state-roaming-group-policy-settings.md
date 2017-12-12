---
title: "Grup İlkesi ve MDM ayarları | Microsoft Docs"
description: "Şirkete ait cihazlarda kullanılmalıdır Yönetimi (MDM) ayarları Grup İlkesi ve mobil cihaz hakkında bilgi sağlar. Bu ilkeler, tüm cihazın kullanıcısına uygulanır."
services: active-directory
keywords: "Grup İlkesi ve kurumsal durumda dolaşım, Kurumsal durumda dolaşım, windows bulut için MDM ayarları nelerdir"
documentationcenter: 
author: tanning
manager: mtillman
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 588084481ffc5cbbeed34e9527271179fa359ed5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="group-policy-and-mdm-settings"></a>Grup İlkesi ve MDM ayarları
Bu ilkeler kullanıcının tüm cihaza uygulandığından yalnızca şirkete ait cihazlarda bu Grup İlkesi ve mobil cihaz Yönetimi (MDM) ayarları kullanın. Kişisel ayarları eşitleme devre dışı bırakmak için bir MDM İlkesi uygulama, kullanıcının sahip olduğu cihaz bu aygıtın kullanımını olumsuz etkiler. Ayrıca, diğer kullanıcı hesaplarında cihaz ilke tarafından etkilenmez.

Kişisel (yönetilmeyen) cihazlar için gezici yönetmek istediğiniz kuruluşların etkinleştirmek veya Grup İlkesi veya MDM kullanmak yerine gezici devre dışı bırakmak için Azure portalını kullanabilirsiniz
Aşağıdaki tablolar kullanılabilir ilke ayarlarını tanımlar.

## <a name="mdm-settings"></a>MDM ayarları
MDM İlkesi ayarları, Windows 10 ve Windows 10 Mobile için geçerlidir.  Windows 10 Mobile destek kullanıcının OneDrive hesabına gezici dayalı yalnızca Microsoft hesabı yok.  Lütfen Azure AD tabanlı eşitleme için hangi cihazlar desteklenir ile ilgili ayrıntılar için "Aygıtları ve uç noktaları" bölümüne bakın.

| Ad | Açıklama |
| --- | --- |
| Microsoft hesabı bağlantıya izin ver |Cihazda bir Microsoft hesabı kullanarak kimlik doğrulaması yapmalarına izin verir |
| Ayarlarımı eşitlemesine izin ver |Kullanıcıların Windows ayarları ve uygulama verilerini dolaşıma izin verir; Bu ilkeyi devre dışı bırakma eşitleme yanı sıra, mobil cihazlarda yedeklemelerinin devre dışı bırakır |

## <a name="group-policy-settings"></a>Grup İlkesi ayarları
Grup İlkesi ayarları, bir Active Directory etki alanına katılmış Windows 10 cihazlarına uygulanır. Tablo ayrıca 'açıklamasında kullanmayın ile' belirtilmiştir eşitleme ayarlarını yönetmek için görünür, ancak kuruluş durumu Dolaşım için Windows 10 için çalışmaz eski ayarları içerir.

| Ad | Açıklama |
| --- | --- |
| Hesaplar: Blok Microsoft hesapları |Bu ilke ayarı, kullanıcıların bu bilgisayarda yeni Microsoft hesapları eklemesini engeller |
| Eşitleme değil |Windows ayarları ve uygulama verilerini dolaştırmayı kullanıcıların engeller |
| Eşitleme kişiselleştirme |Devre dışı bırakır Temalar grubunun eşitleniyor |
| Tarayıcı ayarlarını eşitlemesini değil |Devre dışı bırakır Internet Explorer grubunun eşitleniyor |
| Parola Eşitleme yok |Parolaları grubu devre dışı bırakır eşitleniyor |
| Diğer Windows ayarları eşitleme değil |Diğer Windows Ayarları grubu devre dışı bırakır eşitleniyor |
| Masaüstü kişiselleştirme eşitleme değil |Kullanmayın; hiçbir etkisi yoktur |
| Ölçülen bağlantılarında eşitleme değil |Dolaşım devre dışı bırakır tarifeli cep telefonu 3 G gibi bağlantısı |
| Uygulamaları eşitleme değil |Kullanmayın; hiçbir etkisi yoktur |
| Uygulama ayarları eşitleme değil |Devre dışı bırakır uygulama verileri dolaşımı |
| Başlangıç ayarları eşitleme değil |Kullanmayın; hiçbir etkisi yoktur |

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durumda Dolaşım genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Azure Active Directory'de gezici Kurumsal durumunu etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Ayarlar ve veri dolaşımı SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Windows 10 Dolaşım ayarları başvurusu](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

