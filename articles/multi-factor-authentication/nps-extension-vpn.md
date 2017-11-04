---
title: "NPS uzantısı kullanılarak Azure MFA ile VPN tümleştirme | Microsoft Docs"
description: "Bu makalede, Microsoft Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak Azure MFA ile VPN altyapınız tümleştirme anlatılmaktadır."
services: active-directory
keywords: "Azure MFA, VPN, Azure Active Directory, ağ ilkesi sunucusu uzantısı tümleştirme"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 8ac4c5d88e724febf21fe6bcc8ecf939283f361e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a>VPN altyapınız ile Azure çok faktörlü kimlik doğrulaması (Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak MFA) tümleştirme

## <a name="overview"></a>Genel Bakış

Azure ağ ilkesi sunucusu (NPS) uzantısı kuruluşların Uzaktan Kimlik Doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) istemci kimlik doğrulaması kullanarak bulut tabanlı korumaya izin veren [Azure çok faktörlü kimlik doğrulama (MFA)](multi-factor-authentication-get-started-server-rdg.md), iki aşamalı doğrulama sağlar.

Bu makalede, bir VPN kullanarak ağınıza bağlanmak deneyen kullanıcılar için güvenli iki aşamalı doğrulamayı etkinleştirmek için Azure için NPS uzantısını kullanarak NPS altyapı Azure MFA ile tümleştirmek için yönergeler sağlar. 

Ağ İlkesi ve Erişim Hizmetleri (NPS) kuruluşlar aşağıdaki yetenekleri sağlar:

* Yönetim ve ağ isteklerinin kimin, ne zaman bağlantılara izin verilir, günün bağlantıları süresini ve istemcilerin bağlanmanız ve benzeri güvenlik düzeyini bağlanabileceğini belirtmek için denetim merkezi konumlarını belirtin. Bu ilkelerin her VPN veya Uzak Masaüstü (RD) Ağ Geçidi sunucusunda belirtmek yerine, bu ilkeler merkezi bir konumda bir kez belirtilebilir. RADIUS protokolü merkezi kimlik doğrulama, yetkilendirme ve hesap oluşturma (AAA) sağlamak için kullanılır. 
* Kurmak ve cihazları ağ kaynaklarına Kısıtlanmamış veya kısıtlanmış erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri zorlayın.
* Kimlik doğrulama ve yetkilendirme access 802.1 zorlamak için sağladıkları x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.    

Daha fazla bilgi için bkz: [ağ ilkesi sunucusu (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Güvenliği artırmak ve yüksek düzeyde uyumluluk sağlamak için kuruluşlar tümleştirebilirsiniz NPS kullanıcılar için iki aşamalı doğrulamayı kullandığınızdan emin olmak için Azure MFA ile bağlanmak için VPN sunucuda sanal bağlantı noktası. Kullanıcıların erişim verilmesi için kullanıcının kendi denetiminde sahip bilgilerle kendi kullanıcı adı/parola bileşimi sağlamanız gerekir. Bu bilgiler güvenilir ve kolayca çoğaltılması, cep telefonu numarası, telefona numarası, uygulama, bir mobil cihaz ve benzerleri gibi.

Azure NPS uzantısı kullanıma açılmadan önce tümleşik NPS ve Azure MFA ortamlar için iki aşamalı doğrulamayı uygulamak istediğinizde müşteriler yapılandırıp ayrı bir MFA sunucusu şirket içi ortamda açıklandığı gibi gerekiyordu Uzak Masaüstü Ağ geçidi ve Azure multi-Factor Authentication sunucusu RADIUS kullanma.

Azure NPS uzantısı kullanılabilirliğini şimdi kuruluşlar için güvenli RADIUS istemci kimlik doğrulaması şirket tabanlı MFA çözümünü veya bulut tabanlı bir MFA çözümü dağıtmak için seçmenizi sağlar.
 
## <a name="authentication-flow"></a>Kimlik doğrulama akışı
Bir kullanıcı bir VPN sunucusu üzerindeki bir sanal bağlantı noktasına bağlandığında, bunlar ilk kullanıcı adı/parola ve sertifika tabanlı kimlik doğrulama yöntemlerinin bir birleşimini kullanılmasına izin protokolleri çeşitli kullanarak kimliğini doğrulaması gerekir. 

Kimlik doğrulaması ve kimlik doğrulama ek olarak, kullanıcıların uygun arama izinleri olmalıdır. Basit uygulamalarında, erişim izni bu çevirmeli izinler doğrudan Active Directory kullanıcı nesneleri üzerinde ayarlanır. 

 ![Kullanıcı özellikleri](./media/nps-extension-vpn/image1.png)

Basit uygulamalar için her VPN sunucusu verir veya erişimi her yerel VPN sunucusunda tanımlanan ilkelere bağlı olarak reddeder.

Daha büyük ve daha fazla ölçeklenebilir uygulamalarında, bu verme ilkeleri veya VPN erişimi RADIUS sunucularına merkezi reddedin. Bu durumda, VPN sunucusu bağlantı isteklerini ve hesap iletilerini bir RADIUS sunucusuna iletir bir erişim sunucusu (RADIUS istemcisi) gibi davranır. VPN sunucuda sanal bağlantı noktası bağlanmak için kullanıcıların kimliğinin doğrulanması gerekir ve RADIUS sunucularına merkezi olarak tanımlanmış koşullara uyan. 

Azure NPS uzantısı NPS ile tümleştirildiğinde, başarılı kimlik doğrulaması akışı aşağıdaki gibidir:

1. VPN sunucusu kullanıcı adı ve Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için parola içeren bir VPN kullanıcı kimlik doğrulama isteği alır. 
2. Bir RADIUS istemcisi olarak işlev gören, VPN sunucusunun RADIUS Erişim-İstek iletisi için istek dönüştürür ve iletiyi gönderir (parola şifrelenir) NPS uzantısı yüklü olduğu RADIUS (NPS) sunucusuna. 
3. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır. Kullanıcı adı / parola yanlış olduğundan, RADIUS sunucusu bir erişim reddi iletisi gönderir. 
4. Ağ ilkeleri ve NPS bağlantı isteğini belirtildiği gibi tüm koşullar karşılanıyorsa (örneğin, süresi gün veya grup üyeliği kısıtlamaları), NPS uzantısı Azure MFA ile ikincil kimlik doğrulama isteği tetikler. 
5. Azure MFA, Azure Active Directory ile iletişim kurar, kullanıcının ayrıntılarını alır ve (SMS mesajı, mobil uygulama ve benzeri) kullanıcı tarafından yapılandırılan yöntemini kullanarak ikincil kimlik doğrulaması gerçekleştirir. 
6. MFA testini Başarı sonucu NPS uzantısı sonucu Azure MFA ile iletişim kurar.
7. Bağlantı denemesi kimliği doğrulanmış ve yetkilendirilmiş sonra uzantısı yüklendiği NPS sunucusunun RADIUS Erişim Kabul iletisi VPN sunucusu (RADIUS istemcisi) gönderir.
8. Kullanıcı VPN sunucuda sanal bağlantı noktası erişimi verilir ve şifreli bir VPN tüneli oluşturur.

## <a name="prerequisites"></a>Ön koşullar
Bu bölümde, Uzak Masaüstü Ağ geçidi ile Azure MFA tümleştirme önce gerekli önkoşulları ayrıntıları verilmektedir. Başlamadan önce aşağıdaki önkoşulların yerinde olması gerekir.

* VPN altyapısı
* Ağ İlkesi ve Erişim Hizmetleri (NPS) rol
* Azure MFA lisans
* Windows Server yazılımı
* Kitaplıkları
* Azure AD synched ile şirket içi AD 
* Azure Active Directory GUID kimliği

### <a name="vpn-infrastructure"></a>VPN altyapısı
Bu makalede, Microsoft Windows Server 2016 yerinde kullanarak bir çalışma VPN altyapısına sahip ve VPN sunucusu şu anda bağlantı isteklerini iletmek için bir RADIUS sunucusuna yapılandırılmadığını varsayar. Bu kılavuzda merkezi bir RADIUS sunucusuna kullanmak için VPN altyapısı yapılandırır.

Yerinde bir çalışma altyapısı yoksa Microsoft ve üçüncü taraf sitelere bulabilirsiniz çok sayıda VPN Kurulumu öğreticileri sağlanan yönergeleri izleyerek, hızlı bir şekilde bu altyapı oluşturabilirsiniz. 

### <a name="network-policy-and-access-services-nps-role"></a>Ağ İlkesi ve Erişim Hizmetleri (NPS) rol

NPS rol hizmetinin, RADIUS sunucusu ve istemci işlevselliği sağlar. Bu makalede, NPS rol bir üye sunucu veya etki alanı denetleyicisinde ortamınızda yüklediğiniz varsayılır. Bu kılavuzdaki VPN yapılandırması için RADIUS yapılandırır. NPS rolü bir sunucuya yüklemek _diğer_ VPN sunucunuzun daha.

NPS rolü yükleme hakkında bilgi için Windows Server 2012 hizmet veya üzeri, bkz: [bir NAP sistem durumu ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). Ağ erişim ilkesi (NAP), Windows Server 2016'da kullanım dışıdır. Bir etki alanı denetleyicisinde NPS yüklemek için öneri dahil olmak üzere NPS için en iyi uygulamaları bir açıklaması için bkz: [NPS için en iyi uygulamaları](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA lisans

Gerekli bir Azure AD Premium, Enterprise Mobility artı güvenlik (EMS) veya bir MFA abonelik kullanılabilir olan Azure MFA bir lisans kullanılır. Daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulama alma](multi-factor-authentication-versions-plans.md). Test amacıyla bir deneme aboneliği kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

Windows Server 2008 R2 SP1 NPS uzantısı gerektirir veya üstü yüklü NPS rol hizmetine sahip. Bu kılavuzdaki tüm adımları, Windows Server 2016 kullanılarak gerçekleştirilen.

### <a name="libraries"></a>Kitaplıkları

Bu kitaplıklar uzantısı ile otomatik olarak yüklenir.

-   [Visual Studio 2013 (X64) için Visual C++ yeniden dağıtılabilir paketleri](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory için Windows PowerShell modülü sürümü 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory için Windows PowerShell modülü, zaten var, Kurulum işleminin bir parçası olarak çalışan bir yapılandırma komut dosyası aracılığıyla değilse yüklenir. Zaten yüklü değilse bu modül önceden yüklemeye gerek yoktur.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Şirket içi Active Directory ile Azure Active Directory synched 

NPS uzantısını kullanmak için şirket içi kullanıcıları Azure Active Directory ile eşitlenen ve çok faktörlü kimlik doğrulaması için etkinleştirilmiş olmalıdır. Bu kılavuz, şirket içi kullanıcıları Azure AD Connect kullanarak Active Directory'e ile eşitlenir varsayar. Kullanıcılara MFA için etkinleştirmeye yönelik yönergeler aşağıda verilmiştir.
Azure AD hakkında bilgi için bağlanmak, bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği 
NPS uzantıyı yüklemek için Azure Active Directory GUID bilmeniz gerekir. Sonraki bölümde Azure Active Directory GUID bulmak için yönergeler sağlanmaktadır.

## <a name="configure-radius-for-vpn-connections"></a>VPN bağlantıları için RADIUS yapılandırın

Bir üye sunucuda NPS sunucusu rolünü yüklediyseniz, kimlik doğrulaması ve bu isteği VPN bağlantılarını VPN istemcisi yetkilendirmek için yapılandırmanız gerekir. 

Bu bölümde, ağ ilkesi sunucusu rolünü yüklediyseniz ancak kullanım için altyapınızda yapılandırmadınız varsayar.

>[!NOTE]
>Merkezi bir RADIUS sunucusuna kimlik doğrulaması için kullandığı bir çalışma VPN sunucusu zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir
Bu senaryoda düzgün çalışması için NPS sunucusunun Active Directory'de kayıtlı olması gerekir.

1. Sunucu Yöneticisi'ni açın.
2. Sunucu Yöneticisi'nde **Araçları**ve ardından **ağ ilkesi sunucusu**. 
3. Ağ İlkesi Sunucusu konsolunda sağ **NPS (yerel)**ve ardından **Active Directory'de kayıt sunucusu**. Tıklatın **Tamam** iki kez.

 ![Ağ İlkesi Sunucusu](./media/nps-extension-vpn/image2.png)

4. Konsolunu sonraki yordam için açık bırakın.

### <a name="use-wizard-to-configure-radius-server"></a>RADIUS sunucusu Yapılandırma Sihirbazı'nı kullanın
Standart (Sihirbaz tabanlı) kullanabilirsiniz veya gelişmiş yapılandırma seçeneği RADIUS sunucusunu yapılandırmak için. Bu bölümde, sihirbaz tabanlı standart yapılandırma seçeneğinin kullanımını varsayar.

1. Ağ İlkesi Sunucusu konsolunda **NPS (yerel)**.
2. Standart yapılandırmada seçin **çevirmeli veya VPN bağlantıları için RADIUS sunucusu**ve ardından **yapılandırma VPN veya çevirmeli**.

 ![VPN bağlantısını yapılandırma](./media/nps-extension-vpn/image3.png)

3. Select çevirmeli veya sanal özel ağ bağlantıları türü sayfasında, seçin **sanal özel ağ bağlantıları**, tıklatıp **sonraki**.

 ![Sanal özel ağ](./media/nps-extension-vpn/image4.png)

4. Belirtin çevirmeli veya VPN sunucusu sayfasında, tıklatın **Ekle**.
5. İçinde **yeni RADIUS istemcisi** iletişim kutusu, kolay bir ad sağlayın, çözümlenebilir adını veya VPN sunucusunun IP adresini girin ve paylaşılan gizli bir parola girin. Bu paylaşılan gizli parola uzun ve karmaşık olun. Sonraki bölümdeki adımları için gerek duyduğunuz bu parolayı kaydedin.

 ![Yeni RADIUS istemcisi](./media/nps-extension-vpn/image5.png)

6. Tıklatın **Tamam**ve ardından **sonraki**.
7. Üzerinde **kimlik doğrulama yöntemleri yapılandırma** sayfasında, varsayılan seçimi (Microsoft Şifreli kimlik doğrulaması sürüm 2 (MS-CHAPv2) veya başka bir seçenek belirleyin ve **sonraki**.

  >[!NOTE]
  >Genişletilebilir Kimlik Doğrulama Protokolü (EAP) yapılandırırsanız, MS CHAPv2 veya PEAP kullanmanız gerekir. Diğer bir EAP desteklenir.
 
8. Kullanıcı grupları belirtin sayfasında, tıklatın **Ekle** ve varsa, uygun bir grup seçin. Aksi takdirde, seçim tüm kullanıcılara erişim vermek için boş bırakın.

 ![Kullanıcı gruplarını belirtin](./media/nps-extension-vpn/image7.png)

9. **İleri**’ye tıklayın.
10. IP filtreleri belirtin sayfasında, tıklatın **sonraki**.
11. Şifreleme ayarlarını belirtin sayfasında, varsayılan ayarları kabul edin ve **sonraki**.

 ![Şifreleme belirtin](./media/nps-extension-vpn/image8.png)

12. Bölge adı belirtin sayfasında adını boş bırakın, varsayılan ayarı kabul etmek ve tıklayın **sonraki**.

 ![Bir bölge adı belirtin](./media/nps-extension-vpn/image9.png)

13. Tamamlanıyor yeni çevirmeli veya sanal özel ağ bağlantıları ve RADIUS istemcileri sayfasında tıklatın **son**.

 ![Tam bağlantıları](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>RADIUS yapılandırmasını doğrulayın.
Bu bölümde, Sihirbazı'nı kullanarak oluşturduğunuz yapılandırma ayrıntılarını verir.

1. NPS sunucusunda, NPS (yerel) konsolunda, RADIUS istemcileri'ni genişletin ve seçin **RADIUS istemcileri**.
2. Sihirbazı kullanılarak oluşturulan RADIUS İstemcisi'nin ayrıntılar bölmesinde, sağ tıklayın ve **özellikleri**. RADIUS istemciniz (VPN sunucusu) için özellikler kayıtlarla aşağıda gösterilen benzer olmalıdır.

 ![VPN özellikleri](./media/nps-extension-vpn/image11.png)

3. Tıklatın **iptal**.
4. NPS (yerel) konsolunda, NPS sunucusunu genişletin **ilkeleri**seçip **bağlantı isteği ilkeleri**. Aşağıdaki görüntü benzer VPN bağlantıları İlkesi görmeniz gerekir.

 ![Bağlantı istekleri](./media/nps-extension-vpn/image12.png)

5. İlkeleri altında seçin **ağ ilkeleri**. Aşağıdaki görüntü benzer bir sanal özel ağ (VPN) bağlantıları İlkesi gerekir.

 ![Ağ Özellikleri](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a>VPN sunucusunu, RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın
Bu bölümde, VPN sunucusunun RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın. Bu bölümde VPN sunucusunun çalışma yapılandırmaya sahip ancak VPN sunucusunun RADIUS kimlik doğrulaması kullanacak şekilde yapılandırılmamış olduğu varsayılır. VPN sunucusunu yapılandırdıktan sonra yapılandırmanızı beklendiği gibi çalıştığını doğrulayın.

>[!NOTE]
>RADIUS kimlik doğrulaması kullanan çalışan bir VPN sunucusu yapılandırması zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="configure-authentication-provider"></a>Kimlik doğrulama sağlayıcısı yapılandırma
1. VPN sunucusunda, Sunucu Yöneticisi'ni açın.
2. Sunucu Yöneticisi'nde **Araçları**ve ardından **Yönlendirme ve Uzaktan erişim**.
3. Yönlendirme ve Uzaktan erişim konsolunda sağ  **\[sunucu adı\] (yerel)**ve ardından **özellikleri**.

 ![Yönlendirme ve Uzaktan erişim](./media/nps-extension-vpn/image14.png)
 
4. İçinde **[sunucu adı} (yerel) özellikleri** iletişim kutusu, tıklatın **güvenlik** sekmesi. 
5. Üzerinde **güvenlik** kimlik doğrulama sağlayıcısı altında sekmesini **RADIUS kimlik doğrulaması**ve ardından **yapılandırma**.

 ![RADIUS Kimlik Doğrulaması](./media/nps-extension-vpn/image15.png)
 
6. RADIUS kimlik doğrulaması iletişim kutusunda tıklatın **Ekle**.
7. Adı veya IP adresi önceki bölümde yapılandırılmış RADIUS sunucusunun RADIUS sunucusu Ekle, sunucu adı ekleyin.
8. Paylaşılan gizlilik tıklatın **değişiklik** ve oluşturulan ve daha önce kaydedilmiş paylaşılan gizli parola ekleyin.
9. Zaman aşımı (saniye)'de, değeri arasında bir değer olarak değiştirin. **30** ve **60**. Bu ikinci kimlik doğrulama faktörü tamamlamak için yeterli zamana izin vermek gereklidir.
 
 ![RADIUS sunucusu Ekle](./media/nps-extension-vpn/image16.png)
 
10. Tıklatın **Tamam** tüm iletişim kutularını kapatma işlemi tamamlanana kadar.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme
Bu bölümde, VPN istemci kimlik doğrulaması ve VPN sanal bağlantı noktasına bağlanmayı denediğinizde RADIUS sunucusu tarafından yetkili olduğunu onaylayın. Bu bölümde, bir VPN istemci olarak Windows 10 kullandığınızı varsayar. 

>[!NOTE]
>VPN sunucusuna bağlanmak ve kaydedilmiş ayarları için bir VPN istemcisi zaten yapılandırdıysanız, bir VPN bağlantısı nesnesi kaydetme ve yapılandırma ile ilgili adımları atlayabilirsiniz.
>

1. VPN istemci bilgisayarında **Başlat**ve ardından **ayarları** (dişli simgesi).
2. Pencere ayarlar'ı **ağ ve Internet**.
3. Tıklatın **VPN**.
4. Tıklatın **bir VPN bağlantısı Ekle**.
5. Bir VPN bağlantısı Ekle, VPN sağlayıcısı olarak Windows (yerleşik) belirtmek sonra uygun şekilde kalan alanları doldurun ve tıklatın **kaydetmek**. 

 ![VPN bağlantısı Ekle](./media/nps-extension-vpn/image17.png)
 
6. Açık **ağ ve Paylaşım Merkezi** Denetim Masası'nda.
7. Tıklatın **Bağdaştırıcı ayarlarını değiştir**.

 ![Bağdaştırıcı ayarlarını değiştir](./media/nps-extension-vpn/image18.png)

8. VPN ağ bağlantısına sağ tıklayın ve Özellikler'i tıklatın. 

 ![VPN ağ özellikleri](./media/nps-extension-vpn/image19.png)

9. VPN Özellikleri iletişim kutusunda tıklatın **güvenlik** sekmesi. 
10. Güvenlik sekmesinde, yalnızca olun **Microsoft CHAP Version 2 (MS-CHAP v2)** seçilir ve Tamam'ı tıklatın.

 ![Protokollere izin ver](./media/nps-extension-vpn/image20.png)

11. VPN bağlantısı sağ tıklayın ve **Bağlan**.
12. Ayarlar sayfasında, tıklatın **Bağlan**.

Aşağıda gösterildiği gibi RADIUS sunucusu olarak olay kimliği 6272'in, güvenlik günlüğüne başarılı bir bağlantı görüntülenir.

 ![Olay Özellikleri](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Sorun giderme kılavuzu
VPN sunucusu kimlik doğrulama ve yetkilendirme için merkezi bir RADIUS sunucusu kullanmak üzere yapılandırılmış önce VPN yapılandırmanızı çalışır durumda olduğunu varsayalım. Bu durumda, RADIUS sunucusu yanlış yapılandırılması veya geçersiz kullanıcı adı veya parola kullanımını sorunu kaynaklanabilir olasıdır. Örneğin, kullanıcı alternatif UPN soneki kullanırsanız, oturum açma denemesi başarısız olabilir (en iyi sonuçlar için aynı hesabı adı kullanmanız gerekir). 

Bu sorunları gidermek için başlatmak için bir ideal RADIUS sunucusunda güvenlik olay günlüklerini incelemek için yerdir. Zaman olayları arama kaydetmek için aşağıdaki Göster Olay Görüntüleyicisi'nde rol tabanlı ağ ilkesi ve erişim sunucusu özel görünüm kullanabilirsiniz. Olay Kimliği 6273 burada ağ ilkesi sunucusu bir kullanıcının erişimini reddetti olayları gösterir. 

 ![Olay Görüntüleyicisi](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma
Bu bölümde, mfa ve iki aşamalı doğrulama için hesapları kurma için kullanıcıları etkinleştirme için yönergeler sağlar. 

### <a name="enable-multi-factor-authentication"></a>Multi-factor authentication’ı etkinleştirme
Bu bölümde, Azure AD hesapları için MFA etkinleştirin. Kullanım **Klasik portal** kullanıcılar için MFA'yı etkinleştirmek için. 

1. Bir tarayıcı açın ve gidin [https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Yönetici olarak oturum açın.
3. Portalında, sol gezinti bölmesinde tıklatın **ACTIVE DIRECTORY**.

 ![Varsayılan dizini](./media/nps-extension-vpn/image23.png)

4. Ad sütununda tıklatın **varsayılan dizin** (veya başka bir dizin (uygunsa).
5. Hızlı Başlangıç sayfasında, tıklatın **yapılandırma**.

 ![Varsayılan yapılandırma](./media/nps-extension-vpn/image24.png)

6. YAPILANDIRMA sayfasında aşağı kaydırın ve çok faktörlü kimlik doğrulaması bölümünde tıklayın **hizmet ayarlarını Yönet**.

 ![MFA ayarları yönetme](./media/nps-extension-vpn/image25.png)
 
7. Çok faktörlü kimlik doğrulama sayfasında, varsayılan hizmet ayarlarını gözden geçirin ve ardından **kullanıcılar**. 

 ![MFA Kullanıcıları](./media/nps-extension-vpn/image26.png)
 
8. Kullanıcılar sayfasında için MFA'yı etkinleştirin ve ardından istediğiniz kullanıcıları seçin **etkinleştirmek**.

 ![Özellikler](./media/nps-extension-vpn/image27.png)
 
9. İstendiğinde, tıklatın **etkinleştirmek çok öğeli kimlik doğrulama**.

 ![MFA'yı etkinleştirin](./media/nps-extension-vpn/image28.png)
 
10. **Kapat**’a tıklayın. 
11. Sayfayı yenileyin. MFA durum etkin olarak değiştirilir.

Kullanıcılar çok faktörlü kimlik doğrulamasını etkinleştirme hakkında daha fazla bilgi için bkz: [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>İki aşamalı doğrulama için hesaplarını yapılandırma
Bir hesap MFA için etkinleştirildikten sonra kullanıcıların iki aşamalı doğrulamayı kullanılan ikinci kimlik doğrulama faktörü için kullanılacak güvenilir bir aygıt başarılı bir şekilde yapılandırmadığınız sürece MFA İlkesi tarafından yönetilen kaynaklara oturum açmak mümkün değildir.

Bu bölümde, iki aşamalı doğrulamayla birlikte kullanmak için güvenilir bir aygıt yapılandırın. Bunlar, aşağıdakiler de dahil olmak üzere yapılandırmak kullanabileceğiniz birkaç seçenek vardır:

* **Mobil uygulama**. Bir Windows Phone, Android veya iOS aygıtında Microsoft Authenticator uygulamasını yükleyin. Kuruluşunuzun ilkelerine bağlı olarak, uygulamayı iki moddan birini kullanmak için gereklidir: (aygıtınıza bir bildirim gönderilir) bildirimlerin Doğrulamalar için veya (olduğunuz güncelleştiren bir doğrulama kodu girmek için gerekli doğrulama kodunu kullan Her 30 saniye). 
* **Cep telefonu araması veya SMS**. Bir otomatik telefon çağrısı veya SMS mesajı ya da alabilir. Telefon araması seçeneğiyle aramayı yanıtlayın ve kimlik doğrulaması yapmak için # işaretini tuşuna basın. Metin seçeneğiyle için SMS mesajını yanıtlayın veya oturum açma arabirimine doğrulama kodunu girin.
* **Ofis telefonu araması**. Bu işlem otomatik telefon aramaları için yukarıda açıklanan aynıdır.

Mobil uygulama doğrulama için anında iletme bildirimi almak için kullanılacak bir aygıtı ayarlamak için bu yönergeleri izleyin.

1. Oturum [https://aka.ms/mfasetup](https://aka.ms/mfasetup) veya herhangi bir sitede gibi [https://portal.azure.com](https://portal.azure.com), burada MFA etkin kimlik bilgilerinizi kullanarak kimlik doğrulaması için gerekli. 
2. Kullanıcı adı ve parola ile imzalama sırasında ek güvenlik doğrulaması için hesap ayarlamak ister bir ekrana sahip sunulur.

 ![Ek güvenlik](./media/nps-extension-vpn/image29.png)

3. Tıklatın **şimdi kurmak**.
4. Ek güvenlik doğrulama sayfasında, bir kişi bir türü (kimlik doğrulama telefon, ofis telefonu veya mobil uygulama) seçin. Ardından bir ülke veya bölgeyi seçin ve bir yöntem seçin. Yöntemi, seçtiğiniz kişi türüne göre değişir. Örneğin, mobil uygulama seçerseniz, doğrulama için bildirimlerin mi, yoksa bir doğrulama kodu kullanmayı seçebilirsiniz. Seçtiğiniz adımları varsayın **mobil uygulama** kişi türü.

 ![Telefon kimlik doğrulama](./media/nps-extension-vpn/image30.png)

5. Mobil uygulama seçin, **doğrulama için bildirimlerin**ve ardından **ayarlanan**. 

 ![Mobil uygulama doğrulama](./media/nps-extension-vpn/image31.png)
 
6. Henüz yapmadıysanız, authenticator mobil uygulamasında cihazınıza yükleyin. 
7. Sunulan barkod tarama veya bilgileri el ile girin ve ardından mobil uygulama'ndaki yönergeleri izleyin **Bitti**.

 ![Mobil uygulama yapılandırma](./media/nps-extension-vpn/image32.png)

8. Ek güvenlik doğrulama sayfasında, tıklatın **kişi benim** ve cihazınız için gönderilen bildirimi yanıtlayın.
9. Durumda mobil uygulamaya erişimi kaybetmek ve'ı tıklatın ek güvenlik doğrulama sayfasında kişi bir sayı girin **sonraki**.

 ![Cep telefonu numarası](./media/nps-extension-vpn/image33.png)
 
10. Ek güvenlik doğrulama tıklatın **Bitti**.

Cihaz ikinci bir doğrulama yöntemi sağlamak üzere yapılandırılmıştır. İki aşamalı doğrulama hesaplarını ayarlama hakkında daha fazla bilgi için bkz: [Hesabımı iki aşamalı doğrulama için ayarlama](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma

Bu bölümde VPN sunucusu ile istemci kimlik doğrulaması için Azure MFA kullanmaya VPN yapılandırma için yönergeler sağlar.

Yükleyip NPS uzantısı yapılandırdıktan sonra bu sunucu tarafından işlenen tüm RADIUS tabanlı istemci kimlik doğrulaması Azure MFA kullanmak için gereklidir. Aksi takdirde tüm VPN kullanıcıları Azure MFA kaydedilen, MFA kullanmak üzere yapılandırılmamış olan kullanıcıların kimliklerini doğrulamak için başka bir RADIUS sunucusunu ayarlayın. Veya yalnızca MFA'kaydedilirse, ikinci bir kimlik doğrulama faktörü sağlamak kullanıcıların yüküyle sağlayan bir kayıt defteri girdisi oluşturabilirsiniz. 

Adlı yeni bir dize değeri oluşturun _HKLM\SOFTWARE\Microsoft\AzureMfa REQUIRE_USER_MATCH_ve TRUE veya FALSE değerini ayarlayın. 

 ![Kullanıcı eşleşmesi iste](./media/nps-extension-vpn/image34.png)
 
Değeri TRUE olarak ayarlayın ya da ayarlı değil, tüm kimlik doğrulama isteklerini bir MFA sınaması tabi demektir. Değeri FALSE olarak ayarlarsanız, MFA zorluklar MFA'kaydedilen kullanıcılarına verilir. FALSE ayarı yalnızca sınama veya üretim ortamlarında ekleme süre boyunca kullanın.

### <a name="acquire-azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği alma

Yapılandırma NPS uzantısı'nın bir parçası olarak, Azure AD kiracınız için yönetici kimlik bilgileri ve Azure Active Directory kimliği sağlamanız gerekir. Aşağıdaki adımlar Kiracı kimliğini alma Göster

1. Oturum açmak için Azure portalında [https://portal.azure.com](https://portal.azure.com) Azure kiracının genel Yöneticisi olarak.
2. Sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.
3. **Özellikler**'e tıklayın.
4. Dizin Kimliğinizi panoya kopyalamak için seçin **kopyalama** simgesi.
 
 ![Dizin kimliği](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleyin
NPS uzantısı Ağ İlkesi ve Erişim Hizmetleri (NPS) rolü yüklü olan bir sunucuyu ve bu işlevler tasarımınızda RADIUS sunucusu olarak yüklenmesi gerekir. NPS uzantısı, Uzak Masaüstü sunucusuna yüklemeyin.

1. NPS uzantısını [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Kurulum yürütülebilir dosya (NpsExtnForAzureMfaInstaller.exe) NPS sunucusuna kopyalayın.
3. NPS sunucusunda, çift **NpsExtnForAzureMfaInstaller.exe**. İstenirse, tıklatın **çalıştırmak**.
4. Azure MFA kurulumu için NPS uzantısı iletişim kutusunda Yazılımı Lisans Koşulları'nı gözden denetleyin **lisans şart ve koşullarını kabul ediyorum**, tıklatıp **yükleme**.

 ![NPS uzantısı](./media/nps-extension-vpn/image36.png)
 
5. Azure MFA kurulumu için NPS uzantısı iletişim kutusunda tıklatın **Kapat**.  

 ![Kurulum başarılı](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Sertifikaları kullanmak için bir PowerShell Betiği kullanılarak NPS uzantısı ile yapılandırma
Güvenli iletişimler ve güvence emin olmak için NPS uzantısı tarafından sertifikaları kullanmak için yapılandırmanız gerekir. NPS bileşenler, NPS ile otomatik olarak imzalanan bir sertifika kullanmak için yapılandırır bir Windows PowerShell komut dosyası içerir. 

Komut dosyası, aşağıdaki eylemleri gerçekleştirir:

* Kendinden imzalı bir sertifika oluşturur
* Hizmet üzerinde Azure AD sorumlusu sertifikasının ortak anahtarı ilişkilendirir
* Sertifika yerel makine deposunda saklar
* Sertifikanın özel anahtarı ağ kullanıcı erişimi verir
* Ağ İlkesi Sunucusu hizmetini yeniden başlatır

Kendi sertifikalarını kullanmak istiyorsanız, üzerinde Azure AD hizmet sorumlusu için sertifikanın ortak anahtarının ilişkilendirmek ve benzeri gerekir.
Betik kullanmak için Azure Active Directory yönetici kimlik bilgilerinizi ve daha önce kopyaladığınız Azure Active Directory Kiracı kimliği ile uzantısı sağlar. Komut dosyası, NPS uzantısı yüklediğiniz her NPS sunucusunda çalıştırın.

1. Bir yönetici Windows PowerShell komut istemini açın.
2. PowerShell komut isteminde yazın _cd 'c:\Program Files\Microsoft\AzureMfa\Config'_ve basın **ENTER**.
3. Tür _.\AzureMfsNpsExtnConfigSetup.ps1_ve basın **ENTER**. 
 * Azure Active Directory PowerShell Modülü yüklü olup olmadığını görmek için komut dosyasını denetler. Yüklenmemişse, betik modülü yükler.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. PowerShell modülünü yükleme komut dosyası doğruladıktan sonra Azure Active Directory PowerShell modülü iletişim kutusunu görüntüler. İletişim kutusunda, Azure AD yönetim kimlik bilgilerini ve parolanızı girin ve tıklatın **oturum**. 
 
 ![PowerShell oturum açma](./media/nps-extension-vpn/image39.png)
 
5. İstendiğinde, kopyaladığınız panoya önceki Kiracı kimliği yapıştırın ve tuşuna basın **ENTER**. 

 ![Kiracı Kimliği](./media/nps-extension-vpn/image40.png)

6. Komut dosyası otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri yapar. Çıktı, görüntünün aşağıda gösterildiği gibi şeklindedir.

 ![Otomatik olarak imzalanan sertifika](./media/nps-extension-vpn/image41.png)

7. Sunucuyu yeniden başlatın.
 
### <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Yapılandırmasını doğrulamak için VPN sunucusu ile yeni bir VPN bağlantısı yapmanız gerekir. Kimlik bilgilerinizi birincil kimlik doğrulaması için başarıyla girildikten sonra VPN bağlantısı ikincil kimlik doğrulama bağlantı kurulmadan önce aşağıda gösterildiği gibi başarılı olması bekler. 

 ![Yapılandırmayı doğrulama](./media/nps-extension-vpn/image42.png)

Başarılı bir şekilde Azure MFA önceden yapılandırılmış ikincil doğrulama yöntemi, kimlik doğrulaması, kaynağa bağlanır. Ancak, ikincil kimlik doğrulaması başarılı olmazsa, kaynak için erişim engellenir. 

Aşağıdaki örnekte, bir Windows Phone Doğrulayıcı uygulama ikincil kimlik doğrulaması sağlamak için kullanılır.

 ![Kullanıcı hesabının doğrulanması](./media/nps-extension-vpn/image43.png)

İkincil bir yöntem kullanarak başarıyla doğrulandıktan sonra VPN sunucuda sanal bağlantı noktası erişimi verilir. Ancak, güvenilen bir cihazda bir mobil uygulama kullanarak bir ikincil kimlik doğrulama yöntemini kullanmak için gerekli olduğundan, işlem günlüğünde yalnızca bir kullanıcı adı kullanarak daha daha güvenli olan / parola bileşimi.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi'ni günlüklerini görüntüle
Başarılı oturum açma olaylarını Windows Olay Görüntüleyicisi günlüklerinde görüntülemek için NPS sunucusu üzerindeki Windows Güvenlik günlüğü sorgulamak için aşağıdaki Windows PowerShell komutu yayımlayabilir.

Başarılı oturum açma olaylarını Güvenlik Olay Görüntüleyicisi günlüklerinde sorgulamak için aşağıdaki komutu kullanın,
* _Get-WinEvent - günlükadı güvenlik_ | burada {$_.ID - eq '6272'in '} | FL 

 ![Güvenlik Olay Görüntüleyicisi](./media/nps-extension-vpn/image44.png)
 
Aşağıda gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünüm de görüntüleyebilirsiniz:

 ![Ağ İlkesi erişimi](./media/nps-extension-vpn/image45.png)

Azure MFA için NPS uzantısı yüklendiği sunucuda Olay Görüntüleyicisi'ni uygulama günlüklerini uzantısına özgü bulabileceğiniz **uygulama ve Hizmetleri Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent - günlükadı güvenlik_ | burada {$_.ID - eq '6272'in '} | FL

 ![Olay (Event) Sayısı](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Sorun giderme kılavuzu
Yapılandırma beklendiği gibi çalışmıyorsa, sorun giderme başlamak için uygun bir kullanıcı Azure MFA kullanmak için yapılandırıldığını doğrulamak için yerdir. Bağlanmak için kullanıcının sahip [https://portal.azure.com](https://portal.azure.com). İkincil kimlik doğrulaması için istenir ve başarıyla için kimlik doğrulaması, Azure mfa yapılandırması hatalı ortadan kaldırabilirsiniz.

Azure MFA için kullanıcıları çalışıyorsa, ilgili olay günlüklerini gözden geçirmelidir. Bunlar, güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde tartışılan Azure MFA günlükleri içerir. 

Güvenlik günlüğü başarısız oturum açma olayı (olay kimliği 6273) gösteren bir örnek çıktı aşağıdadır:

 ![Güvenlik günlüğü](./media/nps-extension-vpn/image47.png)

İlgili olay AzureMFA günlüklerinden aşağıdadır:

 ![Azure MFA günlükleri](./media/nps-extension-vpn/image48.png)

Gerçekleştirmek için Gelişmiş Seçenekleri sorun giderme, NPS hizmetinin yüklü olduğu NPS veritabanı biçimi günlük dosyalarına bakın. Bu günlük dosyaları oluşturulur _%SystemRoot%\System32\Logs_ klasörü virgülle ayrılmış metin dosyalarını olarak. Bu bir açıklaması için günlük dosyaları, bkz: [NPS veritabanı biçimi günlük dosyalarını yorumlama](https://technet.microsoft.com/library/cc771748.aspx). 

Bir elektronik tablo veya bir veritabanına almadan yorumlamak bu günlük dosyaları yer alan girişleri zordur. Günlük dosyaları yorumlama yardımcı olmak için çevrimiçi IAS ayrıştırıcıları sayısı bulabilirsiniz. Bir çıkış aşağıdadır böyle indirilebilir [paylaşılan yazılım uygulama](http://www.deepsoftware.com/iasviewer): 

 ![Paylaşılan yazılım uygulama](./media/nps-extension-vpn/image49.png)

Son olarak, için ek seçenekleri sorun giderme, Wireshark gibi bir iletişim kuralı çözümleyicisi kullanabilir veya [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Wireshark aşağıdaki görüntüden VPN sunucusu arasındaki NPS sunucusunun RADIUS iletileri gösterir.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Daha fazla bilgi için bkz: [varolan NPS altyapınızı Azure çok faktörlü kimlik doğrulamasıyla tümleştirmek](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Sonraki adımlar
[Azure Multi-Factor Authentication’ı edinme](multi-factor-authentication-versions-plans.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](multi-factor-authentication-get-started-server-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../active-directory/connect/active-directory-aadconnect.md)

