---
title: PIM - Azure'ı kullanmaya başlayın | Microsoft Docs
description: Azure portalında Azure AD Privileged Identity Management (PIM) kullanmaya başlama hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5b3bff27821964648713b02589c941c99e3eb03d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190098"
---
# <a name="start-using-pim"></a>PIM kullanmaya başlayın

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) ile yönetebilir, denetleyebilir ve kuruluşunuzda erişim izleyin. Azure kaynaklarına, Azure AD’ye ve Office 365 ya da Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişim de bu kapsama dahildir.

Bu makalede Azure AD PIM uygulamasını Azure portalı panonuza nasıl ekleyeceğiniz anlatılmaktadır.

## <a name="first-person-to-use-pim"></a>İlk kişinin PIM'i kullanma

PIM'i kullanmak dizininizde ilk kişi siz, otomatik olarak size atanır [Güvenlik Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) ve [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) dizindeki roller. Kullanıcıların Azure AD dizini rol atamaları, yalnızca ayrıcalıklı rol yöneticileri tarafından yönetilebilir. Ayrıca [güvenlik sihirbazını](pim-security-wizard.md) çalıştırmayı da seçebilirsiniz. Bu sihirbaz ilk keşif ve atama deneyiminde size yol gösterir.

## <a name="add-pim-tile-to-the-dashboard"></a>PIM kutucuğu panoya ekleme

PIM açma daha kolay hale getirmek için PIM kutucuğu, Azure portalı panonuza eklemeniz gerekir.

1. Oturum [Azure portalında](https://portal.azure.com/) dizininizin genel Yöneticisi olarak.

1. Tıklayın **tüm hizmetleri** ve bulma **Azure AD Privileged Identity Management** hizmeti.

    ![Azure AD Privileged Identity Management içindeki tüm hizmetleri](./media/pim-getting-started/pim-all-services-find.png)

1. PIM Hızlı Başlangıç'ı açmak için tıklayın.

1. Denetleme **dikey pencereyi panoya Sabitle** PIM hızlı başlangıç dikey pencereyi panoya sabitleyin.

    ![Dikey pencereyi panoya sabitle](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure panosunda, böyle bir kutucuk görürsünüz:

    ![PIM hızlı kutucuğu](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

PIM ayarladıktan sonra Kimlik Yönetimi görevlerinizi gerçekleştirmek için bu dikey pencereyi kullanabilirsiniz.

![PIM için üst düzey görevler - ekran görüntüsü](./media/pim-getting-started/pim-quickstart-tasks.png)

| Görev + yönetme | Açıklama |
| --- | --- |
| **Rollerim**  | Size atanan uygun ve etkin rollerin bir listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz. |
| **İsteklerim** | Uygun rol atamalarını etkinleştirmek için Bekleyen isteklerinizi görüntüler. |
| **Uygulama erişimi** | Olası gecikmelerini azaltmak ve hemen etkinleştirildikten sonra rol kullanmanıza olanak sağlar. |
| **İstekleri onaylama** | Uygun rolleri dizininizdeki kullanıcılar tarafından etkinleştirme isteklerini listesini görüntüler onaylanacak atanmış. |
| **Erişim gözden geçirin** | Erişimi kendiniz veya başka bir kullanıcı için inceliyor olmanızdan bağımsız tamamlamak için atanan beklemedeki erişim gözden geçirmelerini listeler. |
| **Azure AD Dizin rolleri** | Bir Pano ve Azure AD dizini rol atamalarını yönetmek ayrıcalıklı rol yöneticileri ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |
| **Azure kaynakları** | Bir Pano ve Azure kaynak rol atamalarını yönetmek ayrıcalıklı rol yöneticileri ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD dizin rollerim PIM etkinleştir](pim-how-to-activate-role.md)
- [Azure kaynağı rollerim PIM etkinleştir](pim-resource-roles-activate-your-roles.md)
