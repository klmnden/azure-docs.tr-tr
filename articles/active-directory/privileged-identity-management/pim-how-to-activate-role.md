---
title: Etkinleştirmek veya bir rolü devre dışı bırakma | Microsoft Docs
description: Azure Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri için rol etkinleştirme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0ad95cab1ae8072004a528e449cf828b32500be8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590404"
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Etkinleştirmek veya Azure AD Privileged Identity Management içinde rol devre dışı bırakma
Azure Active Directory (AD) Privileged Identity Management, kuruluşlara ayrıcalıklı Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetme basitleştirir.  

Bir yönetici rolü için uygun yapılmadıysa, ayrıcalıklı eylemler gerçekleştirmek, ihtiyacınız olduğunda bu role aktive edebileceğiniz anlamına gelir. Office 365 özellikleri bazen yönetiyorsanız, bu rol diğer hizmetleri çok etkiler beri Örneğin, kuruluşunuzun ayrıcalıklı rol yöneticileri, kalıcı bir genel yönetici sunamazsınız. Bunun yerine, bunlar, Exchange Online yönetici gibi Azure AD rolleri için uygun yapın. Kendi ayrıcalıklarına sahip olmanız ve ardından önceden belirlenmiş bir süre için yönetici denetim gerekir. Bu rolü etkinleştirmek için istekte bulunabilir.

Bu makalede, Azure AD Privileged Identity Management (PIM) içindeki rollerine etkinleştirmek için gereken yöneticilerin içindir. Bu izinlere ihtiyacınız olur ve işiniz bittiğinde rolünü devre dışı bir rolü etkinleştirmek için adımlarda size yol gösterir. Ayrıca, ayrıcalıklı rol yöneticileri (Önizleme) bir rolü etkinleştirmek için onay isteyebilir. Daha fazla bilgi edinin [PIM onayı iş akışı](./azure-ad-pim-approval-workflow.md) burada.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management kullanıldığını [Azure portalında](https://portal.azure.com/) başka bir portal veya PowerShell çalışılacak gideceğinizi olsa bile, bir rol etkinleştirme istemek için. Azure AD Privileged Identity Management uygulaması, Azure portalında yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizini seçin, sağ üst köşedeki kullanıcı adınızı işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi
Bir rolü gerektiğinde seçerek etkinleştirme isteyebilir **My rolleri** Azure AD Privileged Identity Management uygulaması Gezinti seçeneği sol gezinti sütununda.

1. Oturum [Azure portalında](https://portal.azure.com/) ve Azure AD Privileged Identity Management kutucuğunu seçin.
2. Seçin **Rollerim**. Sayfanın üstündeki gruplandırma, atanan uygun rolleri listesi görüntülenir.
3. Etkinleştirmek için bir rol seçin.
4. Seçin **etkinleştirme**. **Rol etkinleştirme isteği** dikey penceresi görünür.
5. Bu rolü etkinleştirmeden önce bazı roller, multi-Factor Authentication (MFA) gerektirir. Yalnızca bir kez oturum başına kimlik doğrulaması gerekir.
   
    ![MFA ile rol etkinleştirme - ekran görüntüsü önce doğrulayın.](./media/pim-how-to-activate-role/PIM_activation_MFA.png)
6. Etkinleştirme isteği nedeni metin alanına girin.  Bazı roller, sorun bileti numarası girmesini gerektirir.
7. **Tamam**’ı seçin.  Rolü onay gerektirmez, artık etkinleştirilir ve rol (doğrudan listesinin altındaki uygun rol atamaları) etkin rollerin listesinde görünür. Varsa [rolünü onay gerektiren](./azure-ad-pim-approval-workflow.md) etkinleştirmek için bir bildirim kısaca isteği onay bekliyor olduğunu bildiren, tarayıcınızın sağ üst köşesinde görünür.

    ![İstek bildirimi - ekran görüntüsü](./media/pim-how-to-activate-role/PIM_Request_Pending_Toast2.png)

## <a name="deactivate-a-role"></a>Rol devre dışı bırak
Bir rol etkinleştirildikten sonra süre sınırı (uygunluk süresi) ulaşıldığında otomatik olarak devre dışı bırakır.

Yönetim görevlerinizi erken tamamlarsanız, bir rol el ile Azure AD Privileged Identity Management uygulaması devre dışı bırakabilirsiniz.  Seçin **My rolleri**, bitti rolü seçin kullanarak gelen **etkin rol atamaları** gruplandırma ve select **devre dışı bırak**.  

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et
Onay gerektiren bir rolü etkinleştirmesi gerektirmeyen olayda bekleyen istek dilediğiniz zaman iptal edebilirsiniz. Yalnızca select **My rolleri** Azure AD Privileged Identity Management uygulaması Gezinti seçeneği sol gezinti sütununda.

1. Oturum [Azure portalında](https://portal.azure.com/) ve Azure AD Privileged Identity Management kutucuğunu seçin.
2. Seçin **Rollerim**. Sayfanın üstündeki gruplandırma, atanan uygun rolleri listesi görüntülenir.
3. Bir rol seçin.
4. Seçin **Etkinleştirme Onayı beklemede olan** rol etkinleştirme ayrıntıları dikey penceresinde başlık.
5. Seçin **iptal** en üstündeki **onay bekleyen** dikey penceresi.

   ![Bekleyen istek ekran iptal et](./media/pim-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Privileged Identity Management hakkında daha fazla bilgi edinmek ilginizi çekiyorsa, aşağıdaki bağlantılardan daha fazla bilgi vardır.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
