---
title: "Azure Active Directory'de adlandırılmış konumlarını yapılandırma | Microsoft Docs"
description: "Adlandırılmış konumlara yapılandırmayı öğrenin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3b7bd6f4bea111815f647af09ebaa868696b25bc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="configure-named-locations-in-azure-active-directory"></a>Azure Active Directory'de adlandırılmış konumlarını yapılandır

Adlandırılmış konumlarla kuruluşunuzdaki güvenilen IP adres aralıklarını etiketleyebilirsiniz. Azure Active Directory bağlamında adlandırılmış konumlara kullanır:

- Algılanması [risk olayları](active-directory-reporting-risk-events.md) bildirilen hatalı pozitif uyarıların sayısını azaltmak için.  

- [Konum temelli koşullu erişim](active-directory-conditional-access-locations.md).


Bu makalede açıklanmaktadır, nasıl yapılandırabileceğiniz konumları, ortamınızdaki adlı.


## <a name="entry-points"></a>Giriş noktaları

Belirtilen konum yapılandırma sayfasında erişebilirsiniz **güvenlik** tıklatarak Azure Active Directory sayfasının bölümünde:

![Giriş noktaları](./media/active-directory-named-locations/34.png)

- **Koşullu erişim:**

    - İçinde **Yönet** 'yi tıklatın **konumları adlı**.
    
        ![Adlandırılmış konumları komutu](./media/active-directory-named-locations/06.png)

- **Riskli oturum açma işlemleri:**

    - Üstteki araç çubuğunda tıklatın **IP adres aralıklarını bilinen Ekle**.

       ![Adlandırılmış konumları komutu](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Yapılandırma örneği

**Adlandırılmış bir konumu yapılandırmak için:**

1. Oturum [Azure portal](https://portal.azure.com) genel yönetici olarak.

2. Sol bölmede **Azure Active Directory**.

    ![Sol bölmede Azure Active Directory bağlantısı](./media/active-directory-named-locations/01.png)

3. Üzerinde **Azure Active Directory** sayfasında **güvenlik** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim komutu](./media/active-directory-named-locations/05.png)


4. Üzerinde **koşullu erişim** sayfasında **Yönet** 'yi tıklatın **konumları adlı**.

    ![Adlandırılmış konumları komutu](./media/active-directory-named-locations/06.png)


5. Üzerinde **konumları adlı** sayfasında, **yeni konum**.

    ![Yeni konum komutu](./media/active-directory-named-locations/07.png)


6. Üzerinde **yeni** sayfasında, aşağıdakileri yapın:

    ![Yeni bir dikey pencere](./media/active-directory-named-locations/56.png)

    a. İçinde **adı** adlandırılmış konumunuz için bir ad yazın.

    b. İçinde **IP aralıkları** bir IP aralığı yazın. IP aralığı içinde olması gereken *sınıfsız etki alanları arası yönlendirme* (CIDR) biçimi.  

    c. **Oluştur**’a tıklayın.



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

- [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).

- [Azure Active Directory koşullu erişim konumu koşullar](active-directory-conditional-access-locations.md)

- [Azure Active Directory risk olaylarını](active-directory-reporting-risk-events.md).

- [Azure Active Directory portalında riskli oturum açma işlemleri raporu](active-directory-reporting-security-risky-sign-ins.md).  
