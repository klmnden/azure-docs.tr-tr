---
title: Bir Azure Active Directory Kiracı dizinini silmeye | Microsoft Docs
description: Azure AD Kiracı dizinini silme işlemi için hazırlama açıklanmaktadır
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/13/2018
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: b1d3439412e324c71687c43aa9e47c520cb72262
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42062125"
---
# <a name="delete-an-azure-active-directory-tenant"></a>Azure Active Directory kiracısı Sil
Bir kiracı silindiğinde, kiracıda bulunan tüm kaynaklar da silinir. Kiracı, silmeden önce ilişkili kaynakları en aza indirerek hazırlamanız gerekir. Yalnızca Azure Active Directory (Azure AD) genel yönetici portalından Azure AD kiracısı silebilirsiniz.

## <a name="prepare-the-tenant-for-deletion"></a>Kiracı silme işlemi için hazırlama

Bazı denetimleri geçinceye kadar Azure AD'de bir kiracı silinemiyor. Bu denetimler, bir kiracı silinmesinin kullanıcı erişimi, Azure veya Office 365 kaynaklarına oturum olanağı gibi etkiler riskini azaltır. Örneğin, bir aboneliği ile ilişkili Kiracı yanlışlıkla silinirse kullanıcılar o aboneliğe ilişkin Azure kaynaklarına erişemez. Denetlenen koşullar açıklanmaktadır:

* Kiracıyı silebilmeniz için olan bir genel yönetici dışında bir Kiracı Kullanıcı olabilir. Kiracı silinebilmesi için önce diğer tüm kullanıcılar silinmelidir. Kullanıcılar şirket içinden eşitlenmişse, eşitleme ardından kapatılmalıdır ve kullanıcıların Azure portal veya Azure PowerShell cmdlet'leri kullanılarak bulut kiracıda silinmesi gerekir. 
* Kiracıda uygulama olabilir. Kiracı silinebilmesi için tüm uygulamaları kaldırılması gerekir.
* Çok faktörlü kimlik doğrulama sağlayıcısı yok kiracısına bağlı olabilir.
* Microsoft Azure veya Office 365 gibi Microsoft çevrimiçi hizmetlerine ilişkin hiçbir aboneliğin olabilir veya kiracıyla ilişkili Azure AD Premium. Örneğin, bir varsayılan Kiracı sizin için Azure'da oluşturulduysa, Azure aboneliğinizin hala bu Kiracı için kimlik doğrulaması kullanır, bu Kiracı silemezsiniz. Benzer şekilde, başka bir kullanıcı aboneliği ile ilişkili ise, bir kiracı silemezsiniz. 

## <a name="delete-an-azure-ad-tenant"></a>Azure AD kiracısını silme

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) Kiracı için genel yönetici olan bir hesapla.

2. **Azure Active Directory**'yi seçin.

3. Anahtarı silmek istediğiniz kiracıya.
  
  ![Dizin düğmesini Sil](./media/directory-delete-howto/delete-directory-command.png)

4. Seçin **silme directory**.
  
  ![Dizin düğmesini Sil](./media/directory-delete-howto/delete-directory-list.png)

5. Kiracınızda bir veya daha fazla denetimi geçemezse geçirmek hakkında daha fazla bilgi için bir bağlantı edinirsiniz. Tüm denetimlerden başarıyla sonra seçin **Sil** tıklayarak işlemi tamamlar.

## <a name="i-have-an-expired-subscription-but-i-cant-delete-the-tenant"></a>Süresi dolmuş bir Aboneliğim sahibim ancak kiracısını silemiyorum

Azure Active Directory kiracınızın yapılandırıldığında, ayrıca lisans tabanlı abonelikler, kuruluşunuzun Azure Active Directory Premium P2, Office 365 iş ekstra, veya Enterprise Mobility + Security E5 gibi etkinleştirdikten. Tam olarak, yanlışlıkla veri kaybını önlemek için silinmelerinden bu abonelikler dizini silme engelleyin. Abonelik olmalıdır bir **yetki kaldırıldı** Kiracı silinmesine izin vermek için durum. Bir **süresi dolan** veya **iptal** abonelik taşır **devre dışı bırakılmış** durumu ve son aşaması **Deprovisoned** durumu. 

Beklenecekler için deneme Office 365 aboneliği (Ücretli iş ortağı/CSP, Kurumsal Anlaşma veya Toplu Lisanslama dahil değil) süresi dolduğunda, aşağıdaki tabloya bakın. Office 365 veri saklama ve abonelik yaşam döngüsü hakkında daha fazla bilgi için bkz. [Office 365 işletme Aboneliğim sona erdiğinde verilerime ve erişim için ne olur?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Abonelik durumu | Veriler | Veri erişimi
----- | ----- | -----
Etkin (deneme sürümü için 30 gün)  | Tüm erişilebilir veri    | <li>Kullanıcıların Office 365 dosyalar veya uygulamalar için normal erişimi<li>Yöneticilerin Office 365 Yönetim merkezine ve kaynaklara normal erişimi 
Süresi dolan (30 gün)   | Tüm erişilebilir veri    | <li>Kullanıcıların Office 365 dosyalar veya uygulamalar için normal erişimi<li>Yöneticilerin Office 365 Yönetim merkezine ve kaynaklara normal erişimi
Devre dışı (30 gün) | Verileri yalnızca Yöneticisi için erişilebilir  | <li>Kullanıcılar Office 365 dosyalar veya uygulamalar erişemez.<li>Yöneticileri Office 365 Yönetim Merkezine erişim ancak olamaz lisansları atayabilir veya güncelleştirme
Sağlaması (30 gün sonra devre dışı) | Silinen verileri (otomatik olarak silinmesini başka bir hizmetler kullanılıyorsa) | <li>Kullanıcılar Office 365 dosyalar veya uygulamalar erişemez.<li>Yöneticiler satın alın ve diğer Aboneliklerini yönetmek için Office 365 Yönetim merkezine erişebilirsiniz 

Bir aboneliğe koyabilirsiniz bir **Deprovisoned** durumu iş Yönetim Merkezi için Microsoft Store kullanarak 3 gün içinde silinecek. Bu özellik, Office 365 Yönetim merkezine yakında kullanıma sunulacaktır.

1. Oturum [iş Yönetim Merkezi için Microsoft Store](https://businessstore.microsoft.com/manage/) kiracısında genel yönetici olan bir hesapla. İlk varsayılan etki alanı contoso.onmicrosoft.com olan "Contoso" kiracısını silmeye çalışıyorsanız, bir UPN ile aşağıdaki gibi oturum admin@contoso.onmicrosoft.com.

2. Git **Yönet** sekmenize **ürünleri ve Hizmetleri**, iptal etmek istediğiniz aboneliği seçin. Tıkladıktan sonra **iptal**, sayfayı yenileyin.
  
  ![Aboneliği silmek için bağlantısını Sil](./media/directory-delete-howto/delete-command.png)
  
3. Seçin **Sil** abonelik silip hüküm ve koşulları kabul edin. Tüm verileri kalıcı olarak üç gün içinde silinecek. Fikrinizi değiştirirseniz üç gün boyunca abonelik yeniden etkinleştirebilir.
  
  ![hüküm ve koşullar](./media/directory-delete-howto/delete-terms.png)

4. Abonelik durumu değiştirildiğinde artık abonelik silinmek üzere işaretlendi. Abonelik eneters **yetki kaldırıldı** 72 saat belirtin.

5. Kiracınızda, bir aboneliğin silinmesinden ve 72 saat tetiklenirse, oturum sonra tekrar içine Azure AD yönetim merkezini yeniden ve orada gerekli bir eylem ve, Kiracı silinmesini engelleyen herhangi bir abonelik olması gerekir. Başarılı bir şekilde, Azure AD kiracısını silmek mümkün olması gerekir.
  
  ![silme ekranında abonelik denetimi başarılı](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/)
