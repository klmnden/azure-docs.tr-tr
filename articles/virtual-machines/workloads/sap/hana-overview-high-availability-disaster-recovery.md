---
title: "SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs"
description: "Yüksek kullanılabilirlik ve Azure (büyük örnekler) üzerinde SAP HANA olağanüstü durum kurtarma planı oluşturun"
services: virtual-machines-linux
documentationcenter: 
author: saghorpa
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09aa98a35fa8286828a99c49a33a80d5938afe3a
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA büyük örnekleri azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma 

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (DR) kritik SAP HANA Azure (büyük örnekler) sunucuda çalışan önemli yönleri şunlardır. SAP, sistem Tümleştirici ve Microsoft ile düzgün mimari ve sağ yüksek kullanılabilirlik ve olağanüstü durum kurtarma stratejisi uygulamak için iş önemlidir. Ortamınıza özgü kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi dikkate almak önemlidir.

Microsoft, HANA büyük örnekleri ile bazı SAP HANA yüksek kullanılabilirlik özelliklerini destekler. Bu özellikler şunları içerir:

- **Depolama çoğaltma**: depolama sistemin başka bir Azure bölgesindeki başka bir HANA büyük örneği damga tüm veri çoğaltma olanağı. SAP HANA bu yöntem bağımsız olarak çalışır.
- **HANA sistem çoğaltma**: SAP HANA tüm verileri ayrı bir SAP HANA sistemi için çoğaltma. Kurtarma süresi hedefi düzenli aralıklarla veri çoğaltma ile en aza indirilir. SAP HANA zaman uyumsuz ve zaman uyumlu bellek içi ve zaman uyumlu modda destekler. Zaman uyumlu modda aynı veri merkezinde veya küçüktür 100 km birbirinden içinde olan SAP HANA sistemler için önerilir. HANA örneği büyük Damgalar geçerli tasarımında HANA sistem çoğaltma yalnızca yüksek kullanılabilirlik için kullanılabilir. Şu anda HANA sistem çoğaltma başka bir Azure bölgesi olağanüstü durum kurtarma yapılandırmaları için üçüncü taraf ters proxy bileşeni gerekiyor. 
- **Konak otomatik yük devretme**: SAP HANA sistem çoğaltma alternatif olarak kullanmak HANA için bir yerel arıza kurtarma çözümü. Ana düğüm kullanılamaz duruma gelirse, bir veya daha fazla bekleme SAP HANA düğümleri genişleme modunda yapılandırın ve SAP HANA otomatik olarak bir bekleme düğüme yöneltilir.

SAP HANA Azure (büyük örnekler) ile ilgili üç farklı coğrafi bölgeler (ABD, Avustralya ve Avrupa) kapsayan iki Azure bölgelerinden sunulur. İki farklı bölgelerde konak HANA büyük örneği Damgalar depolama anlık görüntüleri çoğaltmak için olağanüstü durum kurtarma yöntemleri sunmak için kullanılan ayrı bir adanmış ağ bağlantı hatları için bağlanır. Çoğaltma varsayılan olarak yapılandırılmadı. Bu, olağanüstü durum kurtarma işlevselliği sıralanmış müşteriler için ayarlanır. Depolama çoğaltma HANA büyük örnekleri için depolama anlık görüntüleri kullanımını bağımlıdır. Farklı bir jeopolitik alanında olan bir DR bölge olarak Azure bölgesini seçmek mümkün değildir. 

Aşağıdaki tabloda, şu anda desteklenen yüksek kullanılabilirlik ve olağanüstü durum kurtarma yöntemleri ve birleşimleri gösterilmektedir:

| HANA büyük durumlarda desteklenen senaryosu | Yüksek kullanılabilirlik seçeneği | Olağanüstü durum kurtarma seçeneği | Yorumlar |
| --- | --- | --- | --- |
| Tek düğüm | Mevcut değil. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu. | |
| Konak otomatik yük devretme: N + m<br /> 1 + 1 dahil | Etkin rolü alma bekleme ile mümkün.<br /> HANA rol anahtarı denetler. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR eşitleme. | HANA birim kümeleri tüm düğümlere (n + m) bağlanmış.<br /> DR sitesi düğümleri aynı sayıda olmalıdır. |
| HANA sistem çoğaltma | Birincil veya ikincil kurulumu ile mümkün.<br /> İkincil bir yük devretme durumunda birincil rolü taşır.<br /> Yük devretme denetim HANA sistem çoğaltma ve işletim sistemi. | Ayrılmış DR kurulumu.<br /> Çok amaçlı DR kurulumu.<br /> Depolama çoğaltma kullanarak DR eşitleme.<br /> HANA sistem çoğaltma kullanarak DR henüz olmayan üçüncü taraf bileşenler mümkün değildir. | Disk birimleri ayrı kümesi her düğüme bağlı.<br /> Yalnızca disk birimleri üretim sitedeki ikincil çoğaltmasının DR konuma çoğaltılan.<br /> Bir dizi birimleri DR sitede gereklidir. | 

Ayrılmış bir DR Kurulum burada DR sitesi HANA büyük örneği biriminde herhangi bir iş yükü veya üretim dışı sistem çalıştırmak için kullanılmayan ' dir. Birim pasif ve yalnızca bir olağanüstü durum yük devretme yürütülürse dağıtılır. Ancak, birçok müşteri için tercih edilen bir seçenek değil.

Çok amaçlı DR Kurulum, DR sitesi HANA büyük örneği biriminde üretim dışı iş yükü çalıştığı ' dir. Olağanüstü durum, durumunda üretim dışı sistemini kapatmak, depolama çoğaltılan (ek) birim kümeleri bağlayın ve sonra üretim HANA örneği başlatın. HANA büyük örneği olağanüstü durum kurtarma işlevselliği kullanmak müşterilerin çoğu, bu yapılandırma kullanın. 


Aşağıdaki SAP makalelerinde SAP HANA yüksek kullanılabilirlik hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA yüksek kullanılabilirlik Teknik İnceleme](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [SAP HANA Yönetim Kılavuzu](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP HANA sistem çoğaltma SAP Akademi Video](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP destek Not #1999880 – SAP HANA sistem çoğaltma hakkında SSS](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [Yukarı destek Not #2165547 – SAP HANA geri SAP ve SAP HANA sistem çoğaltma ortamında geri yükleme](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [En az/sıfır kapalı kalma süresi ile donanım Exchange için destek Not #1984882 – kullanarak SAP HANA sistem çoğaltma SAP](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Olağanüstü durum kurtarma HANA büyük örneğiyle ağ konuları

HANA büyük örneklerinin olağanüstü durum kurtarma işlevsellikten yararlanmak için iki farklı Azure bölgeleri ağ bağlantısı tasarlamanız gerekir. Şirket içi bir Azure ExpressRoute bağlantı hattı bağlantısı, ana Azure bölgesini ve şirket içi olağanüstü durum kurtarma bölgenizi kadar olan başka bir bağlantı hattı bağlantı gerekir. Bu ölçü bir durum kapsayan bir Azure bölgesinde bir sorun olduğunda, bir Microsoft Enterprise sınır yönlendiricisi (MSEE) konum dahil olmak üzere.

İkinci bir ölçü HANA büyük örnekleri diğer bölgede bağlanan bir expressroute bağlantı hattı için bölgeler birinde SAP HANA azure'da (büyük örnekler) bağlanmak tüm Azure sanal ağları bağlanabilir. Bu *arası bağlantı*, bölge # 1'de Azure sanal ağı üzerinde çalışan hizmetleri, bölge #2 ve diğer yönden HANA büyük örneği birimleri bağlanabilir. Bu ölçü, burada Azure ile şirket içi konumunuz bağlanan yalnızca biri MSEE konumları çevrimdışı bir durumda giderir.

Aşağıdaki grafikte bir olağanüstü durum kurtarma için esnek yapılandırması gösterilmektedir:

![Olağanüstü durum kurtarma için en uygun yapılandırma](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-when-you-use-hana-large-instances-storage-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için HANA büyük örnekleri depolama çoğaltma kullandığınızda diğer gereksinimler

Bir olağanüstü durum kurtarma Kurulumu HANA büyük örneğiyle için ek gereksinimler şunlardır:

- SAP HANA Azure (büyük örnekler) SKU'ları üretim SKU'ları olarak aynı boyutta üzerinde sipariş ve bunları olağanüstü durum kurtarma bölgede dağıtabilirsiniz. Geçerli müşteri dağıtımlarında, bu örnekler üretim dışı HANA örnekleri çalıştırmak için kullanılır. Biz olarak başvurmak *çok amaçlı DR kurulumları*.   
- Her, SAP HANA olağanüstü durum kurtarma sitede kurtarmak istediğiniz Azure (büyük örnekler) SKU üzerinde DR sitesinde ek depolama alanı sıralamanız gerekir. Ek depolama alanı satın alma, depolama birimlerini ayırmak olanak tanır. Olağanüstü durum kurtarma Azure bölgesi, üretime Azure bölgesi depolama çoğaltmayı hedefi olan birimler ayırabilirsiniz.
 

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Veritabanları işletim için en önemli yönlerinden bunları yapma çeşitli yıkıcı olaylarından korumaktır. Bu olaylar nedenini herhangi bir şeyin doğal afetler basit kullanıcı hataları olabilir.

Zaman içinde bir noktaya geri yüklemenizi yeteneği olan bir veritabanını, yedekleme (gibi birisi kritik verileri silmeden önce), mümkün olduğunca yakın önce kesintisi olduğu şekilde bir duruma geri yükleme sağlar.

En iyi sonuçlar için yedeklemeler iki tür gerçekleştirilmesi gerekir:

- Veritabanı Yedeklemeleri: tam, artımlı ya da değişiklik yedekleme
- İşlem günlüğü yedeklemeleri

Bir uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemeleri yanı sıra depolama anlık görüntüleri ile yedeklemelerini gerçekleştirebilir. Depolama anlık görüntüleri, işlem günlüğü yedeklemeleri değiştirmeyin. İşlem günlüğü yedeklemeleri, veritabanını zaman içinde belirli bir noktaya geri yüklemenizi veya zaten yürütülen işlemler günlüklerinden boşaltmak için önemli kalır. Ancak, depolama anlık görüntüleri, veritabanının İleri alma görüntüsünü hızlı bir şekilde sağlayarak kurtarma hızlandırabilir. 

SAP HANA Azure (büyük örnekler) üzerinde iki yedekleme ve geri yükleme seçenekleri sunar:

- Bunu kendiniz (DIY). Yeterli disk alanı olduğundan emin olmak için hesaplamak sonra tam veritabanı gerçekleştirmek ve disk yedekleme yöntemleri kullanarak günlük yedeklemelerine. Ya da doğrudan HANA büyük örneği birimlerine veya için ağ dosya paylaşımları (bir Azure sanal makine (VM) ayarlanmış NFS) bağlı birimleri yedekleyebilirsiniz. İkinci durumda, müşterilerin azure'da bir Linux VM ayarlama, Azure Storage VM'e ekleyin ve bu VM'de yapılandırılmış bir NFS sunucusu üzerinden depolama paylaşmak. Doğrudan HANA büyük örneği birimlerine ekleme birimleri karşı yedekleme yapıyorsanız, (Azure depolama birimine dayalı NFS paylaşımlarını dışarı aktaran bir Azure VM ayarladıktan sonra) yedeklemeler için Azure storage hesabı kopyalamanız gerekir. Veya bir Azure yedekleme kasası veya Azure soğuk depolama kullanabilirsiniz. 

   Başka bir seçenek bir Azure depolama hesabı kopyalandıktan sonra yedekleri depolamak için bir üçüncü taraf veri koruma aracını kullanmaktır. Dıy yedekleme seçeneği de uyumluluk ve denetleme amaçları için uzun süreler depolamak için gereken verileri için gerekli olabilir. Her durumda, bir VM ve Azure Storage ile temsil edilen NFS paylaşımlarını içine yedeklemeleri kopyalanır.

- Yedekleme ve geri yükleme SAP HANA Azure (büyük örnekler) üzerinde temel alınan alt yapısını sağlayan işlevselliği. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümde rest HANA büyük örnekleriyle sunulan yedekleme ve geri yükleme işlevselliği giderir. Bu bölümde ayrıca ilişki yedekleme kapsayan ve geri yükleme için olağanüstü durum kurtarma sahip HANA büyük örnekleri tarafından sunulan işlevselliği.

> [!NOTE]
> Altyapının HANA büyük örnekleri tarafından kullanılan anlık görüntü teknolojisi SAP HANA anlık görüntü bir bağımlılığa sahiptir. Bu noktada, SAP HANA anlık görüntüleri SAP HANA çok müşterili veritabanı kapsayıcıların birden çok Kiracı ile birlikte çalışmaz. Bu nedenle, SAP HANA çok müşterili veritabanı kapsayıcılarında birden çok kiracıya dağıttığınızda bu yöntem Yedekleme kullanılamaz. Yalnızca bir kiracı dağıtılır, SAP HANA anlık görüntüleri çalışır.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>SAP HANA depolama anlık görüntüleri (büyük örnekler) Azure üzerinde kullanma

SAP HANA Azure (büyük örnekler) üzerinde temel alınan depolama altyapısını birimlerin depolama anlık görüntüleri destekler. Yedekleme ve geri yükleme birimlerin desteklenir, aşağıdaki noktaları ile:

- Tam veritabanı yedeklemeleri yerine depolama birim anlık görüntülerini üzerinde düzenli aralıklarla alınır.
- Bir anlık görüntü/hana/veri, hana/log ve /hana/shared (/usr/sap içerir) tetiklendiğinde birimleri, depolama anlık görüntü depolama anlık görüntü yürütülmeden önce anlık görüntü bir SAP HANA başlatır. Bu SAP HANA anlık görüntü Kurulum son günlük geri yüklemeler için depolama anlık görüntü kurtarma işleminden sonra noktasıdır.
- Burada depolama anlık görüntü başarıyla yürütüldü noktadan sonra SAP HANA anlık görüntü silinir.
- İşlem günlüğü yedeklemeleri sık gerçekleştirilen ve /hana/logbackups birim veya Azure depolanır. Anlık ayrı olarak almak için işlem günlüğü yedeklemeleri içeren /hana/logbackups birimi tetikleyebilir. Bu durumda, bir HANA anlık görüntüsü yürütülecek gerekmez.
- Microsoft Azure desteği (bir üretim kesinti) veya Azure Hizmet Yönetimi SAP HANA belirli depolama anlık görüntüye geri yüklemek için bir veritabanı belirli bir noktaya zamanında geri yüklemeniz gerekiyorsa, isteyin. Bir planlı bir korumalı alan sistem geri yükleme özgün durumuna örneğidir.
- Depolama anlık görüntüsüne dahil SAP HANA anlık görüntü yürütülen ve depolama anlık görüntü alındıktan sonra depolanan işlem günlüğü yedeklemeleri uygulamak için uzaklık bir noktadır.
- Bu işlem günlüğü yedeklemeleri, veritabanını zaman içinde geri belirli bir noktaya geri yüklemenizi alınır.

Depolama anlık görüntüleri birimleri üç farklı sınıflardaki hedefleme gerçekleştirebilirsiniz:

- Birleştirilmiş bir anlık görüntü/hana/verileri ve (/ usr/sap içerir) /hana/shared üzerinde. Bu anlık görüntü depolama anlık görüntü için hazırlık olarak bir SAP HANA anlık görüntü oluşturulmasını gerektirir. SAP HANA anlık görüntü veritabanı açısından bir depolama tutarlı bir durumda olduğundan emin olun.
- Ayrı bir anlık görüntü/hana/logbackups üzerinden.
- İşletim sistemi için bir bölümü (yalnızca türü ı HANA büyük örneklerinin).


### <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü konuları

>[!NOTE]
>Depolama anlık görüntüleri HANA büyük örneği birimlerine ayrılan depolama alanını kullanabilir. Bu nedenle, aşağıdaki depolama anlık görüntüler ve kaç tane depolama anlık görüntüleri tutmak için zamanlama yönlerini dikkate almanız gerekir. 

SAP HANA Azure (büyük örnekler) üzerinde depolama anlık görüntüleri belirli mekanikleri şunları içerir:

- Belirli bir depolama anlık görüntü (noktasında zaman geçen süre) çok az depolama kullanır.
- Veri içerik değişikliklerini ve dosya depolama biriminde değiştirmek SAP HANA veri içeriği anlık görüntü veri değişikliklerini yanı sıra, özgün blok içeriği depolamak gerekir.
- Sonuç olarak, depolama anlık görüntü boyutunu artırır. Uzun süre anlık görüntü var, daha büyük depolama anlık görüntüsü olur.
- SAP HANA veritabanına birimin depolama anlık görüntü alanı kullanımını daha büyük bir depolama anlık ömrü boyunca yapılan daha fazla değişiklik.

SAP HANA azure'da (büyük örnekler) SAP HANA veri ve günlük birimlerin sabit birim boyutlarını birlikte gönderilir. Bu birimlerin anlık görüntüleri gerçekleştirme birim alanınızı eats. Depolama anlık görüntüleri zamanlamak ne zaman belirlemeniz gerekir. Ayrıca yanı sıra depolama birimleri alanı tüketimini izlemek depoladığınız anlık görüntü sayısını yönetmek gerekir. Masses veri almak veya diğer önemli değişiklikler HANA veritabanına gerçekleştirdiğinizde, depolama anlık görüntüleri devre dışı bırakabilirsiniz. 


Aşağıdaki bölümlerde genel öneriler dahil olmak üzere, bu anlık görüntüleri gerçekleştirmeye yönelik bilgileri sağlayın:

- Donanım 255 anlık görüntüler birim başına karşılayabilir rağmen bu sayının altına de Kal öneririz.
- Depolama anlık görüntüleri gerçekleştirmeden önce izlemek ve boş alan izler.
- Boş disk alanına dayalı depolama anlık görüntü sayısını azaltın. Güvenliğinizi anlık görüntü sayısını azaltın veya birimleri genişletebilirsiniz. Ek depolama alanı 1 terabayttan küçük birimler cinsinden sıralayabilirsiniz.
- SAP HANA SAP platform Geçiş Araçları (R3load) ile veri taşıma veya SAP HANA veritabanlarını yedeklerden geri yükleme gibi etkinlikleri sırasında /hana/data birimdeki depolama anlık görüntüleri devre dışı bırakın. 
- Sırasında daha büyük düzenlemelere SAP HANA tablo depolama anlık görüntüleri, mümkünse kaçınılmalıdır.
- Depolama anlık görüntüler (büyük örnekler) Azure üzerinde SAP HANA olağanüstü durum kurtarma özelliklerini yararlanarak için bir önkoşuldur.

### <a name="setting-up-storage-snapshots"></a>Depolama anlık görüntüleri ayarlama

Depolama anlık görüntüleri HANA büyük örneğiyle ayarlamak için adımlar aşağıdaki gibidir:
1. Perl HANA büyük örnekleri sunucuda Linux işletim sisteminde yüklü olduğundan emin olun.
2. Değiştirme/etc/ssh/ssh\_satır eklemek için yapılandırma _Mac'ler hmac-sha1_.
3. Varsa çalıştırıyorsanız, her bir SAP HANA örnek ana düğüm üzerinde bir SAP HANA yedekleme kullanıcı hesabı oluşturun.
4. SAP HANA HDB istemci tüm SAP HANA büyük örnekleri sunucularına yükleyin.
5. Her bölge üzerinde ilk SAP HANA büyük örnekleri sunucu, anlık görüntü oluşturma denetimleri temel alınan depolama altyapısını erişmek için ortak bir anahtar oluşturun.
6. Yapılandırma dosyasından ve komut dosyalarını kopyalama [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) konumunu **hdbsql** SAP HANA yükleme.
7. Uygun müşteri belirtimleri için gerektiği kadar HANABackupDetails.txt dosyasını değiştirin.

### <a name="step-1-install-the-sap-hana-hdb-client"></a>1. adım: SAP HANA HDB istemci yükleme

SAP HANA Azure (büyük örnekler) üzerinde yüklü Linux işletim sistemi, SAP HANA depolama anlık görüntüleri yedekleme ve olağanüstü durum kurtarma amacıyla yürütmek için gereken komut dosyaları ve klasörleri içerir. Daha yeni sürümlerde denetle [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Komut dosyalarını en son sürümünü 2.1 ' dir.
Ancak, SAP HANA yüklerken HANA büyük örneği birimlerde SAP HANA HDB istemciyi yüklemek üzere sizin sorumluluğunuzdadır olur. (Microsoft HDB istemci veya SAP HANA yüklemez.)

### <a name="step-2-change-the-etcsshsshconfig"></a>2. adım: / etc/değiştirmek ssh/ssh\_yapılandırma

Değişiklik `/etc/ssh/ssh_config` ekleyerek _Mac'ler hmac-sha1_ satır aşağıda gösterildiği gibi:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>3. adım: bir ortak anahtar oluşturma

Depolama anlık görüntü arabirimleri HANA büyük örneği kiracınızın erişimi etkinleştirmek için bir ortak anahtar ile bir oturum açma yapmanız gerekir. Kiracınızda Azure (büyük örnekler) sunucuda ilk SAP HANA üzerinde anlık görüntü oluşturmak için depolama altyapısını erişmek için kullanılacak ortak bir anahtar oluşturun. Ortak anahtarı bir parola depolama anlık görüntü arabirimleri oturum açmak için gerekli değil sağlar. Bir ortak anahtar oluşturma ayrıca parola kimlik bilgileri korumak gerekmez anlamına gelir. SAP HANA büyük örnekleri sunucuda Linux içinde ortak anahtarı oluşturmak için aşağıdaki komutu yürütün:
```
  ssh-keygen –t dsa –b 1024
```
Yeni konumu **_/root/.ssh/id\_dsa.pub**. Gerçek bir parola girmeyin, aksi takdirde her, oturum açtığınızda parolayı girmek için gereklidir. Bunun yerine, seçin **Enter** iki kez oturum açmak için "parola girin" gereksinimini kaldırmak için.

Ortak anahtar klasörlere değiştirerek beklendiği gibi giderilmiştir emin olmak için onay **/root/.ssh/** ve ardından yürütme `ls` komutu. Anahtar mevcut değilse, aşağıdaki komutu çalıştırarak kopyalayabilirsiniz:

![Ortak anahtar şu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Bu noktada, Azure Hizmet Yönetimi SAP HANA başvurun ve ortak anahtar ile verin. Hizmet temsilcisi HANA büyük örneği kiracınız için yontulmuş temel alınan depolama altyapısında kaydetmek için ortak anahtarı kullanır.

### <a name="step-4-create-an-sap-hana-user-account"></a>4. adım: bir SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntü oluşturmaya başlatmak için SAP HANA içinde depolama anlık görüntü betiklerini bir kullanıcı hesabı oluşturmanız gerekir. SAP HANA Studio'dan SAP HANA kullanıcı hesabı bu amaçla oluşturun. Bu hesabı aşağıdaki ayrıcalıklara sahip olmalıdır: **yedekleme yönetici** ve **katalog okuma**. Bu örnekte, kullanıcı adı olan **SCADMIN**. HANA Studio'da oluşturulan kullanıcı hesabı adı büyük/küçük harf duyarlıdır. Seçtiğinizden emin olun **Hayır** kullanıcının sonraki oturum açma parolasını değiştirmesine izin gerektirme.

![Bir kullanıcı HANA Studio'da oluşturma](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>5. adım: SAP HANA kullanıcı hesabı yetkilendirme

Bu adımda, komut dosyaları parolaları çalışma zamanında göndermek gerekmemesi oluşturduğunuz, SAP HANA kullanıcı hesabı yetkilendirin. SAP HANA komutu `hdbuserstore` bir veya daha fazla SAP HANA düğümde depolanan bir SAP HANA kullanıcı anahtarı oluşturulmasını sağlar. Kullanıcı anahtarı parolalardan komut dosyası işlemi içinde yönetmek zorunda kalmadan SAP HANA kullanıcı erişimi sağlar. Komut dosyası oluşturma işlemi daha sonra ele alınmıştır.

>[!IMPORTANT]
>Aşağıdaki farklı çalıştır komutunu `root`. Aksi takdirde, komut dosyası düzgün çalışamaz.

Girin `hdbuserstore` gibi komut:

**Olmayan MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

Aşağıdaki örnekte, kullanıcının olduğu **SCADMIN01**, konak adı **lhanad01**, ve örnek numarasını **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Bir SAP HANA genişleme yapılandırmanız varsa, tüm komut dosyası tek bir sunucudan yönetmeniz gerekir. Bu örnekte, SAP HANA anahtar **SCADMIN01** her ana bilgisayarın hangi ana anahtarıyla ilgili yansıtır şekilde değiştirilmesi gerekir. SAP HANA yedekleme hesabı HANA DB örneği sayısıyla düzeltmek. Anahtar atandığı ana bilgisayar üzerinde yönetici ayrıcalıklarına sahip olmalıdır ve genişleme yapılandırmaları için Yedekleme kullanıcı tüm SAP HANA örnekleri için erişim haklarına sahip olmalıdır. Üç genişleme düğümü varsayılarak adlara sahip olması **lhanad01**, **lhanad02**, ve **lhanad03**, komut dizisi şöyle görünür:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>6. adım: anlık görüntü komut dosyalarını almak, anlık görüntüleri yapılandırma ve test yapılandırması ve bağlantı

Gelen komut dosyalarını en son sürümünü indirme [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). İndirilmiş betikler ve metin dosyası için çalışma dizini kopyalayın **hdbsql**. Geçerli HANA yüklemeler için bu gibi /hana/shared/D01/exe/linuxx86 dizindir\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl 
HANABackupCustomerDetails.txt 
``` 


Dosya ve farklı komut dosyaları amacı şöyledir:

- **Azure\_hana\_backup.pl**: cron depolama anlık görüntüleri HANA veri/günlük/paylaşılan birimler, / hana/logbackups birim ya da OS (örneklerinde Type ı SKU'ları, HANA büyük) yürütmek için bu kodla zamanlayın.
- **Azure\_hana\_çoğaltma\_status.pl**: Bu komut dosyası üretim site çoğaltma durumu olağanüstü durum kurtarma sitesine geçici temel ayrıntıları sağlar. Çoğaltma yer aldığı ve öğelerin boyutu, gösterir emin olmak için komut dosyası izleyicileri çoğaltılmış olduğunu. Ayrıca Kılavuzu bir çoğaltma çok uzun sürüyorsa veya bağlantı kapalı olduğunda sağlar.
- **Azure\_hana\_anlık görüntü\_details.pl**: Bu komut, ortamınızda bulunan tüm anlık görüntüleri, birim başına hakkında temel ayrıntılar listesini sağlar. Bu komut, birincil sunucuda veya sunucu birim olağanüstü durum kurtarma konumu olarak çalıştırılabilir. Komut dosyası anlık görüntüleri içeren her bir birim ayrıntılarıyla aşağıdaki bilgileri sağlar:
   * Bir birimdeki toplam anlık görüntü boyutu
   * Her anlık görüntü o birimdeki aşağıdaki ayrıntıları içerir: 
      - Anlık görüntü adı 
      - Oluşturma işlemi süresi 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme ilgiliyse bu anlık görüntü ile ilişkili kimliği
- **Azure\_hana\_anlık görüntü\_delete.pl**: Bu komut dosyasını bir depolama anlık görüntü veya anlık görüntü kümesi siler. SAP HANA yedekleme kimliği HANA Studio'da bulunan olarak veya depolama anlık görüntü adı kullanabilirsiniz. Şu anda, yedekleme kimliği yalnızca HANA veri/günlük/paylaşılan birimler için oluşturulan anlık görüntülerin bağlıdır. Aksi takdirde, girilen anlık görüntü kimliği eşleşen tüm anlık görüntüleri aradığı anlık görüntü kimliği girilirse,  
- **testHANAConnection.pl**: Bu komut dosyası SAP HANA örneği bağlantısını test eder ve depolama anlık görüntüler ayarlamak için gereklidir.
- **testStorageSnapshotConnection.pl**: Bu komut dosyası iki amaca sahiptir. İlk olarak, betikleri çalıştırır HANA büyük örneği birim atanan depolama sanal makine ve depolama anlık görüntü arabirimi, HANA büyük örneklerinin erişim sahip olmasını sağlar. İkinci amacı test ettiğiniz HANA örneği için geçici bir anlık görüntü oluşturmaktır. Bu komut dosyası her HANA örneği için yedekleme betikleri beklendiği gibi çalışmasını sağlamak için bir sunucu üzerinde çalıştırmanız gerekir.
- **removeTestStorageSnapshot.pl**: Bu komut dosyası komut dosyası ile oluşturulan anlık görüntü test siler **testStorageSnapshotConnection.pl**. 
- **HANABackupCustomerDetails.txt**: Bu dosya, SAP HANA yapılandırmanızı uyum değişiklik yapmanız değiştirilebilir yapılandırma dosyadır.

 
Depolama anlık görüntüleri çalışan komut için Denetim ve yapılandırma dosyası HANABackupCustomerDetails.txt dosyasıdır. Dosya amaçlıdır ve kurulum için ayarlayın. Almış olmanız gerekir **depolama yedekleme adı** ve **depolama IP adresi** SAP HANA örneklerinizi dağıtıldığında Azure Hizmet Yönetimi'nden. Sıralama ya da bu dosyasındaki değişkenleri hiçbirini aralığı dizisi değiştiremezsiniz. Aksi takdirde komut dosyaları düzgün çalışması için yapmayacağınız. Ayrıca, IP adresini büyütme düğüm veya ana düğüm (genişleme varsa) SAP HANA Azure Hizmet Yönetimi aldığınız. SAP HANA yükleme sırasında aldığınız HANA örnek numarasını da bildiğiniz. Şimdi yedek adı yapılandırma dosyasına eklemeniz gerekir.

Depolama yedek adı ve depolama IP adresi doldurulmuş sonra büyütme veya genişleme dağıtımı için yapılandırma dosyasını aşağıdaki gibi görünür. Yapılandırma dosyasında aşağıdaki veri doldurmanız gerekir:
- Tek düğüm veya ana düğüm IP adresi
- HANA örnek sayısı
- Yedek adı 
    
```
#Provided by Microsoft Service Management
Storage Backup Name: client1hm3backup
Storage IP Address: 10.240.20.31
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 
Node 1 HANA instance number:
Node 1 HANA userstore Name:
```

>[!NOTE]
>Şu anda yalnızca düğüm 1 ayrıntıları gerçek HANA depolama anlık görüntü komut dosyasında kullanılır. Ana yedekleme düğümü hiç değişirse, zaten başka bir düğüme onun yerine düğüm 1 ayrıntıları değiştirerek sürebilir sağladıktan böylece veya tüm HANA düğümlerinden erişim sınamanızı öneririz.

Tüm yapılandırma verilerini HANABackupCustomerDetails.txt dosyaya yerleştirin sonra yapılandırmaları ilgili HANA örnek verileri doğru olup olmadığını kontrol gerekir. Komut dosyası kullanma `testHANAConnection.pl`. Bu komut dosyasını bir SAP HANA büyütme veya genişleme yapılandırmasını bağımsızdır.

```
testHANAConnection.pl
```

Bir SAP HANA genişleme yapılandırmanız varsa, ana HANA örneği gerekli HANA sunucuları ve örneklerine erişimi olduğundan emin olun. Test komut dosyasına bir parametre yok ancak komut dosyasının düzgün çalışması HANABackupCustomerDetails.txt yapılandırma dosyasına verilerinizi eklemeniz gerekir. Yalnızca kabuk komutu hata kodları döndürülür, bu komut dosyası hata denetlemek için her örnek onun mümkün değildir. Betik bazı yararlı açıklamaları, iki kez kontrol etmek için buna rağmen sağlar.

Komut dosyasını çalıştırmak için aşağıdaki komutu girin:
```
 ./testHANAConnection.pl
```
Komut dosyasını başarıyla HANA örneğinin durumunu elde ederse HANA bağlantısı başarılı bir ileti görüntüler.


Sonraki test HANABackupCustomerDetails.txt yapılandırma dosyasına put verileri temel alan depolama bağlantısını denetleyin ve sonra bir test anlık görüntü yürütecek adımdır. Yürütmeden önce `azure_hana_backup.pl` komut dosyası, bu test yürütmeniz gerekir. Bir birim anlık görüntü yok içeriyorsa, birim boş olup olmadığını veya anlık görüntü ayrıntıları almak için bir SSH hatası olup olmadığını belirlemek mümkün değildir. Bu nedenle, iki adım komut dosyasını çalıştırır:

- Kiracının sanal makine depolama ve arabirimleri anlık görüntüleri yürütülecek komut dosyaları için erişilebilir olduğunu doğrular.
- HANA örneği tarafından her birim için bir test veya kukla, anlık görüntü oluşturur.

Bu nedenle, HANA örneği bağımsız değişken olarak dahil edilir. Yürütme başarısız olursa, depolama bağlantısı denetlenirken hata sağlamak mümkün değil. Olsa bile hata denetimi, betik yararlı ipuçları sağlar.

Komut dosyası olarak çalıştırın:
```
 ./testStorageSnapshotConnection.pl <HANA SID>
```
Ardından, depolama birimine önceki kurulum adımlarını ve HANABackupCustomerDetails.txt dosyasında yapılandırılmış verilerle sağlanan ortak anahtar kullanarak oturum açmak betik çalışır. Oturum açma başarılı olursa, aşağıdaki içeriği gösterilir:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Sorunları depolama konsol bağlanırken meydana gelirse, çıktı şu şekildedir:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Bir başarılı oturum açma depolama sanal makine arabirimlerine sonra komut dosyasını #2. Aşama ile devam eder ve bir test anlık görüntüsü oluşturur. Çıkış, SAP HANA üç düğümlü genişleme yapılandırılmasını için burada gösterilir:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Test anlık görüntü ile komut dosyası başarıyla çalıştırıldı, gerçek depolama anlık görüntüleri yapılandırma ile devam edebilirsiniz. Başarılı olmazsa, şimdi geçmeden önce sorunları araştırın. İlk gerçek anlık görüntüleri tümü tamamlanıncaya kadar test anlık görüntü geçici kalmalı.


### <a name="step-7-perform-snapshots"></a>7. adım: anlık görüntüler gerçekleştirin

Tüm hazırlık adımları tamamlandığı depolamanın anlık görüntü yapılandırmasını başlatabilirsiniz. SAP HANA ölçek büyütme ve ölçek genişletme yapılandırmalarla zamanlanması için komut dosyası çalışır. Cron aracılığıyla betiklerinin yürütülmesi zamanlamanız gerekir. 

Anlık görüntü yedeklerini üç tür oluşturulabilir:
- **HANA**: anlık görüntü yedekleme içinde/hana/verileri içeren birimlerin ve/hana paylaşılan (/usr/sap de içeren) / ele alınmıştır koordineli bir anlık görüntü tarafından birleştirilmiş. Tek dosya geri yükleme, bu anlık görüntüden mümkündür.
- **Günlükleri**: / hana/logbackups birim anlık görüntüsü yedeği. Bu depolama anlık görüntü yürütmek için hiç HANA anlık görüntü tetiklenir. Bu depolama birimi SAP HANA işlem günlüğü yedeklemeleri içeren yönelik bir birimdir. SAP HANA işlem günlüğü yedeklemeleri günlük büyüme kısıtlamak ve olası veri kaybını önlemek için daha sık gerçekleştirilir. Tek dosya geri yükleme, bu anlık görüntüden mümkündür. Altında beş dakika sıklığını azaltın değil.
- **Önyükleme**: HANA büyük örneğinin önyükleme mantıksal birim numarası (LUN) içeren birimin anlık görüntüsünü. Bu anlık görüntü yedekleme yalnızca Type ı SKU'ları, HANA büyük örnekleriyle mümkündür. LUN önyükleme içeren birim anlık görüntüden geri yüklemeler tek dosyalı gerçekleştiremezsiniz. Türü II SKU'ları, HANA büyük örnekleri için işletim sistemi düzeyinde yedek alın ve tek tek dosyaların de geri yükleyin. Belge başvurun "[türü II SKU'ları için işletim sistemi Yedekleme gerçekleştirmek için nasıl](os-backup-type-ii-skus.md)" daha fazla ayrıntı için.


Bu anlık görüntüleri üç farklı türde çağrı sözdizimi şu şekildedir:
```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <HANA SID> manual 30

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <HANA SID> manual 30

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot none manual 30

```

Şu parametrelerin belirtilmesi gerekir:

- İlk parametre anlık görüntü yedekleme türünü belirtir. İzin verilen değerler: **hana**, **günlükleri**, ve **önyükleme**. 
- İkinci parametre **HANA SID** (ister HM3) veya **hiçbiri**. İlk parametre değeri olduğu sağladıysanız **hana** veya **günlükleri**, bu parametrenin değerini ise **HANA SID** (HM3 gibi), aksi takdirde, önyükleme birimi yedekleme için değerdir**hiçbiri**. 
- Üçüncü bir anlık görüntüsü veya anlık görüntü türü için yedekleme etiketi parametresidir. İki amaca sahiptir. Bu anlık görüntüler hakkında nelerdir bildiğiniz bir amaç için bir ad vermek için böylelikle. Komut dosyası azure için ikinci amaçtır\_hana\_bu özel etiketin altında korunur depolama anlık görüntü sayısını belirlemek için backup.pl. Aynı türde iki depolama anlık görüntü yedekleme zamanlarsanız (gibi **hana**), iki farklı etiketleri ve 30 anlık görüntüleri saklanması her biri için 60 depolama birimlerin anlık görüntüleri etkilenen ile sonuna kalacaklarını tanımlayın. 
- Dördüncü parametrenin önekiyle tutulması için aynı anlık görüntü (etiketi) anlık görüntü sayısını tanımlayarak anlık görüntü saklama dolaylı olarak tanımlar. Bu parametre, zamanlanmış bir yürütme cron aracılığıyla için önemlidir. 

Bir genişleme söz konusu olduğunda, komut dosyası bazı ek tüm HANA sunucuların erişebildiğinden emin olmak için denetimi gerçekleştirir. Komut dosyası, ayrıca bir SAP HANA anlık görüntüsünü oluşturmadan önce dönüş örneklerinin uygun durumu HANA hepsinin denetler. SAP HANA anlık görüntü depolama anlık görüntü tarafından izlenir.

Betik yürütme işlemi `azure_hana_backup.pl` aşağıdaki üç ayrı aşamaya anlık görüntü depolama oluşturur:

1. SAP HANA anlık görüntü yürütür
2. Bir depolama anlık görüntü yürütür
3. Depolama anlık görüntü çalışmaya başlamadan önce oluşturulan SAP HANA anlık görüntü kaldırır

Betik yürütmek için onu HDB yürütülebilir klasöründen kopyalanmıştır çağırın. 

Saklama dönemi komut yürüttüğünüzde, parametre olarak gönderilen anlık görüntü sayısı yönetilen (gibi **30**, daha önce gösterilen). Bu nedenle, depolama anlık görüntüleri tarafından kapsanan süreyi ikisinden biri bir işlev değil: yürütme ve komut dosyası yürütülürken bir parametre olarak gönderilen anlık görüntü sayısını süre. Tutulur anlık görüntü sayısını komut dosyası, eski depolama anlık görüntü aynı etiketinin çağrısı bir parametresi olarak adlı sayıyı aşıyor varsa (önceki örnekte **el ile**) yeni bir anlık görüntü yürütülmeden önce silinir. Çağrının son parametre sayısı tutulur anlık görüntü sayısını denetlemek için kullanabileceğiniz olarak sayı, verin. Bu numara ile aynı zamanda, dolaylı olarak, anlık görüntüleri için kullanılan disk alanı denetleyebilirsiniz. 

> [!NOTE]
>Etiketi değiştirmek hemen sayım yeniden başlatır. Başka bir deyişle, anlık görüntülerin yanlışlıkla silinmez şekilde etiketleme katı olması gerekir.

### <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türleri için anlık görüntü sıklığı olup, HANA büyük örneği olağanüstü durum kurtarma işlevselliği veya kullanmasına bağlı olarak değişir. Depolama anlık görüntü HANA büyük örnekleri olağanüstü durum kurtarma işlevselliğini kullanır. Depolama anlık görüntü bağlı depolama anlık görüntü sıklığı ve yürütme dönemlerini bakımından özel bazı öneriler gerektirebilir. 

Önemli noktalar ve izleyin önerileri bunu yapmanızı varsayıyoruz *değil* HANA büyük örnekleri sunar olağanüstü durum kurtarma işlevini kullanın. Bunun yerine, yedeklemeler ve son 30 gün için zaman içinde nokta kurtarma sağlamak için bir yol olarak depolama anlık görüntüleri kullanın. Anlık görüntüler ve alan sayısı sınırlamaları verildiğinde, müşterilerin aşağıdaki gereksinimleri göz önünde bulundurduğunuzdan:

- Zaman içinde nokta kurtarma için kurtarma süresi.
- Kullanılan alan.
- Kurtarma noktası hedefi ve potansiyel olağanüstü durum kurtarma için kurtarma süresi hedefi.
- Diskleri karşı HANA tam veritabanı yedeklemeleri Son Yürütme. Tam veritabanı yedeklemesi her diskleri karşı veya **backint** arabirimi gerçekleştirilir, depolama anlık görüntüleri yürütülmesi başarısız olur. Tam veritabanı yedeklemeleri depolama anlık görüntüleri üstünde yürütme planlıyorsanız, depolama anlık görüntüleri yürütülmesi bu süre boyunca devre dışı bırakıldığından emin olun.
- Birim başına anlık görüntü sayısı 255 sınırlıdır.


Olağanüstü durum kurtarma işlevselliği HANA büyük örneklerinin kullanmayan müşteriler için anlık görüntü dönemi daha az sıklıkta belirtir. Böyle durumlarda, biz birleşik anlık görüntüleri /hana/data ve (/usr/sap içerir) /hana/shared 12 veya 24 saat dönemlerini gerçekleştirme müşterileri görmek ve bütün bir ayı kapsayacak şekilde anlık görüntüleri tutmak. Aynı günlük yedekleme birimi anlık görüntüleri ile geçerlidir. Ancak, SAP HANA işlem günlüğü yedeklemeleri günlük yedekleme birimi karşı yürütülmesi 15 dakikalık dönem için 5 dakika içinde gerçekleşir.

Cron kullanarak zamanlanmış depolama anlık görüntüleri gerçekleştirmenizi öneririz. Ayrıca tüm yedeklemeler için aynı komut dosyasını kullanın ve olağanüstü durum kurtarma gereksinimleri öneririz. Komut dosyasını değiştirmek gereken çeşitli eşleşecek şekilde girişleri İstenen yedekleme kez. Bu anlık görüntüleri tüm farklı cron kendi yürütme süresi bağlı olarak, zamanlanan: saat, 12 saatlik, günlük veya haftalık. 

/Etc/crontab cron zamanlamada örneği şuna benzeyebilir:
```
00 1-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 46
10 00 * * *  ./azure_hana_backup.pl hana HM3 dailyhana 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log HM3 dailylogback 28
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```
Önceki örnekte, yok/hana/verileri içeren birimlerin ve (/ usr/sap içerir) /hana/shared kapsayan bir saatlik birleşik anlık görüntü konumları. Bu tür bir anlık görüntü daha hızlı zaman içinde nokta için bir kurtarma son iki gün içinde kullanılabilir. Ayrıca, bu birimlerde günlük bir anlık görüntü yok. Bu nedenle, kapsamı iki gün saatlik anlık görüntüleri artı kapsam dört hafta tarafından günlük anlık görüntüleri var. Ayrıca, işlem günlüğü yedekleme birimi her gün bir kez desteklenir. Bu yedeklemeler de dört hafta boyunca tutulur. Crontab üçüncü satırda gördüğünüz gibi HANA işlem günlüğü yedeklemesi her beş dakikada yürütülecek şekilde zamanlanır. Depolama anlık görüntüleri yürütme farklı cron işlerinin başlangıç dakika dağılır, böylece bu anlık görüntülerin belirli bir noktada aynı anda zamanında yürütülmez. 

Aşağıdaki örnekte, / hana/veri ve/hana paylaşılan / (dahil olmak üzere/usr/sap) konumları saatlik düzenli olarak içeren birimlere kapsayan birleşik bir anlık görüntü gerçekleştirin. Bu anlık görüntüler için iki gün tutun. İşlem günlüğü yedekleme birimlerin anlık görüntüleri beş dakikalık temelinde yürütülür ve dört saat için tutulur. Olarak daha önce HANA işlem günlük dosyasının yedeğini beş dakikada yürütülecek şekilde zamanlanır. İşlem günlüğü yedeklemesi başlatıldıktan sonra işlem günlüğü yedekleme biriminin anlık görüntüsü ile iki dakikalık bir gecikmeyle gerçekleştirilir. Bu iki dakika içinde SAP HANA işlem günlüğü yedeklemesi normal koşullarda tamamlanmalıdır. Olarak önce önyükleme içeren birimi LUN günde bir kez yedekleme depolama anlık görüntü tarafından yedeklenir ve dört hafta boyunca tutulur.

```
10 0-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log HM3 logback 48
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```

Aşağıdaki grafikte LUN önyükleme hariç önceki örnekte, dizileri gösterilmektedir:

![Yedeklemeler ve anlık görüntüleri arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanına yapılan değişiklikleri belgelemek için /hana/log birim karşı normal yazma işlemleri gerçekleştirir. Düzenli aralıklarla, SAP HANA /hana/data birime bir kayıt noktası yazar. Belirtildiği gibi crontab içinde bir SAP HANA işlem günlüğü yedeklemesi her beş dakikada çalıştırılır. SAP HANA anlık görüntü /hana/data ve /hana/shared birimlerinin birleşik depolama anlık görüntü tetikleme sonucunda her saat yürütülür de görebilirsiniz. Birleştirilmiş depolama anlık görüntü HANA anlık görüntü başarılı olduktan sonra yürütülür. Crontab belirtildiği gibi depolama anlık görüntü /hana/logbackup birimde her beş dakika, yaklaşık iki dakika HANA işlem günlüğü yedeklemesi sonra yürütülür.


>[!IMPORTANT]
> SAP HANA yedeklemeler için depolama anlık görüntüleri kullanımını, yalnızca anlık görüntüleri SAP HANA işlem günlüğü yedeklemeleri ile birlikte gerçekleştirildiğinde faydalıdır. Bu işlem günlüğü yedeklemeleri dönemleri depolama anlık görüntüleri arasında karşılamayabilir olması gerekir. 

Kullanıcılara taahhüdü 30 günlük bir zaman içinde nokta kurtarma oluşturmadıysanız, aşağıdakileri yapın:

- Olağanüstü durumlarda, bir birleşik depolama anlık görüntü üzerinde/hana/veri ve /hana/shared olan 30 günden eski erişim olanağı gerekir.
- Herhangi bir birleşik depolama anlık görüntüyü arasındaki süreyi kapsar bitişik işlem günlüğü yedeklemeleri vardır. Bu nedenle, en eski birimin anlık görüntüsünü işlem günlüğü yedekleme 30 günden daha eski olması gerekir. Azure depolama alanında bulunan başka bir NFS paylaşımına işlem günlüğü yedeklemeleri kopyalarsanız, bu durumda değil. Bu durumda, bir NFS paylaşımına eski işlem günlüğü yedeklemeleri çekme.

Depolama anlık görüntüler ve işlem günlüğü yedeklemeleri nihai depolama çoğaltması yararlanmak için işlem günlüğü yedeklemeleri SAP HANA Yazar konumunu değiştirmeniz gerekir. Bu değişiklik HANA Studio'da yapabilirsiniz. SAP HANA tam günlük kesimleri otomatik olarak yedekler de, bir günlük yedekleme aralığını belirleyici olacak şekilde belirtmeniz gerekir. Olağanüstü durum kurtarma seçeneğini kullandığınızda, genellikle günlük yedeklerini belirleyici bir süresi olan yürütmek istediğiniz çünkü bu özellikle doğrudur. Şu durumda biz günlük yedekleme aralığı 15 dakika sürdü.

![SAP HANA yedekleme zamanlaması SAP HANA Studio'da günlüğe kaydeder](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Her 15 dakikadan daha sık yedeklemeler seçebilirsiniz. Bu, sık olağanüstü durum kurtarma ile birlikte gerçekleştirilir. Bazı müşteriler, işlem günlüğü yedeklemeleri her beş dakikada gerçekleştirin.  

Veritabanı hiçbir zaman yedeklendiğinden, son yedekleme kataloğu içinde bulunması gereken tek bir yedekleme girişi oluşturmak için bir dosya tabanlı veritabanı yedeklemesi gerçekleştirmeyi adımdır. Aksi takdirde, SAP HANA belirtilen günlük Yedeklemelerinizin başlatamaz.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı bir yedek alın](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


6. adımda yürütüldüğü test anlık görüntü Ayrıca, ilk başarılı depolama anlık görüntüleri yürütülen sonra silebilirsiniz. Bunu yapmak için komut dosyasını çalıştırmak `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Sayısını ve boyutunu disk birimi anlık görüntü izleme

Bir depolama biriminde sayısı anlık görüntüler ve bu anlık görüntülerin depolama kullanımını izleyebilirsiniz. `ls` Değil Göster komutu anlık görüntü dizini ya da dosyaları. Ancak, Linux işletim sistemi komutu `du` aynı birimlerde depolandığından, bu depolama anlık görüntülerin ayrıntılarını gösterir. Komut aşağıdaki seçenekler kullanılabilir:

- `du –sh .snapshot`: Anlık görüntü dizini içindeki tüm anlık görüntüleri toplam sağlar.
- `du –sh --max-depth=1`: Kaydedilen tüm anlık görüntüleri listeler **.snapshot** klasörü ve her anlık görüntü boyutu.
- `du –hc`: Tarafından tüm anlık görüntüleri kullanılan toplam boyutu sağlar.

Birimler üzerinde tüm depolama alınan ve depolanan anlık görüntüleri kullanma değil emin olmak için bu komutları kullanın.

>[!NOTE]
>LUN önyükleme anlık görüntüleri önceki komutlarla görünür değildir.

### <a name="getting-details-of-snapshots"></a>Anlık görüntü ayrıntıları alınıyor
Anlık görüntü daha fazla bilgi edinmek için de komut dosyasını kullanabilirsiniz `azure_hana_snapshot_details.pl`. Olağanüstü durum kurtarma konumu active server ise bu komut dosyası iki konumdan birinde çalıştırabilirsiniz. Komut dosyası anlık görüntüleri içeren her bir birim ayrıntılarıyla aşağıdaki çıkış sağlar: 
   * Bir birimdeki toplam anlık görüntü boyutu
   * Her anlık görüntü o birimdeki aşağıdaki ayrıntıları içerir: 
      - Anlık görüntü adı 
      - Oluşturma işlemi süresi 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme ilgiliyse bu anlık görüntü ile ilişkili kimliği

Komut dosyası yürütme söz dizimi şu şekildedir:

```
./azure_hana_snapshot_details.pl 
```

Komut dosyası HANA yedekleme Kimliği almak çalışacağından, SAP HANA örneğine bağlanması gerekir. Yapılandırma dosyası doğru şekilde ayarlanması HANABackupCustomerDetails.txt gerektiriyor. İki anlık görüntüsü bir birimdeki çıktısı şuna benzeyebilir:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Dosya düzeyinde depolama anlık görüntü geri yükleme
Anlık Görüntü türleri hana ve günlükleri için anlık görüntü doğrudan birimlerin erişebilir **.snapshot** dizini. Her anlık görüntü için bir alt yoktur. Anlık görüntüden o alt gerçek dizin yapıda noktasında önceki durumunda anlık kapsamındaki her dosyasını kopyalamanız.

>[!NOTE]
>Tek dosya geri yükleme LUN önyükleme anlık görüntüler için çalışmaz. **.Snapshot** directory önyükleme LUN gösterilmez. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Bir sunucu üzerinde anlık görüntü sayısını azaltır

Daha önce açıklandığı gibi belirli etiketleri depoladığınız anlık görüntü sayısını azaltabilirsiniz. Son iki bir anlık görüntü başlatmak için komut etiketi ve korumak istediğiniz anlık görüntü sayısını parametreleridir.

```
./azure_hana_backup.pl hana HM3 hanadaily 30
```

Önceki örnekte, anlık görüntü etiketidir **müşteri** ve korunacak bu etiketle anlık görüntü sayısını **30**. Disk alanı tüketimini yanıt olarak depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Son parametre kümesi komut dosyasını çalıştırmak için 15'e, örneğin, anlık görüntü sayısını azaltmak için kolay yoludur **15**:

```
./azure_hana_backup.pl hana HM3 hanadaily 15
```

Bu ayar betik çalıştırırsanız, yeni depolama anlık görüntüsü de dahil olmak üzere anlık görüntü sayısını 15'tir. 15 eski anlık görüntüleri silindi ancak 15 en son anlık görüntüleri korunur.

 >[!NOTE]
 > Birden fazla bir saat öncesine anlık görüntüler varsa bu komut dosyası anlık görüntü sayısını azaltır. Komut dosyası değerinden bir saat öncesine anlık görüntüleri silmez. Bu kısıtlamalar sunulan isteğe bağlı olağanüstü durum kurtarma işlevselliği ilişkilidir.

Belirli bir yedekleme etiketin anlık görüntü kümesi korumak istiyorsanız, **hanadaily** sözdizimi örneklerde, komut dosyasıyla yürütebilir **0** bekletme sayı olarak. Bu etiket eşleşen tüm anlık görüntüleri kaldırır. Ancak, tüm anlık görüntüleri kaldırma olağanüstü durum kurtarma özelliklerini etkileyebilir.

Betik kullanmak için belirli anlık görüntüleri silmek için ikinci olasılığı olan `azure_hana_snapshot_delete.pl`. Bu komut dosyası, bir anlık görüntü veya HANA Studio'da veya anlık görüntü adı aracılığıyla bulunan olarak HANA yedekleme kimliği kullanarak ya da anlık görüntü kümesini silmek için tasarlanmıştır. Şu anda, yedekleme kimliği yalnızca için oluşturulan anlık görüntülerin bağlıdır **hana** anlık görüntü türünde. Yedekleme türü anlık görüntü **günlükleri** ve **önyükleme** SAP HANA anlık görüntü gerçekleştirmeyin. Bu nedenle, bu anlık görüntüler için bulunacak yedekleme hiç kimlik yok. Anlık görüntü adı girilirse, tüm anlık görüntüleri girilen anlık görüntü adı ile eşleşmesi farklı birimlerde arar. Betik çağrısı sözdizimi şöyledir:

```
./azure_hana_snapshot_delete.pl 

```

Kullanıcı olarak komut dosyası yürütme **kök**.

Bir anlık görüntü seçerseniz, tek tek her anlık görüntü silme becerisine sahip. Anlık görüntü içeren birimi ilk sağlayın ve sonra anlık görüntü adı sağlayın. Anlık görüntü o birim varsa ve birden fazla bir saat öncesine silinir. Birim adları ve anlık görüntü adları yürüterek bulabileceğiniz `azure_hana_snapshot_details` komut dosyası. 

>[!IMPORTANT]
>Yalnızca silmekte olduğunuz anlık görüntü üzerinde var olan verilerin ise, silme yürütüyorsa sonra veri sonsuza dek kaybolur.

   

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüye kurtarma

Bir üretim aşağı senaryo karşılaşırsanız, Microsoft Azure desteğiyle müşterilerde olarak depolama anlık görüntüden kurtarma işlemi başlatılabilir. Veriler bir üretim sisteminde silinir ve üretim veritabanını geri yüklemek için verileri almak için tek yol ise yüksek aciliyet sağlasa da olacaktır.

Farklı bir durumda bir zaman içinde nokta kurtarma düşük aciliyet olabilir ve önceden gün planlı. Yüksek öncelikli bir sorun oluşturma yerine, bu kurtarma SAP HANA ile Azure Hizmet Yönetimi planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılım yükseltmeyi planlama. Ardından geliştirme paket yükseltmeden önce durumunu temsil eden bir anlık görüntü geri dönmek gerekir.

İstek göndermeden önce hazırlamanız gerekir. SAP HANA Azure Hizmet Yönetimi ekipteki isteği işlemek ve geri yüklenen birimleri sağlayın. Daha sonra anlık görüntü tabanlı HANA veritabanını geri yükleyin. İstek için hazırlamak nasıl şöyledir:

>[!NOTE]
>Kullanıcı arabirimi, kullanmakta olduğunuz SAP HANA yayın bağlı olarak aşağıdaki ekran görüntüleri kadar değişiklik gösterebilir.

1. Anlık görüntüyü geri yüklemek için karar verin. Aksi takdirde toplamasını sürece yalnızca hana/veri birimi geri yüklendi. 

2. HANA örneği kapatın.

 ![HANA örneği Kapat](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Veri birimleri, her HANA veritabanına düğümde çıkarın. Veri birimleri işletim sistemine bağlı olduğundan, anlık görüntü geri yükleme başarısız olur.
 ![Veri birimleri, her HANA veritabanına düğümde çıkarın](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Belirli bir anlık görüntüye geri yükleme hakkında istemek üzere bir Azure destek isteği açın.

 - Geri yükleme sırasında: Azure Hizmet Yönetimi SAP HANA düzenleme, doğrulama ve doğru depolama anlık görüntü geri yüklendiğinde onay emin olmak için bir konferans katılmayı isteyebilir. 

 - Geri yükleme sonrasında: Azure Hizmet Yönetimi SAP HANA bildirir, size, depolama anlık görüntü geri olduğunda.

5. Geri yükleme işlemi tamamlandıktan sonra tüm veri birimleri yeniden bağlayın.

 ![Tüm veri birimleri yeniden bağlayın](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. SAP HANA Studio aracılığıyla HANA veritabanına bağlandığınızda bunlar otomatik olarak değil oluşturduysanız SAP HANA Studio içinde kurtarma seçeneklerini seçin. Aşağıdaki örnek, son HANA anlık görüntüye geri yüklemeyi gösterir. Bir depolama anlık bir HANA anlık görüntü katıştırır. En son depolama anlık görüntüye geri yüklerseniz, en son HANA anlık görüntü olmalıdır. (Daha eski bir depolama anlık görüntüye geri yüklerseniz, depolama anlık görüntünün alındığı zamana dayalı HANA anlık görüntü bulmanız gerekir.)

 ![SAP HANA Studio içinde kurtarma seçeneklerini seçin](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Seçin **belirli veri yedekleme veya depolama anlık veritabanını kurtarma**.

 ![Kurtarma türünü belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Seçin **belirt yedekleme kataloğu olmadan**.

 ![Yedekleme konumu belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. İçinde **hedef türü** listesinde **anlık görüntü**.

 ![Yedekleme kurtarma penceresine belirtin](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Seçin **son** kurtarma işlemini başlatmak üzere.

 ![Kurtarma işlemini başlatmak için "bitir" seçin](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. HANA veritabanına geri ve depolama anlık görüntüsüne dahil HANA anlık kurtarıldı.

 ![HANA veritabanına geri ve HANA anlık görüntüye kurtarıldı](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>En son durumunu kurtarma

Aşağıdaki işlem, depolama anlık görüntüsüne dahil HANA anlık görüntü geri yükler. Bunun ardından işlem günlüğü yedeklemeleri veritabanını en son durumuna depolama anlık görüntü geri yüklemeden önce geri yükler.

>[!IMPORTANT]
>Devam etmeden önce işlem günlüğü yedeklemeleri tam ve bitişik zincirine sahip olduğunuzdan emin olun. Bu yedeklemeler veritabanının geçerli durumu geri yükleyemezsiniz.

1. 1-6, gelen adımları [en son HANA anlık görüntüye kurtarma](#recovering-to-the-most-recent-hana-snapshot).

2. Seçin **en son durumuna veritabanını kurtarma**.

 !["En son durumuna veritabanını Kurtar" seçin](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. En son HANA günlük yedeklerini konumunu belirtin. Konumun tüm HANA işlem günlüğü yedeklemeleri HANA anlık görüntüsü en son durumuna içermesi gerekir.

 ![En son HANA günlük yedeklerini konumunu belirtin](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Veritabanını kurtarmak bir taban olarak bir yedek seçin. Bizim örneğimizde, ekran görüntüsü HANA anlık görüntü depolama anlık görüntüsüne dahil HANA anlık görüntüsüdür. 

 ![Veritabanını kurtarmak bir taban olarak bir yedek seçin.](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Clear **kullanım değişim yedeklemeleri** HANA anlık görüntü saati ve en son durum farkları yoksa, kutuyu.

 ![Hiçbir farkları varsa "Kullanım değişim yedeklemeleri" onay kutusunu temizleyin](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Özet ekranında seçin **son** geri yükleme yordamı başlatmak için.

 ![Özet ekranında "Bitir"'yi tıklatın](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Zaman içinde başka bir noktaya kurtarma
Arasındaki (depolama anlık görüntüsüne dahil) HANA anlık görüntü HANA anlık görüntü zaman içinde nokta kurtarmadan daha sonraki bir zamanda bir noktaya kurtarmak için aşağıdakileri yapın:

1. Tüm işlem günlüğü yedeklemeleri HANA anlık görüntü kurtarmayı gerçekleştirmek istediğiniz zamana sahip olduğunuzdan emin olun.
2. Altında yordama başlamadan [en son durumuna kurtarma](#recovering-to-the-most-recent-state).
3. Yordamın 2 içinde adımda **kurtarma türünü belirtin** penceresinde, seçin **zamanında aşağıdaki noktası veritabanını kurtarma**ve noktası süresini belirtin. Ardından 3-6 adımlarını tamamlayın.

### <a name="monitoring-the-execution-of-snapshots"></a>Anlık görüntü yürütme izleme

HANA büyük örnekleri depolama anlık görüntüleri kullanma gibi aynı zamanda bu depolama anlık görüntülerin yürütülmesi izlemeniz gerekir. Bir depolama anlık görüntü yürütür komut dosyası çıkışını bir dosyaya yazar ve Perl betikleri aynı konuma kaydeder. Her Depolama anlık görüntü için ayrı bir dosyaya yazılır. Her dosya çıktısını anlık görüntü betiği yürüten çeşitli aşamaları açıkça gösterir:

1. Bir anlık görüntü oluşturmak için gereken birimleri bulun.
2. Bu birimlerden alınan anlık görüntüleri bulun.
3. Belirttiğiniz anlık görüntü sayısını eşleşecek şekilde nihai varolan anlık görüntüleri silin.
4. SAP HANA anlık görüntü oluşturun.
5. Depolama anlık görüntü üzerinde bir birim oluşturun.
6. SAP HANA anlık görüntüyü silin.
7. En son anlık görüntüye yeniden adlandırma **.0**.

En önemli tanımlanan betik cab bu bölümü parçasıdır:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Komut dosyası HANA anlık görüntü oluşturma kayıtları nasıl bu örnekten görebilirsiniz. Genişleme durumda da, bu işlem ana düğümde başlatılır. Ana düğüm SAP HANA anlık görüntü çalışan düğümlerinin her biri zaman uyumlu oluşturulmasını başlatır. Ardından, depolama anlık görüntüsü alınır. Depolama anlık görüntüleri başarılı yürütme sonrasında HANA anlık görüntü silinir. Ana düğümden HANA anlık görüntü silme işlemi başlatılır.


## <a name="disaster-recovery-principles"></a>Olağanüstü durum kurtarma ilkeleri
HANA büyük örnekleriyle farklı Azure bölgelerinde HANA büyük örneği Damgalar arasında bir olağanüstü durum kurtarma işlevi sunar. Örneğin, Azure ABD Batı bölgede HANA büyük örneği birimleri dağıtırsanız, BİZE Doğu bölgede olağanüstü durum kurtarma birimi olarak HANA büyük örneği birimlerini kullanabilirsiniz. DR bölgede başka bir HANA büyük örneği birim için ödeme yapmak gerektirdiği için daha önce belirtildiği gibi olağanüstü durum kurtarma otomatik olarak yapılandırılmamış. Olağanüstü durum kurtarma Kurulumu ölçek büyütme ve bunun yanı sıra ölçek genişletme ayarları için çalışır. 

Şu ana kadar dağıtılan senaryolarda, müşterilerimizin yüklü HANA örneğini kullanan üretim dışı sistemlerinin çalıştırmak için DR bölgede birimi kullanın. HANA büyük örneği birim üretim için kullanılan SKU aynı sku'sunun olması gerekir. Azure üretim bölgesinde sunucusu birimi ve olağanüstü durum kurtarma bölge arasında disk yapılandırması şu şekildedir:

![DR kurulum yapılandırma açısından disk](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Bu genel bakış grafikte gösterildiği gibi daha sonra disk birimleri ikinci bir set sipariş gerekir. Hedef disk birimleri olağanüstü durum kurtarma birimleri üretim örneğinde üretim birimler olarak aynı boyuttadır. Bu disk birimleri olağanüstü durum kurtarma sitesini HANA büyük örneği sunucu biriminde ile ilişkilendirilmiş. Aşağıdaki birimler üretim bölgesi DR siteye çoğaltılır:

- / hana/veri
- / hana/logbackups 
- /hana/shared (/ usr/sap içerir)

SAP HANA işlem günlüğü, bu birimlerdeki geri yükleme yapılır biçimde gerekli olmadığı için /hana/log birim çoğaltılmaz. 

Olağanüstü durum kurtarma işlevinin temel depolama çoğaltma işlevselliği HANA büyük örneği altyapısı tarafından sunulan sunmuştur. Depolama tarafında kullanılan işlev değişiklikleri için depolama birimi durum gibi zaman uyumsuz bir biçimde çoğaltma değişiklikleri sabit akışı yoktur. Bunun yerine, bu birimlerin anlık görüntüleri düzenli olarak oluşturulan olgu güvenen bir mekanizmadır. Zaten çoğaltılmış bir anlık görüntü ve henüz çoğaltılmaz yeni bir anlık görüntü arasındaki aralığı, ardından hedef disk birimleri olağanüstü durum kurtarma siteye aktarılır.  Bu anlık görüntüler birimlerde ve olağanüstü durum kurtarma yük devretme durumunda depolanır, bu birimlerde geri yüklenmesi gerekiyor.  

Veri miktarını anlık görüntüleri arasındaki farkları daha küçük olur önce ilk birimin tam veri aktarımını olmalıdır. Sonuç olarak, DR sitesi birimlerin her biri üretim sitede gerçekleştirilen birim anlık görüntülerini içerir. Bu olgu sonunda üretim sistem geri alma olmadan kayıp verileri kurtarmak için bir önceki durumuna almak için bu DR sistem kullanmanıza olanak sağlar.

HANA sistem çoğaltma yüksek kullanılabilirlik işlevselliği üretim sitenizdeki kullandığınız durumlarda, yalnızca birimler Katman 2 (veya çoğaltma) örneğinin çoğaltılır. Korumak ya da ikincil çoğaltma (Katman 2) sunucu birimi veya bu birimindeki SAP HANA örnek aşağı ele bu yapılandırma, DR sitesi için depolama çoğaltma gecikmesi neden olabilir. 

>[!IMPORTANT]
>Çok katmanlı HANA sistem olarak, çoğaltma ile HANA büyük örneği olağanüstü durum kurtarma işlevini kullanırken olağanüstü durum kurtarma siteye çoğaltma için Katman 2 HANA örneği veya sunucu biriminin bir kapanma engeller.


>[!NOTE]
>HANA büyük örneği depolama çoğaltma işlevselliği yansıtma ve depolama anlık görüntüleri çoğaltılıyor. Bu nedenle, bu belgenin yedekleme bölümünde tanıtılan depolama anlık görüntüleri gerçekleştirmezseniz, olamaz herhangi siteye çoğaltma için olağanüstü durum kurtarma. Depolama anlık görüntü yürütme, depolama siteye çoğaltma için olağanüstü durum kurtarma için bir önkoşuldur.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Olağanüstü durum kurtarma senaryosunda hazırlama
HANA büyük örneklerinde Azure bölgesi üretimde çalışan bir üretim sistemi olduğunu varsayalım. Belge aşağıdaki varsayalım HANA sistem SID'si "PRD." olduğunu HANA büyük olağanüstü durum kurtarma Azure bölgesi çalışan örnekleri üzerinde çalışan bir üretim dışı sistemine sahip varsayıyoruz. Belgeler için biz SID'si "TST." olduğunu varsayın. Bu nedenle yapılandırma şöyle görünür:

![DR Kurulum başlangıcı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Sunucu örneği zaten ek depolama birimi kümesiyle verildi değil, SAP HANA Azure Hizmet Yönetimi birimleri ek kümesi üretim çoğaltma için hedef olarak TST çalıştırdığınız HANA büyük örneği birimine ekleyecek HANA örneğinde. Bu amaç için üretim HANA örneğinizi SID'si sağlamanız gerekir. SAP HANA Azure hizmet yönetimi üzerinde bu birimlerin eki onayladıktan sonra bu birimlerin HANA büyük örneği birimine bağlama gerekir.

![DR Kurulum sonraki adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Sonraki adım, olağanüstü durum kurtarma TST HANA örneği çalıştırdığı Azure bölgesi HANA büyük örneği biriminde ikinci SAP HANA örneği yüklemektir. Yeni yüklenen SAP HANA örneği aynı SID olmalıdır. Oluşturulan kullanıcılar için aynı kullanıcı kimliği ve üretim örneğinin grup kimliği olması gerekir. Yükleme başarılı olursa, gerekir:
- Olağanüstü durum kurtarma Azure bölgesi HANA büyük örneği birimi üzerinde yeni yüklenen SAP HANA örneğini durdurun.
- Bu PRD birimler çıkarın ve SAP HANA Azure Hizmet Yönetimi başvurun. Bunlar depolama çoğaltma hedefi çalışması sırasında erişilebilir olamayacağı için birimlerin birimine bağlı kalın olamaz.  

![Çoğaltma kurmadan önce DR Kurulum adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Azure bölgesi üretimde PRD birimleri ve olağanüstü durum kurtarma Azure bölgesi PRD birimlerin arasındaki çoğaltma ilişkisini kurmak için işlemler ekibinin geçiyor.

>[!IMPORTANT]
>Çoğaltılmış SAP HANA veritabanına olağanüstü durum kurtarma sitedeki tutarlı bir duruma geri yüklemek gerekli olmadığından /hana/log birim çoğaltılan değil.

Ayarlayın veya RPO ve RTO olağanüstü durumda almak için depolama anlık görüntü yedekleme zamanlamasını ayarlamak için sonraki adım olacaktır. Kurtarma noktası hedefi en aza indirmek için aşağıdaki çoğaltma aralıkları HANA büyük örneği hizmetinde ayarlayın:
- Birleştirilmiş bir anlık görüntü tarafından ele alınan birimler (anlık görüntü türünde = **hana**) 15 dakikada bir olağanüstü durum kurtarma sitedeki eşdeğer depolama birimi hedefler için çoğaltılır.
- İşlem günlüğü yedekleme birimi (anlık görüntü türünde = **günlükleri**) üç dakikada olağanüstü durum kurtarma sitedeki eşdeğer depolama birimi hedefler için çoğaltır.

Kurtarma noktası hedefi en aza indirmek için şunu ayarlayın:
- Gerçekleştirmek bir **hana** türü depolama anlık görüntü (bkz: "7. adım: anlık görüntüleri") 30 dakikada 1 saat.
- SAP HANA işlem günlüğü yedeklemeleri her 5 dakikada bir gerçekleştirin.
- Gerçekleştirmek bir **günlükleri** 5-15 dakikada bir anlık görüntü depolama yazın. Bu aralık süresi ile bir RPO yaklaşık 15-25 dakika elde edebilirsiniz olmalıdır.

Bu kurulum, işlem günlüğü yedeklemeleri, depolama anlık görüntüler ve HANA işlem günlüğü çoğaltma sırası birim ve /hana/data yedekleme ve (/ usr/sap içerir) /hana/shared Bu grafikte gösterilen veriler gibi görünmelidir:

 ![Bir işlem günlüğü yedek anlık görüntüsü ve bir zaman ekseni ek yansıtma arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Olağanüstü durum kurtarma durumunda bile daha iyi bir RPO'su elde etmek için HANA işlem günlüğü yedeklemeleri SAP HANA Azure (büyük örnekler) ile ilgili diğer Azure bölgesine kopyalayabilirsiniz. Bu ek RPO azaltma elde etmek için aşağıdaki kaba adımları gerçekleştirin:

1. Geri HANA Hareketi oluşturan gibi bir sıklıkla /hana/logbackups için olabildiğince oturum açın.
2. İşlem günlüğü yedeklemeleri NFS paylaşımına kopyalamak için kullanım rsync Azure sanal makinelerinde barındırılan. Sanal makineleri Azure üretim bölgesinde ve DR bölgelerdeki Azure Sanal Ağları için geçerlidir. Üretim HANA büyük örnekleri Azure'a bağlanılıyor hattına iki Azure sanal ağlar bağlanmanız gerekir. Grafikleri görmek [ağ HANA büyük örneğiyle olağanüstü durum kurtarma değerlendirmeleri](#Network-considerations-for-disaster-recovery-with-HANA-Large-Instances) bölümü. 
3. İşlem günlüğü yedeklemeleri VM'deki bölgede NFS bağlı Canlı depolama verildi.
4. Bir olağanüstü durum yük devretme durumda /hana/logbackups birimde daha yakın zamanda yapılan ile olağanüstü durum kurtarma sitede NFS üzerinde işlem günlüğü yedeklemeleri paylaşmak bulduğunuz işlem günlüğü yedeklemeleri ek niteliğindedir. 
5. Şimdi DR bölgesine üzerinden kaydedilebilir son yedeğini geri yüklemek için bir işlem günlüğü yedeklemesi başlatabilirsiniz.

Çoğaltma ilişkisi Kurulum sahip HANA büyük örneği operations onaylayın ve yürütme depolama anlık görüntüsü yedekleri başlatmak gibi verileri çoğaltılacak başlatır.

![Çoğaltma kurmadan önce DR Kurulum adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Çoğaltma ilerledikçe, anlık görüntüleri PRD birimlerde olağanüstü durum kurtarma Azure bölgeleri geri yüklenmez. Bunlar yalnızca depolanır. Böyle bir durumda birimlerin bağlı olduğundan, olağanüstü durum kurtarma Azure bölgesi sunucu biriminde PRD SAP HANA örnek yüklendikten sonra bu birimlerin uri'siyle neden kaldırdı durumu temsil eder. Bunlar ayrıca henüz geri yüklenmemiş depolama yedeklemeleri temsil eder.

Bir yük devretme durumunda depolama anlık görüntüsü en son yerine daha eski bir depolama anlık görüntü geri yüklemek seçebilirsiniz.

## <a name="disaster-recovery-failover-procedure"></a>Olağanüstü durum kurtarma yük devretme yordamı
İstediğiniz veya DR sitesi için yük devretme için gerekirse, SAP HANA Azure işlemleri ekipteki etkileşimde gerekir. Kaba adımlarda, işlem kadarki şöyle:

1. HANA büyük örnekleri olağanüstü durum kurtarma biriminde HANA üretim dışı örneğini çalıştırdığınız için bu örnek Kapat gerekir. Önceden yüklenmiş bir etkinliği olmayan HANA üretim örneği olduğunu varsayalım.
2. SAP HANA işlemleri çalışır durumda olduğundan emin olun. Bu denetim için aşağıdaki komutu kullanın: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. Çıktı göstermesi gerekir **hdbdaemon** işlem durdurulmuş bir duruma ve diğer hiçbir HANA işlem çalışan veya başlatılmış bir durumda.
3. Hangi anlık görüntü adı veya SAP HANA yedekleme kimliği geri olağanüstü durum kurtarma sitesi sahip istediğinize karar verin. Gerçek bir olağanüstü durum kurtarma durumlarda, bu anlık görüntü genellikle en son anlık görüntüsüdür. Kayıp verileri kurtarmanız gerekiyorsa, önceki bir anlık görüntüsünü seçin.
4. Kişi Azure yüksek öncelikli destek isteği destekler ve bu anlık görüntü (ad ve tarih anlık görüntü) veya DR sitesinde HANA yedekleme kimliği geri yüklemek için isteyin. Geri yükleme yalnızca /hana/data toplu işlemleri varsayılandır. / Hana/logbackups birimleri de sahip olmak istiyorsanız, özellikle, durum gerekir. */Hana/shared birimini geri öneriyoruz değil.* Bunun yerine, global.ini dışı gibi belirli bir dosya seçmelisiniz **.snapshot** dizin ve alt dizinlerinde, / hana/yeniden bağlayın sonra paylaşılan birim için PRD. Operations tarafında, aşağıdaki adımları gerçekleşecek: bir. Olağanüstü durum kurtarma birimlere üretim birimden anlık görüntü çoğaltma durduruldu. Kesinti üretim sitedeki bir DR ihtiyacınız neden ise bu zaten meydana gelmiş.
    b. Depolama adı anlık görüntü veya seçtiğiniz kimliği olağanüstü durum kurtarma birimlerde geri yedekleme anlık görüntüsünü alın.
    c. Geri yüklendikten sonra olağanüstü durum kurtarma bölgede HANA büyük örneği birimlerine takılması olağanüstü durum kurtarma birimler kullanılabilir.
5. Olağanüstü durum kurtarma birimlerini olağanüstü durum kurtarma sitedeki HANA büyük örneği birimine bağlayın. 
6. Şu ana kadar etkinliği olmayan SAP HANA üretim örneği başlatın.
7. Ayrıca RPO süresini azaltmak için işlem günlüğü yedekleme günlükleri kopyalamak seçerseniz, bu işlem günlüğü yedeklemeleri yeni takılı DR/hana/logbackups dizine birleştirmeniz gerekir. Var olan yedekleri üzerine yazmayın. Yalnızca bir depolama anlık görüntüsü en son çoğaltma ile çoğaltılmamış yeni yedeklerini kopyalayın.
8. Olağanüstü durum kurtarma Azure bölgesi /hana/shared/PRD biriminde çoğaltılmamış anlık görüntüleri dışında tek dosyaları da geri yükleyebilirsiniz.

Geri yüklenen depolama anlık görüntü ve kullanılabilir işlem günlüğü yedeklemeleri göre SAP HANA üretim örneği kurtarma adımları dizisini içerir. Adımları şöyle görünür:

1. Yedekleme konumu değiştirme **/hana/logbackups** SAP HANA Studio'yu kullanarak.
   ![DR Kurtarma yedekleme konumunu değiştirme](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA yedek dosya konumlarını tarayan ve geri yüklemek için en son işlem günlüğü yedeklemesi önerir. Tarama aşağıdaki belirir gibi bir ekran kadar birkaç dakika sürebilir: ![DR kurtarma için işlem günlüğü yedeklemeleri listesi](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

3. Varsayılan ayarlardan bazıları ayarlayın:

      - Clear **değişim yedeklemeleri kullanmak**.
      - Seçin **başlatılamadı günlüğü alanı**.

   ![Başlatılamadı günlüğü alanı ayarlama](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. **Son**’u seçin.

   ![DR geri yükleme işlemini tamamladıktan](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Bir ilerleme penceresi, burada gösterildiği gibi görünmelidir. Örnek 3 düğümlü genişleme SAP HANA yapılandırmasının bir olağanüstü durum kurtarma geri yüklenmesiyle ilgilidir göz önünde bulundurun.

![Geri yükleme durumu](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Geri yükleme sırasında askıda görünüyorsa **son** ekranında ve ilerleme durumu ekranı tüm SAP HANA çalışan düğümlerine örnekleri onaylamak için onay çalıştığını göstermez. Gerekirse, SAP HANA örnekleri el ile başlatın.


### <a name="failback-from-dr-to-a-production-site"></a>DR gelen bir üretim site geri dönme
Bir üretim siteye geri DR başarısız olabilir. Yük devretme olağanüstü durum kurtarma siteye kayıp verileri kurtarmak için gerekmez ve sorunları Azure bölgesi üretimde tarafından neden oldu durum bakalım. Bu, SAP üretim yükünüzü biraz olağanüstü durum kurtarma sitede çalışmakta olan anlamına gelir. Üretim sitesini sorunlarını çözümlendi olarak üretim sitede yeniden çalıştırmak istiyor. Veri kaybı yapılamıyor çünkü üretim sitesini adımla geri çeşitli adımları ve Azure işlemleri ekipteki SAP HANA Kapat işbirliği içerir. Bu sorun çözümlendikten sonra üretim sitesine geri Eşitlemeyi başlatmak için işlemler ekibinin tetiklemek için size bağlıdır.

Adımlar dizisi şöyle görünür:

1. SAP HANA Azure işlemleri ekipteki şimdi üretim durumu temsil eden olağanüstü durum kurtarma depolama birimleri üretim depolama birimlerden eşitlemek için tetikleyici alır. Bu durumda üretim sitesini HANA büyük örneği biriminde kapatılır.
2. SAP HANA Azure işlemleri ekipteki çoğaltma izler ve bir müşteri olarak bildiren önce bir nım elde emin olur.
3. Olağanüstü durum kurtarma sitede üretim HANA örneği kullanan uygulamaları kapatın. Ardından bir HANA işlem günlüğü yedeklemesi gerçekleştirin. Ardından olağanüstü durum kurtarma sitedeki HANA büyük örneği birimlerdeki HANA örneği durdurun.
4. Olağanüstü durum kurtarma sitedeki HANA büyük örneği biriminde HANA örneği kapattığınızda, işletim ekibi el ile disk birimleri yeniden eşitler.
5. SAP HANA Azure işlemleri ekipteki HANA büyük örneği birim üretim sitenin yeniden başlatır ve bunu size aktarır. SAP HANA örneği HANA büyük örneği birim başlangıç sırasında bir kapanma durumunda olduğundan emin olun.
6. Olağanüstü durum kurtarma siteye daha önce devretmek zaman olduğu gibi aynı veritabanı geri yükleme adımlarını gerçekleştirin.

### <a name="monitoring-disaster-recovery-replication"></a>Olağanüstü durum kurtarma çoğaltmasını izleme

Komut dosyası yürütme, depolama çoğaltma ilerleme durumunu izleyebilirsiniz `azure_hana_replication_status.pl`. Olağanüstü durum kurtarma konumda çalıştıran bir biriminden bu komut dosyasının çalıştırılması gerekir. Aksi takdirde, beklendiği gibi işlev geçiyor değil. Komut dosyası olsun veya olmasın çoğaltma etkin bağımsız olarak çalışır. Komut dosyası, her bir olağanüstü durum kurtarma konumu kiracınızda HANA büyük örneği birim için çalıştırılabilir. Önyükleme birimi hakkındaki ayrıntıları almak için kullanılamaz.

Komut dosyası gibi arayın:
```
./replication_status.pl <HANA SID>
```

Çıkış, aşağıdaki bölümlere birim aşağı ayrılır:  

- Bağlantı durumu
- Geçerli çoğaltma etkinliği
- Çoğaltılan son anlık görüntü 
- En son anlık görüntü boyutu
- Anlık görüntüler--son tamamlanan anlık görüntü çoğaltma arasındaki geçerli gecikme süresi ve şimdi  

Bağlantı durumu olarak gösterir **etkin** konumları arasındaki bağlantı kapalı veya bir yük devretme olayından şu anda devam eden sürece. Çoğaltma etkinliği herhangi bir veri şu anda çoğaltılmakta olan veya boş olup olmadığını ya da diğer etkinlikleri şu anda bağlantısını gerçekleştiği giderir. Çoğaltılan son anlık görüntü yalnızca olarak görünmesi gereken `snapmirror…`. Son anlık görüntünün boyutu sonra görüntülenir. Son olarak, gecikme süresi gösterilir. Çoğaltma tamamlandığında için zamanlanmış çoğaltma zamandan süresini gecikme süresini temsil eder. Çoğaltma başlatıldı olsa bile bir gecikme süresi veri çoğaltması, özellikle ilk çoğaltma bir saatten daha büyük olabilir. Gecikme süresi devam eden çoğaltma tamamlanana kadar artmaya devam edecek.

Bir çıkış şöyle:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












