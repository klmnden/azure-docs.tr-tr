---
title: Windows sanal masaüstü önizlemede - Azure Kiracı oluşturma
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 3d418d9f18c98e1b6fdf39924ab41dae77fba291
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204744"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü önizlemesinde bir kiracı oluşturma

Windows sanal masaüstü önizlemede Kiracı oluşturma, Masaüstü Sanallaştırma çözümünüzü oluşturmaya yönelik ilk adımdır. Bir kiracı, bir veya daha fazla konak havuzları grubudur. Birden çok oturumu konak azure'da sanal makineler olarak çalışan ve Windows sanal masaüstü hizmete kayıtlı her konak havuzu oluşur. Her konak havuzu kullanıcılara Uzak Masaüstü ve Uzaktan uygulama kaynakları yayımlamak için kullanılan bir veya daha fazla uygulama grupları da oluşur. Bir kiracı ile ana bilgisayar havuzları, uygulama grupları oluşturma, kullanıcıları atama ve hizmeti ile bağlantı kurun.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Windows sanal masaüstü hizmetine izin Azure Active Directory izinleri.
> * Bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın.
> * Bir Windows sanal masaüstü kiracısı oluşturun.

Windows sanal masaüstü Kiracı ayarlamak için ihtiyacınız olanlar aşağıda verilmiştir:

* [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Kiracı kimliği için Windows sanal masaüstü kullanıcılar.
* Azure Active Directory Kiracı içinde bir genel yönetici hesabı.
   * Bu müşterileri için bir Windows sanal masaüstü Kiracı oluşturma bulut çözümü sağlayıcısı (CSP) kuruluşlar için de geçerlidir. Bir CSP kuruluştaysanız, müşterinin Azure Active Directory örneğine genel yönetici olarak oturum açabilir olması gerekir.
   * Azure Active Directory kiracısı Windows sanal masaüstü Kiracı oluşturmaya çalıştığınız yönetici hesabı belirlenmelidir. Bu işlem, Azure Active Directory B2B desteklemiyor (konuk) hesaplar.
   * Yönetici hesabı, bir iş veya Okul hesabı olması gerekir.
* Azure aboneliği.

## <a name="grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service"></a>Windows sanal masaüstü Önizleme hizmeti verme Azure Active Directory izinleri

Zaten izinleri sanal Windows Masaüstü için bu Azure Active Directory örneğine verdiyseniz, bu bölümü atlayın.

Windows sanal masaüstü olanak tanır izinleri verme, sorgulama için Azure Active Directory Yönetim ve son kullanıcı görevleri.

Hizmet izinleri vermek için:

1. Bir tarayıcı açın ve bağlanma [Windows sanal masaüstü onay sayfası](https://rdweb.wvd.microsoft.com).
2. İçin **onay seçeneği** > **sunucu uygulaması**, dizin kimliği ve Azure Active Directory Kiracı adı girin ve ardından **Gönder**.
        
   Bulut çözümü sağlayıcısı müşterisi için müşterinin Microsoft iş ortağı portalı ID kimliğidir. Kurumsal müşteriler, kimliği altında yer alan **Azure Active Directory** > **özellikleri** > **dizin kimliği**.
3. Windows sanal masaüstü onay sayfası bir genel yönetici hesabıyla oturum açın. Örneğin, Contoso kuruluşundaki ile olsaydı, hesabınızı olabilir admin@contoso.com veya admin@contoso.onmicrosoft.com.  
4. **Kabul Et**’i seçin.
5. Bir dakika bekleyin.
6. Geri Git [Windows sanal masaüstü onay sayfası](https://rdweb.wvd.microsoft.com).
7. Git **onay seçeneği** > **istemci uygulaması**aynı Azure Active Directory Kiracı adı veya dizin kimliği girin ve ardından **Gönder**.
8. 3 adımda gibi Windows sanal masaüstü onay sayfası genel yönetici olarak oturum açın.
9. **Kabul Et**’i seçin.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>Bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın

Bir Azure Active Directory kullanıcı atama TenantCreator uygulama rolü Azure Active Directory örneği ile ilişkili bir Windows sanal masaüstü kiracısı oluşturmak bu kullanıcı sağlar. TenantCreator rol atamak için genel yönetici hesabı kullanmanız gerekir.

TenantCreator uygulama rolü atamak için:

1. Bir tarayıcı açın ve bağlanma [Azure portalında](https://portal.azure.com) genel Yönetici hesabınızla.
   
   Azure Active Directory birden çok kiracıyla çalışıyorsanız, bir özel tarayıcı oturumu açıp kopyalama ve adres çubuğuna URL'leri yapıştırmak için en iyi bir uygulamadır.
2. Azure portalında arama çubuğunda arama **kurumsal uygulamalar** altında görünen giriş seçip **Hizmetleri** kategorisi.
3. İçinde **kurumsal uygulamalar**, arama **Windows Sanal Masaüstü**. Önceki bölümdeki onay için sağlanan iki uygulama görürsünüz. Bu iki uygulamaları seçin **Windows Sanal Masaüstü**.
   !["Windows sanal masaüstü için", "Kurumsal uygulamalarında." aranırken arama sonuçlarının bir ekran görüntüsü "Windows sanal masaüstü" adlı uygulama vurgulanır.](media/tenant-enterprise-app.png)
4. **Kullanıcı ve gruplar**'ı seçin. Uygulama için izin verilen yönetici ile zaten listelendiğini görebilirsiniz **varsayılan erişim** rolü atanmamış. Bu bir Windows sanal masaüstü kiracısı oluşturmak yeterli değil. Eklemek için bu yönergeleri devam **TenantCreator** bir kullanıcıya rol.
   !["Windows sanal masaüstü" Kurumsal uygulamayı yönetmek için atanan gruplar ve kullanıcılar bir ekran görüntüsü. "Varsayılan erişim için." yalnızca bir atama ekran görüntüsü gösterir](media/tenant-default-access.png)
5. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** dikey penceresi.
6. Windows sanal masaüstü kiracınız oluşturan bir kullanıcı hesabı için arama yapın. Kolaylık olması için bu genel yönetici hesabını olabilir.

   !["TenantCreator" eklemek için bir kullanıcı seçme ekran görüntüsü](media/tenant-assign-user.png)

   > [!NOTE]
   > Bir kullanıcı (veya bir kullanıcı içeren bir grubu) seçin bu Azure Active Directory örneğinden kaynağı. Bir konuk (B2B) kullanıcı veya hizmet sorumlusu seçemezsiniz.

7. Kullanıcı hesabı seçin, **seçin** düğmesini ve ardından **atama**.
8. Üzerinde **Windows sanal masaüstü - kullanıcılar ve gruplar** sayfasında, yeni girdisiyle gördüğünüzü doğrulayın **TenantCreator** Windows sanal masaüstü Kiracı oluşturacak kullanıcıya atanan rolü.
   !["Windows sanal masaüstü" Kurumsal uygulamayı yönetmek için atanan gruplar ve kullanıcılar bir ekran görüntüsü. Ekran görüntüsü, artık "TenantCreator" rolü atanmış bir kullanıcı, ikinci bir giriş içerir.](media/tenant-tenant-creator-added.png)

Üzerinde Windows sanal masaüstü kiracınızı oluşturmak devam etmeden önce iki parça bilgi gerekir:
- Azure Active Directory Kiracı Kimliğinizi (veya **dizin kimliği**)
- Azure abonelik Kimliğinizi

Azure Active Directory Kiracı Kimliğinizi bulmak için (veya **dizin kimliği**):
1. Aynı Azure portalında oturumda, arama **Azure Active Directory** altında görünen girişi seçin ve arama çubuğunu **Hizmetleri** kategorisi.
   ![Azure Portalı'nda "Azure Active Directory" için arama sonuçlarının bir ekran görüntüsü. Arama sonucu "Hizmetler" altında vurgulanır.](media/tenant-search-azure-active-directory.png)
2. Bulana kadar aşağıya kaydırın **özellikleri**ve ardından bu seçeneği belirleyin.
3. Aranacak **dizin kimliği**ve ardından Pano simgesini seçin. Kullanabilmesi için kullanışlı bir konuma yapıştırarak olarak sonraki **AadTenantId** değeri.
   ![Azure Active Directory özelliklerinin ekran görüntüsü. Fare, kopyalamak ve yapıştırmak "dizin kimliği" için Pano simgenin üzerine geldiğinizde.](media/tenant-directory-id.png)

Azure abonelik Kimliğinizi bulmak için:
1. Aynı Azure portalında oturumda, arama **abonelikleri** altında görünen girişi seçin ve arama çubuğunu **Hizmetleri** kategorisi.
   ![Azure Portalı'nda "Azure Active Directory" için arama sonuçlarının bir ekran görüntüsü. Arama sonucu "Hizmetler" altında vurgulanır.](media/tenant-search-subscription.png)
2. Windows sanal masaüstü hizmet bildirimleri almak için kullanmak istediğiniz Azure aboneliğini seçin.
3. Aranacak **abonelik kimliği**ve bir Pano simge görünene kadar değerin üzerine gelin. Pano simgesini seçin ve bunu kullanabilmeniz için kullanışlı bir konuma yapıştırın. sonraki olarak **Azuresubscriptionıd** değeri.
   ![Azure abonelik özelliklerini görüntüsü. Fare, kopyalamak ve yapıştırmak "abonelik kimliği" için Pano simgenin üzerine geldiğinizde.](media/tenant-subscription-id.png)

## <a name="create-a-windows-virtual-desktop-preview-tenant"></a>Bir Windows sanal masaüstü Önizleme kiracısı oluşturma

Azure Active Directory'i sorgulamasına Windows sanal masaüstü hizmeti izinlerin ve TenantCreator rolü atanmış bir kullanıcı hesabına göre bir Windows sanal masaüstü kiracısı oluşturabilirsiniz.

İlk olarak, [indirin ve Windows sanal masaüstü modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Windows sanal masaüstüne Bu cmdlet ile TenantCreator kullanıcı hesabını kullanarak oturum açın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bundan sonra Azure Active Directory kiracısı ile ilişkili yeni bir Windows sanal masaüstü kiracısı oluşturun:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Köşeli parantez içindeki değerleri, kuruluşunuz ve Kiracı için uygun değerlerle değiştirin. Yeni Windows sanal masaüstü kiracınız için seçtiğiniz adın genel olarak benzersiz olmalıdır. Örneğin, Contoso kuruluş için Windows sanal masaüstü TenantCreator olduğunuzu varsayalım. Cmdlet, çalıştıracağınız şöyle görünür:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Sonraki adımlar

Kiracınızın oluşturduktan sonra bir hizmet sorumlusu Azure Active Directory'de oluşturmak ve Windows sanal masaüstü içinde bir rol atamanız gerekir. Hizmet sorumlusu, Windows sanal masaüstü Azure konak havuz oluşturmak teklifi Market'te başarılı bir şekilde dağıtmanıza olanak sağlar. Ana bilgisayar havuzları hakkında daha fazla bilgi için Windows sanal masaüstü konak havuzu oluşturmak için öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [PowerShell ile hizmet sorumluları ve rol atamaları oluşturma](./create-service-principal-role-powershell.md)
