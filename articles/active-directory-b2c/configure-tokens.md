---
title: Belirteçleri - Azure Active Directory B2C'yi yapılandırma | Microsoft Docs
description: Azure Active Directory B2C belirteç ömrü ve uyumluluk ayarlarını yapılandırmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e1163c88a100ebb7500607475ab5740557904137
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511324"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de belirteçleri yapılandırma

Bu makalede, nasıl yapılandırılacağını öğrenmek [yaşam süresi ve uyumluluk belirteci](active-directory-b2c-reference-tokens.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="prerequisites"></a>Önkoşullar

[Kullanıcı akışı Oluştur](tutorial-create-user-flows.md) etkinleştirme kullanıcıların kaydolmak ve uygulamanız için oturum açın.

## <a name="configure-token-lifetime"></a>Belirteç ömrü yapılandırma

Herhangi bir kullanıcı Akış belirteç süresini yapılandırabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizine kullandığınızdan emin olun. Seçin **dizin ve abonelik filtresi** üst menüdeki ve Azure AD B2C kiracınızı içeren dizini seçin.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **kullanıcı akışları (ilke)** .
5. Daha önce oluşturduğunuz kullanıcı akışı açın. 
6. Seçin **özellikleri**.
7. Altında **belirteç ömrü**, uygulamanızın ihtiyaçlarına uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Belirteç ömrü yapılandırma](./media/configure-tokens/token-lifetime.png)

8. **Kaydet**’e tıklayın.

## <a name="configure-token-compatibility"></a>Belirteç uyumluluk yapılandırın

1. Seçin **kullanıcı akışları (ilke)** .
2. Daha önce oluşturduğunuz kullanıcı akışı açın. 
3. Seçin **özellikleri**.
4. Altında **belirteç uyumluluk ayarları**, uygulamanızın ihtiyaçlarına uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Belirteç uyumluluk yapılandırın](./media/configure-tokens/token-compatibility.png)

5. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [erişim belirteçleri kullanma](active-directory-b2c-access-tokens.md).



