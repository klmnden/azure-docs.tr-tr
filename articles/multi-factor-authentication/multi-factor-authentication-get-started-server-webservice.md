---
title: Azure MFA Sunucusu Mobil Uygulama Web Hizmeti | Microsoft Belgeleri
description: "Microsoft Authenticator uygulaması ek bir bant dışı kimlik doğrulama seçeneği sunar.  MFA sunucusunun kullanıcılar için anında iletme bildirimleri kullanmasına olanak tanır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/15/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 999361daa2faebe3e88cab0b6085a938d6f40e9d
ms.openlocfilehash: d3a122d7d26635e13281b1cba450937519ed4be6


---
# <a name="getting-started-with-the-mfa-server-mobile-app-web-service"></a>MFA Sunucusu Mobil Uygulama Web Hizmeti’ni kullanmaya başlama
Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik telefon çağrısı yapmak veya SMS göndermek yerine, Azure Multi-Factor Authentication, kullanıcının akıllı telefonu ya da tabletindeki Microsoft Authenticator uygulamasına anında iletme bildirimi gönderir. Kullanıcının oturum açma işlemini tamamlamak için uygulamada **Doğrula** seçeneğine dokunması (ya da PIN’i girip “Kimliği Doğrula” seçeneğine dokunması ) yeterlidir. 

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez. 

Azure Multi-Factor Authentication Sunucusu dışında bir sunucuya kullanıcı portalını yüklemek aşağıdaki adımları gerektirir:

1. Web hizmeti SDK’sını yükleme
2. Mobil uygulama web hizmetini yükleme
3. Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma
4. Microsoft Authenticator uygulamasını son kullanıcılar için etkinleştirme

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için, uygulamanın Mobil Uygulama Web Hizmeti ile başarıyla iletişim kurabilmesini sağlamak amacıyla aşağıdakiler gereklidir:

* Azure Multi-Factor Authentication Sunucusu v6.0 veya üzeri
* Microsoft® [Internet Information Services (IIS) IIS 7.x veya üzerini](http://www.iis.net/) çalıştıran İnternet’e yönelik bir web sunucusuna Mobil Uygulama Web Hizmeti’ni yükleme
* ASP.NET v4.0.30319’un yüklenmesi, kaydedilmesi ve İzinli olarak ayarlanması
* Gerekli rol hizmetleri ASP.NET ve IIS 6 Metatabanı Uyumluluğu’nu içerir.
* Mobil Uygulama Web Hizmeti’nin genel bir URL ile erişilebilir olması
* Mobil Uygulama Web Hizmeti’nin bir SSL sertifikası ile güvenli hale getirilmesi.
* Azure Multi-Factor Authentication Sunucusu’nun yüklendiği sunucudaki IIS 7.x ya da üzeri bir sürüme Azure Multi-Factor Authentication Web Hizmeti SDK’sını yükleme
* Azure Multi-Factor Authentication Web Hizmeti SDK’sının bir SSL sertifikası ile güvenli hale getirilmesi.
* Mobil Uygulama Web Hizmeti’nin SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabilmesi
* Mobil Uygulama Web Hizmeti’nin "PhoneFactor Admins" güvenlik grubunun üyesi olan bir hizmet hesabının kimlik bilgilerini kullanarak Azure MFA Web Hizmeti SDK’sında kimliğini doğrulayabilmesi. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucudaysa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.


## <a name="install-the-web-service-sdk"></a>Web hizmeti SDK’sını yükleme
Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication (MFA) Sunucusu’nda halihazırda yüklü değilse, bu sunucuya gidin ve Azure MFA Sunucusu’nu açın. 

1. Web Hizmeti SDK’sı simgesine tıklayın.
2. **Web Hizmeti SDK’sını Yükle**’ye tıklayın ve verilen yönergeleri uygulayın. 

Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Bu amaç için otomatik olarak imzalanan bir sertifika kullanılabilir. Kullanıcı Portalı web sunucusunun SSL bağlantısı başlatırken bu sertifikaya güvenebilmesi için sertifikayı sunucudaki Yerel Bilgisayar hesabının “Güvenilen Kök Sertifika Yetkilileri” deposuna aktarın.

![Kurulum](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

## <a name="install-the-mobile-app-web-service"></a>Mobil uygulama web hizmetini yükleme
Mobil uygulama web hizmetini yüklemeden önce aşağıdaki ayrıntılara dikkat edin:

* Azure MFA Kullanıcı Portalı İnternet’e yönelik sunucuda zaten yüklüyse, Web Hizmeti SDK’sına ilişkin kullanıcı adı, parola ve URL Kullanıcı Portalı’nın web.config dosyasından kopyalanabilir.
* İnternet'e yönelik web sunucusunda bir web tarayıcısı açmak ve web.config dosyasına girilen Web hizmeti SDK’sının URL’sine gitmek faydalıdır. Tarayıcı web hizmetine başarıyla gidebilirse, sizden kimlik bilgilerinizi ister. Aynen dosyada göründüğü gibi web.config dosyasına girilen parola girilen kullanıcı adını ve parolayı girin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
* Mobil Uygulama Web Hizmeti web sunucusunun önünde ters bir proxy ya da güvenlik duvarı yer alıyorsa ve SSL boşaltma gerçekleştiriyorsa, Mobil Uygulama Web Hizmeti’nin https yerine http kullanabilmesi için Mobil Uygulama Web Hizmeti web.config dosyasını düzenleyebilirsiniz. Güvenlik duvarı/ters proxy’ye yönelik Mobil Uygulamadan alınan SSL hala gereklidir. Aşağıdaki anahtarları \<appSettings\> bölümüne ekleyin: 

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-service"></a>Hizmeti yükleme

1. Azure Multi-Factor Authentication Sunucusu’nda Windows Gezgini'ni açın ve Azure MFA Sunucusu’nun yüklü olduğu klasöre (genellikle C:\Program Files\Azure Multi-Factor Authentication yolundadır) gidin. Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup yükleme dosyasının 32 bit veya 64 bit sürümünü seçin. Yükleme dosyasını İnternet’e yönelik sunucuya kopyalayın.

2. İnternet’e yönelik web sunucusunda kurulum dosyasını yönetici haklarıyla çalıştırın. Yönetici olarak bir komut istemi açın ve yükleme dosyasının kopyalandığı konuma gidin.

3. Multi-Factor AuthenticationMobileAppWebServiceSetup yükleme dosyasını çalıştırın, isterseniz Site’yi değiştirin ve Sanal dizini “PA” gibi kısa bir adla değiştirin.

  Kullanıcıların Mobil Uygulama Web Hizmeti URL’sini etkinleştirme sırasında mobil cihaza girmesi gerektiğinden, kısa bir sanal dizin adın önerilir.

4. Azure Multi-Factor AuthenticationMobileAppWebServiceSetup yüklenmesi tamamlandıktan sonra, C:\inetpub\wwwroot\PA (veya sanal dizin adını temel alarak uygun dizin) gidin ve web.config dosyasını düzenleyin. 

5. WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ve WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD anahtarlarını bulun. Bu değerleri PhoneFactor Admins güvenlik grubunun üyesi olan bir hizmet hesabının kullanıcı adı ve parolası olarak ayarlayın. Daha önce yüklenmişse, bu Azure Multi-Factor Authentication Kullanıcı Portalı’nın Kimliği olarak kullanılanla aynı hesap olabilir. Satırın sonundaki tırnak işaretlerinin arasına, (value=””/>) Kullanıcı Adı ve Parolayı girdiğinizden emin olun. Etki alanı\kullanıcı adı veya makine\kullanıcı adı gibi tam bir kullanıcı adı kullanın.  

6. pfMobile App Web Service_pfwssdk_PfWsSdk ayarını bulun. *http://localhost:4898/PfWsSdk.asmx* değerini Azure Multi-Factor Authentication Sunucusu’nda çalışan Web Hizmeti SDK’sının URL’si (örneğin, https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) ile değiştirin. 

  Bu bağlantı için SSL kullanıldığından, Web Hizmeti SDK'sına IP adresi ile değil sunucu adı ile başvurmanız gerekir. SSL sertifikası sunucu adı için verilmiş olacaktır ve kullanılan URL’nin sertifikadaki adla eşleşmesi gerekir. Sunucu adı İnternet’e yönelik sunucudan bir IP adresine çözümlenmeyebilir. Bu durumda, Azure Multi-Factor Authentication Sunucusu’nun adını bu IP adresine eşlemek için bu sunucudaki hosts dosyasına bir giriş ekleyin. Değişiklikler yapıldıktan sonra web.config dosyasını kaydedin.

7. Mobil Uygulama Web Hizmeti’nin altında yüklendiği web sitesi halihazırda ortak olarak imzalanmış bir sertifikayla bağlanmadıysa sertifikayı sunucuya yükleyin, IIS Yöneticisi’ni açın ve sertifikayı web sitesine bağlayın.

8. Herhangi bir bilgisayarda web tarayıcısını açın ve Mobil Uygulama Web Hizmeti’nin yüklendiği URL'ye gidin (örneğin, https://www.publicwebsite.com/PA). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma
Artık mobil uygulama web hizmeti yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

1. Azure MFA Sunucusu’nda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. **Kullanıcıların Mobil Uygulamaları etkinleştirmesine izin ver** kutusunu işaretleyin.
3. **Kullanıcı Kaydına İzin Ver** kutusunu işaretleyin.
4. Mobil uygulama simgesine tıklayın.
5. Azure Multi-Factor AuthenticationMobileAppWebServiceSetup yüklenirken oluşturulan sanal dizinle kullanılan URL’yi girin. Sağlanan alana bir Hesap Adı girilebilir. Bu şirket adı mobil uygulamada görüntülenir. Bu alan boş bırakılırsa, klasik Azure portalında oluşturulan Multi-Factor Auth Sağlayıcınızın adı görüntülenir.

<center>![Kurulum](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>



<!--HONumber=Feb17_HO3-->


