---
title: "Etkinleştirmek veya Office uygulamaları için Azure Active Directory'de LinkedIn tümleştirme devre dışı bırakma | Microsoft Docs"
description: "Etkinleştirmek veya devre dışı LinkedIn tümleştirme Azure Active Directory'de Microsoft uygulamaları için açıklanmaktadır"
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: cdfb5458b020e9d3a3f33cecbeb0ee7b9a48909d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="linkedin-integration-for-office-applications"></a>Office uygulamaları için LinkedIn tümleştirme
Bu makalede Azure Active Directory (Azure AD) LinkedIn tümleştirme sağlanan kullanıcıların kısıtlamak nasıl bildirir. Bazı Microsoft uygulamalarını içinde genel LinkedIn verilere erişmesine olanak tanır, Kiracı eklendiğinde LinkedIn tümleştirme varsayılan olarak etkindir. Her bir kullanıcı, iş veya Okul hesabı LinkedIn hesaplarında bağlanmak bağımsız olarak seçebilirsiniz.

> [!IMPORTANT]
> LinkedIn tümleştirme tüm Azure AD kiracılarıyla aynı anda dağıtıldığı değil. Azure kiracınız alındı, sonra LinkedIn tümleştirme varsayılan olarak etkindir. LinkedIn tümleştirme yerel Git, sovereign ve kamu kiracılar için kullanılabilir değil. Ürün bilgileri güncel bir görünümü için bkz: [Office 365 yol haritası](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) sayfası.

## <a name="linkedin-integration-from-the-user-perspective"></a>Kullanıcı açısından LinkedIn tümleştirme
Kuruluşunuzdaki kullanıcılar kendi işlerine LinkedIn hesabı veya Okul hesabı bağladığınızda [veri sağlamak LinkedIn izin](https://www.linkedin.com/help/linkedin/answer/84077) Microsoft uygulamaları ve kuruluşunuzun sağladığı hizmetler'kullanılacak. [Kullanıcıların hesaplarını bağlantısını kesmek](https://www.linkedin.com/help/linkedin/answer/85097), Microsoft ile veri paylaşımı LinkedIn izinlerini kaldırır. LinkedIn tümleştirme genel kullanıma açık LinkedIn profil bilgilerini kullanır. [Kullanıcılar kendi LinkedIn profilini nasıl görüntülenen denetleyebilir](https://www.linkedin.com/help/linkedin/answer/83) profillerini Microsoft uygulamalarındaki görüntülenebilir olup olmadığını da dahil olmak üzere LinkedIn gizlilik ayarları kullanarak.

## <a name="privacy-considerations"></a>Gizlilik konuları
Azure AD'de LinkedIn Integration etkinleştirme Microsoft uygulamaları ve kullanıcılarınızın LinkedIn bilgilerin bazıları erişmek için hizmetler sağlar. Okuma [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement/) LinkedIn tümleştirme Azure AD'de etkinleştirirken gizlilik konuları hakkında daha fazla bilgi edinmek için. 

## <a name="manage-linkedin-integration"></a>LinkedIn tümleştirmeyi Yönet
LinkedIn tümleştirme kuruluşlar için Azure AD içinde varsayılan olarak etkindir. LinkedIn Integration etkinleştirme LinkedIn profilleri Outlook'ta görüntüleme gibi Microsoft Hizmetleri içinde LinkedIn özellikleri kullanmak için kuruluşunuzdaki tüm kullanıcılar sağlar. LinkedIn tümleştirme devre dışı bırakma LinkedIn özellikleri Microsoft uygulamaları ve Hizmetleri kaldırır ve veri LinkedIn ve kuruluşunuzun Microsoft Hizmetleri aracılığıyla arasında paylaşımı durdurur.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>Etkinleştirmek veya devre dışı kuruluşunuzun Azure portalında LinkedIn tümleştirme

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) Azure AD Kiracı için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde, select **kullanıcı ayarlarını**.
4. Altında **LinkedIn tümleştirme**seçin **Evet** veya **Hayır** etkinleştirme veya devre dışı LinkedIn tümleştirme.
   ![LinkedIn Integration etkinleştirme](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>Etkinleştirmek veya Grup İlkesi kullanarak, kuruluşunuzun Office 2016 uygulamaları için LinkedIn tümleştirmesi devre dışı

1. Karşıdan [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extract **ADMX** dosyaları ve kopyalamak için **merkezi bir depoya**.
3. Açık Grup İlkesi Yönetimi.
4. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturmak: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016**  >  **Çeşitli** > **LinkedIn tümleştirme izin**.
5. Seçin **etkin** veya **devre dışı**.
  * İlke olduğunda **etkin**, **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 Seçenekleri iletişim kutusunda bulunan ayarı etkindir. Bu ayrıca kuruluşunuzdaki kullanıcıların Office uygulamalarında LinkedIn özellikleri kullanabileceğiniz anlamına gelir.
  * İlke olduğunda **devre dışı**, **Göster LinkedIn özellikleri Office uygulamalarında** bulunan ayarlama Office 2016 seçeneklerinde iletişim devre dışı durumuna ayarlanır ve son kullanıcılar, bu ayar değiştiremiyor. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özellikleri kullanamazsınız. 

Bu Grup İlkesi yalnızca yerel bilgisayar için Office 2016 uygulamaları etkiler. Bunlar LinkedIn Office 2016 uygulamalarını devre dışı bıraksanız bile kullanıcılar Office 365 boyunca profili kartları LinkedIn özelliklerini görebilir. 

### <a name="learn-more"></a>Daha fazla bilgi edinin 
* [LinkedIn bilgileri ve Microsoft uygulamalarınızdaki özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Sonraki adımlar
Azure Portalı'ndaki geçerli LinkedIn tümleştirme ayarınız görmek için aşağıdaki bağlantıyı kullanın:

[LinkedIn tümleştirmesini yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 