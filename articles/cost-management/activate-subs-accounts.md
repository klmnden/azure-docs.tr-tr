---
title: "Azure abonelikleri ve hesapları etkinleştirme | Microsoft Docs"
description: "Yeni ve mevcut hesapları için Azure Resource Manager API'leri kullanılarak erişimi etkinleştir ve ortak hesap sorunları çözün."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 03/01/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: a0dc2ee201c1729b10cd363553cdf5d61ec87748
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="activate-azure-subscriptions-and-accounts-with-azure-cost-management"></a>Azure abonelikleri ve Azure Yönetimi maliyeti hesaplarıyla etkinleştir

Azure Kaynak Yöneticisi kimlik bilgilerinizi eklemek veya Yönetimi hesaplarını ve abonelikleri Azure Kiracı içinde bulmak Azure maliyeti sağlar. Ayrıca, sanal makinelere etkin Azure tanılama uzantısını varsa, Azure Yönetimi maliyeti CPU ve bellek gibi genişletilmiş ölçümleri toplayabilir. Bu makalede, Azure Resource Manager API'leri için yeni ve mevcut hesapları kullanarak erişimi etkinleştirmek açıklar. Ayrıca, ortak hesap sorunların nasıl çözümleneceğini açıklar.

Azure maliyeti yönetim, Azure aboneliği verilerinizden en iyi erişemiyor, abonelik olduğunda _etkinleştirilmemiş_. Düzenlemeniz gerekir _etkinleştirilmemiş_ Azure Yönetimi maliyeti bunlara erişebilmesi için hesapları.

## <a name="required-azure-permissions"></a>Gereken Azure izinler

Bu makaledeki yordamları tamamlamak için belirli izinler gerekir. Siz veya Kiracı Yöneticisi aşağıdaki izinleri her ikisi de olması gerekir:

- Azure AD kiracınıza CloudynCollector uygulamayı kaydetme izni.
- Azure aboneliğinizin bir role uygulama atama olanağı.

Hesaplarınızı Azure aboneliğinizin olması gerekir `Microsoft.Authorization/*/Write` CloudynCollector uygulama atamak için erişim. Bu eylem aracılığıyla verilen [sahibi](../active-directory/role-based-access-built-in-roles.md#owner) rol veya [kullanıcı erişimi Yöneticisi](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) rol.

Hesabınızı atanmışsa **katkıda bulunan** rolüne sahip olmadığınız uygulama atamak için yeterli izni. Azure aboneliğinize CloudynCollector uygulama atama çalışılırken bir hata alırsınız.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleyin

1. İçine oturum [Azure portal](https://portal.azure.com).
2. Azure portalında seçin **Azure Active Directory**.
3. Azure Active Directory'de seçin **kullanıcı ayarlarını**.
4. Denetleme **uygulama kayıtlar** seçeneği.
    - Bu ayarlanırsa **Evet**, sonra da yönetici olmayan kullanıcılar AD uygulamaları kaydedebilirsiniz. Bu ayar, Azure AD kiracısı içinde herhangi bir kullanıcı bir uygulama kaydedebilirsiniz anlamına gelir. Gereken Azure abonelik izinleri devam edebilirsiniz.  
    ![Uygulama kayıtlar](./media/activate-subs-accounts/app-register.png)
    - Varsa **uygulama kayıtlar** seçeneği **Hayır**, yalnızca Kiracı yönetici kullanıcıların Azure Active Directory uygulamaları kaydedebilirsiniz sonra. Kiracı yöneticinizle CloudynCollector uygulama kaydetmeniz gerekir.


## <a name="add-an-account-or-update-a-subscription"></a>Hesap eklemek veya aboneliği güncelleştir

Bir hesap güncelleştirme aboneliği eklediğinizde, Azure verilerinizi Azure maliyeti yönetim erişimi verin.

### <a name="add-a-new-account-subscription"></a>Yeni bir hesap (abonelik) Ekle

1. Azure maliyeti Yönetim Portalı'nda üst sağ dişli simgesini tıklatın ve seçin **bulut hesapları**.
2. Tıklatın **yeni hesap eklemek** ve **yeni hesap eklemek** kutusu görüntülenir. Gerekli bilgileri girin.  
    ![Yeni hesap kutusunu Ekle](./media/activate-subs-accounts//add-new-account.png)

### <a name="update-a-subscription"></a>Bir aboneliği güncelleştir

1. Güncelleştirmek istiyorsanız bir _etkinleştirilmemiş_ Azure maliyeti yönetim hesapları Yönetimi ' nde zaten var. abonelik sağ üst tarafındaki düzenleme kalem simgesine tıklayın _GUID Kiracı_. Abonelikler, üst Kiracı altında gruplandırılır, böylece abonelikleri ayrı ayrı etkinleştirme kaçının.
    ![Abonelikleri yeniden Bul](./media/activate-subs-accounts/existing-sub.png)
2. Gerekirse Kiracı kimliği girin Kiracı Kimliğinizi bilmiyorsanız, bulmak için aşağıdaki adımları kullanın:
    1. İçine oturum [Azure portal](https://portal.azure.com).
    2. Azure portalında seçin **Azure Active Directory**.
    3. Kiracı Kimliği almak için seçin **özellikleri** Azure AD kiracınız için.
    4. Dizin kimliği GUID kopyalayın. Bu değer, Kiracı kimliğidir.
    Daha fazla bilgi için bkz: [alma Kiracı kimliği](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
3. Gerekirse, kimliğinizi oranı seçin Oranı Kimliğinizi bilmiyorsanız, bulmak için aşağıdaki adımları kullanın.
    1. Sağ üst köşede Azure Portalı'nın, kullanıcı bilgilerinizi tıklayın ve ardından **faturamı görüntüle**.
    2. Altında **Faturalama hesabı**, tıklatın **abonelikleri**.
    3. Altında **My abonelikleri**, aboneliği seçin.
    4. Kimliği altında gösterildiğini hızınızı **Teklif kimliği**. Aboneliği için teklif Kimliğini kopyalayın.
4. Yeni Hesap Ekle (veya abonelik Düzenle) kutusunda, **kaydetmek** (veya **sonraki**). Azure portalına yönlendirilirsiniz.
5. Portalında oturum açın. Tıklatın **kabul** Azure maliyeti yönetim toplayıcısı yetkilendirmek için Azure hesabınıza erişim.

    Azure maliyeti yönetim hesapları Yönetim sayfasına yönlendirilirsiniz ve aboneliğiniz ile güncelleştirilir **etkin** hesap durumu. Resource Manager sütununun altında yeşil onay işareti simgesi görüntülemelidir.

    Bir veya daha fazla abonelik için yeşil onay simgesi görmüyorsanız, bu abonelik için reader uygulaması (CloudynCollector) oluşturmak için izinlere sahip değil anlamına gelir. Abonelik için daha yüksek izinleri olan bir kullanıcı bu işlemi yinelemeniz gerekir.

Gözcü [için Azure Resource Manager Azure maliyeti yönetimi ile bağlanma](https://youtu.be/oCIwvfBB6kk) video işleminde size kılavuzluk eder.

>[!VIDEO https://www.youtube.com/embed/oCIwvfBB6kk?ecver=1]

## <a name="resolve-common-indirect-enterprise-set-up-problems"></a>Ortak dolaylı Kurumsal kurulum sorunlarını gidermek

Azure maliyeti Yönetim Portalı'nı ilk kez kullandığınızda, bir Kurumsal Anlaşma veya Bulut çözümü sağlayıcısı (CSP) kullanıcı iseniz aşağıdaki iletileri görebilirsiniz:

- *Belirtilen API anahtarını bir üst düzey kayıt anahtarı değil* görüntülenen **ayarlamak yukarı Azure Yönetimi maliyeti** Sihirbazı.
- *Doğrudan kayıt – Hayır* Kurumsal Anlaşma Portalı'nda görüntülenir.
- *Son 30 gün boyunca hiç kullanım verisi bulunamadı. Biçimlendirme Azure hesabınız için etkinleştirildiğini emin olmak için dağıtımcı temasa* Azure maliyeti Yönetim Portalı'nda görüntülenir.

Bir Azure Kurumsal anlaşmasına bir satıcı aracılığıyla ya da CSP satın önceki iletileri gösterir. Satıcınıza veya CSP etkinleştirmek gereken _biçimlendirme_ Azure hesabı için verilerinizi Azure maliyeti Yönetimi'nde görüntüleyebilmesi için.

Sorunları gidermeye yönelik şöyledir:

1. Satıcınızla etkinleştirmek gereken _biçimlendirme_ hesabınız için. Yönergeler için bkz: [dolaylı müşteri ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).
2. Azure maliyeti Management ile kullanmak için Azure Kurumsal anlaşmasına anahtar oluşturun. Yönergeler için bkz: [bir Azure Kurumsal anlaşmasına ve görünüm veri maliyet kaydetmek](https://docs.microsoft.com/en-us/azure/cost-management/quick-register-ea).

Yalnızca bir Azure hizmeti yönetici maliyet yönetimini etkinleştirebilirsiniz. Ortak yönetici izinleri yeterli değil.

Azure maliyeti yönetimini ayarlamak için Azure Enterprise sözleşmesi API anahtarı oluşturmadan önce yönergeleri izleyerek Azure faturalama API etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için raporlama API'leri genel bakış](../billing/billing-enterprise-api.md)
- [Microsoft Azure enterprise portal raporlama API](https://ea.azure.com/helpdocs/reportingAPI) altında **API veri erişimini etkinleştirme**

Departman yöneticilerinin, hesap sahipleri ve kuruluş yöneticileri izinleri vermek gerekebilecek _görüntüleyin ücret_ faturalama API ile.

## <a name="next-steps"></a>Sonraki adımlar

- İlk öğreticide maliyet yönetimi için zaten tamamlanmış yapmadıysanız, hem okuma [gözden kullanım ve maliyetleri](tutorial-review-usage.md).
