---
title: Bir Azure AD hak yönetimi (Önizleme) - Azure Active Directory erişim paketinde erişim isteği
description: My erişim portalı bir Azure Active Directory hak yönetimi (Önizleme) erişim paketinde erişim istemek için kullanmayı öğrenin.
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
ms.date: 04/19/2019
ms.author: rolyon
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39a50240b4360c5b4adcd6020c2b80b0f06315f7
ms.sourcegitcommit: 9ad75f83bbf0fc4623b7995794f33bbf823b31c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2019
ms.locfileid: "64541563"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management-preview"></a>Bir Azure AD hak yönetimi (Önizleme) erişim paketinde erişim isteği

> [!IMPORTANT]
> Azure Active Directory (Azure AD) Yetkilendirme Yönetimi, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="sign-in-to-the-my-access-portal"></a>My erişim portalında oturum açın

Burada bir erişim paketine erişim isteğinde bulunabileceği My erişim portalı oturum açmak için ilk adımdır bakın.

**Önkoşul rolü:** İstek sahibi

1. E-posta veya çalıştığınız proje ya da İş Yöneticisi'nden bir ileti arayın. E-posta erişimi ihtiyacınız olacak erişim paket bağlantısı içermelidir. Bağlantı ile başlar:

    `https://myaccess.microsoft.com`

1. Bağlantıyı açın.

1. My erişim portalında oturum açın.

    Kuruluş hesabınızı kullandığınızdan emin olun. Emin değilseniz, kontrol, proje ya da İş Yöneticisi ile.

## <a name="request-an-access-package"></a>İstek bir erişim paketi

My erişim Portalı'nda erişim paket bulduktan sonra bir istek gönderebilirsiniz.

**Önkoşul rolü:** İstek sahibi

1. Erişim paketi seçmek için onay işaretine tıklayın.

    ![My erişim portalı - erişim paketleri](./media/entitlement-management-shared/my-access-access-packages.png)

1. Tıklayın **erişim isteği** isteği erişim bölmesini açmak için.

1. Varsa **İş Gerekçesi** kutusu görüntülenir, erişmesi için bir gerekçe yazın.

1. Varsa **belirli bir süre için istek?** etkinleştirilmişse, select **Evet** veya **Hayır**.

1. Gerekirse, başlangıç tarihi belirtin ve bitiş tarihi.

    ![My erişim portalı - erişim isteği](./media/entitlement-management-shared/my-access-request-access.png)

1. İşiniz bittiğinde tıklayın **Gönder** isteğinizi gönderebilirsiniz.

1. Tıklayın **istek geçmişi** isteklerinizi ve durumunu bir listesini görmek için.

    Erişim paket onay gerektiriyorsa, isteği onay bekliyor durumda sunulmuştur.

## <a name="cancel-a-request"></a>Bir isteği iptal et

Erişim isteği gönderme ve istek yine **onay bekleyen** durum istek iptal edebilirsiniz.

**Önkoşul rolü:** İstek sahibi

1. My erişim portalında sol tıklayın **istek geçmişi** isteklerinizi ve durumunu bir listesini görmek için.

1. Tıklayın **görünümü** bağlantı isteği iptal etmek istiyor.

1. İstek yine ise **onay bekleyen** durum tıklayabilirsiniz **iptal isteği** isteği iptal etmek için.

    ![My erişim portalı - isteği iptal et](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. Tıklayın **istek geçmişi** isteği iptal edildi onaylamak için.

## <a name="select-a-policy"></a>İlke seçme

Uygulama birden çok ilkelerine sahip bir erişim paketine erişim isteğinde bulunan bir ilke seçin istenebilir. Örneğin, bir erişim Paket Yöneticisi erişim paket sahip iki ilke çalışanları iki grupları için yapılandırabilirsiniz. İlk 60 gün boyunca erişime izin vermek ve onayı iste. İkinci ilkeyi erişime izin vermek için 2 gün ve onay gerekli değil. Bu senaryo karşılaşırsanız, kullanmak istediğiniz ilke seçmeniz gerekir.

**Önkoşul rolü:** İstek sahibi

## <a name="next-steps"></a>Sonraki adımlar

- [Onaylayın veya reddedin erişim istekleri](entitlement-management-request-approve.md)
- [İstek işlemini ve e-posta bildirimleri](entitlement-management-process.md)
