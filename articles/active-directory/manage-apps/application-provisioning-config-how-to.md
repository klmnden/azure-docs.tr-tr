---
title: Bir Azure AD galeri uygulamasına kullanıcı hazırlamanın nasıl yapılandırılacağını | Microsoft Docs
description: Nasıl hızlı bir şekilde sağlama ve sağlamayı kaldırma zaten Azure AD uygulama galerisinde listelenmiş uygulamaları için zengin bir kullanıcı hesabı yapılandırabilirsiniz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: be43f0e100bc96de2be916cbf52bca7d3ba51431
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784526"
---
# <a name="how-to-configure-user-provisioning-to-an-azure-ad-gallery-application"></a>Azure AD galeri uygulaması için kullanıcı sağlamayı yapılandırma

*Kullanıcı hesabı sağlama* , oluşturma, güncelleştirme ve/veya kullanıcı hesabı kayıtlarını bir uygulamanın yerel kullanıcı profili deposunda devre dışı bırakma işlemidir. Çoğu Bulut ve SaaS uygulamaları, kullanıcıların rol ve izinler kendi yerel kullanıcı profili deposunda saklar ve bu tür bir kullanıcı kaydı yerel depolarındaki varlığını olan *gerekli* çoklu oturum açma ve iş erişim için.

Azure portalında **sağlama** bu uygulama için hangi sağlama modları desteklenir, kurumsal bir uygulamayı görüntüler için sekmesinde sol gezinti bölmesinde. Bu iki değerden biri olabilir:

## <a name="configuring-an-application-for-manual-provisioning"></a>El ile sağlama için uygulamayı yapılandırma

*El ile* sağlama anlamına gelir kullanıcı hesaplarını bu uygulama tarafından sağlanan yöntemleri kullanarak el ile oluşturulması gerekir. Bu, bu uygulama için bir Yönetim Portalı'nda günlüğe kaydetme ve bir web tabanlı kullanıcı arabirimi kullanarak kullanıcı ekleme gelebilir. Veya, o uygulama tarafından sağlanan bir mekanizma kullanarak kullanıcı hesabı ayrıntıları içeren bir elektronik tablo karşıya. Uygulama tarafından sağlanan belgelere bakın veya wat mekanizmaları kullanılabilir olduğunu belirlemek için uygulamanın geliştiricisine başvurun.

El ile belirli bir uygulama için gösterilen tek mod ise, hiçbir otomatik Azure AD Bağlayıcısı sağlama uygulama için henüz oluşturulduğunu anlamına gelir. Veya uygulamanın alacağı bir otomatik sağlama bağlayıcı oluşturmak önkoşul kullanıcı yönetimini API desteklemiyor anlamına gelir.

Otomatik sağlama için belirli bir uygulama için destek talebinde bulunmak isterseniz, bir istek kullanarak doldurabilirsiniz [Azure Active Directory Uygulama isteklerini](https://aka.ms/aadapprequest).

## <a name="configuring-an-application-for-automatic-provisioning"></a>Otomatik sağlama için uygulamayı yapılandırma

*Otomatik* bu uygulama için bir Azure AD Bağlayıcısı sağlama geliştirilmiştir anlamına gelir. Azure AD sağlama hizmeti ve nasıl çalıştığı hakkında daha fazla bilgi için bkz. [sağlama kaldırmayı Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin ve](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning).

Belirli kullanıcılar ve gruplar için bir uygulama sağlama hakkında daha fazla bilgi için bkz. [kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning).

Etkinleştirme ve otomatik sağlama yapılandırmak için gereken gerçek adımlar, uygulamaya bağlı olarak farklılık gösterir.

>[!NOTE]
>Uygulamanız için sağlama ve bu adımlar hem uygulama hem de sağlama bağlantısı oluşturmak için Azure AD'de yapılandırmak için aşağıdaki ayarlama özgü Kurulum öğretici bularak başlamanız gerekir. 
>
>

Uygulama öğreticileri bulunabilir [nasıl Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

Gözden geçirin ve hangi kullanıcı (veya grup) özellikleri akış Azure ad uygulama tanımlayan iş akışları ve öznitelik eşlemelerini yapılandırma sağlamayı ayarlama ayarlanırken dikkate alınması gereken önemli bir şey olabilir. Bu içerir "eşleştirme özelliği" ayarı benzersiz biçimde tanımlayan ve kullanıcılar/gruplar iki sistem arasındaki eşleştirmek için kullanılır. Bu önemli bir işlemle ilgili daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı Azure Active Directory'de SaaS uygulamaları için öznitelik eşlemelerini sağlama özelleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings)

