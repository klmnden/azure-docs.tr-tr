<properties
    pageTitle="Sorun giderme: Bu uygulamaya buradan erişemezsiniz | Microsoft Azure"
    description="Bu konu, ulaşmak istediğiniz uygulamaya erişmeniz için izlemeniz gereken düzeltme adımlarını belirlemenize yardımcı olur."
    services="active-directory"
    keywords="cihaz temelli koşullu erişim, cihaz kaydı, cihaz kaydını etkinleştirme, cihaz kaydı ve MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# Sorun giderme: Bu uygulamaya buradan erişemezsiniz

Office 365 SharePoint Online gibi bir uygulamaya erişirken erişim engellendi sayfasıyla karşılaştınız.  
Şimdi ne yapacaksınız?

Bu kılavuz, ulaşmak istediğiniz uygulamaya erişmeniz için izlemeniz gereken düzeltme adımlarını belirlemenize yardımcı olur.



Cihazınız hangi cihaz platformunda çalışıyor?
Bu sorunun yanıtı sizi bu konu başlığındaki doğru bölüme götürür:
 

-   Windows cihazı

-   iOS cihazı (iPhone veya iPad)

-   Android cihazı

## Windows cihazından erişim

Cihazınız Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 ise uygun nedeni, uygulamaya erişmeye çalıştığınızda karşınıza çıkan sayfayı tanımlayarak seçin.

### Cihaz kayıtlı değil

Cihazınız Azure Active Directory’ye kayıtlı değilse ve uygulama, cihaz temelli bir ilkeyle korunuyorsa aşağıdaki içeriğe sahip bir sayfa görebilirsiniz:

![Senaryo](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

 

Cihazınız, kuruluşunuzun Active Directory etki alanına katılmışsa aşağıdakileri deneyebilirsiniz:

1.  Windows’da iş hesabınızı (Active Directory hesabı) kullanarak oturum açtığınızdan emin olun.

2.  Kurumsal ağınıza VPN veya Doğrudan Erişim aracılığıyla bağlanın.

3.  Bağlantı kurulduktan sonra Windows tuşu + ‘L’ tuşunu kullanarak Windows oturumunuzu kilitleyin.

4.  İş hesabınızın kimlik bilgilerini girerek Windows oturumunuzun kilidini açın.

5.  Bir dakika bekleyin ve uygulamaya yeniden erişmeyi deneyin.

6.  Aynı sayfayla karşılaşırsanız yöneticinize başvurun ve ‘Ayrıntılı bilgi’ bağlantısına tıkladıktan sonra ayrıntıları belirtin.

Cihazınız etki alanına katılmamışsa ve Windows 10 çalıştırıyorsa iki seçeneğiniz vardır: 

1. Azure AD'ye Katılım’ı çalıştırmak
2. Windows’a iş veya okul hesabınızı eklemek. 

İki seçenek arasındaki farklar hakkında daha fazla bilgi için bkz. [Çalışma alanınızda Windows 10 cihazlarını kullanma](active-directory-azureadjoin-windows10-devices.md).

Azure AD’ye Katılım’ı çalıştırmak için şunları yapın (Windows Phone’da kullanılamaz):

**Windows 10 Yıldönümü Güncelleştirmesi**   

1.  Ayarlar uygulamasını başlatın.

2.  ‘Hesaplar’a, sonra da ‘İş veya okula erişim’ seçeneğine gidin.

3.  ‘Bağlan’a tıklayın.

4.  Sayfanın alt kısmındaki ‘Bu cihazı Azure AD’ye ekle’yi seçin.

5.  Kuruluşunuzun kimliğini doğrulayın, gerekirse MFA kanıtı belirtin ve işlem tamamlanana kadar adımları izleyin.

6.  Oturumu kapatın ve iş hesabınızı kullanarak oturum açın.

7.  Uygulamaya yeniden erişmeyi deneyin.




**Windows 10 Kasım 2015 Güncelleştirmesi**


1.  Ayarlar uygulamasını başlatın.

2.  ‘Sistem’e, sonra da ‘Hakkında’ya gidin.
    
3.  ‘Azure AD'ye Katıl’a tıklayın.

4.  Kuruluşunuzun kimliğini doğrulayın, gerekirse MFA kanıtı belirtin ve işlem tamamlanana kadar adımları izleyin.

5.  Oturumu kapatın ve iş hesabınızı (Azure AD hesabı) kullanarak oturum açın.

6.  Uygulamaya yeniden erişmeyi deneyin.


İş veya okul hesabınızı eklemek için aşağıdakileri yapın:

**Windows 10 Yıldönümü Güncelleştirmesi**   

1.  Ayarlar uygulamasını başlatın.

2.  ‘Hesaplar’a, sonra da ‘İş veya okula erişim’ seçeneğine gidin.

3.  ‘Bağlan’a tıklayın.

4.  Kuruluşunuzun kimliğini doğrulayın, gerekirse MFA kanıtı belirtin ve işlem tamamlanana kadar adımları izleyin.

5.  Uygulamaya yeniden erişmeyi deneyin.    


**Windows 10 Kasım 2015 Güncelleştirmesi**
    
1.  Ayarlar uygulamasını başlatın.
2.  ‘Hesaplar’a, sonra da ‘Hesaplarınız’a gidin.
3.  ‘İş veya okul hesabı ekle’ye tıklayın.
4.  Kuruluşunuzun kimliğini doğrulayın, gerekirse MFA kanıtı belirtin ve işlem tamamlanana kadar adımları izleyin.
5.  Uygulamaya yeniden erişmeyi deneyin.

Cihazınız etki alanına katılmamışsa ve Windows 8.1 çalıştırıyorsa Çalışma Alanına Katılım gerçekleştirebilir ve aşağıdakileri yaparak Microsoft Intune’a kaydolabilirsiniz:

1.  PC Ayarları'nı başlatın.

2.  ‘Ağ’a, sonra da ‘Çalışma Alanı’na gidin.

3.  ‘Katıl’a tıklayın.

4.  Kuruluşunuzun kimliğini doğrulayın, gerekirse MFA kanıtı belirtin ve işlem tamamlanana kadar adımları izleyin.

5.  ‘Aç’a tıklayın.

6.  İşlem tamamlanana kadar bekleyin.

7.  Uygulamaya yeniden erişmeyi deneyin.


## Desteklenmeyen tarayıcı

Uygulamaya şu tarayıcılardan erişiyorsanız aşağıdakine benzer bir sayfayla karşılaşırsınız:

1.  Windows 10 veya Windows Server 2016’da Chrome, Firefox veya Microsoft Edge ya da Microsoft Internet Explorer olmayan diğer tüm tarayıcılar.

2.  Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2’de Firefox.
 

![Senaryo](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")


Tek düzeltme seçeneği, cihaz platformunuz tarafından desteklenen bir tarayıcı kullanmaktır.

## iOS cihazından erişim

iPhone’lara veya iPad’lere yönelik yönergeler yakında paylaşılacaktır.

## Android cihazından erişim

Android telefonlara veya tabletlere yönelik yönergeler yakında paylaşılacaktır.


## Sonraki adımlar

[Azure Active Directory koşullu erişimi](active-directory-conditional-access.md)




<!--HONumber=Aug16_HO4-->


