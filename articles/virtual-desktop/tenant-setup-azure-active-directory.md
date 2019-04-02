---
title: Windows sanal masaüstü önizlemede - Azure Kiracı oluşturma
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 1c66b3de9e18cb74c43f20499e4065c7ec7ae5ca
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58801690"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü önizlemesinde bir kiracı oluşturma

Windows sanal masaüstü önizlemede Kiracı oluşturma, Masaüstü Sanallaştırma çözümünüzü oluşturmaya yönelik ilk adımdır. Bir kiracı, bir veya daha fazla konak havuzları grubudur. Birden çok oturumu konak azure'da sanal makineler olarak çalışan ve Windows sanal masaüstü hizmete kayıtlı her konak havuzu oluşur. Her konak havuzu kullanıcılara Uzak Masaüstü ve Uzaktan uygulama kaynakları yayımlamak için kullanılan bir veya daha fazla uygulama grupları da oluşur. Bir kiracı ile konak havuzları, uygulama grupları oluşturma, kullanıcıları atama ve hizmeti ile bağlantı kurun.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

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
* Azure abonelik kimliği

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
7. Git **onay seçeneği** > **istemci uygulaması**, dizin kimliği ve aynı Azure AD Kiracı adı girin ve ardından seçin **Gönder**.
8. Windows sanal masaüstü onay sayfasına geri adım 3'te yaptığınız gibi genel yönetici olarak oturum açın.
9. **Kabul Et**’i seçin.

## <a name="assign-the-tenantcreator-application-role-to-a-user-in-your-azure-active-directory-tenant"></a>Bir kullanıcı Azure Active Directory kiracınızdaki TenantCreator uygulama rolü atayın

Bir Azure Active Directory kullanıcı atama TenantCreator uygulama rolü Azure Active Directory ile ilişkili bir Windows sanal masaüstü kiracısı oluşturmak bu kullanıcı sağlar. TenantCreator rol atamak için genel yönetici hesabı kullanmanız gerekir.

Genel Yönetici hesabınızla TenantCreator uygulama rolü atamak için:

1. Bir tarayıcı açın ve bağlanma [Azure Active Directory portalında](https://aad.portal.azure.com) genel Yönetici hesabınızla.
   - Birden çok Azure AD kiracıları ile çalışıyorsanız, bu URL'leri adrese yapıştırın ve bir özel tarayıcı oturumu açıp kopyalama en iyi uygulamadır.
2. Seçin **kurumsal uygulamalar**, arama **Windows Sanal Masaüstü**. Önceki bölümdeki onay için sağlanan iki uygulama görürsünüz. Bu iki uygulamaları seçin **Windows Sanal Masaüstü**.
3. Seçin **kullanıcılar ve gruplar**, ardından **Kullanıcı Ekle**.
4. Kullanıcıları ve grupları seçin **atama Ekle** dikey penceresi
5. Windows sanal masaüstü kiracınız oluşturan bir kullanıcı hesabı için arama yapın.
   - Kolaylık olması için bu genel yönetici hesabını olabilir.
6. Kullanıcı hesabı seçin, **seçin** düğmesini ve ardından **atama**.

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

Köşeli parantez içindeki değerleri, kuruluşunuz ve Kiracı için geçerli değerleri ile değiştirilmelidir. Örneğin, Contoso kuruluş için Windows sanal masaüstü TenantCreator olduğunuzu varsayalım. Cmdlet, çalıştıracağınız şöyle görünür:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

## <a name="next-steps"></a>Sonraki adımlar

Kiracınızın oluşturduktan sonra konak havuzu olmak gerekir. Ana bilgisayar havuzları hakkında daha fazla bilgi için Windows sanal masaüstü konak havuzu oluşturmak için öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Windows sanal masaüstü konak havuzu Öğreticisi](./create-host-pools-azure-marketplace.md)
