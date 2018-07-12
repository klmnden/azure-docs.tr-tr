---
title: Güvenlik en iyi yöntemler için Iaas iş yüklerini azure'da | Microsoft Docs
description: " Azure Iaas iş yüklerinin geçişini bizim tasarımları yeniden değerlendirmek için fırsatlar sunar "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: barclayn
ms.openlocfilehash: 37620e70377e3f1fbeeeb73aaa294c5f54cf5b3d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724102"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Azure Iaas iş yükleri için en iyi güvenlik uygulamaları

Bir hizmet (Iaas) senaryoları olarak çoğu altyapısında [Azure sanal makineleri (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/) bulut kullanan kuruluşlar için ana iş yükü olan bilgi işlem. Bu durum, özellikle dikkati çekiyor [karma senaryolar](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) kuruluşlar yavaş iş yüklerini buluta taşımak istediğiniz. Böyle senaryolarda izleyin [Iaas için genel güvenlik konuları](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx)ve en iyi güvenlik uygulamaları, tüm Vm'lere uygulayabilirsiniz.

Güvenlik için sizin sorumluluğunuzdadır, bulut hizmeti türüne bağlıdır. Aşağıdaki grafik, hem Microsoft hem de sizin sorumluluğunuzdadır bakiyesini özetlenmektedir:

![Sorumluluk alanları](./media/azure-security-iaas/sec-cloudstack-new.png)

Güvenlik gereksinimleri bir dizi farklı türde iş yükleri gibi etkenlere bağlı olarak farklılık gösterir. Bu en iyi bir tek başına sistemlerinizin güvenliğini sağlayabilirsiniz. Başka bir şey gibi güvenlik, size uygun seçenekleri belirleyin ve nasıl çözümler birbirine boşlukları doldurarak tamamlayabilir görmek sahip.

Bu makalede, çeşitli VM Güvenlik en iyi uygulamalar ele alınmaktadır, her müşteri ve Vm'leri ile doğrudan deneyimler Microsoft'un kendi türetilmiş.

Bir fikrim fikir birliği üzerinde en iyi uygulamaları temel alır ve geçerli Azure platform özellikleriyle çalışma ve özellik kümeleri. Fikirlerini ve teknolojileri zaman içinde değişebileceği için bu makale güncelleştirilecektir bu değişiklikleri yansıtır.

## <a name="use-privileged-access-workstations"></a>Ayrıcalıklı erişim iş istasyonları kullanın

Kuruluşlar genellikle kalan siber saldırılara yükseltilmiş haklara sahip hesaplar'ı kullanırken Yöneticiler eylemleri gerçekleştirmek için prey. Bu kötü amaçlı etkinliğin sonucu olmayabilir ancak yapılandırmayı ve işlemleri izin gerçekleşir. Bu kullanıcılar çoğu kavramsal açısından bu eylemler riskini anlamak, ancak yine de bunları yapmak seçin.

E-posta almasını ve İnternet'e gözatma yeterince zararsız görünen gibi şeyler yapıyorlar. Ancak yükseltilmiş hesapları tarafından kötü amaçlı aktörler tehlikeye maruz bırakabilir. Etkinlikleri, özel olarak hazırlanmış bir e-posta ve diğer teknikleri gözatma Kurumsal erişim elde etmek için kullanılabilir. Tüm Azure yönetim görevlerini yürütülmesi için güvenli yönetim iş istasyonları (Saw) kullanılmasını öneririz. Saw, yanlışlıkla tehlikeye maruz kalma riskinizi azaltır, bir yoludur.

Ayrıcalıklı erişim iş istasyonları (Paw'lar), hassas görevler--bir Internet saldırıları ve tehdit vektörlerinden korunan ayrılmış bir işletim sistemi sağlar. Hassas görevler ve hesapların günlük kullanım iş istasyonları ve cihazlarından ayrılması, güçlü koruma sağlar. Bu ayrım, kimlik avı saldırıları, uygulama ve işletim sistemi güvenlik açıkları, çeşitli kimliğe bürünme saldırıları ve kimlik bilgisi hırsızlığı saldırılarını etkisini sınırlar. (, Pass--Hash ve Pass--Ticket tuş vuruşu kaydetme)

PAW yaklaşımı, bireysel olarak atanmış bir yönetici hesabı kullanacak şekilde tanınmış ve önerilen bir yöntem bir uzantısıdır. Yönetici hesabı, standart kullanıcı hesabından ayrıdır. Bir PAW hassas hesaplar için güvenilir bir iş istasyonu sağlar.

Daha fazla bilgi ve uygulama yönergeleri için bkz [ayrıcalıklı erişim iş istasyonları](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması kullan

Geçmişte, ağ çevrenizi Kurumsal verilere erişimi denetlemek için kullanıldı. Bulut öncelikli, mobil öncelikli bir dünyada denetim düzlemi kimliğidir: herhangi bir CİHAZDAN Iaas hizmetlere erişimi denetlemek için kullanın. Ayrıca görünürlük ve, verilerin nerede ve nasıl kullanıldığı konusunda fikir almak için kullanırsınız. Kullanıcılarınızın Azure dijital kimlik koruma kimlik hırsızlığı aboneliklerinizden ve diğer cybercrimes korumanın taşıdır.

Bir hesap güvenliğini sağlamak için uygulayabileceğiniz en faydalı olduğu durumları adımlardan birini iki öğeli kimlik doğrulamasını etkinleştirmektir. İki öğeli kimlik doğrulama, parola yanı sıra bir şey kullanarak kimlik doğrulaması, bir yoludur. Başka bir kullanıcının parolasını almak için yöneten bir kişi tarafından erişim riskini azaltmaya yardımcı olur.

[Azure multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) basit bir oturum açma işlemi için kullanıcı taleplerini karşılarken, verilere ve uygulamalara erişimi korunmasına yardımcı. Bu, çok sayıda kolay doğrulama seçeneği--telefon araması, SMS mesajı veya mobil uygulama bildirimi ile güçlü kimlik doğrulaması sunar. Kullanıcıların tercih ettikleri yöntemi seçin.

Windows, iOS ve Android çalıştıran mobil aygıtlarda kullanılabilir Microsoft Authenticator mobil uygulamasında multi-Factor Authentication'ı kullanmak için en kolay yoludur. Windows 10 ve Azure Active Directory (Azure AD) ile şirket içi Active Directory tümleştirmesini en son sürümüyle [Windows iş için Hello](../active-directory/active-directory-azureadjoin-passport-deployment.md) sorunsuz çoklu oturum açma için Azure kaynakları için kullanılabilir. Bu durumda, Windows 10 cihaz ikinci faktörlü kimlik doğrulaması için kullanılır.

Azure aboneliğinizi yönetmek hesapları ve sanal makinelerde oturum açabilir hesapları, multi-Factor Authentication'ı kullanarak, yalnızca bir parola kullanmaktan daha büyük bir düzeyde güvenlik sağlar. Diğer formlara iki öğeli kimlik doğrulamayı da çalışabilir, ancak değil zaten üretimde iseler onları dağıtımı karmaşık olabilir.

Aşağıdaki ekran görüntüsünde, Azure multi-Factor Authentication için mevcut olan seçenekler bazıları gösterilmektedir:

![Çok faktörlü kimlik doğrulama seçenekleri](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Sınırlayabilir ve yönetici erişimi sınırlayın

Azure aboneliğinizi yönetmek hesaplarının güvenliğini sağlamak önemlidir. Bu hesapların hiçbirinin güvenliğinin gizlilik emin olmak için sürebilir diğer tüm adımlar değerini ve verilerinizin bütünlüğünü olumsuz duruma getirir. Olabildiğince kısa bir süre önce Resimli tarafından [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) iç saldırıları genel güvenlik tüm kuruluşlar için çok büyük bir tehdit teşkil.

Bunlara benzer aşağıdaki ölçütlere göre kişiler için yönetici hakları değerlendirin:

- Yönetici ayrıcalıkları gerektiren görevleri gerçekleştirdiği?
- Görevler ne sıklıkta gerçekleştiriliyor?
- Görev gerçekleştirilemeyeceğine ilişkin başka bir yönetici tarafından neden yapılamaz özel bir nedeniniz var mı?

Ayrıcalıklı ve neden her kabul edilebilir olmadığını verme için tüm diğer bilinen alternatif yaklaşımlar belgeleyin.

Tam zamanında yönetim gereksiz yükseltilmiş haklara sahip hesaplar varlığını olduğunda bu hakları gerekli olmayan dönemlerde engeller. Böylece yöneticiler işlerini yapmak için hesapları sınırlı bir süre için hakları yükseltilmiş. Daha sonra bu hakları, bir shift veya bir görev tamamlandığında sonunda kaldırılır.

Kullanabileceğiniz [Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md) yönetin, izleyin ve denetleyin erişimi kuruluşunuzdaki. Kuruluşunuzdaki kişiler gerçekleştirmesi eylemleri farkında kalır yardımcı olur. Ayrıca tam zamanında yönetim Azure AD'ye uygun yönetici kavramını sunarak taşır. Bu hesapları yönetici hakları verilmesi olanağına sahip olan bireylerdir. Kullanıcılar bu tür bir etkinleştirme işlemi boyunca gidebilir ve sınırlı bir süre için yönetici haklarına sahip.

## <a name="use-devtest-labs"></a>DevTest Labs kullanın

Azure labs ve geliştirme ortamları için donanım satın alma tanıtan gecikmeleri hemen alır. Bu, test ve geliştirme çevikliği elde etmek, kuruluşların sağlar. Öte yandan, Azure veya kendi benimsenmesini hızlandırmak amacıyla bir teknoloji konusunda eksikliği yönetici hakları atama ile aşırı esnek olması neden olabilir. Bu riski istemeden iç saldırıları kuruluş doğurabilir. Bazı kullanıcılar sahip olması gereken daha çok daha fazla erişim verilmiş.

[Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) hizmet kullanan [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC). RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gerekli erişim düzeyini vermek rollere görevlerini ekibiniz içinde ayırabilirsiniz. RBAC, önceden tanımlı rollerle (sahip, Laboratuvar kullanıcı ve katılımcı) gelir. Bu roller, işbirliği büyük ölçüde basitleştirir ve dış iş ortakları hakları atamak için bile kullanabilirsiniz.

DevTest Labs RBAC kullandığından, ek oluşturmak mümkün [özel roller](../lab-services/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs yalnızca izinlerin yönetimini basitleştirir, sağlanan ortamları alma işlemini basitleştirir. Ayrıca, geliştirme ve test ortamlarında çalışan takımlar tipik zorlukları başa çıkmanıza yardımcı olur. Bazı hazırlık gerektirir, ancak uzun vadede şeyler ekibiniz için kolaylaştırır.

Azure DevTest Labs özelliklere sahiptir:

- Kullanıcılar için kullanılabilir seçenekleri üzerinde yönetici denetimi. Yönetici, izin verilen VM boyutları, en büyük sayı ve VM'lerin VM'ler başlatıldığında ve kapatma gibi merkezi olarak yönetebilir.
- Otomasyon laboratuvar ortamı oluşturma.
- Maliyet İzlemesi.
- Geçici birlikte çalışma için basitleştirilmiş dağıtım VM.
- Self Servis, kullanıcıların kendi labs şablonlarını kullanarak sağlama olanak tanır.
- Yönetme ve tüketimini sınırlamak.

![DevTest Labs](./media/azure-security-iaas/devtestlabs.png)

DevTest Labs kullanımını ek ücret ödemeden ilişkilidir. Laboratuvarlar, ilkeleri, şablonlar ve yapıtlar oluşturulmasını ücretsizdir. Yalnızca sanal makineler, depolama hesapları ve sanal ağlar gibi laboratuvarlarınızı kullanılan Azure kaynakları için ödeme yaparsınız.

## <a name="control-and-limit-endpoint-access"></a>Uç noktası erişim denetimi ve sınırı

Labs veya azure'da üretim sistemleri barındırma sistemlerinizi Internet'ten erişilebilir olması gerektiği anlamına gelir. Varsayılan olarak, yeni bir Windows sanal makine RDP bağlantı noktası Internet'ten erişilebilir olan ve bir Linux sanal makinesi açın SSH bağlantı noktasına sahip. Sınır açık Uç noktalara adımların atılmasından yetkisiz erişim riskini en aza indirmek gereklidir.

Azure teknolojileri yönetici bu uç noktalarına erişimi sınırlamanıza yardımcı olabilir. Azure'da kullanabileceğiniz [ağ güvenlik grupları](../virtual-network/security-overview.md) (Nsg'ler). Azure Resource Manager dağıtımı için kullandığınızda, Nsg'ler, tüm ağlardan erişime yalnızca yönetim uç noktalarına (RDP veya SSH) sınırlayın. Nsg'ler düşünürken, yönlendirici ACL'leri düşünün. Sıkı bir şekilde Azure ağlarınız çeşitli parçalarını arasındaki ağ iletişimini denetlemek için bunları kullanabilirsiniz. Bu, Çevre ağları veya diğer yalıtılmış ağlarda ağ oluşturma işlemiyle benzerdir. Trafiği incelemeyin, ancak ağ kesimleme ile yardımcı olurlar.

Azure Güvenlik Merkezi'ni kullanmayı sanal makinelere erişimi sınırlamak için daha dinamik bir yol olan [tam zamanında Yönetim](../security-center/security-center-just-in-time.md). Güvenlik Merkezi, Azure Vm'lerinizi kilitleyebilir ve gerektiğinde erişim sağlar. İşlemin çalıştığını doğruladıktan sonra temel alan isteyen kullanıcı için erişim sağlayarak kendi [rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md) (RBAC) gerekli izinlere sahiptirler. Azure Güvenlik Merkezi, gerekli ağ güvenlik grupları (Nsg'ler) gelen trafiğe izin veren sonra yapın.

### <a name="site-to-site-vpnvpn-gatewayvpn-gateway-howto-site-to-site-resource-manager-portalmd"></a>[Siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Siteden siteye VPN, şirket içi ağınızı buluta genişletir. Nsg'leri yerel ağa diğer her yerden erişim izin vermeyecek şekilde de değiştirebilirsiniz çünkü bu, Nsg'leri, kullanılacak başka bir fırsat sağlar. Ardından, yönetim ilk VPN aracılığıyla Azure ağa bağlanarak yapılır gerektirebilir.

Siteden siteye VPN seçeneği burada şirket içi kaynaklarınızı Azure ile yakından tümleşik üretim sistemlerine barındırma durumlarda en cazip olabilir.

### <a name="point-to-sitevpn-gatewayvpn-gateway-howto-point-to-site-rm-psmd"></a>[Noktadan siteye](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

Şirket içi kaynaklara erişmesi gereken olmayan sistemleri yönetmek istediğiniz durumlarda. Bu sistemler, kendi Azure sanal ağında yalıtılabilir. Azure'a VPN Administrators can yönetici iş istasyonunda ortamından barındırılan.

>[!NOTE]
>Her iki VPN seçeneği, ACL'ler Nsg'leri erişim yönetimi uç noktalar için Internet'ten izin vermeyecek şekilde yeniden yapılandırmak için kullanabilirsiniz.

### <a name="remote-desktop-gatewayactive-directoryauthenticationhowto-mfaserver-nps-rdgmd"></a>[Uzak Masaüstü Ağ Geçidi](../active-directory/authentication/howto-mfaserver-nps-rdg.md)

Daha ayrıntılı denetimler için bu bağlantıları uygulanırken Uzak Masaüstü sunucularına HTTPS üzerinden güvenli bir şekilde bağlanmak için Uzak Masaüstü Ağ Geçidi'ni kullanabilirsiniz.

Bu özellikler, erişebildiğiniz eklemek için:

- Bağlantıları istekleri belirli sistemlerle sınırlamak için yönetici Seçenekleri'ni kullanın.
- Akıllı kart kimlik doğrulaması veya Azure multi-Factor Authentication.
- Hangi sistemlerin biri ağ geçidi üzerinden bağlanabilir denetler.
- Cihaz ve disk yeniden yönlendirme üzerinde denetim.

### <a name="vm-availability"></a>Sanal Makine kullanılabilirliği

Bir sanal makine yüksek kullanılabilirliğe sahip olması Kritik uygulamalar çalıştırıyorsa, birden çok VM kullanılır önemle tavsiye olur. Daha iyi kullanılabilirlik için en az iki VM oluşturma [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md).

[Azure Load Balancer](../load-balancer/load-balancer-overview.md) yük dengeli VM'ler aynı kullanılabilirlik kümesine ait olmasını da gerektirir. Bu VM'ler, Internet üzerinden erişilmelidir, yapılandırmalısınız bir [Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-internet-overview.md).

## <a name="use-a-key-management-solution"></a>Bir anahtar yönetimi çözümü kullanın

Güvenli anahtar yönetimi buluttaki verileri korumak için gereklidir. İle [Azure anahtar kasası](../key-vault/key-vault-whatis.md), şifreleme anahtarlarını güvenli bir şekilde depolayabilirsiniz ve küçük gizli anahtarları ister parola donanım güvenlik modülleri (HSM'ler) içinde. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz.

Microsoft, anahtarlarınızı FIPS 140-2 Düzey 2 doğrulamasına sahip Hsm'lerde (donanım ve bellenim) işlemleri. Azure günlüğü ile anahtar kullanımını izleyin ve denetleyin: ek çözümleme ve tehdit algılama için Azure veya güvenlik bilgileri ve Olay yönetimi (SIEM) sistemine günlükleri kanal oluşturarak.

Herhangi bir Azure aboneliği oluşturabilir ve anahtar kasalarını kullanabilirsiniz. Anahtar kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, uygulanan ve bir kuruluştaki Azure hizmetlerini yönetmek için sorumlu bir yönetici tarafından yönetilir.

## <a name="encrypt-virtual-disks-and-disk-storage"></a>Sanal diskleri ve disk depolamayı şifreleme

[Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) veri hırsızlığı veya bir disk taşıyarak elde yetkisiz erişimden kesilmeyen yöneliktir. Diski başka bir sistem diğer güvenlik denetimlerini atlama bir yol olarak iliştirilebilir. Disk şifreleme kullanan [BitLocker](https://technet.microsoft.com/library/hh831713) Windows ve Linux'ta DM-Crypt işletim sistemi ve veri sürücüleri şifrelemek için. Azure Disk şifrelemesi, Denetim ve şifreleme anahtarlarını yönetmek için Key Vault ile tümleştirilir. Premium depolama ile standart Vm'leri ve VM'ler için kullanılabilir.

Daha fazla bilgi için [Azure Disk şifrelemesi Windows ve Linux Iaas sanal makineleri](azure-security-disk-encryption.md).

[Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md) bekleyen verilerin korunmasına yardımcı olur. Depolama hesabı düzeyinde etkinleştirilir. Veri merkezlerimize yazıldığı ve siz eriştiğinizde otomatik olarak çözülür şifreler. Bunu, aşağıdaki senaryoları destekler:

- Şifreleme blok blobları, ekleme blobları ve sayfa blobları
- Arşivlenen VHD'ler ve şablonlardan şirket içinden Azure'a duruma şifreleme
- Vhd'lerinizi kullanarak oluşturduğunuz Iaas Vm'leri için temel işletim sistemi ve veri diskleri şifreleme

Azure depolama şifreleme ile devam etmeden önce iki sınırlamaları unutmayın:

- Klasik depolama hesapları kullanılamaz.
- Yalnızca şifreleme etkinleştirildikten sonra yazılan verileri şifreler.

## <a name="use-a-centralized-security-management-system"></a>Güvenlik Merkezi Yönetim sistemini kullanın

Sunucularınızın düzeltme eki uygulama, yapılandırma, olayları ve güvenlik konuları ele alınması etkinlikleri için izlenmesi gerekir. Bu sorunları çözmek için kullanabileceğiniz [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Operations Management Suite güvenlik ve Uyumluluk](https://azure.microsoft.com/services/security-center/). Bu iki seçenek işletim sistemindeki configuration ötesine gidin. Ağ Yapılandırması ve sanal gereç kullanma gibi temel alınan altyapının yapılandırma izlenmesini de sağlanır.

## <a name="manage-operating-systems"></a>İşletim sistemlerini yönetme

Bir Iaas dağıtımında, ortamınızda dağıttığınız sistemlerinin, tıpkı herhangi bir sunucu veya iş istasyonu Yönetim için sorumlusunuz. Düzeltme eki uygulama, sağlamlaştırma, hak atama ve sisteminizin bakımla ilgili herhangi bir etkinlik hala sizin sorumluluğunuzdadır. Şirket içi kaynaklarınız ile sıkı bir şekilde tümleşik sistemler için aynı araçları ve yordamları, şirket içi virüsten koruma ve kötü amaçlı yazılımdan koruma, düzeltme eki uygulama ve yedekleme gibi şeyler için kullandığınız kullanmak isteyebilirsiniz.

### <a name="harden-systems"></a>Sistemleri sağlamlaştırma

Bunlar yüklü olan uygulamalar için gerekli olan yalnızca hizmet uç noktalarını kullanıma sunar, böylece tüm sanal makinelerin Azure Iaas sıkı. Windows sanal makineler için taban çizgileri olarak Microsoft yayımlayan önerilere uyun [güvenlik uyumluluk Yöneticisi](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) çözüm.

Güvenlik uyumluluk Yöneticisi ücretsiz bir araçtır. Hızlı bir şekilde yapılandırın ve Grup İlkesi ve System Center Configuration Manager'ı kullanarak, masaüstü, geleneksel veri merkezi ve özel ve genel bulut yönetmek için kullanabilirsiniz.

Güvenlik uyumluluk Yöneticisi ilkeleri dağıtmak için hazır ve test edilen Desired Configuration Management yapılandırma paketleri sağlar. Bu taban çizgileri dayalı [Microsoft Güvenlik rehberi](https://technet.microsoft.com/library/cc184906.aspx) öneriler ve sektörün en iyi uygulamalar. Yapılandırma değişikliklerini, adres uyumluluk gereksinimleri, yönetmek ve güvenlik tehditleri azaltmaya yardımcı olur.

Güvenlik uyumluluk Yöneticisi, iki farklı yöntemler kullanarak bilgisayarlarınızı geçerli yapılandırmasını içeri aktarmak için kullanabilirsiniz. İlk olarak, Active Directory tabanlı Grup İlkeleri aktarabilirsiniz. İkinci olarak, bir "altın ana" yapılandırmasını içeri aktarabilirsiniz kullanarak başvuru makinesini [LocalGPO aracı](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) yerel Grup İlkesi yedeklenir. Ardından, yerel Grup İlkesi güvenlik uyumluluk Yöneticisi içine aktarabilirsiniz.

Sektördeki en iyi uygulamalar, standartlara karşılaştırın, özelleştirin ve yeni ilkeleri Desired Configuration Management yapılandırma paketlerini oluşturup. Tüm desteklenen dahil olmak üzere Windows 10 Yıldönümü güncelleştirmesi ve Windows Server 2016 için işletim sistemleri, taban çizgilerini yayımlandı.


### <a name="install-and-manage-antimalware"></a>Yükleme ve kötü amaçlı yazılımdan koruma yönetme

Üretim ortamınızdan ayrı olarak barındırılan ortamlar için bulut Hizmetleri ve sanal makinelerinizin korunmasına yardımcı olmak için bir kötü amaçlı yazılımdan koruma uzantısını kullanabilirsiniz. İle tümleşir [Azure Güvenlik Merkezi](../security-center/security-center-intro.md).

[Microsoft Antimalware](azure-security-antimalware.md) gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, altyapı güncelleştirmeleri, raporlama, dışlama olay koleksiyonu örnekleri gibi özellikler içerir ve [PowerShelldesteği](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure kötü amaçlı yazılımdan koruma](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>En son güvenlik güncelleştirmelerini yükleme 

Müşterilerin Azure'a taşımak ilk iş yüklerinin laboratuvarlar ve dönük sistemleri bazılarıdır. Azure'da barındırılan sanal makinelerinizi, Internet'e erişilmesi gereken uygulamaları veya hizmetleri barındırıyorsanız, düzeltme eki uygulama hakkında dikkatli olun. İşletim sisteminin ötesine geçen eki. Üçüncü taraf uygulamaların Açıklarında güvenlik açıklarını da iyi düzeltme eki yönetimi yerinde olduğunda önlenebilir sorunlara neden olabilir.

### <a name="deploy-and-test-a-backup-solution"></a>Dağıtma ve bir yedekleme çözümü test etme

Güvenlik güncelleştirmeleri olduğu gibi bir yedekleme başka bir işlem işleme aynı şekilde ele alınması gerekir. Bu, üretim ortamınızın buluta genişletme parçası olan sistemlerinin geçerlidir. Test ve geliştirme sistemleri, hangi kullanıcıların dayalı olarak, şirket içi ortamlarla deneyimlerini alışık için benzer bir geri yükleme özelliklerini sağlayın yedekleme stratejileri izlemeniz gerekir.

Üretim iş yüklerini Azure'a taşındı. mevcut yedekleme çözümleri ile mümkün olduğunda tümleşmelidir. Veya, kullanabileceğiniz [Azure Backup](../backup/backup-azure-arm-vms.md) yedekleme gereksinimlerinizi çözümüne yardımcı olmak için.

## <a name="monitor"></a>İzleme

### <a name="security-centersecurity-centersecurity-center-intromd"></a>[Güvenlik Merkezi](../security-center/security-center-intro.md)

Güvenlik Merkezi olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumuna devam eden değerlendirmesi sağlar. Gerekli denetimlerin yapılandırılması işlemi boyunca bir öneri listesi size rehberlik eder.

Örneklere şunlar dahildir:

- Tanımlamak ve kötü amaçlı yazılımı kaldırmak için kötü amaçlı yazılımdan koruma sağlama.
- Ağ güvenlik gruplarının ve kuralların trafiği denetlemek için sanal makine yapılandırma.
- Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için web uygulaması güvenlik duvarları sağlama.
- Eksik sistem güncelleştirmelerini dağıtma.
- Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma.

Aşağıdaki görüntüde, Güvenlik Merkezi'nde etkinleştirebilirsiniz seçenekleri bazıları gösterilmektedir.

![Azure Güvenlik Merkezi ilkeleri](./media/azure-security-iaas/security-center-policies.png)

### <a name="operations-management-suiteoperations-management-suiteoperations-management-suite-overviewmd"></a>[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) 

Operations Management Suite yardımcı olan bulut tabanlı BT yönetim çözümü yönetmek ve şirket içi koruma hem de bulut altyapısında bir Microsoft'tur. Operations Management Suite bulut tabanlı bir hizmet olarak uygulandığından, hızlı ve altyapı kaynakları için çok az yatırım dağıtılabilir.

Yeni özellikler otomatik olarak devam eden bakım tasarruf sağlarsınız ve yükseltme maliyetlerinden. Operations Management Suite, aynı zamanda System Center Operations Manager ile tümleştirilir. Farklı bileşenler dahil olmak üzere Azure iş yüklerinizi daha iyi yönetmenize yardımcı olmak için sahip bir [güvenlik ve Uyumluluk](../operations-management-suite/oms-security-getting-started.md) modülü.

Kaynaklarınızı hakkında bilgileri görüntülemek için Operations Management Suite güvenlik ve uyumluluk özellikleri kullanabilirsiniz. Bilgiler, dört ana kategoride düzenlenmiştir:

- **Güvenlik etki alanları**: daha fazla zaman içindeki güvenlik kayıtları keşfedin. Kötü amaçlı yazılım değerlendirmesi erişmek, değerlendirme, ağ güvenlik bilgileri, kimlik ve erişim bilgileri ve Bilgisayarları güvenlik olaylarıyla güncelleştirin. Hızlı erişim için Azure Güvenlik Merkezi panosunu yararlanın.
- **Önemli sorunlar**: Etkin sorunların sayısını ve sorunların önem derecesini hızlıca belirleyin.
- **Algılamalar (Önizleme)**: belirlenmesine karşı kaynaklarınızı gelişmelerden göre güvenlik uyarıları görselleştirme desenleri.
- **Tehdit bilgileri**: belirlenmesine toplam sunucu sayısına göre giden kötü amaçlı IP trafiğini, kötü amaçlı tehdit türü ve burada bu IP'lerin geldiğini gösteren bir harita görselleştirme tarafından desenleri.
- **Ortak Güvenlik sorguları**: ortamınızı izlemek için kullanabileceğiniz en sık kullanılan güvenlik sorgularının listesini bakın. Bu sorgulardan birine tıkladığınızda **arama** dikey penceresi açılır ve bu sorgu için sonuçları gösterir.

Aşağıdaki ekran görüntüsünde, Operations Management Suite görüntüleyebilen bilgileri örneği gösterilmektedir.

![Operations Management Suite güvenlik temelleri](./media/azure-security-iaas/oms-security-baseline.png)

### <a name="monitor-vm-performance"></a>Sanal makine performansını izleme

VM işlem izin verilenden daha fazla kaynak tüketmesine kaynak uygunsuz bir sorun olabilir. Bir VM ile performans sorunlarını, kullanılabilirlik, güvenlik ilkesini ihlal hizmet kesintisi için neden olabilir. Bu nedenle, bir sorun oluştuğunu, ancak VM erişimi öngörülebiliyorsa değil yalnızca izleme olmazsa olmaz ancak aynı zamanda proaktif bir şekilde normal işlem sırasında'nin batısı temel performans karşı.

Analiz tarafından [Azure tanılama günlük dosyaları](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), VM kaynaklarınızı izleyin ve performans ve kullanılabilirlik riske atabilirdi olası sorunları tanımlayın. Azure tanılama uzantısı, Windows tabanlı Vm'lere izleme ve tanılama özellikleri sağlar. Bu özellikler uzantısı bir parçası olarak dahil ederek etkinleştirebilirsiniz [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md).

Ayrıca [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-metrics.md) kaynaklarınızın sistem durumu görünürlük elde etmek için.

VM performansı izleme kuruluşlar, bazı değişiklikler performans desenleri normal veya anormal olup olmadığını belirlemek belirleyemiyoruz. Böyle bir anomali normalden daha fazla kaynak sanal tüketilmesine neden olabilir, olası bir saldırı bir dış kaynağa veya VM'de çalışan güvenliği aşılmış bir işlem olduğunu gösteriyor olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure güvenlik en iyi uygulamalar ve modeller](security-best-practices-and-patterns.md)
