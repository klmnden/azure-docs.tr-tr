---
title: "Azure Active Directory konumlarında adlı | Microsoft Docs"
description: "Ne adlı bilgi konumları olan ve bunların nasıl yapılandırılacağı."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 126646d7460831f0235221595b8a93c88be6146d
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Azure Active Directory'de adlandırılmış konumları

Adlandırılmış konumlarla kuruluşunuzdaki güvenilen IP adres aralıklarını etiketleyebilirsiniz. Azure Active Directory bağlamında adlandırılmış konumlara kullanır:

- Algılanması [risk olayları](active-directory-reporting-risk-events.md) bildirilen hatalı pozitif uyarıların sayısını azaltmak için.  

- [Konum temelli koşullu erişim](active-directory-conditional-access-azure-portal.md#locations).


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

    c. **Oluştur**'a tıklayın.



## <a name="what-you-should-know"></a>Bilmeniz gerekenler

**Toplu güncelleştirmeler**: oluşturduğunuzda veya toplu güncelleştirmeler için adlandırılmış konumlarını güncelleştirin karşıya yükleme veya IP aralıklarını içeren bir CSV dosyası indirme. Karşıya yükleme IP aralıklarını dosyasında listenin üzerine yerine listesine ekler.

![Karşıya yükleme ve indirme bağlantıları](./media/active-directory-named-locations/09.png)


**Sınırlamalar**: en fazla 60 adlandırılmış konumları, her birine atanan bir IP aralığı ile tanımlayabilirsiniz. Yapılandırılmış tek bir adlandırılmış konumunuz varsa, en fazla 500 IP aralıkları için tanımlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için:

- **Risk olayları**, bkz: [Azure Active Directory risk olaylarını](active-directory-reporting-risk-events.md).

- **Koşullu erişim**, bkz: [koşullu erişim Azure Active Directory'de](active-directory-conditional-access-azure-portal.md).

- **Riskli oturum açma işlemleri raporları**, bkz: [riskli oturum açma işlemleri raporu Azure Active Directory portalında](active-directory-reporting-security-risky-sign-ins.md).  
