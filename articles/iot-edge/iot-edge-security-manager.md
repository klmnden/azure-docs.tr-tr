---
title: Azure IOT Edge Güvenlik Yöneticisi | Microsoft Docs
description: IOT Edge cihaz güvenlik tutum sergilemek ve güvenlik hizmetleri bütünlüğünü yönetir.
services: iot-edge
keywords: Güvenlik, öğe, kuşatma, IOT Edge
author: eustacea
manager: timlt
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4594685291396b8b80e62abe57be109f0abbd81d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126352"
---
# <a name="azure-iot-edge-security-manager"></a>Azure IOT Edge Güvenlik Yöneticisi

Azure IOT Edge güvenlik IOT Edge cihazı ve tüm bileşenlerini güvenli Silikon donanım özetleyen tarafından korunması için iyi sınırlanmış güvenlik çekirdek yöneticisidir. Bu, güvenliğin sağlamlaştırılmasını için odak noktasıdır ve özgün cihaz üreticileri (OEM) teknolojisi tümleştirme noktası sağlar.

![Azure IOT Edge Güvenlik Yöneticisi](media/edge-security-manager/iot-edge-security-manager.png)

IOT Edge Güvenlik Yöneticisi'ni IOT Edge cihazı ve tüm yazılım devralınan işlemler bütünlüğünü korumak amaçlar.  Bunu güven donanım temel alınan donanım kök güveni (varsa) güvenli bir IOT Edge çalışma zamanı bootstrap ve işlemlerini bütünlüğünü izlemek devam etmek için geçiş yapar.  IOT Edge Güvenlik Yöneticisi'ni temelde kullanılabiliyorsa güvenli Silikon donanım ile birlikte çalışan yazılım oluşur ve olası en yüksek güvenlik güvencesi sağlamak amacıyla etkin.  

(Ancak bunlarla sınırlı değil) IOT Edge Güvenlik Yöneticisi'nin sorumlulukları şunlardır:

* Güvenli ve Azure IOT Edge cihazı önyükleme ölçülür.
* Cihaz kimlik sağlama ve güven geçişin uygun olduğunda.
* Ana bilgisayar ve cihaz sağlama hizmeti gibi bulut hizmetlerinin bileşenleri cihaz koruyun.
* IOT Edge modülleri benzersiz kimlik modülü ile güvenli bir şekilde sağlayın.
* Ağ geçidi cihaz donanım kök güven notary Hizmetleri aracılığıyla.
* IOT Edge çalışma zamanı operace bütünlüğünü izler.

IOT Edge Güvenlik Yöneticisi başlıca üç bileşenden oluşur:

* IOT Edge güvenlik arka plan programı.
* Donanım güvenlik modülü platform Soyutlama Katmanı (HSM PAL).
* İsteğe bağlıdır, ancak donanım Silikon kök güveni veya HSM kesinlikle önerilir.

## <a name="the-iot-edge-security-daemon"></a>IOT Edge güvenlik arka plan programı

IOT Edge güvenlik arka plan programı IOT Edge Güvenlik Yöneticisi mantıksal işlemleri için sorumlu bir yazılımdır.  Bu, IOT Edge cihazı güvenilir bilgi işlem tabanı önemli bir bölümü oluşur. 

### <a name="design-principles"></a>Tasarım ilkeleri

Kendi sorumluluklarını, IOT Edge güvenlik arka plan programı dengesi ve neredeyse gerçekleştirmek için aşağıdaki temel ilkeler ile uyum sağlamak için amaçlar:

#### <a name="maximize-operational-integrity"></a>İşletimsel bütünlüğü en üst düzeye çıkarın

IOT Edge güvenlik arka plan programı, verilen tüm kök güven donanım defense yeteneğini içinde olası en yüksek bütünlüğü ile çalışmak üzere tasarlanmıştır. Uygun Tümleştirmesi ile güven donanım kök ölçer ve güvenlik arka plan programı statik ve izinsiz kullanıma karşı dayanıklılık zamanında izler.

Fiziksel erişim her zaman bir tehdit IOT cihazlarına genel olarak.  Donanım kök güven, bu nedenle IOT Edge güvenlik daemon bütünlüğünü savunma önemli bir rol oynar.  Donanım kök güven gelen iki model olarak sağlanır:

* Gizli dizileri ve şifreleme anahtarları gibi hassas bilgilerin korunması için güvenli öğeleri.
* Gizli dizileri koruması için güvenli enclaves anahtarları ve ölçüm ve faturalandırma gibi hassas iş yükleri gibi.

Donanım kök güven kullanan için bu iki modeli varlığını ortaya çıkmasına neden yürütme ortamlarını iki çeşit sunar:

* Hassas bilgileri korumak için güvenli öğeleri kullanımını kullanan standart ya da zengin Yürütme Ortamı (REE).
* Hassas bilgileri korumak ve yazılım yürütme koruma sunmak için güvenli kuşatma teknolojisinin kullanımı kullanan güvenilir Yürütme Ortamı (TEE).

Cihazlar için donanım güven kökü olarak güvenli enclaves kullanarak IOT Edge güvenlik arka plan programı hassas mantık kuşatma içinde bulunan beklenir.  Hassas olmayan güvenlik arka plan programı bölümlerini dışında TEE bulunabilir.  Her iki durumda da, özgün tasarım üreticileri (ODM) ve orijinal donanım üreticileri (OEM) ölçmek ve kenar güvenlik arka plan programı önyükleme ve çalışma zamanı bütünlüğü korumak için HSM gelen güven genişletmek için bekleniyor.

#### <a name="minimize-bloat-and-churn"></a>Şişirme en aza indirmek ve değişim sıklığı

Başka bir çekirdek kenar güvenlik arka plan programı için karmaşıklığı en aza indirmek için ilkesidir.  En yüksek güven düzeyi için IOT Edge güvenlik arka plan programı sıkı bir şekilde uygun olduğu durumlarda cihaz donanım kök güven ile birleştirin ve yerel kod çalışır.  Bu tür, belirli donanım ve dağıtım senaryosuna bağlı olarak zorlu olabilir arka plan programı yazılım güven'ın güvenli güncelleştirme yollarını (aksine, sağlanan işletim sistemi güncelleştirme mekanizmaları), donanım kök yoluyla güncelleştirmek için realizations yaygındır.  Güvenlik yenileme IOT cihazları için güçlü bir öneri olsa da, birçok yönden tehdit surface aşırı güncelleştirme gereksinimleri veya büyük güncelleştirme yüklerini genişletebilirsiniz nedeni anlamına gelir.  İşletimsel kullanılabilirliği en üst düzeye çıkarmak için güncelleştirmeleri atlanıyor verilebilir veya kök güven donanım çok büyük bir güncelleştirme yükü işlemek için kısıtlanmış.  Bu nedenle, IOT Edge güvenlik arka plan programı tasarımını ayak izini tutmak için kısa ve bu nedenle güvenilir bilgi işlem temel küçük ve güncelleştirme gereksinimlerini en aza indirmek için.

### <a name="architecture-of-iot-edge-security-daemon"></a>IOT Edge güvenlik arka plan programı mimarisi

![Azure IOT Edge güvenlik arka plan programı](media/edge-security-manager/iot-edge-security-daemon.png)

IOT Edge güvenlik arka plan programı, tüm kullanılabilir donanım kök güvenliğin sağlamlaştırılmasını için güven teknolojinin avantajlarından yararlanmak için geliştirilmiştir.  Bu nedenle daha güvenilir yürütme ortamlarındaki (TEE) donanım teknolojilerden yararlanmak için bir standart/zengin Yürütme Ortamı (REE) ve güvenilir bir yürütme ortamı (TEE) arasında bölünmüş dünya işlem için izin vermek için geliştirilmiştir.  IOT Edge güvenlik daemon mimarisi çekirdek, cihaz ve işlemlerini IOT Edge bütünlüğünü sağlamak için Edge ana bileşenleri etkileşim özelliği etkinleştirmek için role özgü arabirimler ' dir.

#### <a name="cloud-interface"></a>Bulut arabirimi

Cihaz güvenliği gibi güvenlik yenileme için bulut tebrik gibi bulut hizmetlerine erişmek güvenlik daemon IOT Edge bulut arabirimi sağlar.  Örneğin, IOT Edge güvenlik arka plan programı şu anda bu arabirimi Azure IOT hub'ı erişmek için kullandığı [cihaz sağlama hizmeti (DPS)](https://docs.microsoft.com/azure/iot-dps/) cihaz kimlik yaşam döngüsü yönetimi.  

#### <a name="management-api"></a>Yönetim API'si

IOT Edge güvenlik arka plan programı sunar, Edge Aracısı oluşturma/başlatma/durdurma/kaldırırken tarafından çağrılan bir yönetim API'si, edge modülü. IOT Edge güvenlik arka plan programı, tüm etkin modüller için "kayıtları" depolar. Bu kayıtları bir modülün kimlik modülü bazı özelliklerine eşlenir. Bu özelliklerin birkaç örnek kapsayıcı ya da docker kapsayıcının içeriğinin karma çalışan işlemin işlem tanımlayıcısını (PID) verilmiştir.

Bu özellikler iş yükü API (çağıran bir eylem gerçekleştirme yetkisi gerçekleştiğini doğrulamak için aşağıda açıklanan) kullanılır.

Yalnızca IOT Edge Aracısı'ndan çağrılabilir ayrıcalıklı bir API Management API'dir.  IOT Edge güvenlik daemon bootstraps ve IOT Edge Aracısı'nı başlatır olduğundan, IOT Edge Aracısı ile değiştirilmemiş olduğunu Kanıtlanmamış sonra IOT Edge aracısı için örtük bir kaydı oluşturabilirsiniz. İş yükü API kullandığı aynı kanıtlama işlemi yalnızca IOT Edge Aracısı yönetim API erişimi sınırlamak için kullanılır.

#### <a name="container-api"></a>Kapsayıcı API

IOT Edge güvenlik arka plan programı Moby ve Docker gibi kullanılan modül örneklemesi için kapsayıcı sistem ile etkileşimde bulunmak için kapsayıcı arabirimi sunar.

#### <a name="workload-api"></a>İş yükü API

API IOT Edge güvenlik daemon API IOT Edge Aracısı dahil olmak üzere tüm modüller için erişilebilir iş yüküdür. Bir HSM biçiminde kimlik kanıtı sağlar imzalı belirteç veya X509 kök sertifikası ve karşılık gelen güven paket için bir modül. Güven paket tüm modülleri'nın güvenmeli diğer sunucular için CA sertifikaları içerir.

IOT Edge güvenlik arka plan programı, bu API koruma sağlamak için bir kanıtlama işlemi kullanır. Bir modül, bu API çağrısı, IOT Edge güvenlik arka plan programı kimliği için bir kayıt bulmaya çalışır. Başarılı olursa, modül ölçmek için kayıt özelliklerini kullanır. Yeni bir HSM kayıt kökü ölçümü işlem eşleşmeleri sonucunu belirteci veya X509 sertifika oluşturulur ve karşılık gelen açtıysanız CA sertifikaları (güven paket) modülü döndürülür.  Modül bir sunucuyu başlatın veya diğer modüller, IOT Hub'ına bağlanmak için bu sertifikayı kullanır. İmzalı bir belirteç ya da sertifika süre sonu yaklaştığında, yeni bir sertifika istemek için modülün sorumluluğundadır.  Ek hususlar kimlik yenileme işlemini mümkün olduğunca sorunsuz hale getirmek için içine dahil edilir.

### <a name="integration-and-maintenance"></a>Tümleştirme ve Bakım

Microsoft'un ana kod tabanı [GitHub üzerinde IOT Edge güvenlik daemon](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Yükleme ve güncelleştirme

Yerel bir süreci, yükleme ve güncelleştirme IOT Edge güvenlik daemon yönetilir işletim sisteminin paket yönetim sistemi.  Ancak, beklenen, IOT cihazları ile ilgili donanım kök güven kenar ve onun yaşam döngüsü cihazları Güvenli Önyükleme ve güncelleştirmeleri yönetim sistemleri ile yöneterek arka plan programı bütünlüğünü için ek sağlamlaştırma.  İlgili cihaz yeteneklerini uygun olarak bu seçenek keşfetmek için cihazları yetkililerine bırakılır.

#### <a name="versioning"></a>Sürüm oluşturma

IOT Edge çalışma zamanı izler ve IOT Edge güvenlik daemon sürümü bildiriyor. Sürüm olarak bildirilir *runtime.platform.version* özniteliği Edge Aracısı modülünün bildirilen özellik.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Donanım güvenlik modülü platform Soyutlama Katmanı (HSM PAL)

Tüm kök geliştirici veya IOT Edge kullanıcıdan miktarı yalıtmak için güven donanım HSM PAL soyutlar.  Bu, bir birleşimi uygulama programlama arabirimi (API) ve trans etki alanı iletişim yordamları, örneğin standart yürütme ortamı ile güvenli bir kuşatma arasındaki iletişimi oluşur.  HSM PAL gerçek uygulaması belirli güvenli donanımı bağlıdır.  Var IOT ekosisteminde neredeyse tüm güvenli Silikon donanım kullanımını etkinleştirir.

## <a name="secure-silicon-root-of-trust-hardware"></a>Silikon kök güven donanım güvenliğini sağlama

Güvenli Silikon, IOT Edge cihaz donanım içinde yer işareti güven için gereklidir.  Silikon güvenli, Güvenilir Platform Modülü (TPM), katıştırılmış güvenli öğesi (eSM), ARM Trustzone, Intel SGX ve özel güvenli Silikon teknolojileri dahil etmek için çeşitli gelir.  Fiziksel olarak IOT cihazları erişilebilirlikle ilgili tehditleri verilen güvenli Silikon kök güven cihazlarında kullanımını önemle tavsiye edilir.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>IOT Edge Güvenlik Yöneticisi tümleştirmesi ve bakımı

Bir ana amacı, IOT Edge Güvenlik Yöneticisi, kimliği için ve güvenliğini ve bütünlüğünü özel sağlamlaştırma için Azure IOT Edge platformunda savunma ile görevli bileşenleri yalıtır.  Bu nedenle, üçüncü tarafların gibi cihaz üreticileri yapmak için beklenen kullanım, özel güvenlik özellikleri cihaz donanım ile kullanılabilir.  Azure IOT Güvenlik Yöneticisi ile Güvenilir Platform Modülü (TPM) Linux ve Windows platformlarında sağlamlaştırma konusunda örneğe bağlantılarında aşağıdaki sonraki adımlar bölümüne bakın.  Bu örnekler, yazılım veya sanal TPM'ler kullanır ancak ayrık TPM cihazları kullanarak doğrudan geçerli.  

## <a name="next-steps"></a>Sonraki adımlar

Blogu okuyun [akıllı uç güvenliğini sağlama](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

İle bir Edge cihazı oluşturma ve sağlama bir [Linux sanal makinesinde sanal TPM](how-to-auto-provision-simulated-device-linux.md).

Oluşturma ve sağlama bir [Windows sanal TPM Edge cihazında](how-to-auto-provision-simulated-device-windows.md).

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/
