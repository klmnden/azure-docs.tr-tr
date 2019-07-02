---
title: Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçişi
description: Bu makalede, Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçirme gösterilmektedir.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: tutorial
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4819c283a136057ad7c3ffd755fd9e157d99a1bf
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509460"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Azure portalında çok faktörlü kimlik doğrulaması gerektiren bir Klasik ilke geçişi

Bu öğreticide gerektiren bir Klasik ilke geçirme gösterilmektedir **çok faktörlü kimlik doğrulaması** bulut uygulaması için. Bir önkoşul olmamasına karşın, okumanızı öneririz [Azure portalında Klasik ilkeleri geçirme](policy-migration.md) , Klasik ilkeleri geçirme işlemine başlamadan önce.

## <a name="overview"></a>Genel Bakış

Bu makaledeki senaryoda gerektiren bir Klasik ilke geçirme gösterilmektedir **çok faktörlü kimlik doğrulaması** bulut uygulaması için.

![Azure Active Directory](./media/policy-migration/33.png)

Geçiş işlemi aşağıdaki adımlardan oluşur:

1. [Klasik ilke açın](#open-a-classic-policy) yapılandırma ayarlarını almak için.
1. Klasik ilkeniz değiştirmek için yeni bir Azure AD koşullu erişim ilkesi oluşturun. 
1. Klasik ilke devre dışı bırakın.

## <a name="open-a-classic-policy"></a>Klasik ilke açın

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti tıklatın **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration-mfa/01.png)

1. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

   ![Koşullu Erişim](./media/policy-migration-mfa/02.png)

1. İçinde **Yönet** bölümünde **Klasik ilkeleri (Önizleme)** .

   ![Klasik ilkeler](./media/policy-migration-mfa/12.png)

1. Klasik ilkeleri listesinde gerektiren ilkeye tıklayın **çok faktörlü kimlik doğrulaması** bulut uygulaması için.

   ![Klasik ilkeler](./media/policy-migration-mfa/13.png)

## <a name="create-a-new-conditional-access-policy"></a>Yeni bir koşullu erişim ilkesi oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), sol gezinti tıklatın **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration/01.png)

1. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

   ![Koşullu Erişim](./media/policy-migration/02.png)

1. Üzerinde **koşullu erişim** sayfasında açmak için **yeni** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

   ![Koşullu Erişim](./media/policy-migration/03.png)

1. Üzerinde **yeni** sayfasında **adı** metin ilkeniz için bir ad yazın.

   ![Koşullu Erişim](./media/policy-migration/29.png)

1. İçinde **atamaları** bölümünde **kullanıcılar ve gruplar**.

   ![Koşullu Erişim](./media/policy-migration/05.png)

   1. Tüm kullanıcıları seçtiyseniz Klasik ilkeniz varsa tıklayın **tüm kullanıcılar**. 

      ![Koşullu Erişim](./media/policy-migration/35.png)

   1. Klasik ilkeniz seçili grupları varsa, tıklayın **kullanıcıları ve grupları seçin**ve ardından gerekli kullanıcılar ve Gruplar'ı seçin.

      ![Koşullu Erişim](./media/policy-migration/36.png)

   1. Dışlanan grupları varsa, tıklayın **hariç** sekmesini ve sonra gerekli kullanıcıları ve grupları seçin. 

      ![Koşullu Erişim](./media/policy-migration/37.png)

1. Üzerinde **yeni** sayfasında açmak için **bulut uygulamaları** sayfasında **atama** bölümünde **bulut uygulamaları**.
1. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:
   1. Tıklayın **uygulamaları Seç**.
   1. **Seç**'e tıklayın.
   1. Üzerinde **seçin** sayfasında bulut uygulamanızı seçin ve ardından **seçin**.
   1. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.
1. Varsa **çok faktörlü kimlik doğrulaması gerektiren** seçili:

   ![Koşullu Erişim](./media/policy-migration/26.png)

   1. İçinde **erişim denetimleri** bölümünde **Grant**.

      ![Koşullu Erişim](./media/policy-migration/27.png)

   1. Üzerinde **Grant** sayfasında **erişim ver**ve ardından **çok faktörlü kimlik doğrulaması gerektiren**.
   1. **Seç**'e tıklayın.
1. Tıklayın **üzerinde** ilkenizi etkinleştirmek için.

   ![Koşullu Erişim](./media/policy-migration/30.png)

## <a name="disable-the-classic-policy"></a>Klasik ilke devre dışı bırak

Klasik ilkeniz devre dışı bırakmak için **devre dışı** içinde **ayrıntıları** görünümü.

![Klasik ilkeler](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Sonraki adımlar

- Klasik ilke geçişi hakkında daha fazla bilgi için bkz. [Azure portalında Klasik ilkeleri geçirme](policy-migration.md).
- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md).
