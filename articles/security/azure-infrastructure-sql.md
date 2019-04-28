---
title: Azure SQL veritabanı güvenlik özellikleri
description: Bu makalede, Azure SQL veritabanı azure'da müşteri verilerini nasıl koruduğu genel bir açıklamasını sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: cd2ad16f910f5d2b3b801c8d54e9df7660751462
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62121665"
---
# <a name="azure-sql-database-security-features"></a>Azure SQL veritabanı güvenlik özellikleri    
Azure SQL veritabanı, azure'da bir ilişkisel veritabanı hizmetidir. Müşteri verilerinin korunmasına ve müşterilerin bir ilişkisel veritabanı hizmetine beklediğiniz güçlü güvenlik özellikleri sağlamak için SQL veritabanı kendi güvenlik özellikleri kümesi vardır. Bu özellikler, Azure'dan devralınan denetimler üzerine oluşturun.

## <a name="security-capabilities"></a>Güvenlik özellikleri

### <a name="usage-of-the-tds-protocol"></a>TDS protokolünün kullanımı
Azure SQL veritabanı, veritabanı, yalnızca varsayılan bağlantı noktası, TCP/1433 üzerinden erişilebilir olmasını gerektiren yalnızca tablo veri akışı (TDS) protokolü, destekler.

### <a name="azure-sql-database-firewall"></a>Azure SQL veritabanı güvenlik duvarı
Müşteri verilerinin korunmasına yardımcı olmak için aşağıda gösterildiği gibi SQL veritabanı sunucusuna tüm erişimi engeller ve varsayılan bir güvenlik duvarı işlevselliği Azure SQL veritabanı'nı içerir.

![Azure SQL veritabanı güvenlik duvarı][1]

Gateway güvenlik duvarı adresleri müşteriler kabul edilebilir IP adreslerinin aralıklarını belirtmek ayrıntılı denetim sağlayan sınırlayabilirsiniz. Güvenlik Duvarı her isteğin kaynak IP adresine göre erişim verir.

Müşteriler, güvenlik duvarı yapılandırması yönetim portalını kullanarak veya Azure SQL veritabanı yönetimi REST API'sini kullanarak program aracılığıyla elde edebilirsiniz. Azure SQL veritabanı ağ geçidi Güvenlik Duvarı varsayılan olarak, Azure SQL veritabanı örnekleri için tüm müşteri TDS erişimi engeller. Müşteriler erişim erişim denetim listeleri (ACL'ler) kullanarak Azure SQL veritabanı bağlantılara izin vermek için kaynak ve hedef internet adresleri, protokoller ve bağlantı noktası numaralarını yapılandırmanız gerekir.

### <a name="dosguard"></a>DoSGuard
Hizmet (DoS) saldırısı reddi DoSGuard adlı bir SQL veritabanı ağ geçidi hizmeti tarafından sınırlı. DoSGuard, IP adreslerini başarısız oturum açma bilgileri etkin bir şekilde izler. Bir süre içinde birden çok başarısız oturum açma bilgilerinden belirli bir IP adresi varsa, IP adresi hizmetinde önceden tanımlı bir süre için tüm kaynaklara erişimi engellenir.

Ayrıca, Azure SQL veritabanı ağ geçidi gerçekleştirir:

- Veritabanı sunucularına bağlandığında TDS FIPS 140-2 uygulamak için güvenli kanal özelliği anlaşmaları şifrelenmiş bağlantılar doğrulandı.
- Durum bilgisi olan TDS paket inceleme, istemci bağlantılarını kabul eder. Ağ geçidi bağlantı bilgilerini doğrular ve bağlantı dizesinde belirtilen veritabanı adına göre uygun fiziksel sunucu TDS paketlerde geçirir.

Ipam'da Azure SQL veritabanı teklifini ağ güvenliği için yalnızca bağlantı ve çalışmasına izin verecek şekilde gerekli iletişimine izin vermek için ilkesidir. Varsayılan olarak, tüm diğer bağlantı noktaları, protokoller ve bağlantı engellenir. Sanal yerel alan ağları (VLAN'lar) ve ACL'ler ağ iletişimi kısıtlamak için kaynak ve hedef ağlar, protokoller ve bağlantı noktası numaralarını tarafından kullanılır.

Ağ tabanlı bir ACL uygulamak için onaylanmış mekanizmaları bulunan yönlendiriciler ACL'leri içerir ve yük Dengeleyiciler. Bu mekanizmalar, Azure ağı, Konuk VM Güvenlik Duvarı ve müşteri tarafından yapılandırılmış olan Azure SQL veritabanı ağ geçidi güvenlik duvarı kuralları tarafından yönetilir.

## <a name="data-segregation-and-customer-isolation"></a>Veri ayırma ve müşteri yalıtımı
Azure üretim ağı genel olarak erişilebilir sistem bileşenleri iç kaynaklardan arkadaşlarından şekilde yapılandırılmıştır. Genel kullanıma yönelik Azure portalına erişim sağlayan web sunucuları arasında müşteri uygulama örnekleri ve Müşteri verilerinin bulunduğu Azure sanal altyapının, fiziksel ve mantıksal sınırları mevcut.

Genel olarak erişilebilir olan tüm bilgileri Azure üretim ağı içinde yönetilir. Üretim ağı iki öğeli kimlik doğrulama ve sınır koruma mekanizması, önceki bölümde açıklanan ve sonraki bölümde belirtildiği gibi veri yalıtımı işlevleri kullanan güvenlik duvarı ve güvenlik özellik kümesini kullanır.

### <a name="unauthorized-systems-and-isolation-of-the-fc"></a>Yetkisiz sistemleri ve FC yalıtma
Yapı denetleyicisi (FC) merkezi orchestrator Azure yapısı olduğundan, önemli, özellikle müşteri uygulamaları içinde riskli olabilecek FAs gelen tehditleri azaltmak için bir yerde denetimlerdir. FC, cihaz bilgileri (örneğin, MAC adresi) içinde FC önceden yüklü olmadığından herhangi bir donanım algılamaz. FC DHCP sunucularında MAC adresleri önyükleme söylersiniz düğümlerin listesini yapılandırdınız. Yetkisiz sistemleri bağlı olsa da, bunlar değil fabric stoğa alınabilir ve bu nedenle değil bağlı veya doku envanteri içindeki herhangi bir sistemiyle iletişim kurmak için yetkili. Bu yetkisiz sistemleri FC ile iletişim kurmasını ve VLAN ve Azure'a erişmesini riskini azaltır.

### <a name="vlan-isolation"></a>VLAN yalıtımı
Azure üretim ağı, üç birincil VLAN mantıksal olarak ayrılır:

- Ana VLAN: Güvenilmeyen müşteri düğümleri eşitliyor.
- FC VLAN: Güvenilen FCs ve destekleyici sistemlere içerir.
- Cihaz VLAN: Güvenilen ağ ve diğer altyapı cihazları içerir.

### <a name="packet-filtering"></a>Paket filtreleme
IPFilter ve kök işletim sistemi ve konuk işletim sistemi düğümlerinin uygulanan yazılım güvenlik duvarları bağlantı kısıtlamalarını zorla ve sanal makineler arasında yapılan izinsiz trafiği engeller.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hiper yönetici, kök işletim sistemi ve Konuk Vm'leri
' % S'kök işletim sistemi Konuk Vm'lerden ve Konuk Vm'leri birbirinden yalıtılmasını hiper yöneticisine ve kök işletim sistemi tarafından yönetilir.

### <a name="types-of-rules-on-firewalls"></a>Güvenlik duvarı kurallarında türleri
Bir kural olarak tanımlanır:

{Güvenlik Yanıt Merkezi (Src) IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası, hedef Protokolü daraltma veya genişletme, durum bilgisi olan/olmayan, durum bilgisi olan akış zaman aşımı}.

Yalnızca kuralları herhangi biri izin veriliyorsa boş karakter (SYN) zaman uyumlu paketlerine veya izin verilir. TCP için Azure İlkesi, tüm SYN olmayan paketleri içine veya dışına VM izin verdiğini olduğu durum bilgisi olmayan kuralları kullanır. Güvenlik içi istediğiniz konak yığına SYN olmayan yoksayılıyor SYN paketi daha önce görmediği durumunda dayanıklı olmasıdır. TCP protokolü, durum bilgisi olan ve durum bilgisi olmayan SYN tabanlı kural birlikte genel bir durum bilgisi olan bir uygulama davranışını elde eder.

Kullanıcı Veri Birimi Protokolü (UDP), Azure, durum bilgisi olan kural kullanır. UDP paket bir kuralla eşleşen her zaman bir ters akış diğer yönde oluşturulur. Bu akış, yerleşik bir zaman aşımı vardır.

Müşteriler, Azure tarafından sağlanan korumanın en üstünde, kendi güvenlik duvarları ayarlamak için sorumludur. Burada müşteriler gelen ve giden trafik için kuralları tanımlayabilirsiniz.

### <a name="production-configuration-management"></a>Üretim yapılandırma yönetimi
Standart güvenli yapılandırmalar Azure'da ve Azure SQL veritabanı ilgili operasyon ekibi tarafından korunur. Tüm yapılandırma değişiklikleri üretim sistemlerine belgelenmiş ve merkezi izleme sistemi üzerinden izlenir. Yazılım ve donanım değişiklikleri merkezi izleme sistemine izlenir. ACL ilişkili ağ değişiklikleri bir ACL yönetim hizmeti kullanılarak izlenir.

Azure'a tüm yapılandırma değişiklikleri geliştirilen ve hazırlık ortamında test ve bundan sonra üretim ortamına dağıtılır. Yazılım yapıları test bir parçası olarak gözden geçirilir. Güvenlik ve gizlilik denetimleri giriş denetim listesi kriterleri bir parçası olarak gözden geçirilir. Değişiklikler, ilgili dağıtım ekibi tarafından zamanlanan aralıklarla dağıtılır. Yayınları gözden geçirdi ve üretim ortamına dağıtılmadan önce ilgili dağıtım takım personeli tarafından imzalanmış devre dışı.

Değişiklikleri başarı için izlenir. Başarısızlık senaryosunda, değişiklik, önceki durumuna geri alınır veya onay atanan personel hatasıyla yönelik bir düzeltme dağıtılır. Kaynak deposu, Git, TFS, Master Data Services (MDS), çalıştırıcılar, Azure güvenlik izleme, FC ve WinFabric platform merkezi olarak yönetin, uygulamak ve Azure sanal ortamında yapılandırma ayarlarını doğrulamak için kullanılır.

Benzer şekilde, donanım ve ağ değişiklikleri doğrulama adımlarını, kendi yapı gereksinimleri bağlılığın değerlendirmek için oluşturulur. Yayınları gözden geçirdi ve yığın üzerinde bir Eşgüdümlü değişiklik danışma kurulu ilgili grupları (CAB) yetkili.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
