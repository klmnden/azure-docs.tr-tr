---
title: include dosyası
description: include dosyası
services: virtual-machines
author: iainfoulds
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: include file
ms.openlocfilehash: 99e429a2f82d1a9b8d9a87fb3eb4102183c19fe8
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için kullanılabilirlik ve bölgeler
Azure, dünyanın dört bir yanındaki birden fazla veri merkezinde çalışmaktadır. Bu veri merkezleri, coğrafi bölgeler halinde gruplandırılarak uygulamalarınızı oluşturacağınız yeri seçme esnekliği tanır. Sanal makinelerinizin (VM’ler) Azure’da nasıl ve hangi konumda çalıştığının yanı sıra performans, kullanılabilirlik ve yedekliliği artırmak için kullanabileceğiniz seçeneklerin de anlaşılması önemlidir. Bu makalede, Azure’un kullanılabilirlik ve yedeklilik özelliklerine genel bakış sunulmaktadır.

## <a name="what-are-azure-regions"></a>Azure bölgeleri nelerdir?
Tanımlanan coğrafi bölgelerde 'Batı ABD', 'Kuzey Avrupa' veya 'Güneydoğu Asya' gibi Azure kaynakları oluşturun. [Bölgeler ve konumlarının listesini](https://azure.microsoft.com/regions/) gözden geçirebilirsiniz. Her bölge içinde, yedeklilik ve kullanılabilirlik sağlayan birden fazla veri merkezi mevcuttur. Bu yaklaşım uygulamaların VM'ler kullanıcılarınıza yakın oluşturmak için ve yasal, tüm uyumluluk gereklerini karşılamak veya amacıyla vergi tasarlarken esnekliği sağlar.

## <a name="special-azure-regions"></a>Özel Azure bölgeleri
Azure uygulamalarınızı uyumluluğu veya yasal amaçlar için genişletme oluştururken kullanmak isteyebilirsiniz bazı özel bölgeler sahiptir. Bu özel bölgeleri şunlardır:

* **ABD Virginia** ve **ABD Iowa**
  * ABD kamu kuruluşları ve iş ortaklarına yönelik olarak ABD’de bulunan ve denetlenen kişilerce çalıştırılan fiziksel ve mantıksal ağdan yalıtılmış Azure örneği. [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) ve [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA) gibi ek uyumluluk sertifikaları içerir. [Azure Kamu](https://azure.microsoft.com/features/gov/) hakkında daha fazla bilgi alın.
* **Çin Doğu** ve **Çin Kuzey**
  * Bu bölgeler, Microsoft ile 21Vianet arasında, Microsoft’un veri merkezlerini doğrudan yönetmediği benzersiz ortaklık ile kullanıma sunulmaktadır. [Çin’de Microsoft Azure](http://www.windowsazure.cn/) hakkında daha fazla bilgi alın.
* **Almanya Orta** ve **Almanya Kuzeydoğu**
  * Bu bölgeler yapabildiği denetiminde T-sistemleri, Almanca veri güvenliği davranan bir Alman Telekom şirket içinde Almanya müşteri verileri kalır veri güvenlik modelini aracılığıyla kullanılabilir.

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
Belirli VM boyutları ya da depolama türleri gibi bazı hizmetler veya VM özellikleri yalnızca belirli bölgelerde kullanılabilir. Ayrıca, belirli bir bölge seçmenizi gerektirmeyen [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) veya [Azure DNS](../articles/dns/dns-overview.md) gibi bazı genel Azure hizmetleri de vardır. Uygulama ortamınızı tasarlamanıza yardımcı olmak üzere [her bölgedeki Azure hizmetleri kullanılabilirliğini](https://azure.microsoft.com/regions/#services) denetleyebilirsiniz. Ayrıca [program aracılığıyla sorgu desteklenen VM boyutları ve her bölgede kısıtlamaları](../articles/azure-resource-manager/resource-manager-sku-not-available-errors.md).

## <a name="storage-availability"></a>Depolama kullanılabilirliği
Kullanılabilir çoğaltma seçenekleri düşünüldüğünde Azure bölge ve coğrafyalarının anlaşılması önemlidir. Depolama türüne bağlı olarak farklı çoğaltma seçenekleriniz vardır.

**Azure Yönetilen Diskler**
* Yerel olarak yedekli depolama (LRS)
  * Depolama hesabınızı oluşturduğunuz bölge içinde verilerinizi üç kez çoğaltır.

**Depolama hesabı temelli diskler**
* Yerel olarak yedekli depolama (LRS)
  * Depolama hesabınızı oluşturduğunuz bölge içinde verilerinizi üç kez çoğaltır.
* Bölgesel olarak yedekli depolama (ZRS)
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
* Premium yönetilen diskleri Solid-State sürücüler tarafından (SSD) desteklenir ve standart yönetilen disk normal dönen disk ile desteklenir. Hem Premium hem de Standart Yönetilen Diskler, diskin sağlanan kapasitesine göre ücretlendirilir.

**Yönetilmeyen diskler**
* Premium depolama Solid-State sürücüler tarafından (SSD) ile yedeklenir ve disk kapasitesine göre ücretlendirilir.
* Standart depolama, normal dönen disklerle desteklenir ve kullanımdaki kapasiteye ve istenen depolama kullanılabilirliğine göre ücretlendirilir.
  * RA-GRS için, verileri başka bir Azure bölgesine çoğaltmak için gereken bant genişliğine yönelik ek bir Coğrafi Çoğaltma Veri Aktarımı ücreti vardır.

Farklı depolama türleri ve kullanılabilirlik seçenekleri hakkında fiyatlandırma bilgileri için bkz. [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Sanal makineleri mantıksal bir gruplandırması uygulamanızı artıklık ve kullanılabilirlik sağlamak üzere nasıl yapılandırıldığını anlamak Azure izin veren bir veri merkezindeki bir kullanılabilirlik kümesidir. İki veya daha fazla VM'nin kullanılabilirlik sağlamak için yüksek oranda kullanılabilir bir uygulama ve karşılamak için kümesi içinde oluşturulan önerilen [% 99,95 Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Kullanılabilirlik kümesi için hiçbir ücret kendisi, yalnızca oluşturduğunuz her bir VM örneği için ödeme yaparsınız. Tek bir VM, [Azure Premium Depolama](../articles/virtual-machines/windows/premium-storage.md) kullanıyorsa, Azure SLA planlanmamış bakım olayları için geçerli olur. 

Bir kullanılabilirlik kümesi donanım arızalarına karşı koruma sağlamak ve güvenli bir şekilde uygulanması-(FDs) etki alanları ve güncelleme etki alanına (UDs) arıza güncelleştirmeleri izin veren iki ek gruplandırmaları oluşur. [Linux VM](../articles/virtual-machines/linux/manage-availability.md) veya [Windows VM](../articles/virtual-machines/windows/manage-availability.md) kullanılabilirliğini yönetme hakkında daha fazla bilgi alabilirsiniz.

### <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu hata etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

### <a name="update-domains"></a>Güncelleme etki alanları
Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu güncelleme etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır.

### <a name="managed-disk-fault-domains"></a>Disk hata etki alanlarını yönetilen
[Azure Yönetilen Diskler](../articles/virtual-machines/windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). Daha fazla bilgiyi bu hakkında disk hata etki alanları için yönetilen [Linux VM'ler](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) veya [Windows VM'ler](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set).

## <a name="availability-zones"></a>Kullanılabilirlik alanları

[Kullanılabilirlik bölgeleri](../articles/availability-zones/az-overview.md)Alternatif Kullanılabilirlik ayarlar, uygulamaları ve verileri, vm'lerde kullanılabilirliğini sürdürmek zorunda denetim düzeyini genişletin. Kullanılabilirlik Alanı, bir Azure bölgesinin içinde fiziksel olarak ayrılmış bir alandır. Desteklenen bir Azure bölgesine başına üç kullanılabilirlik bölge vardır. Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. Çoğaltılmış VM'ler bölgeleri kullanmak için çözüm mimarisi oluşturma, uygulamaları ve verileri bir veri merkezinde kaybına karşı koruyabilirsiniz. Bir bölge aşılırsa, ardından çoğaltılan uygulamaları ve verileri başka bir bölgede hemen kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Dağıtma hakkında daha fazla bilgi bir [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) kullanılabilirlik bölgesinde VM.

## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](../articles/best-practices-availability-checklist.md).

