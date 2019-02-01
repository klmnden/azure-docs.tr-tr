---
title: Microsoft uygulamaları - Azure Active Directory içinde LinkedIn ile tümleştirmeyi etkinleştirme | Microsoft Docs
description: Etkinleştirme veya Azure Active Directory'de Microsoft uygulamaları LinkedIn ile tümleştirmeyi devre dışı açıklanmaktadır
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 6b62fde7f9b01fbeb133c92b1306f70f56d71d7a
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509635"
---
# <a name="linkedin-integration"></a>LinkedIn ile tümleştirmeyi

Bu makalede, etkinleştirme veya devre dışı yönetim merkezinde Azure Active Directory (Azure AD) kiracınız için LinkedIn ile tümleştirmeyi öğrenebilirsiniz.

> [!IMPORTANT]
> LinkedIn tümleştirme ayarı şu anda Azure AD kiracıları için kullanıma sunulacaktır. Bu Kiracı için kullanıma sunulduğunda, bu varsayılan olarak etkindir.
> 
> Özel durumlar:
> * Ayar, US Government, Microsoft Cloud Almanya veya Azure ve Office 365'i çin'de 21Vianet tarafından işletilen Microsoft Cloud kullanan müşteriler için kullanılamıyor.
> * Ayar Almanya'da sağlanan kiracılar için varsayılan olarak kapalıdır. Ayarı, Microsoft Cloud Almanya kullanan müşteriler için kullanılabilir olmadığını unutmayın.
> * Ayar Fransa'da sağlanan kiracılar için varsayılan olarak kapalıdır.

> Yalnızca etkin varsa tümleştirme çalışır *ve* kullanıcılar, uygulamalara kendileri adına şirket verilerine erişmesini onaylamasına izin verirseniz. Onay ayarı hakkında daha fazla bilgi için bkz: [bir kullanıcının uygulamaya erişimini kaldırmak nasıl](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-the-azure-portal"></a>Etkinleştirmek veya Azure portalında kullanıcılarınız için LinkedIn ile tümleştirmeyi devre dışı

Etkinleştirebilir veya kiracınızda LinkedIn ile tümleştirmeyi tüm kiracınız için veya yalnızca seçili kullanıcılar için devre dışı bırakın.

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) Azure AD kiracınız için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde **kullanıcı ayarları**.
4. Altında **LinkedIn ile tümleştirmeyi**:
  * Seçin **Evet** kiracınızdaki tüm kullanıcıları için LinkedIn ile tümleştirmeyi etkinleştirmek için
  * Seçin **seçili** yalnızca seçilen Kiracı kullanıcılar için LinkedIn ile tümleştirmeyi etkinleştirmek için
  * Seçin **Hayır** LinkedIn ile tümleştirmeyi tüm kullanıcılar için devre dışı bırakmak için ![etkinleştirme LinkedIn tümleştirme](./media/linkedin-integration/linkedin-integration.png)
5. Seçerek işiniz bittiğinde, ayarlarınızı kaydetmek **Kaydet**.

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-group-policy"></a>Etkinleştirmek veya Grup İlkesi'nde kullanıcılarınız için LinkedIn ile tümleştirmeyi devre dışı

1. İndirme [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Ayıklama **ADMX** dosyaları ve bunları merkezi deponuza kopyalayın.
3. Açık Grup İlkesi Yönetimi.
4. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturun: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016** > **çeşitli**  >  **Göster LinkedIn özellikleri Office uygulamalarında**.
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

Geçerli LinkedIn tümleştirme ayarınız Azure portalında görmek için aşağıdaki bağlantıyı kullanın:

[Azure portalında, geçerli LinkedIn tümleştirme ayarını görüntüleyin](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
