---
title: Azure Active Directory’de adlandırılmış konumları yapılandırma | Microsoft Docs
description: Adlandırılmış konumların nasıl yapılandırılacağını öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: d12991813f68a42f9846c1c9c9c31c01d371d1d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107648"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'de adlandırılmış konumları yapılandırma

Adlandırılmış konumlar ile, kuruluşunuzda güvenilir IP adresi aralıklarını etiketleyebilirsiniz. Azure AD, aşağıdakileri yapmak için adlandırılmış konumları kullanır:
- [Risk olaylarında](concept-risk-events.md) hatalı pozitifleri algılama. Güvenilir bir konumdan oturum açılması, kullanıcının oturum açma riskini azaltır.   
- Yapılandırma [konum tabanlı koşullu erişim](../conditional-access/location-condition.md).

Bu hızlı başlangıçta, ortamınızda adlandırılmış konumları nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* Azure AD kiracısı. [Ücretsiz deneme](https://azure.microsoft.com/trial/get-started-active-directory/) için kaydolun. 
* Kiracı için genel yönetici olan bir kullanıcı.
* Kuruluşunuzda belirlenen ve güvenilir olan bir IP aralığı. IP aralığının **Sınıfsız Etki Alanları Arası Yönlendirme (CIDR)** biçiminde olması gerekir.

## <a name="configure-named-locations"></a>Adlandırılmış konumları yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede seçin **Azure Active Directory**, ardından **koşullu erişim** gelen **güvenlik** bölümü.

    ![Koşullu erişim sekmesi](./media/quickstart-configure-named-locations/entrypoint.png)

3. **Koşullu Erişim** sayfasında **Adlandırılmış konumlar**’ı ve **Yeni konum**’u seçin.

    ![Adlandırılmış konum](./media/quickstart-configure-named-locations/namedlocation.png)

6. Yeni sayfada formu doldurun. 

   * **Ad** kutusuna, adlandırılmış konumunuz için bir ad girin.
   * **IP aralıkları** kutusuna CIDR biçiminde IP aralığını girin.  
   * **Oluştur**’a tıklayın.
    
     ![Yeni dikey pencere](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

- [Azure AD koşullu erişim](../active-directory-conditional-access-azure-portal.md).
- [Konum koşulları Azure AD koşullu erişim](../conditional-access/location-condition.md)
- [Riskli oturum açma işlemleri raporu](concept-risky-sign-ins.md).  
