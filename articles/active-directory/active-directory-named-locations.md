---
title: "Azure Active Directory konumlarında adlı | Microsoft Docs"
description: "Konumları adlı yapılandırarak, kuruluşunuz tarafından sahip olunan IP adreslerini oluşturmak sahip önleyebilirsiniz Impossible için hatalı pozitif sonuç seyahat alışılmadık konumlara risk olayı türü."
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
ms.date: 09/20/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: da437908509e40386ed23863648bd6956b308186
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Azure Active Directory'de adlandırılmış konumları

Azure Active Directory adlandırılmış konumlara özelliğiyle, kuruluşların güvenilir IP adres aralıklarını etiketleyebilirsiniz. Ortamınızda, adlandırılmış konumlarını algılanması bağlamında kullanabilirsiniz [risk olayları](active-directory-reporting-risk-events.md). Bu özellik için bildirilen hatalı pozitif uyarıların sayısını azaltır *Impossible seyahat alışılmadık konumlara* risk olayı türü. 

## <a name="configuration"></a>Yapılandırma

Adlandırılmış bir konumu yapılandırmak için:

1. Oturum [Azure portal](https://portal.azure.com) genel yönetici olarak.

2. Sol bölmede **Azure Active Directory**.

    ![Sol bölmede Azure Active Directory bağlantısı](./media/active-directory-named-locations/01.png)

3. Üzerinde **Azure Active Directory** dikey penceresindeki **güvenlik** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim komutu](./media/active-directory-named-locations/05.png)


4. Üzerinde **koşullu erişim** dikey penceresindeki **Yönet** 'yi tıklatın **konumları adlı**.

    ![Adlandırılmış konumları komutu](./media/active-directory-named-locations/06.png)


5. Üzerinde **konumları adlı** dikey penceresinde tıklatın **yeni konum**.

    ![Yeni konum komutu](./media/active-directory-named-locations/07.png)


6. Üzerinde **yeni** dikey penceresinde aşağıdakileri yapın:

    ![Yeni bir dikey pencere](./media/active-directory-named-locations/56.png)

    a. İçinde **adı** adlandırılmış konumunuz için bir ad yazın.

    b. İçinde **IP aralıkları** bir IP aralığı yazın. IP aralığı içinde olması gereken *sınıfsız etki alanları arası yönlendirme* (CIDR) biçimi.  

    c. **Oluştur**'a tıklayın.



## <a name="what-you-should-know"></a>Bilmeniz gerekenler

**Toplu güncelleştirmeler**: oluşturduğunuzda veya toplu güncelleştirmeler için adlandırılmış konumlarını güncelleştirin karşıya yükleme veya IP aralıklarını içeren bir CSV dosyası indirme. Karşıya yükleme IP aralıklarını dosyasında listenin üzerine yerine listesine ekler.

![Karşıya yükleme ve indirme bağlantıları](./media/active-directory-named-locations/09.png)


**Sınırlamalar**: en fazla 60 adlandırılmış konumları, her birine atanan bir IP aralığı ile tanımlayabilirsiniz. Yapılandırılmış tek bir adlandırılmış konumunuz varsa, en fazla 500 IP aralıkları için tanımlayabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Risk olaylar hakkında daha fazla bilgi için bkz: [Azure Active Directory risk olaylarını](active-directory-reporting-risk-events.md).

