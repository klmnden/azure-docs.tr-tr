---
title: Windows sanal masaüstü önizlemede - Azure Kiracı oluşturma
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 2c9682746201306f1b99a04462819618225caa11
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164256"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü önizlemesinde bir kiracı oluşturma

Windows sanal masaüstü önizlemede Kiracı oluşturma, Masaüstü Sanallaştırma çözümünüzü oluşturmaya yönelik ilk adımdır. Bir kiracı, bir veya daha fazla konak havuzları grubudur. Birden çok oturumu konak azure'da sanal makineler olarak çalışan ve Windows sanal masaüstü hizmete kayıtlı her konak havuzu oluşur. Her konak havuzu kullanıcılara Uzak Masaüstü ve Uzaktan uygulama kaynakları yayımlamak için kullanılan bir veya daha fazla uygulama grupları da oluşur. Bir kiracı ile konak havuzları, uygulama grupları oluşturma, kullanıcıları atama ve hizmeti ile bağlantı kurun.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Windows sanal masaüstü hizmetine izin Azure Active Directory izinleri.
> * Bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın.
> * Bir Windows sanal masaüstü kiracısı oluşturun.

Windows sanal masaüstü Kiracı ayarlamak için ihtiyacınız olanlar aşağıda verilmiştir:

* [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Kiracı kimliği için Windows sanal masaüstü kullanıcılar.
* Azure Active Directory Kiracı içinde bir genel yönetici hesabı.
   * Bu bulut çözümü sağlayıcısı (CSP) kuruluşlar için müşterilere bir Windows sanal masaüstü Kiracı oluşturma için de geçerlidir. CSP kuruluşuysanız, müşterinin Azure Active Directory genel Yöneticisi olarak oturum açamıyor olması gerekir.
   * Azure Active Directory kiracısı Windows sanal masaüstü Kiracı oluşturmaya çalıştığınız yönetici hesabı belirlenmelidir. Bu işlem, Azure Active Directory B2B desteklemiyor (konuk) hesaplar.
   * Yönetici hesabı, bir iş veya Okul hesabı olması gerekir.
* Bir Azure aboneliği

## <a name="grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service"></a>Windows sanal masaüstü Önizleme hizmeti verme Azure Active Directory izinleri

Zaten izinleri sanal Windows Masaüstü için bu Azure Active Directory verdiyseniz, bu bölümü atlayın.

Windows sanal masaüstü izinleri verme, sağlar, Azure Active Directory'niz için sorgu yönetim ve son kullanıcı görevleri.

Hizmet izinleri vermek için:

1. Bir tarayıcı açın ve bağlanma [Windows sanal masaüstü onay sayfası](https://rdweb.wvd.microsoft.com).
2. İçin **onay seçeneği** > **sunucu uygulaması**, dizin kimliği ve Azure Active Directory Kiracı adı girin ve ardından seçin **Gönder**.
        Bulut çözümü sağlayıcısı müşterisi için müşterinin Microsoft iş ortağı portalı ID kimliğidir. Kurumsal müşteriler, kimliği altında yer alan **Azure Active Directory** > **özellikleri** > **dizin kimliği**.
3. Windows sanal masaüstü onay sayfası bir genel yönetici hesabıyla oturum açın. Örneğin, Contoso kuruluşundaki ile olsaydı, hesabınızı olabilir admin@contoso.com veya admin@contoso.onmicrosoft.com.  
4. **Kabul Et**’i seçin.
5. Bir dakika bekleyin.
6. Geri gidin [Windows sanal masaüstü onay sayfası](https://rdweb.wvd.microsoft.com).
7. Git **onay seçeneği** > **istemci uygulaması**, aynı Azure Active Directory Kiracı adı veya dizin kimliği girin ve ardından seçin **Gönder**.
8. Windows sanal masaüstü onay sayfasına geri adım 3'te yaptığınız gibi genel yönetici olarak oturum açın.
9. **Kabul Et**’i seçin.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>Bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın

Bir Azure Active Directory kullanıcı atama TenantCreator uygulama rolü Azure Active Directory ile ilişkili bir Windows sanal masaüstü kiracısı oluşturmak bu kullanıcı sağlar. TenantCreator rol atamak için genel yönetici hesabı kullanmanız gerekir.

Genel Yönetici hesabınızla TenantCreator uygulama rolü atamak için:

1. Bir tarayıcı açın ve bağlanma [Azure portalında](https://portal.azure.com) genel Yönetici hesabınızla.
   - Azure Active Directory birden çok kiracıyla çalışıyorsanız, bir özel tarayıcı oturumu açıp kopyalama ve URL'leri adres çubuğuna yapıştırın en iyi uygulamadır.
2. Azure portalında arama çubuğunda arama **kurumsal uygulamalar** altında görünen giriş seçip **Hizmetleri** kategorisi.
3. İçinde **kurumsal uygulamalar**, arama **Windows Sanal Masaüstü**. Önceki bölümdeki onay için sağlanan iki uygulama görürsünüz. Bu iki uygulamaları seçin **Windows Sanal Masaüstü**.
        !["Windows sanal masaüstü", "Kurumsal uygulamalar" aranırken arama sonuçlarının bir ekran görüntüsü. "Windows sanal masaüstü" adlı uygulama vurgulanır.](media/tenant-enterprise-app.png)
4. **Kullanıcı ve gruplar**'ı seçin. Uygulama için izin verilen yönetici ile zaten listelendiğini görebilirsiniz **varsayılan erişim** rolü atanmamış. Bu bir Windows sanal masaüstü kiracısı oluşturmak yeterli değil. Eklemek için bu yönergeleri devam **TenantCreator** bir kullanıcıya rol.
        !["Windows sanal masaüstü" Kurumsal uygulamayı yönetmek için atanan gruplar ve kullanıcılar bir ekran görüntüsü. Ekran görüntüsü "İçin erişim varsayılan" olan yalnızca bir atama gösterir.](media/tenant-default-access.png)
5. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** dikey penceresi.
6. Windows sanal masaüstü kiracınız oluşturan bir kullanıcı hesabı için arama yapın. Kolaylık olması için bu genel yönetici hesabını olabilir.

    !["TenantCreator" eklemek için bir kullanıcı seçerek bir ekran görüntüsü.](media/tenant-assign-user.png)

   > [!NOTE]
   > Bir kullanıcı (veya bir kullanıcı içeren bir grubu) seçin bu Azure Active Directory'den kaynağı. Bir konuk (B2B) kullanıcı veya hizmet sorumlusu seçemezsiniz.

7. Kullanıcı hesabı seçin, **seçin** düğmesini ve ardından **atama**.
8. Üzerinde **Windows sanal masaüstü - kullanıcılar ve gruplar** sayfasında, yeni girdisiyle gördüğünüzü doğrulayın **TenantCreator** Windows sanal masaüstü Kiracı oluşturacak kullanıcıya atanan rolü.
        !["Windows sanal masaüstü" Kurumsal uygulamayı yönetmek için atanan gruplar ve kullanıcılar bir ekran görüntüsü. Ekran görüntüsü, artık "TenantCreator" rolü atanmış bir kullanıcı, ikinci bir giriş içerir.](media/tenant-tenant-creator-added.png)

Üzerinde Windows sanal masaüstü kiracınızı oluşturmak devam etmeden önce iki parça bilgi gerekir:
- Azure Active Directory Kiracı Kimliğinizi (veya **dizin kimliği**)
- Azure abonelik Kimliğinizi

Azure Active Directory Kiracı Kimliğinizi bulmak için (veya **dizin kimliği**):
1. Aynı Azure portalında oturumda, arama **Azure Active Directory** altında görünen girişi seçin ve arama çubuğunu **Hizmetleri** kategorisi.
        ![Azure Portalı'nda "Azure Active Directory" için arama sonuçlarının bir ekran görüntüsü. Arama sonucu "Hizmetler" altında vurgulanır.](media/tenant-search-azure-active-directory.png)
2. Bulana kadar aşağıya kaydırın **özellikleri**, ardından bu seçeneği belirleyin.
3. Aranacak **dizin kimliği**, ardından Pano simgesini seçin. Yapıştırın bir, kullanabilmesi için kullanışlı bir konum olarak sonraki **AadTenantId**.
        ![Azure Active Directory özelliklerinin ekran görüntüsü. Fare, "Dizinine kopyalamak ve yapıştırmak kimliği" Pano simgenin üzerine geldiğinizde.](media/tenant-directory-id.png)

Azure abonelik Kimliğinizi bulmak için:
1. Aynı Azure portalında oturumda, arama **abonelikleri** altında görünen girişi seçin ve arama çubuğunu **Hizmetleri** kategorisi.
        ![Azure Portalı'nda "Azure Active Directory" için arama sonuçlarının bir ekran görüntüsü. Arama sonucu "Hizmetler" altında vurgulanır.](media/tenant-search-subscription.png)
2. Windows sanal masaüstü hizmet bildirimleri almak için kullanmak istediğiniz Azure aboneliğini seçin.
3. Aranacak **abonelik kimliği**, Pano simge görünene kadar değerin üzerine gelin. Pano simgesini seçin ve bunu kullanabilmeniz için kullanışlı bir konuma yapıştırın. sonraki olarak **Azuresubscriptionıd**.
        ![Azure abonelik özelliklerini görüntüsü. Fare, "abonelik kopyalamak ve yapıştırmak kimliği için" Pano simgenin üzerine geldiğinizde.](media/tenant-subscription-id.png)

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Bir Windows sanal masaüstü Önizleme kiracısı oluşturma

Azure Active Directory sorgulamak için Windows sanal masaüstü hizmet izinleri verildi ve TenantCreator rolü atanmış bir kullanıcı hesabına göre bir Windows sanal masaüstü kiracısı oluşturabilirsiniz.

İlk olarak, [indirin ve Windows sanal masaüstü modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Windows sanal masaüstünü Bu cmdlet ile TenantCreator kullanıcı hesabını kullanarak oturum açın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bundan sonra Azure Active Directory kiracısı ile ilişkili yeni bir Windows sanal masaüstü kiracısı oluşturun:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Köşeli parantez içindeki değerleri, kuruluşunuz ve Kiracı için geçerli değerleri ile değiştirilmelidir. Yeni Windows sanal masaüstü kiracınız için seçtiğiniz adın genel olarak benzersiz olmalıdır. Örneğin, Contoso kuruluş için Windows sanal masaüstü TenantCreator olduğunuzu varsayalım. Cmdlet, çalıştıracağınız şöyle görünür:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Sonraki adımlar

Kiracınızın oluşturduktan sonra Azure Active Directory'de Hizmet sorumlusu oluşturma ve Windows sanal masaüstü içinde bir rol atamanız gerekir. Hizmet sorumlusu, Windows sanal masaüstü Azure konak havuz oluşturmak teklifi Market'te başarılı bir şekilde dağıtmanıza olanak sağlar. Ana bilgisayar havuzları hakkında daha fazla bilgi için Windows sanal masaüstü konak havuzu oluşturmak için öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [PowerShell ile hizmet sorumluları ve rol atamaları oluşturma](./create-service-principal-role-powershell.md)
