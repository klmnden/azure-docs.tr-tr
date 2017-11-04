---
title: "Azure Active Directory doğrudan kimlik doğrulaması güvenlik derin-Dalış | Microsoft Docs"
description: "Bu makalede, şirket içi hesaplarınızı Azure Active Directory (Azure AD) doğrudan kimlik doğrulama nasıl koruduğunu açıklanmaktadır."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: billmath
ms.openlocfilehash: a5feadd851b166d0a9a77bd1d124cdd599d5d701
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory doğrudan kimlik doğrulaması güvenlik derin-Dalış

Bu makale, geçişli kimlik doğrulaması nasıl çalıştığını daha ayrıntılı bir açıklama sağlar. Daha fazla özellik güvenlik yönlerini de odaklanır. Bu konuda, güvenlik ve BT yöneticileri, baş uyumluluk ve güvenlik görevlileri ve diğer BT uzmanlarının sorumlu BT güvenliği ve uyumluluğu küçük ve orta kuruluşlar veya büyük ölçekli işletmeler için ilgi olacaktır.

Ele konular şunlardır:
- Kimlik doğrulama aracıları nasıl yüklü ve kayıtlı hakkında ayrıntılı teknik bilgi.
- Kullanıcı oturum açma sırasında parola şifreleme hakkında ayrıntılı teknik bilgiler.
- Kanallar güvenlik kimlik doğrulama aracıları ve Azure Active Directory (Azure AD) şirket içi.
- Nasıl kimlik doğrulaması aracıları işletimsel güvenli tutulması hakkında ayrıntılı teknik bilgi.
- Diğer güvenlikle ilgili konular.

## <a name="key-security-capabilities"></a>Anahtar güvenlik özellikleri

Bu özellik anahtar güvenlik yönleri şunlardır:
- Oturum açma isteklerinin kiracılar arasında yalıtım sağlar güvenli bir çoklu kiralanan mimari üzerine inşa edilmiştir.
- Şirket içi parolaları hiçbir zaman bulut herhangi bir biçimde depolanır.
- Dinler ve parola doğrulaması isteklerine yanıt, şirket içi kimlik doğrulaması aracılar, yalnızca giden bağlantılar ağınızdaki olun. Bir çevre ağında (DMZ) bu kimlik doğrulama aracıları yüklemek için gereksinimi yoktur.
- Yalnızca standart bağlantı noktaları (80 ve 443) Azure AD kimlik doğrulama aracılardan gelen giden iletişim için kullanılır. Hiçbir gelen bağlantı noktalarının güvenlik duvarını açılması gerekir. 
  - Bağlantı noktası 443 tüm kimliği doğrulanmış giden iletişim için kullanılır
  - Bağlantı noktası 80 yalnızca sertifika iptal listeleri (CRL'ler) yüklemek için özelliği tarafından kullanılan sertifikaların hiçbirinin iptal edilmiş emin olmak için kullanılır.
  - Bkz: [burada](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites) ağ gereksinimleri tam listesi için.
- Tarafından kabul edilen kimlik doğrulama Aracısı karşı doğrulama, Active Directory içi önce bulutta oturum açma sırasında kullanıcı tarafından sağlanan parolaları şifrelenir.
- Azure AD arasında HTTPS kanal ve bir şirket içi kimlik doğrulama Aracısı karşılıklı kimlik doğrulaması tarafından korunmaktadır.
- Bunu sorunsuz bir şekilde Azure AD bulut koruma özellikleri (çok faktörlü kimlik doğrulaması dahil), koşullu erişim ilkeleri gibi kimlik koruması ve akıllı kilitleme tümleşiktir.

## <a name="components-involved"></a>İlgili bileşenleri

Azure AD işletimsel, hizmet ve veri güvenliği hakkında genel bilgi için bkz [Güven Merkezi](https://azure.microsoft.com/support/trust-center/). Kullanıcı oturum açma için doğrudan kimlik doğrulama kullanıldığında, aşağıdaki bileşenleri oynayan:
- **Azure AD STS**: bir durum bilgisi olmayan güvenlik belirteci hizmeti (oturum açma isteklerini işleyen ve kullanıcıların tarayıcılar, istemciler veya gerektiği gibi hizmetler için güvenlik belirteçleri STS).
- **Azure Service Bus**: Kurumsal Mesajlaşma ile iletişimi bulut etkin ve yardımcı olan geçişler iletişimi bağlanmak şirket içi çözümler ile bulut sağlar.
- **Azure AD Connect kimlik doğrulama Aracısı (kimlik doğrulama Aracısı)**: dinler ve parola doğrulaması isteklerine yanıt verdiğini bir şirket içi bileşeni.
- **Azure SQL veritabanı**: meta verileri ve şifreleme anahtarları dahil kiracınıza ait kimlik doğrulama aracılar ile ilgili bilgileri tutar.
- **Active Directory (AD)**: şirket içi Active kullanıcı hesaplarınızı (ve bunların parolalarını) saklandığı dizin,.

## <a name="installation-and-registration-of-authentication-agents"></a>Yükleme ve kimlik doğrulama Aracısı kaydı

Kimlik doğrulama aracısı yüklü ve Azure AD ile kayıtlı olduğunda, [doğrudan Azure AD Connect'i kullanarak kimlik doğrulamasını etkinleştir](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature) veya ne zaman, [yüksek oranda kullanılabilirliğini sağlamak için ek kimlik doğrulama Aracısı Ekle oturum açma isteklerini](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability). Bir kimlik doğrulama aracısı çalışma alma üç ana aşamaları içerir:

- Kimlik Doğrulama Aracısı yükleme
- Kimlik Doğrulama Aracısı kaydı
- Kimlik Doğrulama Aracısı başlatma

Bunların her biri aşağıdaki konularda ayrıntılı incelenecektir.

### <a name="authentication-agent-installation"></a>Kimlik Doğrulama Aracısı yükleme

Yalnızca genel Yöneticiler (Azure AD Connect veya tek başına kullanarak) bir kimlik doğrulama Aracısı bir şirket içi sunucuya yükleyebilirsiniz. Yüklemesi için bu iki yeni giriş ekler **Denetim Masası -> Programlar -> Programlar ve Özellikler** listesi:
- Kimlik Doğrulama Aracısı uygulama kendisi. Bu ile çalıştırır [ağ hizmeti](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) ayrıcalıkları.
- Otomatik kimlik doğrulama aracısını güncelleştirmek için kullanılan güncelleştirici uygulama. Bu ile çalıştırır [yerel sistem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları.


### <a name="authentication-agent-registration"></a>Kimlik Doğrulama Aracısı kaydı

Kimlik doğrulama aracısını yükledikten sonra kendi Azure AD ile kaydetmeniz gerekiyor. Azure AD bunu Azure AD ile güvenli iletişim için kullanabileceği benzersiz dijital kimlik sertifikası her kimlik doğrulama Aracısı atayarak yapar.

Azure AD bu belirli kimlik doğrulama Aracısı kiracınız için parola doğrulama isteklerini işlemek için yetkili tek olduğunu bilmesi için kayıt yordamı, Kiracı kimlik doğrulama aracısı ayrıca bağlar. Bu yordam, kaydetmeniz her yeni kimlik doğrulama Aracısı yinelenir.

İşte nasıl kimlik doğrulaması aracıları kendilerini Azure AD'ye kaydedin:

![Aracı kaydı](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta1.png)

1. Azure AD, ilk Azure AD kimlik bilgilerini ile oturum açmak için genel yönetici ister. Sırasında oturum açma, kimlik doğrulama Aracısı bir erişim adına genel yönetici kullanabileceğiniz belirteci alır.
2. Kimlik Doğrulama Aracısı ardından bir anahtar çifti – bir ortak anahtar ve özel anahtarı oluşturur.
    - Bu anahtar çiftini standart kullanılarak oluşturulan **RSA 2048 bit** şifreleme.
    - Özel anahtar asla kimlik doğrulama Aracısı yüklenmiş olduğu şirket içi sunucu bırakır.
3.  Kimlik Doğrulama Aracısı "kayıt" Azure AD ile HTTPS üzerinden, istekte bulunan aşağıdaki bileşenleri ile istekte bulunur:
    - Erişim adım 1'de edinilen belirteci.
    - 2. adımda oluşturulan ortak anahtarı.
    - A **sertifika imzalama isteği** (CSR veya sertifika isteği). Bu, sertifika yetkilisi olarak Azure AD ile bir dijital kimlik sertifikası için uygulamaktır.
4. Azure AD kaydı isteği erişim belirteci doğrular ve isteği bir genel yönetici geldiğini doğrular.
5. Azure AD sonra imzalar ve dijital kimliği sertifika kimlik doğrulaması aracıya geri verir.
    - Sertifika ile imzalanmış **Azure AD kök sertifika yetkilisi (CA)**. Bu CA olduğuna dikkat edin _değil_ Windows 's **güvenilen kök sertifika yetkilileri** depolar.
    - Bu CA'ın yalnızca doğrudan kimlik doğrulama özelliği tarafından kullanılır. Bu gibi durumlarda bu için imzalama CSR yalnızca kimlik doğrulama Aracısı kaydı sırasında kullanılır.
    - Bu CA'ın Azure AD içinde herhangi bir hizmet tarafından kullanılmaz.
    - Sertifikanın konu (**ayırt edici ad veya**) ayarlamak, **Kiracı kimliği**. Bu, Kiracı benzersiz olarak tanımlayan bir GUID'dir. Yalnızca Kiracı ile kullanmak için sertifika kapsamları.
6. Azure AD kimlik doğrulama Aracısı'nın ortak anahtarı yalnızca Azure AD erişimi olan bir Azure SQL veritabanında depolar.
7. (5. adımda verilen) sertifika şirket içi sunucuda depolanan **Windows sertifika deposunda** (özellikle de  **[CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE)**  Konum) ve hem kimlik doğrulama Aracısı hem de güncelleştirici uygulamalar tarafından kullanılır.

### <a name="authentication-agent-initialization"></a>Kimlik Doğrulama Aracısı başlatma

Kimlik Doğrulama Aracısı başladığında, bir sunucu veya kayıt sonra ilk kez yeniden başlatın, güvenli parola doğrulaması isteklerini kabul başlatmak ve Azure AD hizmeti ile iletişim için bir yol gerekiyor.

![Aracı başlatma](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta2.png)

Kimlik doğrulama aracıları nasıl başlatılan aşağıda verilmiştir:



1. Kimlik Doğrulama Aracısı, Azure AD ile giden "önyükleme" istek yaparak başlatır. 
    - Bu önyükleme istek bağlantı noktası 443 üzerinden yapılan ve (kimlik doğrulama Aracısı kaydı sırasında verilen aynı sertifikayı kullanarak) karşılıklı kimlik doğrulaması bir HTTPS kanalı sona erer.
2. Azure AD sağlayarak bu önyükleme isteğine yanıt bir **erişim tuşu** (Kiracı Kimliğinizi tarafından tanımlanan) kiracınız için benzersiz olan bir Azure Service Bus kuyruğuna.
3. Kimlik Doğrulama Aracısı kalıcı bir giden HTTPS bağlantı (üzerinden bağlantı noktası 443) sıra yapar. 
    - Artık, almak ve parola doğrulama isteklerini işlemek hazırdır.

Varsa, Kiracı'birden çok kimlik doğrulama aracı kayıtlı sonra her biri aynı Azure Service Bus kuyruğuna bağlayan Başlatma yordamını sağlar. 

## <a name="processing-sign-in-requests"></a>Oturum açma isteklerini işleme 

Aşağıdaki diyagramda, doğrudan kimlik doğrulama kullanıcı oturum açma isteklerinin nasıl işlediği gösterilmektedir.

![Oturum açma işleme](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta3.png)

Doğrudan kimlik doğrulaması oturum açma kullanıcı isteği şu şekilde yapar: 

1. Bir kullanıcı bir uygulamaya erişmeye çalıştığında (örneğin, Outlook Web App - [https://outlook.office365.com/owa](https://outlook.office365.com/owa).)
2. Kullanıcı zaten oturum açmışsa değil, uygulama tarayıcı Azure AD oturum açma sayfasına yönlendirir.
3. Azure AD STS hizmeti kullanıcı oturum açma sayfasına geri ile yanıt verir.
4. Kullanıcının kullanıcı adı ve parola Azure AD oturum açma sayfasına girer ve "Oturum" düğmesine tıklar.
5. Kullanıcı adı ve parola Azure AD STS HTTPS POST isteği gönderilir.
6. Azure AD STS kiracınızın Azure SQL veritabanından kayıtlı tüm kimlik doğrulama aracılar için ortak anahtarları alır ve bunları kullanarak parola şifreler. 
    - 'N' şifrelenmiş parola üreten 'N' kimlik doğrulama aracısı üzerinde kiracınızın kayıtlı değerleri.
7. Azure AD STS parola doğrulama isteği (kullanıcı adı ve şifrelenmiş parola değerlerini) Azure Service Bus kuyruğuna üzerine kiracınız için belirli yerleştirir.
8. Azure Service Bus kuyruğuna başlatılmış kimlik doğrulaması aracılarını kalıcı olarak bağlı olduğundan, kullanılabilen kimlik doğrulama aracıları birini parola doğrulama isteği alır.
9. Kimlik Doğrulama Aracısı (tanımlayıcıyı kullanarak), ortak anahtarı belirli şifrelenmiş parola değeri bulur ve özel anahtarını kullanarak şifresini çözer.
10. Kullanıcı adı ve parola kullanarak şirket içi Active Directory karşı doğrulamak kimlik doğrulama aracısı çalışır  **[Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)**  (ile **dwLogonType** parametre kümesine **LOGON32_LOGON_NETWORK**). 
    - Bu, kullanıcıların oturum açma, federe senaryoda imzalamak için Active Directory Federasyon Hizmetleri (AD FS) tarafından kullanılan aynı API'dir.
    - Bu etki alanı Contoller bulmak için Windows Server'daki standart çözümleme işlemi kullanır.
11. Kimlik Doğrulama Aracısı sonucu Active Directory'den alır (başarı, kullanıcı adı veya hatalı parola, parolanın süresi doldu, kapatma, vb. kullanıcı kilitli).
12. Kimlik Doğrulama Aracısı bağlantı noktası 443 üzerinden giden bir karşılıklı kimlik doğrulaması HTTPS kanalı üzerinden Azure AD STS sonucu iletir. Karşılıklı kimlik doğrulaması için kimlik doğrulama Aracısı kaydı sırasında daha önce verilen aynı sertifikayı kullanır.
13. Azure AD STS bu sonucu belirli oturum açma isteği kiracınız karşılık gelen doğrular.
14. Azure AD STS ile oturum açma yordamı yapılandırıldığı şekilde devam eder. Örneğin, parola doğrulama başarılı olursa, kullanıcı çok faktörlü kimlik doğrulaması için kimlik doğrulaması veya yeniden uygulamaya yönlendirilir.

## <a name="operational-security-of-authentication-agents"></a>Kimlik doğrulama aracıların işlem güvenliği

Doğrudan kimlik doğrulama işletimsel güvenli kalmasını sağlamak için düzenli aralıklarla Azure AD sertifikalarını yeniler. Bu yenileme Azure AD'den tetiklenir ve kimlik doğrulama aracıları kendileri tarafından yönetilmeyen.

![İşletimsel güvenlik](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta4.png)

Bir kimlik doğrulama Aracısı kendi güven Azure AD ile nasıl yeniler aşağıda verilmiştir:

1. Kimlik Doğrulama Aracısı Azure düzenli aralıklarla ping sertifikasını yenilemek için zaman olup olmadığını denetlemek için AD (birkaç saatte). 
    - Bu denetim, (kayıt sırasında verilen aynı sertifikayı kullanarak) karşılıklı kimlik doğrulaması bir HTTPS kanalı üzerinden yapılır.
2. Hizmet yenilemek için zaman olduğunu gösteriyorsa, kimlik doğrulama aracısı yeni bir anahtar çifti oluşturur: bir ortak anahtar ve özel anahtarı.
    - Bu anahtarları standart kullanılarak oluşturulan **RSA 2048 bit** şifreleme.
    - Özel anahtar hiçbir zaman şirket içi sunucunun bırakır.
3. Kimlik Doğrulama Aracısı sonra "sertifika yenileme" Azure AD ile HTTPS üzerinden, istekte bulunan aşağıdaki bileşenleri ile istekte bulunur:
    - Varolan bir sertifikayı (alınır **CERT_SYSTEM_STORE_LOCAL_MACHINE** Windows sertifika deposunda konumunda). Olmadığından genel bir yönetici bu yordamda, söz konusu var. hiçbir erişim adına genel yönetici gerekli belirteci.
    - 2. adımda oluşturulan ortak anahtarı.
    - A **sertifika imzalama isteği** (CSR veya sertifika isteği). Bu, sertifika yetkilisi olarak Azure AD ile yeni bir dijital kimliği sertifika uygulamaktır.
4. Azure AD varolan bir sertifikayı, sertifika yenileme isteğini doğrular. Daha sonra istek üzerinde kiracınızın kayıtlı bir kimlik doğrulama Aracısı geldiğini doğrular.
5. Var olan sertifikasının hala geçerli ise, Azure AD sonra yeni bir dijital kimliği sertifika imzalar ve yeni sertifika kimlik doğrulaması aracıya geri verir. 
6. Var olan sertifikasının süresi dolduysa, Azure AD kimlik doğrulama Aracısı kayıtlı kimlik doğrulaması aracıları, kiracının listeden siler. Ardından bir genel yönetici el ile yüklemeniz ve yeni bir kimlik doğrulama Aracısı kaydetmeniz gerekir.
    - Sertifika ile imzalanmış **Azure AD kök sertifika yetkilisi (CA)**.
    - Sertifikanın konu (**ayırt edici ad veya**) ayarlamak, **Kiracı kimliği** kiracınız benzersiz olarak tanımlayan bir GUID. Diğer bir deyişle, sertifikayı kiracınız için kapsamlıdır.
6. Azure AD kimlik doğrulama Aracısı "Yeni" ortak anahtarını ancak erişimi olan bir Azure SQL veritabanında depolar. Ve kimlik doğrulama aracı ile ilişkili "eski" ortak anahtar geçersiz kılar.
7. (5. adımda verilen) yeni sertifika sonra sunucuda depolanan **Windows sertifika deposunda** (özellikle de  **[CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER)**  Konum).
    - Güven yenileme yordamı etkileşimsiz (varlığını genel yönetici) yapıldığından, kimlik doğrulama Aracısı artık varolan bir sertifikayı güncelleştirmek için erişimi **CERT_SYSTEM_STORE_LOCAL_MACHINE** konumu. Unutmayın sertifikanın kendisi de **CERT_SYSTEM_STORE_LOCAL_MACHINE** konum Bu yordam sırasında kaldırılmaz.
8. Yeni sertifika bu noktasından kimlik doğrulaması için kullanılır. Sertifikada sonraki her sertifikanın yenilenmesini değiştirir **CERT_SYSTEM_STORE_LOCAL_MACHINE** konumu.

## <a name="auto-update-of-authentication-agents"></a>Kimlik doğrulama aracıları otomatik güncelleştirme

Yeni bir sürümü yayımlandığında güncelleştirici uygulama kimlik doğrulama Aracısı otomatik olarak güncelleştirir. Kiracınız için olan parola doğrulama isteklerini işlemez. 

Azure AD barındıran işaretli olarak yazılımın yeni sürümünü **Windows Installer paketi (MSI)**. MSI kullanılarak imzalanmıştır [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) ile **SHA256** Özet algoritması olarak. 

![Otomatik güncelleştirme](./media/active-directory-aadconnect-pass-through-authentication-security-deep-dive/pta5.png)

İşte nasıl otomatik olarak güncelleştirilen bir kimlik doğrulama Aracısı alır:

1. Güncelleştirici Azure düzenli aralıklarla ping kullanılabilir kimlik doğrulama Aracısı'nın yeni bir sürümü olup olmadığını denetlemek için AD (her bir saat). 
    - Bu denetim, (kayıt sırasında verilen aynı sertifikayı kullanarak) karşılıklı kimlik doğrulaması bir HTTPS kanalı üzerinden yapılır. Kimlik Doğrulama Aracısı ve güncelleştirici sunucuda depolanan sertifika paylaşır.
2. Yeni bir sürümü kullanılabilir durumda, Azure AD imzalı MSI geri güncelleştirici döndürür.
3. MSI Microsoft tarafından imzalanır güncelleştirici doğrular.
4. Güncelleştirici MSI çalışır. Aşağıdaki adımlar bu eylem yapar (güncelleştirici çalışır Not [yerel sistem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları):
    - Kimlik Doğrulama Aracı hizmetini durdurur.
    - Kimlik Doğrulama Aracısı'nın yeni sürümü yükler.
    - Kimlik Doğrulama Aracısı hizmetini yeniden başlatır.

>[!NOTE]
>Kiracı'kayıtlı birden çok kimlik doğrulama aracısı varsa, Azure AD sertifikalarını yenileme değil veya aynı anda güncelleştirin. Bunun yerine Azure AD, oturum açma isteklerinin yüksek kullanılabilirlik sağlamak için kademeli olarak böylece yapar.


## <a name="next-steps"></a>Sonraki adımlar
- [**Geçerli sınırlamalar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [**Hızlı Başlangıç** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - hale getirmek ve Azure AD doğrudan kimlik doğrulama çalıştıran.
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Nasıl çalışır** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -Azure AD doğrudan kimlik doğrulama nasıl çalıştığını temellerini öğrenin.
- [**Sık sorulan sorular** ](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.

