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
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e07c53192ea2c8b792256af944c81c9c909dc55
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368135"
---
# <a name="consent-to-linkedin-account-connections-for-an-azure-active-directory-organization"></a>Bir Azure Active Directory kuruluş için LinkedIn hesabı bağlantıları kabul edersiniz

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

## <a name="use-the-azure-portal-to-enable-linkedin-account-connections"></a>LinkedIn hesabı bağlantıları etkinleştirmek için Azure portalını kullanma

LinkedIn hesabı bağlantıları yalnızca yalnızca seçili kullanıcılar, kuruluşunuzdaki tüm kuruluşunuzdan erişmesini istediğiniz kullanıcıları için etkinleştirebilirsiniz.

1. Oturum [Azure AD yönetim merkezini](https://aad.portal.azure.com/) Azure AD kuruluş için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde **kullanıcı ayarları**.
4. Altında **LinkedIn hesabı bağlantıları**, bazı Microsoft uygulamaları içinde LinkedIn bağlantılarını erişmek için kullanıcıların hesaplarını bağlamaları açmasına imkan tanıyın. Veri yok, kullanıcıların hesaplarıyla bağlantı onay kadar paylaşılır.

  * Seçin **Evet** kuruluştaki tüm kullanıcılar için hizmetine onay verme
  * Seçin **seçili** kuruluştaki yalnızca seçili kullanıcılar için onay verme
  * Seçin **Hayır** kuruluşunuzdaki kullanıcılar için izninizi geri almak için

    ![LinkedIn hesabı bağlantıları kuruluştaki tümleştirin](./media/linkedin-integration/linkedin-integration.png)

5. İşiniz bittiğinde **Kaydet** ayarlarınızı kaydetmek için.
     
## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>LinkedIn hesabı bağlantıları etkinleştirmek için Grup İlkesi'ni kullanın

1. İndirme [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Ayıklama **ADMX** dosyaları ve bunları merkezi deponuza kopyalayın.
3. Açık Grup İlkesi Yönetimi.
4. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturun: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016** > **çeşitli**  >  **Göster LinkedIn özellikleri Office uygulamalarında**.
5. Seçin **etkin** veya **devre dışı bırakılmış**.
  
   Durum | Etki
   ------ | ------
   **Etkin** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçeneklerinde seçeneği etkinleştirilmiştir. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özelliklerini kullanabilirsiniz.
   **Devre dışı** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçenekleri ayarı devre dışı bırakıldı ve son kullanıcılar, bu ayar değiştirilemez. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özellikleri kullanamaz.

Bu Grup İlkesi yalnızca yerel bir bilgisayar için Office 2016 uygulamaları etkiler. Kullanıcılar, Office 2016 uygulamalarında LinkedIn devre dışı bırakırsanız, LinkedIn özellikleri Office 365'te yine de görebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı onayı ve paylaşımı için LinkedIn verileri](linkedin-user-consent.md)

* [LinkedIn bilgilerini ve Microsoft uygulamalarınızda özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

* [Azure portalında, geçerli LinkedIn tümleştirme ayarını görüntüleyin](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
