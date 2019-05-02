---
title: Dynamics CRM | Azure Market
description: Dynamics CRM için sağlama yönetimi yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pabutler
ms.openlocfilehash: 6fdab26bb5a4da5402a3a0a895a7c8835ef22c2f
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935791"
---
# <a name="configure-lead-management-for-dynamics-crm-online"></a>Çevrimiçi sağlama Yönetimi Dynamics CRM için yapılandırma

Bu makalede, Dynamics CRM Online satış fırsatlarını işlemek için nasıl ayarlanacağı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki kullanıcı izinleri bu makaledeki adımları tamamlamak için gerekli olmayan:
- Dynamics CRM Online örneğiniz çözümü yüklemek için bir yönetici olmanız gerekir.
- Sağlama hizmeti için yeni bir hizmet hesabı oluşturmak için bir kiracı yöneticisi olmanız gerekir.

<a name="install-the-solution"></a>Çözüm yükleme
--------------------

1.  İndirme [Microsoft Market neden yazıcı çözüm](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) ve yerel olarak kaydedin.

2.  Dynamics CRM Online'ı açın ve Ayarlar'a gidin.
    >[!NOTE]
    >Ardından sonraki ekran görüntüsünde seçenekleri görmüyorsanız, ihtiyaç duyduğunuz izinleri yok.
 
       ![Dynamics Kurulum görüntüle](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline1.png)

3.  Seçin **alma**ve ardından 1. adımda indirdiğiniz çözümü seçin.
 
    ![Dynamics içeri aktarma seçeneği](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline2.png)

4.  Çözüm yüklemeyi tamamlayın.

## <a name="configure-user-permissions"></a>Kullanıcı izinleri yapılandırma

Yazmak için bir hizmet hesabı bizimle paylaşın ve hesap izinlerini yapılandırmak için kullandığınız Dynamics CRM Örneğinizdeki içine yol açar.

Hizmet hesabı oluşturma ve izinleri atamak için aşağıdaki adımları kullanın. Kullanabileceğiniz **Azure Active Directory** veya **Office 365**.

### <a name="azure-active-directory"></a>Azure Active Directory

Hiçbir zaman müşteri adayları almaya devam etmek için kullanıcı adı/parola güncelleştirilmesi gerekmeyen eklenen avantajı aldığından bu seçenek önerilir. Azure Active Directory bu seçeneği kullanmak için uygulamanızı Active Directory Uygulama kimliği, uygulama anahtarı ve dizin kimliği sağlayın.

Azure Active Directory için Dynamics CRM yapılandırmak için aşağıdaki adımları kullanın.

1.  Oturum [Azure portalında](https://portal.azure.com/) ve Azure Active Directory hizmetini seçin.

2.  Seçin **özellikleri** kopyalayın **dizin kimliği**. Bulut iş ortağı Portalı'nda ihtiyacınız Kiracı hesabı kimlik bilgileriniz budur.

    ![Dizin kimliği Al](./media/cloud-partner-portal-lead-management-instructions-dynamics/directoryid.png)

3.  Seçin **uygulama kayıtları**ve ardından **yeni uygulama kaydı**.
4.  Uygulama adı girin.
5.  Türü için **Web uygulaması / API**.
6.  Bir URL sağlayın. Bu alan, müşteri adayları için gerekli değildir, ancak bir uygulama oluşturmak için gereklidir.
7. **Oluştur**’u seçin.
8.  Uygulamanız kaydedilir, seçin **özellikleri** seçip **uygulama kimliğini kopyalama**. Bulut iş ortağı Portalı'nda bu bağlantı bilgilerini kullanacaksınız.
9.  Özellikleri, uygulama, çok kiracılı olarak ayarlayın ve ardından **Kaydet**.

10. Seçin **anahtarları** ve ayarlamak süre ile yeni bir anahtar oluşturmak *her zaman geçerli olsun*. Seçin **Kaydet** anahtarı oluşturulamadı. 
11. Anahtarları menüsünde **anahtar değerini kopyalayın.** Bulut iş ortağı portalı için gerekeceği için bu değer bir kopyasını kaydedin.
    
    ![Dynamics kayıtlı anahtarını Al](./media/cloud-partner-portal-lead-management-instructions-dynamics/registerkeys.png)
    
12. Seçin **gerekli izinler** seçip **Ekle**. 
13. Seçin **Dynamics CRM Online** yeni API olarak ve izinlerini denetleyin *kuruluş kullanıcıları olarak erişim CRM Online*.

14. Dynamics CRM, kullanıcılar'a gidin ve geçiş yapmak için "Etkin kullanıcılar" açılan menüyü seçin **uygulama kullanıcıları**.
    
    ![Uygulama kullanıcıları](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuserfirst.PNG)

15. Seçin **yeni** yeni bir kullanıcı oluşturun. Seçin **kullanıcı: Uygulama kullanıcısı** açılır.
    
    ![Yeni Uygulama kullanıcısı ekleyin](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuser.PNG)

16. İçinde **yeni kullanıcı**adı sağlayın ve bu bağlantı ile kullanmak istediğiniz e-posta. Yapıştırın **uygulama kimliği** Azure portalında oluşturduğunuz uygulama için.

     ![Yeni kullanıcı yapılandırma](./media/cloud-partner-portal-lead-management-instructions-dynamics/leadgencreateuser.PNG)

17. Bu kullanıcı için bir bağlantı yapılandırmayı tamamlamak için bu makaledeki "Güvenlik ayarları" gidin.

### <a name="office-365"></a>Office 365

Azure Active Directory kullanmak istemiyorsanız, üzerinde yeni bir kullanıcı kaydedebilirsiniz *Microsoft 365 Yönetim merkezini*. Kullanıcı adı/parola, müşteri adaylarını almaya devam etmek için her 90 günde güncelleştirmek için gerekli olacaktır.

Office 365, Dynamics CRM için yapılandırmak için aşağıdaki adımları kullanın.

1. Oturum [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com).

2. Seçin **yönetici** Döşe.

    ![Office Online Yönetim](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline3.png)

3. Seçin **kullanıcı ekleme**.

    ![Kullanıcı ekleme](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline4.png)

4. Müşteri adayı yazıcı hizmeti için yeni bir kullanıcı oluşturun. Aşağıdaki ayarları yapılandırın:

    -   Bir parola girin ve "Bu kullanıcı ilk kez oturum açarken parola değiştirmesi olun" seçeneğinin işaretini kaldırın.
    -   "Kullanıcı (yönetici erişimi yok)" kullanıcı için rolü seçin.
    -   Sonraki ekran görüntüsünde gösterilen ürün lisansını seçin. Seçtiğiniz lisans için ücret ödersiniz. Çözüm, Dynamics CRM Online Basic lisansı ile de çalışır.
    
    ![Kullanıcı izinleri ve lisans yapılandırın](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline5.png)

## <a name="security-settings"></a>Güvenlik ayarları

Son adım, müşteri adaylarını yazmak için oluşturulan kullanıcı etkinleştirmektir.

1.  Dynamics CRM online oturum açın.
2.  Üzerinde **ayarları**seçin **güvenlik**.
    
    ![Güvenlik ayarları](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline6.png)

3.  Oluşturduğunuz kullanıcıyı seçin **kullanıcı izinleri**ve ardından **kullanıcı rollerini Yönet**. Denetleme **Microsoft Market neden yazıcı** rol atamak için.

    ![Kullanıcı rolü atama](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline7.png)\

    >[!NOTE]
    >Bu rol, içeri aktarılan ve yalnızca müşteri adaylarını yazmak ve uyumluluğu sağlamak için çözüm sürümünü izlemek için izinleri olan çözüm tarafından oluşturulur.

4.  Güvenlik, seçin **güvenlik rolleri** ve müşteri adayı Microsoft Market yazıcı için rol bulur.
    
    ![Güvenlik sağlama yazıcı yapılandırın](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline10.jpg)\

5. Seçin **temel kayıtlar** sekmesi. Oluşturma/okuma/yazma kullanıcı varlığı için kullanıcı Arabirimi sağlar.

    ![Oluşturma/okuma/yazma için kullanıcı etkinleştir](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline11.jpg)\

## <a name="wrap-up"></a>Kaydır

Dynamics CRM, bulut iş ortağı portalı için oluşturulmuş hesap bilgilerini ekleyerek sağlama yönetimi için yapılandırma tamamlayın. Örneğin:

-   **Azure Active Directory** - **uygulama kimliği** (örnek: *23456052-AAAA-bbbb-8662-1234df56788f*), **dizin kimliği** (örnek: *12345678-8af1-4asf-1234-12234d01db47*), ve **uygulama anahtarı** (örnek: *1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=*).
-   **Office 365** - **Url** (örnek: *https://contoso.crm4.dynamics.com*), **kullanıcı adı** (örnek: *contoso\@ contoso.onmicrosoft.com*), ve **parola** (örnek: *P\@ssw0rd*).
