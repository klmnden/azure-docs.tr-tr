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
ms.translationtype: Human Translation
ms.sourcegitcommit: b9a3b64d9de48f17a295ca7a9ea58cf26e8f83ed
ms.openlocfilehash: 6945529b8189c270ca6e5c41be0cd0d9caa37cb4
ms.contentlocale: tr-tr
ms.lasthandoff: 02/28/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ı kullanmaya başlama
Azure Active Directory (AD) Privileged Identity Management sayesinde kuruluşunuz içindeki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Azure AD'deki ve Office 365 ya da Microsoft Intune gibi diğer çevrimiçi Microsoft hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

Bu makalede Azure AD PIM uygulamasını Azure portalı panonuza nasıl ekleyeceğiniz anlatılmaktadır.

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management'ı kullanmadan önce uygulamayı Azure portalı panonuza eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com/) dizininizin genel yöneticisi olarak oturum açın.
2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınızı seçin. PIM'i kullanacağınız dizini seçin.
3. **Daha fazla hizmet** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

Dizininizde Azure AD Privileged Identity Management'ı ilk kez siz kullanıyorsanız [güvenlik sihirbazı](active-directory-privileged-identity-management-security-wizard.md) ilk atama deneyiminizde size yol gösterir. Bunun ardından, otomatik olarak dizinin ilk **Güvenlik yöneticisi** ve **Ayrıcalıklı rol yöneticisi** olursunuz. Yalnızca bir ayrıcalıklı rol yöneticisi diğer yöneticiler için erişimi yönetmek üzere bu uygulamaya erişebilir.  

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme
Azure AD Privileged Identity Management ayarlandığında, uygulamayı her açtığınızda gezinti dikey penceresini görürsünüz. Kimlik yönetimi görevlerinizi gerçekleştirmek için bu dikey pencereyi kullanın.

![PIM için üst düzey görevler - ekran görüntüsü](./media/active-directory-privileged-identity-management-getting-started/pim_tasks.png)

* **Rollerimi etkinleştir** seçeneği, size atanan görevlerin listesine erişmenizi sağlar. Burada uygun olduğunuz tüm rolleri etkinleştirirsiniz.
* **Ayrıcalıklı rolleri yönet** panosu; ayrıcalıklı rol yöneticileri tarafından rol atamalarını yönetmek, rol etkinleştirme ayarlarını değiştirmek, erişim incelemeleri başlatmak ve daha birçok işlevi gerçekleştirmek üzere kullanılır. Bu panodaki seçenekler, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır.
* **Ayrıcalıklı erişimi incele** seçeneği (erişimi kendiniz için veya başka bir kullanıcı için inceliyor olmanızdan bağımsız olarak) sizi tamamlamanız gereken beklemedeki erişim incelemelerine götürür. 

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Privileged Identity Management'a genel bakış](active-directory-privileged-identity-management-configure.md), kuruluşunuzdaki yönetim erişimini nasıl yönetebileceğinize ilişkin daha fazla ayrıntı içerir.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

