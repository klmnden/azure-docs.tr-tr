---
title: Azure SQL veritabanı güvenlik özellikleri
description: Bu makale, Azure SQL veritabanı genel bir açıklamasını Azure müşteri verileri koruyan sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: cca8febb004029b13b0df09a047da701c4528e8e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102737"
---
# <a name="microsoft-azure-sql-database-security-features"></a>Microsoft Azure SQL veritabanı güvenlik özellikleri    
Microsoft Azure SQL veritabanı Azure'daki bir ilişkisel veritabanı hizmeti sağlar. Müşteri verileri korumak ve müşteriler bir ilişkisel veritabanı hizmetinden beklediğiniz güçlü güvenlik özellikleri sağlamak için SQL veritabanı güvenlik özellikleri kendi kümesi vardır. Bu özellikler Azure kaynağından devralındı denetimleri sırasında oluşturun.

## <a name="security-capabilities"></a>Güvenlik özellikleri

### <a name="usage-of-tabular-data-stream-tds-protocol"></a>Tablo verisi akışı (TDS) protokolü kullanımı
Microsoft Azure SQL veritabanı yalnızca veritabanının yalnızca varsayılan bağlantı noktası, TCP/1433 üzerinden erişilebilir olmasını gerektirir TDS protokolü destekler.

### <a name="microsoft-azure-sql-database-firewall"></a>Microsoft Azure SQL veritabanı güvenlik duvarı
Müşterilerin verilerin korunmasına yardımcı olmak için Microsoft Azure SQL veritabanı, varsayılan olarak aşağıda gösterildiği gibi SQL veritabanı sunucusuna tüm erişimi engelleyen bir güvenlik duvarı işlevselliği içerir.

![Azure SQL veritabanı güvenlik duvarı][1]

Ağ geçidi Güvenlik Duvarı'nı kabul edilebilir IP adreslerinin aralıklarını belirtmek için ayrıntılı bir denetim müşterilere izin vermeyi adresleri sınırlama yeteneği sağlar. Güvenlik Duvarı'nı her isteğin kaynak IP adresine göre erişim verir.

Güvenlik duvarı yapılandırması, bir Yönetim Portalı veya program aracılığıyla Microsoft Azure SQL veritabanı yönetimi REST API'si kullanılarak gerçekleştirilebilir. Varsayılan olarak Microsoft Azure SQL veritabanı ağ geçidi Güvenlik Duvarı'nı Microsoft Azure SQL veritabanları için tüm müşteri TDS erişimi engeller. Kaynak ve hedef Internet adresleri, protokoller ve bağlantı noktası numaralarını tarafından Microsoft Azure SQL veritabanı bağlantılara izin vermek için ACL'leri kullanarak erişim yapılandırılması gerekir.

### <a name="dosguard"></a>DoSGuard
Hizmet Reddi (DoS) saldırıları DoSGuard adlı bir SQL veritabanı ağ geçidi hizmeti tarafından azaltılır. DoSGuard etkin olarak IP adreslerini başarısız oturum açma bilgileri izler. Bir süre içinde belirli bir IP adresinden gelen birden çok başarısız oturum açma girişimi varsa, IP adresi ön tanımlı bir süre boyunca hizmetindeki herhangi bir kaynağa erişimi engellendi.

Yukarıdakilerin yanı sıra Microsoft Azure SQL veritabanı ağ geçidi de gerçekleştirir:

- Güvenli kanal yetenek anlaşmaları TDS FIPS 140-2 uygulamak için veritabanı sunucularına bağlanırken şifreli bağlantıları doğrulandı.
- İstemcilerden gelen bağlantıları kabul sırasında durum bilgisi olan TDS paket incelemesi. Ağ geçidi bağlantı bilgilerini doğrular ve bağlantı dizesinde belirtilen veritabanı adına göre uygun fiziksel sunucu TDS paketlerde geçirir.

Microsoft Azure SQL veritabanı sunumun ağ güvenliği için kapsayıcı ilkesini yalnızca bağlantı ve Hizmeti'nin çalışmasına izin vermek gerekli olan iletişimi izin vermektir. Varsayılan olarak, tüm diğer bağlantı noktaları, protokoller ve bağlantı engellenir. VLAN'ları ve ACL'leri, ağ iletişimleri tarafından kaynak ve hedef ağlara, protokoller ve bağlantı noktası numaralarını kısıtlamak için kullanılır.

Ağ tabanlı ACL'ler uygulamak için onaylanan düzenekler içerir: yönlendiriciler ve yük dengeleyici üzerindeki ACL'lerin. Bu, Azure ağı, Konuk VM Güvenlik Duvarı'nı ve müşteri tarafından yapılandırılan Microsoft Azure SQL veritabanı ağ geçidi güvenlik duvarı kuralları tarafından yönetilir.

## <a name="data-segregation-and-customer-isolation"></a>Veriler arasında ayrım yapma ve müşteri yalıtımı
Azure üretim ağı, genel olarak erişilebilir sistem bileşenleri iç kaynaklardan yinelenmeli şekilde yapılandırılmıştır. Genel kullanıma yönelik Azure Portalı'nı ve müşteri uygulama örnekleri ile müşteri verilerinin bulunduğu temel Azure sanal altyapı, erişim sağlayan web sunucuları arasında fiziksel ve mantıksal sınırları mevcut.

Tüm genel olarak erişilebilir bilgileri Azure üretim ağı içinde yönetilir. Üretim ağı iki öğeli kimlik doğrulama ve sınır koruma mekanizmalarını tabidir, güvenlik duvarı ve güvenlik özellik önceki bölümde açıklanan kümesini kullanır ve aşağıda belirtildiği gibi verileri yalıtım işlevlerini kullanır.

### <a name="unauthorized-systems-and-isolation-of-fc"></a>Yetkisiz sistemleri ve FC yalıtımı
Microsoft Azure doku merkezi orchestrator FC olduğuna göre önemli tehditlerinden, özellikle müşteri uygulamalar içindeki riskli SK'lar azaltmak yerinde denetimleridir. FC, aygıt bilgileri (örneğin, MAC adresi) içinde FC önceden yüklenmediğini herhangi bir donanım tanımıyor. FC DHCP sunucularında önyükleme istekli düğümlerinin MAC adresleri listesi yapılandırdınız. Yetkisiz sistemleri bağlı olsa da, bunlar değil doku stoğa birleştirilmiş ve bu nedenle değil bağlı veya doku stok içinde herhangi bir sistemiyle iletişim kurmak için yetkili. FC ile iletişim kurmasını ve VLAN ve Azure erişmesini yetkisiz sistemleri riskini azaltır.

### <a name="vlan-isolation"></a>VLAN yalıtımı
Azure üretim ağı üç birincil VLAN mantıksal olarak ayrılır:

- Ana VLAN – bağlantılar güvenilmeyen müşteri düğümler
- FC VLAN – güvenilen FCs ve sistemlerini destekleyen içerir
- VLAN – cihaz güvenilir ağ ve diğer altyapı aygıtları içerir

### <a name="packet-filtering"></a>Paket filtreleme
IPFilter ve kök işletim sistemi ve konuk işletim sistemi düğümlerin uygulanan yazılım güvenlik duvarları bağlantı kısıtlamalarını zorla ve VM'ler arasındaki yetkisiz trafiğini engelleyebilirsiniz.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hiper yönetici, kök işletim sistemi ve Konuk VM'ler
Yalıtım Konuk sanal makineleri ve Konuk sanal makineleri birbirinden kök işletim sistemi, hiper yönetici ve kök işletim sistemi tarafından yönetilir.

### <a name="types-of-rules-on-firewalls"></a>Güvenlik duvarı kurallarının türleri
Bir kural olarak tanımlanır:

{Güvenlik Yanıt Merkezi (Src) IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası, hedef protokolü, giriş/çıkış, durum bilgisi olan/durum bilgisiz, durum bilgisi olan akış zaman aşımı}.

Yalnızca kuralları herhangi biri veriyorsa Eşitlemeye paketleri içeri veya dışarı izin verilir. TCP için Microsoft Azure ilke, yalnızca tüm Eşitlemeye olmayan paketleri VM giriş / çıkış izin verdiğini olduğu durum bilgisiz kurallarını kullanır. Tüm ana bilgisayar yığınına Eşitlemeye paket daha önce görmediği varsa Eşitlemeye olmayan yoksayılıyor esnek güvenlik dayanır. Kural durum bilgisiz SYNbased ile birlikte bir durum bilgisi olan bir uygulama genel davranışını elde eder ve TCP iletişim durum bilgisi olan kuralıdır.

Kullanıcı Veri Birimi Protokolü (UDP) için Microsoft Azure durum bilgisi olan kural kullanır. UDP paket bir kuralla eşleşiyorsa her zaman, bir geriye doğru akış ters yönde oluşturulur. Bu akış yerleşik bir zaman aşımı vardır.

Müşteriler, Microsoft Azure sağladıkları en üstünde, kendi güvenlik duvarları ayarlamak için sorumludur. Burada müşteriler gelen ve giden trafik için kuralları tanımlayabilir.

### <a name="production-configuration-management"></a>Üretim yapılandırma yönetimi
Standart güvenli yapılandırmalar, Azure ve Microsoft Azure SQL veritabanı ilgili işletim ekipleri tarafından korunur. Üretim sistemlerine tüm yapılandırma değişikliklerini belgelenen ve merkezi izleme sistemi üzerinden izlenir. Yazılım ve donanım değişiklikleri merkezi izleme sistemi üzerinden izlenir. ACL ilgili ağ değişiklikleri ACL Yönetim Hizmeti (AMS) kullanılarak izlenir.

Microsoft Azure tüm yapılandırma değişikliklerini geliştirilen ve hazırlama ortamında test; ve bundan sonra üretim ortamında dağıtılmış. Yazılım derlemeleri, test bir parçası olarak incelenen. Güvenlik ve gizlilik denetimleri giriş denetim listesi ölçütünün bir parçası incelenen. Değişiklikleri zamanlanan aralıklarla ilgili dağıtım ekibi tarafından dağıtılır. Sürümleri gözden ve üretim ortamına dağıtılmadan önce ilgili dağıtım takım personeli tarafından imzalanmış devre dışı.

Değişiklikleri başarı için izlenir. Bir hata senaryosu değişikliktir önceki durumuna geri alındı veya bir düzeltme onay belirlenen personelinin hatayla adres dağıtılır. Kaynak deposu, Git, TFS, MDS, koşucular, Azure güvenlik izleme (ASM), FC ve WinFabric platform merkezi olarak yönetmek, uygulayın ve Azure sanal ortamda yapılandırma ayarlarını doğrulamak için kullanılır.

Benzer şekilde, donanım ve ağ değişiklikleri yapı gereksinimleri bunların bağlılığı değerlendirmek için doğrulama adımlarını oluşturulur. Sürümleri gözden ve yığını arasında bir Eşgüdümlü değişiklik danışma Panosu (CAB aracılığıyla) ilgili gruplarının yetkili.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure Information sistem bileşenleri ve sınırlar](azure-infrastructure-components.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
