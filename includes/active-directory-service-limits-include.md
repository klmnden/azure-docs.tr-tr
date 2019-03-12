---
title: include dosyası
description: include dosyası
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/21/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 38f2dd301ddc2a5f8d28322856b2011bd2034c30
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554705"
---
Kullanım kısıtlamaları ve Azure Active Directory (Azure AD) hizmeti için diğer hizmet sınırlamaları aşağıda verilmiştir.

| Kategori | Sınırlar |
| --- | --- |
| Dizinler | Tek bir kullanıcı en fazla 500 Azure AD dizini veya bir konuk olarak ait olabilir.<br/>Tek bir kullanıcı en fazla 20 dizin oluşturabilirsiniz. |
| Etki Alanları | En fazla 900 yönetilen etki alanı adları ekleyebilir. Tüm şirket içi Active Directory ile Federasyon için etki alanlarınızı ayarlarsanız, her dizinde 450'den fazla etki alanı adları ekleyebilir. |
| Nesneler |<ul><li>En fazla 500.000 nesne Azure Active Directory ücretsiz sürüm kullanıcıları tarafından tek bir dizinde oluşturulabilir.</li><li>Yönetici olmayan bir kullanıcı en fazla 250 nesne oluşturabilir. Etkin nesneler ve sayısı kotayı doğru geri yüklenecek silinmiş nesneleri. Silinen nesneler yalnızca 30 günden kısa süre önce geri yüklemek kullanılabilir. Silinen artık sayısı bu değeri kotanın doğru geri yüklemek kullanılabilir olan nesneler Çeyrek 30 gündür. Belki de [bir yönetici rolü atama](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) art arda normal görevleri sırasında bu kotayı aşacak gibi yönetici olmayan kullanıcılar için.</li></ul> |
| Şema uzantıları |<ul><li>Dize türü uzantılar en fazla 256 karakter olabilir. </li><li>İkili tür uzantılar 256 bayt ile sınırlıdır.</li><li>Yalnızca 100 uzantı değerleri, çapraz *tüm* türleri ve *tüm* uygulamaları tek bir nesne olarak yazılabilir.</li><li>Yalnızca kullanıcı, Grup, TenantDetail, cihaz, uygulama ve ServicePrincipal varlık türü dize veya ikili türü tek değerli öznitelikleri ile genişletilebilir.</li><li>Şema uzantıları yalnızca Graph API'si 1.21 sürümü önizlemede kullanılabilir. Bir uzantıyı kaydetmek için uygulamaya yazma erişimi verilmelidir.</li></ul> |
| Uygulamalar |En fazla 100 kullanıcı, tek bir uygulamanın sahibi olabilir. |
| Gruplar |<ul><li>En fazla 100 kullanıcı, tek bir grubun sahibi olabilir.</li><li>İstediğiniz sayıda nesne tek bir grubun üyesi olabilir.</li><li>Bir kullanıcı herhangi bir sayıda grup üyesi olabilir.</li><li>Bir grup Azure AD Connect kullanarak şirket içi Active Directory'nizden Azure Active Directory'ye eşitleyebileceğiniz üye sayısı 50. 000'üyelerine sınırlıdır.</li></ul> |
| Erişim Paneli |<ul><li>Kullanıcı başına erişim panelinde görülen uygulama sayısı sınırı yoktur. Bu Azure AD Premium veya Enterprise Mobility Suite lisansları atanmış kullanıcılar için geçerlidir.</li><li>En fazla 10 uygulama kutucuğu, her kullanıcı için erişim panelinde görülebilir. Bu sınır, ücretsiz lisansları atanmış kullanıcılar veya Azure Active Directory, Azure AD temel sürümleri için geçerlidir. Uygulama kutucuklarına kutusu, Salesforce veya Dropbox örneklerindendir. Bu sınır, yönetici hesapları için geçerli değildir.</li></ul> |
| Reports | Herhangi bir raporda en fazla 1.000 satır görüntülenebilir veya indirilebilir. Bunun üzerindeki veriler kesilir. |
| Yönetim birimleri | Bir nesne en fazla 30 idari birimin üyesi olabilir. |
| Yönetici rolleri ve izinleri | <li>Bir grup sahibi olarak eklenemez.<li>Bir grup, rol atanamaz.<li>Varsayılan olarak kullanıcının Azure AD'de kullanıcı ayarları olan Kiracı anahtarları dışında izinleri değiştirilemez. |
