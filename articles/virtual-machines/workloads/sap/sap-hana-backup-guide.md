---
title: SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde | Microsoft Docs
description: SAP HANA için yedekleme kılavuz SAP HANA için iki ana yedekleme olasılık, Azure sanal makinelerde sağlar.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 9e5b124643b753f404ba6012d3df998f567be59a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Azure Sanal Makineler’de SAP HANA için yedekleme kılavuzu

## <a name="getting-started"></a>Başlarken

SAP HANA Azure sanal makinelerde çalışan yedekleme Kılavuzu yalnızca Azure özel konular anlatmaktadır. Genel SAP HANA yedekleme ilgili öğeler için SAP HANA belgelerine bakın (bkz _SAP HANA yedekleme belgelerine_ bu makalenin ilerisinde yer).

İki ana yedekleme olasılıklarını SAP HANA Azure sanal makinelerinde bu makaleyi odağını açıktır:

- Dosya sistemine Azure Linux sanal makine içinde HANA yedekleme (bkz [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md))
- HANA yedekleme tabanlı depolama anlık görüntüleri kullanarak Azure storage blobu anlık görüntü özelliği el ile veya Azure yedekleme hizmeti üzerinde (bkz [tabanlı depolama anlık görüntü SAP HANA yedeklemeyi](sap-hana-backup-storage-snapshots.md))

SAP HANA yedekleme doğrudan SAP HANA ile tümleştirmek üçüncü taraf yedekleme araçları sağlayan API sunar. (, Bu kılavuz kapsamında değildir.) SAP HANA kullanılabilir sağ bu API artık temel Azure Backup hizmeti ile doğrudan hiçbir tümleştirilmesi yoktur.

SAP HANA resmi olarak desteklenen GS5 Azure VM türüne sahip başka bir kısıtlama OLAP iş yükleri için tek örnek olarak (bkz [sertifikalı Iaas platformları Bul](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html) SAP Web sitesinde). Bu makalede, SAP HANA kullanılabilir hale azure'da için yeni sunumu olarak güncelleştirilir.

Ayrıca SAP HANA karma çözümünü mevcut değil SAP HANA fiziksel sunucularda sanallaştırılmamış çalıştığı azure'da. Ancak, bu SAP HANA Azure yedekleme Kılavuzu SAP HANA çalıştığı bir Azure VM'de, çalışan değil SAP HANA saf bir Azure ortamı kapsayan &quot;büyük örnekleri.&quot; Bkz: [SAP HANA (büyük örnekler) genel bakış ve Azure ile ilgili mimari](hana-overview-architecture.md) Bu yedekleme çözümü hakkında daha fazla bilgi için &quot;büyük örnekleri&quot; depolama anlık görüntü tabanlı.

Azure üzerinde desteklenen SAP ürünler hakkında genel bilgiler bulunabilir [SAP Not 1928533](https://launchpad.support.sap.com/#/notes/1928533).

Aşağıdaki üç rakamı şu anda yerel Azure özelliklerini kullanarak SAP HANA yedekleme seçeneklerine genel bakış sağlar ve ayrıca üç olası gelecekteki yedekleme senaryoları gösterir. İlgili makaleler [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) ve [tabanlı depolama anlık görüntü SAP HANA yedeklemeyi](sap-hana-backup-storage-snapshots.md) çoklu terabayt boyutu olan SAP HANA yedeklemeler için boyutu ve performans konuları dahil olmak üzere daha ayrıntılı Bu seçenekler açıklanmaktadır.

![Bu şekilde, geçerli VM durumunu kaydetmek için iki olasılık gösterilmektedir.](media/sap-hana-backup-guide/image001.png)

Bu şekilde geçerli VM durumunu da VM diskleri el ile anlık görüntüsünü veya Azure Backup hizmeti aracılığıyla kaydetme olasılığını gösterilmektedir. Bu yaklaşım, bir içermiyor ile&#39;SAP HANA yedeklemeleri yönetmeniz gerekir. Disk anlık görüntü senaryonun dosya sistemi tutarlılığı ve bir uygulamayla tutarlı disk durumu iştir. Tutarlılık konu bölümünde anlatılan _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_ bu makalenin ilerisinde yer. Özellikleri ve Azure Backup hizmeti SAP HANA yedeklemeler için ilgili kısıtlamaları da bu makalenin sonraki bölümlerinde ele alınmıştır.

![Bu şekilde, VM içinde yedek dosya SAP HANA ayırdığınız için seçenekleri gösterilmektedir.](media/sap-hana-backup-guide/image002.png)

Bu şekilde VM içindeki bir SAP HANA dosya yedekleme yapmayı ve ardından onu HANA yedekleme dosyaları farklı araçlarla başka bir yerde saklamak için seçenekler gösterilmektedir. Anlık görüntü tabanlı bir yedekleme çözümü daha uzun bir HANA Yedekleme yapmayı gerektirir, ancak bütünlük ve tutarlılık ilgili avantajları vardır. Bu makalenin sonraki bölümlerinde daha ayrıntılı bilgi sağlanır.

![Bu şekilde olası bir gelecekteki SAP HANA yedekleme senaryo gösterilmektedir](media/sap-hana-backup-guide/image003.png)

Bu şekilde olası bir gelecekteki SAP HANA yedekleme senaryosu gösterilmiştir. SAP HANA ikincil bir çoğaltma yedek almak izin veriliyorsa, yedekleme stratejilerini için ek seçenekler eklersiniz. Şu anda SAP HANA Wiki postasına göre olası değil:

_&quot;İkincil tarafında yedeklemeleri gerçekleştirmeye mümkün mü?_

_Hayır, şu anda yalnızca verileri alabilir ve günlük yedeklemelerine birincil tarafındaki. Otomatik günlük yedekleme etkinse ikincil dışarıdan devralma sonra günlük yedeklerini otomatik olarak var. yazılır.&quot;_

## <a name="sap-resources-for-hana-backup"></a>SAP HANA yedekleme için kaynaklar

### <a name="sap-hana-backup-documentation"></a>SAP HANA yedekleme belgeleri

- [SAP HANA yönetimine giriş](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Yedekleme ve kurtarma stratejisini planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [HANA ABAP DBACOCKPIT kullanarak yedeklemeyi zamanlama](http://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Veri yedeklemeleri zamanlamanın (SAP HANA Pilot)](http://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- SAP HANA hakkında SSS yedekleme [SAP Not 1642148](https://launchpad.support.sap.com/#/notes/1642148)
- SAP HANA veritabanına ve depolama anlık görüntüleri hakkında SSS [SAP Not 2039883](https://launchpad.support.sap.com/#/notes/2039883)
- Yedekleme ve kurtarma için uygun ağ dosya sistemleri [SAP Not 1820529](https://launchpad.support.sap.com/#/notes/1820529)

### <a name="why-sap-hana-backup"></a>Neden HANA yedekleme SAP?

Azure storage, kullanılabilirliği ve güvenilirliği kutunun dışında sunar (bkz [Microsoft Azure Storage'a giriş](../../../storage/common/storage-introduction.md) Azure storage hakkında daha fazla bilgi için).

İçin en az &quot;yedekleme&quot; Azure Azure VM SAP HANA sunucuya bağlı VHD'ler SAP HANA veri ve günlük dosyalarını tutma SLA'ları Bel sağlamaktır. Bu yaklaşım, VM hataları ancak SAP HANA veri ve günlük dosyalarını ya da veri veya dosyaları yanlışlıkla silme gibi mantıksal hataları için değil potansiyel hasarı kapsar. Yedeklemeler de uyumluluğu veya yasal nedenlerle için gereklidir. Kısacası, gerçek her zaman bir gereksinimi SAP HANA yedeklemeler için.

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>Nasıl SAP HANA yedekleme doğruluğundan emin olun.
Depolama anlık görüntüleri kullanarak, bir test geri yükleme farklı bir sistem üzerinde çalışan önerilir. Bu yaklaşım, bir yedekleme yedeklemesi için doğru ve iç işlemler olduğundan emin olun ve beklendiği gibi iş geri yüklemek için bir yol sağlar. Bu önemli bir durumdayken hurdle şirket içi, gerekli kaynakları geçici olarak bu amaçla sağlayarak bulutta gerçekleştirmek çok daha kolaydır.

Göz önünde, basit bir geri yükleme yapmaya devam ve HANA çalışır durumda olup olmadığı denetleniyor ve çalıştıran yeterli değil. İdeal olarak, bir geri yüklenen veritabanı yolunda olduğundan emin olmak için bir tablo tutarlılık denetimi çalıştırmanız gerekir. SAP HANA sunar tutarlılık denetimleri açıklanan çeşitli türlerde [SAP Not 1977584](https://launchpad.support.sap.com/#/notes/1977584).

Tablo tutarlılık denetimi hakkında bilgi de bulunabilir SAP Web sitesinde bulunan [tablo ve Katalog tutarlılık denetimleri](http://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b).

Standart dosya yedeklemeler için bir test geri yükleme gerekli değildir. Hangi yedekleme geri yükleme için kullanılabilir denetlemek yardımcı iki SAP HANA araç vardır: hdbbackupdiag ve hdbbackupcheck. Bkz: [el ile denetimi olup olmadığını bir kurtarma mümkün olduğundan](https://help.sap.com/saphelp_hanaplatform/helpdata/en/77/522ef1e3cb4d799bab33e0aeb9c93b/content.htm) Bu araçlar hakkında daha fazla bilgi için.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>Artıları ve eksileri HANA yedekleme depolama anlık görüntü karşılaştırması

SAP içermiyor&#39;t ya da HANA yedekleme depolama anlık görüntü karşı tercih verin. Böylece bir durum ve kullanılabilir depolama teknolojisi bağlı olarak kullanılacak belirleyebilir kendi Artıları ve eksileri, listeler (bkz [planlama bilgisayarınızı yedekleme ve kurtarma stratejisini](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

Azure üzerinde Azure blob'u özelliği mevcut değil anlık görüntü olgusu unutmayın&#39;t garantisi dosya sistemi tutarlılığı (bkz [PowerShell ile kullanma blob anlık görüntüleri](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)). Sonraki bölümde _depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığını_, bu özellik ile ilgili bazı hususlar anlatılmaktadır.

Ayrıca, bu makalede anlatıldığı gibi sık blob anlık görüntüleri ile çalışırken, fatura etkilerini anlamak varsa: [anlama nasıl anlık görüntüleri tahakkuk ücretleri](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)— bu olmayan&#39;t Azure sanal kullanarak olarak olarak belirgin diskler.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>Depolama anlık görüntüleri duruma getirirken SAP HANA veri tutarlılığı

Dosya sistemi ve uygulama tutarlılığı bir karmaşık depolama anlık görüntüleri alırken bir sorundur. Sorunları önlemek için en kolay yolu, SAP HANA kapatın, hatta belki de tüm sanal makine olacaktır. Bir kapanma demo veya prototip veya geliştirme sistemi ortak olabilir, ancak bir üretim sistem için bir seçenek değil.

Azure blob'u özelliği mevcut değil anlık görüntü göz önünde bulundurmanız varsa Azure üzerinde&#39;t garantisi dosya sistemi tutarlılığı. Var olduğu sürece yalnızca tek bir sanal disk dahil edilen ancak kullanarak SAP HANA özelliği, anlık görüntü düzgün çalışır. Ancak denetlenecek bile tek bir diske sahip, ek öğelere sahip. [SAP Not 2039883](https://launchpad.support.sap.com/#/notes/2039883) SAP HANA yedeklemeler depolama anlık görüntüleri aracılığıyla hakkında önemli bilgiler vardır. XFS dosya sistemiyle bunu çalıştırmak gerekli olduğunu, örneğin, değinmektedir **xfs\_Dondur** tutarlılığı garanti etmek için bir depolama anlık görüntü başlamadan önce (bkz [xfs\_freeze(8) - Linux adam sayfa](https://linux.die.net/man/8/xfs_freeze) hakkında ayrıntılı bilgi için **xfs\_Dondur**).

Tutarlılık konu daha zor olduğu tek bir dosya sistemi birden çok disk/birim yayılan bir durumda olur. Örneğin, mdadm veya LVM kullanarak ve bölümlemesine tarafından. SAP durumları bahsedilen Not:

_&quot;Ancak depolama sistemi SAP HANA veri birim başına bir depolama anlık görüntü oluşturulurken g/ç tutarlılığı garanti gerektiğini aklınızda bulundurun, yani atomik bir işlem olarak bir SAP HANA hizmete özgü veriler biriminin anlık görüntüsünün alınması olmalıdır.&quot;_

Dört Azure sanal diskler yayılan bir XFS dosya sistemi varsayıldığında, aşağıdaki adımları HANA veri alanını temsil tutarlı bir anlık görüntüsü sağlar:

- HANA anlık görüntü hazırlama
- Dosya sistemi donmasına (örneğin, **xfs\_Dondur**)
- Azure üzerinde tüm gerekli blob anlık görüntülerini oluşturma
- Dosya Sistemi Çöz
- HANA anlık görüntü onaylayın

Yukarıdaki yordamı hangi dosya sistemi olsun güvenli tarafında olması için tüm durumlarda kullanmak için önerilir. Veya tek bir disk veya mdadm veya birden çok disklere LVM üzerinden şeritleme ise.

HANA anlık görüntü doğrulamak önemlidir. Nedeniyle &quot;kopyalama yazarken,&quot; SAP HANA bu sırada ek disk alanı yok gerekebilir modu anlık görüntü hazırlayın. Bu&#39;s SAP HANA anlık görüntü onaylandıktan kadar yeni yedeklemeleri başlatmak de mümkün değildir.

Azure Backup hizmeti, dosya sistemi tutarlılığı halletmeniz için Azure VM uzantıları kullanır. Bu VM uzantıları, tek başına kullanım için kullanılamaz. SAP HANA tutarlılık yönetmek hala sahip. İlgili makaleye bakın [SAP HANA Azure Backup dosya düzeyinde](sap-hana-backup-file-level.md) daha fazla bilgi için.

### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA Yedekleme Zamanlama stratejisi

SAP HANA makaleyi [planlama bilgisayarınızı yedekleme ve kurtarma stratejisini](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) yedeklemeler için temel bir plan durumları:

- (Günlük) anlık görüntü depolama
- Dosya veya bacint biçimi (haftada bir kez) kullanarak tam yedekleme
- Otomatik günlük yedekleri

İsteğe bağlı olarak, bir depolama anlık görüntülerini kullanmadan tamamen gidebilir; Artımlı ya da fark yedeklemeler gibi HANA değişim yedeklemeleri yerini (bakın [değişim yedeklemeleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm)).

HANA Yönetim Kılavuzu bir örnek listesi sağlar. Bu, bir SAP HANA belirli bir noktaya yedeklemelerini aşağıdaki dizisi kullanılarak geri olduğunu önerir:

1. Tam yedekleme
2. Değişiklik yedeklemesi
3. Artımlı Yedekleme 1
4. Artımlı yedekleme 2
5. Günlük yedekleri

Ne zaman ve ne sıklıkta belirli bir yedekleme türü olacağını bir tam zamanlama ile ilgili bu genel bir kılavuz vermek mümkün değildir — çok müşteriye özgü ve kaç tane veri değişikliklerini sistemde ortaya bağlıdır. Bir temel genel rehberlik görülebilir, SAP taraftan yapma bir tam HANA yedekleme haftada bir kez önerilir.
İlgili günlüğü yedeklemeleri, SAP HANA belgelerine bakın [günlük yedeklerini](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/bb7e33bb571014a03eeabba4e37541/content.htm).

SAP de önerir sonsuz aşmasını önlemek için yedekleme Kataloğu'nun bazı temizlik yapılması (bkz [yedekleme kataloğu ve yedekleme depolama için Housekeeping](http://help.sap.com/saphelp_hanaplatform/helpdata/en/ca/c903c28b0e4301b39814ef41dbf568/content.htm)).

### <a name="sap-hana-configuration-files"></a>SAP HANA yapılandırma dosyaları

SSS bölümünde belirtildiği gibi [SAP Not 1642148](https://launchpad.support.sap.com/#/notes/1642148), SAP HANA yapılandırma dosyalarını bir standart HANA yedeklemenin bir parçası değildir. Bunlar, bir sistem geri yükleme için gerekli değildir. HANA yapılandırma el ile geri yüklemeden sonra değiştirilemedi. Bir geri yükleme işlemi sırasında aynı özel yapılandırmasını almak istediğiniz durumlarda HANA yapılandırma dosyaları ayrı olarak yedeklemek gereklidir.

Standart HANA yedeklemeler ayrılmış HANA yedek dosya sistemine kullanacaksanız, bir de aynı yedekleme dosya sistemi için yapılandırma dosyalarını kopyalayın ve ardından her şeyi birlikte cool gibi son bir depolama hedefi kopyalayın blob depolama.

### <a name="sap-hana-cockpit"></a>SAP HANA yönetim ekranı

SAP HANA Pilot izleme ve SAP HANA bir tarayıcı yoluyla yönetme olanağı sunar. SAP HANA yedeklemeler işlenmesini de sağlar ve bu nedenle SAP HANA Studio ve ABAP DBACOCKPIT alternatif olarak kullanılabilir (bkz [SAP HANA Pilot](https://help.sap.com/saphelp_hanaplatform/helpdata/en/73/c37822444344f3973e0e976b77958e/content.htm) daha fazla bilgi için).

![SAP HANA Pilot veritabanı yönetim ekran bu şekilde gösterilmektedir](media/sap-hana-backup-guide/image004.png)

Bu şekil, sol tarafta SAP HANA Pilot veritabanı yönetim ekran ve yedekleme döşeme gösterir. Yedekleme döşeme görmek için oturum açma hesabı uygun kullanıcı izinleri gerektirir.

![SAP HANA Pilot yedeklemeler devam eden sağlarken izlenebilir](media/sap-hana-backup-guide/image005.png)

Yedeklemeler, devam eden ve bu işlem tamamlandıktan sonra tüm yedekleme ayrıntıları kullanılabilir olsa da, SAP HANA Pilot izlenebilir.

![Bir Azure SLES 12 VM Gnome masaüstüyle Firefox kullanma örneği](media/sap-hana-backup-guide/image006.png)

Önceki ekran görüntüleri bir Azure Windows sanal makineden yapıldı. Bunu bir Azure SLES 12 VM Gnome masaüstüyle Firefox kullanarak bir örnektir. SAP HANA Pilot SAP HANA yedekleme zamanlamaları tanımlamak için seçeneğini gösterir. Biri de görüldüğü gibi yedekleme dosyalarının tarih/saat öneki olarak önerir. SAP HANA Studio'da varsayılan önektir &quot;tam\_veri\_yedekleme&quot; bir tam dosya yedeğini yaparken. Benzersiz bir önek kullanılması önerilir.

### <a name="sap-hana-backup-encryption"></a>SAP HANA yedek şifreleme

SAP HANA veri ve günlük şifrelenmesini sağlar. SAP HANA veri ve günlük şifrelenmezse yedekleri de şifrelenmez. SAP HANA yedeklemeleri şifrelemek için üçüncü taraf çözümü çeşit kullanmak üzere müşteri kadar değil. Bkz: [veri ve günlük birimi şifreleme](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/01f36fbb5710148b668201a6e95cf2/content.htm) SAP HANA şifreleme hakkında daha fazla bilgi için.

Microsoft Azure üzerinde bir müşteri Iaas VM şifreleme özelliği şifrelemek için kullanabilirsiniz. Örneğin, bir VM'ye bağlı SAP HANA yedeklemelerini depolamak ve ardından bu diskleri kopyalarını için kullanılan özel veri diskleri kullanabilirsiniz.

Azure Backup hizmeti, şifrelenen VM'ler/disklere işleyebilir (bkz [Azure yedekleme ile sanal makineleri yedeklemek ve geri yükleme şifrelenmiş](../../../backup/backup-azure-vms-encryption.md)).

SAP HANA VM ve şifreleme olmadan, disklerini korumak ve şifreleme etkinleştirildiğini bir depolama hesabında SAP HANA yedekleme dosyalarını depolamak için başka bir seçenek olabilir (bakın [bekleyen veri için Azure depolama hizmeti şifrelemesi](../../../storage/common/storage-service-encryption.md)).

## <a name="test-setup"></a>Test Kurulumu

### <a name="test-virtual-machine-on-azure"></a>Azure üzerinde test sanal makinesi

SAP HANA yüklemenin Azure GS5 VM'deki aşağıdaki yedekleme/geri yükleme testleri için kullanıldı.

![Bu şekilde HANA test VM için Azure portalına genel bakış parçası gösterilmektedir](media/sap-hana-backup-guide/image007.png)

Bu şekilde HANA test VM için Azure portalına genel bakış parçası gösterilmektedir.

### <a name="test-backup-size"></a>Test yedekleme boyutu

![Bu şekil HANA Studio'da yedekleme konsolundan alındığı ve HANA dizin sunucusu 229 GB yedekleme dosyası boyutunu gösterir](media/sap-hana-backup-guide/image008.png)

Bir kukla tablo gerçekçi performans verileri çıkarmaya üzerinde 200 GB toplam veri yedekleme boyutunu alma verilerle doldurulmuş. Şekil HANA Studio'da yedekleme konsolundan alındığı ve HANA dizin sunucusu 229 GB yedekleme dosyası boyutunu gösterir. Testler için varsayılan yedekleme önek SAP HANA Studio'da "COMPLETE_DATA_BACKUP" kullanıldı. Gerçek üretim sistemlerinde, daha kullanışlı bir önek tanımlanması gerekir. SAP HANA Pilot tarih önerir.

### <a name="test-tool-to-copy-files-directly-to-azure-storage"></a>Azure depolama alanına doğrudan dosyaları kopyalamak için aracı test

SAP HANA yedekleme dosyalarını doğrudan Azure blob depolama veya Azure dosya paylaşımları için aktarmak için çünkü her iki hedeflerini destekler ve Otomasyon betikleri kendi komut satırı arabirimi nedeniyle içine kolayca tümleştirilebilir blobxfer aracı kullanıldı. Blobxfer aracı kullanılabilir [GitHub](https://github.com/Azure/blobxfer).

### <a name="test-backup-size-estimation"></a>Yedekleme boyutu tahmin test

SAP HANA yedekleme boyutunu tahmin etmek önemlidir. Bu tahmin paralellik bir dosya kopyalama sırasında nedeniyle yedekleme dosyalarının sayısı en fazla yedekleme dosyası boyutu tanımlayarak performansını artırmaya yardımcı olur. (Bu ayrıntıları bu makalenin sonraki bölümlerinde açıklanmıştır.) Biri de tam yedekleme veya bir delta yedeklemeyi (artımlı ya da fark) yapmak karar vermeniz gerekir.

Neyse ki, yedekleme dosyalarının boyutunu tahminleri basit bir SQL deyimi yoktur: **seçin \* M gelen\_yedekleme\_BOYUTU\_TAHMİNLER** (bkz [dosya sisteminde bir veri yedekleme için ihtiyaç duyulan alanı tahmin](https://help.sap.com/saphelp_hanaplatform/helpdata/en/7d/46337b7a9c4c708d965b65bc0f343c/content.htm)).

![Bu SQL deyimini çıktısını diskteki tam veri yedek hemen hemen gerçek boyutunu eşleşir](media/sap-hana-backup-guide/image009.png)

Test sistemi için disk üzerinde tam veri yedekleme hemen hemen gerçek boyutu bu SQL deyimini çıktısını eşleşir.

### <a name="test-hana-backup-file-size"></a>Test HANA yedek dosya boyutu

![Bir HANA yedek dosyaları en fazla dosya boyutunu sınırlamak HANA Studio yedekleme Konsolu sağlar](media/sap-hana-backup-guide/image010.png)

HANA Studio yedekleme konsol HANA yedek dosyaları en fazla dosya boyutunu sınırlamak bir izin verir. Örnek ortamında, bu özellik, bir 230 GB yedek dosyanın yerine birden çok daha küçük yedekleme dosyalarını almak mümkün kılar. Daha küçük dosya boyutu performans üzerinde önemli bir etkisi vardır (ilgili makaleye bakın [SAP HANA Azure Backup dosya düzeyinde](sap-hana-backup-file-level.md)).

## <a name="summary"></a>Özet

Aşağıdaki tablolarda Artıları ve eksileri Azure sanal makinelerde çalışan SAP HANA veritabanına yedekleme çözümleri Göster test sonuçlarına dayanarak.

**SAP HANA dosya sistemini yedeklemek ve son yedekleme hedefi için yedekleme dosyalarını daha sonra kopyalayın**

|Çözüm                                           |Uzmanları                                 |Simgeler                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|VM disklerde HANA yedekleri koru                      |Hiçbir ek Yönetimi çalışmalarını     |Yerel VM disk alanını eats           |
|Blob depolama alanına yedekleme dosyalarını kopyalamak için Blobxfer aracı |Birden çok dosya kopyalamak için cool blob storage kullanma seçimi paralellik | Ek aracı Bakım ve özel komut dosyaları | 
|Powershell veya CLI yoluyla BLOB kopyalama                    |Gerekirse, hiçbir ek araç Azure Powershell veya CLI gerçekleştirilebilir |el ile işlemi, komut dosyası oluşturma ve geri yükleme için kopyalanan BLOB yönetiminin halletmeniz müşteri vardır.|
|NFS paylaşımına kopyalayın                                  |Diğer VM HANA sunucudaki etkisi olmadan yedekleme dosyaları sonrası işlenmesi|Yavaş kopyalama işlemi|
|Azure dosya hizmeti Blobxfer Kopyala                |Yerel VM disk alanı yemek değil|Hiçbir doğrudan HANA yedekleme, boyutu kısıtlaması şu anda 5 TB adresindeki dosya paylaşımının tarafından destek yazma|
|Azure Yedekleme aracısı                                 | Tercih edilen bir çözüm olacaktır         | Şu anda kullanılamıyor Linux    |



**Yedekleme SAP HANA depolama anlık görüntü tabanlı**

|Çözüm                                           |Uzmanları                                 |Simgeler                                  |
|---------------------------------------------------|-------------------------------------|--------------------------------------|
|Azure yedekleme hizmeti                               | Blob anlık görüntü tabanlı VM yedeklemeyi sağlar | Dosya düzeyinde geri yükleme kullanmadığınızda, gereken yeni bir SAP HANA lisans anahtar da anlaşılacağı geri yükleme işlemi için yeni bir VM oluşturulmasını gerektirir|
|El ile blob anlık görüntüler                              | Oluşturma ve benzersiz VM kimliği değiştirmeden belirli VM diskleri geri yüklemek için esneklik|Müşteri tarafından yapılması gerekir el ile çalışma|

## <a name="next-steps"></a>Sonraki adımlar
* [SAP HANA Azure yedekleme dosya düzeyinde](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneği açıklar.
* [SAP HANA yedekleme depolama anlık görüntü tabanlı](sap-hana-backup-storage-snapshots.md) depolama anlık görüntü tabanlı yedekleme seçeneği açıklar.
* Yüksek kullanılabilirlik ve olağanüstü durum kurtarma SAP HANA planlama azure'da (büyük örnekler) oluşturmayı öğrenmek için bkz: [SAP HANA (büyük örnekler) Azure üzerinde yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
