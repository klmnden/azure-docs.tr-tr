---
title: Ekleme veya bir grup Azure Active Directory'de başka bir gruptan Kaldır | Microsoft Docs
description: Gruba eklemek veya bir Azure Active Directory'yi kullanarak başka bir gruptan kaldırmak öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: c28fe5ef226fac993fde221b16bfa875ba4845ca
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579777"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Nasıl yapılır: ekleme veya bir grup Azure Active Directory'yi kullanarak başka bir gruptan kaldırma
Bu makalede, bir grup Azure Active Directory'yi kullanarak başka bir gruptan ekleyip yardımcı olur.

>[!Note]
>Üst grubun silmeye çalışıyorsanız, bkz [güncelleştirmek veya bir grup ve üyelerini silmek nasıl](active-directory-groups-delete-group.md).

## <a name="add-a-group-as-a-member-to-another-group"></a>Bir grubu başka bir gruba üye olarak ekleyin.
Varolan bir grup üyesi grubu (alt) ve bir üst grubu oluşturma başka bir mevcut gruba ekleyebilirsiniz. Grubu üyeleri, öznitelikleri ve yapılandırma zamandan tasarruf üst grubunun özelliklerini devralır.

### <a name="to-add-a-group-as-a-member-to-another-group"></a>Bir grubu başka bir gruba üye olarak eklemek için

1. Oturum [Azure portalında](https://portal.azure.com) dizinde genel yönetici hesabını kullanarak.

2. Seçin **Azure Active Directory**ve ardından **grupları**.

3. Üzerinde **gruplar - tüm gruplar** sayfasında arayın ve başka bir grubun üyesi olmak için grubu seçin. Bu alıştırma için kullandığımız **MDM İlkesi - Batı** grubu.

    >[!Note]
    >Grubunuzun bir üyesi olarak aynı anda yalnızca bir gruba ekleyebilirsiniz. Ayrıca, **grup** kutusu görünen giriş için bir kullanıcı veya cihaz adı herhangi bir bölümünü eşleşmesi temeline göre filtreler. Ancak, joker karakterler desteklenmez.

    ![Seçili grupları - MDM ilkesiyle tüm grupları sayfası - Batı grubu](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. Üzerinde **MDM İlkesi - Batı - grup üyeliklerini** sayfasında **grup üyeliklerini**seçin **Ekle**, grubunuzun bir üyesi olmanız ve ardından istediğinizgrububulun **Seçin**. Bu alıştırma için kullandığımız **MDM İlkesi - tüm kuruluş** grubu.

    **MDM İlkesi - Batı** grup, artık üye **MDM İlkesi - tüm kuruluş** grubu, tüm özellikleri ve MDM İlkesi - tüm kuruluş grubu yapılandırmasını devralıyor.

    ![Başka bir gruba grubu ekleyerek bir grup üyeliği oluştur](media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. Gözden geçirme **MDM İlkesi - Batı - grup üyeliklerini** grup ve üye ilişkiyi görmek için sayfayı.

    ![Üst grubun gösteren MDM İlkesi - Batı - grup üyeliklerini sayfası](media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Grup ve üye ilişkinin daha ayrıntılı bir görünüm için Grup adını seçin (**MDM İlkesi - tüm kuruluş**) ve göz atın **MDM İlkesi - Batı** Ayrıntıları sayfası.

    ![Grup üyeliği sayfası hem üye ve Grup ayrıntılarını gösterme](media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>Üye grubu başka bir gruptan Kaldır
Varolan bir üye grubu başka bir gruptan kaldırabilirsiniz. Ancak, üyelik kaldırılması ayrıca tüm devralınan öznitelikleri ve kullanıcılarınız için özellikleri kaldırır.

### <a name="to-remove-a-member-group-from-another-group"></a>Üye grubu başka bir gruptan kaldırmak için
1. Üzerinde **gruplar - tüm gruplar** sayfasında arayın ve başka bir grubun üyesi olarak kaldırılacak grubunu seçin. Bu alıştırma için yeniden kullanıyoruz **MDM İlkesi - Batı** grubu.

2. Üzerinde **MDM İlkesi - Batı genel bakış** sayfasında **grup üyeliklerini**.

    ![MDM İlkesi - Batı genel bakış sayfası](media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. Seçin **MDM İlkesi - tüm kuruluş** gelen grup **MDM İlkesi - Batı - grup üyeliklerini** sayfasında ve ardından **Kaldır** gelen **MDM İlkesi - Batı** Ayrıntıları sayfası.

    ![Grup üyeliği sayfası hem üye ve Grup ayrıntılarını gösterme](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>Ek bilgiler
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

- [Gruplar ve üyeler görüntüleyin](active-directory-groups-view-azure-portal.md)

- [Temel bir grup oluşturma ve üye ekleme](active-directory-groups-create-azure-portal.md)

- [Üye ekleme veya gruptan kaldırma](active-directory-groups-members-azure-portal.md)

- [Grup ayarlarınızı düzenleyin](active-directory-groups-settings-azure-portal.md)

- [Gruba göre kullanıcılar için lisans atama](../users-groups-roles/licensing-groups-assign.md)
