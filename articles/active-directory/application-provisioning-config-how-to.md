---
title: "Azure AD galeri uygulamaya kullanıcı sağlamayı yapılandırma | Microsoft Docs"
description: "Nasıl hızlı sağlama ve Azure AD uygulama galerisinde zaten listelenen uygulamaları etkinleştirmektir zengin bir kullanıcı hesabı yapılandırabilirsiniz"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2e38fcb30ea7632339a3ba8815a536872cfcc69e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulamaya kullanıcı sağlamayı yapılandırma

*Kullanıcı hesabı sağlama* oluşturma, güncelleştirme ve/veya bir uygulamanın yerel kullanıcı profili deposundaki kullanıcı hesabı kayıtlarını devre dışı bırakma, işlemidir. Çoğu Bulut ve SaaS uygulamaları izinleri ve kullanıcı rolü, kendi yerel kullanıcı profili deposunda depola ve bu tür bir kullanıcı kaydı yerel depolarındaki varlığını olduğundan *gerekli* çoklu oturum açma ve iş erişimi için.

Azure portalında **sağlama** hangi sağlama modları bu uygulama için desteklenen bir kuruluş uygulama görüntüler için sol Gezinti Bölmesi'nde sekmesinde. Bu iki değerden biri olabilir:

## <a name="configuring-an-application-for-manual-provisioning"></a>El ile sağlama için uygulamayı yapılandırma

*El ile* sağlama anlamına gelir kullanıcı hesaplarını bu uygulama tarafından sağlanan yöntemleri kullanarak el ile oluşturulması gerekir. Bu uygulama için bir yönetim portalı oturum açma ve bir web tabanlı kullanıcı arabirimini kullanarak kullanıcı ekleme anlamına gelebilir. Veya uygulama tarafından sağlanan bir mekanizma kullanarak, kullanıcı hesabı ayrıntı elektronik karşıya. Uygulama tarafından sağlanan belgelere bakın veya wat mekanizmaları kullanılabilir olduğunu belirlemek için uygulama geliştiricisine başvurun.

El ile belirli bir uygulama için gösterilen yalnızca modu ise, bağlayıcı sağlama hiçbir otomatik Azure AD uygulama için henüz oluşturulduğunu anlamına gelir. Veya otomatik sağlama Bağlayıcısı yapı alacağı önkoşul kullanıcı yönetimi API uygulamasını desteklemezse anlamına gelir.

Belirli bir uygulamanın için otomatik sağlamaya yönelik destek istemek istiyorsanız, bir isteğiyle doldurabilir <http://aka.ms/aadapprequest>.

## <a name="configuring-an-application-for-automatic-provisioning"></a>Otomatik sağlama için uygulamayı yapılandırma

*Otomatik* bağlayıcı sağlama Azure AD bu uygulama için geliştirilmiştir anlamına gelir. Azure AD sağlama hizmeti ve nasıl çalıştığı hakkında daha fazla bilgi için bkz: [otomatikleştirmek kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini Azure Active Directory ile SaaS uygulamalarına](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Belirli kullanıcılar ve gruplar uygulamaya sağlama hakkında daha fazla bilgi için bkz: [kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

Etkinleştirme ve otomatik sağlamayı yapılandırmak için gereken gerçek adımları uygulamaya bağlı olarak değişir.

>[!NOTE]
>Kurulum Öğreticisi, uygulamanız için sağlama ve bu adımlar uygulama ve sağlama bağlantı oluşturmak için Azure AD yapılandırmak için aşağıdaki ayarlanıyor özgü bularak başlamanız gerekir. 
>
>

Uygulama öğreticileri bulunabilir [SaaS uygulamaları Azure Active Directory ile tümleştirme için nasıl öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Gözden geçirin ve hangi kullanıcı (veya grubu) özellikleri akışı Azure AD'den uygulamaya tanımlamak iş akışları ve öznitelik eşlemelerini yapılandırmak için sağlama yukarı ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleşen özellik" ayarı benzersiz olarak tanımlamak ve kullanıcıları/grupları iki sistem arasında eşleştirmek için kullanılabilir. Bu önemli işlemi hakkında daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini hazırlama özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

