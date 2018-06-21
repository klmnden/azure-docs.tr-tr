---
title: include dosyası
description: include dosyası
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 06/20/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 10b5fbe0a03d3ea712edc9df603bbcea5e188a02
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296541"
---
Azure Active Directory hizmetine yönelik kullanım kısıtlamaları ve diğer hizmet sınırlamaları aşağıda verilmiştir.

| Kategori | Sınırlar |
| --- | --- |
| Dizinler |Tek bir kullanıcı en fazla 20 Azure Active Directory dizin ile ilişkili olabilir.<br />Olası birleşimlerin örnekleri: <ul> <li>Tek bir kullanıcı 20 dizin oluşturur.</li><li>Tek bir kullanıcı üye olarak 20 dizine eklenir.</li><li>Tek bir kullanıcı 10 dizin oluşturur ve daha sonra başkaları tarafından 10 farklı dizine eklenir.</li></ul> |
| Etki Alanları | En fazla 900 yönetilen etki alanı adları ekleyebilir. Tüm şirket içi Active Directory ile Federasyon etki alanlarınızı ayarı, her dizin için en fazla 450 etki alanı adları ekleyebilir. |
| Nesneler |<ul><li>En fazla 500.000 nesneleri tek bir dizin ücretsiz sürüm Azure Active Directory kullanıcıları tarafından oluşturulabilir.</li><li>Yönetici olmayan bir kullanıcı en fazla 250 nesne oluşturabilir.</li></ul> |
| Şema uzantıları |<ul><li>Dize türü uzantılar en fazla 256 karakter olabilir. </li><li>İkili tür uzantılar 256 bayt ile sınırlıdır.</li><li>Herhangi bir tek nesneye 100 uzantı değeri (TÜM türlerde ve TÜM uygulamalarda) yazılabilir.</li><li>“Dize” türü veya “İkili” türü tek değerli özniteliklerle yalnızca “User”, “Group”, “TenantDetail”, “Device”, “Application” ve “ServicePrincipal” varlıkları uzatılabilir.</li><li>Şema uzantıları yalnızca Graph API’si 1.21-önizleme sürümünde kullanılabilir. Bir uzantıyı kaydetmek için uygulamaya yazma erişimi verilmelidir.</li></ul> |
| Uygulamalar |En fazla 100 kullanıcı tek bir uygulama sahipleri olabilir. |
| Gruplar |<ul><li>En fazla 100 kullanıcı tek bir grubun sahiplerini olabilir.</li><li>Azure Active Directory’de herhangi bir sayıdaki nesne tek bir grubun üyesi olabilir.</li><li>Bir grupta, Azure Active Directory Dizin Eşitleme (DirSync) kullanarak şirket içi Active Directory’den Azure Active Directory’ye eşitleyebileceğiniz üye sayısı 15 bin üye ile sınırlıdır.</li><li>Bir grupta Azure AD Connect kullanarak şirket içi Active Directory’den Azure Active Directory’ye eşitleyebileceğiniz üye sayısı 50 bin üye ile sınırlıdır.</li></ul> |
| Erişim Paneli |<ul><li>Azure AD Premium veya Enterprise Mobility Suite lisansı atanmış kullanıcılar için, son kullanıcı başına Erişim Panelinde görülebilen uygulama sayısı sınırsızdır.</li><li>Azure Active Directory Ücretsiz ya da Azure AD Temel sürümlerinin lisansına sahip kullanıcılar için, son kullanıcı başına Erişim Panelinde en fazla 10 uygulama kutucuğu (örnek: Box, Salesforce veya Dropbox) görülebilir. Bu sınır, Yönetici hesapları için geçerli değildir.</li></ul> |
| Reports | Herhangi bir raporda en fazla 1.000 satır görüntülenebilir veya indirilebilir. Bunun üzerindeki veriler kesilir. |
| Yönetim birimleri | Bir nesne en fazla 30 idari birimin üyesi olabilir. |
