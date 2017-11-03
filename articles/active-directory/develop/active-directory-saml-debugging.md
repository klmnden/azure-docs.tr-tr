---
title: "SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama | Microsoft Docs"
description: "SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama öğrenin "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 246709effcff1c38d14db3848fe2fad836ad90da
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>SAML tabanlı çoklu oturum açma Azure Active Directory uygulamalarında hata ayıklama
SAML tabanlı uygulama tümleştirmesi hata ayıklarken, genellikle gibi bir araç kullanmanız yararlı olur [Fiddler](http://www.telerik.com/fiddler) SAML isteği SAML yanıtını ve uygulamaya verilen gerçek SAML belirteci görmek için. SAML belirteci inceleyerek, tüm gerekli öznitelikler, kullanıcı adı SAML konu ve URI veren beklenen şekilde geldiğini emin olabilirsiniz.

![][1]

SAML belirteci içeren Azure AD'den yanıt genellikle bir HTTP 302 yeniden yönlendirme sonra https://login.windows.net oluşur ve gönderilen biridir yapılandırılmış için **yanıt URL'si** uygulamanın. 

SAML belirteci bu satırı seçip, ardından seçerek görüntüleyebilirsiniz **denetçiler > WebForms** sağ panelde sekmesindedir. Buradan, sağ **SAMLResponse** değer ve seçin **TextWizard için Gönder**. Ardından **gelen Base64** gelen **dönüştürme** belirteç kodunu çözer ve içeriğini görmek için menüsü.

**Not**: Bu HTTP isteğinin içeriğini görmek için fiddler'ı, yapmanız gereken HTTPS trafiği şifrelerinin yapılandırmak için isteyebilir.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştirme](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png