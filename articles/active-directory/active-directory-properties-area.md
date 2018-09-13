---
title: Azure AD'de, kuruluşunuzun gizlilik bilgileri ekleyin | Microsoft Docs
description: Kuruluşunuzun gizlilik bilgileri için Azure Active Directory (Azure AD) özellikleri alanı ekleme işlemi açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: a34fa2b8c2d966af108664c219a222fb9a5b7abc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35972069"
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgileri ekleme
Bu makalede, bir kiracı Yöneticisi Azure portalı üzerinden bir kuruluşun Azure Active Directory (Azure AD) kiracısına gizlilikle ilgili bilgileri nasıl ekleyebileceğinizi açıklar.

İç çalışanlarınızın ve dış Konukları ilkelerinizi gözden geçirebilmeniz için genel gizlilik ilgili kişisi hem kuruluşunuzun gizlilik bildirimini ekleyin kesinlikle öneririz. Gizlilik bildirimlerini benzersiz olarak oluşturulur ve her işletme için hazırlanmış olduğundan bir avukat Yardım almak için sizinle kesinlikle öneririz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Erişim özellikleri alan gizlilik bilgilerinizi eklemek için

1.  Kiracı Yöneticisi olarak Azure portalında oturum açın.

2.  Sol gezinti seçin **Azure Active Directory**ve ardından **özellikleri**.

    **Özellikleri** alanında görünür.

    ![Azure AD özellikleri alanı gizlilik bilgileri alanı vurgulama](./media/active-directory-properties-area/properties-area.png)

3.  Çalışanlarınız için gizlilik bilgilerinizi ekleyin:

    - **Teknik konular ilgili kişisi** Kuruluşunuzdaki teknik destek için iletişim kurulacak kişinin e-posta adresini yazın.
    
    - **Genel gizlilik ilgili kişisi.** Kişinin kişisel veri gizliliği ile ilgili sorguları kişiye e-posta adresini yazın. Bu kişi ayrıca bir veri ihlali ise Microsoft ile iletişim kim. Burada listelenen herhangi bir kişi varsa, genel Yöneticiler Microsoft ile iletişim kurar.

    - **Gizlilik bildirimi URL'si.** Kuruluşunuzun her ikisi de nasıl işlediğini açıklar, kuruluşunuzun belge bağlantı türü iç ve dış konuğun veri gizliliği.

        >[!Important]
        >Kendi gizlilik bildirimi ya da kendi gizlilik ilgili kişisi dahil etmezseniz, dış konuklarınız metnini görmek **Gözden Geçiren izinleri** ifadesini kutusu  **< _kuruluş adınız_> incelemeniz için koşullarına bir bağlantı sağlamadı**. Örneğin, bir kuruluşun B2B işbirliği erişmek için davet aldıklarında Konuk kullanıcı bu iletiyi görür.

        ![B2B işbirliği gözden geçirme izinler kutusunda iletisi](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory B2B işbirliği Davetiyesi kullanımı](https://aka.ms/b2bredemption)
- [Azure Active Directory'de bir kullanıcı profili bilgileri ekleme veya değiştirme](fundamentals/active-directory-users-profile-azure-portal.md)