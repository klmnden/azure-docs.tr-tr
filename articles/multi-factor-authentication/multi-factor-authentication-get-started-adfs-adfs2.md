---
title: AD FS 2.0 ile Azure MFA Sunucusunu kullanma | Microsoft Belgeleri
description: "Bu, nasıl Azure MFA ve AD FS 2.0 kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 7ad06e9086d6532b3de2ba060b37424051f022c2


---
# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirin.
Bu makale, Azure Active Directory ile birleştirilen ve kaynakları şirket içinde veya bulutta güvenli hale getirmek isteyen kuruluşlara yöneliktir. Kaynaklarınızı, Azure Multi-Factor Authentication Sunucusu’nu kullanarak ve değeri yüksek uç noktalarda iki aşamalı doğrulamanın tetiklenmesi için AD FS ile çalışacak şekilde yapılandırarak koruyun.

Bu belge AD FS 2.0 ile Multi-Factor Authentication Sunucusu kullanmayı ele alır.  [Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-w2k12.md) hakkında daha fazla bilgi alın.

## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0’ı bir ara sunucu ile güvenli hale getirme
Ara sunucu ile AD FS 2.0’ı güvenli hale getirmek için ADFS ara sunucusunda Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve Sunucu’yu yapılandırın.

### <a name="configure-iis-authentication"></a>IIS kimlik doğrulamasını yapılandırma
1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde **IIS Kimlik Doğrulaması** simgesine tıklayın.
2. **Form Tabanlı** sekmesine tıklayın.
3. **Ekle…** düğmesine tıklayın.
   <center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak algılamak için, Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusuna Oturum Açma URL’sini girin (https://sso.contoso.com/adfs/ls gibi) ve Tamam’a tıklayın.
5. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın. Bu özellik hakkında daha fazla bilgi için yardım dosyasına bakın.
6. Sayfa değişkenleri otomatik olarak algılanamıyorsa Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda **Elle Belirt** düğmesine tıklayın.
7. Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda, URL Gönder alanına (https://sso.contoso.com/adfs/ls gibi) ADFS oturum açma sayfası URL’sini girin ve bir uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir. URL Gönder ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın.
8. İstek biçimini "POST veya GET" olarak ayarlayın.
9. Kullanıcı adı değişkeni (ctl00$ContentPlaceHolder1$UsernameTextBox) ve Parola değişkenini (ctl00$ContentPlaceHolder1$PasswordTextBox) girin. Form tabanlı oturum açma sayfanız bir etki alanı metin kutusu görüntülerse, Etki alanı değişkenini de girin. Oturum açma sayfasındaki giriş kutularının adlarını bulmak için, bir web tarayıcısında oturum açma sayfasına gitmeniz, sayfada sağ tıklamanız ve **Kaynağı Görüntüle** seçeneğini belirlemeniz gerekebilir.
10. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
    <center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. **Gelişmiş...** düğmesine tıklayarak gelişmiş ayarları gözden geçirin. Özel bir reddetme sayfa dosyası seçme, çerez kullanarak başarılı kimlik doğrulamalarını web sitesinde önbelleğe alma ve birincil kimlik bilgilerinin nasıl doğrulanacağını seçme gibi ayarları yapılandırabilirsiniz.
12. ADFS ara sunucusunun etki alanına katılması pek olası olmadığından, kullanıcı içeri aktarma ve ön kimlik doğrulama için etki alanı denetleyicisine bağlanmak amacıyla LDAP’yi kullanabilirsiniz. Gelişmiş Form Tabanlı Web Sitesi iletişim kutusunda **Birincil Kimlik Doğrulama** sekmesine tıklayın ve **LDAP Bağlama** için Ön Kimlik Doğrulaması türünü seçin.
13. Tamamlandığında Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için **Tamam** düğmesine tıklayın. Gelişmiş ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın.
14. İletişim kutusunu kapatmak için **Tamam** düğmesine tıklayın.
15. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.
16. IIS eklentisini istediğiniz düzeyde etkinleştirmek için **Yerel Modül** sekmesine tıklayın ve sunucuyu, ADFS ara sunucusunun altında çalıştığı web sitesini (“Varsayılan Web Sitesi” gibi) veya ADFS ara sunucu uygulamasını (“adfs” altındaki “ls” gibi) seçin.
17. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusuna tıklayın.
18. IIS kimlik doğrulaması şimdi etkinleştirildi.

### <a name="configure-directory-integration"></a>Dizin tümleştirmesini yapılandırma
IIS kimlik doğrulamasını etkinleştirdiniz, ancak LDAP aracılığıyla Active Directory’de (AD) ön kimlik doğrulama yapabilmek için etki alanı denetleyicisi için LDAP bağlantısı yapılandırmanız gerekir.

1. **Dizin Tümleştirme** simgesine tıklayın.
2. Ayarlar sekmesinde **Özel LDAP yapılandırması kullan** radyo düğmesini seçin.
   <center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. **Düzenle...** düğmesine tıklayın.
4. LDAP Yapılandırmasını Düzenle iletişim kutusunda, AD etki alanı denetleyicisine bağlanmak için gerekli bilgilerle alanları doldurun. Alanların açıklamaları aşağıdaki tabloda yer almaktadır: Bu bilgiler, Azure Multi-Factor Authentication Yardım dosyasında da bulunmaktadır.
5. **Test** düğmesine tıklayarak LDAP bağlantısını test edin.
   <center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. LDAP bağlantı testi başarılı olursa **Tamam** düğmesine tıklayın.

### <a name="configure-company-settings"></a>Şirket ayarlarını yapılandırma
1. Daha sonra **Şirket Ayarları** simgesine tıklayın ve **Kullanıcı Adı Çözümleme** sekmesini seçin.
2. **Kullanıcı adlarını eşleştirmek için LDAP benzersiz tanımlayıcı özniteliği** radyo düğmesini seçin.
3. Kullanıcılar, kullanıcı adlarını “etki alanı\kullanıcı adı” biçiminde girerse Sunucunun, LDAP sorgusunu oluşturduğunda etki alanını kullanıcı adından çıkarabilmesi gerekir. Bu, bir kayıt defteri ayarı aracılığıyla yapılabilir.
4. Kayıt defteri düzenleyicisini açın ve 64 bit sunucuda HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor öğesine gidin. 32 bit sunucuda, “Wow6432Node” öğesini yoldan çıkarın. “UsernameCxz_stripPrefixDomain” adlı bir DWORD kayıt defteri anahtarı oluşturun ve değeri 1 olarak ayarlayın. Azure Multi-Factor Authentication artık ADFS ara sunucusunu güvenli hale getirir.

Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açarken iki aşamalı doğrulamanın gerekli olmaması için iç IP adreslerini güvenilir listeye almak istiyorsanız [Güvenilen IP’ler](#trusted-ips) bölümüne bakın.

<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>Ara sunucu olmadan AD FS 2.0 Direct
AD FS ara sunucusu kullanılmadığında AD FS’yi güvenli hale getirebilirsiniz. AD FS sunucusunda Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve aşağıdaki adımları kullanarak Sunucu’yu yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde **IIS Kimlik Doğrulaması** simgesine tıklayın.
2. **HTTP** sekmesine tıklayın.
3. **Ekle…** düğmesine tıklayın.
4. Temel URL Ekle iletişim kutusunda, HTTP kimlik doğrulamasının gerçekleştirildiği ADFS web sitesinin URL’sini (https://sso.domain.com/adfs/ls/auth/integrated gibi) Temel URL alanına girin. Ardından bir uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. İsterseniz, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın. Bu özellik hakkında daha fazla bilgi için yardım dosyasına bakın.
7. İsterseniz tanımlama bilgisi önbellek kutusunu işaretleyin.
   <center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. **Tamam** düğmesine tıklayın.
9. IIS eklentisini istediğiniz düzeyde etkinleştirmek için **Yerel Modül** sekmesine tıklayın ve sunucuyu, web sitesini (“Varsayılan Web Sitesi” gibi) veya ADFS uygulamasını (“adfs” altındaki “ls” gibi) seçin.
10. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusuna tıklayın. Azure Multi-Factor Authentication artık ADFS’yi güvenli hale getirir.

Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açarken iki aşamalı doğrulamanın gerekli olmaması için iç IP adreslerini güvenilir listeye almak istiyorsanız Güvenilen IP’ler bölümüne bakın.

## <a name="trusted-ips"></a>Güvenilen IP'ler
Güvenilen IP'ler, kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, kullanıcıları ofiste oturum açtıklarında iki aşamalı doğrulamadan muaf tutabilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz.

### <a name="to-configure-trusted-ips"></a>Güvenilen IP'leri yapılandırmak için
1. IIS Kimlik Doğrulaması bölümünde **Güvenilen IP'ler** sekmesine tıklayın.
2. **Ekle…** düğmesine tıklayın.
3. Güvenilen IP Ekle iletişim kutusu göründüğünde **Tek bir IP**, **IP aralığı** ve **Alt ağ** radyo düğmelerinden birini seçin.
4. Güvenilir listede olması gereken IP adresini, IP adresleri aralığını ya da alt ağı girin. Bir alt ağ giriyorsanız uygun Ağ maskesini seçip **Tamam** düğmesine tıklayın. Güvenilen IP şimdi eklendi.

<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>



<!--HONumber=Dec16_HO1-->


