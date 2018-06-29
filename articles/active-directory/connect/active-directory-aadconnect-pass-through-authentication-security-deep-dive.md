---
title: Azure Active Directory doğrudan kimlik doğrulaması güvenlik derin Dalış | Microsoft Docs
description: Şirket içi hesaplarınızı Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması nasıl koruduğunu bu makalede
services: active-directory
keywords: Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 967184d9a7590dc0b8c88a49cf178bbd9eb83267
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063604"
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory doğrudan kimlik doğrulaması güvenlik derinlemesine bakış

Bu makale, Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması nasıl çalıştığını daha ayrıntılı bir açıklama sağlar. Özellik Güvenlik yönlerini odaklanır. Bu makalede güvenlik ve BT yöneticileri, baş uyumluluk ve güvenlik görevlileri içindir ve BT güvenliği ve uyumluluğu en küçük ve orta sorumlu olan diğer BT uzmanlarının boyuta sahip kuruluşlar veya büyük kuruluşlar.

Ele konular şunlardır:
- Yüklemek ve kimlik doğrulama aracıları kaydetmek hakkında ayrıntılı teknik bilgi.
- Parolalar şifreleme kullanıcı oturum açma sırasında hakkında ayrıntılı teknik bilgiler.
- Kanallar güvenlik kimlik doğrulama aracıları ve Azure AD şirket içi.
- Kimlik Doğrulama Aracısı işletimsel güvenli tutmaya hakkında ayrıntılı teknik bilgi.
- Diğer güvenlikle ilgili konular.

## <a name="key-security-capabilities"></a>Anahtar güvenlik özellikleri

Bu özellik anahtar güvenlik yönleri şunlardır:
- Oturum açma isteklerinin kiracılar arasında yalıtım sağlar güvenli bir çoklu kiralanan mimari üzerine inşa edilmiştir.
- Şirket içi parolaları hiçbir zaman bulut herhangi bir biçimde depolanır.
- Böylece, şirket içi dinler ve yanıtlamak için parola doğrulaması yalnızca giden bağlantılar ağınızdaki isteklerde kimlik doğrulaması aracılar. Bir çevre ağında (DMZ) bu kimlik doğrulama aracıları yüklemek için gereksinimi yoktur.
- Yalnızca standart (80 ve 443 bağlantı noktalarını), Azure AD kimlik doğrulama aracılardan gelen giden iletişim için kullanılır. Güvenlik duvarını gelen bağlantı noktalarını açmanız gerekmez. 
  - Bağlantı noktası 443 tüm kimliği doğrulanmış giden iletişim için kullanılır.
  - Bağlantı noktası 80, bu özellik tarafından kullanılan sertifikaların hiçbirinin iptal edilmiş olduğundan emin olmak için yalnızca sertifika iptal listeleri (CRL'ler) yüklemek için kullanılır.
  - Ağ gereksinimleri tam listesi için bkz: [Azure Active Directory doğrudan kimlik doğrulaması: Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-the-prerequisites).
- Şirket içi kimlik doğrulama Aracısı bunlara karşı doğrulama Active Directory kabul etmeden önce bulutta oturum açma sırasında kullanıcılara sağlamak parolaları şifrelenir.
- Azure AD arasında HTTPS kanal ve şirket içi kimlik doğrulama Aracısı karşılıklı kimlik doğrulaması kullanılarak güvenlik altına alınır.
- Özellik sorunsuz bir şekilde koşullu erişim ilkeleri, (Azure çok faktörlü kimlik doğrulaması dahil) gibi Azure AD bulut koruması özelliklerine sahip kimlik koruması ve akıllı kilitleme tümleştirir.

## <a name="components-involved"></a>İlgili bileşenleri

Azure AD işletimsel, hizmet ve veri güvenliği hakkında genel bilgi için bkz [Güven Merkezi](https://azure.microsoft.com/support/trust-center/). Kullanıcı oturum açma için doğrudan kimlik doğrulaması kullandığınızda aşağıdaki bileşenleri oynayan:
- **Azure AD STS**: oturum açma isteklerini işleyen ve kullanıcıların tarayıcılar, istemciler veya gerektiği gibi hizmetler için güvenlik belirteçleri durum bilgisiz güvenlik belirteci hizmeti (STS).
- **Azure Service Bus**: Kurumsal Mesajlaşma ile iletişimi bulut etkin ve yardımcı olan geçişler iletişimi bağlanmak şirket içi çözümler ile bulut sağlar.
- **Azure AD Connect kimlik doğrulama Aracısı**: dinler ve parola doğrulaması isteklerine yanıt verdiğini bir şirket içi bileşeni.
- **Azure SQL veritabanı**: kiracınıza ait kimlik doğrulama aracıları, bunların meta verileri ve şifreleme anahtarlarına hakkında bilgi içerir.
- **Active Directory**: şirket içi etkin kullanıcı hesapları ve parolaları saklandığı dizin.

## <a name="installation-and-registration-of-the-authentication-agents"></a>Yükleme ve kimlik doğrulama Aracısı kaydı

Kimlik doğrulama aracısı yüklü ve Azure AD ile kayıtlı olduğunda, ya da:
   - [Azure AD ile doğrudan kimlik doğrulamasını etkinleştir Bağlan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature)
   - [Oturum açma isteklerini yüksek kullanılabilirliğini sağlamak için daha fazla kimlik doğrulama aracılarını ekleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability) 
   
Bir kimlik doğrulama aracısı çalışma alma üç ana aşamaları içerir:

1. Kimlik Doğrulama Aracısı yükleme
2. Kimlik Doğrulama Aracısı kaydı
3. Kimlik Doğrulama Aracısı başlatma

Aşağıdaki bölümlerde bu aşamalar ayrıntılı açıklanmaktadır.

### <a name="authentication-agent-installation"></a>Kimlik Doğrulama Aracısı yükleme

Yalnızca genel Yöneticiler bir şirket içi sunucu üzerinde bir kimlik doğrulama Aracısı (Azure AD Connect veya tek başına kullanarak) yükleyebilirsiniz. Yükleme için iki yeni giriş ekler **Denetim Masası** > **programları** > **programlar ve Özellikler** listesi:
- Kimlik Doğrulama Aracısı uygulama kendisi. Bu uygulama ile çalışır [NetworkService](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) ayrıcalıkları.
- Otomatik güncelleştirme kimlik doğrulama aracısı için kullanılan güncelleştirici uygulama. Bu uygulama ile çalışır [LocalSystem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları.

### <a name="authentication-agent-registration"></a>Kimlik Doğrulama Aracısı kaydı

Kimlik doğrulama aracısını yükledikten sonra kendi Azure AD ile kaydetmeniz gerekiyor. Azure AD, Azure AD ile güvenli iletişim için kullanabileceği benzersiz, dijital kimliğini sertifika her kimlik doğrulama Aracısı atar.

Kayıt yordamı, ayrıca, Kiracı kimlik doğrulama Aracısı bağlar. Bu, Azure AD bu belirli kimlik doğrulama Aracısı kiracınız için parola doğrulama isteklerini işlemek için yetkili tek olduğunu bilir sağlar. Bu yordam, kaydetmeniz her yeni kimlik doğrulama Aracısı yinelenir.

Kimlik Doğrulama Aracısı kendilerini Azure AD ile kaydetmek için aşağıdaki adımları kullanın:

![Aracı kaydı](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD genel yönetici Azure AD kimlik bilgilerini ile oturum açın, ilk kez istediğinde. Oturum açma sırasında kimlik doğrulama Aracısı adına genel yönetici kullanabileceğiniz bir erişim belirteci alır.
2. Kimlik Doğrulama Aracısı ardından bir anahtar çifti oluşturur: bir ortak anahtar ve özel anahtarı.
    - Anahtar çiftini üzerinden standart RSA 2048 bit şifreleme oluşturulur.
    - Özel anahtar, kimlik doğrulama Aracısı bulunduğu şirket içi sunucuda kalır.
3. Kimlik Doğrulama Aracısı "kayıt" Azure AD ile HTTPS üzerinden, istekte bulunan aşağıdaki bileşenleri ile istekte bulunur:
    - 1. adımda alınan erişim belirteci.
    - 2. adımda oluşturulan ortak anahtarı.
    - Sertifika imzalama isteği (CSR veya sertifika isteği). Bu istek, sertifika yetkilisi (CA) olarak Azure AD ile bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD kaydı isteği erişim belirteci doğrular ve isteği bir genel yönetici geldiğini doğrular.
5. Ardından Azure AD imzalar ve dijital kimlik sertifikası kimlik doğrulaması aracıya geri gönderir.
    - Azure AD'de kök CA sertifikasını imzalamak için kullanılır. 

     >[!NOTE]
     > Bu CA'dır _değil_ Windows güvenilen kök sertifika yetkilileri alanında depolar.
    - CA'ın yalnızca doğrudan kimlik doğrulama özelliği tarafından kullanılır. CA'ın yalnızca kimlik doğrulama Aracısı kaydı sırasında CSR imzalamak için kullanılır.
    -  Diğer Azure AD hizmetlerinde hiçbiri bu CA'yı kullanın.
    - Sertifikanın konu (ayırt edici adı veya DN) Kiracı kimliğinizi ayarlayın Bu DN Kiracı benzersiz olarak tanımlayan bir GUID'dir. Bu DN sadece Kiracı kullanımı için sertifika kapsamları.
6. Azure AD kimlik doğrulama Aracısı'nın ortak anahtarı yalnızca Azure AD erişimi olan bir Azure SQL veritabanında depolar.
7. (5. adımda verilen) sertifikası şirket içi sunucunun Windows sertifika deposunda depolanır (özellikle de [CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE) konumu). Hem kimlik doğrulama Aracısı hem de güncelleştirici uygulamalar tarafından kullanılır.

### <a name="authentication-agent-initialization"></a>Kimlik Doğrulama Aracısı başlatma

Kimlik Doğrulama Aracısı başladığında, bir sunucu veya kayıt sonra ilk kez yeniden başlatın, parola doğrulama isteklerini kabul başlatmak ve Azure AD hizmetiyle güvenli iletişim için bir yol gerekiyor.

![Aracı başlatma](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

İşte nasıl kimlik doğrulaması aracıları başlatılır:

1. Kimlik Doğrulama Aracısı, Azure AD ile giden bir önyükleme isteği yapar. 
    - Bu istek bağlantı noktası 443 üzerinden yapılan ve karşılıklı kimlik doğrulaması HTTPS kanal oluşturur. İstek, kimlik doğrulama Aracısı kaydı sırasında verilen aynı sertifikayı kullanır.
2. Azure AD kiracınıza özgüdür ve Kiracı kimliğinizi tarafından tanımlanan bir Azure Service Bus kuyruğuna erişim tuşu sağlayarak isteğe yanıt.
3. Kimlik Doğrulama Aracısı kalıcı bir giden HTTPS bağlantı (üzerinden bağlantı noktası 443) sıra yapar. 
    - Kimlik Doğrulama Aracısı artık almak ve parola doğrulama isteklerini işlemek hazırdır.

Varsa, Kiracı'birden çok kimlik doğrulama aracı kayıtlı sonra her biri aynı Service Bus kuyruğuna bağlayan Başlatma yordamını sağlar. 

## <a name="process-sign-in-requests"></a>Oturum açma işlem isteği 

Aşağıdaki diyagramda, doğrudan kimlik doğrulama kullanıcı oturum açma isteklerinin nasıl işlediği gösterilmektedir.

![İşlem oturum açma](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Doğrudan kimlik doğrulaması oturum açma kullanıcı isteği şu şekilde yapar: 

1. Bir kullanıcı bir uygulama, örneğin, erişmeyi dener [Outlook Web App](https://outlook.office365.com/owa).
2. Kullanıcı zaten oturum açmışsa değil, uygulama tarayıcı Azure AD oturum açma sayfasına yönlendirir.
3. İle Azure AD STS hizmet yanıt geri **kullanıcı oturum açma** sayfası.
4. Kullanıcı adlarını içine kullanıcının girdiği **kullanıcı oturum açma** sayfası ve seçer **sonraki** düğmesi.
5. Kullanıcı parolasını içine girer **kullanıcı oturum açma** sayfası ve seçer **oturum açma** düğmesi.
6. Kullanıcı adı ve parola Azure AD STS HTTPS POST isteği gönderilir.
7. Azure AD STS kiracınızın Azure SQL veritabanından kayıtlı tüm kimlik doğrulama aracıları için ortak anahtarları alır ve bunları kullanarak parola şifreler. 
    - Üzerinde kiracınızın kayıtlı "N" kimlik doğrulaması aracılar için "N" şifrelenmiş parola değerlerini üretir.
8. Azure AD STS, kullanıcı adı ve Service Bus kuyruğuna kiracınız için belirli üzerine şifrelenmiş parola değerlerini oluşan parola doğrulama isteği yerleştirir.
9. Başlatılmış kimlik doğrulaması aracılarını kalıcı olarak Service Bus kuyruğuna bağlandığından, kullanılabilen kimlik doğrulama aracıları birini parola doğrulama isteği alır.
10. Kimlik Doğrulama Aracısı, ortak anahtarı için belirli bir tanımlayıcıyı kullanarak ve özel anahtarını kullanarak çözer şifrelenmiş parola değeri bulur.
11. Kimlik doğrulama aracısını kullanarak kullanıcı adı ve parola şirket içi Active Directory karşı doğrulama girişiminde [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx) ile **dwLogonType** parametre için **LOGON32_LOGON_NETWORK**. 
    - Bu API, Active Directory Federasyon Hizmetleri (AD FS) federe oturum açma senaryosunda kullanıcılar imzalamak için kullanılan aynı bir API'dir.
    - Bu API standart çözümleme işlemi Windows Server'ın etki alanı denetleyicisi bulmak için kullanır.
12. Sonuç Active Directory'den başarı, kullanıcı adı veya hatalı parola gibi kimlik doğrulama Aracısı alır veya parolanın süresi doldu.
13. Kimlik Doğrulama Aracısı bağlantı noktası 443 üzerinden giden bir karşılıklı kimlik doğrulaması HTTPS kanalı üzerinden Azure AD STS sonucu iletir. Karşılıklı kimlik doğrulaması için kimlik doğrulama Aracısı kaydı sırasında daha önce verilen sertifikayı kullanır.
14. Azure AD STS bu sonucu belirli oturum açma isteği kiracınız karşılık gelen doğrular.
15. Azure AD STS ile oturum açma yordamı yapılandırıldığı şekilde devam eder. Örneğin, parola doğrulama başarılı olursa, kullanıcı için çok faktörlü kimlik doğrulaması yüküyle veya yeniden uygulamaya yönlendirilir.

## <a name="operational-security-of-the-authentication-agents"></a>Kimlik doğrulama aracıların işlem güvenliği

Doğrudan kimlik doğrulama işletimsel güvenli kalmasını sağlamak için düzenli aralıklarla Azure AD kimlik doğrulama Aracısı sertifikalarını yeniler. Azure AD yenilemeleri tetikler. Kimlik doğrulama aracı tarafından kendileri yenilemeleri yönetilmeyen.

![İşletimsel güvenlik](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Bir kimlik doğrulama aracısının güven Azure AD ile yenilemek için:

1. Kimlik Doğrulama Aracısı Azure düzenli aralıklarla ping AD birkaç saatte sertifikasını yenilemek için zaman olup olmadığını denetleyin. 
    - Bu onay karşılıklı kimlik doğrulaması HTTPS kanal üzerinden yapılır ve kayıt sırasında verilen aynı sertifikayı kullanır.
2. Hizmet yenilemek için zaman olduğunu gösteriyorsa, kimlik doğrulama aracısı yeni bir anahtar çifti oluşturur: bir ortak anahtar ve özel anahtarı.
    - Bu anahtarları üzerinden standart RSA 2048 bit şifreleme üretilir.
    - Özel anahtar hiçbir zaman şirket içi sunucunun bırakır.
3. Kimlik Doğrulama Aracısı sonra "sertifika yenileme" Azure AD ile HTTPS üzerinden, istekte bulunan aşağıdaki bileşenleri ile istekte bulunur:
    - Windows sertifika deposunda CERT_SYSTEM_STORE_LOCAL_MACHINE konumunda alınır mevcut sertifika. Olmadığından genel bir yönetici bu yordamda, söz konusu var. adına genel yönetici gerekli herhangi bir erişim belirteci.
    - 2. adımda oluşturulan ortak anahtarı.
    - Sertifika imzalama isteği (CSR veya sertifika isteği). Bu istek, sertifika yetkilisi olarak Azure AD ile yeni bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD varolan bir sertifikayı, sertifika yenileme isteğini doğrular. Daha sonra istek üzerinde kiracınızın kayıtlı bir kimlik doğrulama Aracısı geldiğini doğrular.
5. Var olan sertifikasının hala geçerli ise, Azure AD sonra yeni bir dijital kimliği sertifika imzalar ve yeni sertifika kimlik doğrulaması aracıya geri verir. 
6. Var olan sertifikasının süresi dolduysa, Azure AD kimlik doğrulama Aracısı kayıtlı kimlik doğrulaması aracıları, kiracının listeden siler. Ardından bir genel yönetici el ile yüklemeniz ve yeni bir kimlik doğrulama Aracısı kaydetmeniz gerekir.
    - Azure AD kök CA sertifikasını imzalamak için kullanın.
    - Sertifikanın konu (ayırt edici adı veya DN), Kiracı kimliği Kiracı benzersiz olarak tanımlayan bir GUID ayarlayın. DN'si kiracınızı yalnızca sertifikaya kapsamları.
6. Azure AD kimlik doğrulama Aracısı'nın yeni ortak anahtarı ancak erişimi olan bir Azure SQL veritabanında depolar. Ayrıca kimlik doğrulama aracı ile ilişkili eski ortak anahtar geçersiz kılar.
7. (5. adımda verilen) yeni sertifika daha sonra sunucunun Windows sertifika deposunda depolanır (özellikle de [CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER) konumu).
    - Etkileşimli olmayan (varlığını genel yönetici) güven yenileme yordamı olur çünkü kimlik doğrulama Aracısı CERT_SYSTEM_STORE_LOCAL_MACHINE konumda var olan bir sertifikayı güncelleştirmek için erişim artık yok. 
    
   > [!NOTE]
   > Bu yordamı sertifika CERT_SYSTEM_STORE_LOCAL_MACHINE konumdan kaldırmaz.
8. Yeni sertifika bu noktasından kimlik doğrulaması için kullanılır. Sonraki her sertifikanın yenilenmesini CERT_SYSTEM_STORE_LOCAL_MACHINE konumun sertifikada değiştirir.

## <a name="auto-update-of-the-authentication-agents"></a>Kimlik doğrulama aracıları otomatik güncelleştirme

Yeni bir sürümü yayımlandığında güncelleştirici uygulama kimlik doğrulama Aracısı otomatik olarak güncelleştirir. Uygulama, kiracınız için olan parola doğrulama isteklerini işlemez. 

Azure AD barındıran işaretli olarak yazılımın yeni sürümünü **Windows Installer paketi (MSI)**. MSI kullanılarak imzalanmış [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) Özet algoritması olarak SHA256 ile. 

![Otomatik güncelleştirme](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

Bir kimlik doğrulama Aracısı otomatik güncelleştirme için:

1. Azure AD güncelleştirici uygulama ping kullanılabilir kimlik doğrulama Aracısı'nın yeni bir sürümü olup olmadığını denetlemek için her saat. 
    - Bu denetim, kayıt sırasında verilen aynı sertifika ile karşılıklı kimlik doğrulaması HTTPS kanal üzerinden yapılır. Kimlik Doğrulama Aracısı ve güncelleştirici sunucuda depolanan sertifika paylaşır.
2. Yeni bir sürümü kullanılabilir durumda, Azure AD imzalı MSI geri güncelleştirici döndürür.
3. MSI Microsoft tarafından imzalanır güncelleştirici doğrular.
4. Güncelleştirici MSI çalışır. Bu eylem, aşağıdaki adımları içerir:

 > [!NOTE]
 > İle güncelleştirici çalıştırır [yerel sistem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları.

    - Kimlik Doğrulama Aracı hizmetini durdurur
    - Kimlik Doğrulama Aracısı'nın yeni sürümü sunucuya yükler
    - Kimlik Doğrulama Aracısı hizmetini yeniden başlatır

>[!NOTE]
>Kiracı'kayıtlı birden çok kimlik doğrulama aracısı varsa, Azure AD sertifikalarını yenileme değil veya aynı anda güncelleştirin. Bunun yerine, Azure AD, oturum açma isteklerini yüksek kullanılabilirliğini sağlamak için kademeli olarak böylece yapar.
>


## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD doğrudan kimlik doğrulamasını başlamak ve çalıştırmak.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için Kiracı akıllı kilitleme yeteneği yapılandırın.
- [Nasıl çalışır](active-directory-aadconnect-pass-through-authentication-how-it-works.md): Azure AD doğrudan kimlik doğrulama nasıl çalıştığını temellerini öğrenin.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul için sık sorulan sorulara yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): doğrudan kimlik doğrulama özelliği ile ortak sorunları çözmeyi öğrenin.
- [Azure AD sorunsuz SSO](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
