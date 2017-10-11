---
title: "IIS Kimlik Doğrulaması ve Azure MFA Sunucusu | Microsoft Docs"
description: "Bu, IIS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.openlocfilehash: ab6f9110dccd3cfc15092f535650e8d8cb1af13c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>IIS web uygulamaları için Azure Multi-Factor Authentication Sunucusu

Microsoft IIS web uygulamaları ile IIS kimlik doğrulamasını etkinleştirmek ve yapılandırmak için Azure Multi-Factor Authentication (MFA) Sunucusu’nun IIS Kimlik Doğrulaması bölümünü kullanın. Azure MFA Sunucusu, Azure Multi-Factor Authentication eklemek üzere IIS web sunucusuna yapılan istekleri filtreleyebilen bir eklenti yükler. IIS eklentisi Form Tabanlı Kimlik Doğrulaması ve Tümleşik Windows HTTP Kimlik Doğrulaması için destek sağlar. Güvenilen IP’ler iç IP adreslerini iki öğeli kimlik doğrulamasından muaf tutmak için de kullanılabilir.

![IIS Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile Form Tabanlı IIS Kimlik Doğrulaması kullanma
Form tabanlı kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve Sunucu’yu aşağıdaki yordama göre yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde IIS Kimlik Doğrulaması simgesine tıklayın.
2. **Form Tabanlı** sekmesine tıklayın.
3. **Ekle**'ye tıklayın.
4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak algılamak için, Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusuna Oturum Açma URL’sini (https://localhost/contoso/auth/login.aspx gibi) girin ve **Tamam**’a tıklayın.
5. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, **Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
6. Sayfa değişkenleri otomatik olarak algılanamıyorsa Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda **Elle Belirt**’e tıklayın.
7. Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda, URL Gönder alanına oturum açma sayfası URL’sini girin ve Uygulama adı girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
8. Doğru İstek biçimini seçin. Bu, çoğu web uygulaması için **POST or GET** olarak ayarlanır.
9. Kullanıcı adı değişkenini, Parola değişkenini ve Etki alanı değişkenini (oturum açma sayfasında görünüyorsa) girin. Girdi kutularının adlarını bulmak için bir web tarayıcısında oturum açma sayfasına gidin, sayfaya sağ tıklayın ve **Kaynağı Görüntüle**’yi seçin.
10. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, **Azure Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
11. Aşağıdaki gibi gelişmiş ayarları gözden geçirmek için **Gelişmiş**’e tıklayın:

  - Özel bir reddetme sayfa dosyası seçme
  - Tanımlama bilgilerini kullanarak web sitesinde başarılı kimlik doğrulamalarını belirli bir dönem boyunca önbelleğe alma
  - Birincil kimlik bilgilerinin bir Windows Etki Alanı, LDAP dizinine göre mi yoksa RADIUS sunucusuna göre mi doğrulanacağını belirtin.

12. Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için **Tamam**’a tıklayın.
13. **Tamam** düğmesine tıklayın.
14. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile Tümleşik Windows Kimlik Doğrulaması kullanma
Tümleşik Windows HTTP kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure MFA Sunucusu’nu yükleyin ve aşağıdaki adımları izleyerek Sunucu’yu yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde IIS Kimlik Doğrulaması simgesine tıklayın.
2. **HTTP** sekmesine tıklayın.
3. **Ekle**'ye tıklayın.
4. Taban URL’si Ekle iletişim kutusuna, HTTP kimlik doğrulamasının gerçekleştirileceği web sitesinin URL’sini (http://localhost/owa gibi) girin ve bir Uygulama adı sağlayın (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. Varsayılan yeterli değilse, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, **Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
7. İsterseniz **Tanımlama bilgisi önbelleği** kutusunu işaretleyin.
8. **Tamam** düğmesine tıklayın.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu için IIS Eklentilerini Etkinleştirme
Form Tabanlı ya da HTTP kimlik doğrulaması URL’lerini ve ayarlarını yapılandırdıktan sonra, Azure Multi-Factor Authentication IIS eklentilerinin IIS’de yüklenmesi ve etkinleştirilmesi gereken konumları seçin. Aşağıdaki yordamı kullanın:

1. IIS 6'da çalıştırılıyorsa **ISAPI** sekmesine tıklayın. Bu site için Azure Multi-Factor Authentication ISAPI filtresi eklentisini etkinleştirmek için web uygulamasının altında çalıştığı web sitesini (örneğin, Varsayılan Web Sitesi) seçin.
2. IIS 7 veya üzeri çalıştırılıyorsa **Yerel Modül** sekmesine tıklayın. IIS eklentisini istenen düzeylerde etkinleştirmek için sunucu, web siteleri veya uygulamaları seçin.
3. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusuna tıklayın. Azure Multi-Factor Authentication artık seçilen IIS uygulamasını güvenli hale getirir. Kullanıcıların sunucuya aktarıldığından emin olun.

## <a name="trusted-ips"></a>Güvenilen IP'ler
Güvenilen IP'ler kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, ofisten oturum açarken kullanıcıların Azure Multi-Factor Authentication’dan muaf tutulmasını isteyebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz. Güvenilen IP’leri yapılandırmak için aşağıdaki yordamı kullanın:

1. IIS Kimlik Doğrulaması bölümünde **Güvenilen IP'ler** sekmesine tıklayın.
2. **Ekle**'ye tıklayın.
3. Güvenilen IP'leri Ekle iletişim kutusu göründüğünde **Tek IP**, **IP aralığı** veya **Alt ağ** radyo düğmesini seçin.
4. Güvenilir listesinde olması gereken IP adresini, IP adresleri aralığını ya da alt ağı girin. Bir alt ağ giriyorsanız uygun Ağ maskesini seçip **Tamam**’a tıklayın. Güvenilir listesi şimdi eklendi.
