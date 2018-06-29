---
title: Azure Information sistem bileşenleri ve sınırlar
description: Bu makalede Microsoft Azure Mimarisi ve Yönetim genel bir açıklamasını sağlar.
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
ms.openlocfilehash: 8db1dce5fcc56c229d1fdd746bafbd2fae2c9bad
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102709"
---
# <a name="azure-information-system-components-and-boundaries"></a>Azure Information sistem bileşenleri ve sınırlar
Bu makalede Microsoft Azure Mimarisi ve Yönetim genel bir açıklamasını sağlar. Azure sistem ortamındaki aşağıdaki ağlar oluşur:

- Microsoft Azure üretim ağı (Azure ağı)
- Microsoft Corporate ağ (Corpnet ağ)

İşlemler ve Bakım Azure ağ ve CorpNet ağların için ayrı BT ekiplerinin sorumludur.

## <a name="azure-architecture"></a>Azure Mimarisi
Microsoft Azure platformu ve altyapı oluşturma, dağıtma ve yönetme uygulamalar ve hizmetler için veri merkezleri Microsoft tarafından yönetilen bir ağ üzerinden bulut ' dir. Müşteriler tarafından belirtilen kaynak sayısına bağlı olarak, Azure kaynak gereksinimleri temelinde VM'ler oluşturur. Bu sanal makineleri için tasarlanmış bir Microsoft Azure hiper yöneticide çalıştırmak bulutta kullanın ve ortak erişilebilir değil.

Her Azure fiziksel sunucu düğümünde doğrudan donanımın üzerinde çalışır bir hiper yönetici yoktur. Hiper yönetici Konuk sanal makineleri (VM'ler) değişken sayıda bir düğüm böler. Her düğümü de bir özel "Kök" ana bilgisayar işletim sistemi çalıştıran VM vardır. Her VM üzerinde Windows Güvenlik Duvarı etkin. Yalnızca bu bağlantı noktalarını açın ve adreslenebilir, dahili olarak veya harici olarak açıkça müşteri tarafından yapılandırılan hizmet tanımı dosyasında tanımlanan bağlantı noktalarıdır. Tüm trafiği ve disk ve ağ erişimi mediated kök işletim sistemi ve hiper yönetici tarafından.

Ana bilgisayar katmanındaki sorumluyu Azure Vm'lerinin en son Windows Server özelleştirilmiş ve sağlamlaştırılmış bir sürümünü çalıştırın. Microsoft Azure yalnızca sanal makineleri ana bilgisayar için gerekli bileşenleri içeren Windows Server stripped-down sürümünü kullanır. Bu, performansı artırmak ve saldırı yüzeyini azaltmak için gerçekleştirilir. Makine sınırları üzerinde işletim sistemi güvenliğinin bağımlı değil hiper yönetici tarafından uygulanır.

**Azure Yönetimi yapı denetleyicilerine (FCs) tarafından**: Azure içinde fiziksel sunucuları (Kanatlar/düğümler) üzerinde çalışan sanal makineleri yaklaşık 1000 "kümeler" halinde gruplandırılır. Sanal makineleri bağımsız olarak FC adlı ölçeklendirilmiş ve yedek platform yazılım bir bileşen tarafından yönetilir.

Her FC yaşam döngüsü, küme ve hükümleri içinde çalışan uygulamaların yönetir ve kendi denetimi altındaki donanım durumunu izler. Bir sunucu başarısız olduğunu belirlediğinde, VM örnekleri sağlıklı sunucularda reincarnating gibi her iki autonomic işlemlerini yürütür. FC de dağıtma, güncelleştirme ve uygulamaları ölçeklendirme gibi uygulama yönetimi işlemleri gerçekleştirir.

Veri Merkezi kümeler halinde ayrılmıştır. Kümeleri FC düzeyinde hataları yalıtmak ve hataların belirli sınıfları içinde gerçekleşme küme ötesinde sunucuları etkilemesini önleyin. Belirli bir Azure Küme hizmet FCs FC küme gruplandırılır.

**Donanım envanterini**: Azure donanım ve ağ aygıtları envanterini önyükleme yapılandırma işlemi sırasında hazır ve datacenter.xml yapılandırma dosyasında belgelenmiştir. Herhangi bir yeni donanım ve ağ bileşenleri Azure üretim ortamına girme önyükleme yapılandırma işlemi izlemeniz gerekir. FC datacenter.xml yapılandırma dosyasında listelenen tüm envanteri yönetilmesinden sorumludur.

**FC yönetilen OS**: işletim sistemi takım sağlar, sanal sabit diskler (VHD) form dağıtılan işletim sistemi görüntülerini tüm ana bilgisayar ve Konuk VM'ler Azure üretim ortamında. İşletim sistemi takım bunlar otomatik çevrimdışı derleme sürecinde "Temel görüntüleri" oluşturur. Base görüntüsü, işletim sistemi, çekirdek ve diğer çekirdek bileşenlerini değiştiren ve yapılmadığını Azure ortamı desteklemek için en iyi duruma getirilmiş sürümüdür.

Doku yönetilen işletim sistemi görüntüleri üç tür vardır:

- Ana bilgisayar işletim sistemi – konak işletim sistemi konak Vm'lerinde çalışan özelleştirilmiş bir işletim sistemi değil.
- Yerel OS – tüm hiper olmayan kiracılar (örneğin, Azure Storage) çalıştıran yerel işletim sistemi
- Konuk işletim sistemi – Konuk Konuk VM'ler üzerinde çalıştırılan işletim sistemi

Ana bilgisayar ve işletim sistemleri yerel FC yönetilen bulutta kullanılmak üzere tasarlanmış ve genel olarak erişilebilir değil.

**Ana bilgisayar ve yerel OS**: ana bilgisayar işletim sistemi ve yerel işletim sistemi sıkı işletim sistemi görüntülerini barındıran doku aracıları (FA) ve bir işlem düğümünde (düğüm üzerinde ilk VM olarak çalışır) ve depolama düğümleri üzerinde çalıştırın. Kullanmanın yararları, temel görüntü konak en iyi duruma getirilmiş ve yerel OS API'leri veya yüksek güvenlik riskleri sunmak ve işletim sisteminin ayak artırmak kullanılmayan bileşenleri tarafından kullanıma sunulan yüzey alanını azaltır olur. Bu sınırlı ayak işletim sistemleri, yalnızca Azure için gerekli bileşenleri içerir. Bu performansı artırır ve saldırı yüzeyini azaltır.

**Konuk işletim sistemi**: Konuk işletim sistemi Vm'lerinde çalışan Azure iç bileşenleriniz Uzak Masaüstü Protokolü (RDP) dış müşterileri çalıştırmak için fırsat. Taban çizgisi yapılandırma ayarlarına yapılan değişiklikler değişikliğin üzerinden dönün ve yönetim işlemi serbest olması gerekir.

## <a name="azure-datacenters"></a>Azure veri merkezleri
Microsoft bulut altyapısı ve işlemleri (MCIO) ekibin Microsoft'un fiziksel altyapı ve datacenter olanaklar tüm Microsoft online Services yönetir. MCIO öncelikle veri merkezleri içinde fiziksel ve ortam denetimleri yönetme yanı sıra yönetme ve dış çevre ağ aygıtlarını (sınır yönlendiricileri ve Datacenter yönlendiriciler) destekleyen sorumludur. MCIO, veri merkezi rafları tam minimum sunucu donanımında ayarlamak için sorumludur. Müşterilerin doğrudan etkileşimi Azure ile sahip.

## <a name="service-management--service-teams"></a>Hizmet Yönetimi & hizmet takımları
Azure hizmet desteği birtakım 'Hizmeti takımlar' bilinen mühendislik grupları tarafından yönetilir. Her hizmet ekibi Azure desteği alanı sorumludur. Her hizmet takım araştırmak ve hizmet hatalarını çözümlemek için bir mühendislik kullanılabilir 24 x 7 yapmanız gerekir. Hizmet takımlar varsayılan olarak, Azure'da çalışan donanımına fiziksel erişim sahip değilse.

Hizmet takımlar şunlardır:

- Uygulama Platformu
- Azure Active Directory
- Azure İşlem
- Azure ağı
- Bulut mühendislik Hizmetleri
- ISSD: güvenlik
- Çok Faktörlü Kimlik Doğrulama
- SQL Veritabanı
- Depolama

## <a name="types-of-users"></a>Kullanıcıların türleri
Tüm Azure iç kullanıcılar erişimleri müşteri verilerini (erişim veya erişimi yok) tanımlayan hassasiyet düzeyine sahip kategorilere çalışan durumlarına sahiptir. Çalışanlar (veya Yükleniciler) Microsoft iç kullanıcılar olarak kabul edilir. Diğer tüm kullanıcıların dış kullanıcılar olarak kabul edilir. Kullanıcı ayrıcalıkları Azure (kimlik doğrulaması gerçekleştikten sonra yetkilendirme izni) aşağıdaki tabloda açıklanmıştır:

| Rol | İç veya dış | Duyarlılık düzeyi | Yetkili ayrıcalıkları ve gerçekleştirilen İşlevler | Erişim türü
| --- | --- | --- | --- | --- |
| Azure veri merkezi mühendislik | İç | Müşteri verileri için erişim yok | Şirket içi fiziksel güvenliğinin yönetme; Veri merkezi ve patrols yürütün ve tüm giriş noktalarını izleyin; Escort Hizmetleri içine ve dışına (yemek, temizleme) genel hizmetler veya veri merkezi içinde BT iş sağlar belirli temizlenmiş olmayan datacenter personeli gerçekleştirir; Rutin izleme ve Bakım ağ donanımı gerçekleştir; Olay yönetimi ve çeşitli araçları kullanarak onarım iş gerçekleştirir; Rutin izleme ve veri merkezleri fiziksel donanım Bakımı gerçekleştir; Özellik sahipleri isteğe bağlı ortam erişim. Olay raporu günlüğü adli incelemelere gerçekleştirmek ve zorunlu güvenlik eğitim & ilkesi gereksinimlerini gerektiren özellikli; İşletimsel sahipliği ve tarayıcılar ve günlük toplama gibi kritik güvenlik araçlarını Bakımı. | Kalıcı erişim ortamı |
| Microsoft Azure olay önceliklendirme (hızlı yanıt mühendisleri) | İç | Müşteri verilerine erişim | Altyapı işlemleri, destek ve Azure mühendislik ekipleri arasında iletişimi yönetmek; Önceliklendirme platform olayları, dağıtım sorunlarını ve hizmet istekleri. | Yalnızca zaman içinde ortamına erişim - müşteri olmayan sistemleri için sınırlı sürekli erişime sahip |
| Microsoft Azure dağıtım mühendisleri | İç | Müşteri verilerine erişim | Dağıtım/yükseltmeler platform bileşenleri, yazılım ve Microsoft Azure desteklemek zamanlanmış yapılandırma değişiklikleri gerçekleştirin. | Yalnızca zaman içinde ortamına erişim - müşteri olmayan sistemleri için sınırlı sürekli erişime sahip |
| Microsoft Azure müşteri kesinti desteği (Kiracı) | İç | Müşteri verilerine erişim | Hata ayıklama ve tek tek işlem kiracılar ve Microsoft Azure hesapları için platform kesintileri ve hataları tanılamak; Hataları çözümleyin ve platform/müşteriye, sürücü teknik geliştirmeleri destek arasında önemli düzeltmelerin sürücü. | Yalnızca zaman içinde ortamına erişim - müşteri olmayan sistemleri için sınırlı sürekli erişime sahip |
| Microsoft Azure Canlı Site mühendisleri (mühendisleri izleme) & olayı | İç | Müşteri verilerine erişim | Tanılama ve tanılama araçlarını kullanarak platform durumu Azaltıcı sorumlu; Düzeltmeler için birim sürücüleri sürücü, kesintilere karşı kaynaklanan öğeleri onarın ve kesinti geri yükleme eylemlerini yardımcı. | Yalnızca zaman içinde ortamına erişim - müşteri olmayan sistemleri için sınırlı sürekli erişime sahip |
|Müşteriler Microsoft Azure | Dış | Yok | Yok | Yok |

Azure benzersiz tanımlayıcıları tüm varlıklar/Azure ortamının bir parçası olan cihazlar için kuruluş kullanıcılarının ve müşterilerin (veya kurumsal kullanıcılar adına hareket işlemleri) kimliğini doğrulamak için kullanır.

**Microsoft Azure iç kimlik doğrulaması**: Azure iç bileşenler arasındaki iletişim, TLS şifrelemesiyle korunur. Çoğu durumda, X.509 sertifikalarını otomatik olarak imzalanan. Özel durumlar ve FCs Azure Ağ dışından gelen erişilemedi bağlantıları sertifikalarla için yapılır. FCs Microsoft Certificate, yetkili (güvenilen bir kök CA tarafından desteklenen CA) tarafından verilen sertifikalara sahip. Bu, kolayca geri alınması FC ortak anahtarlar sağlar. Ayrıca, böylece geliştiriciler yeni uygulama görüntüsü gönderdiğinde, bunlar tüm katıştırılmış gizli korumak için bir FC ortak anahtarıyla şifrelenir FC ortak anahtarlar Microsoft geliştirici araçları tarafından kullanılır.

**Microsoft Azure donanım aygıtı kimlik doğrulaması**: FC, denetimi altındaki çeşitli donanım aygıtları için kendi kimliğini doğrulamak için kullanılan kimlik bilgileri kümesi (anahtarlar ve/veya parolaları) korur. Taşıma, kalıcı yapma ve bu kimlik bilgilerini kullanmak için kullanılan sistem hassas, gizli veya özel bilgiler Azure geliştiricileri, Yöneticiler ve yedekleme hizmetleri/personel erişimi engellemek için tasarlanmıştır.

Ağ donanım cihazlarına erişmek için kullanılan kimlik bilgileri aktarmak için ortak anahtar FC Kurulum ve FC yeniden yapılandırma sırasında kullanılan FC'ın ana kimliği temelinde şifreleme zaman. Kimlik bilgileri alınır ve bunları gerektiğinde FC tarafından şifresi çözülür.

**Ağ aygıtlarını**: ağ hizmeti hesabı ağ aygıtlarına (yönlendiriciler, anahtarlar ve yük Dengeleyiciler) kimlik doğrulaması bir Microsoft Azure istemci etkinleştirmek için Azure ağ ekibi tarafından yapılandırılır.

## <a name="secure-service-administration"></a>Hizmet Yönetimi güvenli
Microsoft Azure işlemler personel, güvenli yönetim iş istasyonları kullanmak için gereklidir (kurlar; müşteriler uygulamak benzer denetimleri ayrıcalıklı erişimli iş istasyonlarının veya Patiler kullanarak). TESTERE yaklaşım, yönetim personeli için ayrı yönetici ve kullanıcı hesaplarını kullanmak için tanınmış önerilen uygulama uzantısıdır. Bu yöntem, kullanıcının standart kullanıcı hesabından ayrı ayrı olarak atanmış bir yönetici hesabı kullanır. Derlemeleri hassas bu hesaplar için güvenilir bir iş istasyonu sağlayarak bu hesabı ayırma uygulama gördünüz.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapı güvenli hale getirmek için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure olanakları, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirliği](azure-infrastructure-availability.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Microsoft Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure üretim işlemleri ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verileri koruma](azure-protection-of-customer-data.md)
