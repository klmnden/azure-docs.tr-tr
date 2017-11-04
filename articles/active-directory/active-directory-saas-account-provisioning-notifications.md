---
title: "Hesap sağlama bildirimleri | Microsoft Docs"
description: "Hesap sağlama bildirimleri etkinleştirerek dikkat etmeniz gereken kullanıcı sağlamak için ilgili sorunların bildirim aldığından emin olmak öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a637aac7-f06b-48ef-a66d-639835a8edec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.openlocfilehash: a59e9805bd4b694a44a6c83382cce2872530cde3
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="account-provisioning-notifications"></a>Hesap Sağlama Bildirimleri
Kullanıcı sağlama ile üçüncü taraf SaaS uygulamalarında kullanıcıları yönetme sürecini otomatik hale getirebilirsiniz. <br>
Bu otomatik bir işlem olmakla birlikte, bu işlem ile etkileşim zaman zaman gereklidir. <br>
Bu yapıldığında, örneğin durumu üçüncü ile veri değişimi için yapılandırdığınız hesabı için parola taraf SaaS uygulama süresi doldu. 

Hesap sağlama bildirimleri etkinleştirerek, dikkat etmeniz gereken kullanıcı sağlamak için ilgili sorunlar bildirim aldığından emin olabilirsiniz.

Etkinleştirmek veya hesap sağlama bildirimleri yapılandırma bir üçüncü taraf SaaS uygulaması için sağlama, kullanıcı bir parçası olarak devre dışı bırakın.

![Kullanıcı hazırlama][1] 

Hesap sağlama bildirimleri etkinleştirmek için ilgili onay kutusunu seçin **onay** iletişim sayfası ve alıcının e-posta diğer adı sonra yazın.

![Hesap Sağlama Bildirimleri][2]

Bir dağıtım listesi alıcısı olarak girebilirsiniz. Ancak, bildirim e-posta yalnızca Azure AD yöneticileri tarafından erişilebilir olan raporlar bağlar olduğunu dikkate almak önemlidir.

Hesap sağlama bildirimleri etkin varsa, kullanıcı sağlama için ilgili önemli sorunları hakkında postaları alır. Ancak, bir e-posta aşırı önlemek için yalnızca bir bildirim e-posta her SaaS uygulaması için bildirim e-posta etkinleştirilmiş günde alırsınız.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Kullanıcı sağlama/sağlamayı SaaS uygulamaları için otomatik hale getirme](active-directory-saas-app-provisioning.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png
