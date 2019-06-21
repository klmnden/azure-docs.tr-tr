---
title: LinkedIn hesabı bağlantıları - Azure Active Directory için yönetici onayı | Microsoft Docs
description: Etkinleştirme veya devre dışı Azure Active Directory'de Microsoft uygulamalarında LinkedIn tümleştirme hesabı bağlantıları açıklanmaktadır
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/29/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36ca46d6df9c32f23d3051d1205c3c6b39e69f5a
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164703"
---
# <a name="integrate-linkedin-account-connections-in-azure-active-directory"></a>Azure Active Directory'de LinkedIn hesabı bağlantıları tümleştirin

Bazı Microsoft uygulamaları içinde LinkedIn bağlantılarını erişmek için kuruluşunuzdaki kullanıcıların izin verebilirsiniz. Veri yok, kullanıcıların hesaplarıyla bağlantı onay kadar paylaşılır. Kuruluşunuz Azure Active Directory (Azure AD) tümleştirebilirsiniz [Yönetim Merkezi](https://aad.portal.azure.com).

> [!IMPORTANT]
> LinkedIn hesabı bağlantıları ayarı şu anda piyasaya sunuluyor kuruluşlara Azure AD. Bunu kuruluşunuza kullanıma sunulduğunda, bu varsayılan olarak etkindir.
> 
> Özel durumlar:
> * Ayar, US Government, Microsoft Cloud Almanya veya Azure ve Office 365'i çin'de 21Vianet tarafından işletilen Microsoft Cloud kullanan müşteriler için kullanılamıyor.
> * Ayar Almanya'da sağlanan kiracılar için varsayılan olarak kapalıdır. Ayarı, Microsoft Cloud Almanya kullanan müşteriler için kullanılabilir olmadığını unutmayın.
> * Ayar Fransa'da sağlanan kiracılar için varsayılan olarak kapalıdır.
>
> Kuruluşunuz için LinkedIn hesabı bağlantıları etkinleştirildikten sonra kullanıcılar, uygulamalara kendileri adına şirket verilerine erişme izni sonra hesabı bağlantıları çalışmaz. Kullanıcı onay ayarı hakkında daha fazla bilgi için bkz: [bir kullanıcının uygulamaya erişimini kaldırmak nasıl](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-linkedin-account-connections-in-the-azure-portal"></a>Azure portalında LinkedIn hesabı bağlantıları etkinleştir

LinkedIn hesabı bağlantıları yalnızca yalnızca seçili kullanıcılar, kuruluşunuzdaki tüm kuruluşunuzdan erişmesini istediğiniz kullanıcıları için etkinleştirebilirsiniz.

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com/) Azure AD kuruluş için genel yönetici olan bir hesapla.
1. Seçin **kullanıcılar**.
1. Üzerinde **kullanıcılar** dikey penceresinde **kullanıcı ayarları**.
1. Altında **LinkedIn hesabı bağlantıları**, bazı Microsoft uygulamaları içinde LinkedIn bağlantılarını erişmek için kullanıcıların hesaplarını bağlamaları açmasına imkan tanıyın. Veri yok, kullanıcıların hesaplarıyla bağlantı onay kadar paylaşılır.

    * Seçin **Evet** kuruluşunuzdaki tüm kullanıcılar için hizmetini etkinleştirmek için
    * Seçin **seçili grup** yalnızca seçili kullanıcılar, kuruluşunuzda bir grup hizmeti etkinleştirmek için
    * Seçin **Hayır** kuruluşunuzdaki tüm kullanıcıların izninizi geri almak için

    ![LinkedIn hesabı bağlantıları kuruluştaki tümleştirin](./media/linkedin-integration/linkedin-integration.png)

1. İşiniz bittiğinde **Kaydet** ayarlarınızı kaydetmek için.

> [!Important]
> Kullanıcıların hesaplarını bağlamaları onay kadar LinkedIn ile tümleştirmeyi kullanıcılarınız için tam olarak etkinleştirilmedi. Kullanıcılarınız için hesabı bağlantıları etkinleştirdiğinizde veri paylaşılır.

### <a name="assign-selected-users-with-a-group"></a>Seçili kullanıcılar bir grup atama
Biz bağlanabilmesine birçok bireysel kullanıcılar yerine tek bir grup için LinkedIn ile Microsoft hesaplarını etkinleştirmek üzere bir kullanıcı grubu tercih yapma seçeneğine sahip kullanıcıların listesini belirtir 'Seçili' seçeneği yerini almıştır. LinkedIn hesabı bağlantıları seçilen bireysel kullanıcılar için etkin yoksa, herhangi bir şey gerekmez. LinkedIn hesabı bağlantıları seçili bireysel kullanıcılar için daha önce etkinleştirdiyseniz, aşağıdakileri yapmalısınız:

1. Bireysel kullanıcılardan oluşan geçerli listesini alma
1. Şu anda etkin tek tek kullanıcılar grubuna Taşı
1. Önceki gruptan seçili gruptaki ayarlama Azure AD Yönetim merkezinde LinkedIn hesabı bağlantıları kullanın.

> [!NOTE]
> Şu anda seçili, bireysel kullanıcılar grubuna taşıma olsa bile, yine de Microsoft uygulamalarında LinkedIn bilgilerini görebilir.

### <a name="get-the-current-list-of-selected-users"></a>Seçili kullanıcıları geçerli listesini alın

1. Microsoft 365'te Yönetici hesabınızla oturum açın.
1. https://linkedinselectedusermigration.azurewebsites.net/ kısmına gidin. LinkedIn hesabı bağlantıları için seçili kullanıcıların listesini görürsünüz.
1. Listeyi bir CSV dosyasına dışarı aktarın.

### <a name="move-the-currently-selected-individual-users-to-a-group"></a>Şu anda seçili bireysel kullanıcılar grubuna Taşı

1. PowerShell’i başlatın
1. Çalıştırarak Azure AD modülünü yükleme `Install-Module AzureAD`
1. Şu betiği çalıştırın:

  ``` PowerShell
  $groupId = "GUID of the target group"
  
  $users = Get-Content 
  Path to the CSV file
  
  $i = 1
  foreach($user in $users} { Add-AzureADGroupMember -ObjectId $groupId -RefObjectId $user ; Write-Host $i Added $user ; $i++ ; Start-Sleep -Milliseconds 10 }
  ```

Azure AD Yönetim merkezinde ayarı LinkedIn hesabı bağlantıları seçili gruptaki iki adımı grubundan kullanmak için bkz. [etkinleştirme LinkedIn hesabı bağlantıları Azure portalında](#enable-linkedin-account-connections-in-the-azure-portal).

## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>LinkedIn hesabı bağlantıları etkinleştirmek için Grup İlkesi'ni kullanın

1. İndirme [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
1. Ayıklama **ADMX** dosyaları ve bunları merkezi deponuza kopyalayın.
1. Açık Grup İlkesi Yönetimi.
1. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturun: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016** > **çeşitli**  >  **Göster LinkedIn özellikleri Office uygulamalarında**.
1. Seçin **etkin** veya **devre dışı bırakılmış**.
  
   Eyalet | Etki
   ------ | ------
   **Etkin** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçeneklerinde seçeneği etkinleştirilmiştir. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özelliklerini kullanabilirsiniz.
   **Devre dışı** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçenekleri ayarı devre dışı bırakıldı ve son kullanıcılar, bu ayar değiştirilemez. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özellikleri kullanamaz.

Bu Grup İlkesi yalnızca yerel bir bilgisayar için Office 2016 uygulamaları etkiler. Kullanıcılar, Office 2016 uygulamalarında LinkedIn devre dışı bırakırsanız, LinkedIn özellikleri Office 365'te yine de görebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı onayı ve paylaşımı için LinkedIn verileri](linkedin-user-consent.md)

* [LinkedIn bilgilerini ve Microsoft uygulamalarınızda özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

* [Azure portalında, geçerli LinkedIn tümleştirme ayarını görüntüleyin](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
