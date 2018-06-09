---
title: Etkinleştirmek veya bir rolü devre dışı bırakma | Microsoft Docs
description: Azure Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri için rol etkinleştirme konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ebbac74f68142f7d0528c89f21f8c7cecbb82289
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233405"
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Etkinleştirmek veya Azure AD Privileged Identity Management rollerinde devre dışı bırakma
Azure Active Directory (AD) Privileged Identity Management, kuruluşların kaynaklarına Azure AD'de ve Office 365 veya Microsoft Intune gibi diğer Microsoft online services ayrıcalıklı erişimi nasıl yönetmek basitleştirir.  

Bir yönetici rolü için uygun yapılmışsa, ayrıcalıklı eylemler gerçekleştirmek gerektiğinde bu rol etkinleştirebilirsiniz anlamına gelir. Office 365 özellikleri bazen yönetiyorsanız, bu rolü diğer hizmetler çok etkiler Örneğin, kuruluşunuzun ayrıcalıklı rol Yöneticiler kalıcı genel yönetici kararı değil. Bunun yerine, bunlar, Exchange Online yönetici gibi Azure AD rolleri için uygun yapın. Bu rol, ayrıcalıklarına sahip olmanız gerekir ve ardından önceden belirlenmiş bir süre için yönetici denetim gerekir etkinleştirmek isteyebilirsiniz.

Bu makalede, Azure AD Privileged Identity Management (PIM) içindeki rollerine etkinleştirmek için gereken yöneticileri içindir. Bu izinlere sahip olmanız gerekir ve tamamladığınızda rolünü devre dışı bir rolü etkinleştirmek için adımlarda size yol gösterir. Ayrıca, ayrıcalıklı rol yöneticileri rolü (Önizleme) etkinleştirmek için onay gerektirebilir. Daha fazla bilgi edinmek [PIM onay iş akışları](./privileged-identity-management/azure-ad-pim-approval-workflow.md) burada.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management uygulamada kullanmak [Azure portal](https://portal.azure.com/) başka bir portal veya PowerShell çalışmasına olmaya olsa bile bir rol etkinleştirme istemek için. Azure Portal'da Azure AD Privileged Identity Management uygulaması yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizin seçin, sağ köşedeki kullanıcı adınıza işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="activate-a-role"></a>Bir rolü etkinleştirmesi
Bir rolü üstlenmesine gerektiğinde seçerek etkinleştirme isteyebilir **My rolleri** Azure AD Privileged Identity Management uygulaması Gezinti seçeneğinde sol gezinti sütun.

1. Oturum [Azure portal](https://portal.azure.com/) ve Azure AD Privileged Identity Management kutucuk seçin.
2. Seçin **My rolleri**. Sayfanın üst kısmındaki gruplandırmasında atanmış uygun rollerinizi listesi görüntülenir.
3. Etkinleştirmek için bir rol seçin.
4. Seçin **etkinleştirme**. **Rol etkinleştirme isteği** dikey penceresi görünür.
5. Bazı roller, rol etkinleştirilebilmesi için çok faktörlü kimlik doğrulama (MFA) gerektirir. Yalnızca bir kez oturum başına kimlik doğrulaması gerekir.
   
    ![MFA ile rol etkinleştirme - ekran görüntüsü önce doğrulayın][2]
6. Etkinleştirme isteğinin nedenini metin alanına girin.  Bazı roller, sorun bileti numarası girmesini gerektirir.
7. **Tamam**’ı seçin.  Rol onay gerektirmiyorsa, şimdi etkinleştirilir ve rol (doğrudan listesinin altındaki uygun rol atamalarını) etkin rollerinin listesi görüntülenir. Varsa [rolünü onay gerektiren](./privileged-identity-management/azure-ad-pim-approval-workflow.md) etkinleştirmek için bildirim kısaca isteğidir onay bekleyen bildiren tarayıcınızın sağ üst köşesinde görüntülenir.

    ![Bildirim - ekran görüntüsü istek][3]

## <a name="deactivate-a-role"></a>Bir rolü devre dışı bırakma
Bir rolü etkinleştirildikten sonra süresi sınırına (uygun süresi) ulaşıldığında otomatik olarak devre dışı bırakır.

Yönetim görevlerinizi erken tamamlarsanız, bir rolde el ile Azure AD Privileged Identity Management uygulaması devre dışı bırakabilirsiniz.  Seçin **My rolleri**, tamamladıktan rolünü seçin gelen kullanarak **etkin rol atamalarını** gruplandırma ve select **devre dışı bırak**.  

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal et
Onay gerektiren bir rolü etkinleştirmesi gerektirmeyen durumunda herhangi bir zamanda bekleyen isteği iptal edebilirsiniz. Yalnızca select **My rolleri** Azure AD Privileged Identity Management uygulaması Gezinti seçeneğinde sol gezinti sütun.

1. Oturum [Azure portal](https://portal.azure.com/) ve Azure AD Privileged Identity Management kutucuk seçin.
2. Seçin **My rolleri**. Sayfanın üst kısmındaki gruplandırmasında atanmış uygun rollerinizi listesi görüntülenir.
3. Bir rol seçin.
4. Seçin **etkinleştirme durumda onay bekleyen** rol etkinleştirme ayrıntıları dikey penceresinde başlık.
5. Seçin **iptal** en üstündeki **onay bekleyen** dikey.

   ![Bekleyen isteği ekran iptal et][4]

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Privileged Identity Management hakkında daha fazla bilgi edinmek isterseniz, aşağıdaki bağlantılardan daha fazla bilgi sahip.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
