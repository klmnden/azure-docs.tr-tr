---
title: include dosyası
description: include dosyası
services: virtual-machines
author: zr-msft
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2018
ms.author: zarhoads
ms.custom: include file
ms.openlocfilehash: 7f33312d0a5fbe383d438408d471dd9ae09d0332
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156237"
---
# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için kullanılabilirlik ve bölgeler
Azure, dünyanın dört bir yanındaki birden fazla veri merkezinde çalışmaktadır. Bu veri merkezleri, coğrafi bölgeler halinde gruplandırılarak uygulamalarınızı oluşturacağınız yeri seçme esnekliği tanır. Sanal makinelerinizin (VM’ler) Azure’da nasıl ve hangi konumda çalıştığının yanı sıra performans, kullanılabilirlik ve yedekliliği artırmak için kullanabileceğiniz seçeneklerin de anlaşılması önemlidir. Bu makalede, Azure’un kullanılabilirlik ve yedeklilik özelliklerine genel bakış sunulmaktadır.

## <a name="what-are-azure-regions"></a>Azure bölgeleri nelerdir?
'Batı ABD', 'Kuzey Avrupa' veya 'Güneydoğu Asya' gibi tanımlı coğrafi bölgelerde Azure kaynaklarını oluşturun. [Bölgeler ve konumlarının listesini](https://azure.microsoft.com/regions/) gözden geçirebilirsiniz. Her bölge içinde, yedeklilik ve kullanılabilirlik sağlayan birden fazla veri merkezi mevcuttur. Bu yaklaşım, kullanıcılarınıza en yakın Vm'leri oluşturmak ve herhangi bir yasal, uyumluluk gereklerini karşıladığınızdan veya Vergi amaçları için uygulamaları tasarlarken esnekliği sağlar.

## <a name="special-azure-regions"></a>Özel Azure bölgeleri
Azure, uyumluluk veya hukuk gereksinimlerine uygulamalarınızı oluştururken kullanmak isteyebileceğiniz bazı özel bölgeleri sahiptir. Bu özel bölgeleri şunlardır:

* **US Gov Virginia** ve **US Gov Iowa**
  * ABD kamu kuruluşları ve iş ortaklarına yönelik olarak ABD’de bulunan ve denetlenen kişilerce çalıştırılan fiziksel ve mantıksal ağdan yalıtılmış Azure örneği. [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) ve [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA) gibi ek uyumluluk sertifikaları içerir. [Azure Kamu](https://azure.microsoft.com/features/gov/) hakkında daha fazla bilgi alın.
* **Çin Doğu** ve **Çin Kuzey**
  * Bu bölgeler, Microsoft ile 21Vianet arasında, Microsoft’un veri merkezlerini doğrudan yönetmediği benzersiz ortaklık ile kullanıma sunulmaktadır. [Çin’de Microsoft Azure](http://www.windowsazure.cn/) hakkında daha fazla bilgi alın.
* **Almanya Orta** ve **Almanya Kuzeydoğu**
  * Bu bölgeler, Almanya'da Müşteri verilerinin Alman veri güvenilen kişisi olarak davranan Deutsche Telekom şirketi, Denetim T-Systems'ın altında yapabildiği kalır bir veri Emanetçisi modeli aracılığıyla kullanılabilir.

## <a name="region-pairs"></a>Bölge çiftleri
Her Azure bölgesi aynı coğrafyadaki (ABD, Avrupa veya Asya) başka bir bölgeyle eşleştirilir. Bu yaklaşım her iki bölgeyi de aynı anda etkileyen bir doğal felaket, toplumsal karmaşa, güç kesintisi veya fiziksel ağ kesintisi olasılığını azaltması gereken bir coğrafyada VM depolama gibi kaynak çoğaltma işlemlerine olanak tanır. Bölge çiftlerinin diğer avantajları şunlardır:

* Daha geniş bir Azure kesintisi durumunda, uygulamalar için geri yükleme süresini azaltmak üzere her çift içinden bir bölgeye öncelik verilir. 
* Kapalı kalma süresini ve uygulama kesintisi riskini azaltmak amacıyla, planlı Azure güncelleştirmeleri, bölge çiftlerine tek tek uygulanır.
* Veriler, vergi ve yasa uygulama yetkisi bakımından çiftiyle aynı coğrafyada (Brezilya Güney hariç) bulunmaya devam eder.

Bölge çiftlerinin örnekleri şunlardır:

| Birincil | İkincil |
|:--- |:--- |
| Batı ABD |Doğu ABD |
| Kuzey Avrupa |Batı Avrupa |
| Güneydoğu Asya |Doğu Asya |

[Bölgesel çiftlerin tam listesini burada](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions) görebilirsiniz.

## <a name="feature-availability"></a>Özellik kullanılabilirliği
Belirli VM boyutları ya da depolama türleri gibi bazı hizmetler veya VM özellikleri yalnızca belirli bölgelerde kullanılabilir. Ayrıca, belirli bir bölge seçmenizi gerektirmeyen [Azure Active Directory](../articles/active-directory/fundamentals/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) veya [Azure DNS](../articles/dns/dns-overview.md) gibi bazı genel Azure hizmetleri de vardır. Uygulama ortamınızı tasarlamanıza yardımcı olmak üzere [her bölgedeki Azure hizmetleri kullanılabilirliğini](https://azure.microsoft.com/regions/#services) denetleyebilirsiniz. Ayrıca [program aracılığıyla sorgu desteklenen VM boyutları ve her bölgede kısıtlamaları](../articles/azure-resource-manager/resource-manager-sku-not-available-errors.md).

## <a name="storage-availability"></a>Depolama kullanılabilirliği
Kullanılabilir çoğaltma seçenekleri düşünüldüğünde Azure bölge ve coğrafyalarının anlaşılması önemlidir. Depolama türüne bağlı olarak farklı çoğaltma seçenekleriniz vardır.

**Azure Yönetilen Diskler**
* Yerel olarak yedekli depolama (LRS)
  * Depolama hesabınızı oluşturduğunuz bölge içinde verilerinizi üç kez çoğaltır.

**Depolama hesabı temelli diskler**
* Yerel olarak yedekli depolama (LRS)
  * Depolama hesabınızı oluşturduğunuz bölge içinde verilerinizi üç kez çoğaltır.
* Alanlar arası yedekli depolama (ZRS)
  * Tek bir bölge ya da iki bölgedeki iki veya üç tesiste verilerinizi üç kez çoğaltır.
* Coğrafi olarak yedekli depolama (GRS)
  * Verilerinizi birincil bölgeden yüzlerce kilometre uzaktaki bir ikincil bölgeye çoğaltır.
* Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)
  * GRS ile olduğu gibi verilerinizi ikincil bölgeye çoğaltır, ancak daha sonra ikincil konumdaki verilere salt okunur erişim sağlar.

Aşağıdaki tabloda, depolama çoğaltma türleri arasındaki farkları hızlı bir genel bakış sunulmaktadır:

| Çoğaltma stratejisi | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Veriler birden çok tesis arasında çoğaltılır. |Hayır |Evet |Evet |Evet |
| Veriler ikincil konumdan ve birincil konumdan okunabilir. |Hayır |Hayır |Hayır |Evet |
| Ayrı düğümlerde tutulan veri kopyası sayısı. |3 |3 |6 |6 |

[Azure Depolama çoğaltma seçenekleri hakkında buradan](../articles/storage/common/storage-redundancy.md) daha fazla bilgi alabilirsiniz. Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../articles/virtual-machines/windows/managed-disks-overview.md).

### <a name="storage-costs"></a>Depolama maliyetleri
Fiyatlar seçtiğiniz depolama türüne ve kullanılabilirliğe bağlı olarak değişir.

**Azure Yönetilen Diskler**
* Premium yönetilen diskler Solid-State sürücüleri (SSD) yedeklenir ve standart yönetilen diskler, normal dönen disklerle desteklenir. Hem Premium hem de Standart Yönetilen Diskler, diskin sağlanan kapasitesine göre ücretlendirilir.

**Yönetilmeyen diskler**
* Premium depolama, yedeklenen Solid-State sürücüleri (SSD) ve diskin kapasitesine göre ücretlendirilir.
* Standart depolama, normal dönen disklerle desteklenir ve kullanımdaki kapasiteye ve istenen depolama kullanılabilirliğine göre ücretlendirilir.
  * RA-GRS için, verileri başka bir Azure bölgesine çoğaltmak için gereken bant genişliğine yönelik ek bir Coğrafi Çoğaltma Veri Aktarımı ücreti vardır.

Farklı depolama türleri ve kullanılabilirlik seçenekleri hakkında fiyatlandırma bilgileri için bkz. [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Mantıksal bir gruplandırması olan Vm'leri Azure yedeklilik ve kullanılabilirlik sağlamak için uygulamanızı nasıl oluşturulduğunu anlamasına olanak tanıyan bir veri merkezinde bir kullanılabilirlik kümesidir. İki veya daha fazla sanal makine için yüksek oranda kullanılabilir bir uygulama sağlamak ve karşılamak için bir kullanılabilirlik içinde oluşturulan önerilen [% 99,95 Azure SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Kullanılabilirlik kümesi için bir ücret ödemeden kendisini, yalnızca oluşturduğunuz her bir VM örneği için ödeme yaparsınız. Tek bir VM kullandığında [Azure premium SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd), Azure SLA planlanmamış bakım olayları için geçerlidir.

Bir kullanılabilirlik kümesi, donanım hatalarına karşı koruyan ve güncelleştirmelerin güvenli bir şekilde uygulanması - hata etki alanları (FD) ve güncelleştirme etki alanları (UD) izin veren iki ek gruplandırmalarını oluşur. [Linux VM](../articles/virtual-machines/linux/manage-availability.md) veya [Windows VM](../articles/virtual-machines/windows/manage-availability.md) kullanılabilirliğini yönetme hakkında daha fazla bilgi alabilirsiniz.

Ancak, hata etki alanları, yüksek kullanılabilirlik yapıları kullanmayın birden çok işlem kaynaklarını ayırma benzeşim karşıtlığı yüksek bir olasılık yoktur, bu benzeşim karşıtlığı garanti edilmez.

### <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu hata etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

### <a name="update-domains"></a>Etki alanlarını güncelleştir
Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu güncelleme etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır.

![Kullanılabilirlik kümeleri](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>Yönetilen disk hata etki alanları
[Azure Yönetilen Diskler](../articles/virtual-machines/windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). Daha fazla bilgi edinebilirsiniz bunlar hakkında yönetilen disk hata etki alanları için [Linux Vm'leri](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) veya [Windows Vm'leri](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set).

![Yönetilen kullanılabilirlik kümesi](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

## <a name="availability-zones"></a>Kullanılabilirlik alanları

[Kullanılabilirlik alanları](../articles/availability-zones/az-overview.md)kullanılabilirlik alternatif ayarlar, uygulamaları ve verileri sanal makinelerinizin kullanılabilirliğini sürdürmek için sahip olduğunuz denetimi düzeyini genişletin. Kullanılabilirlik Alanı, bir Azure bölgesinin içinde fiziksel olarak ayrılmış bir alandır. Üç kullanılabilirlik alanı desteklenen bir Azure bölgesi başına vardır. Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. Çoğaltılan VM'ler bölgelerinde kullanılacak çözümlerinizi tasarlayarak, uygulamalarınızın ve verilerinizin bir veri merkezi kaybına karşı koruyabilirsiniz. Bir bölge tehlikedeyse, ardından çoğaltılan uygulamaları ve verileri başka bir bölgede anında kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Dağıtma hakkında daha fazla bilgi bir [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) bir kullanılabilirlik alanında VM.

## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](../articles/best-practices-availability-checklist.md).

