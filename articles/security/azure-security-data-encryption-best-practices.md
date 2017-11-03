---
title: "Veri güvenliği ve şifreleme en iyi uygulamalar | Microsoft Docs"
description: "Bu makalede veri güvenliği için en iyi yöntemler kümesi sağlar ve şifreleme kullanılarak Azure özellikleri."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: 81136e53756adfdba2f07c103b042499fe2967db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Azure veri güvenliği ve şifreleme en iyi uygulamalar
Veri koruma bulutta anahtarlarından birini verilerinizi ortaya çıkabilecek ve bu durum için hangi denetimlerin kullanılabilir olası durumlar için hesap. Amacıyla Azure veri güvenlik ve şifreleme en iyi uygulamaları önerileri aşağıdaki verilerinin durumları geçici bir çözüm olacaktır:

* : Çalışmıyorken Bu depolama nesneleri, kapsayıcıları ve statik olarak fiziksel medyada mevcut türleri manyetik veya optik disk olması tüm bilgileri içerir.
* Aktarım sırasında: Ne zaman veri bileşenleri, konumlara veya ağ bir hizmet veri yolu (Başlangıç, bulut şirket içi ve ExpressRoute gibi karma bağlantılar dahil olmak üzere tersi,) programları gibi üzerinde arasında ya da bir giriş/çıkış işlemi sırasında aktarıldığı , da olduğu düşünülen hareket halinde.

Bu makalede Azure data güvenlik ve şifreleme en iyi uygulamaları koleksiyonu aşağıdakiler ele alınacaktır. Bu en iyi uygulamaları, Azure veri güvenliği, şifreleme ve kendiniz gibi müşterilerin deneyimleri bizim deneyimlerden türetilir.

En iyi her uygulama için açıklayacağız:

* En iyi uygulama nedir
* Bu en iyi uygulama etkinleştirmek istediğiniz neden
* En iyi uygulama olarak etkinleştirmek başarısız olursa ne sonucu olabilir
* En iyi uygulama için olası alternatifler
* Nasıl en iyi uygulama olarak etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure veri güvenliği ve şifreleme en iyi yöntemler makalesi anlaşma fikir ve Azure platformu özellikleri ve özellik kümeleri dayanır. Zaman içinde görüşlerini ve teknolojileri değiştirebilirsiniz ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure veri güvenlik ve şifreleme en iyi uygulamalar şunlardır:

* Çok faktörlü kimlik doğrulamasını zorunlu
* Kullanım rol tabanlı erişim denetimi (RBAC)
* Azure virtual machines şifreleme
* Donanım güvenlik modelleri kullanma
* Güvenli iş istasyonları ile yönetme
* SQL veri şifrelemeyi etkinleştir
* Aktarımdaki verileri korumak
* Dosya düzeyinde veri şifrelemeyi zorunlu kılma

## <a name="enforce-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını zorunlu
İlk adımda veri erişimi ve Microsoft Azure denetiminde kullanıcının kimliğini etkinleştirmektir. [Azure multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) yalnızca bir kullanıcı adı ve parola'den başka bir yöntem kullanarak kullanıcının kimliğini doğrulayan bir yöntemdir. Bu kimlik doğrulama yöntemi yardımcı erişimi korumaya veri ve uygulamalara basit bir oturum açma işlemi için kullanıcı talebine toplantı oluştu.

Kullanıcılarınız için Azure MFA etkinleştirerek, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı ekliyorsunuz. Bu durumda, bir işlem bir dosya sunucusunda veya, SharePoint Online'da bulunan bir belge erişiyor. Ayrıca, Azure MFA yardımcı BT güvenliği aşılmış bir kimlik bilgisi kuruluşunuzun veri erişimi olmasını olasılığını azaltmak için.

Örneğin: kullanıcılarınız için Azure MFA zorlamak ve kullanıcının kimlik bilgileri aşılıp aşılmadığını bir telefon araması veya kısa mesaj doğrulaması kullanacak şekilde yapılandırın, saldırganın kendisinin kullanıcının telefonuna erişemeyecektir beri herhangi bir kaynağa erişim mümkün olmayacaktır. Bu ek kimlik koruması katmanı eklemeyin kuruluşlar veri güvenliğinin aşılmasına neden kimlik bilgisi hırsızlığı saldırısına daha açıktır.

Kimlik doğrulama denetimi içi tutmak istediğiniz kuruluşlar için bir alternatif kullanmaktır [Azure çok faktörlü kimlik doğrulama sunucusu](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), MFA şirket içi olarak da bilinir. Bu yöntemi kullanarak çok faktörlü kimlik doğrulaması, MFA sunucusu şirket içi korurken zorlayabilir devam edersiniz.

Azure MFA hakkında daha fazla bilgi için lütfen makaleyi okuyun [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Kullanım rol tabanlı erişim denetimi (RBAC)
Temelinde erişimi kısıtlayabilirsiniz [bilmeniz](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri. Bu, veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için zorunludur. Azure rol tabanlı erişim denetimi (RBAC), kullanıcılar, gruplar ve uygulamalar belirli bir kapsamda izinleri atamak için kullanılabilir. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir.

Yararlanabileceğiniz [yerleşik RBAC rolleri](../active-directory/role-based-access-built-in-roles.md) ayrıcalıkları kullanıcılara atamak için azure'da. Kullanmayı *depolama hesabı katkıda bulunan* depolama hesaplarını yönetmek için gereken bulut operatörleri için ve *Klasik depolama hesabı katkıda bulunan* Klasik depolama hesaplarını yönetmek için rol. Sanal makineleri ve depolama hesabı yönetmesi gereken bulut operatörleri için onlara eklemeyi düşünün *sanal makine Katılımcısı* rol.

Veri erişim denetimi RBAC gibi özellikler yararlanarak zorlamaz kuruluşlar kendi kullanıcıları için gerekenden daha fazla ayrıcalık vermiş. Bazı kullanıcılar ilk başta olmamalıdır veri erişmesini sağlayarak bu verileri güvenliğinin aşılmasına neden olabilir.

Makaleyi okuyarak Azure RBAC hakkında daha fazla bilgiyi [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Azure Virtual Machines şifreleme
Çoğu kuruluş için [bekleyen verileri şifreleme](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizliliği, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır. Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine (VM) diskleri şifrelemek BT yöneticilerine sağlar. Azure Disk şifrelemesi endüstri standart BitLocker özelliği, Windows ve Linux işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için DM-Crypt özelliği yararlanır.

Koruma ve Kuruluş güvenliği ve uyumluluk gereksinimleri karşılamak için verilerinizi korumaya yardımcı olmak için Azure Disk şifrelemesi yararlanabilirsiniz. Kuruluşlar, ayrıca riskleri ilgili yetkisiz veri erişimi azaltmaya yardımcı olmak için şifreleme kullanmayı düşünmeniz gerekir. Ayrıca, bunlara hassas verileri yazma önce sürücüleri şifreleme önerilir.

Azure depolama hesabınızdaki kalan verileri korumak için VM veri birimleri ve önyükleme birimi şifrelemek emin olun. Şifreleme anahtarları ve gizli anahtarları yararlanarak koruma [Azure anahtar kasası](../key-vault/key-vault-whatis.md).

Şirket içi Windows sunucuları için en iyi yöntemler aşağıdaki şifreleme göz önünde bulundurun:

* Kullanım [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) için veri şifreleme
* Kurtarma bilgileri AD DS'de depolar.
* BitLocker anahtarları tehlikeye, ya da sürücüdeki tüm örneklerini BitLocker meta verileri kaldırmak için sürücüyü biçimlendirmek öneririz veya şifresini çözmek ve sürücünün tamamını yeniden şifrelemek herhangi bir sorun varsa.

Veri şifrelemeyi zorunlu olmayan kuruluşlar, veri hırsızlığı kötü amaçlı veya standart dışı kullanıcılar gibi veri bütünlüğü sorunları maruz kalabilir olasılığı daha yüksektir ve Temizle biçimindeki verileri yetkisiz erişim elde hesapları tehlikeye. Bu riskleri yanı sıra endüstri düzenlemelerle uyumlu olması kullanan şirket gerekir kanıtlamak dikkatli ve veri güvenliğini artırmak için doğru güvenlik denetimleri kullanarak.

Azure Disk şifrelemesi hakkında daha fazla makalesini okuyarak bilgi [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Donanım güvenlik modülleri kullanma
Endüstri şifreleme çözümleri gizli anahtarlarına verileri şifrelemek için kullanın. Bu nedenle, bu anahtarları güvenli şekilde depolanan önemlidir. Anahtar Yönetimi, verileri şifrelemek için kullanılan gizli anahtarlarını depolamak için de olduğundan veri koruması'nın ayrılmaz bir parçası haline gelir.

Azure disk şifrelemesi kullanan [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) denetlemek ve disk şifreleme anahtarları ve gizli anahtar kasası aboneliğinizde yönetmenize yardımcı olmak için while Azure bekleyen sanal makine disklerdeki tüm veriler şifrelenir sağlama depolama alanı. Anahtarları ve ilke kullanımı denetlemek için Azure anahtar kasası kullanmanız gerekir.

Uygun güvenlik denetimleri, verileri şifrelemek için kullanılan gizli anahtarların korunmasına yerinde olmaması için ilgili birçok devralınmış riskleri vardır. Saldırganlar gizli anahtarlarına erişimi varsa, verilerin şifresini çözmek ve gizli bilgilere erişme potansiyeline sahip olacaktır.

Azure sertifika yönetimi için genel öneriler hakkında daha fazla makalesini okuyarak bilgi [Azure sertifika yönetimi: ilgilenmenin](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Azure anahtar kasası hakkında daha fazla bilgi için okuma [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Güvenli iş istasyonları ile yönetme
Son kullanıcı saldırılar çoğunluğu hedef olduğundan uç nokta saldırı birincil noktalarından birini haline gelir. Bir saldırgan uç nokta etkilediğinde, kendisinin kuruluşunuzun verilerini erişmek için kullanıcı kimlik bilgilerini yararlanabilirsiniz. Çoğu uç nokta saldırıları son kullanıcıların kendi yerel iş istasyonlarını Yöneticiler bulunmasına yararlanamazsınız.

Güvenli yönetim iş istasyonu kullanarak bu riskleri azaltabilirsiniz. Kullanmanızı öneririz bir [ayrıcalıklı erişim iş istasyonları (PENÇE)](https://technet.microsoft.com/library/mt634654.aspx) iş istasyonları saldırı yüzeyini azaltmak için. Bu güvenli yönetim iş istasyonları, bunlardan bazıları azaltmaya yardımcı olabilir saldırıları Yardım verilerinizi daha güvenli olduğundan emin olun. Sağlamlaştırmak ve iş istasyonunuzu kilitlemek için PENÇE kullandığınızdan emin olun. Bu, yüksek güvenlik çıkışların hassas hesapları, görevleri ve veri koruması sağlamak için önemli bir adımdır.

Endpoint Protection olmaması, verilerinizi riske, verileri veri konumu (Bulut veya şirket içi) bağımsız olarak kullanmak için kullanılan tüm cihazlar arasında güvenlik ilkelerini zorlamak emin olun.

Ayrıcalıklı hakkında daha fazla erişim iş istasyonu makalesini okuyarak öğrenebilirsiniz [ayrıcalıklı erişimi güvenli hale getirme](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL veri şifrelemeyi etkinleştir
[Azure SQL veritabanında saydam veri şifreleme](https://msdn.microsoft.com/library/dn948096.aspx) (TDE), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemelerinizi gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur ve işlem günlüğü dosyalarını REST uygulamasında yapılacak değişiklikler gerektirir.  TDE, veritabanının tamamını Depolama veritabanı şifreleme anahtarını adlı bir simetrik anahtar kullanarak şifreler.

Tüm depolama bile şifreli olduğunda, aynı zamanda, veritabanınızı şifrelemek çok önemlidir. Bu veri koruması derinliği yaklaşımda savunma uygulamasıdır. Kullanıyorsanız [Azure SQL veritabanı](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) ve kredi kartı veya sosyal güvenlik numarası gibi hassas verileri korumak istiyorsanız, birçoğu gereksinimlerini karşılayan 140-2 doğrulanmış 256 bit AES şifreleme FIPS veritabanlarıyla şifreleyebilirsiniz endüstri standartları (örn., HIPAA, PCI).

Dosyaları ile ilgili anlaşılması önemlidir [arabellek havuzu genişletme](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) TDE kullanarak bir veritabanı şifreli olduğunda şifrelenmez. BitLocker gibi dosya sistemi düzeyinde şifreleme araçları kullanmanız gerekir veya [şifreleme dosya sistemi](https://technet.microsoft.com/library/cc700811.aspx) (EFS) dosyaları için BPE ilgili.

Yetkili bir kullanıcı bu yana veritabanı ile TDE, şifreli olsa bile bir güvenlik yöneticisi veya bir veritabanı yöneticisi verilere erişebilirsiniz gibi aşağıdaki önerileri de izlemelisiniz:

* Veritabanı düzeyinde SQL kimlik doğrulaması
* RBAC rollerini kullanarak azure AD kimlik doğrulaması
* Kullanıcılar ve uygulamalar ayrı hesaplarının kimliğini doğrulamak için kullanmanız gerekir. Bu şekilde kullanıcılar ve uygulamalar için izinler sınırlayabilir ve kötü amaçlı etkinliği riskleri azaltın
* Uygulama veritabanı düzeyi güvenlik (örneğin, db_datareader veya db_datawriter) sabit veritabanı rollerinin veya kullanarak seçili veritabanı nesnelerini açık izinleri vermek, uygulamanız için özel rolleri oluşturabilirsiniz

Veritabanı düzeyinde şifreleme kullanılarak olmayan kuruluşlar daha açıktır. SQL veritabanlarında bulunan verilere tehlikeye atabilir saldırıları için olabilir.

SQL TDE'nin şifreleme hakkında daha fazla makalesini okuyarak bilgi [saydam veri şifrelemesi ile Azure SQL veritabanı](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Aktarımdaki verileri korumak
Aktarımdaki verileri koruma temel veri koruma stratejinizin parçası olmalıdır. Verileri geri ve İleri birçok konumlardan taşıma olduğundan, genel, her zaman SSL/TLS protokolleri farklı konumlar arasında veri değişimi için kullanmanız önerilir. Bazı durumlarda, şirket içi ve bulut arasındaki tüm iletişim kanalını ayırmak isteyebilirsiniz sanal özel ağ (VPN) kullanarak altyapı.

Şirket içi altyapınızı ve Azure arasında taşıma verileri için HTTPS veya VPN gibi uygun güvenlik önlemleri göz önünde bulundurmalısınız.

Azure birden çok iş istasyonlarında bulunan şirket içi erişimi güvenli hale getirmek için gereken kuruluşlar için kullanmak [Azure siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Azure bulunan bir iş istasyonu şirket içi erişimi güvenli hale getirmek için gereken kuruluşlar için kullanmak [noktası siteye VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Büyük veri kümeleri taşınabilir ayrılmış bir yüksek hızlı WAN bağlantısı üzerinden gibi [ExpressRoute](https://azure.microsoft.com/services/expressroute/). ExpressRoute kullanmayı seçerseniz, ayrıca uygulama düzeyi kullanarak verileri şifreleyebilir [SSL/TLS](https://support.microsoft.com/kb/257591) veya diğer protokoller için ek koruma.

Azure Portalı aracılığıyla Azure Storage ile etkileşim, tüm işlemleri HTTPS oluşur. [Storage REST API'sini](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS de kullanılabilir ile etkileşim kurmasına üzerinden [Azure Storage](https://azure.microsoft.com/services/storage/) ve [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/).

Aktarımdaki verileri korumak için başarısız olan kuruluşlar için daha açıktır [man-in--middle saldırıları](https://technet.microsoft.com/library/gg195821.aspx), [gizli dinleme](https://technet.microsoft.com/library/gg195641.aspx) ve oturumu ele geçirme. Bu tür saldırıları gizli verilere erişmesini ilk adımı olabilir.

Azure VPN seçeneği hakkında daha fazla makalesini okuyarak bilgi [planlama ve tasarım VPN ağ geçidi için](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Dosya düzeyinde veri şifrelemeyi zorunlu kılma
Bir başka verileriniz için güvenlik düzeyini artırabilirsiniz koruma katmanı dosya konumuna bakılmaksızın dosyasının kendisini, şifreleme.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) dosyalarınızın ve e-posta güvenli hale getirmek için şifreleme, kimlik ve yetkilendirme ilkelerini kullanır. Azure RMS birden çok cihazda çalışır — telefonları, Tablet ve PC'leri hem kuruluşunuz içinde hem de kuruluşunuz dışındaki koruma tarafından. Azure RMS bile, kuruluşunuzun sınırları dışına çıktığında veriler devam koruma düzeyini eklediğinden bu olası bir yetenektir.

Dosyalarınızı korumak için Azure RMS kullandığınızda, endüstri standardı şifreleme ile tam destek kullanmakta olduğunuz [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Veri koruma için Azure RMS yararlanan, denetimi altında olmayan depolama kopyaladığınız olsa dahi koruma dosyayla beraber kalır güvence sahip bir bulut depolama hizmeti gibi BT'nin. Aynı oluştuğunda e-posta ile paylaşılan dosyaları dosya yönergeleri içeren bir e-posta eki olarak korunur korumalı ekin nasıl açılacağına dair.

Azure RMS benimseme için planlama yaparken şunları öneririz:

* Yükleme [RMS sharing uygulaması](https://technet.microsoft.com/library/dn339006.aspx). Bu uygulama tarafından bir Office Eklentisi-kullanıcıların kolayca dosyaları doğrudan koruyabilmeniz için Office uygulamalarıyla ile tümleşir.
* Uygulamaları ve Hizmetleri Azure RMS'yi destekleyecek şekilde yapılandırma
* Oluşturma [özel şablonlar](https://technet.microsoft.com/library/dn642472.aspx) iş gereksinimlerinizi yansıtır. Örneğin: tüm üst gizliliği uygulanması gereken üst gizli veriler için bir şablon ilgili e-postaları.

Üzerinde zayıf kuruluşlar [veri sınıflandırması](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ve dosya koruması veri sızıntısını daha açıktır. Uygun dosya koruma kuruluşlar iş Öngörüler elde, uygunsuz kullanım için izleyebilir ve kötü amaçlı erişimi engellemek için dosyaları mümkün olmayacaktır.

Azure RMS hakkında daha fazla makalesini okuyarak bilgi [Azure Rights Management ile çalışmaya başlama](https://technet.microsoft.com/library/jj585016.aspx).
