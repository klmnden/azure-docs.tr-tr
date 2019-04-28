---
title: SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de | Microsoft Docs
description: SAP HANA için yedekleme Kılavuzu SAP HANA için iki ana yedekleme olanaklar, Azure sanal makinelerinde sağlar.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: 89896fab7b1c359007ed23d4f9d9771e366ca68a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60937092"
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Azure Sanal Makineler’de SAP HANA için yedekleme kılavuzu

## <a name="getting-started"></a>Başlarken

Azure sanal Makineler'de çalışan SAP HANA için yedekleme Kılavuzu yalnızca Azure özel konular açıklanmaktadır. Genel SAP HANA yedekleme ilgili öğeleri, SAP HANA belgeleri denetleyin (bkz _SAP HANA backup belgeleri_ bu makalenin ilerleyen bölümlerinde).

Bu makalenin odak noktası, iki ana yedekleme olasılık SAP HANA için Azure sanal makinelerinde açıktır:

- Azure Linux sanal makine, dosya sistemi HANA yedeklemesi (bkz [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md))
- Tabanlı yedeklemeyi HANA depolama anlık görüntüleri kullanılarak el ile Azure depolama blobu anlık görüntü özelliği ya da Azure Backup hizmeti (bkz [tabanlı depolama anlık görüntüleri üzerinde SAP HANA yedeklemeyi](sap-hana-backup-storage-snapshots.md))

SAP HANA bir yedekleme doğrudan SAP HANA ile tümleştirmek üçüncü taraf yedekleme araçları sağlayan bir API sunar. (Bu, bu kılavuzun kapsamı içinde değil.) Kullanılabilir sağ artık bu API'ye bağlı Azure Backup hizmeti ile SAP HANA'ın doğrudan hiçbir tümleştirme yoktur.

SAP HANA gibi Azure M serisi, çeşitli Azure VM türleri resmi olarak desteklenir. Tam listesi SAP HANA sertifikalı Azure Vm'leri için kullanıma [sertifikalı Iaas platformları Bul](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Bu makalede, kullanılabilir Azure üzerinde SAP HANA için yeni sunumları olarak güncelleştirilecektir.

Ayrıca bir SAP HANA karma çözüm kullanılabilir var. azure'da SAP HANA fiziksel sunucularda sanallaştırılmamış çalıştığı Ancak, bu SAP HANA Azure yedekleme Kılavuzu SAP HANA çalıştığı Azure VM'deki değil üzerinde çalışan SAP HANA saf bir Azure ortamı kapsayan &quot;büyük örnekler.&quot; Bkz: [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](hana-overview-architecture.md) Bu yedekleme çözümü hakkında daha fazla bilgi için &quot;büyük örnekler&quot; depolama anlık görüntülerine dayalı.

Azure'da desteklenen SAP ürünleri hakkında genel bilgiler bulunabilir [SAP notu 1928533](https://launchpad.support.sap.com/#/notes/1928533).

Aşağıdaki üç şekilde şu anda yerel Azure özelliklerini kullanarak SAP HANA yedekleme seçeneklerine genel bakış sağlar ve ayrıca üç olası gelecekteki yedekleme senaryoları göster. İlgili makaleler [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) ve [tabanlı depolama anlık görüntüleri üzerinde SAP HANA yedeklemeyi](sap-hana-backup-storage-snapshots.md) Bu seçenekler için SAP boyut ve performans konuları dahil olmak üzere daha ayrıntılı açıklanmaktadır Çoklu terabayt boyutunda olan HANA yedeklemeler.

![Bu şekilde geçerli VM durumu kaydetmek için iki olasılıklar gösterilmektedir.](media/sap-hana-backup-guide/image001.png)

Bu şekilde, geçerli VM durumu, Azure Backup hizmeti veya el ile anlık görüntü VM disklerinin aracılığıyla kaydetme olasılığını gösterilmektedir. Bu yaklaşım, bir eklenmemişse&#39;SAP HANA yedeklemelerini yönetme zorunda. Disk anlık görüntü senaryosunun dosya sistemi tutarlılığı ve uygulamayla tutarlı bir disk durumu zorluktur. Tutarlılık konu bölümünde anlatılan _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_ bu makalenin ilerleyen bölümlerinde. Özellikler ve kısıtlamalar SAP HANA yedeklemeler için ilgili Azure Backup hizmeti, ayrıca bu makalenin sonraki bölümlerinde ele alınmıştır.

![Bu şekilde sanal makine içinde yedekleme dosya bir SAP HANA alma seçenekleri gösterilmektedir.](media/sap-hana-backup-guide/image002.png)

Bu şekilde, VM içindeki bir SAP HANA dosya yedekleme ve ardından, HANA yedekleme dosyaları farklı araçlarla başka bir yerde saklamak için seçenekler gösterilmektedir. HANA yedekleme anlık görüntü tabanlı bir yedekleme çözümü kıyasla daha çok zaman gerektirir, ancak bunu bütünlüğü ve tutarlılığı ilgili avantajları vardır. Bu makalenin ilerleyen bölümlerinde daha ayrıntılı bilgi sağlanır.

![Bu şekilde olası bir gelecek SAP HANA yedekleme senaryo gösterilmektedir.](media/sap-hana-backup-guide/image003.png)

Bu şekilde, olası gelecek SAP HANA yedekleme senaryo gösterilmektedir. SAP HANA ikincil bir çoğaltma yedeklemelerin alma izin veriliyorsa, yedekleme stratejileri için ek seçenekler eklersiniz. Şu anda SAP HANA Wiki postasına göre mümkün değildir:

_&quot;İkincil tarafa yedeklemelerinin alınacağı mümkündür?_

_Hayır, şu anda yalnızca verilerin ve günlük yedeklemelerine birincil tarafında. Otomatik günlük yedeği etkinse sonra ikincil tarafa devralma günlüğü yedeklerini otomatik olarak var. yazılır.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP HANA yedeklemesi kaynakları

### <a name="sap-hana-backup-documentation"></a>SAP HANA backup belgeleri

- [SAP HANA yönetimine giriş](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Yedekleme ve kurtarma stratejisini planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [HANA ABAP DBACOCKPIT kullanarak yedeklemeyi zamanlayın](https://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Zamanlama veri yedekleri (SAP HANA Kokpit)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- SAP HANA hakkında SSS yedekleme içinde [SAP notu 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- SAP HANA veritabanı ve depolama anlık görüntüleri hakkında SSS [SAP notu 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Yedekleme ve kurtarma için uygun ağ dosya sistemleri [SAP notu 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Neden HANA yedeklemesi SAP?

Azure depolama, kullanılabilirlik ve güvenilirlik hazır sağlar (bkz [Microsoft Azure Storage'a giriş](../../../storage/common/storage-introduction.md) Azure depolama hakkında daha fazla bilgi için).

İçin en düşük &quot;yedekleme&quot; Azure üzerinde SAP HANA sunucusu VM'sine bağlı Azure VHD'leri SAP HANA veri ve günlük dosyalarını SLA'ları dayanan sağlamaktır. Bu yaklaşım, VM hataları, ancak SAP HANA veri ve günlük dosyaları ya da yanlışlıkla veri veya dosya silme gibi mantıksal hataları değil potansiyel hasarı kapsar. Yedeklemeler de uyumluluğu veya yasal nedenlerle gereklidir. Kısacası, var. her zaman SAP HANA yedeklemeler için bir gereksinimi

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>SAP HANA yedeklemesi doğruluğunu doğrulama
Depolama anlık görüntüleri kullanarak, bir test geri yükleme farklı bir sistemde çalışan önerilir. Bu yaklaşım, bir yedekleme, yedekleme için doğru ve iç işlemler olduğundan emin olun ve iş beklenen şekilde geri yüklemek için bir yol sağlar. Bu bir önemli olmakla birlikte hurdle şirket içinde gerekli kaynakları geçici olarak bu amaçla sağlayarak bulutta gerçekleştirmek çok daha kolaydır.

Unutmayın, bir basit geri yükleme yapmaya devam ve HANA yukarı olup olmadığı denetlenirken ve çalıştıran yeterli değildir. İdeal olarak, bir geri yüklenen veritabanı iyi olduğundan emin olmak için tablo tutarlılık denetimini çalıştırmanız gerekir. SAP HANA sunar tutarlılık denetimleri açıklanan çeşitli türlerde [SAP notu 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Tablo tutarlılık denetimi hakkında bilgi de bulunabilir SAP Web sitesinde [tablo ve Katalog tutarlılık denetimleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Standart dosya yedeklemeler için bir test geri gerekli değildir. Hangi yedekleme geri yükleme için kullanılabilir denetlemek için yardımcı olan iki SAP HANA araçlar vardır: hdbbackupdiag ve hdbbackupcheck. Bkz: [el ile denetimi olup olmadığını bir kurtarma mümkün olduğu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) Bu araçlar hakkında daha fazla bilgi için.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Artıları ve eksileri HANA yedekleme depolama anlık görüntüleri karşılaştırması

SAP eklenmemişse&#39;t ya da HANA yedekleme depolama anlık görüntüleri ve öncelik verin. Böylece durum ve kullanılabilir depolama teknolojisine bağlı olarak kullanmak istediğiniz birini belirleyebilir, Artıları ve eksileri, listeler (bkz [planlama Backup ve kurtarma stratejisini](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

Azure'da, Azure blob özelliği eklenmemişse anlık görüntü olgusu dikkat&#39;t garantisi dosya sistemi tutarlılığı (bkz [PowerShell kullanarak blob anlık görüntüleri](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). Sonraki bölümde _depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı_, bu özellikle ilgili bazı hususlar anlatılmaktadır.

Ayrıca, sık bu makalede açıklandığı gibi blob anlık görüntüleri ile çalışırken fatura etkilerini anlamak varsa: [Anlama nasıl anlık görüntüleri tahakkuk ücretleri](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)— bunu birincile&#39;t kullanarak Azure sanal diskler olarak gibi belirgin.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>Depolama anlık görüntülerini çekerken SAP HANA veri tutarlılığı

Dosya sistemi ve uygulama tutarlılığı bir karmaşık depolama anlık görüntülerini çekerken sorunudur. Sorunları önlemek için en kolay yolu, SAP HANA kapatın, hatta belki de tüm sanal makine olacaktır. Bir kapanma bir tanıtım veya prototipi veya geliştirme sistemi ile ortak olabilir ancak bir üretim sistemi için bir seçenek değil.

Azure blob özelliği eklenmemişse anlık görüntü akılda tutulması gereken varsa Azure'da&#39;t garantisi dosya sistemi tutarlılığı. Var olduğu sürece yalnızca tek bir sanal disk dahil ancak kullanarak SAP HANA özelliği, anlık görüntü düzgün çalışır. Ancak bile tek bir diske sahip, Kontrol edilecek ek öğeler gerekir. [SAP notu 2039883](https://launchpad.support.sap.com/#/notes/2039883) depolama anlık görüntüleri ile SAP HANA yedeklemeler hakkında önemli bilgiler vardır. XFS dosya sistemi ile bunu çalıştırmak gerekli olduğunu, örneğin, bahsetmeleri **xfs\_dondurma** tutarlılığı güvence altına almak için bir depolama anlık görüntüleri başlatmadan önce (bkz [xfs\_freeze(8) - Linux man sayfası ](https://linux.die.net/man/8/xfs_freeze) hakkında ayrıntılı bilgi için **xfs\_dondurma**).

Tutarlılık konusunda tek dosya sistemi birden çok disk/birim nereden yayılan bir durumda bile daha zor hale gelir. Örneğin, mdadm veya LVM kullanarak ve bölümlenerek tarafından. Durumları bahsedilen SAP Note:

_&quot;Ancak SAP HANA veri birimi başına bir depolama anlık görüntüsü oluşturulurken g/ç tutarlılığı güvence altına almak için depolama sistemi olan göz önünde bulundurun, atomik işlem olarak anlık görüntüsünün alınması, bir SAP HANA hizmete özgü veri hacminin yani olmalıdır.&quot;_

Aşağıdaki adımlar, dört Azure sanal diskler kapsayan bir XFS dosya sistemi yoktur varsayıldığında, HANA veri alanını temsil eden tutarlı bir anlık görüntü sağlar:

- HANA anlık görüntü hazırlama
- Dosya sistemi dondurma (örneğin, **xfs\_dondurma**)
- Azure'da tüm gerekli blob anlık görüntüleri oluşturma
- Dosya Sistemi Çöz
- HANA anlık görüntü onaylayın

Hangi dosya sistemi ne olursa olsun güvenli tarafında olması için tüm durumlarda yukarıdaki yordamı kullanmak için önerilir. Ya da tek bir disk veya mdadm veya birden çok disk arasında LVM aracılığıyla bölümleme türüyle ise.

HANA anlık görüntü onaylamak önemlidir. Şu nedenle &quot;kopyalama yazarken,&quot; SAP HANA değil ancak bu ek disk alanı gerektirebilir anlık görüntü hazırlama modu. Bunu&#39;s SAP HANA anlık görüntü onaylanana kadar yeni yedeklemeleri başlatmak de mümkün değildir.

Azure Backup hizmeti, Azure VM uzantıları dosya sistemi tutarlılığı halletmeniz için kullanır. Bu sanal makine uzantıları, tek başına kullanım için kullanılamaz. Bir SAP HANA tutarlılık yönetmek yine de vardır. İlgili makaleye göz atın [SAP HANA dosya düzeyi Azure yedekleme](sap-hana-backup-file-level.md) daha fazla bilgi için.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA yedekleme stratejisi planlama

SAP HANA makaleyi [planlama Backup ve kurtarma stratejisini](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) yedeklemeler için bir temel plan durumları:

- (Günlük) anlık görüntü depolama
- Dosya veya bacint biçimi (haftada bir) kullanarak tam yedekleme
- Otomatik günlük yedeklemeler

İsteğe bağlı olarak, bir depolama anlık görüntüleri tamamen gidebilir; HANA delta yedeklemeler, artımlı ya da fark yedekleri gibi tarafından değiştirilebilir (bkz [değişim yedeklemeleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

HANA Yönetim Kılavuzu, bir örnek listesi sağlar. Bu, bir SAP HANA belirli bir noktaya yedekleme aşağıdaki dizi kullanılarak geri olmasını önerir:

1. Tam yedekleme
2. Değişiklik yedeği
3. Artımlı Yedekleme 1
4. Artımlı yedekleme 2
5. Günlük yedekleme

Ne zaman ve ne sıklıkta belirli bir yedekleme türü olması gerektiğini bir tam zamanlama ile ilgili genel bir kılavuz vermek mümkün değildir — çok müşteriye özgü ve kaç tane veri sistemde değişiklikler üzerinde bağlıdır. Genel rehberlik sağlaması görülebilir, SAP taraftan bir temel öneri yapma bir tam HANA için haftada bir yedeklemedir.
İlgili günlüğü yedeklemeleri, SAP HANA belgelerine bakın [günlük yedeklerinin](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

Ayrıca SAP önerir sonsuz büyüyen korumak için yedekleme Kataloğu'nun bazı temizlik yapmak (bkz [yedekleme kataloğunu ve yedekleme depolama alanı için Housekeeping](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>SAP HANA yapılandırma dosyaları

SSS içinde belirtildiği gibi [SAP notu 1642148](https://launchpad.support.sap.com/#/notes/1642148), SAP HANA yapılandırma dosyaları, standart bir HANA yedeklemenin bir parçası değildir. Bunlar bir sistemin geri yüklemek için gerekli değildir. HANA yapılandırmasını el ile geri yüklemeden sonra değiştirilemedi. Bir geri yükleme işlemi sırasında aynı özel yapılandırma almak istersiniz durumunda, HANA yapılandırma dosyaları ayrı olarak yedeklemek gereklidir.

Standart HANA yedeklemeler için adanmış bir HANA yedekleme dosya sistemi kullanacaksanız, bir Ayrıca aynı yedekleme dosya sistemi için yapılandırma dosyalarını kopyalayın ve ardından her şeyi birlikte seyrek gibi son depolama hedefi kopyalayın blob depolama.

### <a name="sap-hana-cockpit"></a>SAP HANA Kokpit

SAP HANA Kokpit izleme ve SAP HANA bir tarayıcı aracılığıyla yönetme olanağı sunar. Ayrıca SAP HANA yedeklemeleri işlenmesini sağlar ve bu nedenle SAP HANA Studio ve ABAP DBACOCKPIT bir alternatifi olarak kullanılabilir (bkz [SAP HANA Kokpit](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) daha fazla bilgi için).

![Bu şekilde SAP HANA Kokpit veritabanı yönetim ekranında gösterilmektedir.](media/sap-hana-backup-guide/image004.png)

Bu şekilde, sol taraftaki SAP HANA Kokpit veritabanı yönetim ekranında ve yedekleme kutucuk gösterilmektedir. Yedekleme kutucuğu görmek için oturum açma hesabı uygun kullanıcı izinlerini gerektirir.

![Yedeklemeler devam ederken SAP HANA Kokpit izlenebilir](media/sap-hana-backup-guide/image005.png)

Yedeklemeler devam eden olduklarını ve işlem tamamlandığında, tüm yedekleme ayrıntıları kullanılabilir olsa da SAP HANA Kokpit izlenebilir.

![Gnome masaüstü ile Azure SLES 12 VM'de Firefox kullanma örneği](media/sap-hana-backup-guide/image006.png)

Önceki ekran görüntüleri bir Azure Windows sanal makinesinden yapıldı. Bu, Firefox Gnome masaüstü ile Azure SLES 12 VM'de kullanarak bir örnektir. Bu, yedekleme zamanlamaları SAP HANA SAP HANA Kokpit tanımlama seçeneği gösterir. Bir de görebileceğiniz gibi yedekleme dosyaları için tarih/saat öneki olarak önerir. SAP HANA Studio varsayılan önekidir &quot;tam\_veri\_yedekleme&quot; bir tam dosya yedeğini yaparken. Benzersiz bir önek kullanılması önerilir.

### <a name="sap-hana-backup-encryption"></a>SAP HANA yedekleme şifrelemesi

SAP HANA veri ve günlük şifrelenmesini sağlar. SAP HANA veri ve günlük şifrelenmemişse yedekleri de şifrelenmez. Buna SAP HANA yedeklemeleri şifrelemek için üçüncü taraf çözümü biçimi kullanmak için en fazla müşteri var. Bkz: [veri ve günlük birim şifrelemesi](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) SAP HANA şifrelemesi hakkında daha fazla bilgi için.

Microsoft Azure'da Iaas VM şifreleme özelliği şifrelemek için bir müşteri kullanabilirsiniz. Örneğin, bir sanal Makineye bağlı SAP HANA yedeklemeleri depolamak ve ardından bu diskleri kopyalarını için kullanılan adanmış veri diskleri kullanabilirsiniz.

Azure Backup hizmeti, şifrelenmiş sanal makineleri/diskleri işleyebilir (bkz [sanal makinelerini Azure Backup ile yedekleme ve geri yükleme şifrelenmiş](../../../backup/backup-azure-vms-encryption.md)).

SAP HANA VM ve şifreleme olmadan, disklerini korumak ve bir depolama hesabında şifreleme etkinleştirildi SAP HANA yedek dosyalarını depolamak için başka bir seçenek olabilir (bakın [bekleyen veriler için Azure depolama hizmeti şifrelemesi](../../../storage/common/storage-service-encryption.md)).

## <a name="test-setup"></a>Test Kurulumu

### <a name="test-virtual-machine-on-azure"></a>Azure üzerinde test sanal makinesi

Yaptığımız testleri gerçekleştirmek için Azure GS5 VM'deki bir SAP HANA yüklemesi aşağıdaki yedekleme/geri yükleme testleri için kullanıldı. İlkeler, M serisi VM'ler ile aynıdır.

![Bu şekilde HANA test sanal makinesi için Azure portalına genel bakış bölümü gösterilmektedir.](media/sap-hana-backup-guide/image007.png)

Bu şekilde, HANA test sanal makinesi için Azure portalına genel bakış bölümü gösterilmektedir.

### <a name="test-backup-size"></a>Test yedekleme boyutu

![Bu şekil, HANA Studio yedekleme konsolundan alındığı ve 229 GB HANA dizin sunucusu yedekleme dosyası boyutunu gösterir](media/sap-hana-backup-guide/image008.png)

İşlevsiz bir tablo, gerçekçi performans verilerini türetmek için üzerinde 200 GB toplam veri yedekleme boyutunu alma verilerle doldurulmuş. Şekil, HANA Studio yedekleme konsolundan alındığı ve HANA dizini sunucusunun 229 GB yedekleme dosyası boyutunu gösterir. Varsayılan yedekleme ön eki "COMPLETE_DATA_BACKUP" SAP HANA Studio testleri için kullanıldı. Gerçek üretim sisteminde daha kullanışlı bir önek tanımlanmalıdır. SAP HANA Kokpit tarih/saat önerir.

### <a name="test-tool-to-copy-files-directly-to-azure-storage"></a>Doğrudan Azure Depolama'ya dosya kopyalamak için aracı test

Doğrudan Azure blob depolama veya Azure dosya paylaşımları için yedekleme dosyalarının SAP HANA aktarmak için çünkü her iki hedeflerini destekler ve Otomasyon betikleri nedeniyle, komut satırı arabirimi ile kolayca tümleştirilebilir blobxfer aracı kullanıldı. Blobxfer Aracı'nı kullanılabilir [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Yedekleme boyutu tahmin test

SAP HANA yedekleme boyutunu tahmin etmek önemlidir. Bu tahmini bir dosya kopyalama sırasında paralellik nedeniyle yedekleme dosyalarının sayısı için en fazla yedekleme dosyası boyutu tanımlayarak performansını artırmaya yardımcı olur. (Bu ayrıntılar bu makalenin sonraki bölümlerinde açıklanmıştır.) Bir de tam yedekleme veya bir delta yedeklemeyi (artımlı ya da fark) yapmak karar vermeniz gerekir.

Neyse ki, yedekleme dosyalarının boyutuna tahminleri basit bir SQL deyimi yok: **seçin \* m\_yedekleme\_BOYUTU\_TAHMİNLERİ** (bkz [tahmin Dosya sistemindeki bir veri yedekleme için ihtiyaç duyulan alanı](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![Bu SQL deyimi çıktısını diskteki tam yedekleme neredeyse gerçek boyutu eşleşir](media/sap-hana-backup-guide/image009.png)

Test sistemi için tam yedekleme diskte neredeyse gerçek boyutu bu SQL deyimi çıktısını eşleşir.

### <a name="test-hana-backup-file-size"></a>Test HANA yedekleme dosya boyutu

![HANA Studio yedekleme konsolu bir HANA için yedekleme dosyalarının en büyük dosya boyutunu sınırlamak sağlar.](media/sap-hana-backup-guide/image010.png)

HANA Studio yedekleme konsolu bir HANA için yedekleme dosyalarının en büyük dosya boyutunu sınırlamak sağlar. Örnek ortamda bu özellik, birden çok daha küçük yedekleme dosyalarını bir 230 GB yedekleme dosyası yerine almak mümkün kılar. Daha küçük dosya boyutu performansı üzerinde önemli bir etkisi vardır (ilgili makaleye göz atın [SAP HANA dosya düzeyi Azure yedekleme](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Özet

Test sonuçlarını Artıları ve eksileri çözümleri, Azure sanal makineler üzerinde çalışan bir SAP HANA veritabanını yedeklemek için aşağıdaki tablolarda gösterilmiştir Temel.

**SAP HANA ' için dosya sistemi yedekleyin ve yedekleme dosyalarının son yedekleme hedefine daha sonra kopyalayın.**

|Çözüm                                           |Uzmanları                                 |Simgeler                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|VM diskleri üzerinde HANA yedekleri koru                      |Hiçbir ek yönetim çabalarınızı     |Yerel sanal disk alanını eats           |
|Yedekleme dosyaları blob depolama alanına kopyalanacak Blobxfer aracı |Birden çok dosya kopyalamak için paralellik seçim seyrek erişimli blob depolama kullanma | Ek aracı Bakım ve özel betik oluşturma | 
|Powershell veya CLI aracılığıyla BLOB kopyalama                    |Gerekirse, hiçbir ek araç, Azure Powershell veya CLI gerçekleştirilebilir |el ile işlem, betik oluşturma ve geri yükleme için kopyalanan BLOB yönetim ilgileniriz müşterinin|
|NFS paylaşımına kopyalayın                                  |HANA sunucudaki etkisi olmayan diğer VM yedekleme dosyalarının son işlemi|Yavaş kopyalama işlemi|
|Azure dosya Hizmeti'ne Blobxfer Kopyala                |Yerel sanal disk alanı yemek değil|Hiçbir doğrudan desteği şu anda 5 TB dosya paylaşımının yedekleme, boyutu kısıtlaması HANA tarafından yazma|
|Azure Yedekleme aracısı                                 | Tercih edilen bir çözüm olacaktır         | Linux üzerinde şu an kullanılamıyor    |



**Depolama anlık görüntülerine dayalı SAP HANA yedekleme**

|Çözüm                                           |Uzmanları                                 |Simgeler                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure yedekleme hizmeti                               | VM yedekleme blob anlık görüntülerine dayalı sağlar | Dosya düzeyinde geri yükleme kullanılmadığında, yeni bir SAP HANA lisans anahtarı gerekli olduğu anlamına gelir geri yükleme işlemi için yeni bir sanal makine oluşturulmasını gerektirir|
|El ile blob anlık görüntüleri                              | Esneklik oluşturma ve benzersiz VM kimliği değiştirmeden belirli VM disklerini geri yükleme|Müşteri tarafından yapılması gereken tüm el ile çalışma|

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneğini açıklar.
* [SAP HANA yedeklemesi depolama anlık görüntülerine dayalı](sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği açıklar.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP hana (büyük örnekler) azure'da planlama oluşturma hakkında bilgi almak için bkz: [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
