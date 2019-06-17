---
title: Onaylayın veya Azure AD hak yönetimi (Önizleme) - Azure Active Directory erişim isteklerini reddedin
description: My erişim portalı onaylamak veya bir Azure Active Directory hak yönetimi (Önizleme) erişim paketinde isteklerini reddetmek için kullanmayı öğrenin.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b2d07638f6c6f153ee3640273fbee5e56df0ab2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64541533"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management-preview"></a>Onaylayın veya Azure AD hak yönetimi (Önizleme) erişim isteklerini reddedin

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure AD hak yönetimi ile erişim paketleri için onay gerektirmesini ilkelerini yapılandırma ve bir veya daha fazla onaylayanları seçin. Bu makalede nasıl belirlenmiş onaylayanlar onaylayabilir veya erişim paketler için istekleri reddetme.

## <a name="open-request"></a>Açık isteği

Onaylayın veya reddedin erişim isteklerini ilk adımı bulun ve onay bekleyen erişim isteği açın sağlamaktır. Erişim isteği açmanın iki yolu vardır.

**Önkoşul rolü:** Onaylayan

1. Microsoft Azure'dan onaylamak veya bir isteği reddetmek isteyen bir e-posta için bakın. Örnek e-posta aşağıda verilmiştir:

    ![Paket e-posta erişim isteğini onayla](./media/entitlement-management-shared/email-approve-request.png)

1. Tıklayın **onaylama veya reddetme isteğini** erişim isteğinde bulunmak için bağlantı.

1. My erişim portalında oturum açın.

E-posta yoksa, aşağıdaki adımları izleyerek onayınızı bekleyen erişim istekleri bulabilirsiniz.

1. My erişim portalında oturum açın [ https://myaccess.microsoft.com ](https://myaccess.microsoft.com).

1. Sol menüde **onaylar** erişim istekleri onay bekleyen bir listesini görmek için.

1. Üzerinde **bekleyen** sekmesinde, isteği bulun.

## <a name="approve-or-deny-request"></a>Onaylayın veya reddedin isteği

Onay bekleyen bir erişim isteği açtıktan sonra bir onaylama olun veya karar engelle yardımcı olacak Ayrıntılar görebilirsiniz.

**Önkoşul rolü:** Onaylayan

1. Tıklayın **görünümü** erişim isteği bölmesini açmak için bağlantı.

1. Tıklayın **ayrıntıları** erişim isteği ayrıntılarını görmek için.

    Ayrıntılar, kullanıcının adını, kuruluş, isteğin gönderildiği ve isteğin süresi dolar İş Gerekçesi sağlanırsa, başlangıç ve bitiş tarihi erişim.

1. Tıklayın **onaylama** veya **Reddet**.

1. Gerekirse, bir neden girin.

    ![My erişim portalı - erişim isteği](./media/entitlement-management-shared/my-access-approve-request.png)

1. Tıklayın **Gönder** kararınız göndermek için.

    Bir ilke birden çok onaylayanla yapılandırılmışsa, yalnızca bir onaylayanın onay bekliyor hakkında karar vermek gerekir. Approver kendi kararı erişim isteği gönderdikten sonra istek tamamlandı ve artık diğer onaylayanlar onaylamak veya reddetmek istek kullanılabilir. Diğer onaylayanlar, istek kararı ve kendi My erişim Portalı'ndaki karar mercii görebilirsiniz. Şu anda yalnızca tek aşamalı onay desteklenir.

    Yapılandırılmış onaylayanlar hiçbiri onaylayın veya reddedin erişim isteği mümkün değilse istek yapılandırılmış istek süresi sonra süresi dolar. Kullanıcı kendi erişim isteğinin süresinin dolduğunu ve erişim isteği yeniden göndermek gereken bildirim.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir erişim paket erişim isteği](entitlement-management-request-access.md)
- [İstek işlemini ve e-posta bildirimleri](entitlement-management-process.md)
