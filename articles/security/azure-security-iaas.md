---
title: Güvenlik en iyi yöntemler için Iaas azure'da iş yüklerini | Microsoft Docs
description: " Azure Iaas iş yüklerinin geçişi bizim tasarımları yeniden değerlendirmeye fırsatlar getirir "
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
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 1a6ff01274c4a47730ffe45275aed9d122994260
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366282"
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Azure Iaas iş yükleri için en iyi güvenlik uygulamaları

İş yükleri için Azure altyapı (Iaas) hizmet olarak taşıma hakkında düşünmeye başlattığınız gibi bazı noktalar bilginiz büyük olasılıkla gerçekleşmiş. Sanal ortamlar güvenliğini sağlama deneyimi zaten sahip olabilir. Azure Iaas taşıdığınızda, sanal ortamlar korumak ve uzmanlık uygulamak ve varlıklarınızı güvenliğini sağlamaya yardımcı olmak için yeni bir seçenek kümesini kullanın.

Azure için şirket içi kaynaklar bire bir olarak getirmek bekliyoruz değil, söyleyerek başlayalım. Yeni zorluklar ve yeni seçenekler varolan yeniden değerlendirmeye fırsat Getir deigns, araçları ve işler.

Güvenlik için sizin sorumluluğunuzdadır bulut hizmetinin türüne dayalıdır. Aşağıdaki grafikte hem Microsoft hem de, sorumluluğu bakiyesini özetlenmektedir:


![Sorumluluk alanları](./media/azure-security-iaas/sec-cloudstack-new.png)


Biz, kuruluşunuzun güvenlik gereksinimlerine yardımcı olabilecek mevcut seçeneklerden bazıları ele alacağız. Güvenlik gereksinimleri, farklı türlerde iş yükleri için değişebilir aklınızda bulundurun. Bu en iyi uygulamaları bir tek başına sistemlerinizi güvenliğini sağlayabilirsiniz. Başka bir şey gibi güvenlik, size uygun seçenekleri seçin ve nasıl çözümler birbirine boşluklar doldurarak tamamlayabilir görmek sahip.

## <a name="use-privileged-access-workstations"></a>Ayrıcalıklı erişimli iş istasyonlarının kullanın

Kuruluşlar çoğunlukla kalan cyberattacks için yükseltilmiş haklara sahip hesaplar kullanırken Yöneticiler eylemleri gerçekleştirmek için prey. Genellikle bu amaçla yapılır değildir ancak yapılandırmayı ve işlemleri izin verdiğinden. Bu kullanıcılar çoğu kavramsal açısından bu eylemleri riskini anlamak ancak bunları yapmak hala seçin.

E-posta denetleme ve Internet'e gözatma yetecek zararsız göründüğü gibi işlemler yapılıyor. Ancak kötü amaçlı aktörler tarafından aşmaya yükseltilmiş hesapları doğurabilir. Etkinlikler, özel olarak hazırlanmış bir e-postaları veya diğer teknikleri gözatma kuruluşunuza erişmek için kullanılabilir. Tüm Azure yönetim görevlerini gerçekleştirme için güvenli yönetim iş istasyonları (kurlar) kullanılmasını öneririz. Kurlar yanlışlıkla tehlikeye maruz azaltma bir yoludur.

Ayrıcalıklı erişimli iş istasyonlarının (Patiler) adanmış bir işletim sistemi görevlerde hassas--Internet saldırıları ve tehdit vektörlerini korumalı bir sağlar. Bu önemli görevleri ve günlük kullanım iş istasyonları ve aygıtları hesaplarından ayırma güçlü koruma sağlar. Bu ayrım kimlik avı saldırıları, uygulama ve işletim sistemi güvenlik açıkları, çeşitli kimliğe bürünme saldırılarını ve kimlik bilgisi hırsızlığı saldırıların etkisini sınırlar. (Pass--Hash ve geçişi anahtar günlüğü tuş vuruşu)

PENÇE yaklaşım, tek tek atanmış bir yönetici hesabı kullanmak için tanınmış ve önerilen bir yöntem uzantısıdır. Yönetici hesabını bir standart kullanıcı hesabından ayrıdır. Bir PENÇE hassas bu hesaplar için güvenilir bir iş istasyonu sağlar.

Daha fazla bilgi ve uygulama yönergeleri için bkz [ayrıcalıklı erişimli iş istasyonlarının](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını kullan

Geçmişte, Kurumsal verilere erişimi denetlemek için ağ çevre kullanıldı. Bir bulut ilk olarak, mobil ilk dünyada denetim düzlemi kimliktir: herhangi bir aygıttan Iaas hizmetlere erişimi denetlemek için kullanın. Ayrıca görünürlük ve nerede ve nasıl verilerinizi kullanılıyor içine öngörüleri almak için kullanırsınız. Azure kullanıcılarınızın dijital kimliğini koruma aboneliklerinizi kimlik hırsızlığa ve diğer cybercrimes korumanın dönüm olur.

Bir hesap güvenliğini sağlamak için uygulayabileceğiniz en yararlı adımları iki faktörlü kimlik doğrulamasını etkinleştirmek için biridir. İki faktörlü kimlik doğrulaması parola yanı sıra bir şey kullanarak kimlik doğrulaması bir yoludur. Bu, başka birinin parolanızı almak için yöneten bir kişi tarafından erişim riskini azaltmaya yardımcı olur.

[Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md) basit bir oturum açma işlemi için kullanıcı talebine buluştururken veri ve uygulamalara erişimi korunmasına yardımcı. Kolay doğrulama seçenekleri--telefon araması, SMS mesajı veya mobil uygulama bildirimi aralıklı güçlü kimlik doğrulaması sunar. Kullanıcıların tercih yöntemi seçin.

Windows, iOS ve Android çalıştıran mobil aygıtlarda kullanılabilir Microsoft Authenticator mobil uygulamasında çok faktörlü kimlik doğrulamasını kullanmak için en kolay yoludur. Windows 10 ve Azure Active Directory (Azure AD) ile şirket içi Active Directory Tümleştirme en son sürümü ile [iş için Windows Hello](../active-directory/active-directory-azureadjoin-passport-deployment.md) sorunsuz tek oturum açma için Azure kaynakları için kullanılabilir. Bu durumda, Windows 10 cihaz ikinci faktörü olarak kimlik doğrulaması için kullanılır.

Azure aboneliğinizi yönetmek hesapları ve sanal makinelere oturum açarak hesapları için çok faktörlü kimlik doğrulaması kullanarak, yalnızca bir parola kullanmaktan daha büyük bir düzeyde güvenlik sağlar. İki öğeli kimlik doğrulama başka biçimlerde da çalışabilir, ancak değil zaten üretimde oldukları bunların dağıtımı karmaşık olabilir.

Aşağıdaki ekran görüntüsü bazı Azure çok faktörlü kimlik doğrulaması için kullanılabilir seçenekleri gösterir:

![Çok faktörlü kimlik doğrulama seçenekleri](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Sınırlayabilir ve yönetici erişimini kısıtlamak

Azure aboneliğinizi yönetmek hesaplarını güvenli hale getirme son derece önemlidir. Bu hesapların hiçbirini tehlikeye gizli kalmasını sağlamak için sürebilir diğer tüm adımlar değerini ve verilerinizin bütünlüğünü üzerindeki geçersiz kılar. Son olarak Resimli tarafından [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) iç saldırıları yönelik genel güvenlik tüm kuruluşlar için büyük bir tehdit.

Kişiler için yönetici hakları, bunlara benzer aşağıdaki ölçütlere göre değerlendirin:

- Bunlar, yönetici ayrıcalıkları gerektiren görevler gerçekleştiriyorsunuz?
- Ne sıklıkta görevler gerçekleştirilir?
- Şirket adına başka bir yönetici tarafından görevler neden gerçekleştirilemiyor belirli bir nedenle var mı?

Ayrıcalık ve neden her kabul edilebilir değil. verme için tüm diğer bilinen alternatif yaklaşımlar belge.

Tam zamanında yönetim kullanımını gereksiz yükseltilmiş haklara sahip hesaplar varlığını ne zaman bu hakları gerekmeyen dönemlerde engeller. Yöneticiler işlerini yapabilmesi için hesapları sınırlı bir süre için hakları yükseltilmiş. Ardından, bu hakları bir kaydırma veya bir görev tamamlandığında sonunda kaldırılır.

Kullanabileceğiniz [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) yönetmek için izleme ve Denetim erişim, kuruluşunuzda. Kuruluşunuzdaki kişiler gerçekleştirmesi eylemleri farkında kalır yardımcı olur. Aynı zamanda yalnızca zaman yönetimi için Azure AD uygun admins kavramı sunarak beraberinde getirir. Bu hesapları yönetici hakları verilmesi olanağına sahip olan bireylerdir. Kullanıcıların bu tür bir etkinleştirme işlemi boyunca gidebilir ve sınırlı bir süre için yönetici hakları verilmesi.


## <a name="use-devtest-labs"></a>DevTest Labs kullanın

Labs ve geliştirme ortamları için Azure kullanarak kuruluşlarının hemen donanım tedarik tanıtır gecikmeler gerçekleştirerek test ve geliştirme çevikliği kazanırsınız olanak tanır. Ne yazık ki, Azure veya kendi benimseme artırılmasına yardımcı olur isteği ile benzerlik eksikliği yönetici hakları atama ile aşırı izin neden olabilir. Bu riski kasıtsız olarak iç saldırıları kuruluşa doğurabilir. Bazı kullanıcılar sahip olması gereken daha çok daha fazla erişim izni.

[Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) hizmet kullanır [Azure rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC). RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gerekli erişim düzeyini vermek rollere görevlerini ekibiniz içinde ayırabilirsiniz. RBAC önceden tanımlı rollerle (sahibi, Laboratuvar kullanıcı ve katkıda bulunan) gelir. Dış iş ortakları hakları atamak ve büyük ölçüde işbirliği basitleştirmek için bu rolleri bile kullanabilirsiniz.

DevTest Labs RBAC kullandığından, bu ek, oluşturmak mümkündür [özel roller](../lab-services/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs yalnızca izinlerin yönetimini basitleştirir, sağlanan ortamları alma işlemini basitleştirir. Ayrıca, geliştirme ve test ortamları üzerinde çalıştığınız takım tipik diğer zorluklar uğraşmanız yardımcı olur. Bazı hazırlık gerektirir, ancak uzun vadede şeyler ekibiniz için kolaylaştırır.

Azure DevTest Labs özellikleri içerir:

- Kullanıcılar için kullanılabilir seçenekleri üzerinde yönetimsel denetim. Yönetici, izin verilen VM boyutlarını, VM'lerin ve VM'ler başlatıldığında en fazla ve kapatma gibi merkezi olarak yönetebilir.
- Otomasyon laboratuvar ortamı oluşturma.
- Maliyet İzlemesi.
- Geçici birlikte çalışma için basitleştirilmiş dağıtım VM'lerin.
- Self Servis, şablonları kullanarak kendi labs sağlamak kullanıcıların sağlar.
- Yönetme ve tüketimini sınırlamak.

![DevTest Labs kullanarak bir laboratuvar oluşturma](./media/azure-security-iaas/devtestlabs.png)

Ek ücret ödemeden DevTest Labs kullanımı ile ilişkilidir. Labs, ilkeleri, şablonları ve yapıları oluşturulmasını ücretsizdir. Yalnızca sanal makineler, depolama hesapları ve sanal ağlar gibi labs kullanılan Azure kaynakları için ücret ödersiniz.



## <a name="control-and-limit-endpoint-access"></a>Uç noktası erişim denetimi ve sınırı

Barındırma Labs veya Azure üretim sistemlerinde sistemlerinizi Internet'ten erişilebilir olmasını gerektiği anlamına gelir. Varsayılan olarak, yeni bir Windows sanal makine RDP bağlantı noktası Internet'ten erişilebilir içeriyor ve Linux sanal makine açık SSH bağlantı noktası içeriyor. Kullanıma sunulan sınırı Uç noktalara adımları alma, yetkisiz erişim riskini en aza indirmek gereklidir.

Azure teknolojileri, bu yönetim uç noktalarına erişimi sınırlamak yardımcı olabilir. Azure'da, kullandığınız [ağ güvenlik grubu](../virtual-network/security-overview.md) (Nsg'ler). Azure Resource Manager dağıtım için kullandığınızda Nsg'ler yalnızca yönetim uç (RDP veya SSH) tüm ağlardan erişimi sınırlayın. Nsg'ler düşünürken, yönlendirici ACL'ler düşünün. Sıkı bir şekilde Azure ağlarınızı çeşitli kesimleri arasındaki ağ iletişimi denetlemek için bunları kullanabilirsiniz. Bu, çevre ağı veya diğer yalıtılmış ağlarda ağlar oluşturmaya benzer. Trafiği incelemek değil, ancak ağ kesimleri ile yardımcı olurlar.


Azure'da, yapılandırdığınız bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) şirket içi ağınızdan. Bulut, şirket içi ağınıza bir siteden siteye VPN genişletir. Nsg'ler yerden erişim'den yerel ağ diğer izin vermeyecek şekilde de değiştirebilirsiniz çünkü bu, Nsg'ler, kullanılacak başka bir fırsatı verir. Ardından, yönetim ilk VPN aracılığıyla Azure ağa bağlanarak yapılır gerektirebilir.

Siteden siteye VPN seçeneğini burada şirket içi kaynaklarınızı Azure ile yakından tümleşik üretim sistemlerine barındırma durumlarda en cazip olabilir.

Alternatif olarak, kullanabileceğiniz [noktası site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) , şirket kaynaklarına erişimi gerekmeyen sistemleri yönetmek istediğiniz durumlarda seçeneği. Bu sistemlere kendi Azure sanal ağındaki yalıtılmış. Administrators can VPN Azure içine yönetimsel iş istasyonunda ortamından barındırılan.

>[!NOTE]
>Erişim Yönetimi uç noktaları için Internet'ten izin vermeyecek şekilde Nsg'ler ACL'lerin yapılandırılacağını ya da VPN seçeneğini kullanabilirsiniz.

Başka bir seçenek dikkate değer bir [Uzak Masaüstü Ağ Geçidi](../active-directory/authentication/howto-mfaserver-nps-rdg.md) dağıtım. Bu bağlantılar için daha ayrıntılı denetimler uygulanırken Uzak Masaüstü sunucularına HTTPS üzerinden güvenli bir şekilde bağlanmak için bu dağıtım kullanabilirsiniz.

Erişiminiz eklenecek özellikleri:

- Bağlantı isteklerini belirli sistemlerden sınırlamak için yönetici Seçenekleri'ni kullanın.
- Akıllı kart kimlik doğrulaması veya Azure çok faktörlü kimlik doğrulaması.
- Hangi sistemleri birisi yoluyla ağ geçidi bağlanabilir denetler.
- Diski ve aygıt yeniden yönlendirme denetler.

## <a name="use-a-key-management-solution"></a>Anahtar yönetimi çözümü kullanın

Güvenli anahtar yönetimi buluttaki verileri korumak için gereklidir. İle [Azure anahtar kasası](../key-vault/key-vault-whatis.md)küçük parolaları gibi ve güvenli bir şekilde şifreleme anahtarlarını saklamak parolalar donanım güvenlik modülleri (HSM'ler). Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz.

Microsoft işlemleri anahtarlarınızı FIPS 140-2, Düzey 2 doğrulanmış HSM'ler (donanım ve bellenim). Azure günlük ile izleme ve denetim anahtar kullanımı: Azure veya güvenlik bilgileri ve Olay yönetimi (SIEM) sistemine ek analiz ve tehdit algılama için günlükleri kanal.

Bir Azure aboneliği olan herhangi bir kişi oluşturabilir ve anahtar kasalarını kullanabilirsiniz. Anahtar kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, uygulanabilir ve bir kuruluştaki Azure hizmetleri yönetmek için sorumlu bir yönetici tarafından yönetiliyor.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Sanal diskleri ve disk depolamayı şifrelemek

[Azure Disk şifrelemesi](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) tehdit, veri hırsızlığı veya bir disk taşıyarak elde yetkisiz erişimden Etkilenme giderir. Diski başka bir sistem diğer güvenlik denetimlerini atlayarak bir yolu olarak bağlı. Disk şifrelemesi kullanır [BitLocker](https://technet.microsoft.com/library/hh831713) Windows ve Linux içindeki DM-crypt kullanan işletim sistemi ve veri sürücüleri şifrelemek için. Azure Disk şifrelemesi denetlemek ve şifreleme anahtarlarını yönetmek için anahtar kasası ile tümleşir. Premium depolama ile standart VM'ler ve VM'ler için kullanılabilir.

Daha fazla bilgi için bkz: [Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](azure-security-disk-encryption.md).

[Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md) REST, verilerinizi korumaya yardımcı olur. Depolama hesap düzeyinde etkinleştirilir. Bizim veri merkezlerinde yazılır ve siz eriştiğinizde otomatik olarak çözülür gibi verileri şifreler. Aşağıdaki senaryoları destekler:

- Şifreleme blok blobları, ekleme blobları ve sayfa BLOB'ları
- Şifreleme arşivlenmiş VHD'leri ve şirket içinden Azure'a duruma şablonları
- Temel işletim sistemi ve veri diskleri şifreleme Vhd'lerinizi kullanılarak oluşturulan Iaas VM'ler için

Azure depolama şifrelemesi ile devam etmeden önce iki sınırlamaları dikkat edin:

- Klasik depolama hesaplarında kullanılamaz.
- Yalnızca şifreleme etkinleştirildikten sonra yazılan verileri şifreler.

## <a name="use-a-centralized-security-management-system"></a>Bir Merkezi güvenlik yönetimi sistemi kullanın

Sunucularınızın düzeltme eki uygulama, yapılandırma, olayları ve güvenlik sorunlarının düşünülebilir etkinlikler için izlenmesi gerekir. Bu sorunları çözmek için kullanabileceğiniz [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Operations Management Suite güvenlik ve Uyumluluk](https://azure.microsoft.com/services/security-center/). Bu seçeneklerin ikisi de işletim sistemini yapılandırmada ötesine gidin. Ayrıca ağ yapılandırması ve sanal gereç kullanımı gibi temel altyapıyı yapılandırma izlenmesini sağlar.

## <a name="manage-operating-systems"></a>İşletim sistemlerini yönetme

Bir Iaas dağıtımında, ortamınızda hala dağıttığınız sistemlerinin, başka bir sunucuya veya iş istasyonu gibi yönetim sorumludur. Düzeltme eki uygulama, sağlamlaştırma, hakları ataması ve sisteminizin bakımıyla ilgili herhangi bir etkinlik hala sizin sorumluluğunuzdadır. Şirket içi kaynaklarınıza ile sıkı bir şekilde tümleşik sistemler için aynı araçları ve virüsten koruma ve kötü amaçlı yazılımdan koruma, düzeltme eki uygulama ve yedekleme gibi şeyler için şirket içi kullanmakta olduğunuz yordamları kullanmak isteyebilirsiniz.

### <a name="harden-systems"></a>Sistemleri sağlamlaştırmak
Yüklenen uygulamalar için gerekli olan hizmet uç kullanıma böylece Azure Iaas tüm sanal makinelerin sıkı. Windows sanal makineler için Microsoft için taban çizgisi olarak yayımlar önerilere uyun [güvenlik uyumluluk Yöneticisi](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) çözümü.

Güvenlik uyumluluk Yöneticisi ücretsiz bir araçtır. Hızlı bir şekilde yapılandırmak ve masaüstü bilgisayarlar, geleneksel veri merkezi ve özel ve genel bulut Grup İlkesi ve System Center Configuration Manager kullanarak yönetmek için kullanabilirsiniz.

Güvenlik uyumluluk Yöneticisi ilkeleri Dağıt hazır ve test edilmiş Desired Configuration Management yapılandırma paketleri sağlar. Bu taban çizgileri dayalı [Microsoft Güvenlik Kılavuzu](https://technet.microsoft.com/library/cc184906.aspx) öneriler ve endüstri en iyi uygulamalar. Yapılandırma değişikliklerini, adres uyumluluk gereksinimleri, yönetme ve güvenlik tehditlerini azaltılmasına yardımcı olur.

İki farklı yöntemler kullanarak bilgisayarlarınızın geçerli yapılandırma içeri aktarmak için güvenlik uyumluluk Yöneticisi'ni kullanabilirsiniz. İlk olarak, Active Directory tabanlı Grup İlkeleri aktarabilirsiniz. İkinci olarak, bir "altın ana" yapılandırmasını içeri aktarabilirsiniz kullanarak başvuru makinesini [LocalGPO aracı](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) yerel Grup İlkesi yedeklenir. Bu gibi durumlarda, yerel Grup İlkesi sonra güvenlik uyumluluk Yöneticisi içine aktarabilirsiniz.

Standartlarınıza endüstrideki en iyi uygulamaları için karşılaştırın, özelleştirin ve yeni ilkeler ve Desired Configuration Management yapılandırma paketleri oluşturun. Tüm desteklenen Windows 10 Anniversary Update ve Windows Server 2016 içeren işletim sistemi için taban çizgilerini yayımlandı.


### <a name="install-and-manage-antimalware"></a>Yükleme ve kötü amaçlı yazılımdan koruma yönetme

Üretim ortamınızdan ayrı olarak barındırılan ortamlarda, kötü amaçlı yazılımdan koruma uzantı, sanal makinelerinizi korumak ve bulut Hizmetleri için kullanabilirsiniz. İle tümleştirilir [Azure Güvenlik Merkezi](../security-center/security-center-intro.md).


[Microsoft Antimalware](azure-security-antimalware.md) gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, altyapı güncelleştirmeleri, örnekleri, dışlama olay toplama, raporlama gibi özellikler içerir ve [PowerShelldesteği](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure kötü amaçlı yazılımdan koruma](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>En son güvenlik güncelleştirmelerini yükleyin
Müşteriler için Azure taşırken ilk iş yüklerinin labs ve dışa dönük sistemleri bazılarıdır. Azure barındırılan sanal makinelerinizi uygulamalar veya Internet'e erişilmesi gereken hizmetler barındırıyorsanız, düzeltme eki uygulama hakkında temkinli olabilir. İşletim sisteminin ötesine geçen düzeltme eki. Üçüncü taraf uygulamalar yüklenmemiş güvenlik açıklarından de iyi düzeltme eki yönetimi yerinde olduğunda önlenebilir sorunlara neden olabilir.

### <a name="deploy-and-test-a-backup-solution"></a>Dağıtma ve bir yedekleme çözümüne sınama

Güvenlik güncelleştirmeleri olduğu gibi bir yedekleme başka bir işlem işleyecek şekilde ele alınması gerekir. Bu, üretim ortamınıza buluta genişletme parçası olan sistemlerinin geçerlidir. Test ve geliştirme sistemleri hangi kullanıcıların dayalı olarak, şirket içi ortamlarla deneyimlerini alışık için benzer bir geri yükleme özelliklerini sağlayın yedekleme stratejilerini izlemeniz gerekir.

Üretim iş yükleri için Azure taşınmış varolan yedekleme çözümleri ile mümkün olduğunda tümleştirmeniz. Veya, kullanabileceğiniz [Azure yedekleme](../backup/backup-azure-arm-vms.md) , yedekleme gereksinimlerinize gidermeye yardımcı olacak.


## <a name="monitor"></a>İzleme

[Güvenlik Merkezi](../security-center/security-center-intro.md) olası güvenlik açıklarını tanımlamak için Azure kaynaklarınızın güvenlik durumunu devam eden değerlendirilmesini sağlar. Gerekli denetimlerin yapılandırılması işlemi boyunca bir öneri listesi size rehberlik eder.

Örneklere şunlar dahildir:

- Tanımlamak ve kötü amaçlı yazılımları kaldırmanıza yardımcı olmak için kötü amaçlı yazılımdan koruma sağlama.
- Ağ güvenlik gruplarını ve sanal makinelere doğru trafiği denetlemek kurallarını yapılandırma.
- Web uygulamalarınızı hedefleyen saldırılara karşı korumaya yardımcı olmak için web uygulaması güvenlik duvarı sağlama.
- Eksik sistem güncelleştirmelerini dağıtma.
- Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma.

Aşağıdaki resimde bazı Güvenlik Merkezi'nde etkinleştirme seçenekleri gösterilmektedir.

![Azure Güvenlik Merkezi ilkeleri](./media/azure-security-iaas/security-center-policies.png)

[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan bir Microsoft bulut tabanlı BT yönetimi çözümüdür. Operations Management Suite bulut tabanlı bir hizmet olarak uygulandığı için hızlı ve en düşük yatırım altyapı kaynaklar ile dağıtılabilir.

Yeni özellikler otomatik olarak devam eden bakım kaydetme teslim edilir ve maliyetleri yükseltin. Operations Management Suite ayrıca System Center Operations Manager ile tümleştirilir. Daha iyi dahil olmak üzere yüklerinizi Azure yönetmenize yardımcı olmak için farklı bileşenlere sahip bir [güvenlik ve Uyumluluk](../operations-management-suite/oms-security-getting-started.md) modülü.

Kaynaklar hakkında bilgileri görüntülemek için Operations Management Suite güvenlik ve uyumluluk özellikleri kullanabilirsiniz. Bilgiler, dört ana kategoriye düzenlenmiştir:

- **Güvenlik etki alanları**: daha fazla güvenlik kayıtları zamanla keşfedin. Kötü amaçlı yazılım değerlendirmesi erişim, değerlendirme, ağ güvenlik bilgileri, kimlik ve erişim bilgileri ve bilgisayarlar güvenlik olayları ile güncelleştirin. Hızlı erişim için Azure Güvenlik Merkezi panosunu yararlanın.
- **Önem düzeyindeki sorunlar**: hızlı bir şekilde etkin sorunlar sayısı ve bu sorunların önem derecesini tanımlayın.
- **Algılama (Önizleme)**: saldırı tanımlamak, kaynaklarına karşı durum gibi güvenlik uyarıları görselleştirme tarafından desenleri.
- **Tehdit Intelligence**: saldırı tanımlamak giden kötü amaçlı IP trafik, kötü amaçlı tehdit türünü ve burada bu IP'leri geliyor gelen gösteren bir harita sunucularıyla toplam sayısı görselleştirme tarafından desenleri.
- **Ortak Güvenlik sorguları**: listesi ortamınızı izlemek için kullanabileceğiniz en yaygın güvenlik sorgular için bkz. Bu sorguları birine tıkladığınızda **arama** dikey penceresi açılır ve bu sorgu için sonuçları gösterir.

Aşağıdaki ekran görüntüsünde Operations Management Suite görüntüleyebilirsiniz bilgileri örneği gösterilmektedir.

![Operations Management Suite güvenlik temelleri](./media/azure-security-iaas/oms-security-baseline.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md)
