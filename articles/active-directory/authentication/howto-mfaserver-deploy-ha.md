---
title: Yüksek kullanılabilirlik için - Azure Active Directory'de Azure MFA sunucusu yapılandırma
description: Azure multi-Factor Authentication sunucusu yapılandırmaları yüksek kullanılabilirlik sağlayan birden çok örneğini dağıtın.
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
ms.openlocfilehash: 2ddf0885ce7615e06b78eccbd6424e63cc6103c2
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547014"
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Azure multi-Factor Authentication sunucusu için yüksek kullanılabilirliği yapılandırma

Azure sunucusu MFA dağıtımınız ile yüksek kullanılabilirlik elde etmek için birden çok MFA sunucusu dağıtmanız gerekir. Bu bölümde, Azure MFS sunucu dağıtımı, yüksek kullanılabilirlik hedeflerin elde etmek için yük dengeli bir tasarım bilgileri sağlar.

## <a name="mfa-server-overview"></a>MFA sunucusu genel bakış

Azure MFA sunucusu hizmet mimarisi, aşağıdaki diyagramda gösterildiği gibi birçok bileşenden oluşur:

 ![MFA sunucusu mimari bileşenler](./media/howto-mfaserver-deploy-ha/mfa-ha-architecture.png)

MFA sunucusu yüklü olan Azure multi-Factor Authentication yazılım bir Windows Server ' dir. MFA sunucusu örneğinin çalışması için Azure MFA hizmeti tarafından etkinleştirilmelidir. Birden fazla MFA sunucusu şirket yüklü olabilir.

Yüklenen ilk MFA sunucusu MFA sunucusu tarafından varsayılan olarak Azure MFA hizmetini etkinleştirme sırasında yöneticisidir. Ana MFA sunucusu PhoneFactor.pfdata veritabanı yazılabilir bir kopyasına sahip olur. MFA sunucusu örneğini izleyen yüklemelerde Astları bilinir. MFA Astları çoğaltılmış salt okunur veritabanının bir kopyasını PhoneFactor.pfdata vardır. MFA sunucularını uzaktan yordam çağrısı (RPC) kullanarak bilgileri çoğaltın. Tüm MFA sunucularının topluca ya da etki alanına katılmış veya bilgi çoğaltmak için bağımsız olması gerekir.

İki öğeli kimlik doğrulaması gerekli olduğunda hem MFA ana şablonunun hem de bağımlı MFA sunucularını MFA hizmeti ile iletişim kurar. Örneğin, bir kullanıcı iki öğeli kimlik doğrulama gerektiren bir uygulamaya erişim kazanmak çalıştığında, kullanıcının ilk gibi Active Directory (AD) bir kimlik sağlayıcısı tarafından doğrulanır.

AD ile başarılı kimlik doğrulamasından sonra MFA sunucusu MFA hizmeti ile iletişim kurar. MFA sunucusu veya uygulama için kullanıcı erişimini engellediği için mfa'yı hizmetinden bildirim bekler.

MFA ana sunucu çevrimdışı olursa, kimlik doğrulamaları işlenmeye devam ediyor ancak MFA veritabanında değişiklikler yapılmasını gerektiren işlemler işlenemiyor. (Örnekler: kullanıcıların, Self Servis PIN değişiklikleri, değişen kullanıcı bilgilerini veya kullanıcı portalına erişim eklenmesini)

## <a name="deployment"></a>Dağıtım

Azure MFA sunucusu ve ilgili bileşenlerini Yük Dengeleme için aşağıdaki önemli noktalara dikkat edin.

* **Yüksek kullanılabilirlik elde etmek için RADIUS standardını kullanarak**. RADIUS sunucuları olarak Azure MFA sunucusu kullanıyorsanız, birincil RADIUS kimlik doğrulaması hedefi ve diğer Azure MFA sunucusu ikincil kimlik doğrulaması hedefleri olarak olası bir MFA sunucusu yapılandırabilirsiniz. Ancak, ikincil kimlik doğrulaması hedefe karşı doğrulanabilmesinden önce birincil kimlik doğrulaması hedef kimlik doğrulama başarısız olduğunda gerçekleşmesi bir zaman aşımı süresi için beklemeniz gerekir çünkü bu yöntem, yüksek kullanılabilirlik elde etmek için pratik olmayabilir. RADIUS istemcisi ve RADIUS sunucuları (Bu durumda, Azure MFA RADIUS sunucuları olarak davranan sunucuları) arasındaki RADIUS trafiği Yük Dengelemesi daha verimli olur böylece işaret edebilir tek bir URL ile RADIUS istemcileri yapılandırabilirsiniz.
* **MFA Astları el ile yükseltmeniz gerekir**. Ana Azure MFA sunucusu çevrimdışı olursa, ikincil Azure MFA sunucularının MFA istekleri işlemeye devam eder. Ancak, bir ana MFA sunucusu kullanılabilir olana kadar Yöneticileri olmayan kullanıcılar ekleyebilir veya MFA ayarlarını değiştirin ve kullanıcılar kullanıcı portalını kullanarak değişiklikler yapabilir değil. Bir MFA yükseltme yöneticisi rolünü bağımlı her zaman elle yapılan bir işlemdir.
* **Bileşenlerin separability**. Azure MFA sunucusu, aynı Windows Server örneğine veya farklı örneklerinde yüklü birden fazla bileşenden oluşur. Bu bileşenler, kullanıcı portalı, mobil uygulama Web hizmeti ve ADFS bağdaştırıcısını (aracı) içerir. Bu separability, kullanıcı portalı ve mobil uygulama Web sunucusu çevre ağından yayımlamak için Web uygulaması Ara sunucusu kullanmayı mümkün kılar. Bu tür bir yapılandırma, aşağıdaki diyagramda gösterildiği gibi tasarım genel güvenlik ekler. MFA Kullanıcı Portalı ve mobil uygulama Web sunucusu ayrıca HA yük dengeli yapılandırmasında dağıtılabilir.

   ![MFA sunucusu ile bir çevre ağı](./media/howto-mfaserver-deploy-ha/mfasecurity.png)

* **Tek kullanımlık parola (OTP) SMS (diğer adıyla tek yönlü SMS) üzerinden, yük dengeli trafiği ise Yapışkan oturumlar kullanılmasını gerektiren**. Tek yönlü SMS MFA sunucusu, kullanıcıların bir OTP içeren bir kısa mesaj göndermek neden bir kimlik doğrulama seçenektir. Kullanıcı MFA testini tamamlamanız için bir komut istemi penceresinde OTP girer. Azure MFA sunucularının yükünü, ilk kimlik doğrulaması isteği sunan sunucunun kullanıcıdan OTP iletiyi alır sunucunun olmalıdır; başka bir MFA sunucusu OTP yanıt alırsa, kimlik doğrulaması sınaması başarısız olur. Daha fazla bilgi için [SMS eklenen Azure MFA sunucusu üzerinden bir saat parola](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Kullanıcı Portalı ve mobil uygulama Web hizmeti dağıtımları yük dengeli gerektiren Yapışkan oturumlar**. Her oturum, yük MFA Kullanıcı Portalı ile mobil uygulama Web hizmeti Dengeleme, aynı sunucuda kalır gerekir.

## <a name="high-availability-deployment"></a>Yüksek kullanılabilirlik dağıtımı

Aşağıdaki diyagramda bir tam HA yük dengeli uygulamasını Azure mfa'yı ve bileşenlerini, ADFS başvurusu için birlikte gösterir.

 ![Azure MFA Server HA uygulama](./media/howto-mfaserver-deploy-ha/mfa-ha-deployment.png)

Yukarıdaki diyagramda şu öğelerden gelenlere numaralandırılmış alan için unutmayın.

1. İki Azure MFA sunucusu (MFA1 ve MFA2) yük dengeli (mfaapp.contoso.com) olan ve statik bağlantı noktasını kullanmak üzere yapılandırılmış (4443 PhoneFactor.pfdata veritabanını çoğaltmak için). Web hizmeti SDK'sı, her AD FS sunucuları ile TCP bağlantı noktası 443 üzerinden iletişimi etkinleştirmek için MFA sunucusu yüklenir. MFA sunucularını, durum bilgisi olmayan bir yük dengeli yapılandırmasında dağıtılır. Ancak, OTP SMS kullanmak istiyorsanız, durum bilgisi olan Yük Dengeleme kullanmanız gerekir.
   ![Azure MFA Server - App server HA](./media/howto-mfaserver-deploy-ha/mfaapp.png)

   > [!NOTE]
   > RPC dinamik bağlantı noktaları kullandığından, güvenlik duvarları RPC potansiyel olarak kullanabileceğiniz dinamik bağlantı noktası aralığını kadar açmak için önerilmez. Bir güvenlik duvarınız varsa **arasında** MFA uygulama sunucularınız bir statik bağlantı çoğaltma trafiği bağımlı ve asıl sunucuları arasında iletişim kurmak ve bu, güvenlik duvarı bağlantı noktasını açmak için MFA sunucusu yapılandırmanız gerekir. Bir DWORD kayıt defteri değerinde oluşturarak statik bağlantı noktasını zorlayabilirsiniz ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` adlı ```Pfsvc_ncan_ip_tcp_port``` ve uygun bir statik bağlantı noktası değerini ayarlar. Bağlantıları her zaman ana bağımlı MFA sunucuları tarafından başlatılan, statik bağlantı noktasını yalnızca ana gereklidir, ancak herhangi bir zamanda Yöneticisi olarak bir alt yükseltebilirsiniz olduğundan, tüm MFA sunucularındaki statik bağlantı noktasını ayarlamanız gerekir.

2. İki kullanıcı portalı/MFA mobil uygulama sunucularını (MFA yukarı MAS1 ve MFA yukarı MAS2) içinde Yük Dengelemesi yapılıyor bir **durum bilgisi olan** yapılandırma (mfa.contoso.com). Yapışkan oturumlar MFA Kullanıcı Portalı ve mobil uygulama hizmeti Yük Dengeleme gereksinimini olduğunu hatırlayın.
   ![Azure MFA sunucusu - kullanıcı portalı ve mobil uygulama hizmeti HA](./media/howto-mfaserver-deploy-ha/mfaportal.png)
3. AD FS sunucu grubu yük dengeli ve çevre ağdaki yük dengeli ADFS proxy üzerinden yayımlanan ' dir. Her bir ADFS sunucusu TCP bağlantı noktası 443 üzerinden bir tek bir yük dengeli URL (mfaapp.contoso.com) kullanarak Azure MFA sunucusu ile iletişim kurmak için AD FS aracı kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure MFA sunucusunu yükleme ve yapılandırma](howto-mfaserver-deploy.md)
