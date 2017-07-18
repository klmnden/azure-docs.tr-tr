---
title: "Azure AD Privileged Identity Management ile çalışmaya başlama | Microsoft Belgeleri"
description: "Azure portalında Azure Active Directory Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri nasıl yöneteceğinizi öğrenin."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.contentlocale: tr-tr
ms.lasthandoff: 07/10/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ı kullanmaya başlama
Azure Active Directory (AD) Privileged Identity Management sayesinde kuruluşunuz içindeki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Azure AD'deki ve Office 365 ya da Microsoft Intune gibi diğer çevrimiçi Microsoft hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

Bu makalede Azure AD PIM uygulamasını Azure portalı panonuza nasıl ekleyeceğiniz anlatılmaktadır.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management'ı kullanmadan önce uygulamayı Azure portalı panonuza eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com/) dizininizin genel yöneticisi olarak oturum açın.
2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınızı seçin. PIM'i kullanmak istediğiniz dizini seçin.
3. **Daha fazla hizmet** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

Dizininizde Azure AD Privileged Identity Management’ı kullanan ilk kişiyseniz, dizinde **Güvenlik yöneticisi** ve **Ayrıcalıklı rol yöneticisi** rolleri otomatik olarak size atanır. Yalnızca ayrıcalıklı rol yöneticileri kullanıcıların rol atamalarını yönetebilir. Ayrıca [güvenlik sihirbazını](active-directory-privileged-identity-management-security-wizard.md) çalıştırmayı da seçebilirsiniz. Bu sihirbaz ilk keşif ve atama deneyiminde size yol gösterir.

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme
Azure AD Privileged Identity Management ayarlandığında, uygulamayı her açtığınızda gezinti dikey penceresini görürsünüz. Kimlik yönetimi görevlerinizi gerçekleştirmek için bu dikey pencereyi kullanın.

![PIM için üst düzey görevler - ekran görüntüsü](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* **Rollerim**, size atanan rollerin listesine erişmenizi sağlar. Bu bölümde, uygun olduğunuz tüm rolleri etkinleştirebilirsiniz.
* **İstekleri Onayla (Önizleme)**, dizininizdeki kullanıcılardan gelen beklemedeki etkinleştirme isteklerinin listesini görüntüler. [Daha fazla bilgi edinin.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Bekleyen İstekler (Önizleme)**, etkinleştirilebilecek tüm geçerli istekleri görüntüler.
* **Erişimi Gözden Geçir**, erişimi kendiniz için veya başka bir kullanıcı için gözden geçiriyor olmanızdan bağımsız olarak, sizi tamamlamanız gereken beklemedeki erişim gözden geçirmelerine götürür.
* “Yönet” bölümünün altında yer alan **Azure AD Dizin Rolleri** panosu, ayrıcalıklı rol yöneticileri tarafından rol atamalarını yönetmek, rol etkinleştirme ayarlarını değiştirmek, erişim gözden geçirmelerini başlatmak ve daha birçok işlevi gerçekleştirmek üzere kullanılır. Bu panodaki seçenekler, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Privileged Identity Management'a genel bakış](active-directory-privileged-identity-management-configure.md), kuruluşunuzdaki yönetim erişimini nasıl yönetebileceğinize ilişkin daha fazla ayrıntı içerir.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

