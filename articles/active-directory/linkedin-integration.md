---
title: "Microsoft uygulamaları ve Hizmetleri Azure Active Directory'de LinkedIn bağlantılarında etkinleştirme | Microsoft Docs"
description: "Etkinleştirmek veya devre dışı LinkedIn hesabı Azure Active Directory'de Microsoft uygulamaları bağlantılarında açıklanmaktadır"
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 03/15/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 3bf224edea9e6da0d0eadb6fb6a409248de3d0e3
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>Microsoft uygulamaları ve Hizmetleri için LinkedIn hesap bağlantıları
Bu makalede, Azure Active Directory (Azure AD) Yönetim Merkezi'nden kiracınız için LinkedIn hesap bağlantıları yönetmek nasıl öğrenebilirsiniz. 

> [!IMPORTANT]
> LinkedIn hesabı bağlantıları işlevi şu anda Azure AD kiracıları için Alınmakta. Kiracınız için toplu Bu, varsayılan olarak etkindir. Avustralya, Kanada, Çin, Fransa, Almanya, Hindistan, Güney Kore, İngiltere, Japonya ve Güney Afrika barındırılan Exchange Online posta kutularıyla ABD hükümeti müşterileri ve kuruluşlar için kullanılamaz. Bu posta kutusu konumları desteği yakında geliyor.  Ürün bilgileri güncel bir görünümü için bkz: [Office 365 yol haritası](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) sayfası.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Nasıl LinkedIn hesap bağlantılar kullanıcıya görünür
Bazı Microsoft uygulamalarını içinde ortak LinkedIn profil bilgilerini görmek kullanıcıların LinkedIn hesap bağlantıları sağlar. Kiracı kullanıcılar LinkedIn ve Microsoft iş bağlanmak veya Okul hesapları ek LinkedIn profil bilgilerini görmek için seçebilirsiniz. Daha fazla bilgi için bkz: [LinkedIn bilgileri ve Microsoft uygulamaları ve Hizmetleri özellikleri](https://go.microsoft.com/fwlink/?linkid=850740).

Bunlar, kuruluşunuzdaki kullanıcılar LinkedIn ve Microsoft iş bağlanmak veya Okul hesapları, iki seçeneğiniz vardır: 
* Her iki hesap arasında veri paylaşımı için izinler verebilirsiniz. İzni verdikleri Bunun anlamı LinkedIn hesaplarında kullanıcıların Microsoft iş veya Okul hesabı yanı sıra ile kendi Microsoft veri paylaşmak için iş veya Okul hesabı LinkedIn hesaplarıyla veri paylaşmak için. LinkedIn ile paylaşılan verileri Çevrimiçi Hizmetler bırakır. 
* Verileri yalnızca LinkedIn hesaplarına kendi Microsoft iş ve Okul hesabı paylaşmak için izin ver

Kullanıcıların LinkedIn ve Microsoft arasında paylaşılan veriler hakkında daha fazla bilgi için iş veya Okul hesapları, bkz: [işiniz veya okulunuz adresindeki Microsoft uygulamalarındaki LinkedIn](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Kullanıcıların hesaplarını bağlantısını kesmek](https://www.linkedin.com/help/linkedin/answer/85097) ve herhangi bir zamanda paylaşım izinleri veri kaldırın. 
* [Kullanıcılar kendi LinkedIn profilini nasıl görüntülenen denetleyebilir](https://www.linkedin.com/help/linkedin/answer/83), de dahil olmak üzere kendi profilini Microsoft uygulamalarındaki olup olmadığı görüntülenebilir.

## <a name="privacy-considerations"></a>Gizlilik konuları
LinkedIn etkinleştirme hesap bağlantılarına izin veren Microsoft uygulamaları ve Hizmetleri, kullanıcılarınızın LinkedIn bilgilerin bazıları erişmek için. Okuma [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement/) LinkedIn hesap bağlantıları Azure AD'de etkinleştirirken gizlilik konuları hakkında daha fazla bilgi edinmek için. 

## <a name="manage-linkedin-account-connections"></a>LinkedIn hesap bağlantılarını yönetme
LinkedIn hesabı bağlantıları işlevi tüm kiracınız için varsayılan olarak açıktır. Kiracınızda Seçili kullanıcılar için LinkedIn hesap bağlantıları etkinleştirmek veya LinkedIn hesap bağlantıları tüm Kiracı için devre dışı seçebilirsiniz. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>Etkinleştirmek veya LinkedIn hesap bağlantısı Azure portalında Kiracı için devre dışı

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) Azure AD Kiracı için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde, select **kullanıcı ayarlarını**.
4. Altında **LinkedIn hesap bağlantıları**:
  * Seçin **Evet** kiracınızdaki tüm kullanıcıları için LinkedIn hesap bağlantıları etkinleştirmek için
  * Seçin **seçili** LinkedIn etkinleştirmek için kullanıcılar yalnızca seçilen Kiracı bağlantılarında hesabı
  * Seçin **Hayır** LinkedIn hesap bağlantıları tüm kullanıcılar için devre dışı bırakmak için ![etkinleştirme LinkedIn hesap bağlantıları](./media/linkedin-integration/LinkedIn-integration.png)
5. Seçerek bittiğinde ayarlarınızı kaydetmek **kaydetmek**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>Etkinleştirmek veya devre dışı LinkedIn hesap bağlantıları için Grup İlkesi kullanarak, kuruluşunuzun Office 2016 uygulamaları

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
Azure portalında ayarı geçerli LinkedIn hesap bağlantılarınızı görmek için aşağıdaki bağlantıyı kullanın:

[LinkedIn hesap bağlantılarını yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 