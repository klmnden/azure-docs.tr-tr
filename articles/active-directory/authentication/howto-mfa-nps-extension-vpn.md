---
title: VPN ağ ilkesi sunucusu uzantısı kullanılarak Azure MFA ile tümleştirme | Microsoft Docs
description: VPN altyapınız, Microsoft Azure için Ağ İlkesi Sunucusu Uzantısı kullanılarak Azure MFA ile tümleştirin.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 08/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: cfdb89ae833dc2450a4670a84af305f1caa10591
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Azure için ağ ilkesi sunucusu uzantısını kullanarak VPN altyapınız Azure MFA ile tümleştirme

## <a name="overview"></a>Genel Bakış

Azure ağ ilkesi sunucusu (NPS) uzantısı kuruluşların Uzaktan Kimlik Doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) istemci kimlik doğrulaması kullanarak bulut tabanlı korumaya izin veren [Azure çok faktörlü kimlik doğrulama (MFA)](howto-mfaserver-nps-rdg.md), iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısını kullanarak NPS altyapı MFA ile tümleştirmek için yönergeler sağlar. Bu işlem bir VPN kullanarak ağa bağlanma girişiminde kullanıcılar için güvenli iki aşamalı doğrulamayı etkinleştirir. 

Ağ İlkesi ve Erişim Hizmetleri imkanı kuruluşlar için:

* Merkezi bir konum belirtmek için ağ isteklerinin denetim ve Yönetim için ata:

    * Kimin bağlayabilir miyim 
    
    * Gün bağlantıların ne zaman izin verilir 
    
    * Bağlantı süresi
    
    * İstemcileri bağlanmak için kullanmaları gereken güvenlik düzeyi

    Merkezi bir konumda olduklarını sonra her VPN veya Uzak Masaüstü Ağ Geçidi sunucusunda ilkeleri belirtmek yerine bunu yapar. RADIUS protokolü merkezi kimlik doğrulama, yetkilendirme ve hesap oluşturma (AAA) sağlamak için kullanılır. 

* Kurmak ve cihazları ağ kaynaklarına Kısıtlanmamış veya kısıtlanmış erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri zorlayın.

* Kimlik doğrulama ve yetkilendirme access 802.1 zorlamak için bir yol sağlamak x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.   
Daha fazla bilgi için bkz: [ağ ilkesi sunucusu](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Güvenliği artırmak ve yüksek bir uyumluluk düzeyini sağlamak için kuruluşlar NPS kullanıcıların VPN sunucuda sanal bağlantı noktasına bağlanmak için iki aşamalı doğrulamayı kullandığınızdan emin olmak için Azure multi-Factor Authentication ile tümleştirebilirsiniz. Kullanıcıların erişim verilmesi için kullanıcı adı ve parola birleşimi ve bunların denetim diğer bilgileri sağlamaları gerekir. Bu bilgileri güvenilir ve kolayca çoğaltılması gerekir. Cep telefonu numarası, telefona numarası veya bir uygulamayı bir mobil cihazda içerebilir.

Azure NPS uzantısı kullanılabilirliği önce NPS için iki aşamalı doğrulamayı uygulayan isteyen müşteriler tümleşik ve MFA ortamları yapılandırmak ve bir şirket içi ortamda ayrı bir MFA sunucusu korumak gerekiyordu. Bu kimlik doğrulama türü, Azure multi-Factor Authentication sunucusu RADIUS kullanan Uzak Masaüstü Ağ geçidi ile sunulur.

NPS ile Azure uzantısı, kuruluşların şirket içi dayalı MFA çözümünü veya bulut tabanlı bir MFA çözümü dağıtarak RADIUS istemci kimlik doğrulaması güvenliğini sağlayabilirsiniz.
 
## <a name="authentication-flow"></a>Kimlik doğrulama akışı
Kullanıcılar VPN sunucusu üzerindeki bir sanal bağlantı noktasına bağlandığında, bunlar ilk çeşitli protokollerini kullanarak kimlik doğrulaması gerekir. Protokolleri kullanıcı adı ve parola ve sertifika tabanlı kimlik doğrulama yöntemlerinin bir birleşimini kullanılmasına izin verin. 

Kimlik doğrulaması ve kendi kimlik doğrulama ek olarak, kullanıcıların uygun arama izinleri olmalıdır. Basit uygulamalarında, erişim izni arama izinleri doğrudan Active Directory kullanıcı nesneleri ayarlanır. 

![Kullanıcı özellikleri](./media/howto-mfa-nps-extension-vpn/image1.png)

Basit uygulamalarında her VPN sunucusu verir veya erişimi her yerel VPN sunucusunda tanımlanan ilkelere bağlı olarak reddeder.

Daha büyük ve daha fazla ölçeklenebilir uygulamalarında VPN erişim vermek veya reddetmek ilkeleri RADIUS sunucularına merkezi olarak kullanılır. Bu durumlarda, VPN sunucusu bağlantı isteklerini ve hesap iletilerini bir RADIUS sunucusuna iletir bir erişim sunucusu (RADIUS istemcisi) gibi davranır. VPN sunucuda sanal bağlantı noktası bağlanmak için kullanıcıların kimliğinin doğrulanması gerekir ve RADIUS sunucularında merkezi olarak tanımlanmış koşullara uyan. 

Azure NPS uzantısı NPS ile tümleştirildiğinde, başarılı kimlik doğrulama akışı, aşağıdaki gibi olur:

1. VPN sunucusu kullanıcı adı ve Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için parola içeren bir VPN kullanıcı kimlik doğrulama isteği alır. 

2. Bir RADIUS istemcisi olarak işlev gören, VPN sunucusu isteği için bir RADIUS dönüştürür *erişim isteği* iletisi ve RADIUS sunucusuna (şifrelenmiş parola ile), NPS uzantısı yüklendiği gönderir. 

3. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır. Kullanıcı adı veya parola yanlış ise, RADIUS sunucusu gönderir bir *erişim reddi* ileti. 

4. Ağ ilkeleri ve NPS bağlantı isteğini belirtildiği gibi tüm koşullar karşılanıyorsa (örneğin, süresi gün veya grup üyeliği kısıtlamaları), Azure multi-Factor Authentication ile ikincil kimlik doğrulama isteği NPS uzantısı tetikler. 

5. Azure çok faktörlü kimlik doğrulaması Azure Active Directory ile iletişim kurar, kullanıcının ayrıntılarını alır ve ikincil kimlik doğrulama (hücre telefon araması, SMS mesajı veya mobil uygulama) kullanıcı tarafından yapılandırılan yöntemini kullanarak gerçekleştirir. 

6. MFA testini başarılı olduğunda, Azure multi-Factor Authentication NPS uzantısı sonucu iletişim kurar.

7. Bağlantı denemesi kimliği doğrulanmış ve yetkilendirilmiş sonra uzantısı yüklü olduğu NPS bir RADIUS gönderir *erişim kabul* (RADIUS istemcisi) VPN sunucusuna ileti.

8. Kullanıcı VPN sunucuda sanal bağlantı noktası erişimi verilir ve şifreli bir VPN tüneli oluşturur.

## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, MFA Uzak Masaüstü Ağ geçidi ile tümleştirebilirsiniz önce tamamlanması gereken önkoşulları ayrıntılarını verir. Başlamadan önce aşağıdaki önkoşulların yerinde olmalıdır:

* VPN altyapısı
* Ağ İlkesi ve Erişim Hizmetleri rolü
* Azure çok faktörlü kimlik doğrulama lisansı
* Windows Server yazılımı
* Kitaplıkları
* Azure Active Directory (Azure AD) ile eşitlenen şirket içi Active Directory 
* Azure Active Directory GUID kimliği

### <a name="vpn-infrastructure"></a>VPN altyapısı
Bu makalede, Microsoft Windows Server 2016 kullanan bir çalışma VPN altyapısına sahip ve VPN sunucunuzun şu anda bağlantı isteklerini iletmek için bir RADIUS sunucusuna yapılandırılmadığını varsayar. Makalede, merkezi bir RADIUS sunucusu kullanmak üzere VPN altyapıyı yapılandırın.

Yerinde bir çalışma VPN altyapısı yoksa Microsoft ve üçüncü taraf sitelere bulabileceğiniz çok sayıda VPN Kurulumu öğreticileri yer alan yönergeleri izleyerek bir hızlı bir şekilde oluşturabilirsiniz. 
            
### <a name="the-network-policy-and-access-services-role"></a>Ağ İlkesi ve Erişim Hizmetleri rolü

Ağ İlkesi ve Erişim Hizmetleri, RADIUS sunucusu ve İstemcisi işlevselliği sağlar. Bu makalede, Ağ İlkesi ve Erişim Hizmetleri rolünün bir üye sunucu veya etki alanı denetleyicisinde ortamınızda yüklediğinizi varsayar. Bu kılavuzda, RADIUS VPN yapılandırması için yapılandırın. Ağ İlkesi ve Erişim Hizmetleri rolünün bir sunucuya yüklemek *dışında* VPN sunucunuzun.

Ağ İlkesi ve Erişim Hizmetleri rolünü yükleme hakkında bilgi için Windows Server 2012 hizmet veya daha sonra bkz: [bir NAP sistem durumu ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). NAP, Windows Server 2016'da kullanım dışıdır. Bir etki alanı denetleyicisinde NPS yüklemek için öneri dahil olmak üzere NPS için en iyi uygulamaları bir açıklaması için bkz: [en iyi uygulamalar için NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Azure MFA lisans

Azure çok faktörlü kimlik doğrulaması için bir lisansı gereklidir ve Azure AD Premium, Enterprise Mobility + güvenlik veya çok faktörlü kimlik doğrulaması tek başına lisans kullanılabilir. Tüketim tabanlı lisansları kullanıcı başına veya başına kimlik doğrulama lisansı gibi Azure MFA için NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulama alma](concept-mfa-licensing.md). Test amacıyla bir deneme aboneliği kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

Daha sonra Ağ İlkesi ve Erişim Hizmetleri rolü yüklü veya Windows Server 2008 R2 SP1 NPS uzantısı gereklidir. Bu kılavuzdaki tüm adımları Windows Server 2016 ile gerçekleştirilmiştir.

### <a name="libraries"></a>Kitaplıkları

Aşağıdaki kitaplıklar NPS uzantısı ile otomatik olarak yüklenir:

-   [Visual Studio 2013 (X64) için Visual C++ yeniden dağıtılabilir paketleri](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory için Windows PowerShell modülü sürümü 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory PowerShell modülü mevcut değilse, Kurulum işleminin bir parçası olarak çalışan bir yapılandırma betiğini birlikte yüklenir. Zaten yüklü değilse önceden modülünü yüklemek için gerek yoktur.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Şirket içi Active Directory ile eşitlenmiş Azure Active Directory 

NPS uzantısını kullanmak için şirket içi kullanıcıları Azure Active Directory ile eşitlenen ve MFA için etkinleştirilmiş olmalıdır. Bu kılavuz, şirket içi kullanıcıları Azure AD Connect yoluyla Azure Active Directory ile eşitlenen varsayar. Kullanıcılara MFA için etkinleştirmeye yönelik yönergeler aşağıda verilmiştir.

Azure AD Connect hakkında daha fazla bilgi için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği 

NPS uzantıyı yüklemek için Azure Active Directory GUID bilmeniz gerekir. Sonraki bölümde Azure Active Directory GUID bulmak için yönergeler sağlanmaktadır.

## <a name="configure-radius-for-vpn-connections"></a>VPN bağlantıları için RADIUS yapılandırın

NPS rol bir üye sunucuda yüklü değilse, kimlik doğrulaması ve bu istekleri VPN bağlantılarını VPN istemcisi yetkilendirmek için yapılandırmanız gerekir. 

Bu bölümde, Ağ İlkesi ve Erişim Hizmetleri rolünü yüklediyseniz ancak kullanım için altyapınızda yapılandırmadınız varsayar.

> [!NOTE]
> Merkezi bir RADIUS sunucusuna kimlik doğrulaması için kullandığı bir çalışma VPN sunucusu zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir
Bu senaryoda düzgün çalışması için NPS sunucusunun Active Directory'de kayıtlı olmalıdır.

1. Sunucu Yöneticisi'ni açın.

2. Sunucu Yöneticisi'nde seçin **Araçları**ve ardından **ağ ilkesi sunucusu**. 

3. Ağ İlkesi Sunucusu konsolunda sağ **NPS (yerel)** ve ardından **Active Directory'de kayıt sunucusu**. Seçin **Tamam** iki kez.

    ![Ağ İlkesi Sunucusu](./media/howto-mfa-nps-extension-vpn/image2.png)

4. Konsolunu sonraki yordam için açık bırakın.

### <a name="use-wizard-to-configure-the-radius-server"></a>RADIUS sunucusu Yapılandırma Sihirbazı'nı kullanın
Standart (Sihirbaz tabanlı) kullanabilirsiniz veya gelişmiş yapılandırma seçeneği RADIUS sunucusunu yapılandırmak için. Bu bölümde, sihirbaz tabanlı standart yapılandırma seçeneği kullandığınız varsayılır.

1. Ağ İlkesi Sunucusu konsolunda seçin **NPS (yerel)**.

2. Altında **standart yapılandırma**seçin **çevirmeli veya VPN bağlantıları için RADIUS sunucusu**ve ardından **yapılandırma VPN veya çevirmeli**.

    ![VPN bağlantısını yapılandırma](./media/howto-mfa-nps-extension-vpn/image3.png)

3. İçinde **çevirmeli veya sanal özel ağ bağlantı türünü seçin** penceresinde, seçin **sanal özel ağ bağlantıları**ve ardından **sonraki**.

    ![Sanal özel ağ](./media/howto-mfa-nps-extension-vpn/image4.png)

4. İçinde **belirtin çevirmeli veya VPN sunucusu** penceresinde, seçin **Ekle**.

5. İçinde **yeni RADIUS istemcisi** penceresinde, kolay bir ad sağlayın, çözümlenebilir adını veya VPN sunucusunun IP adresini girin ve ardından paylaşılan gizli bir parola girin. Paylaşılan gizli parola uzun ve karmaşık olun. Sonraki bölümde gerekir çünkü bunu kaydedin.

    ![Yeni RADIUS istemcisi](./media/howto-mfa-nps-extension-vpn/image5.png)

6. Seçin **Tamam**ve ardından **sonraki**.

7. İçinde **kimlik doğrulama yöntemleri yapılandırma** penceresinde, varsayılan seçim kabul (**Microsoft Şifreli kimlik doğrulaması sürüm 2 [MS-CHAPv2])** veya başka bir seçenek belirleyin ve işaretleyin **sonraki** .

    > [!NOTE]
    > Genişletilebilir Kimlik Doğrulama Protokolü (EAP) yapılandırırsanız, Microsoft Karşılıklı Kimlik Doğrulama Protokolü (CHAPv2) veya Korumalı Genişletilebilir Kimlik Doğrulama Protokolü (PEAP) kullanmanız gerekir. Diğer bir EAP desteklenir.
 
8. İçinde **kullanıcı gruplarını belirtmek** penceresinde, seçin **Ekle**ve ardından uygun bir grup seçin. Seçimi hiçbir grup zaten varsa, tüm kullanıcılara erişim vermek için boş bırakın.

    ![Kullanıcı gruplarını belirtmek penceresi](./media/howto-mfa-nps-extension-vpn/image7.png)

9. **İleri**’yi seçin.

10. İçinde **belirtin IP filtreleri** penceresinde, seçin **sonraki**.

11. İçinde **şifreleme ayarlarını belirt** penceresinde, varsayılan ayarları kabul edin ve ardından **sonraki**.

    ![Şifreleme ayarları belirtmek penceresi](./media/howto-mfa-nps-extension-vpn/image8.png)

12. İçinde **bir bölge adı belirtin** penceresinde, bölge adını boş bırakın, varsayılan ayarı kabul edin ve ardından **sonraki**.

    ![Bir bölge adı penceresi belirtin](./media/howto-mfa-nps-extension-vpn/image9.png)

13. İçinde **Tamamlanıyor yeni çevirmeli veya sanal özel ağ bağlantıları ve RADIUS istemcileri** penceresinde, seçin **son**.

    !["Tamamlanıyor yeni çevirmeli veya sanal özel ağ bağlantıları ve RADIUS istemcileri" penceresi](./media/howto-mfa-nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>RADIUS yapılandırmasını doğrulayın.
Bu bölümde Sihirbazı'nı kullanarak oluşturduğunuz yapılandırma ayrıntılarını verir.

1. NPS (yerel) konsolunda Ağ İlkesi sunucusundaki genişletin **RADIUS istemcileri**ve ardından **RADIUS istemcileri**.

2. Ayrıntılar bölmesinde, oluşturduğunuz ve ardından RADIUS istemcisi sağ **özellikleri**. RADIUS istemciniz (VPN sunucusu) özelliklerini olanlar aşağıda gösterildiği gibi olmalıdır:

    ![VPN özellikleri](./media/howto-mfa-nps-extension-vpn/image11.png)

3. Seçin **iptal**.

4. NPS (yerel) konsolunda Ağ İlkesi sunucusundaki genişletin **ilkeleri**ve ardından **bağlantı isteği ilkeleri**. VPN bağlantıları ilke aşağıdaki görüntüde gösterildiği gibi görüntülenir:

    ![Bağlantı istekleri](./media/howto-mfa-nps-extension-vpn/image12.png)

5. Altında **ilkeleri**seçin **ağ ilkeleri**. Aşağıdaki görüntüde gösterildiği ilke benzer bir sanal özel ağ (VPN) bağlantıları İlkesi görmeniz gerekir:

    ![Ağ ilkeleri](./media/howto-mfa-nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>VPN RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın
Bu bölümde, VPN sunucunuzu, RADIUS kimlik doğrulaması kullanacak şekilde yapılandırın. Yönergeleri bir VPN sunucusu bir çalışma yapılandırmasını sahip ancak RADIUS kimlik doğrulaması kullanacak şekilde yapılandırılmamış olduğu varsayılır. VPN sunucusunu yapılandırdıktan sonra yapılandırma beklendiği gibi çalıştığını doğrulayın.

> [!NOTE]
> RADIUS kimlik doğrulaması kullanan çalışan bir VPN sunucusu yapılandırması zaten varsa, bu bölümü atlayabilirsiniz.
>

### <a name="configure-authentication-provider"></a>Kimlik doğrulama sağlayıcısı yapılandırma
1. VPN sunucusunda, Sunucu Yöneticisi'ni açın.

2. Sunucu Yöneticisi'nde seçin **Araçları**ve ardından **Yönlendirme ve Uzaktan erişim**.

3. İçinde **Yönlendirme ve Uzaktan erişim** penceresinde sağ  **\<sunucu adı > (yerel)** ve ardından **özellikleri**.

    ![Yönlendirme ve Uzaktan erişim penceresi](./media/howto-mfa-nps-extension-vpn/image14.png)
 
4. İçinde  **\<sunucu adı > (yerel) özellikleri** penceresinde, seçin **güvenlik** sekmesi. 

5. Üzerinde **güvenlik** sekmesinde, altında **kimlik doğrulama sağlayıcısı**seçin **RADIUS kimlik doğrulaması**ve ardından **yapılandırma**.

    ![RADIUS Kimlik Doğrulaması](./media/howto-mfa-nps-extension-vpn/image15.png)
 
6. İçinde **RADIUS kimlik doğrulaması** penceresinde, seçin **Ekle**.

7. İçinde **RADIUS sunucusu Ekle** penceresinde aşağıdakileri yapın:

    a. İçinde **sunucu adı** kutusunda, adı veya önceki bölümde yapılandırılmış RADIUS sunucusunun IP adresini girin.

    b. İçin **paylaşılan gizlilik**seçin **değişiklik**ve ardından oluşturduğunuz ve daha önce kaydedilmiş paylaşılan gizli parolayı girin.

    c. İçinde **zaman aşımı (saniye)** kutusunda, bir değer seçerek **30** aracılığıyla **60**.  
    Zaman aşımı değeri ikinci kimlik doğrulama faktörü tamamlamak için yeterli zamana izin vermek gereklidir.
 
    ![RADIUS sunucusu Ekle penceresi](./media/howto-mfa-nps-extension-vpn/image16.png)
 
8. **Tamam**’ı seçin.

### <a name="test-vpn-connectivity"></a>VPN bağlantısını test etme
Bu bölümde, VPN istemci kimlik doğrulaması ve VPN sanal bağlantı noktasına bağlanmayı denediğinizde RADIUS sunucusu tarafından yetkili olduğunu onaylayın. Yönergeleri, VPN istemci olarak Windows 10 kullandığınızı varsayar. 

> [!NOTE]
> VPN sunucusuna bağlanmak ve kaydedilmiş ayarları için bir VPN istemcisi zaten yapılandırdıysanız, bir VPN bağlantısı nesnesi kaydetme ve yapılandırma ile ilgili adımları atlayabilirsiniz.
>

1. VPN istemci bilgisayarınızda seçin **Başlat** düğmesine tıklayın ve ardından **ayarları** düğmesi.

2. İçinde **Windows ayarları** penceresinde, seçin **ağ ve Internet**.

3. Seçin **VPN**.

4. Seçin **bir VPN bağlantısı Ekle**.

5. İçinde **bir VPN bağlantısı Ekle** penceresi, **VPN sağlayıcısı** kutusunda **Windows (yerleşik)**, uygun şekilde kalan alanları doldurun ve ardından seçin**Kaydetmek**. 

    !["Bir VPN bağlantısı Ekle" penceresi](./media/howto-mfa-nps-extension-vpn/image17.png)
 
6. Git **Denetim Masası**ve ardından **ağ ve Paylaşım Merkezi**.

7. Seçin **Bağdaştırıcı ayarlarını değiştir**.

    ![Bağdaştırıcı ayarlarını değiştir](./media/howto-mfa-nps-extension-vpn/image18.png)

8. VPN ağ bağlantısına sağ tıklayın ve ardından **özellikleri**. 

    ![VPN ağ özellikleri](./media/howto-mfa-nps-extension-vpn/image19.png)

9. VPN Özellikleri penceresinde seçin **güvenlik** sekmesi. 

10. Üzerinde **güvenlik** sekmesinde, yalnızca olun **Microsoft CHAP Version 2 (MS-CHAP v2)** seçili ve ardından **Tamam**.

    !["Bu protokollere izin ver" seçeneği](./media/howto-mfa-nps-extension-vpn/image20.png)

11. VPN bağlantısına sağ tıklayın ve ardından **Bağlan**.

12. İçinde **ayarları** penceresinde, seçin **Bağlan**.  
    Aşağıda gösterildiği gibi RADIUS sunucusu olarak olay kimliği 6272'in, güvenlik günlüğüne başarılı bir bağlantı görüntülenir:

    ![Olay Özellikler penceresi](./media/howto-mfa-nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>RADIUS sorunlarını giderme

VPN sunucusu kimlik doğrulama ve yetkilendirme için merkezi bir RADIUS sunucusu kullanmak üzere yapılandırılmış önce VPN yapılandırmanızı çalışır durumda olduğunu varsayalım. Yapılandırma çalışıyorsanız sorunu RADIUS sunucusu yanlış yapılandırılması veya geçersiz kullanıcı adı veya parola kullanımını kaynaklanır olasıdır. Örneğin, kullanıcı alternatif UPN soneki kullanırsanız, oturum açma girişimi başarısız olabilir. En iyi sonuçlar için aynı hesabı adı kullanın. 

Bu sorunları gidermek için başlatmak için bir ideal RADIUS sunucusunda güvenlik olay günlüklerini incelemek için yerdir. Zaman olayları arama kaydetmek için rol tabanlı ağ ilkesi ve erişim sunucusu özel görünüm Olay Görüntüleyicisi'nde, aşağıda gösterildiği gibi kullanabilirsiniz. "Olay kimliği 6273" nerede NPS bir kullanıcının erişimini reddetti olayları gösterir. 

![Olay Görüntüleyici](./media/howto-mfa-nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma

Kullanıcılar için çok faktörlü kimlik doğrulaması yapılandırma konusunda yardım almak için makalelerine bakın [bir kullanıcı veya grup için iki aşamalı doğrulama gerektirecek şekilde nasıl](howto-mfa-userstates.md) ve [Hesabımı iki aşamalı doğrulama için ayarlama](../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma

Bu bölümde VPN sunucusu ile istemci kimlik doğrulaması için MFA kullanmak için VPN yapılandırma için yönergeler sağlar.

Yükleyip NPS uzantısı yapılandırdıktan sonra bu sunucu tarafından işlenen tüm RADIUS tabanlı istemci kimlik doğrulaması MFA kullanmak için gereklidir. Tüm VPN kullanıcıları Azure çok faktörlü kimlik doğrulamasına kayıtlı olmayan istiyorsanız, aşağıdakilerden birini yapabilirsiniz:

* MFA kullanmak üzere yapılandırılmamış olan kullanıcıların kimliklerini doğrulamak için başka bir RADIUS sunucusu ayarlayın. 

* Azure çok faktörlü kimlik doğrulaması kaydedilirse, ikinci bir kimlik doğrulama faktörü sağlamak kullanıcıların kimlik doğrulaması sağlayan bir kayıt defteri girişi oluşturun. 

Adlı yeni bir dize değeri oluşturun _HKLM\SOFTWARE\Microsoft\AzureMfa REQUIRE_USER_MATCH_ve değeri *True* veya *False*. 

!["Kullanıcı eşleşmesi iste" ayarı](./media/howto-mfa-nps-extension-vpn/image34.png)
 
Değer ayarlanmışsa *doğru* veya tüm kimlik doğrulama isteklerini bir MFA sınaması tabi boştur. Değer ayarlanmışsa *yanlış*, MFA zorluklar yalnızca Azure çok faktörlü kimlik doğrulamasına kayıtlı olan kullanıcılara verilir. Kullanım *False* ekleme süresi boyunca yalnızca sınama veya üretim ortamları ayarlama.

### <a name="obtain-the-azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği elde

Yapılandırma NPS uzantısı'nın bir parçası olarak, yönetici kimlik bilgilerini ve Azure AD kiracınıza Kimliğini sağlamanız gerekir. Aşağıdakileri yaparak kimliği alın:

1. Oturum [Azure portal](https://portal.azure.com) Azure kiracının genel Yöneticisi olarak.

2. Sol bölmede seçin **Azure Active Directory** düğmesi.

3. Seçin **özellikleri**.

4. Azure AD Kimliğinizi kopyalamak için seçin **kopyalama** düğmesi.
 
    ![Azure AD kimliği](./media/howto-mfa-nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleyin
NPS uzantısı, Ağ İlkesi ve Erişim Hizmetleri rolü yüklü olan bir sunucuyu ve bu işlevleri tasarımınızda RADIUS sunucusu olarak yüklenmelidir. Yapmak *değil* NPS uzantısı, Uzak Masaüstü sunucusuna yükleyin.

1. NPS uzantısını [Microsoft Download Center](https://aka.ms/npsmfa). 

2. Kurulum yürütülebilir dosyasını kopyalayın (*NpsExtnForAzureMfaInstaller.exe*) NPS sunucusu için.

3. NPS sunucusunda, çift **NpsExtnForAzureMfaInstaller.exe** ve istenirse, seçin **çalıştırmak**.

4. İçinde **Azure MFA kurulumu için NPS uzantısı** penceresinde, gözden geçirme Yazılımı Lisans Koşulları'nı seçin **lisans şart ve koşullarını kabul ediyorum** onay kutusunu işaretleyin ve ardından **Yükleme**.

    !["NPS uzantısı için Azure MFA Kurulum" penceresi](./media/howto-mfa-nps-extension-vpn/image36.png)
 
5. İçinde **Azure MFA kurulumu için NPS uzantısı** penceresinde, seçin **Kapat**.  

    !["Kurulum başarılı" onay penceresi](./media/howto-mfa-nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>Bir PowerShell komut dosyası kullanarak NPS uzantısı ile sertifikalar kullanmak için yapılandırma
Güvenli iletişimler ve güvence emin olmak için NPS uzantısı tarafından kullanım için sertifikaları yapılandırın. NPS bileşenler, NPS ile otomatik olarak imzalanan bir sertifika kullanmak için yapılandırır bir Windows PowerShell komut dosyası içerir. 

Komut dosyası, aşağıdaki eylemleri gerçekleştirir:

* Otomatik olarak imzalanan bir sertifika oluşturur.
* Üzerinde Azure AD asıl hizmet sertifikasının ortak anahtarı ilişkilendirir.
* Sertifika yerel makine deposunda saklar.
* Sertifikanın özel anahtarı ağ kullanıcı erişimi verir.
* NPS hizmetini yeniden başlatır.

Kendi sertifikalarını kullanmak istiyorsanız, sertifikanın ortak anahtarının üzerinde Azure AD hizmet sorumlusu ile ilişkilendirmek ve benzeri.

Betik kullanmak için Azure Active Directory yönetici kimlik bilgilerinizi ve daha önce kopyaladığınız Azure Active Directory Kiracı kimliği ile uzantısı sağlar. Komut dosyası, NPS uzantısı yüklediğiniz her NPS sunucusunda çalıştırın.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. PowerShell komut isteminde girin **cd "c:\Program Files\Microsoft\AzureMfa\Config"** ve ardından Enter seçin.

3. Sonraki komut isteminde girin **.\AzureMfsNpsExtnConfigSetup.ps1**ve ardından Enter seçin. Azure AD PowerShell Modülü yüklü olup olmadığını görmek için komut dosyasını denetler. Yüklenmemişse, betik modülü yükler.
 
    ![PowerShell](./media/howto-mfa-nps-extension-vpn/image38.png)
 
    PowerShell modülü yükleme komut dosyası doğruladıktan sonra oturum açma Azure Active Directory PowerShell modülü penceresinde görüntüler. 

4. Azure AD yönetici kimlik bilgileri ve parolanızı girin ve ardından **oturum**. 
 
    ![Oturum açma PowerShell penceresi](./media/howto-mfa-nps-extension-vpn/image39.png)
 
5. Komut isteminde, daha önce kopyaladığınız Kiracı kimliği yapıştırın ve ardından Enter seçin. 

    ![Kiracı Kimliği](./media/howto-mfa-nps-extension-vpn/image40.png)

    Komut dosyası otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri yapar. Çıktısı gibi aşağıdaki görüntüde şöyledir:

    ![Otomatik olarak imzalanan sertifika](./media/howto-mfa-nps-extension-vpn/image41.png)

6. Sunucuyu yeniden başlatın.

### <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama
Yapılandırmasını doğrulamak için VPN sunucusu ile yeni bir VPN bağlantısı oluşturmanız gerekir. Birincil kimlik doğrulaması için kimlik bilgileriniz başarıyla girdikten sonra VPN bağlantısı ikincil kimlik doğrulama bağlantı kurulmadan önce aşağıda gösterildiği gibi başarılı olması bekler. 

![Windows ayarlarını VPN penceresi](./media/howto-mfa-nps-extension-vpn/image42.png)

Başarılı bir şekilde Azure MFA önceden yapılandırılmış ikincil doğrulama yöntemi, kimlik doğrulaması, kaynağa bağlanır. Ancak, ikincil kimlik doğrulama başarısız olursa, kaynak için erişim engellenir. 

Aşağıdaki örnekte, bir Windows Phone Microsoft Authenticator uygulamasını ikincil kimlik doğrulama sağlar:

![Kullanıcı hesabının doğrulanması](./media/howto-mfa-nps-extension-vpn/image43.png)

İkincil bir yöntem kullanarak başarıyla doğruladınız sonra VPN sunucuda sanal bağlantı noktası erişimi verilir. Güvenilen bir cihazda bir mobil uygulama kullanarak bir ikincil kimlik doğrulama yöntemi için gerekli için oturum açma işlemi yalnızca bir kullanıcı adı ve parola birleşimi kullanmakta olduğunuz varsa daha daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi'ni günlüklerini görüntüle
Başarılı oturum açma olaylarını Windows Olay Görüntüleyicisi günlüklerinde görüntülemek için sorgu Windows güvenliği oturum NPS sunucusunda aşağıdaki PowerShell komutunu girerek:

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL 

![PowerShell Güvenlik Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-vpn/image44.png)
 
Aşağıda gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünüm de görüntüleyebilirsiniz:

![Ağ ilkesi sunucusu günlük](./media/howto-mfa-nps-extension-vpn/image45.png)

Azure çok faktörlü kimlik doğrulaması için NPS uzantısı yüklendiği sunucuda uzantısına özgü Olay Görüntüleyicisi'ni uygulama günlüklerini bulabilirsiniz *uygulama ve Hizmetleri Logs\Microsoft\AzureMfa*. 

    _Get-WinEvent -Logname Security_ | where {$_.ID -eq '6272'} | FL

![Olay Görüntüleyicisi'ni "Olay sayısı" bölmesi](./media/howto-mfa-nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu
Yapılandırma beklendiği gibi çalışmıyor olursa, kullanıcı MFA kullanmak için yapılandırıldığını doğrulayarak sorun giderme başlar. Bağlanmak için kullanıcının sahip [Azure portal](https://portal.azure.com). Kullanıcı ikincil kimlik doğrulaması için istenir ve başarılı şekilde bağlantı kurup kimlik doğrulaması, bir sorun olarak mfa yapılandırması hatalı ortadan kaldırabilirsiniz.

MFA kullanıcı için çalışıyorsa, ilgili Olay Görüntüleyici günlüklerini gözden geçirin. Günlükler güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde tartışılan Azure multi-Factor Authentication günlükleri içerir. 

Bir başarısız oturum açma olayı (olay kimliği 6273) görüntüleyen bir güvenlik günlüğü örneği aşağıda gösterilmiştir:

![Başarısız oturum açma olayı gösteren güvenlik günlüğü](./media/howto-mfa-nps-extension-vpn/image47.png)

Azure multi-Factor Authentication günlüğünden ilgili olay burada gösterilir:

![Azure multi-Factor Authentication günlüğe kaydeder](./media/howto-mfa-nps-extension-vpn/image48.png)

Gelişmiş sorun giderme yapmak için NPS hizmetinin yüklendiği NPS veritabanı biçimi günlük dosyalarına bakın. Günlük dosyaları oluşturulur _%SystemRoot%\System32\Logs_ klasörü virgülle ayrılmış metin dosyalarını olarak. Günlük dosyaları açıklaması için bkz: [NPS veritabanı biçimi günlük dosyalarını yorumlama](https://technet.microsoft.com/library/cc771748.aspx). 

Bunları bir elektronik tablo veya bir veritabanı için dışarı aktarılmadığı sürece yorumlamak bu günlük dosyaları yer alan girişleri zordur. Birçok Internet Kimlik Doğrulama Hizmeti (günlük dosyalarını yorumlama yardımcı olmak için çevrimiçi araçları ayrıştırma IAS) bulabilirsiniz. Bir çıkış böyle indirilebilir [paylaşılan yazılım uygulama](http://www.deepsoftware.com/iasviewer) burada gösterilir: 

![Paylaşılan yazılım uygulama](./media/howto-mfa-nps-extension-vpn/image49.png)

Ek sorun giderme işlemleri yapmak için Wireshark gibi bir iletişim kuralı çözümleyicisi kullanabilir veya [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Wireshark aşağıdaki görüntüden VPN sunucusu ile NPS arasında RADIUS iletileri gösterir.

![Microsoft Message Analyzer](./media/howto-mfa-nps-extension-vpn/image50.png)

Daha fazla bilgi için bkz: [varolan NPS altyapınızı Azure çok faktörlü kimlik doğrulamasıyla tümleştirmek](howto-mfa-nps-extension.md). 

## <a name="next-steps"></a>Sonraki adımlar
[Azure çok faktörlü kimlik doğrulama Al](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)

