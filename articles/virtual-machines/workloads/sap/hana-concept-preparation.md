---
title: Olağanüstü durum kurtarma ilkeler ve üzerinde SAP HANA (büyük örnekler) azure'da hazırlama | Microsoft Docs
description: Olağanüstü durum kurtarma ilkeler ve üzerinde SAP HANA (büyük örnekler) azure'da hazırlama
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb1ed063cb11a82d786badd3f63b2d4b6932ce13
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709722"
---
# <a name="disaster-recovery-principles"></a>Olağanüstü durum kurtarma ilkeleri

HANA büyük örnekleri, farklı Azure bölgelerindeki HANA büyük örneği Damgalar arasında bir olağanüstü durum kurtarma işlevi sunar. Örneğin, ABD Batı bölgesinde Azure HANA büyük örneği birimleri dağıtırsanız, ABD Doğu bölgesinde olağanüstü durum kurtarma birimleri HANA büyük örneği birimleri kullanabilirsiniz. DR bölgesindeki başka bir HANA büyük örneği birim için ödeme yapmanızı gerektirdiğinden daha önce bahsedildiği gibi olağanüstü durum kurtarma otomatik olarak yapılandırılmamış. Olağanüstü durum kurtarma Kurulumu ölçek artırma hem de ölçek genişletme ayarları için çalışır. 

Şu ana kadar dağıtılmış senaryolarda, müşterilerin yüklü bir HANA örneği kullanan üretim dışı sistemlere çalıştırmak için DR bölgesinde birimi kullanın. HANA büyük örneği birim üretim amaçları için kullanılan SKU olarak aynı SKU'ların olması gerekir. Aşağıdaki görüntüde Azure üretim bölgesinde sunucusu birimi arasında hangi disk yapılandırmasını gösterir ve olağanüstü durum kurtarma bölgesindeki şuna benzer:

![Disk açısından DR Kurulum yapılandırması](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Bu genel bakış grafikte gösterildiği gibi ardından birimlerin ikinci bir set sipariş gerekir. Hedef disk birimleri, olağanüstü durum kurtarma birimi üretim örneği için üretim birimler olarak aynı boyuttadır. Bu disk birimleri HANA büyük örneği sunucu biriminde olağanüstü durum kurtarma siteniz ile ilişkilidir. Aşağıdaki birimleri üretim bölgesi DR sitesine çoğaltılır:

- / hana/veri
- / hana/logbackups 
- /hana/shared (/ usr/sap içerir)

SAP HANA işlem günlüğü, bu birimlerin geri yükleme yapılır şekilde gerekli değildir çünkü /hana/log birimin çoğaltılmaz. 

Olağanüstü durum kurtarma işlevlerini temel HANA büyük örneği altyapısı tarafından sağlanan depolama çoğaltma işlev sunulur. Depolama tarafında kullanılan işlevselliği depolama birimine değişikliği olduktan gibi zaman uyumsuz olarak çoğaltın değişiklikleri sabit bir akış değil. Bunun yerine, bu birimlerin anlık görüntülerin düzenli olarak oluşturulan olgu dayanan bir mekanizmadır. Önceden çoğaltılmış bir anlık görüntü ve henüz çoğaltılmaz yeni bir anlık görüntü arasındaki fark, ardından hedef disk birimlere olağanüstü durum kurtarma sitesine aktarılır.  Bu anlık görüntüler, birimlerinde depolanır ve olağanüstü durum kurtarma yük devretme, varsa bu birimlerde geri yüklenmelidir.  

Anlık görüntü arasındaki farkları daha küçük bir veri miktarını raporluyor ilk tam veri biriminin aktarımını olmalıdır. Sonuç olarak, DR sitesi birimlerin her birinde gerçekleştirilen üretim sitede birim anlık görüntülerini içerir. Sonuç olarak, bu DR sistemine üretim sistemine geri alma olmadan kayıp veri kurtarmak için önceki bir duruma almak için kullanabilirsiniz.

Bir HANA büyük örneği biriminde birden çok bağımsız SAP HANA örnekleri ile bir MCOD dağıtımı varsa, tüm SAP HANA örnekleri için DR tarafına çoğaltılan depolama alıyorsanız bekleniyor.

Burada, HANA sistem çoğaltması yüksek kullanılabilirlik işlevi üretim sitenizi ve depolama tabanlı çoğaltma DR sitesi için durumlarda düğümlerinin her iki birincil siteden DR örneğine birimleri çoğaltılır. Hem birincil hem de DR için ikincil çoğaltma uyum sağlamak için DR sitesinde ek depolama alanı (aynı boyut birincil düğüm itibariyle) satın almanız gerekir. 



>[!NOTE]
>HANA büyük Örnek Depolama çoğaltma işlevselliği yansıtma ve depolama anlık görüntüleri çoğaltılıyor. Bu makalede yedekleme ve geri yükleme kısmında tanıtılan depolama anlık görüntüleri gerçekleştirme, olağanüstü durum kurtarma sitesine herhangi bir çoğaltma olamaz. Depolama anlık görüntüsü yürütme, depolama çoğaltma, olağanüstü durum kurtarma siteniz için bir önkoşuldur.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Olağanüstü durum kurtarma senaryosunun hazırlama
Bu senaryoda, Azure bölgesi üretimde HANA büyük örnekler üzerinde çalışan bir üretim sistemine sahip. Aşağıdaki adımları için HANA sistem SID "PRD" olduğunu ve HANA büyük örnekleri DR Azure bölgesi içinde çalışan bir üretim dışı sistemi olduğunu varsayalım. İkincisi, SID'si "." tst'dir olduğunu varsayalım Aşağıdaki resimde, bu yapılandırma gösterilmektedir:

![DR Kurulum başlangıcı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Sunucu örneği zaten varsa, ek depolama alanı birim kümesi, Azure Hizmet Yönetimi ekler üzerinde SAP HANA ile birimlerin ek kümesini TST üzerinde çalıştırdığınız HANA büyük örneği birimine üretim çoğaltma hedefi olarak sipariş edilen HANA örneği. Bu amaç için SID üretim HANA örneğinizin sağlamanız gerekir. Azure hizmet yönetimi üzerinde SAP HANA bu birimlerin eki onayladıktan sonra bu birimlerdeki HANA büyük örneği birimine bağlamak gerekir.

![DR Kurulum sonraki adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Sonraki adım, HANA büyük örneği birimi TST HANA örneği çalıştıracağınız DR Azure bölgesinde ikinci bir SAP HANA örneği yüklemenizi içindir. Yeni yüklenen SAP HANA örneği aynı SID olmalıdır. Oluşturulan kullanıcılar aynı UID ve üretim örneği olan bir grup kimliği olması gerekir. Okuma [yedekleme ve geri yükleme](hana-backup-restore.md) Ayrıntılar için. Yükleme başarılı oldu, geçmeniz gerekir:

- Adım 2 açıklanan depolama anlık görüntü hazırlama / yürütme [yedekleme ve geri yükleme](hana-backup-restore.md).
- Henüz yapmadıysanız, HANA büyük örneği birim DR birim için bir ortak anahtar oluşturun. Adım 3 / açıklanan depolama anlık görüntü hazırlama bkz [yedekleme ve geri yükleme](hana-backup-restore.md).
- Bakımını *HANABackupCustomerDetails.txt* yeni HANA örneği ve test mi depolama bağlantıları aşağıdaki düzgün şekilde çalışır.  
- Yeni yüklenen SAP HANA örneği DR Azure bölgesinde HANA büyük örneği biriminde durdurun.
- Bu PRD birimler çıkarın ve Azure hizmet yönetimi üzerinde SAP HANA ile iletişime geçin. Bunlar depolama çoğaltma hedefi çalışması sırasında erişilebilir olamayacağı için birimler birimine bağlı kalın olamaz.  

![Çoğaltma kurmadan önce DR Kurulum adımı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

İşlemler ekibinin Azure bölgesi üretimde PRD birimleri ve DR Azure bölgesi PRD birimlerin arasındaki çoğaltma ilişkisini oluşturur.

>[!IMPORTANT]
>Çoğaltılmış SAP HANA veritabanı olağanüstü durum kurtarma siteniz olarak tutarlı bir duruma geri yüklemek gerekli olduğundan /hana/log birimin çoğaltılmaz.

Ardından, ayarlayın veya RPO ve RTO olağanüstü durumda almak için depolama anlık görüntü yedekleme zamanlamasını ayarlayın. Kurtarma noktası hedefi en aza indirmek için aşağıdaki çoğaltma aralıkları HANA büyük örneği hizmetinde ayarlayın:
- Birleştirilmiş bir anlık görüntü tarafından kapsanan birimlerin (anlık görüntü türü **hana**), 15 dakikada bir olağanüstü durum kurtarma siteniz olarak eşdeğer depolama birimi hedeflerin için çoğaltma kümesi.
- İşlem günlüğü yedekleme biriminin (anlık görüntü türü **günlükleri**), 3 dakikada bir olağanüstü durum kurtarma siteniz olarak eşdeğer depolama birimi hedeflerin için çoğaltma kümesi.

Kurtarma noktası hedefi en aza indirmek için şunu ayarlayın:
- Gerçekleştirmek bir **hana** türü depolama anlık görüntüsü (bkz: "7. adım: Anlık görüntüleri gerçekleştir:") 30 dakikada 1 saate.
- SAP HANA işlem günlüğü yedeklemeleri her 5 dakikada gerçekleştirin.
- Gerçekleştirmek bir **günlükleri** 5-15 dakikada bir anlık görüntü depolama yazın. Bu aralığı dönem, yaklaşık 25 15 dakikalık bir RPO elde edin.

Bu kurulum, işlem günlüğü yedeklemeleri, depolama anlık görüntüleri ve çoğaltma HANA işlem dizisini yedekleme birimi ve/hana/verilerini günlüğe kaydedebilirsiniz ve (/ usr/sap içerir) /hana/shared Bu grafikte gösterilen veriler gibi görünebilir:

 ![Bir işlem günlüğü yedekleme anlık görüntüsü ve ek yansıtma zaman ekseninde arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Olağanüstü durum kurtarma durumunda daha da iyi bir RPO elde etmek için HANA işlem günlüğü yedeklemeleri SAP HANA (büyük örnekler) azure'da diğer Azure bölgesine kopyalayabilirsiniz. Daha fazla RPO hacmin elde etmek için aşağıdaki adımları gerçekleştirin:

1. HANA hareketi geri oluşturan sık /hana/logbackups için olabildiğince oturum açın.
1. NFS paylaşım barındırılan Azure sanal makineler için işlem günlüğü yedeklemeleri kopyalamak için rsync kullanın. Azure sanal ağları Azure üretim bölgesinde ve DR bölgelerde Vm'leri alır. Her iki Azure sanal ağları üretim HANA büyük örnekleri Azure'a bağlanan bağlantı hattına bağlanmanız gerekmez. Grafikleri görmek [ağ HANA büyük örnekleri ile olağanüstü durum kurtarma değerlendirmeleri](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) bölümü. 
1. NFS için VM'deki bölgedeki işlem günlüğü yedeklemeleri bağlı tutun depolama verildi.
1. Bir olağanüstü durum yük devretme durumda /hana/logbackups birimde daha yakın zamanda gerçekleştirilen ile olağanüstü durum kurtarma siteniz NFS üzerinde işlem günlüğü yedeklerini paylaşmak bulduğunuz işlem günlüğü yedeklemeleri tamamlar. 
1. DR bölgesindeki için üzerinden kaydedilebilir en son yedeği geri yüklemek için bir işlem günlüğü yedeklemesi başlatın.

Veri çoğaltma HANA büyük örneği işlem çoğaltma ilişkisi Kurulum onaylayın ve yürütme depolama anlık görüntüsü yedekleri başlattığınız başlar.

![Çoğaltma kurmadan önce DR Kurulum adımı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

DR Azure bölgeleri PRD birimlerin anlık görüntü çoğaltma ilerledikçe geri yüklenmiyor gibi. Bunlar yalnızca depolanır. Birimlerin böyle bir durumda bağlı olduğundan, DR Azure bölgesi içinde sunucu birimindeki PRD SAP HANA örneği yüklendikten sonra bu birimlerin takılamadı durumu temsil eder. Bunlar ayrıca henüz geri yüklenmez depolama yedeklemeleri temsil eder.

Bir yük devretme varsa, en son depolama anlık görüntüsü yerine daha eski bir depolama anlık görüntü geri yüklemek seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Başvuru [olağanüstü durum kurtarma yük devretme yordamı](hana-failover-procedure.md).
