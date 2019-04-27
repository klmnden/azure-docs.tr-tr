---
title: Azure Information sistem bileşenleri ve sınırlar
description: Bu makalede, Microsoft Azure mimarisinin ve Yönetime genel bir açıklamasını sağlar.
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
ms.openlocfilehash: b390dc9bd2b690837a85a5bab361a534b9c9d5a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587215"
---
# <a name="azure-information-system-components-and-boundaries"></a>Azure Information sistem bileşenleri ve sınırlar
Bu makalede, Azure mimarisinin ve Yönetime genel bir açıklamasını sağlar. Azure sistem ortam aşağıdaki ağlar oluşur:

- Microsoft Azure üretim ağı (Azure ağı)
- Microsoft Kurumsal ağın (corpnet)

Ayrı BT ekipleri, işlemler ve Bakım bu ağ için sorumludur.

## <a name="azure-architecture"></a>Azure Mimarisi
Azure bilgi işlem platformu ve altyapı geliştirme, dağıtma ve yöneten uygulamalar ve hizmetler için veri merkezleri ağı üzerinden bir buluttur. Microsoft, bu veri merkezleri yönetir. Belirttiğiniz kaynakların sayısına bağlı olarak, Azure sanal makineleri (VM'ler) kaynak ihtiyaca göre oluşturur. Bu VM'ler için tasarlanmış olan bir Azure hiper Yöneticisi, çalışan, bulutta kullanın ve ortak erişilebilir değil.

Her Azure fiziksel sunucu düğümünde doğrudan donanım üzerinde çalışan bir hiper yönetici yok. Hiper yönetici, bir düğüm Konuk VM'lerin değişkeni bir sayıya böler. Her düğüm bir kök konak işletim sistemi çalıştıran sanal makine de vardır. Her sanal makinede Windows Güvenlik Duvarı etkin. Hangi bağlantı noktalarının adreslenebilir Hizmet tanım dosyası yapılandırarak tanımlarsınız. Bu bağlantı noktaları yalnızca açık ve adreslenebilir, dahili veya harici olanlardır. Tüm trafiği ve disk ve ağ erişimi hiper yöneticisine ve kök işletim sistemi tarafından cout.

Ana bilgisayar katmanındaki, Azure sanal makinelerini en son Windows Server özelleştirilmiş ve sağlamlaştırılmış bir sürümünü çalıştırın. Azure, yalnızca konak sanal makineleri için gerekli bileşenleri içeren Windows Server sürümünü kullanır. Bu, performansı geliştirir ve saldırı yüzeyini azaltan. Makine sınırları, işletim sistemi güvenlik bağlı olmayan hiper yönetici tarafından uygulanır.

### <a name="azure-management-by-fabric-controllers"></a>Yapı denetleyicilerine duyulan Azure Yönetimi

Azure'da fiziksel sunucuları (dikey pencereleri/düğüm) üzerinde çalışan Vm'leri yaklaşık 1000 kümeler halinde gruplandırılır. Vm'leri, birbirinden bağımsız olarak yapı denetleyicisi (FC) adlı bir genişletilmiş ve yedekli platformu yazılım bileşeni tarafından yönetilir.

Her FC, küme ve hükümleri çalışan uygulamaların yaşam döngüsünü yöneten ve denetimi altındaki donanım durumunu izler. Bu işlem, bir sunucu başarısız olduğunu belirlediğinde, sağlıklı sunucuları VM örneklerinde reincarnating gibi autonomic işlemlerini çalışır. FC de dağıtma, güncelleştirme ve ölçeklendirme uygulamaları gibi uygulama yönetimi işlemlerini gerçekleştirir.

Veri Merkezi kümeler halinde bölünmüştür. Kümeleri FC düzeyinde hatalarını yalıtmak ve hataların bazı sınıflar içinde ortaya küme ötesinde sunucuları etkilemesini önleyin. Belirli bir Azure kümesine hizmet FCs bir FC küme gruplanır.

### <a name="hardware-inventory"></a>Donanım envanteri

FC önyükleme yapılandırma işlemi sırasında Azure donanım ve ağ cihazları envanterini hazırlar. Herhangi bir yeni donanım ve ağ bileşenlerini Azure üretim ortamına girme önyükleme yapılandırma işlemi izlemelidir. FC datacenter.xml yapılandırma dosyasında listelenen tüm envanteri yönetmekten sorumludur.

### <a name="fc-managed-operating-system-images"></a>FC-yönetilen işletim sistemi görüntüleri

İşletim sistemi takım formunda, sanal sabit tüm konak ve Konuk Vm'leri Azure üretim ortamında dağıtılan diskler, görüntüleri sağlar. Takım, otomatik çevrimdışı yapı işleminde bu temel görüntüleri oluşturur. Temel görüntü, işletim sistemi, çekirdek ve diğer temel bileşenleri alınan değişiklik ve Azure ortamı desteklemek için iyileştirilmiş sürümüdür.

Fabric yönetilen işletim sistemi görüntüleri üç tür vardır:

- Ana bilgisayar: Bir özelleştirilmiş VM'ler ana bilgisayarda çalışan işletim sistemi.
- Yerel: Bir yerel kiracılar (örneğin, Azure depolama) üzerinde çalışan işletim sistemi. Bu işletim sistemi, tüm hiper yönetici yok.
- Konuk: Konuk Vm'lerde çalışan bir konuk işletim sistemi.

Konak ve yerel FC yönetilen işletim sistemi bulutta kullanmak için tasarlanmıştır ve genel olarak erişilebilir değildir.

#### <a name="host-and-native-operating-systems"></a>Konak ve yerel işletim sistemleri

Ana bilgisayar hem de yerel yapı aracıları barındırmak ve işlem düğümü (ilk sanal makine çalışırken düğümde) ve depolama düğümleri çalışan sıkı işletim sistemi görüntüleridir. En iyi duruma getirilmiş temel görüntüleri host ve yerel kullanmanın avantajı API'leri veya kullanılmayan bileşenleri tarafından kullanıma sunulan yüzey alanını azaltmasıdır. Bu yüksek güvenlik riskleri sunmak ve işletim sisteminin kapladığı alanı artırın. Azaltılmış Ayak izi işletim sistemleri, yalnızca Azure'a gerekli bileşenleri içerir.

#### <a name="guest-operating-system"></a>Konuk işletim sistemi

Konuk işletim sistemi VM'ler üzerinde çalışan azure iç bileşenleri, Uzak Masaüstü Protokolü çalıştırmak için bir fırsat sahiptir. Taban çizgisi yapılandırma ayarlarına yapılan değişiklikler değişikliğiyle gidin ve sürüm Yönetimi işlemi gerekir.

## <a name="azure-datacenters"></a>Azure veri merkezleri
Microsoft bulut altyapısı ve işlemler (MCIO) ekibi, tüm Microsoft online services fiziksel altyapı ve veri merkezi tesislerinde yönetir. MCIO veri merkezleri içinde fiziksel ve ortam denetimleri yönetme yanı sıra yönetmek ve dış çevre ağ cihazlarını (örneğin, uç yönlendiricileri ve veri merkezi yönlendiriciler) desteklemek için öncelikli olarak sorumludur. MCIO, raflar veri merkezinde çıplak en düşük sunucu donanımına ayarlamak için sorumludur. Müşteriler Azure ile doğrudan etkileşimi vardır.

## <a name="service-management-and-service-teams"></a>Hizmet Yönetimi ve Azure hizmet ekipleri
Hizmet takımlar olarak bilinen çeşitli mühendislik gruplarının Azure hizmeti desteği yönetin. Her hizmet takımının, bir Azure destek alanı sorumludur. Her hizmet takımının araştırıp hizmetinde hatalarını çözümlemek için bir mühendis kullanılabilir 24 x 7 yapmanız gerekir. Hizmet ekipleri varsayılan olarak, Azure'da çalışan donanımına fiziksel erişim sahip değilse.

Hizmet ekipleri şunlardır:

- Uygulama Platformu
- Azure Active Directory
- Azure İşlem
- Azure ağ
- Bulut Hizmetleri Mühendisliği
- ISSD: Güvenlik
- Çok Faktörlü Kimlik Doğrulama
- SQL Veritabanı
- Depolama

## <a name="types-of-users"></a>Kullanıcı türleri
Çalışanların (veya yüklenicilerdir) Microsoft iç kullanıcılar olarak değerlendirilir. Dış kullanıcılar için diğer tüm kullanıcılar olarak kabul edilir. Tüm Azure iç kullanıcılar erişimleri müşteri verilerini (erişim veya erişim yok) tanımlayan bir duyarlılık düzeyinde kategorilere çalışan durumlarına sahiptir. (Kimlik doğrulaması gerçekleştikten sonra yetkilendirme izni) azure'a kullanıcı ayrıcalıkları, aşağıdaki tabloda açıklanmıştır:

| Rol | İç veya dış | Duyarlılık düzeyi | Yetkili ayrıcalıklar ve gerçekleştirilen İşlevler | Erişim türü
| --- | --- | --- | --- | --- |
| Azure veri merkezi mühendisi | İç | Müşteri verilerine erişim yok | Şirket içi fiziksel güvenliğini yönetin. Veri Merkezi içine ve dışına patrols yürütmenize ve tüm giriş noktalarını izleyin. İşaretli olmayan escort içine ve dışına veri merkezi belirli personeli (örneğin, Yemek veya temizleme) genel hizmetler veya veri merkezi BT iş sağlar. Rutin izleme ve ağ donanım Bakımı gerçekleştirin. Olay yönetimi ve onarım iş çeşitli araçları kullanarak gerçekleştirin. Rutin izleme ve Veri merkezlerindeki fiziksel donanım Bakımı gerçekleştirin. Mülk sahiplerinin'ten isteğe bağlı ortam erişim. Adli araştırmalar yapmak, olay raporlarının oturum ve zorunlu güvenlik eğitim ve İlkesi gereksinimleri var mı özellikli. İşletimsel sahipliği ve bunların bakımını tarayıcılar ve günlük toplama gibi kritik güvenlik araçları. | Ortam kalıcı erişim. |
| Azure olay önceliklendirme (hızlı yanıt mühendisleri) | İç | Müşteri verilerine erişim | MCIO, destek ve mühendislik ekipleri arasındaki iletişimi yönetin. Platform olayları, dağıtım sorunlarını ve hizmet isteklerini değerlendirin. | Sınırlı kalıcı olmayan müşteri sistemlerine erişimi olan tam zamanında erişim ortamına. |
| Azure dağıtım mühendisleri | İç | Müşteri verilerine erişim | Dağıtabilir ve platform bileşenleri, yazılım ve Azure desteklemek üzere zamanlanmış yapılandırma değişiklikleri yükseltin. | Sınırlı kalıcı olmayan müşteri sistemlerine erişimi olan tam zamanında erişim ortamına. |
| Azure müşteri kaybı desteği (Kiracı) | İç | Müşteri verilerine erişim | Hata ayıklama ve tek tek işlem kiracılar ve Azure hesapları için platform kesintileri ve hatalarını tanılayın. Hataları analiz edin. Kritik düzeltmeleri platform veya müşteri sürücü ve teknik geliştirmeler arasında destek öncülük edin. | Sınırlı kalıcı olmayan müşteri sistemlerine erişimi olan tam zamanında erişim ortamına. |
| Azure Canlı site mühendisleri (mühendisleri izleme) ve olay | İç | Müşteri verilerine erişim | Tanılama ve tanılama araçlarını kullanarak platform durumu azaltın. Düzeltmeleri birim sürücüsü için sürücü kesintilere karşı edilen öğelerin onarın ve kesinti geri yükleme eylemlerini yardımcı. | Sınırlı kalıcı olmayan müşteri sistemlerine erişimi olan tam zamanında erişim ortamına. |
|Azure müşterileri | Dış | Yok | Yok | Yok |

Azure, kurumsal kullanıcılar ve müşterilere (veya kurumsal kullanıcılar adına hareket işlemleri) kimliğini doğrulamak için benzersiz tanımlayıcıları kullanır. Bu, tüm varlıkları ve Azure ortamının bir parçası olan cihazlar için geçerlidir.

### <a name="azure-internal-authentication"></a>Azure iç kimlik doğrulaması

Azure iç bileşenleri arasındaki iletişim, TLS şifrelemesi ile korunur. Çoğu durumda, otomatik olarak imzalanan X.509 sertifikaları. Sertifikalar için FCs olarak Azure Ağ dışından erişilebilir bağlantıları olan bir özel durum sertifikalardır. FCs Microsoft Certificate, yetkili (güvenilen bir kök CA tarafından desteklenen CA) tarafından verilen sertifikalara sahip. Bu, kolayca geri alınması FC ortak anahtarlar sağlar. Ayrıca, Microsoft Geliştirici Araçları, FC ortak anahtarları kullanın. Geliştiriciler yeni uygulama görüntüsü gönderdiğinde, görüntüleri katıştırılmış gizli dizileri korumak için bir FC ortak anahtarıyla şifrelenir.

### <a name="azure-hardware-device-authentication"></a>Azure donanım cihaz kimlik doğrulaması

FC denetimi altındaki çeşitli donanım aygıtları için kendi kimliğini doğrulamak için kullanılan kimlik bilgileri kümesini (anahtarlar ve/veya parolaları) tutar. Microsoft, bu kimlik bilgileri için erişimi engellemek için bir sistem kullanır. Özellikle, taşıma, süreklilik ve bu kimlik bilgilerini kullan hassas, gizli veya özel bilgiler için Azure geliştiricileri, Yöneticiler ve yedekleme hizmetleri ve personelin erişimi engellemek için tasarlandı.

Microsoft, FC'ın ana kimlik genel anahtarını temel alan şifreleme kullanır. Bu, FC Kurulum ve ağ aygıtları erişmek için kullanılan kimlik bilgileri aktarmak için FC yeniden yapılandırma kez gerçekleşir. FC kimlik bilgilerini gerektiğinde, FC alır ve bunların şifresini çözer.

### <a name="network-devices"></a>Ağ aygıtları

Azure ağ ekibi, ağ hizmeti hesapları, ağ cihazlarına (yönlendiriciler, anahtarlar ve yük Dengeleyiciler) kimlik doğrulaması bir Azure istemci etkinleştirmek için yapılandırır.

## <a name="secure-service-administration"></a>Hizmet Yönetimi güvenli hale getirme
Azure operasyon personeli, güvenli yönetim iş istasyonları (Saw) kullanmak için gereklidir. Müşteriler, ayrıcalıklı erişim iş istasyonları kullanarak benzer denetimlerini uygulayabilir. Saw ile yönetim personeli kullanıcının standart kullanıcı hesaplarından ayrı ayrı ayrı atanmış bir yönetici hesabı kullanın. SAW hassas hesaplar için güvenilir bir iş istasyonu sağlayarak bu hesabı ayrımı uygulamalar oluşturur.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Azure altyapısının güvenliğini sağlamak için yaptığı hakkında daha fazla bilgi için bkz:

- [Azure özellikleri, şirket içi ve fiziksel güvenlik](azure-physical-security.md)
- [Azure altyapı kullanılabilirlik](azure-infrastructure-availability.md)
- [Azure ağ mimarisi](azure-infrastructure-network.md)
- [Azure üretim ağı](azure-production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](azure-infrastructure-sql.md)
- [Azure Üretim Operasyon ve Yönetimi](azure-infrastructure-operations.md)
- [Azure altyapı izleme](azure-infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](azure-infrastructure-integrity.md)
- [Azure müşteri verilerini koruma](azure-protection-of-customer-data.md)
