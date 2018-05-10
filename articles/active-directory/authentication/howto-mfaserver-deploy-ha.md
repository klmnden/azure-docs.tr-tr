---
title: Azure MFA sunucusu yüksek kullanılabilirlik için yapılandırma | Microsoft Docs
description: Azure multi-Factor Authentication sunucusu yüksek kullanılabilirlik sağlamak yapılandırmalarında birden çok örneğini dağıtın.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 08/23/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 75d3906819174a8bc3eeaa247dffa937a02ceab1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Azure multi-Factor Authentication sunucusu yüksek kullanılabilirlik için yapılandırma

Azure sunucusu MFA dağıtımınız ile yüksek kullanılabilirlik elde etmek için birden çok MFA sunucusu dağıtmak için gerekir. Bu bölümde, Azure MFS sunucu dağıtımı, yüksek kullanılabilirlik hedeflerini elde etmek için yük dengeli bir tasarım hakkında bilgiler sağlar.

## <a name="mfa-server-overview"></a>MFA sunucusu genel bakış

Azure MFA sunucusu hizmet mimarisi, aşağıdaki çizimde gösterildiği gibi birçok bileşenden oluşur:

 ![MFA sunucusu mimarisi](./media/howto-mfaserver-deploy-ha/mfa-ha-architecture.png)

Azure multi-Factor Authentication yazılımı yüklü olan bir Windows Server bir MFA sunucusudur. MFA sunucusu örneğini Azure MFA hizmeti tarafından çalışması için etkinleştirilmesi gerekir. Birden fazla MFA sunucusu içi yüklü olabilir.

Yüklü ilk MFA sunucusu MFA sunucusu varsayılan olarak Azure MFA hizmeti tarafından etkinleştirme sırasında yöneticisidir. Ana MFA sunucusu PhoneFactor.pfdata veritabanı yazılabilir bir kopyası bulunur. MFA sunucusu örneklerinin sonraki yüklemelerini slaves bilinir. MFA slaves çoğaltılmış salt okunur veritabanının bir kopyasını PhoneFactor.pfdata sahip. MFA sunucusu uzak yordam çağrısı (RPC) kullanarak bilgileri çoğaltarak. Tüm MFA sunucularının topluca ya da etki alanına katılmış veya bilgi çoğaltmak için tek başına olması gerekir.

İki faktörlü kimlik doğrulaması gerekli olduğunda MFA ana ve bağımlı MFA sunucuları MFA hizmetiyle iletişim kurar. Örneğin, bir kullanıcı iki öğeli kimlik doğrulama gerektiren bir uygulamaya erişim girişiminde bulunduğunda, kullanıcı ilk gibi Active Directory (AD) bir kimlik sağlayıcısı tarafından doğrulanır.

AD ile başarılı kimlik doğrulamasından sonra MFA sunucusu MFA hizmetiyle iletişim kurar. MFA sunucusu veya uygulama için kullanıcı erişimini reddetmek için MFA hizmetinden bildirim bekler.

MFA ana sunucu çevrimdışı olursa, kimlik doğrulamaları hala işlenebilir, ancak MFA veritabanında yapılan değişiklikler gerektiren işlemler işlenemiyor. (Örnekler: Ayrıca kullanıcılar, Self Servis PIN değişiklikleri ve kullanıcı bilgilerini değiştirme)

## <a name="deployment"></a>Dağıtım

Yük Dengeleme Azure MFA sunucusu ve ilişkili bileşenler için aşağıdaki önemli noktaları göz önünde bulundurun.

* **Yüksek kullanılabilirlik elde etmek için RADIUS Standart kullanarak**. RADIUS sunucuları olarak Azure MFA sunucuları kullanıyorsanız, bir MFA sunucusu birincil RADIUS kimlik doğrulaması hedefi ve diğer Azure MFA sunucusu ikincil kimlik doğrulama hedefleri olarak potansiyel olarak yapılandırabilirsiniz. Ancak, ikincil kimlik doğrulama hedefe karşı doğrulanabilmesinden önce birincil kimlik doğrulaması hedef kimlik doğrulama başarısız olduğunda gerçekleşmesi bir zaman aşımı süresi beklemeniz gerekir çünkü yüksek kullanılabilirlik elde etmek için bu yöntem pratik olmayabilir. RADIUS istemcisi ve RADIUS sunucuları (Bu durumda, Azure MFA RADIUS sunucuları olarak hareket sunucuları) arasındaki RADIUS trafiği dengelemek için daha verimlidir böylelikle işaret edebilir tek bir URL ile RADIUS istemcileri yapılandırabilirsiniz.
* **MFA slaves el ile yükseltmeniz gerektiği**. Ana Azure MFA sunucusunun çevrimdışı olması durumunda ikincil Azure MFA sunucuları MFA istekleri işlemeye devam eder. Ancak, bir ana MFA sunucusu kullanılabilir olana kadar Yöneticileri olmayan kullanıcılar ekleyebilir veya MFA ayarları değiştirebilirsiniz ve kullanıcılar Kullanıcı Portalı'nı kullanarak değişiklik yapabilirsiniz değil. MFA ikincil ana rolüne yükseltme her zaman el ile bir işlemdir.
* **Bileşenlerinin separability**. Azure MFA sunucusu, aynı Windows Server örneğine veya farklı örneklerinde yüklü birçok bileşenden oluşur. Bu bileşenler, kullanıcı portalı, mobil uygulama Web hizmeti ve ADFS bağdaştırıcı (aracı) içerir. Bu separability Kullanıcı Portalı ile mobil uygulama Web sunucusu çevre ağından yayımlamak için Web uygulaması proxy'si kullanmayı mümkün kılar. Bu tür bir yapılandırma, aşağıdaki çizimde gösterildiği gibi tasarımınızın genel güvenlik ekler. Ayrıca MFA Kullanıcı Portalı ve mobil uygulama Web sunucusu HA yük dengeli yapılandırmasında dağıtılabilir.

   ![MFA sunucusu çevre ağı ile](./media/howto-mfaserver-deploy-ha/mfasecurity.png)

* **Tek kullanımlık parola (OTP) SMS (diğer adıyla tek yönlü SMS) üzerinden trafik yükü dengelenmiş ise Yapışkan oturumları kullanılmasını gerektiren**. Tek yönlü SMS kullanıcılar bir OTP içeren bir kısa mesaj göndermek için MFA sunucusunu neden olan bir kimlik doğrulama seçenektir. Kullanıcı, OTP MFA testini tamamlamanız için bir komut istemi penceresinde girer. Azure MFA sunucuları dengelemek ilk kimlik doğrulaması isteği sunan sunucunun kullanıcıdan OTP iletiyi alır sunucu olması gerekir; başka bir MFA sunucusu OTP yanıt alırsa, kimlik doğrulaması sınaması başarısız olur. Daha fazla bilgi için bkz: [SMS eklenen Azure MFA sunucusu üzerinden bir zaman parola](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server).
* **Yük dengeli dağıtımı Kullanıcı Portalı ve mobil uygulama Web hizmeti gerektirir Yapışkan oturumları**. Yük MFA Kullanıcı Portalı ile mobil uygulama Web hizmeti Dengeleme izliyorsanız, aynı sunucuda kalmak her oturum gerekir.

## <a name="high-availability-deployment"></a>Yüksek kullanılabilirlik dağıtımı

Aşağıdaki diyagramda, tam bir HA yük dengeli uygulama, Azure MFA ve başvuru için ADFS birlikte bileşenlerinin gösterir.

 ![Azure MFA Server HA uygulama](./media/howto-mfaserver-deploy-ha/mfa-ha-deployment.png)

Önceki diyagramda gelenlere numaralı alanı için aşağıdaki öğelerden unutmayın.

1. İki Azure MFA sunucusu (MFA1 ve MFA2) yükü dengelenmiş (mfaapp.contoso.com) ve statik bağlantı noktasını kullanmak üzere yapılandırılmış PhoneFactor.pfdata veritabanı çoğaltmak için (4443). Web hizmeti SDK'sı her ADFS sunucularıyla TCP bağlantı noktası 443 üzerinden iletişimi etkinleştirmek için MFA sunucusu yüklenir. MFA sunucusu, durum bilgisi olmayan bir yük dengeli yapılandırmasında dağıtılır. Ancak, OTP SMS kullanmak istiyorsanız, durum bilgisi olan Yük Dengeleme kullanmanız gerekir.
   ![Azure MFA Server - App server HA](./media/howto-mfaserver-deploy-ha/mfaapp.png)

   > [!NOTE]
   > RPC dinamik bağlantı noktaları kullandığından, güvenlik duvarları RPC potansiyel olarak kullanabileceğiniz dinamik bağlantı noktaları aralığı kadar açmak için önerilmez. Bir güvenlik duvarınız varsa **arasında** MFA uygulama sunucularınızın bağımlı ve asıl sunucuları arasındaki çoğaltma trafiği için statik bir bağlantı noktası iletişim ve güvenlik duvarını Bu bağlantı noktasını açmak için MFA sunucusunu yapılandırmanız gerekir. Bir DWORD kayıt defteri değerinde oluşturarak statik bağlantı noktası zorlayabilirsiniz ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` adlı ```Pfsvc_ncan_ip_tcp_port``` ve kullanılabilir bir statik bağlantı noktası değerini ayarlar. Bağlantıları her zaman ana bağımlı MFA sunucuları tarafından başlatılan, statik bağlantı noktası yalnızca ana gereklidir, ancak bağımlı herhangi bir zamanda ana olacak şekilde yükseltebilirsiniz olduğundan, tüm MFA sunucularında statik bağlantı noktasına ayarlamanız gerekir.

2. İki kullanıcı portalı/MFA mobil uygulama sunucularını (MFA yukarı MAS1 ve MFA yukarı MAS2) içinde dengeleneceğini bir **durum bilgisi olan** yapılandırma (mfa.contoso.com). Yapışkan oturumları Yük Dengeleme MFA Kullanıcı Portalı ve mobil uygulama hizmeti için bir gereksinim olan bir geri çağırma.
   ![Azure MFA sunucusu - kullanıcı portalı ile mobil uygulama hizmeti HA](./media/howto-mfaserver-deploy-ha/mfaportal.png)
3. ADFS sunucu grubu yükü dengelenir ve Yük Dengelemesi ADFS proxy'leri çevre ağındaki üzerinden internet yayımlanan ' dir. Her ADFS sunucusu ADFS Aracısı TCP bağlantı noktası 443 bir tek bir yük dengeli URL (mfaapp.contoso.com) kullanarak Azure MFA sunucularıyla iletişim kurmak için kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure MFA sunucusu yükleme ve yapılandırma](howto-mfaserver-deploy.md)
