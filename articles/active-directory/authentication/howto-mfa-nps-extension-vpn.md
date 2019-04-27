---
title: VPN ağ ilkesi sunucusu uzantısı - Azure Active Directory kullanarak Azure MFA ile tümleştirin.
description: VPN altyapınız için Microsoft Azure ağ ilkesi sunucusu uzantısı kullanarak Azure MFA ile tümleştirin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2674e5ca12269d44e111f140fce77bd8bc0c9ae7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60361956"
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Azure için ağ ilkesi sunucusu uzantısı kullanarak VPN altyapınız ile Azure mfa'yı tümleştirme

## <a name="overview"></a>Genel Bakış

Uzak kimlik denetimi içeri arama kullanıcı hizmeti (RADIUS) istemci kimlik doğrulaması kullanarak buluttaki anahtarlarınızı kuruluşların Azure için ağ ilkesi sunucusu (NPS) uzantı sağlayan [Azure multi-Factor Authentication (MFA)](howto-mfaserver-nps-rdg.md), iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısı kullanarak NPS altyapı MFA ile tümleştirmeye yönelik yönergeler sağlar. Bu işlem, ağınıza bir VPN kullanarak bağlanma girişiminde kullanıcılar için güvenli iki aşamalı doğrulamayı etkinleştirir.

Ağ İlkesi ve Erişim Hizmetleri kuruluşlar yeteneği sağlar:

* Denetim belirtmek için ağ isteklerinin ve Yönetim için merkezi bir konum atayın:

  * Kimin bağlanabilir miyim

  * Gün bağlantıların ne zaman izin verilir

  * Bağlantı süresi

  * İstemciler bağlanmak için kullanması gereken güvenlik düzeyi

    Merkezi bir konumda olduklarını sonra her VPN ya da Uzak Masaüstü Ağ Geçidi sunucusuna ilkeleri belirtme yerine bunu yapın. RADIUS protokolü, merkezi kimlik doğrulaması, yetkilendirme ve hesap işlemleri (AAA) sağlamak için kullanılır.

* Kurmak ve cihazları Kısıtlanmamış veya kısıtlanmış ağ kaynaklarına erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri uygular.

* Kimlik doğrulama ve yetkilendirme için 802.1 erişim uygulanmasına yönelik bir yöntem sağlayan x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.
  Daha fazla bilgi için [ağ ilkesi sunucusu](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top).

Güvenliği artırmak ve yüksek düzeyde uyumluluk sağlamak için kuruluşlar NPS kullanıcıların VPN sunucusunda sanal bağlantı noktasına bağlanmak için iki aşamalı doğrulama kullandığınızdan emin olmak için Azure multi-Factor Authentication ile tümleştirebilirsiniz. Kullanıcıların erişim verilmesi için kullanıcı adı ve parola birleşimi ve bunların denetledikleri diğer bilgileri sağlamaları gerekir. Bu bilgileri güvenilir ve kolayca çoğaltılması gerekir. Cep telefonu numarası, bir telefona sayı veya bir uygulamayı mobil cihazda içerebilir.

Azure için NPS uzantısı kullanılabilirliğini önce NPS uygulamak için iki aşamalı doğrulamayı isteyen müşteriler tümleşik ve bir şirket içi ortamda ayrı bir MFA sunucunuzu yapılandırmak ve korumak mfa'yı ortamları vardı. Bu kimlik doğrulama türü, Azure multi-Factor Authentication sunucusu RADIUS kullanan Uzak Masaüstü Ağ geçidi ile sunulur.

Azure için NPS uzantısı ile kuruluşlar şirket içi tabanlı MFA çözümünü veya bir bulut tabanlı MFA çözümünü dağıtarak RADIUS istemci kimlik doğrulaması güvenliğini sağlayabilirsiniz.

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kullanıcılar, bir VPN sunucusu sanal bağlantı noktasına bağlandığında, öncelikle çeşitli protokoller kullanarak kimlik doğrulaması gerekir. Protokolleri, kullanıcı adı ve parola ve sertifika tabanlı kimlik doğrulama yöntemlerinin bir birleşimini kullanılmasına izin verin.

Kimlik doğrulaması ve kimlik doğrulama ek olarak, kullanıcıların çevirmeli uygun izinleri olmalıdır. Basit uygulamalarında, erişime izin veren çevirmeli izinleri doğrudan Active Directory kullanıcı nesneleri üzerinde ayarlanır.

![Active Directory Kullanıcıları ve bilgisayarları, kullanıcı özelliklerinde Arama sekmesi](./media/howto-mfa-nps-extension-vpn/image1.png)

Basit uygulamalarında her VPN sunucusuna erişim izni verilir veya her yerel VPN sunucuda tanımlanmış ilkeler doğrultusunda erişimi engeller.

Uygulamalarında, daha büyük ve daha ölçeklenebilir, RADIUS sunucuları üzerinde VPN erişim verme veya reddetmeye ilkeleri toplanmıştır. Bu durumlarda, VPN sunucusunun bağlantı isteklerini ve bir RADIUS sunucusuna hesabı iletilerini ileten bir erişim sunucusu (RADIUS istemcisi) gibi davranır. VPN sunucusundaki sanal bağlantı noktasına bağlanmak için kullanıcılar kimlik doğrulamasından geçmesi gerekir ve merkezi olarak üzerinde RADIUS sunucuları tanımlanmışsa koşulları.

Azure için NPS uzantısı NPS ile tümleştirildiğinde, başarılı kimlik doğrulaması akışı, aşağıdaki şekilde sonuçlanır:

1. VPN sunucusu kullanıcı adı ve Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için parola içeren bir VPN kullanıcıdan kimlik doğrulama isteği alır.
2. Bir RADIUS istemcisi işlevi gören, VPN sunucusu isteği için bir RADIUS dönüştürür *erişim isteği* iletisi ve RADIUS sunucusuna (şifrelenmiş parola ile), NPS uzantısı yüklendiği gönderir.
3. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır. RADIUS sunucusu kullanıcı adı veya parola yanlışsa, gönderen bir *erişim reddi* ileti.
4. Ağ ilkeleri ve NPS bağlantı isteği belirtilen tüm koşullar karşılanıyorsa (örneğin, zaman gün veya grubun üyelik kısıtlamaları), NPS uzantısı ile Azure multi-Factor Authentication ikincil kimlik doğrulama isteği tetikler.
5. Azure multi-Factor Authentication, Azure Active Directory ile iletişim kurar, kullanıcının ayrıntılarını alır ve ikincil kimlik doğrulaması (hücre telefon araması, SMS mesajı veya mobil uygulama) kullanıcı tarafından yapılandırılan yöntemini kullanarak gerçekleştirir.
6. MFA testini başarılı olduğunda, Azure multi-Factor Authentication için NPS uzantısı sonucu iletişim kurar.
7. Bağlantı denemesi kimliği doğrulanmış ve yetkili sonra NPS uzantısı yüklü olduğu bir RADIUS gönderir *erişim kabul* (RADIUS istemcisi) VPN sunucusuna ileti.
8. Kullanıcı VPN sunucusundaki sanal bağlantı noktası erişimi verilir ve şifrelenmiş bir VPN tüneli oluşturur.

## <a name="prerequisites"></a>Önkoşullar

Bu bölümde MFA Uzak Masaüstü Ağ geçidi ile tümleştirebilirsiniz önce tamamlanması gereken önkoşulları açıklanmaktadır. Başlamadan önce aşağıdaki önkoşulların yerinde olmalıdır:

* VPN altyapısı
* Ağ İlkesi ve Erişim Hizmetleri rolü
* Azure multi-Factor Authentication lisansı
* Windows Server yazılımı
* Kitaplıklar
* Azure Active Directory (Azure AD) ile şirket içi Active Directory
* Azure Active Directory GUID kimliği

### <a name="vpn-infrastructure"></a>VPN altyapısı

Bu makalede, Microsoft Windows Server 2016'yı kullanan bir çalışan VPN altyapıya sahip olduğunuzu ve VPN sunucunuzun şu anda ileri bağlantı isteklerini bir RADIUS sunucusunda yapılandırılmadığından varsayılır. Makalede, VPN altyapısı, merkezi bir RADIUS sunucusu kullanacak şekilde yapılandırın.

Yerinde bir çalışan VPN altyapısı yoksa, hızlı bir şekilde Microsoft ve üçüncü taraf sitelere öğreticilerde bulabileceğiniz çeşitli VPN kurulum yönergeleri izleyerek bir tane oluşturabilirsiniz.

### <a name="the-network-policy-and-access-services-role"></a>Ağ İlkesi ve Erişim Hizmetleri rolü

Ağ İlkesi ve Erişim Hizmetleri, RADIUS sunucusu ve İstemcisi işlevselliği sağlar. Bu makalede, Ağ İlkesi ve Erişim Hizmetleri rolüne bir üye sunucu veya etki alanı denetleyicisinde ortamınızda yüklediğinizi varsayar. Bu kılavuzda, bir VPN yapılandırma için RADIUS yapılandırın. Bir sunucuya ağ ilkesi ve Erişim Hizmetleri rolünü yükleyin *dışında* VPN sunucunuzun.

Ağ İlkesi ve Erişim Hizmetleri rolü yükleme hakkında bilgi için Windows Server 2012 hizmet veya daha sonra bkz [NAP sistem durumu ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). NAP, Windows Server 2016'da kullanım dışı bırakılmıştır. NPS, bir etki alanı denetleyicisinde NPS'yi yüklemek için öneri de dahil olmak üzere en iyi bir açıklaması için bkz. [en iyi uygulamalar için NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA lisans

Azure multi-Factor Authentication için lisans gereklidir ve bir Azure AD Premium, Enterprise Mobility + Security veya bir multi-Factor Authentication tek başına lisansı kullanılabilir. Tüketim tabanlı lisans kullanıcı başına veya kimlik doğrulaması lisans başına gibi Azure mfa NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için [Azure multi-Factor Authentication'ı alma](concept-mfa-licensing.md). Sınama amacıyla bir deneme aboneliğini kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

NPS uzantısı, Windows Server 2008 R2 SP1 gerektirir veya daha sonra Ağ İlkesi ve Erişim Hizmetleri rolü yüklü. Bu kılavuzda yer alan tüm adımları, Windows Server 2016 ile gerçekleştirildi.

### <a name="libraries"></a>Kitaplıklar

Aşağıdaki kitaplıklar NPS uzantısı ile otomatik olarak yüklenir:

-   [Visual Studio 2013 (X64) için Visual C++ yeniden dağıtılabilir paketleri](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory için Windows PowerShell modülü sürüm 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory PowerShell modülü zaten mevcut değilse Kurulum işleminin bir parçası çalışan bir yapılandırma betiği ile yüklenir. Zaten yüklü değilse, önceden modülü yüklemek için gerek yoktur.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Şirket içi Active Directory ile eşitlenmiş Azure Active Directory

NPS uzantısını kullanmak için şirket içi kullanıcıları Azure Active Directory ile eşitlenen ve MFA için etkinleştirilmiş olmalıdır. Bu kılavuz, şirket içi kullanıcıları Azure AD Connect ile Azure Active Directory ile eşitlenen varsayar. Kullanıcı MFA için etkinleştirmeye ilişkin yönergeler aşağıda verilmiştir.

Azure AD Connect hakkında daha fazla bilgi için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği

NPS uzantısını yüklemek için Azure Active Directory GUID bilmeniz gerekir. Sonraki bölümde Azure Active Directory GUID'i bulmak için yönergeler sağlanır.

## <a name="configure-radius-for-vpn-connections"></a>VPN bağlantıları için RADIUS yapılandırın

Bir üye sunucuda NPS rolü yüklü değilse, kimlik doğrulama ve bu istekleri VPN bağlantıları VPN istemci yetkilendirme için yapılandırmanız gerekir. 

Bu bölümde, Ağ İlkesi ve Erişim Hizmetleri rolü yüklü ancak kullanım için altyapınızda yapılandırmadınız varsayılır.

> [!NOTE]
> Merkezi bir RADIUS sunucusuna kimlik doğrulaması için kullandığı çalışma VPN sunucusu zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir

Bu senaryoda düzgün çalışması için Active Directory'de NPS sunucusunun kaydedilmesi gerekir.

1. Sunucu Yöneticisi'ni açın.

2. Sunucu Yöneticisi'nde **Araçları**ve ardından **ağ ilkesi sunucusu**.

3. Sağ tıklayın ağ ilkesi sunucusu konsolunda **NPS (yerel)** ve ardından **Active Directory'de kayıt sunucusu**. Seçin **Tamam** iki kez.

    ![Active Directory menü seçeneğini sunucusunu kaydetme](./media/howto-mfa-nps-extension-vpn/image2.png)

4. Konsolunu sonraki yordam için açık bırakın.

### <a name="use-wizard-to-configure-the-radius-server"></a>RADIUS sunucusu Yapılandırma Sihirbazı'nı kullanın

Standart (Sihirbaz tabanlı) kullanabilirsiniz veya Gelişmiş RADIUS sunucusunu yapılandırmak için yapılandırma seçeneği. Bu bölümde, sihirbaz tabanlı standart yapılandırma seçeneği kullandığınız varsayılır.

1. Ağ İlkesi Sunucusu konsolunda seçin **NPS (yerel)**.

2. Altında **standart yapılandırma**seçin **çevirmeli veya VPN bağlantıları için RADIUS sunucusu**ve ardından **yapılandırma VPN veya çevirmeli**.

    ![Çevirmeli veya VPN bağlantıları için RADIUS sunucusunu yapılandırın](./media/howto-mfa-nps-extension-vpn/image3.png)

3. İçinde **çevirmeli veya sanal özel ağ bağlantı türünü seçin** penceresinde **sanal özel ağ bağlantıları**ve ardından **sonraki**.

    ![Sanal özel ağ bağlantılarını yapılandırma](./media/howto-mfa-nps-extension-vpn/image4.png)

4. İçinde **belirtin çevirmeli veya VPN sunucusu** penceresinde **Ekle**.

5. İçinde **yeni RADIUS istemcisi** penceresinde kolay bir ad girin, çözümlenebilir adını veya VPN sunucusunun IP adresini girin ve ardından paylaşılan bir gizli parolasını girin. Paylaşılan gizli parolasını uzun ve karmaşık olun. Sonraki bölümde gerekeceği için bunu kaydedin.

    ![Yeni RADIUS istemcisi penceresi oluştur](./media/howto-mfa-nps-extension-vpn/image5.png)

6. Seçin **Tamam**ve ardından **sonraki**.

7. İçinde **kimlik doğrulama yöntemleri yapılandırma** penceresi, varsayılan seçimi kabul et (**Microsoft Şifreli kimlik doğrulaması sürüm 2 [MS-CHAPv2])** veya başka bir seçenek belirleyin ve işaretleyin **İleri** .

    > [!NOTE]
    > Genişletilebilir Kimlik Doğrulama Protokolü (EAP) yapılandırırsanız, Microsoft Karşılıklı Kimlik Doğrulama Protokolü (CHAPv2) veya Korumalı Genişletilebilir Kimlik Doğrulama Protokolü (PEAP) kullanmanız gerekir. Diğer bir EAP desteklenir.

8. İçinde **kullanıcı gruplarını belirtmek** penceresinde **Ekle**ve ardından uygun bir grup seçin. Seçim, herhangi bir grubu varsa, tüm kullanıcılara erişim vermek için boş bırakın.

    ![Kullanıcı grupları veya erişimini engellediği penceresini belirtin](./media/howto-mfa-nps-extension-vpn/image7.png)

9. **İleri**’yi seçin.

10. İçinde **IP filtreleri belirtin** penceresinde **sonraki**.

11. İçinde **şifreleme ayarlarını belirt** penceresinde varsayılan ayarları kabul edin ve ardından **sonraki**.

    ![Pencerenin şifreleme ayarlarını belirtin](./media/howto-mfa-nps-extension-vpn/image8.png)

12. İçinde **bir bölge adı belirtin** penceresi, bölge adını boş bırakın, varsayılan ayarını kabul edin ve ardından **sonraki**.

    ![Bir bölge adı penceresi belirtin](./media/howto-mfa-nps-extension-vpn/image9.png)

13. İçinde **Tamamlanıyor yeni çevirmeli ya da sanal özel ağ bağlantıları ve RADIUS istemcileri** penceresinde **son**.

    ![Yapılandırma tamamlandı penceresi](./media/howto-mfa-nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>RADIUS yapılandırmasını doğrula

Bu bölümde, Sihirbazı'nı kullanarak oluşturduğunuz yapılandırma açıklanmaktadır.

1. NPS (yerel) konsolunda Ağ İlkesi sunucusundaki genişletin **RADIUS istemcileri**ve ardından **RADIUS istemcileri**.

2. Ayrıntılar bölmesinde, oluşturduğunuz ve ardından RADIUS istemcisi sağ **özellikleri**. RADIUS istemciniz (VPN sunucusu) özelliklerini olanlar burada gösterildiği gibi olmalıdır:

    ![VPN özellikleri ve yapılandırmasını doğrulayın](./media/howto-mfa-nps-extension-vpn/image11.png)

3. Seçin **iptal**.

4. NPS (yerel) konsolunda Ağ İlkesi sunucusundaki genişletin **ilkeleri**ve ardından **bağlantı isteği ilkeleri**. VPN bağlantıları ilkeyi, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

    ![VPN bağlantısı ilke gösteren bağlantı isteği ilkesi](./media/howto-mfa-nps-extension-vpn/image12.png)

5. Altında **ilkeleri**seçin **ağ ilkeleri**. Aşağıdaki görüntüde gösterildiği ilke benzer bir sanal özel ağ (VPN) bağlantılarına ilke görmeniz gerekir:

    ![Sanal özel ağ bağlantıları ilkesini gösteren ağ ilkeleri](./media/howto-mfa-nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>VPN RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın

Bu bölümde, VPN sunucunuzu, RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın. Yönergeler, VPN sunucusunun çalışan bir yapılandırma sahip ancak RADIUS kimlik doğrulaması kullanacak şekilde yapılandırmadıysanız varsayar. VPN sunucusu yapılandırdıktan sonra yapılandırmanızı beklendiği gibi çalıştığını onaylayın.

> [!NOTE]
> RADIUS kimlik doğrulaması kullanan çalışan bir VPN sunucusu yapılandırması zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="configure-authentication-provider"></a>Kimlik doğrulama sağlayıcısı yapılandırma

1. VPN sunucusunda, Sunucu Yöneticisi'ni açın.

2. Sunucu Yöneticisi'nde **Araçları**ve ardından **Yönlendirme ve Uzaktan erişim**.

3. İçinde **Yönlendirme ve Uzaktan erişim** penceresinde sağ  **\<sunucu adı > (yerel)** ve ardından **özellikleri**.

4. İçinde  **\<sunucu adı > (yerel) özellikleri** penceresinde **güvenlik** sekmesi.

5. Üzerinde **güvenlik** sekmesindeki **kimlik doğrulama sağlayıcısı**seçin **RADIUS kimlik doğrulaması**ve ardından **yapılandırma**.

    ![RADIUS kimlik doğrulaması sağlayıcısı yapılandırmanız](./media/howto-mfa-nps-extension-vpn/image15.png)

6. İçinde **RADIUS kimlik doğrulaması** penceresinde **Ekle**.

7. İçinde **RADIUS sunucusu Ekle** penceresinde aşağıdakileri yapın:

    a. İçinde **sunucu adı** kutusunda, ad veya önceki bölümde yapılandırdığınız RADIUS sunucusunun IP adresini girin.

    b. İçin **paylaşılan gizlilik**seçin **değişiklik**ve ardından oluşturduğunuz ve daha önce kaydedilmiş paylaşılan gizli parolasını girin.

    c. İçinde **zaman aşımı (saniye)** kutusunda, bir değer seçerek **30** aracılığıyla **60**.  
    Zaman aşımı değeri, ikinci faktör kimlik doğrulamasını tamamlamak için yeterli zamana izin vermek gereklidir.

    ![RADIUS sunucusu penceresi zaman aşımını yapılandırma Ekle](./media/howto-mfa-nps-extension-vpn/image16.png)

8. **Tamam**’ı seçin.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme

Bu bölümde, VPN istemci kimlik doğrulaması ve VPN sanal bağlantı noktasına bağlanmaya çalıştığınızda RADIUS sunucusu tarafından yetkili olduğunu onaylayın. Yönergeler, VPN istemci olarak Windows 10 kullandığınızı varsayar.

> [!NOTE]
> VPN istemci ayarlarını kaydettikten ve VPN sunucusuna zaten yapılandırılmışsa, bir VPN bağlantı nesnesi kaydetme ve yapılandırma ile ilgili adımları atlayabilirsiniz.
>

1. VPN istemci bilgisayarınızda seçin **Başlat** düğmesini ve ardından **ayarları** düğmesi.

2. İçinde **Windows ayarları** penceresinde **ağ ve Internet**.

3. Seçin **VPN**.

4. Seçin **bir VPN bağlantısı Ekle**.

5. İçinde **bir VPN bağlantısı Ekle** penceresi içinde **VPN sağlayıcı** kutusunda **Windows (yerleşik)**, uygun şekilde kalan alanları doldurun ve ardından seçin**Kaydet**.

    !["Bir VPN bağlantısı Ekle" penceresi](./media/howto-mfa-nps-extension-vpn/image17.png)

6. Git **Denetim Masası**ve ardından **ağ ve Paylaşım Merkezi**.

7. Seçin **Bağdaştırıcı ayarlarını değiştir**.

    ![Ağ ve Paylaşım Merkezi - Bağdaştırıcı ayarlarını değiştir](./media/howto-mfa-nps-extension-vpn/image18.png)

8. VPN ağ bağlantısına sağ tıklayın ve ardından **özellikleri**.

9. VPN Özellikler penceresinde seçin **güvenlik** sekmesi.

10. Üzerinde **güvenlik** sekmesinde, yalnızca **Microsoft CHAP Version 2 (MS-CHAP v2)** seçili ve ardından **Tamam**.

    !["Bu protokollere izin ver" seçeneği](./media/howto-mfa-nps-extension-vpn/image20.png)

11. VPN bağlantısına sağ tıklayın ve ardından **Connect**.

12. İçinde **ayarları** penceresinde **Connect**.  
    Burada gösterildiği gibi güvenlik günlüğünde olay kimliği 6272'in, RADIUS sunucu üzerinde başarılı bir bağlantı görünür:

    ![Başarılı bağlantının görüntülendiği olay Özellikler penceresi](./media/howto-mfa-nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>RADIUS sorunlarını giderme

VPN sunucusunu kimlik doğrulama ve yetkilendirme için merkezi bir RADIUS sunucusu kullanacak şekilde yapılandırılmış önce VPN yapılandırmanızı çalışır durumda olduğunu varsayalım. Yapılandırma çalışıyorsanız sorunu RADIUS sunucusunun yanlış yapılandırma veya bir geçersiz kullanıcı adı veya parola kullanımını kaynaklanır olasıdır. Örneğin, kullanıcı adı, alternatif UPN soneki kullanırsanız, oturum açma denemesi başarısız olabilir. En iyi sonuçlar için hesap adının aynısını kullanın.

Bu sorunları gidermek için başlatmak için ideal bir RADIUS sunucusu güvenlik olay günlüklerini incelemek için yerdir. Olayları zaman arama kaydetmek için rol tabanlı ağ ilkesi ve erişim sunucusu özel görünümü Olay Görüntüleyicisi'nde, burada gösterildiği gibi kullanabilirsiniz. "Olay kimliği 6273" nerede NPS bir kullanıcıya erişim engellendi. olayları gösterir.

![Olay Görüntüleyicisi'ni gösteren NPAS olayları](./media/howto-mfa-nps-extension-vpn/image22.png)

## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma

Kullanıcılar için multi-Factor Authentication yapılandırma Yardım makalelere göz atın [bir kullanıcı veya grup için iki aşamalı doğrulama gerektirme](howto-mfa-userstates.md) ve [hesabım için iki aşamalı doğrulama ayarlayın](../user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma

Bu bölümde, VPN sunucusu ile istemci kimlik doğrulaması için mfa'yı kullanmaları VPN yapılandırma için yönergeler sağlar.

Yükleyip NPS uzantısı yapılandırdıktan sonra bu sunucu tarafından işlenen tüm istemci RADIUS tabanlı kimlik doğrulaması mfa'yı kullanmaları gerekir. Tüm VPN kullanıcılarınız Azure çok faktörlü kimlik doğrulamasına kayıtlı olmayan, aşağıdakilerden birini yapabilirsiniz:

* Mfa'yı kullanmak için yapılandırılmamış olan kullanıcıların kimliğini doğrulamak için başka bir RADIUS sunucusunu ayarlayın.

* Azure multi-Factor Authentication kaydettiyseniz bir ikinci kimlik doğrulama faktörü sağlamalarının zorluğu olan kullanıcılar izin veren bir kayıt defteri girişi oluşturun.

Adlı yeni bir dize değeri oluşturun _HKLM\SOFTWARE\Microsoft\AzureMfa içinde REQUIRE_USER_MATCH_ve değerine *True* veya *False*.

!["Kullanıcı eşleşmesi iste" ayarı](./media/howto-mfa-nps-extension-vpn/image34.png)

Değer ayarlanmışsa *True* veya tüm kimlik doğrulama isteklerini bir MFA testini tabi boştur. Değer ayarlanmışsa *False*, MFA zorlukları, yalnızca Azure multi-Factor Authentication kayıtlı kullanıcıları için yayımlanır. Kullanım *False* bir ekleme süresi boyunca yalnızca test veya üretim ortamları ayarlama.

### <a name="obtain-the-azure-active-directory-guid-id"></a>Azure Active Directory GUID'i Kimliğini alın

NPS uzantısı yapılandırma işleminin bir parçası olarak, yönetici kimlik bilgileri ve Azure AD kiracınızın Kimliğini sağlamanız gerekir. Aşağıdakileri yaparak Kimliğini alın:

1. Oturum [Azure portalında](https://portal.azure.com) Azure kiracısının genel Yöneticisi olarak.

2. Sol bölmede seçin **Azure Active Directory** düğmesi.

3. Seçin **özellikleri**.

4. Azure AD Kimliğinizi kopyalamak üzere **kopyalama** düğmesi.

    ![Azure portalında Azure AD dizin kimliği](./media/howto-mfa-nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

NPS uzantısı, Ağ İlkesi ve Erişim Hizmetleri rolü yüklü olan bir sunucuyu ve Bu işlevlerden tasarımınızı RADIUS sunucusu olarak yüklenmelidir. Yapmak *değil* NPS uzantısı Uzak Masaüstü sunucunuza yükleyin.

1. NPS uzantısını [Microsoft Download Center](https://aka.ms/npsmfa).

2. Kurulum yürütülebilir dosyasını kopyalayın (*NpsExtnForAzureMfaInstaller.exe*) NPS sunucusuna.

3. NPS sunucusunda çift **NpsExtnForAzureMfaInstaller.exe** siz istenirse seçin **çalıştırma**.

4. İçinde **NPS uzantısı için Azure MFA Kurulumu** penceresinde, gözden geçirme, yazılım lisans koşulları seçin **lisans hüküm ve koşulları kabul ediyorum** onay kutusunu işaretleyin ve ardından **Yükleme**.

    !["NPS uzantısı için Azure mfa'yı Kurulum" penceresi](./media/howto-mfa-nps-extension-vpn/image36.png)

5. İçinde **NPS uzantısı için Azure MFA Kurulumu** penceresinde **Kapat**.  

    !["Kurulum başarılı" onay penceresi](./media/howto-mfa-nps-extension-vpn/image37.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>Bir PowerShell komut dosyası kullanarak NPS uzantısı ile kullanmak için sertifikaları yapılandırma

Güvenli iletişimler ve güvencesi sağlamak için NPS uzantısı tarafından kullanım için sertifikaları yapılandırın. NPS bileşenleri otomatik olarak imzalanan bir sertifika kullanmak için NPS ile yapılandıran bir Windows PowerShell Betiği içerir.

Komut aşağıdaki eylemleri gerçekleştirir:

* Otomatik olarak imzalanan bir sertifika oluşturur.
* Hizmet sorumlusu Azure AD üzerinde sertifikanın ortak anahtarını ilişkilendirir.
* Sertifika, yerel makine deposunda saklar.
* Sertifikanın özel anahtarı ağ kullanıcı erişim verir.
* NPS hizmetini yeniden başlatır.

Kendi sertifikalarını kullanmak istiyorsanız, sertifikanın ortak anahtarı, Azure AD'de hizmet sorumlusu ile ilişkilendirmek ve benzeri.

Betiği kullanmak için Azure Active Directory yönetici kimlik bilgilerinizi ve daha önce kopyaladığınız Azure Active Directory Kiracı Kimliğini uzantısı sağlar. Betik, NPS uzantısını yüklediğiniz her NPS sunucusunda çalıştırın.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. PowerShell komut isteminde girin **cd "c:\Program Files\Microsoft\AzureMfa\Config"** ve ardından Enter tuşuna basın.

3. Sonraki komut isteminde girin **.\AzureMfaNpsExtnConfigSetup.ps1**ve ardından Enter tuşuna basın. Azure AD PowerShell modülünün yüklü olup olmadığını görmek için komut dosyasını denetler. Yüklenmezse, komut sizin için modülünü yükler.

    ![AzureMfsNpsExtnConfigSetup.ps1 yapılandırma betiğini çalıştırma](./media/howto-mfa-nps-extension-vpn/image38.png)

    PowerShell modülünün yükleme betiği doğruladıktan sonra Azure Active Directory PowerShell modülü oturum açma penceresinde görüntüler.

4. Azure AD yönetici kimlik bilgilerini ve parolayı girin ve ardından **oturum**.

    ![Azure AD PowerShell için kimlik doğrulaması](./media/howto-mfa-nps-extension-vpn/image39.png)

5. Komut isteminde, daha önce kopyaladığınız Kiracı Kimliğini yapıştırın ve ardından Enter'ı seçin.

    ![Önce kopyaladığınız Azure AD dizin kimliği girin](./media/howto-mfa-nps-extension-vpn/image40.png)

    Betik, otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri gerçekleştirir. Çıktı aşağıdaki resimdeki gibi.

    ![PowerShell penceresini gösteren otomatik olarak imzalanan sertifika](./media/howto-mfa-nps-extension-vpn/image41.png)

6. Sunucuyu yeniden başlatın.

### <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmayı doğrulamak için VPN sunucusu ile yeni bir VPN bağlantısı oluşturmanız gerekir. Birincil kimlik doğrulaması için başarıyla kimlik bilgilerinizi girdikten sonra bağlantı yeniden kurulmadan önce aşağıda gösterildiği gibi başarılı olması ikincil kimlik doğrulaması için VPN bağlantısı bekler.

![Windows ayarları VPN penceresi](./media/howto-mfa-nps-extension-vpn/image42.png)

Başarılı bir şekilde Azure mfa'yı daha önce yapılandırdığınız ikincil doğrulama yöntemi ile kimlik doğrulaması, kaynağa bağlıdır. Ancak, ikincil kimlik doğrulaması başarısız olursa, kaynağa erişimi reddedilir.

Aşağıdaki örnekte, Microsoft Authenticator uygulamasını Windows Phone üzerinde ikincil kimlik doğrulaması sağlar:

![Windows Phone örnek MFA istemi](./media/howto-mfa-nps-extension-vpn/image43.png)

Başarılı bir şekilde ikincil yöntemi kullanarak kimlik doğrulaması sonra VPN sunucusundaki sanal bağlantı noktasına erişim verilir. Güvenilen bir cihazda mobil uygulama kullanarak bir ikincil kimlik doğrulama yöntemi için gerekli için oturum açma işlemi yalnızca bir kullanıcı adı ve parola birleşimini kullanarak, daha daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi günlüklerini görüntüle

Görüntülemek için aşağıdaki PowerShell komutunu girerek NPS sunucusunda Windows Güvenlik günlüğü Windows Olay Görüntüleyicisi günlüklerinde başarılı oturum açma olaylarını sorgu:

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![PowerShell Güvenlik Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-vpn/image44.png)

Burada gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünümü da görüntüleyebilirsiniz:

![Örnek ağ ilkesi sunucusu günlüğü](./media/howto-mfa-nps-extension-vpn/image45.png)

Azure multi-Factor Authentication için NPS uzantısı yüklendiği sunucuda uzantısına belirli Olay Görüntüleyicisi'ni uygulama günlükleri bulabilirsiniz *uygulama ve Hizmetleri Logs\Microsoft\AzureMfa*.

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![Örnek Olay Görüntüleyicisi'ni AuthZ günlükleri bölmesi](./media/howto-mfa-nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, kullanıcının mfa'yı kullanmak için yapılandırıldığını doğrulayarak sorun giderme başlar. Bağlanma kullanıcının [Azure portalında](https://portal.azure.com). Kullanıcı ikincil kimlik doğrulaması için istenir ve başarıyla için kimlik doğrulaması, hatalı bir yapılandırma sorunu olarak MFA'ın ortadan kaldırabilir.

MFA için kullanıcı olarak çalışıyorsa, ilgili Olay Görüntüleyicisi günlüklerini gözden geçirin. Günlükler, güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde açıklanan Azure multi-Factor Authentication günlükleri içerir.

Bir başarısız oturum açma olayı (olay kimliği 6273) görüntüleyen bir güvenlik günlüğü örneği aşağıda gösterilmiştir:

![Başarısız bir oturum açma etkinliği gösteren güvenlik günlüğü](./media/howto-mfa-nps-extension-vpn/image47.png)

Azure multi-Factor Authentication günlük ilgili bir olayın aşağıda gösterilmiştir:

![Azure multi-Factor Authentication günlüğe kaydeder](./media/howto-mfa-nps-extension-vpn/image48.png)

Gelişmiş sorun giderme yapmak için NPS hizmetinin yüklendiği NPS veritabanı biçimi günlük dosyalarına başvurun. Günlük dosyaları oluşturulur _%SystemRoot%\System32\Logs_ klasör virgülle ayrılmış metin dosyalarını olarak. Günlük dosyalarının açıklaması için bkz [NPS veritabanı biçimi günlük dosyalarını yorumlamak](https://technet.microsoft.com/library/cc771748.aspx).

Bunları bir elektronik tabloya veya veritabanına dışarı aktarılmadığı sürece yorumlamak bu günlük dosyaları girişleri zordur. Birçok Internet Kimlik Doğrulama Hizmeti (günlük dosyaları yorumlama içinde size yardımcı olması için çevrimiçi Araçlar ayrıştırma IAS) bulabilirsiniz. Bir çıkış böyle indirilebilir [paylaşılan yazılım uygulama](https://www.deepsoftware.com/iasviewer) şurada gösterilmiştir:

![Paylaşılan yazılım uygulama IAS ayrıştırıcı örneği](./media/howto-mfa-nps-extension-vpn/image49.png)

Ek sorun giderme yapmak için bir protokol çözümleyici Wireshark gibi kullanabilirsiniz veya [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Aşağıdaki görüntüde Wireshark VPN sunucusu ile NPS arasında RADIUS iletileri gösterir.

![Microsoft Message Analyzer'ın filtrelenen trafik gösteriliyor](./media/howto-mfa-nps-extension-vpn/image50.png)

Daha fazla bilgi için [mevcut NPS altyapınızı Azure multi-Factor Authentication ile tümleştirme](howto-mfa-nps-extension.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure çok faktörlü kimlik doğrulama Al](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
