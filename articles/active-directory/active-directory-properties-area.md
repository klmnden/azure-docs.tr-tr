---
title: Azure AD içinde kuruluşunuzun gizlilik bilgisi ekleyin | Microsoft Docs
description: Kuruluşunuzun gizlilik bilgileri Azure Active Directory (Azure AD) özelliklerini alanına eklemek açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: 8cdf30ed09601a31529073eaedd4ab53780157d5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077553"
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Nasıl yapılır: Azure Active Directory'de, kuruluşunuzun gizlilik bilgisi Ekle
Bu makalede, bir kiracı Yöneticisi bir kuruluşun Azure Portalı aracılığıyla Azure Active Directory (Azure AD) Kiracı için gizlilikle ilgili bilgileri nasıl ekleyebileceğinizi açıklar.

İç çalışanlar ve dış konuklar ilkelerinizi gözden geçirebilmeniz için genel gizlilik kişiniz ve kuruluşunuzun gizlilik bildirimini ekleyin öneririz. Gizlilik bildirimlerini benzersiz olarak oluşturulur ve her iş için özel olarak hazırlanmış olduğundan, Yardım için bir avukat başvurun kesinlikle öneririz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Gizlilik bilgilerinize eklemek için özellikler erişebilir

1.  Azure portalında bir kiracı yönetici olarak oturum açın.

2.  Sol gezinti çubuğu üzerinde seçin **Azure Active Directory**ve ardından **özellikleri**.

    **Özellikleri** alanı görüntülenir.

    ![Azure AD özellikleri alanı gizlilik bilgileri alan vurgulama](./media/active-directory-properties-area/properties-area.png)

3.  Çalışanlarınız için gizlilik bilgilerinizi ekleyin:

    - **Teknik başvurun.** Kuruluşunuzdaki teknik destek için iletişim kurulacak kişinin e-posta adresini yazın.
    
    - **Genel gizlilik başvurun.** Kişisel veri gizliliği hakkında sorgular için bağlantı kurulacak kişinin e-posta adresini yazın. Bu kişi bir veri ihlali ise kimin Microsoft ile iletişim olduğunu. Burada listelenen hiçbir kişi varsa, genel yöneticilerinize Microsoft ile iletişim kurar.

    - **Gizlilik bildirimi URL'si.** Kuruluşunuz her ikisi de nasıl işler açıklayan kuruluşunuzun belge bağlantısını yazın iç ve dış konuğun veri gizliliği.

        >[!Important]
        >Kendi gizlilik bildirimi veya gizlilik kişiniz eklemezseniz, dış konuklar metinde görürsünüz **Gözden Geçiren izinleri** şunu kutusunu  **< _kuruluş adınızı_> bağlantıları için gözden geçirmek kendi koşulları sağlamamış**. Örneğin, bir kuruluşun B2B işbirliği erişmek için bir davet aldıklarında Konuk kullanıcı bu iletiyi görürsünüz.

        ![İleti kutusu B2B işbirliği Gözden Geçiren izinleri](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory B2B işbirliği davet kullanım](https://aka.ms/b2bredemption)
- [Azure Active Directory'de bir kullanıcı için profil bilgileri ekleme veya değiştirme](/active-directory-users-profile-azure-portal.md)