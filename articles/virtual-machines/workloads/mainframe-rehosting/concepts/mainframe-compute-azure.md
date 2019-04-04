---
title: Ana bilgisayar işlem Azure sanal Makineler'e taşımanız
description: Azure işlem kaynakları karşılaştırma perakendecilerden ana bilgisayar kapasite geçirme ve IBM z14 uygulamaları modernleştirebilirsiniz.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 8aea4a74ba84855f011dada70ea75ec0d5fb64fe
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896524"
---
# <a name="move-mainframe-compute-to-azure"></a>Ana bilgisayar işlem Azure'a taşıyın

Ana bilgisayarlar yüksek güvenilirlik ve kullanılabilirlik için bir geçmişi vardır ve çoğu kurum güvenilen temel olacak şekilde devam edin. Bunlar genellikle neredeyse sınırsız ve bilgi işlem gücüne sahip düşündüğünüz. Ancak, bazı kuruluşlar, en büyük kullanılabilir anabilgisayar yeteneğini outgrown. Bu gibi düşünüyorsanız, Azure altyapı çevikliği ve reach tasarrufları sağlar.

Ana bilgisayar iş yüklerini Microsoft Azure'da çalıştırmak için ana bilgisayar ait özellikleri karşılaştırma azure'a nasıl işlem bilmeniz gerekir. Bir IBM z14 ana bilgisayar (en son model şu andan itibaren) bağlı olarak, bu makalede, Azure üzerinde benzer sonuçlar elde etmek nasıl bildirir.

Başlamak için yan yana ortamları göz önünde bulundurun. Azure barındırma ortam için uygulamalarınızı çalıştırmak için bir ana bilgisayar ortamını aşağıdaki şekilde karşılaştırır.

![Azure Hizmetleri ve öykünmesi ortamları teklif karşılaştırılabilir destekler ve geçişi kolaylaştırır.](media/mainframe-compute-azure.png)

Ana bilgisayarlar gücünü genellikle binlerce kullanıcıya güncelleştirmeleri milyonlarca işleme (OLTP) sistemleri çevrimiçi işlem için kullanılır. Bu uygulamalar, çoğunlukla işlem, ekran işleme ve form girişi için yazılım kullanır. Bunlar, müşteri bilgi denetim sistemi (CICS), bilgi yönetim sistemi (IMS) veya işlem arabirimi paket (TIP) kullanabilir.

Şekilde gösterildiği gibi Azure üzerinde bir TPM öykünücü CICS ve IMS iş yükleri başa çıkabilir. Azure batch sistem emulator'da rol iş Denetim Dili (JCL) gerçekleştirir. Azure SQL veritabanı gibi Azure veritabanları için ana bilgisayar veri geçirilir. Azure Hizmetleri veya diğer yazılımları Azure sanal Makineler'de barındırılan sistem yönetimi için kullanılabilir.

## <a name="mainframe-compute-at-a-glance"></a>Bir bakışta ana işlem

Z14 anabilgisayar işlemciler en fazla dört düzenlenmiş *çekmece*. A *çekmecesi* işlemciler ve yonga kümeleri yalnızca bir küme. Her çekmecesi altı (CP) etkin merkezi işlemci yongaları olabilir ve her CP 10 sistem denetleyici (SC) yongaları vardır. Intel x86 terminolojisinde, dört çekmece çekmecesi ve yuva başına 10 çekirdek başına altı yuva yok. Bu mimari, 24 yuvaları ve 240 çekirdek, bir z14 için en çok kaba eşdeğerini sağlar.

Hızlı z14 CP sahip 5.2 GHz saat hızı. Genellikle, bir z14 kutusundaki tüm CPs ile teslim edilir. Gerektiğinde etkin. Bir müşteri, en az dört saat gerçek kullanım rağmen aylık işlem süresi için yaygın olarak ücretlendirilir.

Bir ana bilgisayar işlemci şu türlerden biri olarak yapılandırılabilir:

- Genel amaçlı (GP) işlemci
- Sistem z tümleşik bilgi işlemci (zIIP)
- Linux (IFL) işlemci için tümleşik olanağı
- Sistem yardımcı işlemci (SAP)
- Tümleşik eşlenmesiyle tesis (ICF) işlemci

## <a name="scaling-mainframe-compute-up-and-out"></a>Ölçeklendirme ve ana bilgisayar işlem

IBM ana bilgisayarları kadar 240 çekirdek (tek bir sistem için geçerli z14 boyutu) ölçeklendirme olanağı sunar. Ayrıca, IBM ana bilgisayarları eşlenmesiyle tesis (CF) adlı bir özellik üzerinden genişletebilir. CF birden çok ana bilgisayar sistemleri aynı anda aynı verilere erişim sağlar. CF kümeleri ana bilgisayar paralel Sysplex teknoloji grupları ana işlemciler kullanan. Bu kılavuzda yazıldıktan sonra 64 işlemciler 32 gruplandırmalarını paralel Sysplex özelliği desteklenmiyor. 2.048 işlemcilerin en fazla işlem kapasitesi ölçeklendirmek için bu şekilde gruplanabilir.

Bir CF hesaplama kümeleri doğrudan erişim ile veri paylaşmasına izin verir. Bilgi, önbellek bilgilerini ve paylaşılan veri kaynakları listesini kilitlemek için kullanılır. Paralel bir veya daha fazla CFs kullanarak Sysplex bir "her şey paylaşılmayan" genişleme işlem kümesi zorlayıcı olabilir. Bu özellikler hakkında daha fazla bilgi için bkz. [IBM Z üzerinde paralel Sysplex](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources) IBM Web sitesinde.

Uygulamalar, hem performans ölçeğini genişletme ve yüksek kullanılabilirlik sağlamak için şu özellikleri kullanabilir. CICS paralel Sysplex CF ile nasıl kullanabileceğiniz hakkında daha fazla bilgi için indirme [IBM CICS ve eşlenmesiyle tesis: Temel kavramların ötesine](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf) redbook.

## <a name="azure-compute-at-a-glance"></a>Bir bakışta Azure işlem

Bazı kullanıcılar yanlışlıkla Intel tabanlı sunucular faydalanmanızı kadar güçlü olmadığını düşünün. Ancak yeni çekirdek yoğun, Intel tabanlı sistemler, ana bilgisayarlar olarak kapasitesi çok işlem olarak vardır. Bu bölümde, bilgi işlem ve depolama için Azure hizmet olarak altyapı (Iaas) seçenekleri açıklanmaktadır. Azure hizmet olarak platform (PaaS) seçeneği de sunar, ancak bu makalede karşılaştırılabilir ana bilgisayar kapasite sağlamak Iaas seçimlerine odaklanır.

Azure sanal makineleri bir dizi boyutları ve türleri de bilgi işlem gücü sağlar. Azure'da bir sanal CPU (vCPU) kabaca bir çekirdek bir ana bilgisayar üzerinde karşılık gelir.

Şu anda aralığı Azure sanal makine boyutları sunar 1 ile 128 Vcpu için. Sanal makine (VM) türler, belirli iş yükleri için iyileştirilmiştir. Örneğin, aşağıdaki listede, (Bu makalenin yazıldığı tarih itibarıyla geçerli) VM türleri ve önerilen kullanımları gösterir:

| Boyut     | Türü ve açıklaması                                                                 |
|----------|--------------------------------------------------------------------------------------|
| D Serisi | 3,5 GHz saat hızı en fazla 64 vCPU ile genel amaçlı                           |
| E Serisi | Bellek için iyileştirilmiş ile en fazla 64 Vcpu                                                 |
| F Serisi | En fazla 64 Vcpu ve 3..7 GHz saat hızı için iyileştirilmiş işlem                       |
| H serisi | Yüksek performanslı bilgi işlem (HPC) uygulamaları için en iyi duruma getirilmiş                          |
| L Serisi | Gibi NoSQL veritabanları tarafından desteklenen yüksek performanslı uygulamalar için iyileştirilmiş depolama |
| M Serisi | En büyük işlem ve bellek ile en çok 128 Vcpu VM'ler için iyileştirilmiş                        |

Kullanılabilir VM'ler hakkında daha fazla ayrıntı için bkz: [sanal makine serisi](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

Z14 anabilgisayar kadar 240 çekirdeğe sahip olabilir. Ancak, z14 anabilgisayar neredeyse hiçbir zaman tüm çekirdeklerin tek bir uygulama veya iş yükü için kullanın. Bunun yerine, bir ana bilgisayar iş yüklerini (LPARs) mantıksal bölümlere ayırır ve LPARs derecelendirmesine sahip — MIPS (milyonlarca saniye başına yönergeleri) veya MSU (milyon hizmet birimi). Azure, MIPS (veya MSU) faktör Derecelendirmeye göre bir ana bilgisayar iş yükü çalıştırmak için gereken karşılaştırılabilir VM boyutunu belirlerken.

Genel tahminleri şunlardır:

-   150 MIPS vCPU başına

-   İşlemci başına 1.000 MIPS

Bir LPAR belirli bir iş yükü için doğru VM boyutunu belirlemek için önce sanal makine iş yükü için iyileştirin. Ardından gerekli Vcpu sayısını belirler. Bir koruyucu 150 MIPS vCPU tahminidir. Bu tahmin üzerinde bağlı olarak, örneğin, bir F serisi VM ile 16 Vcpu kolayca 2.400 MIPS ile bir LPAR geldiği bir IBM Db2 iş yükü destekleyebilir.

## <a name="azure-compute-scale-up"></a>Azure işlem ölçek büyütme

M serisi VM'ler, en çok 128 Vcpu (Bu makalenin yazıldığı saatte) ölçeklendirebilirsiniz. 150 MIPS vCPU başına koruyucu tahmini kullanarak, M serisi sanal makine için yaklaşık 19.000 MIPS karşılık gelir. MIPS tahmin etmek için bir ana bilgisayar işlemci başına 1.000 MIPS için genel kuralı. Z14 ana bilgisayar, en fazla 24 işlemcilere sahip ve tek bir ana bilgisayar sistemi için yaklaşık 24.000 MIPS sağlayın.

En büyük tek z14 anabilgisayar, Azure'da kullanılabilen en büyük sanal makine birden çok yaklaşık 5.000 MIPS sahiptir. Henüz iş yüklerini nasıl dağıtılacağını karşılaştırmak önemlidir. Bir ana bilgisayar sistemi hem uygulama hem de ilişkisel bir veritabanı varsa, genellikle aynı fiziksel ana bilgisayar dağıtıldıkları — her biri kendi LPAR. Azure'da aynı çözüm genellikle uygulama ve veritabanı için ayrı, uygun boyutta bir VM için bir VM kullanılarak dağıtılır.

M64 vCPU sistemi uygulamanın desteklediği ve veritabanı için kullanılan bir M96 vCPU, yaklaşık 150 Vcpu gibi gerekli — veya hakkında 24.000 MIPS aşağıdaki şekilde gösterildiği gibi.

![İş yükü dağıtımları 24.000 MIPS karşılaştırma](media/mainframe-compute-mips.png)

LPARs için tek tek sanal makineleri geçirmek için bir yaklaşımdır. Ardından Azure kolayca tek ana bilgisayar sisteminde dağıtıldığı uygulamalarının çoğu için gerekli boyuta ölçeklendirilebilir.

## <a name="azure-compute-scale-out"></a>Azure işlem ölçek genişletme

Azure tabanlı bir çözüm avantajları de ölçeğini yeteneğidir. Neredeyse sınırsız ölçeklendirme yapar hesaplama kapasitesi bir uygulama için kullanılabilir. Azure bilgi işlem gücü ölçeklendirme için birden çok yöntem destekler:

- **Bir kümede yük dengelemeyi.** Bu senaryoda, bir uygulamanın kullanabileceği bir [yük dengeleyici](/azure/load-balancer/load-balancer-overview) veya bir kümede birden çok VM arasında iş yükü kullanıma yaymak için resource manager. Daha fazla kapasiteye ihtiyaç işlem, kümeye ek VM'ler eklenir.

- **Sanal makine ölçek kümeleri.** Bu veri bloğu senaryoda, bir uygulamayı ek ölçeklendirmenin [işlem kaynaklarını](/azure/virtual-machine-scale-sets/overview) VM kullanımını temel alarak. Talep düştüğünde bir ölçek kümesindeki VM sayısını da aşağı, işlem gücünü verimli bir şekilde kullanılmasını sağlamaya gidebilirsiniz.

- **PaaS ölçeklendirme.** Azure PaaS tekliflerini ölçek bilgi işlem kaynakları. Örneğin, [Azure Service Fabric](/azure/service-fabric/service-fabric-overview) istek artışlarını karşılamak için işlem kaynakları ayırır.

- **Kubernetes kümelerini.** Uygulamalar Azure üzerinde kullanabilir [Kubernetes kümelerini](/azure/aks/concepts-clusters-workloads) bilgi işlem Hizmetleri için belirtilen kaynaklar için. Azure Kubernetes Service (AKS) Kubernetes düğümleri, havuzlar ve azure'daki kümeler düzenleyen bir yönetilen hizmettir.

Out işlem kaynaklarını ölçeklendirme için doğru yöntemi seçmek için Azure ve ana bilgisayarlar farkı anlamak önemlidir. Anahtar nasıl — veya — veri işlem kaynakları tarafından paylaşılır. Azure'da veri (varsayılan) genellikle birden çok VM tarafından paylaşılmıyor. Veri paylaşımı tarafından gerekiyorsa birden çok VM içinde bir genişleme işlem kümesi, paylaşılan veriler bu işlevi destekleyen bir kaynak olarak bulunması gerekir. Aşağıdaki bölümde anlatılmaktadır gibi Azure üzerinde veri paylaşımı depolama gerektirir.

## <a name="azure-compute-optimization"></a>Azure işlem en iyi duruma getirme

Her bir katman içinde bir Azure mimari işlemin en iyi duruma getirebilirsiniz. Her ortam için en uygun türünü Vm'leri ve özellikleri kullanın. Db2 kullanan bir CICS uygulamayı desteklemek için azure'da VM'ler dağıtmak için bir olası deseni aşağıdaki şekilde gösterilmiştir. Birincil site, üretim ön üretim ve test VM'ler yüksek kullanılabilirlik ile dağıtılır. İkincil site için yedekleme ve olağanüstü durum kurtarma ' dir.

Her katman aynı zamanda uygun olağanüstü durum kurtarma hizmetleri sağlar. Örneğin, üretim ve veritabanı sanal makinelerinizin soğuk bir kurtarma geliştirme ve test sanal makineleri desteklerken, sık erişimli ya da Orta Gecikmeli kurtarma gerektirebilir.

![Olağanüstü durum kurtarma destekleyen yüksek oranda kullanılabilir dağıtım](media/mainframe-compute-dr.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Azure sanal makineler üzerinde anabilgisayar yeniden barındırma](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Ana bilgisayar depolama Azure'a taşıyın](mainframe-storage-Azure.md)

### <a name="ibm-resources"></a>IBM kaynakları

- [IBM Z üzerinde paralel Sysplex](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS ve bağ tesis: Ötesine](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Bir Db2 pureScale özelliği yükleme için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - örnek bir komut oluşturun](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale kümelenmiş veritabanı çözümü](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM veri Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Kamu

- [Ana bilgisayar uygulamalarını Microsoft Azure kamu Bulutu](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft ve FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Daha fazla geçiş kaynakları

- [Platform Modernizasyonu Alliance: Azure üzerinde IBM Db2](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Azure sanal veri merkezi Lift- and -Shift Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
