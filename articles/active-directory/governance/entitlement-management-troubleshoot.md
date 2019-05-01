---
title: Azure AD hak yönetimi (Önizleme) - Azure Active Directory sorunlarını giderme
description: Bazı öğeleri hakkında bilgi edinin Azure Active Directory hak yönetimi (Önizleme) gidermenize yardımcı olması için denetleme yapmalıdır.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/25/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ca7955186d6c6fb1dff2acd6a2d5badd1cedaef
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541518"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Azure AD hak yönetimi (Önizleme) sorunlarını giderme

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Azure Active Directory (Azure AD) yetkilendirme yönetim gidermenize yardımcı olması için denetleme yapmalıdır bazı öğeler açıklanmaktadır.

## <a name="checklist-for-entitlement-management-administration"></a>Hak Yönetimi için Denetim listesi

* Erişim reddedildi iletisi hak yönetimi yapılandırırken almak ve bir genel Yöneticiyseniz, dizininize sahip olduğundan emin olun bir [Azure AD Premium P2 (veya EMS E5) lisans](entitlement-management-overview.md#prerequisites).  
* İletisi oluşturulurken veya görüntüleme erişimi paketleri reddedildi bir erişim elde edersiniz ve katalog oluşturan grubunun bir üyesi ise, ilk erişim paketinizi oluşturmadan önce bir katalog oluşturmanız gerekir.

## <a name="checklist-for-adding-a-resource"></a>Bir kaynak eklemek için Denetim listesi

* Zaten bir erişim paketi ile yönetmek istediğiniz bir kaynağa atanmış kullanıcılar varsa, erişim paket uygun bir ilke ile kullanıcılara atanan emin olun. Örneğin, bir grup zaten kullanıcılar grubunda olan bir erişim paketi eklemek isteyebilirsiniz. Grubu gerektirir, bu kullanıcılara erişim etseydi, böylece gruba erişimleri kaybetmeyin bunlar erişim paketleri için uygun bir ilkesi olmalıdır. Bu kaynağı içeren erişim paket isteği için kullanıcıların isteyen veya erişim pakete doğrudan atayarak erişim paket atayabilirsiniz. Daha fazla bilgi için [düzenleme ve var olan erişim paketini yönetme](entitlement-management-access-package-edit.md).

## <a name="checklist-for-request-issues"></a>İstek sorunlar için Denetim listesi

* Bir kullanıcı erişim paket erişim isteği istediğinde, bunlar kullandığınızdan emin olmak **My erişim portalı bağlantısı** erişim paketi için. Daha fazla bilgi için [kopyalama My erişim portalı bağlantı](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Bir kullanıcı My erişim Portalı'na erişim paket isteği için oturum açtığında, kuruluş hesabını kullanarak kimlik doğrulaması emin olun. Kuruluş hesabı ya da bir hesap kaynak dizini ya da erişim paket ilkeleri birinde bulunan bir dizin olabilir. Kullanıcı hesabının bir kuruluş hesabı değil ya da dizin ilkeye dahil değil, daha sonra kullanıcı erişim paket tarafından görülmez. Daha fazla bilgi için [erişim paketine erişim isteği](entitlement-management-request-access.md).

* Bir kullanıcı için kaynak dizini açmasını engellenirse, bunlar benim erişim Portalı'nda erişim istemek mümkün olmayacaktır. Kullanıcı erişim isteğinde bulunabileceği önce kullanıcının profilinden oturum açma engelleme kaldırmanız gerekir. Azure portalında oturum açma bloğu kaldırmak için tıklayın **Azure Active Directory**, tıklayın **kullanıcılar**kullanıcıya tıklayın ve ardından **profili**. Düzen **ayarları** bölümünde ve değiştirme **oturum açmayı engelle** için **Hayır**. Daha fazla bilgi için [Ekle veya Azure Active Directory'yi kullanarak kullanıcının profil bilgilerini güncelleştir](../fundamentals/active-directory-users-profile-azure-portal.md)

* Bir kullanıcı bir istek sahibi hem de bir onaylayan ise My erişim Portalı'nda istedikleri erişimi paketi üzerinde görürler değil **onaylar** sayfası. Bu davranış kasıtlıdır - bir kullanıcı kendi isteği onaylayamazsınız. Öğesini ilkesinde yapılandırılmış olan istekte erişim paket emin olun. Daha fazla bilgi için [mevcut ilkeyi düzenleyebilir](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Daha önce dizininizde açmadı, yeni bir dış kullanıcı, bir SharePoint Online sitesine de dahil olmak üzere bir erişim paket alırsa, paket erişim hesabını SharePoint Online'da sağlanana kadar tamamen teslim şekilde gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Kullanıcıların erişim Hak Yönetimi'nde nasıl alındı, raporları görüntüleme](entitlement-management-reports.md)
