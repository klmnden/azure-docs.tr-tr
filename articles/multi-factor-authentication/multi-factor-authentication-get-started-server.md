---
title: "Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama | Microsoft Belgeleri"
description: "Bu, nasıl Azure MFA Sunucusu kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır."
services: multi-factor-authentication
keywords: "kimlik doğrulama sunucusu, azure multi factor authentication uygulaması etkinleştirme sayfası, kimlik doğrulama sunucusu indirme"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: e94120e4-ed77-44b8-84e4-1c5f7e186a6b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 4235dfd0e17b9892787dd86d807b8f1f6e360675
ms.contentlocale: tr-tr
ms.lasthandoff: 06/30/2017

---

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama

<center>![Şirket içi MFA](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Artık şirket içi Multi-Factor Authentication Sunucusu’nı kullanıp kullanmayacağımıza karar verdiğimize göre, devam edebiliriz. Bu sayfa yeni bir sunucu yüklemeyi ve şirket içi Active Directory’de kurulumunu yapmayı ele alır. MFA sunucusu zaten yüklüyse ve yükseltmek istiyorsanız bkz. [En yeni Azure Multi-Factor Authentication Sunucusu’na yükseltme](multi-factor-authentication-server-upgrade.md). Yalnızca web hizmetini yükleme hakkında bilgi almak istiyorsanız bkz. [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtma](multi-factor-authentication-get-started-server-webservice.md).
 
## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

Azure Multi-Factor Authentication Sunucusu'nu indirmeden önce yük ve yüksek kullanılabilirlik gereksinimlerinizi göz önünde bulundurun. Bu bilgileri kullanarak nasıl ve nereye dağıtım gerçekleştireceğinize karar verin. 

Düzenli olarak kimlik doğrulaması yapmasını beklediğiniz kullanıcı sayısı, ihtiyacınız olan bellek miktarı için iyi bir yol gösterici olabilir. 

| Kullanıcılar | RAM |
| ----- | --- |
| 1-10.000 | 4 GB |
| 10.001-50.000 | 8 GB |
| 50.001-100.000 | 12 GB |
| 100.000-200.001 | 16 GB |
| 200.001+ | 32 GB |

Yüksek kullanılabilirlik veya yük dengeleme için birden çok sunucu ayarlamanız mı gerekiyor? Azure MFA Sunucusu ile bu yapılandırmayı ayarlamanın birçok yolu vardır. İlk Azure MFA Sunucunuzu yüklediğinizde bu, ana sunucunuz olur. Diğer tüm sunucular, alt sunucu haline gelir ve kullanıcılarının yanı sıra yapılandırmayı ana sunucuyla otomatik olarak eşitler. Ardından, bir birincil sunucu yapılandırabilir ve geri kalanları yedek sunucu olarak belirleyebilir veya tüm sunucular arasında yük dengeleme ayarlayabilirsiniz. 

Ana MFA Sunucusu çevrimdışı olsa bile alt sunucular iki aşamalı doğrulama isteklerini işleyebilir. Ancak yeni kullanıcı ekleyemezsiniz ve mevcut kullanıcılar, ana sunucu yeniden çevrimiçi olana veya alt sunucu yükseltilene kadar ayarlarını güncelleştiremezler. 

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Azure Multi-Factor Authentication için kullandığınız sunucunun aşağıdaki gereksinimleri karşıladığından emin olun:

| Azure Multi-Factor Authentication Sunucusu Gereksinimleri | Açıklama |
|:--- |:--- |
| Donanım |<li>200 MB boş sabit disk alanı</li><li>x32 veya x64 özellikli işlemci</li><li>1 GB veya daha fazla RAM</li> |
| Yazılım |<li>Windows Server 2008 veya üst sürümü, ana bilgisayar sunucu işletim sistemi ise</li><li>Windows 7 veya üst sürümü, ana bilgisayar istemci işletim sistemi ise</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 veya üst sürümü, kullanıcı portalı veya web hizmeti SDK’sı yüklüyorsanız</li> |

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure Multi-Factor Authentication Sunucusu güvenlik duvarı gereksinimleri
Her MFA sunucusunun bağlantı noktası 443’te aşağıdaki adreslere giden iletişim kurabilmesi gerekir:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Giden güvenlik duvarları bağlantı noktası 443’te kısıtlı ise aşağıdaki IP adresi aralıklarını açın:

| IP Alt ağı | Ağ maskesi | IP aralığı |
|:--- |:--- |:--- |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1 – 134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1 – 134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129 – 70.37.154.254 |

Olay Onayı özelliğini kullanmıyorsanız ve kullanıcılarınız şirket ağındaki cihazlardan doğrulama yapmak için mobil uygulamalar kullanmıyorsa, yalnızca aşağıdaki aralıklar gereklidir:

| IP Alt ağı | Ağ maskesi | IP aralığı |
|:--- |:--- |:--- |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72 – 134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72 – 134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201 – 70.37.154.206 |

## <a name="download-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nu indirme
Azure Multi-Factor Authentication Sunucusu’nu indirmenin iki farklı yolu vardır. Her ikisi de Azure portal aracılığıyla yapılır. Birinci yol Multi-Factor Auth Sağlayıcısı’nı doğrudan yöneterek yapılandır. İkinci yol hizmet ayarları aracılığıyla yapılandır. İkinci seçenek Multi-Factor Auth Sağlayıcısı ya da Azure MFA, Azure AD Premium veya Enterprise Mobility Suite lisansı gerektirir.

> [!Important]
> Bu iki seçenek benzer görünebilir ancak hangisini kullanmanız gerektiğine karar vermeniz önemlidir. Kullanıcılarınız MFA (Azure MFA, Azure AD Premium veya Enterprise Mobility + Security) ile verilen lisanslara sahipse, sunucu indirme sayfasına ulaşmak için Multi-Factor Auth Sağlayıcısı oluşturmayın. Bunun yerine 2. seçeneği kullanarak sunucuyu hizmet ayarları sayfasından indirin. 

### <a name="option-1-download-azure-multi-factor-authentication-server-from-the-azure-classic-portal"></a>1. Seçenek: Azure Multi-Factor Authentication Sunucusu’nu Klasik Azure portalından indirin

MFA için etkin kullanıcı veya kimlik doğrulama başına ödeme yaptığınız için Multi-Factor Auth Sağlayıcınız varsa bu indirme seçeneğini kullanın. 

1. [Klasik Azure portalında](https://manage.windowsazure.com) yönetici olarak oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Active Directory sayfasında **Multi-Factor Auth Sağlayıcıları** ![Multi-Factor Auth Sağlayıcıları](./media/multi-factor-authentication-get-started-server/authproviders.png) seçeneğine tıklayın.
4. Alt kısımdaki **Yönet**’e tıklayın. Yeni bir sayfa açılır.
5. **İndirmeler**’e tıklayın.
6. **İndir** bağlantısına tıklayın.
   ![İndir](./media/multi-factor-authentication-get-started-server/download4.png)
7. İndirmeyi kaydedin.

### <a name="option-2-download-azure-multi-factor-authentication-server-from-the-service-settings"></a>2. Seçenek: Azure Multi-Factor Authentication Sunucusu’nu hizmet ayarlarından indirin

Enterprise Mobility Suite, Azure AD Premium veya Enterprise Cloud Suite lisansına sahipseniz bu indirme seçeneğini kullanın. 

1. [Klasik Azure portalında](https://manage.windowsazure.com) yönetici olarak oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Azure AD örneğinize çift tıklayın.
4. Üst kısımda **Yapılandır**’a tıklayın
5. **Multi-factor authentication** bölümüne inip **Hizmet ayarlarını yönet**’i seçin
6. Hizmetleri ayarları sayfasında, ekranın alt kısmında **Portal'a git**’e tıklayın. Yeni bir sayfa açılır.
   ![İndir](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. **İndirmeler**’e tıklayın,
8. **İndir** bağlantısına tıklayın.
    ![İndir](./media/multi-factor-authentication-get-started-server/download4.png)
9. İndirmeyi kaydedin.

## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nu Yükleme ve Yapılandırma
Artık sunucuyu indirdiğinize göre, yükleyebilir ve yapılandırabilirsiniz.  Yükleme yaptığınız sunucunun, planlama bölümünde listelenen gereksinimleri karşıladığından emin olun. 

Aşağıdaki adımlar yapılandırma sihirbazı ile hızlı kurulumu gösterir. Sihirbazı görmüyorsanız veya yeniden çalıştırmak istiyorsanız, sunucuda **Araçlar** menüsünden seçebilirsiniz.

1. Yürütülebilir dosyaya çift tıklayın. 
2. Yükleme Klasörünü Seçin ekranında klasörün doğru olduğundan emin olun ve **İleri**’ye tıklayın.
3. Yükleme tamamlandıktan sonra **Son**'a tıklayın.  Yapılandırma sihirbazı başlatılır.
4. Yapılandırma sihirbazı karşılama ekranında **Kimlik Doğrulaması Yapılandırma Sihirbazı kullanmayı atla** seçeneğini işaretleyin ve **İleri**’ye tıklayın.  Sihirbaz kapatılır ve sunucu başlatılır.
    ![Bulut](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Sunucuyu indirdiğimiz sayfaya dönerek, **Etkinleştirme Kimlik Bilgileri Oluştur** düğmesine tıklayın. Bu bilgileri verilen kutularda Azure MFA Sunucusu’na kopyalayın ve **Etkinleştir**’e tıklayın.

## <a name="import-users-from-active-directory"></a>Kullanıcıları Active Directory'den içeri aktarma
Artık sunucu yüklendiğine ve yapılandırıldığına göre, hızlı bir şekilde kullanıcıları Azure MFA Sunucusu’na aktarabilirsiniz

1. Azure MFA Sunucusu’nda, solda, **Kullanıcılar**’ı seçin.
2. Alt kısımda, **Active Directory’den içeri aktar**’ı seçin.
3. Artık kullanıcıları tek tek arayabilir ya da içindeki kullanıcılarla birlikte OU’lar için AD dizininde arama yapabilirsiniz.  Bu durumda kullanıcıların OU’su belirtilir.
4. Sağ tarafta tüm kullanıcıları vurgulayın ve **İçeri Aktar**’a tıklayın.  Başarılı olduğunuzu belirten bir açılır pencere görmeniz gerekir.  İçeri aktarma penceresini kapatın.
   ![Bulut](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Kullanıcılara e-posta gönderme
Kullanıcılarınızı MFA Sunucusuna aktardıktan sonra, iki adımlı doğrulamaya kaydolduklarını bildirmek amacıyla kullanıcılara bir e-posta gönderin.

Gönderdiğiniz e-posta, kullanıcılarınızı iki adımlı doğrulama için nasıl yapılandırdığınıza göre belirlenir. Örneğin, telefon numaralarını şirket dizininden alabildiyseniz, kullanıcıların beklentilerini bilebilmesi için e-posta varsayılan telefon numaralarını içermelidir. Telefon numaralarını içeri aktarmadıysanız veya kullanıcılarınız mobil uygulamayı kullanacaksa kullanıcılara hesap kaydını tamamlama yönergelerinin sağlandığı bir e-posta gönderin. E-postaya Azure Multi-Factor Authentication Kullanıcı Portalı’nın köprü bağlantısını ekleyin.

E-postanın içeriği aynı zamanda kullanıcı için ayarlanmış doğrulama yöntemine (telefonla arama, SMS veya mobil uygulama) bağlı olarak değişir.  Örneğin, kullanıcının kimlik doğrularken PIN kullanması gerekiyorsa, e-posta kullanıcıya ilk PIN’ini bildirir.  Kullanıcıların ilk doğrulama sırasında kendi PIN'lerini değiştirmesi gerekir.


### <a name="configure-email-and-email-templates"></a>E-posta ve e-posta şablonlarını yapılandırma
Soldaki e-posta simgesine tıklayarak bu e-postaları gönderme ayarlarını yapabilirsiniz. Bu sayfada, posta sunucunuzun SMTP bilgilerini girebilir ve **Kullanıcılara e-posta gönder** onay kutusunu işaretleyerek e-posta gönderebilirsiniz.

![E-posta Ayarları](./media/multi-factor-authentication-get-started-server/email1.png)

E-posta İçeriği sekmesinde, seçim yapabileceğiniz e-posta şablonlarını görebilirsiniz. Kullanıcılarınızı iki adımlı doğrulama için nasıl yapılandırdığınıza bağlı olarak, size en uygun şablonu seçin.

![E-posta şablonları](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Azure Multi-Factor Authentication Sunucusu kullanıcı verileri nasıl işler?
Şirket içi Multi-Factor Authentication (MFA) Sunucusu kullandığınızda, kullanıcının verileri şirket içi sunucularda depolanır. Kalıcı kullanıcı verileri bulutta depolanmaz. Kullanıcı iki adımlı kimlik doğrulama gerçekleştirdiğinde MFA Sunucusu doğrulamayı gerçekleştirmek üzere Azure MFA bulut hizmetine veri gönderir. Bu kimlik doğrulama istekleri bulut hizmetine gönderildiğinde, aşağıdaki alanlar istekte ve günlüklerde gönderilir, böylece bunlar müşterinin kimlik doğrulama/kullanım raporlarında kullanılabilir. Multi-Factor Authentication Sunucusu’nda etkinleştirilebilecek ya da devre dışı bırakılabilecek şekilde, bazı alanlar isteğe bağıldır. MFA Sunucusu’ndan MFA bulut hizmetlerine iletişim 443 giden bağlantı noktası üzerinden SSL/TLS kullanır. Bu alanlar aşağıdaki gibidir:

* Benzersiz Kimlik - kullanıcı adı veya iç MFA sunucusu kimliği
* Adı ve soyadı (isteğe bağlı)
* E-posta adresi (isteğe bağlı)
* Telefon numarası - sesli arama veya SMS kimlik doğrulaması yapılırken
* Cihaz belirteci - Mobil uygulama kimlik doğrulaması yaparken
* Kimlik doğrulaması modu
* Kimlik doğrulaması sonucu
* MFA Sunucusu adı
* MFA Sunucusu IP’si
* İstemci IP’si - varsa

Yukarıdaki alanlara ek olarak, doğrulama sonucu (başarılı/reddedildi) ve reddetme nedeni kimlik doğrulama verileriyle birlikte depolanır ve kimlik doğrulama/kullanım raporlarıyla kullanıma sunulur.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanıcı self servis işlemleri için [Kullanıcı Portalı](multi-factor-authentication-get-started-portal.md)’nı ayarlayın ve yapılandırın.

- Azure MFA Sunucusunu [Active Directory Federasyon Hizmeti](multi-factor-authentication-get-started-adfs.md), [RADIUS Kimlik Doğrulaması](multi-factor-authentication-get-started-server-radius.md) veya [LDAP Kimlik Doğrulaması](multi-factor-authentication-get-started-server-ldap.md) ile ayarlayıp yapılandırın.

- [RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](multi-factor-authentication-get-started-server-rdg.md)’nu kurun ve yapılandırın. 

- [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtın](multi-factor-authentication-get-started-server-webservice.md).

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](multi-factor-authentication-advanced-vpn-configurations.md).

