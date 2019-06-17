---
title: Bir uygulama için onay gerçekleştirirken beklenmeyen bir hata | Microsoft Docs
description: Bir uygulama ve bunlarla ilgili neler yapabileceğinizi onaylanıyor işlemi sırasında oluşabilecek hatalar açıklanır
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
ms.openlocfilehash: 6dff3be9a9bc7fd897f340e5fe6a4775a4914810
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65824953"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Bir uygulama için onay gerçekleştirirken beklenmedik hata

Bu makalede bir uygulamayı tümleştirip işlemi sırasında oluşabilecek hatalar açıklanmaktadır. Hata iletilerini içerip içermediğini beklenmeyen onayı istemlerini gideriyorsanız bkz [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Azure Active Directory ile tümleştirme, birçok uygulama, çalışması için diğer kaynaklara erişim izinleri gerektirir. Bu kaynakları Azure Active Directory ile bunlara erişmek için izinleri de zaman tümleşik genellikle ortak onay çerçevesine kullanarak istenir. Bir onay istemi, genellikle uygulamanın kullanılır, ancak uygulamanın bir sonraki kullanımda da meydana gelebilir ilk kez oluştuğu görüntülenir.

Belirli bir kullanıcı onayı gerektiren bir uygulama izni için koşulların karşılanması gerekir. Bu koşullar karşılanmazsa, aşağıdaki hatalar oluşabilir.

## <a name="requesting-not-authorized-permissions-error"></a>Yetkili değil izin hatasıyla isteme
* **AADSTS90093:** &lt;clientAppDisplayName&gt; olduğunuz bir veya daha fazla izinleri yetkisi vermek istiyor. Bu uygulamanın sizin adınıza onay verebildiği bir yönetici, başvurun.

Şirket Yöneticisi olmayan bir kullanıcı yalnızca yönetici izni verebilirsiniz izinleri isteyen bir uygulamayı kullanmaya çalıştığında bu hata oluşur. Bu hata, bir yöneticinin kuruluş adına bir uygulama için erişim verme çözülebilir.

## <a name="policy-prevents-granting-permissions-error"></a>İlke engeller izinleri verme hatası
* **AADSTS90093:** Yönetici &lt;tenantDisplayName&gt; verme dan engelleyen bir ilke ayarladığı &lt;uygulama adını&gt; isteniyor izinleri. Bir sistem yöneticisine başvurun &lt;tenantDisplayName&gt;, kimin bu uygulamanın sizin adınıza izin verme.

Bu hata, bir yönetici olmayan kullanıcı onayı gerektiren bir uygulamayı kullanmayı dener sonra kullanıcıların uygulamalara izin vermesi bir şirket Yöneticisi kapatır oluşur. Bu hata, bir yöneticinin kuruluş adına bir uygulama için erişim verme çözülebilir.

## <a name="intermittent-problem-error"></a>Aralıklı olarak ortaya çıkan sorunu hata
* **AADSTS90090:** Oturum açma işlemi çalıştığınız vermek için izinleri kaydı aralıklı bir sorunla karşılaşıldı gibi görünüyor &lt;clientAppDisplayName&gt;. Daha sonra tekrar deneyin.

Bu hata, bir hizmeti aralıklı yan sorun oluştuğunu gösterir. Uygulama yeniden onayı deneyerek çözülebilir.

## <a name="resource-not-available-error"></a>Kaynak kullanılamıyor hatası
* **AADSTS65005:** Uygulama &lt;clientAppDisplayName&gt; bir kaynağa erişmek için izinleri istenen &lt;resourceAppDisplayName&gt; kullanılabilir değil. 

Uygulama geliştiricisine başvurun.

##  <a name="resource-not-available-in-tenant-error"></a>Kaynak Kiracı hata yok
* **AADSTS65005:** &lt;clientAppDisplayName&gt; bir kaynağa erişim isteğinde &lt;resourceAppDisplayName&gt; kuruluşunuz içinde kullanılabilir değil &lt;tenantDisplayName &gt;. 

Bu kaynağın kullanılabilir olduğundan emin olun veya bir yöneticisine başvurun &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>İzinleri türde bir eşleşmeme hatası
* **AADSTS65005:** Uygulama istenen kaynağa erişim izni &lt;resourceAppDisplayName&gt;. Uygulama uygulama kaydı sırasında önceden yapılandırılmış nasıl eşleşmediğinden bu isteği başarısız oldu. Uygulama vendor.* * başvurun

Bu hatalar, kullanıcı onay çalışılırken uygulama (Kiracı) kuruluşunuzun dizininde bulunan bir kaynak uygulamaya erişim izni isterken hepsi oluşur. Bu durum çeşitli nedenlerle oluşabilir:

-   İstemci uygulama geliştiricisinin uygulamasına yanlış, geçersiz bir kaynağa erişim izni istemek bu neden yapılandırdı. Bu durumda, uygulama geliştiricisi, bu sorunu çözmek için istemci uygulamasının yapılandırmasını güncelleştirmeniz gerekir.

-   Hedef kaynak uygulama temsil eden bir hizmet sorumlusu, kuruluşunuzda mevcut değil veya geçmişte var ancak kaldırıldı. İstemci uygulaması izin isteyebilir bu sorunu çözmek için kaynak uygulama için bir hizmet sorumlusu kuruluşta sağlanması gerekir. Hizmet sorumlusu uygulama türüne bağlı olarak çeşitli yollarla içinde sağlanabilir dahil olmak üzere:

    -   (Microsoft yayımlanmış uygulamalar) kaynak uygulama için bir aboneliği alınırken

    -   Kaynak uygulamanın onaylanıyor

    -   Azure portal aracılığıyla uygulama izinleri veriliyor

    -   Azure AD uygulama galerisinden uygulama ekleme

## <a name="next-steps"></a>Sonraki adımlar 

[Uygulamalar, izinler ve onay Azure Active Directory'de (v1 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Kapsamlar, izinler ve onay Azure Active Directory'de (v2.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


