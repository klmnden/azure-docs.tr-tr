---
title: Office 365 grupları - Azure Active Directory için adlandırma ilkesi hızlı Ekle | Microsoft Docs
description: Azure Active Directory'de yeni kullanıcıların eklenmesini var olan kullanıcıların silinmesini açıklar
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 04/24/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b17ef24d753041934f68f3daee950aaa0bec46ba
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734727"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory'de gruplar için adlandırma ilkesi

Bu hızlı başlangıçta kiracınızın gruplarını sıralama ve arama konusunda yardımcı olması amacıyla Azure Active Directory (Azure AD) kiracınızda kullanıcı tarafından oluşturulan Office 365 grupları için adlandırma ilkesini ayarlayacaksınız. Adlandırma ilkesini aşağıdaki gibi amaçlar için kullanabilirsiniz:

* Grubun işlevini, üyelerini, coğrafi bölgesini veya grubu oluşturan kişiyi paylaşma.
* Adres defterindeki grupların kategorilere ayrılmasına yardımcı olma.
* Belirli sözcüklerin grup adlarında ve diğer adlarında kullanılmasını engelleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-portal"></a>Azure portalını kullanarak bir kiracı için adlandırma ilkesinin grubu yapılandırma

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanıcı yönetici hesabıyla.
1. Seçin **grupları**, ardından **adlandırma ilkesinin** adlandırma ilkesi sayfasını açın.

    ![Yönetim merkezinde adlandırma ilkesi sayfasını açın](./media/groups-naming-policy/policy.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Görüntülemek veya önek sonek adlandırma ilkesini Düzenle

1. Üzerinde **adlandırma ilkesinin** sayfasında **Grup adlandırma ilkesi**.
1. Görüntüleyebilir veya geçerli bir önek veya sonek ilkeleri tek tek öznitelikler veya adlandırma ilkesinin bir parçası uygulamak istediğiniz dizeleri seçerek adlandırma düzenleyin.
1. Bir önek veya sonek listeden kaldırmak için önek veya sonek seçin ve sonra seçin **Sil**. Aynı anda birden çok öğe silinebilir.
1. Seçin **Kaydet** değişikliklerinizi yürürlüğe ilkesi için.

### <a name="view-or-edit-the-custom-blocked-words"></a>Görüntüleyin ya da özel engellenen sözcük düzenleyin

1. Üzerinde **adlandırma ilkesinin** sayfasında **engellenen sözcük**.

    ![Düzenle ve adlandırma ilkesi için engellenen sözcük listesi karşıya yükleyin](./media/groups-naming-policy/blockedwords.png)

1. Görüntülemek veya seçerek özel engellenen sözcük geçerli listesini düzenleyin **indirme**.
1. Yeni özel engellenen sözcüklerin listesi dosyası simgesini seçerek karşıya yükleyin.
1. Seçin **Kaydet** değişikliklerinizi yürürlüğe ilkesi için.

İşte bu kadar. Adlandırma ilkenizi ayarladınız ve özel engellenen sözcüklerinizi eklediniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="remove-the-naming-policy-using-azure-portal"></a>Azure portalını kullanarak bir adlandırma ilkesini Kaldır

1. Üzerinde **adlandırma ilkesinin** sayfasında **silme ilkesi**.
1. Silme işlemini onayladıktan sonra adlandırma ilkesi, tüm ön eki soneki dahil olmak üzere kaldırılır adlandırma ilkesi ve özel engellenen sözcük.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalından Azure AD kuruluşunuz için adlandırma ilkesi ayarlama öğrendiniz.

Adlandırma ilkesi için PowerShell cmdlet'leri listesini özel engellenen sözcükler ve son kullanıcı deneyimi Office 365 uygulamalarını ekleme, teknik kısıtlamalar dahil olmak üzere daha fazla bilgi için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Adlandırma ilkesinin PowerShell](groups-naming-policy.md)