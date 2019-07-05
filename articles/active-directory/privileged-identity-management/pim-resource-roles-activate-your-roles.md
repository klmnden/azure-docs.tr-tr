---
title: Azure kaynağı rollerim PIM - Azure Active Directory etkinleştirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM), Azure kaynak rolleri etkinleştirmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdce060006f65f2b0fb08023066ee504578bc552
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67501652"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Azure kaynağı rollerim PIM etkinleştir

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullanarak Azure kaynakları için uygun rolü üyeleri etkinleştirme bir gelecek tarih ve saat için zamanlayabilirsiniz. Bunlar, belirli etkinleştirme süresi üst sınırı (yönetici tarafından yapılandırılır) içinde de seçebilirsiniz.

Bu makalede, Azure Kaynak rolü PIM etkinleştirmek için gereken üyeleri içindir.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi

Bir Azure Kaynak rolü gerektiğinde kullanarak etkinleştirme isteyebilir **rollerim** PIM gezinme seçeneği.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**. PIM kutucuk, panonuza ekleme hakkında daha fazla bilgi için bkz: [PIM kullanmaya başlamak](pim-getting-started.md).

1. Tıklayın **rollerim**.

    ![Rolleri gösteren rolleri sayfam etkinleştirebilir](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Tıklayın **Azure kaynağı rolleri** , uygun bir Azure kaynağı rolleri listesini görmek için.

   ![Rollerim - Azure kaynak rolleri sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. İçinde **Azure kaynağı rolleri** listesinde, etkinleştirmek istediğiniz rol bulur.

    ![Azure kaynağı rolleri - uygun roller listem](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Tıklayın **etkinleştirme** etkinleştirme bölmesini açmak için.

1. Rolünüz multi factor authentication (MFA) gerektiriyorsa, tıklayın **devam etmeden önce kimliğinizi Doğrulamamız**. Yalnızca bir kez oturum başına kimlik doğrulaması gerekir.

    ![Rol etkinleştirme önce MFA ile Kimliğimi doğrula](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Tıklayın **Kimliğimi doğrula** ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![Bir PIN kodu gibi güvenlik sağlamak için ekran](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Sınırlı kapsam belirtmek istiyorsanız, tıklayın **kapsam** kaynak filtre bölmesini açın.

    Yalnızca ihtiyacınız olan kaynakları için erişim istemek için iyi bir uygulamadır. Kaynak Filtresi bölmesinde erişmesi gereken kaynakları ve kaynak gruplarını belirtebilirsiniz.

    ![Etkinleştir - kapsamını belirtmek için kaynak filtre bölmesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Gerekirse, bir özel etkinleştirme başlangıç saatini belirtin. Üye, seçilen bir zaman sonra etkinleştirilir.

1. İçinde **neden** kutusuna, etkinleştirme isteği nedeni girin.

    ![Kapsam, başlangıç zamanı, süre ve neden tamamlanan etkinleştirme bölmesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Tıklayın **etkinleştirme**.

    Rolü onay gerektirmez, etkin ve etkin rollerin listesine eklenir. Rol kullanmak istiyorsanız, sonraki bölümde yer alan adımları izleyin.

    Varsa [rolünü onay gerektiren](pim-resource-roles-approval-workflow.md) etkinleştirmek için bir bildirim isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![Etkinleştirme isteği onay bekliyor bildirimidir](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Hemen etkinleştirildikten sonra bir rol kullanın

Azure kaynak rollerinizi hemen kullanmak için etkinleştirdikten sonra etkinleştirme işleminden sonra herhangi bir gecikme olması durumunda bu adımları izleyin.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **rollerim** , uygun bir listesini görmek için Azure AD rolleri ve Azure kaynağı rolleri.

1. Tıklayın **Azure kaynağı rolleri**.

1. Tıklayın **etkin rollerin** sekmesi.

1. Rol etkin hale geldikten sonra portalı oturumunu kapat ve yeniden oturum açın.

    Rol artık kullanmak için kullanılabilir olmalıdır.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için Bekleyen isteklerinizi durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim** Azure AD rolüne ve Azure Kaynak rolü listesini görmek için ister.

    ![İsteklerim - Bekleyen isteklerinizi gösterildiği Azure kaynak sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Görüntülemek için sağa kaydırma **istek durumu** sütun.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et

Onay gerektiren bir rolü etkinleştirmesi gerekmiyorsa, bekleyen istek dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management'ı açın.

1. Tıklayın **isteklerim**.

1. İptal etmek istediğiniz role tıklayın **iptal** bağlantı.

    İptal'i tıklattığınızda, isteği iptal edilecek. Rolü etkinleştirmek için yeniden, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![İptal işlemini vurgulanmış ile istek listem](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Bir rol etkinleştirdikten sonra izin verilmez

Bir rolü PIM etkinleştirdiğinizde, etkinleştirme anında ayrıcalıklı rol gerektiren tüm portallarında yayılmaz. Bazı durumlarda, değişiklik yayılır olsa bile, web Portalı'nda önbelleğe alma etkisi hemen alma değil değişiklik neden olabilir. Etkinleştirme gecikmesi, ne yapmanız gerektiğini aşağıdadır.

1. Azure portalı oturumunu kapat ve yeniden oturum açın.

    Bir Azure kaynak rolünün'ı etkinleştirdiğinizde, etkinleştirme aşamalarını görürsünüz. Tüm aşamaları tamamlandıktan sonra göreceğiniz bir **oturumunuzu** bağlantı. Oturumu kapatmak için bu bağlantıyı kullanabilirsiniz. Bu etkinleştirme gecikmesi için çoğu zaman çözüm.

1. PIM'de, rolü üyesi olarak listelendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri genişletmek veya yenileme](pim-resource-roles-renew-extend.md)
- [PIM Azure AD'ye rollerimi etkinleştir](pim-how-to-activate-role.md)
