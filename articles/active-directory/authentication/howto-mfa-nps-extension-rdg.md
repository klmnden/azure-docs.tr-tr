---
title: Azure MFA NPS uzantısı ile Uzak Masaüstü Ağ Geçidi tümleştirme | Microsoft Docs
description: Bu makalede, Microsoft Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak Azure MFA ile Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirme anlatılmaktadır.
services: active-directory
keywords: Azure MFA, Uzak Masaüstü Ağ geçidi, Azure Active Directory, ağ ilkesi sunucusu uzantısı tümleştirme
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 0c050ee237650be7d43be2454a2bc3c07f096b8c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>Uzak Masaüstü Ağ Geçidi altyapınızı Azure AD ve ağ ilkesi sunucusu (NPS) uzantısını kullanarak tümleştirme

Bu makalede ayrıntıları Uzak Masaüstü Ağ Geçidi altyapınızı Azure çok faktörlü kimlik doğrulama (MFA) ile tümleştirmek için Microsoft Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak sağlar. 

Ağ İlkesi Sunucusu (NPS) uzantısı korumak müşterilerin Azure sağlayan için bulut tabanlı Azure kullanarak uzaktan kimlik doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) istemci kimlik doğrulaması [çok faktörlü kimlik doğrulama (MFA)](multi-factor-authentication.md). Bu çözüm, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı eklemek için iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısını kullanarak NPS altyapı Azure MFA ile tümleştirmek için adım adım yönergeler sağlar. Bu, Uzak Masaüstü Ağ geçidi için oturum açmayı deneyen kullanıcılar için güvenli doğrulaması sağlar. 

Ağ İlkesi ve Erişim Hizmetleri (NPS) imkanı kuruluşlar aşağıdakileri yapmak için:
* Yönetim Merkezi konumlarını ve denetim bağlanabilecek kişileri belirterek ağ isteklerinin gün bağlantıların ne zaman izin verilir, Bağlantılar'ın süresi ve istemcilerin bağlanmanız ve benzeri güvenlik düzeyini tanımlayın. Bu ilkelerin her VPN veya Uzak Masaüstü (RD) Ağ Geçidi sunucusunda belirtmek yerine, bu ilkeler, bir kez merkezi bir konumda belirtilebilir. RADIUS protokolü merkezi kimlik doğrulama, yetkilendirme ve hesap oluşturma (AAA) sağlar. 
* Kurmak ve cihazları ağ kaynaklarına Kısıtlanmamış veya kısıtlanmış erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri zorlayın.
* Kimlik doğrulama ve yetkilendirme access 802.1 zorlamak için sağladıkları x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.    

Genellikle, ilkeleri kuruluşlar kullanın ve VPN yönetimini merkezileştirme kolaylaştırmak için NPS (RADIUS). Bununla birlikte, çoğu kuruluş NPS basitleştirmek ve RD Masaüstü Bağlantı Yetkilendirme İlkeleri (RD CAP) yönetimini merkezileştirme için de kullanır. 

Kuruluşlar, ayrıca NPS güvenliğini artırmak ve yüksek bir uyumluluk düzeyini sağlamak için Azure MFA ile tümleştirebilirsiniz. Bu, kullanıcıların Uzak Masaüstü Ağ geçidi için oturum açmak için iki aşamalı doğrulamayı kurmak sağlamaya yardımcı olur. Kullanıcıların erişim verilmesi için kullanıcının kendi denetiminde sahip bilgilerle birlikte kendi kullanıcı adı/parola bileşimi sağlamanız gerekir. Bu bilgiler güvenilir ve kolayca çoğaltılması, cep telefonu numarası, telefona numarası, uygulama, bir mobil cihaz ve benzerleri gibi.

Azure NPS uzantısı kullanıma açılmadan önce yapılandırın ve şirket içi ortamda ayrı bir MFA sunucusu anlatıldığışekildekorumaktümleşikNPSveAzureMFAortamlariçinikiaşamalıdoğrulamayıuygulamakistediğinizdemüşterilervardı.[ Uzak Masaüstü Ağ geçidi ve Azure multi-Factor Authentication sunucusu RADIUS kullanan](howto-mfaserver-nps-rdg.md).

Azure NPS uzantısı kullanılabilirliğini şimdi kuruluşlar için güvenli RADIUS istemci kimlik doğrulaması şirket tabanlı MFA çözümünü veya bulut tabanlı bir MFA çözümü dağıtmak için seçmenizi sağlar.

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kullanıcıların Uzak Masaüstü Ağ geçidi üzerinden ağ kaynaklarına erişim verilmesi için bunlar bir RD Bağlantı Yetkilendirme İlkesi (RD CAP) ve bir RD kaynak yetkilendirme ilkesi (RD RAP) belirtilen koşullara uyması gerekir. RD CAP kimin için RD Ağ Geçidi bağlanmak için yetkili belirtin. RD RAP Uzak Masaüstü veya kullanıcı RD Ağ geçidi üzerinden bağlanmasına izin uzak uygulamalar gibi ağ kaynaklarına belirtin. 

RD Ağ geçidi için RD CAP merkezi İlkesi depoyu kullanacak şekilde yapılandırılabilir. RD Ağ Geçidi üzerinde işlenene olarak RD RAP bir merkezi ilke kullanamazsınız. Bir merkezi ilke deposu olarak hizmet veren başka bir NPS sunucusunun RADIUS istemcisi için RD CAP merkezi İlkesi depoyu kullanacak şekilde yapılandırılmış bir RD Ağ Geçidi örnektir.

Azure NPS uzantısı NPS ve Uzak Masaüstü Ağ geçidi ile tümleştirildiğinde, başarılı kimlik doğrulaması akışı aşağıdaki gibidir:

1. Uzak Masaüstü Ağ Geçidi sunucusu Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için Uzak Masaüstü kullanıcı kimlik doğrulama isteği alır. Bir RADIUS istemcisi olarak işlev gören, Uzak Masaüstü Ağ Geçidi sunucusu, RADIUS Erişim-İstek iletisi için istek dönüştürür ve NPS uzantısı yüklendiği RADIUS (NPS) sunucusuna ileti gönderir. 
2. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır ve kullanıcının kimliği doğrulanır.
3. NPS bağlantı isteği ve ağ ilkeleri belirtildiği gibi tüm koşullar karşılanıyorsa (örneğin, süresi gün veya grup üyeliği kısıtlamaları), NPS uzantısı Azure MFA ile ikincil kimlik doğrulama isteği tetikler. 
4. Azure MFA, Azure AD ile iletişim kurar, kullanıcının ayrıntılarını alır ve (SMS mesajı, mobil uygulama ve benzeri) kullanıcı tarafından yapılandırılan yöntemini kullanarak ikincil kimlik doğrulaması gerçekleştirir. 
5. MFA testini Başarı sonucu NPS uzantısı sonucu Azure MFA ile iletişim kurar.
6. Uzantı yüklendiği NPS sunucusunun RADIUS Erişim Kabul iletisi RD CAP ilkesi için Uzak Masaüstü Ağ Geçidi sunucusuna gönderir.
7. Kullanıcının istenen ağ kaynağına RD Ağ Geçidi aracılığıyla erişim izni verilir.

## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, Uzak Masaüstü Ağ geçidi ile Azure MFA tümleştirme önce gerekli önkoşulları ayrıntıları verilmektedir. Başlamadan önce aşağıdaki önkoşulların yerinde olması gerekir.  

* Uzak Masaüstü Hizmetleri (RDS) altyapısı
* Azure MFA lisans
* Windows Server yazılımı
* Ağ İlkesi ve Erişim Hizmetleri (NPS) rol
* Şirket içi Active Directory ile Azure Active Directory synched
* Azure Active Directory GUID kimliği

### <a name="remote-desktop-services-rds-infrastructure"></a>Uzak Masaüstü Hizmetleri (RDS) altyapısı
Çalışan bir Uzak Masaüstü Hizmetleri (RDS) altyapısının yerinde olması gerekir. Bunu yapmanız durumunda aşağıdaki Hızlı Başlangıç şablonu kullanarak Azure'da hızlı bir şekilde bu altyapı oluşturabilirsiniz: [Uzak Masaüstü Oturum Koleksiyonu Oluştur dağıtım](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

El ile test etmek için hızlı bir şekilde bir şirket içi RDS altyapı oluşturmak istiyorsanız, bir dağıtmak için adımları izleyin. 
**Daha fazla bilgi edinin**: [dağıtmak RDS Azure Hızlı Başlangıç ile](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) ve [temel RDS altyapı dağıtımı](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="azure-mfa-license"></a>Azure MFA lisans
Gerekli bir Azure AD Premium, Enterprise Mobility artı güvenlik (EMS) veya bir MFA abonelik kullanılabilir olan Azure MFA bir lisans kullanılır. Tüketim tabanlı lisansları gibi kullanıcı başına veya başına kimlik doğrulama lisansı, Azure MFA için NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulama alma](concept-mfa-licensing.md). Test amacıyla bir deneme aboneliği kullanabilirsiniz. 

### <a name="windows-server-software"></a>Windows Server yazılımı
Windows Server 2008 R2 SP1 NPS uzantısı gerektirir veya üstü yüklü NPS rol hizmetine sahip. Bu bölümdeki tüm adımlar, Windows Server 2016 kullanılarak gerçekleştirilen.

### <a name="network-policy-and-access-services-nps-role"></a>Ağ İlkesi ve Erişim Hizmetleri (NPS) rol
NPS rol hizmetinin işlevselliğinin yanı sıra ağ erişim ilkesi sistem sağlığı hizmeti RADIUS sunucusu ve istemci sağlar. Bu rol altyapınızda en az iki bilgisayara yüklenmesi gerekir: Uzak Masaüstü Ağ geçidi ve başka bir üye sunucu veya etki alanı denetleyicisi. Varsayılan olarak, zaten bilgisayarda Uzak Masaüstü Ağ geçidi olarak yapılandırılmış rolüdür.  Ayrıca NPS rolü üzerinde en az bir etki alanı denetleyicisi veya üye sunucu gibi başka bir bilgisayara yüklemeniz gerekir.

NPS rolü yükleme hakkında bilgi için Windows Server 2012 veya daha eski hizmet için bkz: [bir NAP sistem durumu ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). Bir etki alanı denetleyicisinde NPS yüklemek için öneri dahil olmak üzere NPS için en iyi uygulamaları bir açıklaması için bkz: [NPS için en iyi uygulamaları](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Şirket içi Active Directory ile Azure Active Directory synched
NPS uzantısını kullanmak için şirket içi kullanıcıları Azure AD ile eşitlenen ve MFA için etkinleştirilmiş olmalıdır. Bu bölümde, şirket içi kullanıcıların AD Connect kullanan Azure AD ile eşitlenir varsayar. Azure AD hakkında bilgi için bağlanmak, bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği
NPS uzantıyı yüklemek için Azure AD GUID bilmeniz gerekir. Azure AD GUID bulmak için yönergeler aşağıda verilmiştir.

## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma 
Bu bölümde Azure MFA Uzak Masaüstü Ağ geçidi ile tümleştirmek için yönergeler sağlar. Yönetici olarak, kullanıcılar, çok faktörlü aygıtları veya uygulamaları kendi kendine kaydedebilmek için Azure MFA hizmetini yapılandırmanız gerekir.

Adımları [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](howto-mfa-getstarted.md) Azure AD kullanıcılarınız için MFA'yı etkinleştirmek için. 

### <a name="configure-accounts-for-two-step-verification"></a>İki aşamalı doğrulama için hesaplarını yapılandırma
Bir hesap MFA için etkinleştirildikten sonra ikinci kimlik doğrulama faktörü için kullanın ve iki aşamalı doğrulama kullanarak kimlik doğrulaması için güvenilir bir aygıt başarılı bir şekilde yapılandırmadığınız sürece MFA İlkesi tarafından yönetilen kaynaklara oturum açamaz.

Adımları [ne Azure çok faktörlü kimlik doğrulaması için beni anlama geliyor?](./../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md) anlamak ve aygıtlarınızın MFA için kullanıcı hesabınızla düzgün bir şekilde yapılandırmak için.

## <a name="install-and-configure-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma
Bu bölüm, Uzak Masaüstü Ağ Geçidi istemci kimlik doğrulaması için Azure MFA kullanmaya RDS altyapısını yapılandırmaya yönelik yönergeler sağlar.

### <a name="acquire-azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği alma

Yapılandırma NPS uzantısı'nın bir parçası olarak, Azure AD kiracınız için yönetici kimlik bilgileri ve Azure AD kimliği sağlamanız gerekir. Aşağıdaki adımlar Kiracı kimliğini alma gösterir

1. Oturum [Azure portal](https://portal.azure.com) Azure kiracının genel Yöneticisi olarak.
2. Sol gezinti bölmesinde seçin **Azure Active Directory** simgesi.
3. Seçin **özellikleri**.
4. Özellikleri dikey penceresinde, dizin kimliği yanında tıklatın **kopyalama** kimliği panoya kopyalamak için aşağıda gösterildiği gibi simgesi.

 ![Özellikler](./media/howto-mfa-nps-extension-rdg/image1.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleyin
NPS uzantısı, Ağ İlkesi ve Erişim Hizmetleri (NPS) rolü yüklü bir sunucuya yükleyin. Bu, RADIUS sunucusu tasarımınızın görür. 

>[!Important]
> Uzak Masaüstü Ağ Geçidi sunucunuzda NPS uzantısı yüklemeyin emin olun.
> 

1. Karşıdan [NPS uzantısı](https://aka.ms/npsmfa). 
2. Kurulum yürütülebilir dosya (NpsExtnForAzureMfaInstaller.exe) NPS sunucusuna kopyalayın.
3. NPS sunucusunda, çift **NpsExtnForAzureMfaInstaller.exe**. İstenirse, tıklatın **çalıştırmak**.
4. Azure MFA kurulumu için NPS uzantısı iletişim kutusunda Yazılımı Lisans Koşulları'nı gözden denetleyin **lisans şart ve koşullarını kabul ediyorum**, tıklatıp **yükleme**.
 
  ![Azure MFA Kurulumu](./media/howto-mfa-nps-extension-rdg/image2.png)

5. Azure MFA kurulumu için NPS uzantısı iletişim kutusunda tıklatın **Kapat**. 

  ![Azure MFA için NPS uzantısı](./media/howto-mfa-nps-extension-rdg/image3.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Sertifikaları kullanmak için bir PowerShell Betiği kullanılarak NPS uzantısı ile yapılandırma
Ardından, güvenli iletişimler ve güvence emin olmak için NPS uzantısı tarafından sertifikaları kullanmak için yapılandırmanız gerekir. NPS bileşenler, NPS ile otomatik olarak imzalanan bir sertifika kullanmak için yapılandırır bir Windows PowerShell komut dosyası içerir. 

Komut dosyası, aşağıdaki eylemleri gerçekleştirir:

* Kendinden imzalı bir sertifika oluşturur
* Hizmet üzerinde Azure AD sorumlusu sertifikasının ortak anahtarı ilişkilendirir
* Sertifika yerel makine deposunda saklar
* Sertifikanın özel anahtarı ağ kullanıcı erişimi verir
* Ağ İlkesi Sunucusu hizmetini yeniden başlatır

Kendi sertifikalarını kullanmak istiyorsanız, üzerinde Azure AD hizmet sorumlusu için sertifikanın ortak anahtarının ilişkilendirmek ve benzeri gerekir.

Betik kullanmak için Azure AD yönetici kimlik bilgileriniz ve daha önce kopyaladığınız Azure AD Kiracı kimliği ile uzantısı sağlar. Komut dosyası, NPS uzantısı yüklü olduğu her NPS sunucusunda çalıştırın. Ardından şunları yapın:

1. Bir yönetici Windows PowerShell komut istemini açın.
2. PowerShell komut isteminde yazın **cd 'c:\Program Files\Microsoft\AzureMfa\Config'** ve basın **ENTER**.
3. Tür _.\AzureMfsNpsExtnConfigSetup.ps1_ve basın **ENTER**. Azure Active Directory PowerShell Modülü yüklü olup olmadığını görmek için komut dosyasını denetler. Yüklü değilse, komut dosyası modülünü yükler.

  ![Azure AD PowerShell](./media/howto-mfa-nps-extension-rdg/image4.png)
  
4. PowerShell modülünü yükleme komut dosyası doğruladıktan sonra Azure Active Directory PowerShell modülü iletişim kutusunu görüntüler. İletişim kutusunda, Azure AD yönetim kimlik bilgilerini ve parolanızı girin ve tıklatın **oturum**.

  ![PowerShell hesap açın](./media/howto-mfa-nps-extension-rdg/image5.png)

5. İstendiğinde, kopyaladığınız panoya önceki Kiracı kimliği yapıştırın ve tuşuna basın **ENTER**.

  ![Kiracı kimliği girin](./media/howto-mfa-nps-extension-rdg/image6.png)

6. Komut dosyası otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri yapar. Çıktı görüntünün aşağıda gösterildiği gibi olması gerekir.

  ![Otomatik olarak imzalanan sertifika](./media/howto-mfa-nps-extension-rdg/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidi üzerinde NPS bileşenlerini yapılandırma
Bu bölümde, Uzak Masaüstü Ağ Geçidi bağlantısı Yetkilendirme İlkeleri ve diğer RADIUS ayarlarını yapılandırın.

Kimlik doğrulaması akışı RADIUS iletileri Uzak Masaüstü Ağ geçidi ve NPS sunucusunun yüklendiği NPS sunucusu arasında alınıp gerektirir. Bu, hem Uzak Masaüstü Ağ Geçidi hem de NPS uzantısı yüklendiği NPS sunucusunun RADIUS istemci ayarları yapılandırmalısınız anlamına gelir. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Uzak Masaüstü Ağ Geçidi bağlantı yetkilendirme ilkelerini merkezi deposu kullanmak için yapılandırma
Uzak Masaüstü Bağlantısı Yetkilendirme İlkeleri (RD CAP) bir Uzak Masaüstü Ağ Geçidi sunucusuna bağlanmak için koşulları belirtin. RD CAP yerel olarak depolanabilir (varsayılan) veya bunlar depolanabilir NPS çalıştıran merkezi bir RD CAP Deposu içinde. RDS ile Azure mfa tümleştirmesini yapılandırmak için merkezi bir depo kullanımını belirtmeniz gerekir.

1. RD Ağ Geçidi sunucusunda açın **Sunucu Yöneticisi'ni**. 
2. Menüsünde **Araçları**, işaret **Uzak Masaüstü Hizmetleri**ve ardından **Uzak Masaüstü Ağ Geçidi Yöneticisi**.

  ![Uzak Masaüstü Hizmetleri](./media/howto-mfa-nps-extension-rdg/image8.png)

3. RD Ağ Geçidi Yöneticisi sağ  **\[sunucu adı\] (yerel)**, tıklatıp **özellikleri**.

  ![Sunucu Adı](./media/howto-mfa-nps-extension-rdg/image9.png)

4. Özellikler iletişim kutusunda seçin **RD CAP Deposu** sekmesi.
5. RD CAP Deposu sekmesinde seçin **merkezi NPS çalıştıran sunucuyu**. 
6. İçinde **NPS çalıştıran sunucu için bir ad veya IP adresini girin** alanında, NPS uzantısı yüklendiği sunucunun IP adresini veya sunucu adını yazın.

  ![Adı veya IP adresini girin](./media/howto-mfa-nps-extension-rdg/image10.png)
  
7. **Ekle**'ye tıklayın.
8. İçinde **paylaşılan gizliliği** iletişim kutusu, bir paylaşılan gizlilik girin ve ardından **Tamam**. Bu paylaşılan gizliliği kaydedin ve kaydı güvenli bir şekilde depoladığınızdan emin olun.

 >[!NOTE]
 >Paylaşılan gizliliği RADIUS sunucuları ve istemciler arasında güven oluşturmak için kullanılır. Uzun ve karmaşık bir parola oluşturun.
 >

 ![Paylaşılan parola](./media/howto-mfa-nps-extension-rdg/image11.png)

9. **Tamam**’a tıklayarak iletişim kutusunu kapatın.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Uzak Masaüstü Ağ Geçidi NPS üzerinden RADIUS zaman aşımı değerini yapılandırın
Kullanıcıların kimlik bilgilerini doğrulamak için süresi emin olmak için iki aşamalı doğrulamayı gerçekleştirmek, yanıtları almak ve yanıt RADIUS iletilerini, RADIUS zaman aşımı değeri ayarlamak gereklidir.

1. RD Ağ Geçidi sunucusunda Sunucu Yöneticisi'ni açın. Menüsünde **Araçları**ve ardından **ağ ilkesi sunucusu**. 
2. İçinde **NPS (yerel)** genişletin **RADIUS istemcileri ve sunucuları**seçip **uzak RADIUS sunucu**.

 ![Uzak RADIUS sunucusu](./media/howto-mfa-nps-extension-rdg/image12.png)

3. Ayrıntılar bölmesinde, çift **TH Ağ Geçidi sunucusu GRUBUNU**.

 >[!NOTE]
 >Merkezi sunucu NPS ilkeleri için yapılandırıldığında bu RADIUS sunucu grubu oluşturuldu. RD Ağ Geçidi grubundaki olandan daha fazla olması durumunda bu sunucu veya sunucu grubu için RADIUS iletileri iletir.
 >

4. İçinde **TH Ağ Geçidi sunucu grubu özellikleri** iletişim kutusunda, IP adresi veya RD CAP depolamak ve ardından yapılandırılmış NPS sunucusunun adını seçin **Düzenle**. 

 ![TH Ağ Geçidi sunucusu grubu](./media/howto-mfa-nps-extension-rdg/image13.png)

5. İçinde **RADIUS sunucusu Düzenle** iletişim kutusunda **Yük Dengeleme** sekmesi.
6. İçinde **Yük Dengeleme** sekmesinde **bırakılan isteği kabul edilmeden önce yanıt olmadan saniye sayısı** alanında, varsayılan değer 30 ila 60 saniye arasında bir değer için 3 ile değiştirin.
7. İçinde **sunucu olarak kullanılamaz olduğu belirlendiğinde isteklerinin arasındaki saniye sayısını** alan, eşit veya önceki adımda belirtilen değerden daha büyük bir değere 30 saniye varsayılan değerini değiştirin.

 ![RADIUS sunucusu Düzenle](./media/howto-mfa-nps-extension-rdg/image14.png)

8.  Tıklatın **Tamam** iki kez iletişim kutularını kapatın.

### <a name="verify-connection-request-policies"></a>Bağlantı isteği ilkeleri doğrulayın 
Bir merkezi ilke deposu Bağlantı Yetkilendirme İlkeleri için kullanılacak RD Ağ Geçidi yapılandırdığınızda varsayılan olarak, NPS sunucusunu CAP isteklerini iletmek için RD Ağ Geçidi yapılandırılır. Yüklü, Azure MFA uzantılı NPS sunucusunun RADIUS erişim isteğini işler. Aşağıdaki adımlar, varsayılan bağlantı isteği ilkesi nasıl gösterir. 

1. NPS (yerel) konsolunda, RD Ağ Geçidi'ni genişletin **ilkeleri**seçip **bağlantı isteği ilkeleri**.
2. Sağ **bağlantı isteği ilkeleri**, çift tıklayın ve **TH Ağ Geçidi YETKİLENDİRME İlkesi**.
3. İçinde **TH Ağ Geçidi YETKİLENDİRME İlkesi Özellikleri** iletişim kutusu, tıklatın **ayarları** sekmesi.
4. Üzerinde **ayarları** iletme bağlantı isteği altında sekmesini **kimlik doğrulama**. RADIUS istemcisi, kimlik doğrulama istekleri iletmek için yapılandırılır.

 ![Kimlik doğrulama ayarları](./media/howto-mfa-nps-extension-rdg/image15.png)
 
5. Tıklatın **iptal**. 

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>NPS uzantısı yüklü olduğu sunucuda NPS yapılandırma
NPS uzantısı yüklendiği NPS sunucusunun RADIUS iletileri ile Uzak Masaüstü Ağ Geçidi NPS sunucusunda exchange mümkün olması gerekir. Bu ileti değiş tokuş etkinleştirmek için NPS uzantısı hizmetinin yüklendiği sunucuda NPS bileşenleri yapılandırmanız gerekir. 

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir
Bu senaryoda düzgün çalışması için NPS sunucusunun Active Directory'de kayıtlı olması gerekir.

1. NPS sunucusunda açmak **Sunucu Yöneticisi'ni**.
2. Sunucu Yöneticisi'nde **Araçları**ve ardından **ağ ilkesi sunucusu**. 
3. Ağ İlkesi Sunucusu konsolunda sağ **NPS (yerel)** ve ardından **Active Directory'de kayıt sunucusu**. 
4. Tıklatın **Tamam** iki kez.

 ![AD kayıt sunucu](./media/howto-mfa-nps-extension-rdg/image16.png)

5. Konsolunu sonraki yordam için açık bırakın.

### <a name="create-and-configure-radius-client"></a>Oluşturma ve RADIUS istemci yapılandırma 
Uzak Masaüstü Ağ geçidi, NPS sunucusunun RADIUS istemcisi olarak yapılandırılması gerekiyor. 

1. NPS uzantısı yüklü olduğu, buna NPS sunucusunda **NPS (yerel)** konsolunda, sağ **RADIUS istemcileri** tıklatıp **yeni**.

 ![Yeni RADIUS istemcisi](./media/howto-mfa-nps-extension-rdg/image17.png)

2. İçinde **yeni RADIUS istemcisi** iletişim kutusunda, aşağıdaki gibi kolay bir ad sağlayın _ağ geçidi_ve IP adresi ya da Uzak Masaüstü Ağ Geçidi sunucusunun DNS adı. 
3. İçinde **paylaşılan gizlilik** ve **paylaşılan gizliliği onayla** alanları, önce kullandığınız aynı parolayı girin.

 ![Ad ve adres](./media/howto-mfa-nps-extension-rdg/image18.png)

4. Tıklatın **Tamam** yeni RADIUS istemcisi iletişim kutusunu kapatın.

### <a name="configure-network-policy"></a>Ağ İlkesi yapılandırma
Azure MFA uzantılı NPS sunucusu bağlantı yetkilendirme ilkesi (CAP) için atanmış merkezi ilke deposu olduğunu hatırlayın. Bu nedenle, geçerli bağlantı isteklerini yetkilendirmek için NPS sunucusu bir sınır uygulamak için gerekir.  

1. NPS (yerel) konsolda **ilkeleri**, tıklatıp **ağ ilkeleri**.
2. Sağ **diğer erişim sunucularına yapılan bağlantılar**, tıklatıp **Çoğaltma İlkesi**. 

 ![Yinelenen İlkesi](./media/howto-mfa-nps-extension-rdg/image19.png)

3. Sağ **diğer erişim sunucularına yapılan bağlantılar kopyalama**, tıklatıp **özellikleri**.

 ![Ağ Özellikleri](./media/howto-mfa-nps-extension-rdg/image20.png)

4. İçinde **diğer erişim sunucularına yapılan bağlantılar kopyalama** iletişim kutusunda **ilke adı**, uygun bir ad girin _RDG_CAP_. Denetleme **etkin ilke**seçip **erişim izni verme**. İsteğe bağlı olarak **ağ erişim sunucusu türü**seçin **Uzak Masaüstü Ağ Geçidi**, veya olarak bırakılabilir **belirtilmemiş**.

 ![Bağlantıları kopyası](./media/howto-mfa-nps-extension-rdg/image21.png)

5.  Tıklatın **kısıtlamaları** sekmesini tıklatın ve denetleme **istemcilerin bir kimlik doğrulama yöntemi görüşmesi olmadan bağlanmasına izin ver**.

 ![İstemcilerin bağlanmasına izin ver](./media/howto-mfa-nps-extension-rdg/image22.png)

6. İsteğe bağlı olarak, tıklayın **koşullar** sekmesinde ve bağlantının, örneğin, belirli bir Windows grubu üyeliği yetki verilmesi karşılanması gereken koşulları ekleyin.

 ![Koşullar](./media/howto-mfa-nps-extension-rdg/image23.png)

7. **Tamam**’a tıklayın. İlgili Yardım konusunu görüntülemek isteyip istemediğiniz sorulduğunda tıklatın **Hayır**.
8. Yeni ilke ilkesi etkinleştirilmiş listenin başında olduğundan ve erişim verir emin olun.

 ![Ağ ilkeleri](./media/howto-mfa-nps-extension-rdg/image24.png)

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Yapılandırmayı doğrulamak için Uzak Masaüstü Ağ geçidi için uygun bir RDP istemci ile oturum açmanız gerekir. Bağlantı Yetkilendirme İlkeleri tarafından izin verilen ve Azure MFA için etkinleştirilmiş bir hesap kullandığınızdan emin olun. 

Aşağıdaki görüntüde Göster kullanabileceğiniz **Uzak Masaüstü Web erişimi** sayfası.

![Uzak Masaüstü Web erişimi](./media/howto-mfa-nps-extension-rdg/image25.png)

Kimlik bilgilerinizi birincil kimlik doğrulaması için başarıyla girildikten sonra Uzak Masaüstü Bağlantısı iletişim kutusu uzaktan bağlantıyı başlatan bir durumu aşağıda gösterildiği gibi gösterir. 

Başarılı bir şekilde Azure MFA önceden yapılandırılmış ikincil kimlik doğrulama yöntemi, kimlik doğrulaması, kaynağa bağlanır. Ancak, ikincil kimlik doğrulaması başarılı olmazsa, kaynak için erişim engellenir. 

![Uzak bağlantı başlatma](./media/howto-mfa-nps-extension-rdg/image26.png)

Aşağıdaki örnekte, bir Windows Phone Doğrulayıcı uygulama ikincil kimlik doğrulaması sağlamak için kullanılır.

![Hesaplar](./media/howto-mfa-nps-extension-rdg/image27.png)

İkincil kimlik doğrulama yöntemini kullanarak başarıyla doğrulandıktan sonra normal olarak Uzak Masaüstü Ağ Geçidi içine kaydedilir. Güvenilen bir cihazda bir mobil uygulama kullanarak bir ikincil kimlik doğrulama yöntemini kullanmak için gerekli olduğundan, ancak oturum açma işleminiz Aksi durumda olacak çok daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi'ni günlüklerini görüntüle
Başarılı oturum açma olaylarını Windows Olay Görüntüleyicisi günlüklerinde görüntülemek için Windows Terminal Hizmetleri ve Windows güvenlik günlüklerini sorgulamak için aşağıdaki Windows PowerShell komutu gönderebilirsiniz.

Ağ geçidi işletimsel günlüklerine başarılı oturum açma olayları sorgulamak için _(olay görüntüleyici\uygulamalar ve hizmetler Logs\Microsoft\Windows\TerminalServices-Gateway\Operational)_, aşağıdaki PowerShell komutlarını kullanın:

* _Get-WinEvent - günlükadı Microsoft-Windows-TerminalServices-ağ geçidi/Operational_ | burada {$_.ID - eq '300'} | FL 
* Bu komut, kullanıcının kaynak yetkilendirme ilkesi gereksinimleri (RD RAP) sınamadan ve erişim verildi Göster Windows olayları görüntüler.

![Görünüm Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-rdg/image28.png)

* _Get-WinEvent - günlükadı Microsoft-Windows-TerminalServices-ağ geçidi/Operational_ | burada {$_.ID - eq '200'} | FL
* Bu komut, kullanıcının bağlantı yetkilendirme ilkesi gereksinimlerini sağlandığında göstermek olayları görüntüler.

![Bağlantı Yetkilendirme](./media/howto-mfa-nps-extension-rdg/image29.png)

Bu günlük ve filtre olay kimlikleri, 300 ve 200 üzerinde de görüntüleyebilirsiniz. Başarılı oturum açma olaylarını Güvenlik Olay Görüntüleyicisi günlüklerinde sorgulamak için aşağıdaki komutu kullanın:

* _Get-WinEvent - günlükadı güvenlik_ | burada {$_.ID - eq '6272'in '} | FL 
* Bu komut, merkezi NPS veya RD Ağ Geçidi sunucusu üzerinde çalıştırılabilir. 

![Başarılı oturum açma olayları](./media/howto-mfa-nps-extension-rdg/image30.png)

Aşağıda gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünüm de görüntüleyebilirsiniz:

![Ağ İlkesi ve Erişim Hizmetleri](./media/howto-mfa-nps-extension-rdg/image31.png)

Azure MFA için NPS uzantısı yüklendiği sunucuda Olay Görüntüleyicisi'ni uygulama günlüklerini uzantısına özgü bulabileceğiniz _uygulama ve Hizmetleri Logs\Microsoft\AzureMfa_. 

![Olay Görüntüleyicisi'ni uygulama günlükleri](./media/howto-mfa-nps-extension-rdg/image32.png)

## <a name="troubleshoot-guide"></a>Sorun giderme kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, sorun giderme başlatmak için ilk kullanıcı Azure MFA kullanmak için yapılandırıldığını doğrulamak için yerdir. Bağlanmak için kullanıcının sahip [Azure portal](https://portal.azure.com). Kullanıcıların başarıyla ikincil doğrulama için istenir ve kimlik doğrulaması, Azure mfa yapılandırması hatalı ortadan kaldırabilirsiniz.

Azure MFA için kullanıcıları çalışıyorsa, ilgili olay günlüklerini gözden geçirmelidir. Bunlar, güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde tartışılan Azure MFA günlükleri içerir. 

Aşağıda, güvenlik günlüğü başarısız oturum açma olayı (olay kimliği 6273) gösteren bir örnek çıktı verilmiştir.

![Başarısız oturum açma olayı](./media/howto-mfa-nps-extension-rdg/image33.png)

İlgili olay AzureMFA günlüklerinden aşağıdadır:

![Azure MFA günlük](./media/howto-mfa-nps-extension-rdg/image34.png)

Gerçekleştirmek için Gelişmiş Seçenekleri sorun giderme, NPS hizmetinin yüklü olduğu NPS veritabanı biçimi günlük dosyalarına bakın. Bu günlük dosyaları oluşturulur _%SystemRoot%\System32\Logs_ klasörü virgülle ayrılmış metin dosyalarını olarak. 

Bu bir açıklaması için günlük dosyaları, bkz: [NPS veritabanı biçimi günlük dosyalarını yorumlama](https://technet.microsoft.com/library/cc771748.aspx). Bu günlük dosyaları yer alan girişleri bir elektronik tablo veya bir veritabanına almadan yorumlamaya zor olabilir. Günlük dosyaları yorumlama yardımcı olmak için çevrimiçi çeşitli IAS ayrıştırıcıları bulabilirsiniz. 

Aşağıdaki resimde bir çıktısını gösterir böyle indirilebilir [paylaşılan yazılım uygulama](http://www.deepsoftware.com/iasviewer). 

![Paylaşılan yazılım uygulama](./media/howto-mfa-nps-extension-rdg/image35.png)

Son olarak, için ek seçenekleri sorun giderme, bir iletişim kuralı çözümleyicisi kullanabilir böyle [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

Microsoft Message Analyzer aşağıda görüntüden kullanıcı adını içeren RADIUS protokolü filtre ağ trafiğini gösterir **Contoso\AliceC**.

![Microsoft Message Analyzer](./media/howto-mfa-nps-extension-rdg/image36.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Multi-Factor Authentication’ı edinme](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)
