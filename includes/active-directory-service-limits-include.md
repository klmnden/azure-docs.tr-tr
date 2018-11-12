---
title: include dosyası
description: include dosyası
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 11/06/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 104b4653e1be55c84182571a1e7f333d39f1639f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264198"
---
Kullanım kısıtlamaları ve Azure Active Directory (Azure AD) hizmeti için diğer hizmet sınırlamaları aşağıda verilmiştir.

| Kategori | Sınırlar |
| --- | --- |
| Dizinler | Tek bir kullanıcı en fazla 500 Azure AD dizini veya bir konuk olarak ait olabilir.<br/>Tek bir kullanıcı en fazla 20 dizin oluşturabilirsiniz. |
| Etki Alanları | En fazla 900 yönetilen etki alanı adları ekleyebilir. Tüm şirket içi Active Directory ile Federasyon için etki alanlarınızı ayarı, her dizinde 450'den fazla etki alanı adları ekleyebilir. |
| Nesneler |<ul><li>En fazla 500.000 nesne Azure Active Directory ücretsiz sürüm kullanıcıları tarafından tek bir dizinde oluşturulabilir.</li><li>Yönetici olmayan bir kullanıcı en fazla 250 nesne oluşturabilir.</li></ul> |
| Şema uzantıları |<ul><li>Dize türü uzantılar en fazla 256 karakter olabilir. </li><li>İkili tür uzantılar 256 bayt ile sınırlıdır.</li><li>Herhangi bir tek nesneye 100 uzantı değeri (TÜM türlerde ve TÜM uygulamalarda) yazılabilir.</li><li>“Dize” türü veya “İkili” türü tek değerli özniteliklerle yalnızca “User”, “Group”, “TenantDetail”, “Device”, “Application” ve “ServicePrincipal” varlıkları uzatılabilir.</li><li>Şema uzantıları yalnızca Graph API’si 1.21-önizleme sürümünde kullanılabilir. Bir uzantıyı kaydetmek için uygulamaya yazma erişimi verilmelidir.</li></ul> |
| Uygulamalar |En fazla 100 kullanıcı, tek bir uygulamanın sahibi olabilir. |
| Gruplar |<ul><li>En fazla 100 kullanıcı, tek bir grubun sahibi olabilir.</li><li>İstediğiniz sayıda nesne tek bir grubun üyesi olabilir.</li><li>Bir kullanıcı herhangi bir sayıda grup üyesi olabilir.</li><li>Şirket içi Active Directory'nizden Azure Active Azure AD Connect'i kullanarak dizin eşitleme grubundaki üye sayısı 50 bin üye ile sınırlıdır.</li></ul> |
| Erişim Paneli |<ul><li>Azure AD Premium veya Enterprise Mobility Suite lisansı atanmış kullanıcılar için, son kullanıcı başına Erişim Panelinde görülebilen uygulama sayısı sınırsızdır.</li><li>Azure Active Directory Ücretsiz ya da Azure AD Temel sürümlerinin lisansına sahip kullanıcılar için, son kullanıcı başına Erişim Panelinde en fazla 10 uygulama kutucuğu (örnek: Box, Salesforce veya Dropbox) görülebilir. Bu sınır, Yönetici hesapları için geçerli değildir.</li></ul> |
| Reports | Herhangi bir raporda en fazla 1.000 satır görüntülenebilir veya indirilebilir. Bunun üzerindeki veriler kesilir. |
| Yönetim birimleri | Bir nesne en fazla 30 idari birimin üyesi olabilir. |
