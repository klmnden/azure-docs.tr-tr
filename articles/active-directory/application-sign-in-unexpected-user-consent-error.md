---
title: "Bir uygulama onay gerçekleştirirken beklenmeyen bir hata | Microsoft Docs"
description: "Bir uygulamaya ve bunları hakkında neler yapabileceğinizi onaylıyorsunuz işlemi sırasında oluşabilecek hatalar açıklanır"
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
ms.openlocfilehash: fd500fdd4c8642bad96dcf71eebcf1fad461a35f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Bir uygulama onay gerçekleştirirken beklenmeyen bir hata

Bu makalede, bir uygulamaya onaylıyorsunuz işlemi sırasında oluşabilecek hatalar açıklanır. Sorun giderme beklenmeyen varsa, onay ister olmayan herhangi bir hata iletisi içeren bkz [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Azure Active Directory ile tümleştirme pek çok uygulama çalışması için diğer kaynaklara erişim izinleri gerektirir. Ne zaman bu kaynakları Azure Active Directory ile bunlara erişmek için izinleri de tümleşiktir genellikle ortak izin framework kullanılarak istenir. 

Bu, genellikle bir uygulama kullanılır ancak uygulama üzerinde bir sonraki kullanımı da oluşabilir ilk kez ortaya çıkan görüntülenmesini, bir onay istemi sonuçlanır.

Belirli bir kullanıcının gerektiren bir uygulama izinleri onayı koşulların karşılanması gerekir. Bu koşullar karşılanmazsa, çeşitli hatalar oluşabilir. Bunlar:

## <a name="requesting-not-authorized-permissions-error"></a>Yetki verilmemiş izinleri hata isteme
* **AADSTS90093:** &lt;clientAppDisplayName&gt; olduğunuz bir veya daha fazla izinler yetkisi vermek istiyor. Bu uygulama, sizin adınıza onayı bir yönetici, başvurun.

Şirket Yöneticisi olmayan bir kullanıcı yalnızca bir yönetici verebilirsiniz izinleri isteyen bir uygulamayı kullanmaya çalıştığında bu hata oluşur. Bu hata, uygulama kendi kuruluş adına erişim verilirken bir yönetici tarafından çözülebilir.

## <a name="policy-prevents-granting-permissions-error"></a>İlke engeller izin verme hatası
* **AADSTS90093:** yönetici &lt;tenantDisplayName&gt; atamanızı engeller bir ilke ayarladığı &lt;uygulama adını&gt; , istediği izinleri. Bir sistem yöneticisine başvurun &lt;tenantDisplayName&gt;, kimin sizin adınıza bu uygulama izin verme.

Bu hata, yönetici olmayan bir kullanıcı onay gerektiren bir uygulama kullanma girişiminde sonra kabul uygulamalara, kullanıcılara şirket Yöneticisi kapatır oluşur. Bu hata, uygulama kendi kuruluş adına erişim verilirken bir yönetici tarafından çözülebilir.

## <a name="intermittent-problem-error"></a>Aralıklı sorun hata
* **AADSTS90090:** çalıştığınız vermek için izinleri kaydı aralıklı bir sorunla karşılaştık gibi görünüyor &lt;clientAppDisplayName&gt;. Daha sonra yeniden deneyin.

Bu hata, bir aralıklı Hizmet tarafı sorunu oluştuğunu gösterir. Uygulamayı yeniden kabul deneyerek çözülebilir.

## <a name="resource-not-available-error"></a>Kaynak kullanılamaz hatası
* **AADSTS65005:** uygulama &lt;clientAppDisplayName&gt; bir kaynağa erişmek için izinleri istenen &lt;resourceAppDisplayName&gt; kullanılabilir değil. 

Uygulama geliştiricisine başvurun.

##  <a name="resource-not-available-in-tenant-error"></a>Kaynak Kiracı hata mevcut değil
* **AADSTS65005:** &lt;clientAppDisplayName&gt; bir kaynağa erişim isteğinde &lt;resourceAppDisplayName&gt; , kuruluşunuzda mevcut değil &lt; tenantDisplayName&gt;. 

Bu kaynak kullanılabilir olduğundan emin olun veya bir sistem yöneticisine başvurun &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>İzinleri uyuşmazlığı hatası
* **AADSTS65005:** uygulama istenen kaynağa erişim izni &lt;resourceAppDisplayName&gt;. Uygulama uygulama kaydı sırasında önceden yapılandırılmış nasıl eşleşmediğinden bu isteği başarısız oldu. Uygulama vendor.* * başvurun

Bir kullanıcı için onay çalışılırken uygulama (Kiracı) kuruluşunuzun dizininde bulunan bir kaynak uygulamaya erişim izni isterken tüm ortaya bu hatalar. Bu, çeşitli nedenlerle oluşabilir:

-   İstemci uygulama geliştiricisi uygulamasına yanlış, geçersiz bir kaynak için erişim istemek için neden yapılandırdı. Bu durumda, uygulama geliştiricisi bu sorunu çözmek için istemci uygulaması yapılandırmasını güncelleştirmeniz gerekir.

-   Hedef kaynak uygulama temsil eden bir hizmet sorumlusu kuruluşunuzda mevcut değil veya geçmişte vardı ancak kaldırılmıştır. İstemci uygulaması izinlere isteyebilir bu sorunu çözmek için kaynak uygulama için bir hizmet sorumlusu kuruluşta sağlanmalıdır. Bir uygulamanın türüne bağlı olarak yol sayısı cinsinden durum olabilir dahil olmak üzere:

    -   (Microsoft yayımlanan uygulamaları) kaynak uygulama için bir abonelik alınıyor

    -   Kaynak uygulamaya onaylıyorsunuz

    -   Azure Portalı aracılığıyla uygulama izin verme

    -   Azure AD uygulama galerisinden uygulama ekleme

## <a name="next-steps"></a>Sonraki adımlar 

[Uygulamalar, izinler ve onay Azure Active Directory'de (v1 uç noktası)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Kapsam, izinleri ve onay Azure Active Directory'de (v2.0 uç noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


