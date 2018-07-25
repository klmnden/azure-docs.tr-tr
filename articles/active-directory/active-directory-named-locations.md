---
title: Azure Active Directory'de adlandırılmış konumları yapılandırma | Microsoft Docs
description: Adlandırılmış konumları yapılandırma konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.component: protection
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f739917b201d5255716d22930d7c4bd9e6602f37
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224526"
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Azure Active Directory'de adlandırılmış konumları yapılandırma

Adlandırılmış konumlar ile kuruluşunuzda güvenilen IP adresi aralıkları etiketleyebilirsiniz. Azure Active Directory bağlamında adlandırılmış konumlar kullanır:

- Algılanması [risk olayları](active-directory-reporting-risk-events.md) bildirilen hatalı pozitif sonuç sayısını azaltmak için.  

- [Konum tabanlı koşullu erişim](active-directory-conditional-access-locations.md).


Nasıl yapılandırılacağını açıklar. Bu makalede, ortamınızda konumlarına adı.


## <a name="entry-points"></a>Giriş noktaları

Belirtilen konum yapılandırma sayfasında erişebileceğiniz **güvenlik** tıklayarak Azure Active Directory sayfasının bölümünde:

![Giriş noktaları](./media/active-directory-named-locations/34.png)

- **Koşullu erişim:**

    - İçinde **Yönet** bölümünde **adlandırılmış Konumlar**.
    
        ![Adlandırılmış konumlar komutu](./media/active-directory-named-locations/06.png)

- **Riskli oturum açma işlemleri:**

    - Üst araç çubuğunda tıklatın **bilinen IP adresi aralıklarını Ekle**.

       ![Adlandırılmış konumlar komutu](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Yapılandırma örneği

**Adlandırılmış bir konuma yapılandırmak için:**

1. Oturum [Azure portalında](https://portal.azure.com) genel yönetici olarak.

2. Sol bölmede **Azure Active Directory**.

    ![Sol bölmede Azure Active Directory bağlantısı](./media/active-directory-named-locations/01.png)

3. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

    ![Koşullu erişim komutu](./media/active-directory-named-locations/05.png)


4. Üzerinde **koşullu erişim** sayfasında **Yönet** bölümünde **adlandırılmış Konumlar**.

    ![Adlandırılmış konumlar komutu](./media/active-directory-named-locations/06.png)


5. Üzerinde **adlandırılmış Konumlar** sayfasında **yeni konuma**.

    ![Yeni konum komutu](./media/active-directory-named-locations/07.png)


6. Üzerinde **yeni** sayfasında, aşağıdakileri yapın:

    ![Yeni bir dikey pencere](./media/active-directory-named-locations/61.png)

    a. İçinde **adı** adlandırılmış konumunuz için bir ad yazın.

    b. İçinde **IP aralıklarını** bir IP aralığı yazın. IP aralığı içinde olması gereken *sınıfsız etki alanları arası yönlendirme* biçimini (CIDR).  

    c. **Oluştur**’a tıklayın.



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

- [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).

- [Konum koşulları Azure Active Directory koşullu erişim](active-directory-conditional-access-locations.md)

- [Azure Active Directory risk olayları](active-directory-reporting-risk-events.md).

- [Azure Active Directory portalındaki riskli oturum açma işlemleri raporu](active-directory-reporting-security-risky-sign-ins.md).  
