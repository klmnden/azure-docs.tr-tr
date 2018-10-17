---
title: Azure Active Directory’de adlandırılmış konumları yapılandırma | Microsoft Docs
description: Adlandırılmış konumların nasıl yapılandırılacağını öğrenin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/23/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d99f077449529fb4a4a7f124fe1c0263d4113dee
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363454"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory’de adlandırılmış konumları yapılandırma

Adlandırılmış konumlar ile, kuruluşunuzda güvenilir IP adresi aralıklarını etiketleyebilirsiniz. Azure AD, aşağıdakileri yapmak için adlandırılmış konumları kullanır:
- [Risk olaylarında](concept-risk-events.md) hatalı pozitifleri algılama. Güvenilir bir konumdan oturum açılması, kullanıcının oturum açma riskini azaltır.   
- [Konum tabanlı koşullu erişimi](../conditional-access/location-condition.md) yapılandırma.

Bu hızlı başlangıçta, ortamınızda adlandırılmış konumları nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

* Azure AD kiracısı. [Ücretsiz deneme](https://azure.microsoft.com/trial/get-started-active-directory/) için kaydolun. 
* Kiracı için genel yönetici olan bir kullanıcı.
* Kuruluşunuzda belirlenen ve güvenilir olan bir IP aralığı. IP aralığının **Sınıfsız Etki Alanları Arası Yönlendirme (CIDR)** biçiminde olması gerekir.

## <a name="configure-named-locations"></a>Adlandırılmış konumları yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede, **Azure Active Directory** seçeneğini belirleyin ve sonra **Güvenlik** bölümünden **Koşullu erişim**’i seçin.

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

- [Azure Active Directory’de koşullu erişim](../active-directory-conditional-access-azure-portal.md).

- [Azure AD koşullu erişiminde konum koşulları](../conditional-access/location-condition.md)

- [Azure Active Directory portalındaki riskli oturum açma işlemleri raporu](concept-risky-sign-ins.md).  
