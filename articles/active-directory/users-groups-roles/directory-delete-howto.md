---
title: -Azure Active Directory'ye bir Azure AD dizinini silmeye | Microsoft Docs
description: Self Servis dizinleri dahil olmak üzere silinmek üzere Azure AD dizini hazırlama açıklanmaktadır
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 91ac6b4530414850c52605bac8cb701aa2b877d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60473199"
---
# <a name="delete-a-directory-in-azure-active-directory"></a>Azure Active Directory'de bir dizini silme

Bir Azure AD dizin silindiğinde dizinde bulunan tüm kaynaklar da silinir. Kuruluşunuz, silmeden önce ilişkili kaynakları en aza indirerek hazırlayın. Yalnızca Azure Active Directory (Azure AD) genel yönetici Azure AD dizinini portaldan silebilirsiniz.

## <a name="prepare-the-directory"></a>Dizini hazırlama

Bazı denetimleri geçinceye kadar Azure AD dizini silemezsiniz. Bu denetimler, bir Azure AD dizin silinmesinin kullanıcı erişimi, Azure veya Office 365 kaynaklarına oturum olanağı gibi etkiler riskini azaltır. Örneğin, bir aboneliği ile ilişkili dizin yanlışlıkla silinirse kullanıcılar o aboneliğe ilişkin Azure kaynaklarına erişemez. Şu koşullar denetlenir:

* Dizini silmek için olan bir genel yönetici dışında bir dizinde kullanıcı olabilir. Dizin silinmeden önce diğer tüm kullanıcılar silinmelidir. Kullanıcılar şirket içinden eşitlenmişse, sonra eşitleme önce kapatılması ve Azure portalı veya Azure PowerShell cmdlet'leri kullanılarak bulut dizininde kullanıcıların silinmesi gerekir.
* Dizinde uygulama bulunamaz. Dizin silinmeden önce tüm uygulamaların kaldırılması gerekir.
* Hiçbir multi-Factor authentication sağlayıcılarını dizine bağlı olabilir.
* Microsoft Çevrimiçi Hizmetlerine (dizinle ilişkili Azure AD Premium, Microsoft Azure veya Office 365 gibi) ilişkin hiçbir aboneliğin bulunmaması gerekir. Örneğin, sizin için Azure'da varsayılan bir dizin oluşturulduysa ve Azure aboneliğinizin kimlik doğrulaması için hâlâ bu dizini kullanıyor olması halinde bu dizini silemezsiniz. Benzer şekilde, başka bir kullanıcı dizinle bir aboneliği ilişkilendirdiyse o dizini silemezsiniz.

## <a name="delete-the-directory"></a>Dizini Sil

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com) kuruluşunuzun genel yöneticisi olan bir hesapla.

2. **Azure Active Directory**'yi seçin.

3. Silmek istediğiniz dizine geçin.
  
   ![Silmeden önce kuruluş onaylayın](./media/directory-delete-howto/delete-directory-command.png)

4. Seçin **silme directory**.
  
   ![Kuruluş silme komutunu seçin](./media/directory-delete-howto/delete-directory-list.png)

5. Dizininizi bir veya daha fazla denetimi geçemezse geçirmek hakkında daha fazla bilgi için bir bağlantı edinirsiniz. Tüm denetimlerden başarıyla sonra seçin **Sil** tıklayarak işlemi tamamlar.

## <a name="if-you-cant-delete-the-directory"></a>Olan dizini silemez

Azure AD dizininizi yapılandırıldığında, ayrıca lisans tabanlı abonelikler, kuruluşunuzun Azure AD Premium P2, Office 365 iş ekstra, veya Enterprise Mobility + Security E5 gibi etkinleştirdikten. Yanlışlıkla veri kaybını önlemek için abonelik tam olarak silinene kadar dizini silemezsiniz. Abonelik olmalıdır bir **yetki kaldırıldı** dizin silinmesine izin vermek için durum. Bir **süresi dolan** veya **iptal** abonelik taşır **devre dışı bırakılmış** durumu ve son aşaması **yetki kaldırıldı** durumu.

Beklenecekler için deneme Office 365 aboneliği (Ücretli iş ortağı/CSP, Kurumsal Anlaşma veya Toplu Lisanslama dahil değil) süresi dolduğunda, aşağıdaki tabloya bakın. Office 365 veri saklama ve abonelik yaşam döngüsü hakkında daha fazla bilgi için bkz. [Office 365 işletme Aboneliğim sona erdiğinde verilerime ve erişim için ne olur?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Abonelik durumu | Veriler | Veri erişimi
----- | ----- | -----
Etkin (deneme sürümü için 30 gün) | Tüm erişilebilir veri | Kullanıcıların Office 365 dosyalar veya uygulamalar için normal erişimi<br>Yöneticiler normal Microsoft 365 Yönetim Merkezi ve kaynaklara erişimi 
Süresi dolan (30 gün) | Tüm erişilebilir veri| Kullanıcıların Office 365 dosyalar veya uygulamalar için normal erişimi<br>Yöneticiler normal Microsoft 365 Yönetim Merkezi ve kaynaklara erişimi
Devre dışı (30 gün) | Verileri yalnızca Yöneticisi için erişilebilir | Kullanıcılar Office 365 dosyalar veya uygulamalar erişemez.<br>Yöneticiler Microsoft 365 Yönetim Merkezine erişim ancak olamaz için lisans atama veya güncelleştirme
Sağlaması (30 gün sonra devre dışı) | Silinen verileri (otomatik olarak silinmesini başka bir hizmetler kullanılıyorsa) | Kullanıcılar Office 365 dosyalar veya uygulamalar erişemez.<br>Yöneticiler satın alın ve diğer Aboneliklerini yönetmek için Microsoft 365 Yönetim merkezine erişebilirsiniz

## <a name="delete-a-subscription"></a>Aboneliği silme

Bir abonelik, Microsoft 365 Yönetim merkezini kullanarak üç gün içinde silinecek yetki kaldırıldı durumu koyabilirsiniz.

1. Oturum [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com) kuruluşunuzdaki genel yönetici olan bir hesapla. İlk varsayılan etki alanı contoso.onmicrosoft.com olan "Contoso" dizini silmeye çalışıyorsanız, bir UPN ile aşağıdaki gibi oturum admin@contoso.onmicrosoft.com.

2. Seçin **faturalama** seçip **abonelikleri**, iptal etmek istediğiniz aboneliği seçin. Tıkladıktan sonra **iptal**, sayfayı yenileyin.
  
   ![Aboneliği silmek için bağlantısını Sil](./media/directory-delete-howto/delete-command.png)
  
3. Seçin **Sil** abonelik silip hüküm ve koşulları kabul edin. Tüm verileri kalıcı olarak üç gün içinde silinir. Fikrinizi değiştirirseniz üç gün boyunca abonelik yeniden etkinleştirebilir.
  
   ![hüküm ve koşulları dikkatle okuyun](./media/directory-delete-howto/delete-terms.png)

4. Abonelik durumu değiştirildiğinde artık abonelik silinmek üzere işaretlendi. Abonelik girer **yetki kaldırıldı** 72 saat belirtin.

5. Dizininizde bir aboneliğin silinmesinden ve 72 saat tetiklenirse, oturum sonra tekrar içine Azure AD yönetim merkezini yeniden ve orada gerekli bir eylem ve, dizinin silinmesini engelleyen herhangi bir abonelik olması gerekir. Azure AD dizininizi başarıyla Sil olması gerekir.
  
   ![silme ekranında abonelik denetimi başarılı](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="i-have-a-trial-subscription-that-blocks-deletion"></a>Silinmesini engelleyen bir deneme aboneliği sahibim

Vardır [Self Servis kaydolma ürünleri](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide) Microsoft Power BI, Rights Management Services, Microsoft Power Apps veya Dynamics 365, tek tek kullanıcılar Konuk kullanıcı kimlik doğrulaması için de oluşturur Office 365 üzerinden kaydolabilir gibi Azure AD dizininizi. Veri kaybını önlemek için dizinden tam olarak silinene kadar bu Self Servis ürünleri dizini silme işlemleri engelleyin. Kullanıcı tek tek kaydolan ya da ürün atandığı yalnızca Azure AD Yöneticisi tarafından silinebilir.

Ürünlerin nasıl atandığını, Self Servis kaydolma iki tür vardır: 

* Kuruluş düzeyinde ataması: Bir Azure AD Yöneticisi, kuruluş genelinde ürün atar ve ayrı ayrı lisanslanmaz olsa bile bir kullanıcı etkin hizmet bu kuruluş düzeyinde atamayla kullanıyor.
* Kullanıcı düzeyi atamasını: Bireysel kullanıcı Self Servis kayıt sırasında temelde ürün kendileri için yönetici atar Kuruluşun Yöneticisi tarafından yönetilen olur (bkz [yönetici devralma işlemini bir yönetilmeyen dizinin](domains-admin-takeover.md), sonra yönetici kullanıcılara Self Servis kayıt olmadan doğrudan ürün atayabilirsiniz.  

Self Servis kaydolma ürünün silme başlattığınızda eylemi kalıcı olarak verileri siler ve hizmet için tüm kullanıcı erişimini kaldırır. Teklif tek tek veya kuruluş düzeyinde atanmış herhangi bir kullanıcı daha sonra oturum açma veya var olan tüm veri erişim engellenir. Self Servis kaydolma ürün ile veri kaybını önlemek Dilerseniz ister [Microsoft Power BI panoları](https://docs.microsoft.com/power-bi/service-export-to-pbix) veya [Rights Management Services ilke yapılandırması](https://docs.microsoft.com/azure/information-protection/configure-policy#how-to-configure-the-azure-information-protection-policy), verilerin yedeklendiğinden emin olun ve başka bir yerde kaydedildi.

Şu anda kullanılabilir Self Servis kaydolma ürünler ve hizmetler hakkında daha fazla bilgi için bkz. [kullanılabilir Self Servis programlar](https://docs.microsoft.com/office365/admin/misc/self-service-sign-up?view=o365-worldwide#available-self-service-programs).

Beklenecekler için deneme Office 365 aboneliği (Ücretli iş ortağı/CSP, Kurumsal Anlaşma veya Toplu Lisanslama dahil değil) süresi dolduğunda, aşağıdaki tabloya bakın. Office 365 veri saklama ve abonelik yaşam döngüsü hakkında daha fazla bilgi için bkz. [Office 365 işletme Aboneliğim sona erdiğinde verilerime ve erişim için ne olur?](https://docs.microsoft.com/office365/admin/subscriptions-and-billing/what-if-my-subscription-expires?view=o365-worldwide).

Ürün durumu | Veriler | Veri erişimi
------------- | ---- | --------------
Etkin (deneme sürümü için 30 gün) | Tüm erişilebilir veri | Kullanıcıların Self Servis kaydolma ürün, dosyalar veya uygulamalar için normal erişimi<br>Yöneticiler normal Microsoft 365 Yönetim Merkezi ve kaynaklara erişimi
Silme | Silinen veriler | Kullanıcıların, Self Servis kaydolma ürün, dosyalar veya uygulamalar erişemez.<br>Yöneticiler satın alın ve diğer Aboneliklerini yönetmek için Microsoft 365 Yönetim merkezine erişebilirsiniz

## <a name="how-can-i-delete-a-self-service-sign-up-product-in-the-azure-portal"></a>Bir Self Servis kaydolma ürün Azure portalında nasıl silebilir miyim?

Microsoft Power BI veya Azure Rights Management Services içine gibi Self Servis kaydolma ürün koyabilirsiniz bir **Sil** durumu Azure AD Portalı'nda hemen silinecek.

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) kuruluşunuzdaki genel yönetici olan bir hesapla. İlk varsayılan etki alanı contoso.onmicrosoft.com olan "Contoso" dizini silmeye çalışıyorsanız, bir UPN ile aşağıdaki gibi oturum admin@contoso.onmicrosoft.com.

2. Seçin **lisansları**ve ardından **Self Servis kaydolma ürünleri**. Tüm Self Servis kaydolma ürünlerini ayrı olarak bilgisayar başına aboneliklerinden gelen görebilirsiniz. Kalıcı olarak silmek istediğiniz ürünü seçin. Microsoft Power BI hizmetinde bir örnek aşağıdadır:

    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/directory-delete-howto/licenses-page.png)

3. Seçin **Sil** ürünü silmek ve koşulları kabul etmek için verilerin hemen ve geri alınamayacak biçimde silinir. Bu silme eylemi, tüm kullanıcıları kaldırın ve ürün kuruluş erişimini kaldırmak. Silme işlemine ilerlemek için Evet'i tıklatın.  

    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/directory-delete-howto/delete-product.png)

4. Seçtiğinizde, **Evet**, Self Servis Ürün silme işlemi başlatıldı. Silme işleminin sürmekte size bildirir, bir bildirim yoktur.  

    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/directory-delete-howto/progress-message.png)

5. İçin Self Servis kaydolma ürün durumu değiştirildiğinde artık **silinmiş**. Sayfayı yenileyin, ürünün içinden kaldırılamaz **Self Servis kaydolma ürünleri** sayfası.  

    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/directory-delete-howto/product-deleted.png)

6. Tüm ürünler sildikten sonra Azure AD yönetim merkezine geri tekrar oturum ve gerekli bir eylem ve, dizinin silinmesini engelleyen bir ürün yok. Azure AD dizininizi başarıyla Sil olması gerekir.

    ![Kullanıcı adı yanlış yazmış veya bulunamadı](./media/directory-delete-howto/delete-organization.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/)
