---
title: Microsoft uygulamaları ve Hizmetleri Azure Active Directory'de için LinkedIn bağlantılarını etkinleştirme | Microsoft Docs
description: Etkinleştirme veya devre dışı için Azure Active Directory Microsoft uygulamalarında LinkedIn hesabı bağlantıları açıklanmaktadır
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 4b3ff0b2481b42f516d28ac17f2616685730b7d5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872704"
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>Microsoft uygulamaları ve Hizmetleri için LinkedIn hesabı bağlantıları
Bu makalede, yönetim merkezinde Azure Active Directory (Azure AD) kiracınız için LinkedIn hesabı bağlantıları yönetme konusunda bilgi edinebilirsiniz. 

> [!IMPORTANT]
> LinkedIn hesabı bağlantıları işlevi şu anda Azure AD kiracıları için kullanıma sunulacaktır. Bu Kiracı için kullanıma sunulduğunda, bu varsayılan olarak etkindir. Avustralya, Kanada, Çin, Fransa, Almanya, Hindistan, Güney Kore, Birleşik Krallık, Japonya ve Güney Afrika barındırılan Exchange Online posta kutusu olan ABD kamu müşterilerine ve kuruluşlar için kullanılamaz. Bu posta kutusu konumlar için destek yakında sunulacaktır.  Dağıtım bilgilerini güncel bir görünümü için bkz: [Office 365 yol haritası](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) sayfası.

## <a name="benefit-to-users"></a>Kullanıcılar için yararlı
Kullanıcılar kendi LinkedIn hesabına bağlandıktan sonra LinkedIn bilgi kişiselleştirilmiş bilgiler ve özellikleri çeşitli Microsoft uygulamaları veya hizmetleri göstermek için kullanılır. Kuruluşunuz dışındaki kişilerle olanlar bile kullanıcılar kişiler hakkında Öngörüler ile Microsoft profili karttaki çalıştıkları görebilirsiniz. Zaman içinde daha ilgili ve özel olarak uyarlanmış işlerine LinkedIn deneyimlerini de olur. Örneğin, LinkedIn dayalı yeni bağlantıları önerebilir kullanıcılar ile çalışan veya kendi takvimdeki gün kişilerle ilgili içgörüleri.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>LinkedIn hesabı bağlantıları kullanıcı için nasıl görünür?
LinkedIn hesabı bağlantıları, kullanıcıların bazı Microsoft uygulamaları içinde genel LinkedIn profil bilgilerini görüntüleyebilmesine izin verin. Kiracınızdaki kullanıcılara, LinkedIn ile Microsoft iş veya Okul hesapları ek LinkedIn profil bilgilerini görüntüleyebilmesine seçebilirsiniz. Daha fazla bilgi için [LinkedIn bilgilerini ve özellikleri Microsoft uygulamaları ve Hizmetleri](https://go.microsoft.com/fwlink/?linkid=850740).

Bunlar, kuruluşunuzdaki kullanıcılar bağlanmak LinkedIn ile Microsoft iş veya Okul hesapları, iki seçeneğiniz vardır: 
* Her iki hesap arasında veri paylaşmasına izin verin. İzni verdikleri Bunun anlamı, LinkedIn hesabıyla Microsoft iş veya Okul hesabı, yanı sıra, Microsoft veri paylaşımı için iş veya Okul verileri, LinkedIn hesabıyla paylaşmak için hesabınızı. LinkedIn ile paylaşılan veri Çevrimiçi Hizmetler bırakır. 
* Verileri, LinkedIn hesabıyla Microsoft iş ve Okul hesabı yalnızca paylaşma izni verin

Kullanıcıların LinkedIn ile Microsoft arasında paylaşılan veriler hakkında daha fazla bilgi için iş veya Okul hesapları için bkz: [İş yeriniz veya okulunuz, Microsoft uygulamalarında LinkedIn](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Kullanıcıların hesaplarını kesin](https://www.linkedin.com/help/linkedin/answer/85097) ve veri paylaşım izinleri dilediğiniz zaman kaldırabilirsiniz. 
* [Kullanıcılar, kendi LinkedIn profil nasıl görüntülenen denetleyebilir](https://www.linkedin.com/help/linkedin/answer/83), dahil, kendi profili Microsoft uygulamalarında olup olmadığı görüntülenebilir.

## <a name="privacy-considerations"></a>Gizlilik konuları
LinkedIn hesabı bağlantıları çalışabilmelerini kullanıcılarınızın LinkedIn bilgilerini bazılarına erişmek için Microsoft uygulamalar ve hizmetler. Okuma [Microsoft gizlilik bildirimi](https://privacy.microsoft.com/privacystatement/) LinkedIn hesabı bağlantıları Azure AD'de etkinleştirilirken gizliliğiyle ilgili konular hakkında daha fazla bilgi edinmek için. 

## <a name="manage-linkedin-account-connections"></a>LinkedIn hesabı bağlantıları yönetme
LinkedIn hesabı bağlantıları işlevi tüm kiracınıza için varsayılan olarak açıktır. LinkedIn hesabı bağlantıları tüm kiracınız için devre dışı bırakın veya kiracınızdaki Seçili kullanıcılar için LinkedIn hesabı bağlantıları etkinleştirmek seçebilirsiniz. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>Etkinleştirmek veya devre dışı Azure portalında kiracınız için LinkedIn hesabı bağlantısı

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) Azure AD kiracınız için genel yönetici olan bir hesapla.
2. Seçin **kullanıcılar**.
3. Üzerinde **kullanıcılar** dikey penceresinde **kullanıcı ayarları**.
4. Altında **LinkedIn hesabı bağlantıları**:
  * Seçin **Evet** kiracınızdaki tüm kullanıcıları için LinkedIn hesabı bağlantıları etkinleştirmek için
  * Seçin **seçili** LinkedIn etkinleştirmek için kullanıcılar yalnızca seçilen Kiracı için bağlantı hesabı
  * Seçin **Hayır** LinkedIn hesabı bağlantıları tüm kullanıcılar için devre dışı bırakmak için ![etkinleştirme LinkedIn hesabı bağlantıları](./media/linkedin-integration/linkedin-integration.png)
5. Seçerek işiniz bittiğinde, ayarlarınızı kaydetmek **Kaydet**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>Etkinleştirmek veya devre dışı için Grup İlkesi'ni kullanarak kuruluşunuzun Office 2016 uygulamaları LinkedIn hesabı bağlantıları

1. İndirme [Office 2016 Yönetim Şablonu dosyalarını (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Ayıklama **ADMX** dosyaları ve bunları merkezi deponuza kopyalayın.
3. Açık Grup İlkesi Yönetimi.
4. Aşağıdaki ayarlarla bir Grup İlkesi nesnesi oluşturmak: **Kullanıcı Yapılandırması** > **Yönetim Şablonları** > **Microsoft Office 2016**  >  **Çeşitli** > **Göster LinkedIn özellikleri Office uygulamalarında**.
5. Seçin **etkin** veya **devre dışı bırakılmış**.
  * İlke olduğunda **etkin**, **Göster LinkedIn özellikleri Office uygulamalarında** Office 2016 Seçenekler iletişim kutusundaki seçeneği etkinleştirilmiştir. Bu ayrıca, kuruluşunuzdaki kullanıcıların Office uygulamalarında LinkedIn özellikleri kullanabileceğiniz anlamına gelir.
  * İlke olduğunda **devre dışı**, **Göster LinkedIn özellikleri Office uygulamalarında** ayarı Office 2016 Seçenekleri iletişim kutusu devre dışı durumuna ayarlanır ve son kullanıcılar, bu ayar değiştirilemez. Kuruluşunuzdaki kullanıcılar, Office 2016 uygulamalarında LinkedIn özellikleri kullanamaz.

Bu Grup İlkesi yalnızca yerel bir bilgisayar için Office 2016 uygulamaları etkiler. Bunlar LinkedIn Office 2016 uygulamalarını devre dışı olsa bile kullanıcılar Office 365 genelinde profil kartları LinkedIn özellikleri görebilirsiniz. 

### <a name="learn-more"></a>Daha fazla bilgi edinin 
* [LinkedIn bilgilerini ve Microsoft uygulamalarınızda özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn Yardım Merkezi](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Sonraki adımlar
Azure portalında ayarlama, geçerli LinkedIn hesabı bağlantıları görmek için aşağıdaki bağlantıyı kullanın:

[LinkedIn hesabı bağlantıları yapılandırma](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 