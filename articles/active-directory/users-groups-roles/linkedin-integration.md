---
title: Azure Active Directory'de LinkedIn bağlantıları tümleştirmesini etkinleştirme | Microsoft Docs
description: Etkinleştirme veya devre dışı için Azure Active Directory Microsoft uygulamalarında LinkedIn hesabı bağlantıları açıklanmaktadır
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 0eaa2656313ecd9b64503051265dc16285f0a1f3
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44392294"
---
# <a name="linkedin-account-connections"></a>LinkedIn hesabı bağlantıları

Bu makalede, etkinleştirme veya devre dışı yönetim merkezinde Azure Active Directory (Azure AD) kiracınız için LinkedIn hesabı bağlantıları öğrenebilirsiniz.

> [!IMPORTANT]
> LinkedIn hesabı bağlantıları ayarı şu anda piyasaya sunuluyor Azure AD kiracıları için. Bu Kiracı için kullanıma sunulduğunda, bu varsayılan olarak etkindir. 
> 
> Özel durumlar:
> * Ayar, US Government, Microsoft Cloud Almanya veya Azure ve Office 365'i çin'de 21Vianet tarafından işletilen Microsoft Cloud kullanan müşteriler için kullanılamıyor.
> * Ayar Almanya'da sağlanan kiracılar için varsayılan olarak kapalıdır. Ayarı, Microsoft Cloud Almanya kullanan müşteriler için kullanılabilir olmadığını unutmayın.
> * Ayar Fransa'da sağlanan kiracılar için varsayılan olarak kapalıdır.

> Yalnızca etkin varsa tümleştirme çalışır *ve* kullanıcılar, uygulamalara kendileri adına şirket verilerine erişmesini onaylamasına izin verirseniz. Onay ayarı hakkında daha fazla bilgi için bkz: [bir kullanıcının uygulamaya erişimini kaldırmak nasıl](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-in-the-azure-portal"></a>Etkinleştirmek veya devre dışı Azure portalında kiracınız için LinkedIn hesabı bağlantıları

Etkinleştirebilir veya kiracınızda LinkedIn hesabı bağlantıları yalnızca seçili kullanıcılar veya tüm kiracınız için devre dışı bırakın.

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) Azure AD kiracınız için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde **kullanıcı ayarları**.
4. Altında **LinkedIn hesabı bağlantıları**:
  * Seçin **Evet** kiracınızdaki tüm kullanıcıları için LinkedIn hesabı bağlantıları etkinleştirmek için
  * Seçin **seçili** LinkedIn etkinleştirmek için kullanıcılar yalnızca seçilen Kiracı için bağlantı hesabı
  * Seçin **Hayır** LinkedIn hesabı bağlantıları tüm kullanıcılar için devre dışı bırakmak için ![etkinleştirme LinkedIn hesabı bağlantıları](./media/linkedin-integration/linkedin-integration.png)
5. Seçerek işiniz bittiğinde, ayarlarınızı kaydetmek **Kaydet**.

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-using-group-policy"></a>Etkinleştirmek veya devre dışı Grup İlkesi kullanılarak kiracınız için LinkedIn hesabı bağlantıları

1. İndirme [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Ayıklama **ADMX** dosyaları ve bunları merkezi deponuza kopyalayın.
3. Açık Grup İlkesi Yönetimi.
4. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturmak: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016**  >  **Çeşitli** > **Göster LinkedIn özellikleri Office uygulamalarında**.
5. Seçin **etkin** veya **devre dışı bırakılmış**.
  
 Durum | Etki
------ | ------
**Etkin** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçeneklerinde seçeneği etkinleştirilmiştir. Kuruluşunuzdaki kullanıcılar, Office uygulamalarında LinkedIn özelliklerini kullanabilirsiniz.
 **Devre dışı** | **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 seçenekleri ayarı devre dışı bırakıldı ve son kullanıcılar, bu ayar değiştirilemez. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özellikleri kullanamaz.

Bu Grup İlkesi yalnızca yerel bir bilgisayar için Office 2016 uygulamaları etkiler. Bunlar LinkedIn Office 2016 uygulamalarını devre dışı olsa bile kullanıcılar Office 365 genelinde profil kartları LinkedIn özellikleri görebilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Kuruluşunuzda LinkedIn tümleştirin](linkedin-user-consent.md)

* [LinkedIn bilgilerini ve Microsoft uygulamalarınızda özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Sonraki adımlar
Azure portalında ayarlama, geçerli LinkedIn hesabı bağlantıları görmek için aşağıdaki bağlantıyı kullanın:

[LinkedIn hesabı bağlantıları yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 