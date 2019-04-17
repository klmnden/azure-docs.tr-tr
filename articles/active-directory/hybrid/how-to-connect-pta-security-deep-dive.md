---
title: Azure Active Directory geçişli kimlik doğrulaması güvenlik derin Dalış | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulamasını şirket içi hesaplarınızı nasıl koruduğunu açıklanır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f5e2443a285e065426e3dba0312ef6420097ef1
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617231"
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory geçişli kimlik doğrulaması güvenliğe derinlemesine bakış

Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması nasıl çalıştığına ilişkin daha ayrıntılı bir açıklama sağlar. Bu özelliğin güvenlik konuları üzerinde odaklanır. Bu makalede, güvenlik ve BT yöneticileri, baş uyumluluk ve güvenlik sorumluları için getirilmiştir ve BT güvenlik ve uyumluluk en küçük ve orta sorumlu olan diğer BT uzmanlarının boyutlardaki kuruluşların veya büyük işletmelere.

Ele alınan konular şunlardır:
- Yükleme ve kimlik doğrulama aracılarının kaydetme konusunda ayrıntılı teknik bilgiler.
- Kullanıcı oturum açma sırasında parola şifrelemesini hakkında ayrıntılı teknik bilgiler.
- Kanal arasında güvenlik kimlik doğrulama aracılarının ve Azure AD şirket içi.
- Kimlik doğrulama aracılarının işletimsel olarak güvenliği konusunda ayrıntılı teknik bilgiler.
- Güvenlikle ilgili diğer konular.

## <a name="key-security-capabilities"></a>Temel güvenlik özellikleri

Bu özellik anahtar güvenlik yönleri şunlardır:
- Oturum açma isteklerinin kiracılar arasında yalıtım sağlayan bir güvenli çok kiracılı mimari üzerine inşa edilmiştir.
- Şirket içi parolaları, hiçbir zaman herhangi bir şekilde bulutta depolanır.
- Böylece, şirket içi kimlik doğrulama aracılarının dinlemek ve yanıtlamak için parola doğrulama isteği yalnızca ağınızdaki giden bağlantılar yapmak. Bir çevre ağında (DMZ) bu kimlik doğrulama aracılarının yükleneceği gereksinimi yoktur. Katman 0 sistemleri gibi kimlik doğrulama aracılarının çalıştıran tüm sunucuların en iyi uygulama olarak değerlendir (bkz [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).
- Yalnızca standart bağlantı noktalarına (80 ve 443), Azure AD kimlik doğrulama aracılarının giden iletişim için kullanılır. Güvenlik duvarınızı gelen bağlantı noktalarını açmanız gerekmez. 
  - 443 numaralı bağlantı noktası, kimliği doğrulanmış tüm giden iletişim için kullanılır.
  - 80 numaralı bağlantı noktası, yalnızca sertifika iptal listelerini (CRL'ler) yüklemek için bu özelliği tarafından kullanılan sertifika iptal edilmiş emin olmak için kullanılır.
  - Ağ gereksinimlerini tam listesi için bkz. [Azure Active Directory geçişli kimlik doğrulaması: Hızlı Başlangıç](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites).
- Oturum açma sırasında kullanıcılara sağlamak parolaları bulutta, şirket içi kimlik doğrulama aracılarının bunları Active Directory karşı doğrulama için kabul etmeden önce şifrelenir.
- Azure AD arasında HTTPS kanalı ve şirket içi kimlik doğrulama Aracısı karşılıklı kimlik doğrulaması kullanılarak korunmaktadır.
- İle sorunsuz bir şekilde çalışarak kullanıcı hesaplarınızı korur [Azure AD koşullu erişim ilkeleri](../active-directory-conditional-access-azure-portal.md), multi-Factor Authentication (MFA) dahil olmak üzere [eski bir kimlik doğrulama engelleme](../conditional-access/conditions.md) ve [ deneme yanılma parola saldırılarını filtreleme](../authentication/howto-password-smart-lockout.md).

## <a name="components-involved"></a>İlgili bileşenleri

Azure AD işletimsel, hizmet ve veri güvenliği hakkındaki genel ayrıntılar için bkz. [Güven Merkezi](https://azure.microsoft.com/support/trust-center/). Kullanıcı oturum açma için geçişli kimlik doğrulaması kullandığınızda aşağıdaki bileşenleri ilgilidir:
- **Azure AD STS**: Oturum açma isteklerini işleyen ve kullanıcıların tarayıcıları, istemcileri veya gerektiği gibi hizmetler için güvenlik belirteçleri durum bilgisi olmayan güvenlik belirteci hizmeti (STS).
- **Azure Service Bus**: Kurumsal Mesajlaşma ile bulut özellikli iletişim ve yardımcı olan geçişleri iletişimi şirket içi çözümlere bulut aracılığıyla bağlanmanızı sağlar.
- **Azure AD Connect kimlik doğrulaması Aracısı**: Dinler ve parola doğrulaması isteklerine yanıt veren bir şirket içi bileşeni.
- **Azure SQL veritabanı**: Kiracınızın kimlik doğrulama aracılarının, bunların meta verileri ve şifreleme anahtarları gibi ilgili bilgileri tutar.
- **Active Directory**: Böylece, şirket içi Active Directory kullanıcı hesaplarınızı ve parolalarını depolandığı.

## <a name="installation-and-registration-of-the-authentication-agents"></a>Yükleme ve kayıt kimlik doğrulama aracılarının

Kimlik doğrulama aracılarının yüklü ve Azure AD'ye kayıtlı olduğunda, ya da:
   - [Azure AD üzerinden geçişli kimlik doğrulamasını etkinleştir bağlanma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature)
   - [Oturum açma isteklerini yüksek kullanılabilirliğini sağlamak için daha fazla kimlik doğrulama aracılarının Ekle](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability) 
   
Bir kimlik doğrulama Aracısı çalışmasını üç ana aşama içerir:

1. Kimlik Doğrulama Aracısı yükleme
2. Kimlik Doğrulama Aracısı kaydı
3. Kimlik doğrulama aracısını başlatma

Aşağıdaki bölümlerde ayrıntılı Bu aşamalar açıklanmaktadır.

### <a name="authentication-agent-installation"></a>Kimlik Doğrulama Aracısı yükleme

Yalnızca genel Yöneticiler bir şirket içi sunucusunda kimlik doğrulaması Aracısı (Azure AD Connect veya tek başına kullanarak) yükleyebilirsiniz. Yükleme için iki yeni girişler ekler **Denetim Masası** > **programlar** > **programlar ve Özellikler** listesi:
- Kimlik Doğrulama Aracısı uygulamanın kendisi. Bu uygulama ile birlikte çalıştıran [NetworkService](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) ayrıcalıkları.
- Güncelleştirici uygulama kimlik doğrulama Aracısı otomatik olarak güncelleştirmek için kullanılır. Bu uygulama ile birlikte çalıştıran [LocalSystem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları.

### <a name="authentication-agent-registration"></a>Kimlik Doğrulama Aracısı kaydı

Kimlik Doğrulama Aracısı yükledikten sonra kendi Azure AD'ye kaydetmeniz gerekir. Azure AD, Azure AD ile güvenli iletişim için kullanabileceği benzersiz, dijital kimlik sertifika her kimlik doğrulama Aracısı atar.

Kayıt yordamı da kiracınız ile kimlik doğrulama Aracısı bağlar. Bu, Azure AD bu belirli kimlik doğrulama Aracısı kiracınız için parola doğrulama isteklerini işlemek için yetkili yalnızca biri olduğunu bilir sağlar. Bu yordam, kaydetmeniz her yeni kimlik doğrulama aracısı için tekrarlanır.

Kimlik doğrulama aracılarının kendilerini Azure AD ile kaydetmek için aşağıdaki adımları kullanın:

![Aracı kaydı](./media/how-to-connect-pta-security-deep-dive/pta1.png)

1. Azure AD genel yönetici Azure AD kimlik bilgileriyle oturum açın, ilk ister. Oturum açma sırasında kimlik doğrulama Aracısı adına genel yönetici kullanabileceğiniz bir erişim belirteci alır.
2. Kimlik doğrulaması Aracısı, ardından bir anahtar çifti oluşturur: bir ortak anahtar ve özel anahtar.
    - Anahtar çiftini standart RSA 2048 bit şifreleme oluşturulur.
    - Özel anahtar, kimlik doğrulama Aracısı bulunduğu şirket içi sunucuda kalır.
3. Kimlik Doğrulama Aracısı "kayıt" istek istekte bulunan aşağıdaki bileşenleri ile HTTPS üzerinden Azure AD'ye yapar:
    - 1. adımda alınan erişim belirteci.
    - 2. adımda oluşturulan genel anahtarı.
    - Sertifika imzalama isteği (CSR ya da sertifika isteği). Bu istek, sertifika yetkilisi (CA) olarak Azure AD ile bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD erişim belirteci kayıt isteği doğrular ve isteği genel Yöneticisi geldiğini doğrular.
5. Ardından Azure AD imzalar ve dijital kimlik sertifikası kimlik doğrulaması aracıya geri gönderir.
    - Azure AD'de kök CA sertifikasını imzalamak için kullanılır. 

      > [!NOTE]
      > Bu CA _değil_ Windows güvenilen kök sertifika yetkilileri deposunda.
    - CA'yı yalnızca doğrudan kimlik doğrulama özelliği tarafından kullanılır. CA, yalnızca kimlik doğrulama aracı kaydı sırasında CSR imzalamak için kullanılır.
    -  Bir Azure AD Hizmetleri hiçbiri bu CA'yı kullanın.
    - Sertifikanın konu (ayırt edici adı veya DN) Kiracı kimliğinizdir ayarlanır Bu DN kiracınızda benzersiz olarak tanımlayan bir GUID'dir. Bu DN yalnızca kiracınız ile kullanmak için sertifika kapsamları.
6. Azure AD kimlik doğrulaması Aracısı ortak anahtarı yalnızca Azure AD erişimi olan bir Azure SQL veritabanında depolar.
7. (5. adımında) sertifikanın şirket içi sunucunun Windows sertifika depolama alanında depolanır (özellikle de [CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE) konum). Hem kimlik doğrulama Aracısı hem de güncelleştirici uygulamalar tarafından kullanılır.

### <a name="authentication-agent-initialization"></a>Kimlik doğrulama aracısını başlatma

Kimlik Doğrulama Aracısı başladığında, bir sunucu veya kayıt sonra ilk kez başlatın, Azure AD'ye hizmetiyle güvenli iletişim kurabilmesi ve parola doğrulama isteklerini kabul eden başlatmak için bir yol gerekir.

![Aracısını başlatma](./media/how-to-connect-pta-security-deep-dive/pta2.png)

Kimlik doğrulama aracılarının nasıl başlatılır aşağıda verilmiştir:

1. Kimlik doğrulaması Aracısı, Azure AD'ye giden bir önyükleme isteği yapar. 
    - Bu istek bağlantı noktası 443 üzerinden yapılır ve karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı. İstek kimlik doğrulama aracısı kayıt sırasında verilen sertifikanın aynısını kullanır.
2. Azure AD kiracınız için benzersizdir ve Kiracı kimliğiniz tarafından tanımlanan bir Azure Service Bus kuyruğuna bir erişim anahtarı sağlayarak isteğine yanıt verir
3. Kimlik doğrulaması Aracısı, kalıcı bir giden HTTPS bağlantı (bağlantı noktası 443) üzerinden sıranın yapar. 
    - Kimlik Doğrulama Aracısı artık almak ve parola doğrulama isteklerini işlemek hazırdır.

Varsa birden çok kimlik doğrulama aracılarının kiracınızda kayıtlı ve ardından her biri aynı Service Bus kuyruğuna bağlanır başlatma yordamı sağlar. 

## <a name="process-sign-in-requests"></a>Oturum açma isteklerini işle 

Aşağıdaki diyagramda, geçişli kimlik doğrulaması kullanıcı oturum açma isteklerinin nasıl işlediği gösterilmektedir.

![Oturum açma işlemi](./media/how-to-connect-pta-security-deep-dive/pta3.png)

Geçişli kimlik doğrulaması, kullanıcı oturum açma isteği şu şekilde işler: 

1. Bir kullanıcı bir uygulama, örneğin, erişmeye [Outlook Web App](https://outlook.office365.com/owa).
2. Kullanıcı zaten oturum açmamış, uygulama tarayıcı Azure AD oturum açma sayfasına yönlendirir.
3. Azure AD STS hizmet yanıt ile geri **kullanıcı oturum açma** sayfası.
4. Kullanıcının girdiği kullanıcı adı içine **kullanıcı oturum açma** sayfası ve seçer **sonraki** düğmesi.
5. Kullanıcı parolasını içine girer **kullanıcı oturum açma** sayfası ve seçer **oturum** düğmesi.
6. Kullanıcı adı ve parola, bir HTTPS POST isteğinde Azure AD sts'ye gönderilir.
7. STS Azure AD kiracınız Azure SQL veritabanı'ndan kayıtlı tüm kimlik doğrulama aracıları için ortak anahtarları alır ve bunları kullanarak parolayı şifreler.
    - Kiracınızda kayıtlı "N" kimlik doğrulaması aracısı için "N" Şifreli parola değerlerini üretir.
8. Azure AD STS, kullanıcı adı ve kiracınız için belirli bir Service Bus kuyruğu üzerine şifreli parola değerleri oluşan parola doğrulama isteği yerleştirir.
9. Kullanılabilir kimlik doğrulama aracılarının biri, başlatılan kimlik doğrulama aracılarının kalıcı olarak Service Bus kuyruğuna bağlı olduğundan, parola doğrulama isteği alır.
10. Kimlik doğrulaması Aracısı, bir tanımlayıcıyı kullanarak ortak anahtarıyla özgüdür ve özel anahtarı kullanarak çözer şifreli parola değeri bulur.
11. Kimlik Doğrulama Aracısı kullanarak kullanıcı adı ve parola şirket içi Active Directory karşı doğrulama girişiminde [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx) ile **dwLogonType** parametre kümesi için **LOGON32_LOGON_NETWORK**. 
    - Bu API, Active Directory Federasyon Hizmetleri (AD FS) federe oturum açma senaryosunda kullanıcılar imzalamak için kullanılan aynı bir API'dir.
    - Bu API standart çözümleme işlemi Windows Server'ın etki alanı denetleyicisinin yerini belirlemek için kullanır.
12. Kimlik Doğrulama Aracısı başarılı, kullanıcı adı veya parola yanlış gibi Active Directory'den sonucu alır veya parolasının süresi doldu.

   > [!NOTE]
   > Kimlik doğrulaması Aracısı oturum açma işlemi sırasında başarısız olursa, tüm oturum açma isteği bırakılır. Var. hiçbir el dışı oturum açma isteklerinin bir kimlik doğrulaması Aracısı'ndan başka bir kimlik doğrulama Aracısı şirket içi Bu aracıları yalnızca bulutla ve birbirleriyle iletişim kurar.
13. Kimlik Doğrulama Aracısı bağlantı noktası 443 üzerinden giden bir karşılıklı kimlik doğrulaması yapılan HTTPS kanalı üzerinden Azure AD'ye STS'ye sonucu iletir. Karşılıklı kimlik doğrulaması için kimlik doğrulama aracısı kayıt sırasında daha önce verilen sertifikayı kullanır.
14. Azure AD STS, bu sonuç belirli oturum açma isteği kiracınıza karşılık gelen doğrular.
15. Azure AD STS ile oturum açma yordamı yapılandırıldığı şekilde devam eder. Örneğin, parola doğrulama başarılı olduysa, kullanıcının çok faktörlü kimlik doğrulaması için kimlik doğrulaması veya uygulamaya yeniden yönlendirildi.

## <a name="operational-security-of-the-authentication-agents"></a>Kimlik doğrulama aracılarının işletimsel güvenlik

Geçişli kimlik doğrulaması işletimsel olarak güvenli kalmasını sağlamak için düzenli aralıklarla Azure AD kimlik doğrulama aracılarının sertifikalarını yeniler. Azure AD, yenilemeler tetikler. Yenileme, kimlik doğrulama aracılarının kendileri tarafından yönetilmeyen.

![İşletimsel güvenlik](./media/how-to-connect-pta-security-deep-dive/pta4.png)

Azure AD ile kimlik doğrulaması Aracısı'nın güven yenilemek için:

1. Kimlik doğrulaması Aracısı, Azure düzenli olarak ping atar AD birkaç saatte sertifikasını yenilemek için saat olup olmadığını denetleyin. Sertifika ermesinden 30 gün önceden yenilenir.
    - Bu kontrolü karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerinden gerçekleştirilir ve kayıt sırasında verilen sertifikanın aynısını kullanır.
2. Hizmeti yenilemek için saat olduğunu gösteriyorsa, kimlik doğrulama aracısı yeni bir anahtar çifti oluşturur: bir ortak anahtar ve özel anahtar.
    - Bu anahtarlar standart RSA 2048 bit şifreleme oluşturulur.
    - Özel anahtarı şirket içi sunucu hiçbir zaman ayrılmaz.
3. Kimlik Doğrulama Aracısı ardından "sertifika yenileme" Azure AD'ye HTTPS üzerinden, istekte bulunan aşağıdaki bileşenleri ile istekte:
    - Windows sertifika deposu CERT_SYSTEM_STORE_LOCAL_MACHINE konumu alınır mevcut sertifika. Var. hiç genel yönetici bu yordamda ilgili genel yönetici adına gereken herhangi bir erişim belirteci olması
    - 2. adımda oluşturulan genel anahtarı.
    - Sertifika imzalama isteği (CSR ya da sertifika isteği). Bu istek, sertifika yetkilisi olarak Azure AD ile yeni bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD, mevcut sertifikayı sertifika yenileme isteğini doğrular. Daha sonra istek kiracınızda kayıtlı bir kimlik doğrulama Aracısı geldiğini doğrular.
5. Mevcut sertifika hala geçerli ise Azure AD sonra yeni bir dijital kimliği sertifikayı imzalar ve yeni sertifika kimlik doğrulaması aracıya geri verir. 
6. Var olan sertifikanın süresi doldu, Azure AD kimlik doğrulama Aracısı kiracınızın kayıtlı kimlik doğrulama aracılarının listeden siler. Daha sonra el ile yükleyin ve yeni bir kimlik doğrulama Aracısı'nı kaydetmek genel yönetici gerekir.
    - Azure AD kök CA sertifikasını imzalamak için kullanın.
    - Sertifikanın konu (ayırt edici adı veya DN), kiracınızda benzersiz olarak tanımlayan bir GUID, Kiracı Kimliğine ayarlayın. DN, kiracınıza yalnızca sertifika kapsamlar.
6. Azure AD kimlik doğrulaması aracısı yeni ortak anahtarı ancak erişimi olan bir Azure SQL veritabanında depolar. Kimlik Doğrulama Aracısı ile ilişkili eski ortak anahtar geçersiz kılar.
7. (5. adımında) yeni sertifikanın Windows sertifika deposu sunucusunda depolanır (özellikle de [CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER) konum).
    - Etkileşimli olmayan (varlığını genel yönetici) güven yenileme yordamı olur çünkü kimlik doğrulama Aracısı artık CERT_SYSTEM_STORE_LOCAL_MACHINE konumdaki mevcut bir sertifikayı güncelleştirmek için erişim vardır. 
    
   > [!NOTE]
   > Bu yordam CERT_SYSTEM_STORE_LOCAL_MACHINE konumundan sertifika kaldırmaz.
8. Yeni sertifika üzerinde bu noktadan itibaren kimlik doğrulaması için kullanılır. Sonraki her sertifikanın yenilenmesini CERT_SYSTEM_STORE_LOCAL_MACHINE konumda sertifika değiştirir.

## <a name="auto-update-of-the-authentication-agents"></a>Kimlik doğrulama aracılarının otomatik güncelleştirme

(Hata düzeltmeleri veya performans geliştirmeleri ile) yeni bir sürümü yayımlandığında güncelleştirici uygulaması kimlik doğrulaması Aracısı otomatik olarak güncelleştirir. Güncelleştirici uygulaması, kiracınız için herhangi bir parolayı doğrulama isteğinin işlemez.

Azure AD işaretli olarak yazılımın yeni sürümünü barındıran **Windows Installer paketi (MSI)**. MSI kullanarak oturum açmış [Microsoft Authenticode](https://msdn.microsoft.com/library/ms537359.aspx) Özet algoritması SHA256 olan. 

![Otomatik güncelleştirme](./media/how-to-connect-pta-security-deep-dive/pta5.png)

Bir kimlik doğrulama Aracısı otomatik güncelleştirme için:

1. Azure AD güncelleştirici uygulaması ping kullanılabilir kimlik doğrulama Aracısı'nın yeni bir sürümü olup olmadığını denetlemek için her saat. 
    - Bu onay, kayıt sırasında verilen aynı sertifikayı kullanarak karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerinden gerçekleştirilir. Sunucusunda depolanan sertifika kimlik doğrulaması aracısı ve güncelleştirici paylaşın.
2. Yeni bir sürüm varsa, Azure AD imzalı MSI güncelleştirici için geri döndürür.
3. Güncelleştirici MSI Microsoft tarafından imzalanmış olduğunu doğrular.
4. Güncelleştirici MSI çalıştırır. Bu eylem, aşağıdaki adımları içerir:

   > [!NOTE]
   > Güncelleştirici çalıştırır [yerel sistem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları.

    - Kimlik Doğrulama Aracısı hizmetini durdurur
    - Sunucuya kimlik doğrulaması Aracısı'nın yeni sürümü yükler
    - Kimlik Doğrulama Aracısı hizmetini yeniden başlatır

>[!NOTE]
>Birden çok kimlik doğrulama aracılarının kiracınızda kayıtlı varsa, Azure AD sertifikalarını yenileyemeyecektir veya aynı anda bunları güncelleştirin. Bunun yerine, Azure AD, bu nedenle oturum açma isteklerini yüksek kullanılabilirliğini sağlamak için bir kerede yapar.
>


## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Hangi senaryolar desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](how-to-connect-pta-quick-start.md): Azure AD geçişli kimlik doğrulaması ve çalışır duruma getirin.
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://aka.ms/adfstoptadpdownload) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Akıllı kilitleme özelliğini, kullanıcı hesapları korumak için kiracınızı yapılandırın.
- [Nasıl çalıştığını](how-to-connect-pta-how-it-works.md): Azure AD geçişli kimlik doğrulaması nasıl çalıştığına ilişkin temel bilgileri öğrenin.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Sık sorulan soruların yanıtlarını bulun.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): Geçişli kimlik doğrulaması özelliği olan yaygın sorunların nasıl çözümleneceğini öğrenin.
- [Azure AD sorunsuz çoklu oturum açma](how-to-connect-sso.md): Tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
