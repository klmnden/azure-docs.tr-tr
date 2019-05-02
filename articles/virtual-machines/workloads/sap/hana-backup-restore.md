---
title: HANA yedekleme ve geri yükleme (büyük örnekler) Azure üzerinde SAP HANA üzerinde | Microsoft Docs
description: HANA yedeklemesi gerçekleştirin ve SAP HANA (büyük örnekler) azure'da üzerinde geri yükleme
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c03a7e5763f580bf1e17232a5850064710c8227
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707239"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

>[!IMPORTANT]
>Bu belge, yönetim belgeleri SAP HANA veya SAP notları hiçbir yerini almaktadır. Okuyucu düz bir anlayış ve SAP HANA yönetimi ve işlemleri, özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları uzmanlığa sahip beklenmektedir. Bu belgede, SAP HANA Studio ekran görüntüleri gösterilmektedir. SAP HANA'dan yayın için yayın içerik, yapısı ve SAP Yönetim Araçları ve araçları ekranlar yapısını değiştirebilirsiniz.

Adımlar ve ortamınızdaki ve HANA sürümleri ve sürümler ile yapılan işlemler alıştırma önemlidir. Bu belgede açıklanan bazı işlemler, daha iyi bir genel anlamak için Basitleştirilmiş ve nihai işlemi el kitapları için ayrıntılı adımları olarak kullanılmak üzere tasarlanmamıştır. İşlemi el kitapları yapılandırmalarınız için oluşturmak istiyorsanız, test ve işlemlerinizi çalışma ve belirli yapılandırmalarınız için ilgili işlemleri belge gerekir. 

İşletim veritabanları için en önemli yönlerinden bunları yıkıcı olaylara karşı korur sağlamaktır. Neden bu olayların her şey doğal felaketlere basit kullanıcı hataları olabilir.

Bir veritabanını, zamanda herhangi bir noktasına geri yükleme olanağıyla yedekleme (gibi birisi kritik verileri silinmeden önceki), mümkün olduğunca yakın önce kesinti olduğu şekilde bir duruma geri yükleme sağlar.

İki tür yedekleme, geri yükleme yeteneği elde etmek için gerçekleştirilmelidir:

- Veritabanı Yedeklemeleri: Tam, artan ya da fark yedekleri
- İşlem günlüğü yedeklemeleri

Bir uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemeleri yanı sıra depolama anlık görüntüleri ile yedeklemelerini gerçekleştirebilir. Depolama anlık görüntüleri, işlem günlüğü yedeklemeleri değiştirmeyin. İşlem günlüğü yedeklemeleri önemli veritabanını belirli bir noktaya geri yükleme ya da önceden kaydedilen işlem sayısı günlüklerinden boş kalır. Ancak, depolama anlık görüntüleri, veritabanının sarma görüntüsünü hızlı bir şekilde sağlayarak kurtarma hızlandırabilirsiniz. 

SAP HANA (büyük örnekler) azure'da iki yedekleme ve geri yükleme seçeneği sunar:

- Yazılanları (DIY). Yeterli disk alanı olduğundan emin olduktan sonra aşağıdaki disk yedekleme yöntemlerden birini kullanarak tam bir veritabanı ve günlük yedekleri gerçekleştirin. Ya da doğrudan HANA büyük örneği birimlerine veya ağ dosya paylaşımları'için (NFS), bir Azure sanal makinesinde (VM) ayarlama bağlı birimleri yedekleyebilirsiniz. İkinci durumda, müşterilerin azure'da bir Linux sanal makinesi ayarlama, Azure depolama VM'e ekleyin ve bu sanal makinede yapılandırılan NFS sunucusu üzerinden depolama paylaşmak. HANA büyük örneği birimlerine doğrudan ekleme birimler yedekleme yapıyorsanız, (Azure depolama alanına dayalı NFS paylaşımlarını dışarı aktaran bir Azure VM ayarladıktan sonra), Azure depolama hesabınız için yedeklemeleri kopyalamanız gerekir. Ayrıca, Azure yedekleme kasası veya Azure soğuk depolama da kullanabilirsiniz. 

   Başka bir seçenek, bir Azure depolama hesabına kopyaladıktan sonra yedeklemeleri depolamak için üçüncü taraf veri koruma aracını kullanmaktır. Kendin YAP yedekleme seçeneği ayrıca, uyumluluk ve denetleme amaçları için uzun süreler için depolamanız gereken veriler için gerekli olabilir. Her durumda, bir sanal makine ve Azure depolama ile temsil edilen NFS paylaşımlarını içine yedeklemeleri kopyalanır.

- Altyapısını yedekleme ve geri yükleme işlevselliğinin. Yedekleme ve geri yükleme işlevselliğinin, SAP hana (büyük örnekler) azure'da temel alınan altyapı sağlar. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümün geri kalanında, HANA büyük örnekleri ile sunulan yedekleme ve geri yükleme işlevselliği ele alır. Bu bölüm ayrıca ilişki yedekleme kapsar ve geri yükleme sahip için olağanüstü durum kurtarma işlevselliğin sunduğu HANA büyük örnekleri.

> [!NOTE]
>   Altyapının HANA büyük örnekleri tarafından kullanılan anlık görüntü teknoloji, SAP HANA anlık görüntüleri bağımlılığı vardır. Bu noktada, SAP HANA anlık görüntüleri SAP HANA çok kiracılı veritabanı kapsayıcıların birden fazla Kiracı ile birlikte çalışmaz. Yalnızca tek bir kiracı dağıtılır, bu yöntem kullanılabilir ve SAP HANA anlık görüntüleri çalışır.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>(Büyük örnekler) Azure üzerinde SAP hana depolama anlık görüntüleri kullanma

SAP HANA (büyük örnekler) azure'da temel alınan depolama altyapısını birimlerin depolama anlık görüntüleri destekler. Hem yedekleme hem de birimlerin geri yüklenmesi desteklenir, ile aşağıdaki önemli noktalar:

- Tam veritabanı yedeklemeleri yerine, depolama birimi anlık görüntüleri sık kullanılan olarak alınır.
- Anlık görüntü /hana/data ve /hana/shared (/usr/sap içerir) üzerinden tetiklerken birimler, anlık görüntü teknoloji bir SAP HANA depolama anlık görüntüleri yürütülmeden önce anlık görüntü başlatır. Bu SAP HANA anlık görüntü Kurulum nihai günlük geri yüklemeler için depolama anlık görüntüsünün kurtarma işleminden sonra noktasıdır. HANA anlık başarılı olması etkin bir HANA örneği gerekir.  HSR senaryosunda, depolama anlık görüntüleri HANA anlık görüntü burada gerçekleştirilemiyor geçerli ikincil düğüm üzerinde desteklenmiyor.
- Depolama anlık görüntüleri başarıyla yürütüldükten sonra SAP HANA anlık görüntüsü silinir.
- İşlem günlüğü yedeklemeleri sık alınır ve /hana/logbackups toplu ya da azure'da depolanır. Ayrı olarak anlık görüntüsünü almak için işlem günlüğü yedeklemeleri içeren /hana/logbackups birimi tetikleyebilirsiniz. Bu durumda, bir HANA anlık görüntüsü yürütme gerekmez.
- Bir veritabanını belirli bir noktaya geri gerekir, bu Microsoft Azure desteği (bir üretim kesinti) veya SAP HANA Azure geri yükleme isteği için belirli depolama anlık görüntü. Bir planlı bir korumalı alan sistem geri yükleme özgün durumuna buna bir örnektir.
- Depolama anlık görüntüsüne dahil SAP HANA anlık görüntü yürütülen ve depolama anlık görüntü alındıktan sonra depolanan işlem günlüğü yedeklemeleri uygulamak için uzaklık bir noktadır.
- Bu işlem günlüğü yedeklemeleri geri belirli bir noktaya veritabanını geri yönlendirilirsiniz.

Depolama anlık görüntüleri birimlerin üç sınıf hedefleyen gerçekleştirebilirsiniz:

- Birleşik bir anlık görüntü/hana/veri ve /hana/shared (/ usr/sap içerir). Bu anlık görüntüyü bir SAP HANA anlık görüntü depolama anlık görüntüleri için hazırlık olarak oluşturulmasını gerektirir. SAP HANA anlık görüntü veritabanı bir depolama açısından tutarlı bir durumda olduğundan emin olur. Ve geri yüklemek için diğer bir deyişle bir noktaya ayarlama işlemi üzerinde çalışır.
- Ayrı bir anlık görüntü/hana/logbackups üzerinden.
- Bir işletim sistemi bölümü.

En son anlık görüntü betikleri ve belgelerinden almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Anlık görüntü betik paketi indirme zaman [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), üç dosyayı PDF belgeleri için sağlanan işlevselliği biridir alın. Bölüm yönergesinde boyunca 'anlık görüntü araçları alma' olduğunda devam etmesini sağlayın aracı set indiriliyor.

## <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü konuları

>[!NOTE]
>Depolama anlık görüntüleri, HANA büyük örneği birimlerine ayrılan depolama alanını kullanır. Depolama anlık görüntüleri ve kaç depolama anlık görüntüleri tutmak için zamanlama ilgili aşağıdaki noktaları göz önünde bulundurmanız gerekir. 

Belirli mekaniklerini (büyük örnekler) Azure üzerinde SAP HANA depolama anlık görüntüleri içerir:

- Belirli depolama anlık görüntü (noktada zaman alınır) az depolama kullanır.
- Veri içerik değişikliklerini ve SAP HANA veri dosyaları depolama biriminde değiştirin içeriği anlık görüntü veri değişikliklerini yanı sıra, özgün blok içeriği depolamak gerekir.
- Sonuç olarak, depolama anlık görüntü boyutunu artırır. Uzun süre anlık görüntü var, daha büyük depolama anlık görüntüsü olur.
- SAP HANA veritabanı birimin anlık görüntü depolama alanı tüketimi daha büyük bir depolama anlık ömrü boyunca yapılan değişiklik.

(Büyük örnekler) Azure üzerinde SAP HANA, SAP HANA veri ve günlük birimlerini için sabit bir birim boyutları ile birlikte gelir. Bu birimlerin anlık görüntüleri gerçekleştirme birim alanınızı eats. Belirlemek depolama anlık görüntüleri zamanlamak ne zaman gerekir. Ayrıca, depolamanın anlık görüntü sayısını yönetme yanı sıra depolama birimleri alanı tüketimini izlemek gerekir. Veri masses aktardığınızda veya diğer önemli değişiklikler HANA veritabanına gerçekleştirmek, depolama anlık görüntüleri devre dışı bırakabilirsiniz. 


Aşağıdaki bölümlerde, genel öneriler de dahil olmak üzere, bu anlık görüntüler gerçekleştirmek için bilgileri sağlayın:

- Donanım birim başına 255 anlık görüntüleri karşılayabilir olsa da bu sayının altına kalmasını istiyorsunuz. 250 veya daha az kullanılması önerilir.
- Depolama anlık görüntüleri gerçekleştirmeden önce izlemek ve boş alan izler.
- Boş alan bağlı depolama anlık görüntü sayısını azaltın. Sizi anlık görüntü sayısını azaltabilirsiniz veya birimleri genişletebilirsiniz. Ek depolama alanı 1 terabayt birimleri cinsinden sipariş edebilirsiniz.
- SAP platformu Geçiş Araçları (R3load) ile SAP HANA ile verileri taşıma veya SAP HANA veritabanlarını yedeklerden geri yükleme gibi etkinlikleri sırasında depolama anlık görüntüleri /hana/data birimde devre dışı bırakın. 
- Sırasında daha büyük düzenlemelere tablolarının SAP HANA depolama anlık görüntüleri, mümkünse kaçınılmalıdır.
- Depolama anlık görüntüleri, olağanüstü durum kurtarma özellikleri SAP hana (büyük örnekler) Azure'da yararlanarak için bir önkoşuldur.

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Self Servis depolama anlık görüntüleri kullanma önkoşulları

Anlık görüntü betiği başarıyla çalıştığından emin olmak için Perl Linux işletim sisteminde HANA büyük örnekleri sunucuda yüklü olduğundan emin olun. Perl, HANA büyük örneği biriminde önceden yüklenmiş olarak sunulur. Perl sürümü denetlemek için aşağıdaki komutu kullanın:

`perl -v`

![Ortak anahtarı, bu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Depolama anlık görüntüleri ayarlama

Depolama anlık görüntüleri HANA büyük örnekleri ile ayarlamak için aşağıdaki adımları izleyin:
1. Perl HANA büyük örnekleri sunucuda Linux işletim sisteminde yüklü olduğundan emin olun.
1. Değiştirme/etc/ssh/ssh\_satır eklemek için yapılandırma _Mac hmac-sha1_.
1. Varsa çalıştırıyorsanız, her bir SAP HANA örneği için ana düğüm üzerinde SAP HANA yedekleme kullanıcı hesabı oluşturun.
1. SAP HANA HDB istemci tüm SAP HANA büyük örnekleri sunucularına yükleyin.
1. İlk SAP HANA büyük örnekleri sunucusunda, her bölgenin anlık görüntü oluşturmayı denetleyen temel alınan depolama altyapıya erişim için ortak bir anahtar oluşturun.
1. Betikler ve yapılandırma dosyasından kopyalayın [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) konumunu **hdbsql** SAP HANA yükleme.
1. Değiştirme *HANABackupDetails.txt* için uygun müşteriyi belirtimleri gerektiğinde dosya.

En son anlık görüntü betikleri ve belgelerinden almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Ayrıntılı adımlar için yukarıda listelenen başvurmak [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

### <a name="consideration-for-mcod-scenarios"></a>MCOD senaryoları için önemli noktalar
Çalıştırıyorsanız bir [MCOD senaryo](https://launchpad.support.sap.com/#/notes/1681092) bir HANA büyük örneği biriminde birden çok SAP HANA örnekleri ile sağlanan her bir SAP HANA örnekleri için ayrı depolama birimi vardır. MDC ve diğer önemli noktalar hakkında daha fazla bilgi için kontrol [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) bölüm **' Anımsanması gereken önemli noktalar'**.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>1. Adım: SAP HANA HDB istemcisini yükleme

Linux işletim sistemi yüklü (büyük örnekler) Azure üzerinde SAP HANA, SAP HANA depolama anlık görüntüleri yedekleme ve olağanüstü durum kurtarma amacıyla yürütmek gereken komut dosyaları ve klasörleri içerir. Daha yeni sürümlerde denetle [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Komut en son sürümü 4.0 ' dir. Farklı komut dosyaları, aynı ana sürümüne ait içinde farklı alt sürümleri olabilir.

SAP HANA yüklerken HANA büyük örneği birimlerine göre SAP HANA HDB istemciyi yüklemek için sizin sorumluluğunuzdur.

### <a name="step-2-change-the-etcsshsshconfig"></a>2. Adım: Değiştirme/etc/ssh/ssh\_yapılandırma

Bu adım için daha yeni sürümlerde denetimi ayrıntılı açıklanan [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) bölümdeki **'Depolama ile iletişimi etkinleştir'**


### <a name="step-3-create-a-public-key"></a>3. Adım: Bir ortak anahtar oluşturma

Depolama anlık görüntü arabirimler HANA büyük örneği kiracınızın erişimi etkinleştirmek için bir ortak anahtar ile oturum açma yordamı yapmanız gerekir. Kiracınızda Azure (büyük örnekler) sunucusundaki ilk SAP HANA üzerinde depolama altyapıya erişim için kullanılacak ortak bir anahtar oluşturun. Ortak anahtar, parola depolama anlık görüntü arabirimleri oturum açmak için gerekli değildir sağlar. Bir ortak anahtar oluşturma ayrıca parola kimlik bilgilerini korumak gerekmez anlamına gelir. Tam genel oluşturma adımları anahtar açıklanan [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) bölümdeki **'Depolama ile iletişimi etkinleştir'**


### <a name="step-4-create-an-sap-hana-user-account"></a>4. Adım: Bir SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntüleri oluşturulmasını başlatmak için depolama anlık görüntü betikleri kullanabileceğiniz SAP HANA'da bir kullanıcı hesabı oluşturmanız gerekir. Bu amaç için SAP HANA Studio içinden bir SAP HANA kullanıcı hesabı oluşturun. Kullanıcı için MDC SYSTEMDB SID veritabanı altında değildir ve oluşturulmalıdır. Tek kapsayıcı ortamında kullanıcı Kiracı veritabanında oluşturulur. Bu hesap aşağıdaki ayrıcalıklara sahip olmalıdır: **Yedekleme Yönetim** ve **okuma katalog**. Kullanıcı ve kullanıcı kullanmak tam adımlar için okuma [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) bölümdeki **'SAP HANA ile iletişimi etkinleştir'**


### <a name="step-5-authorize-the-sap-hana-user-account"></a>5. Adım: SAP HANA kullanıcı hesabı yetki

Bu adımda, betiklerin çalışma zamanında parolalar göndermek gerekmez, sizin oluşturduğunuz, SAP HANA kullanıcı hesabı yetkilendirin. SAP HANA komut `hdbuserstore` bir veya daha fazla SAP HANA düğümde depolanan bir SAP HANA kullanıcı anahtarı oluşturulmasını sağlar. Kullanıcı anahtarı kullanıcı SAP HANA komut dosyası süreci içinde parolalarını yönetmenize gerek kalmadan erişebilir. Betik oluşturma işlemi, bu makalenin sonraki bölümlerinde ele alınmıştır.

>[!IMPORTANT]
>Anlık görüntü komutları yürütülür aynı kullanıcı bağlamı ile bu yapılandırma komutlarını çalıştırın. Aksi takdirde, anlık görüntü komutları düzgün çalışamaz.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>6. Adım: Anlık görüntü betiklerini alma, anlık görüntüleri yapılandırma ve test yapılandırması ve bağlantı

Betiklerin en son sürümünü indirin [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Betikleri yüklenmesi olacak şekilde majorly betikleri 4.0 sürümü ile değiştirildi. Tam ayrıntılı bilgi edinmek için [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) bölümdeki **'SAP HANA ile iletişimi etkinleştir'**

Komutları için kullanılacak tam sırasını bölümünü okuyun **'Anlık görüntü Araçları (varsayılan), kolay yükleme'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Varsayılan yükleme kullanımını öneririz. Sürümünden yükseltmek istiyorsanız 3.x-4.0 denetleyin bölümü **'var olan yüklemeyi yükseltmek'** , [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 4.0 araç kümesinde kaldırmak için yönergeleri izleyin **'Anlık görüntü Araçları'nın kaldırılmasını'** içinde [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

İçinde açıklanan adımları yürütün unutmayın **'Anlık görüntü Araçları'nın tam Kurulum'** , [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Farklı betikleri ve dosyaları amacı yüklü olarak listelendiği ve ayrıntılı **'Bu anlık görüntü Araçlar nelerdir?'** Belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Anlık görüntü araçları yapılandırmadan önce ayrıca HANA yedekleme konum ve ayarlar doğru bir şekilde açıklandığı gibi yapılandırılmış emin olun **'SAP HANA Yapılandırması'** belgenin [Microsoft araçları SAP HANA için anlık görüntü azure'da](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Yapılandırmanın anlık görüntüsünü araç ayrıntılı olarak açıklanan **'Yapılandırma dosyası - HANABackupCustomerDetails.txt'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="testing-connectivity-with-sap-hana"></a>SAP HANA ile bağlantı test ediliyor

Tüm yapılandırma verilerini içine yerleştirdiğiniz sonra *HANABackupCustomerDetails.txt* dosya, yapılandırmaları için HANA örneği verileri doğru olup olmadığını denetleyin. Betiği kullanmak `testHANAConnection`, bir SAP HANA ölçek büyütme veya ölçek genişletme yapılandırmasını bağımsız olduğu.

Ayrıntılar için bkz. **'Denetle, SAP HANA - testHANAConnection bağlantıyla'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf)

#### <a name="testing-storage-connectivity"></a>Depolama bağlantı test ediliyor

Sonraki adım yerleştirmiş verileri temel alan depolama bağlantısı denetlemektir *HANABackupCustomerDetails.txt* yapılandırma dosyası ve bir test anlık görüntü yürütebilirsiniz. Yürütmeden önce `azure_hana_backup` komutu, bu test çalıştırması gerekir. Bu test için komutları dizisini listelenen **'Denetle depolama - testStorageSnapshotConnection bağlantıyla'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Sonra bir başarılı oturum açma depolama sanal makine arabirimleri, betik, Aşama 2 ile devam eder ve test anlık görüntüsünü oluşturur. Çıktı, SAP HANA için bir üç düğümlü genişleme yapılandırması aşağıda gösterilmiştir:

Test anlık görüntü ile betiği başarıyla yürütüldü, gerçek depolama anlık görüntüleri zamanlama ile devam edebilirsiniz. Başarılı olmazsa, geçmeden önce sorunları araştırın. Gerçek ilk anlık görüntülerin, bunlar tamamlanana kadar geçici olarak test anlık görüntü kalmalıdır.


### <a name="step-7-perform-snapshots"></a>7. Adım: Anlık görüntüleri

Hazırlık adımlarını tamamladığınızda, yapılandırın ve planlayın gerçek depolama anlık görüntüleri başlayabilirsiniz. Zamanlanmış komut dosyası, SAP HANA ölçek büyütme ve ölçek genişletme yapılandırmaları ile çalışır. Düzenli ve normal yedekleme betik yürütme işlemi için cron yardımcı programını kullanarak betiği zamanlayın. 

Tam komut sözdizimi ve işlevselliği için okuma **'Gerçekleştirme anlık görüntü yedekleme - azure_hana_backup'** belgenin [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).  

Betik yürütme işlemi `azure_hana_backup` aşağıdaki üç aşamaya anlık görüntü depolama oluşturur:

1. SAP HANA anlık yürütür
1. Depolama anlık görüntüleri yürütür
1. Depolama anlık görüntünün yürütmeden önce oluşturulan SAP HANA anlık görüntü kaldırır

Betiği çalıştırmak için kopyalanmış olması HDB yürütülebilir klasöründen çağırın. 

Saklama dönemi komut yürütürken, parametre olarak gönderilen bir anlık görüntü sayısı ile yönetilir. Depolama anlık görüntüleri tarafından kapsanan süreyi betiği yürütülürken, bir parametre olarak gönderilen bir anlık görüntü sayısını ve yürütme süresi bir işlevdir. Tutulan anlık görüntü sayısını betiğin çağrısında bir parametre olarak adlandırılan sayıyı aşarsa, yeni bir anlık görüntü yürütülmeden önce aynı etiketi en eski depolama anlık görüntüsünü silinir. Çağrının son parametre sayısı tutulduğu anlık görüntü sayısını denetlemek için kullanabileceğiniz olarak sayı size sunar. Bu sayı ile de, dolaylı olarak, anlık görüntüler için kullanılan disk alanını denetleyebilirsiniz. 


## <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türleri için anlık görüntü sıklığı, HANA büyük örneği olağanüstü durum kurtarma işlevlerini kullanmasına bağlı olarak değişir. Bu işlev özel öneriler depolama anlık görüntü sıklığı ve yürütme süreleri için yapmanızı şart koşabileceği depolama anlık görüntüleri kullanır. 

Önemli noktalar ve öneriler izleyin yapmanızı varsayılır *değil* HANA büyük örnekleri sağlayan olağanüstü durum kurtarma işlevlerini kullanın. Bunun yerine, son 30 güne ait kurtarma noktasını zamanında sağlayabilir ve yedeklemelere sahip depolama anlık görüntüleri kullanın. Anlık görüntüler ve alan sayısı sınırlamaları göz önünde bulundurulduğunda, müşterilerin aşağıdaki gereksinimleri göz önünde bulundurduğunuzdan:

- Belirli bir noktaya kurtarma için kurtarma zamanı.
- Kullanılan alan.
- Kurtarma noktası ve kurtarma zamanı hedeflerine olası bir olağanüstü durum kurtarma için.
- HANA tam veritabanı yedeklemeleri diskleri karşı nihai yürütülmesi. Tam veritabanı yedeği her diskleri karşı veya **backint** arabirimi gerçekleştirilir, depolama anlık görüntüleri yürütülmesi başarısız oluyor. Depolama anlık görüntüleri üzerinde tam veritabanı yedeklemeleri yürütme planlıyorsanız, depolama anlık görüntüleri yürütülmesini bu süre boyunca devre dışı bırakıldığından emin olun.
- Anlık görüntü sayısı (250 ile sınırlı) birim başına.

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

HANA büyük örnekleri olağanüstü durum kurtarma işlevlerini kullanma müşteriler, anlık görüntü daha az sıklıkta dönemdir. Böyle durumlarda, müşteriler birleşik anlık görüntüleri /hana/data ve (/usr/sap içerir) /hana/shared 12 saat veya 24 saatlik dönem içinde gerçekleştirmek ve bunlar bir ay için anlık görüntüleri tutmak. Aynı günlük yedekleme birimi anlık görüntüleri ile geçerlidir. Bununla birlikte, SAP HANA işlem günlüğü yedeklemeleri günlük yedekleme birimi karşı yürütülmesi 5-15 dakika dönemlerde gerçekleşir.

Zamanlanmış depolama anlık görüntüleri cron kullanarak en iyi şekilde gerçekleştirilir. Tüm yedeklemeleri ve olağanüstü durum kurtarma gereksinimleri için aynı komut dosyasını kullanın ve yedekleme sürelerine istenen komut dosyası girişleri çeşitli eşleşecek şekilde değiştirin. Bu anlık görüntüler tüm farklı cron yürütme zamanları bağlı olarak, zamanlanmış: saat, 12 saatlik, günlük veya haftalık. 

/ Etc/crontab bir cron zamanlama örneği verilmiştir:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
Önceki örnekte, yok/hana/verileri içeren birimlerin ve (/ usr/sap içerir) /hana/shared/SID kapsayan birleştirilmiş bir saatlik anlık konumları. Bu tür bir anlık görüntü için daha hızlı bir-belirli bir noktaya kurtarma son iki gün içinde kullanın. Ayrıca, bu birimlerde günlük anlık görüntü yok. Bu nedenle, iki gün kapsamı saatlik anlık görüntüleri yanı sıra kapsamı dört haftasını tarafından günlük anlık görüntüler görüntülenerek sahip. Ayrıca, işlem günlüğü yedekleme birimi günlük yedek desteklenir. Bu yedeklemeler de dört hafta boyunca tutulur. Crontab üçüncü satırında gördüğünüz gibi HANA işlem günlüğü yedeklemesi her 5 dakikada yürütülecek şekilde zamanlanır. Depolama anlık görüntüleri yürütülen farklı cron işlerinin başlangıç zamanları dağılır, böylece bu anlık görüntülerin belirli bir noktada aynı anda zaman yürütülmez. 

Aşağıdaki örnekte, gerçekleştirdiğiniz/hana/veri ve /hana/shared/SID (/ usr/sap dahil) içeren birim kapsayan birleşik bir anlık görüntü saatlik olarak konumları. Bu anlık görüntüler'i iki gün boyunca tutun. İşlem günlüğü yedekleme birimlerin anlık görüntüleri 5 dakikalık olarak yürütülen ve 4 saat boyunca tutulur. Olarak önce HANA işlem günlüğü dosyasının yedeğini 5 dakikada yürütülecek şekilde zamanlanır. İşlem günlüğü yedeklemesi başlatıldıktan sonra işlem günlüğü yedekleme birimin anlık görüntüsünü 2 dakikalık bir gecikmeyle gerçekleştirilir. Bu 2 dakika içinde normal koşullarda SAP HANA işlem günlüğü yedeklemesinin tamamlanması gerekir. Olarak önce önyükleme içeren birimi LUN günde bir kez yedekleme depolama anlık görüntü tarafından desteklenir ve dört hafta boyunca tutulur.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

Aşağıdaki grafikte, önceki örnekte, LUN önyükleme hariç dizileri gösterilmektedir:

![Yedekleme ve anlık görüntü arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanına yapılan değişiklikleri belgelemek için /hana/log birime yönelik normal yazma gerçekleştirir. Düzenli olarak, SAP HANA /hana/data birime bir kayıt noktası yazar. Bir SAP HANA işlem günlüğü yedeklemesi crontab içinde olarak belirtilen, her 5 dakikada bir çalıştırılır. Ayrıca, bir SAP HANA anlık görüntü /hana/data ve /hana/shared/SID birimler üzerinde birleşik depolama anlık görüntü tetikleme sonucunda saatte yürütülür bakın. Birleştirilmiş depolama anlık görüntüleri, HANA anlık görüntünün başarılı olduktan sonra yürütülür. Crontab belirtildiği gibi depolama anlık görüntüleri /hana/logbackup birimdeki her 5 dakika, yaklaşık 2 dakika sonra HANA işlem günlüğü yedeklemesi yürütülür.

> 

>[!IMPORTANT]
> Anlık görüntüleri SAP HANA işlem günlüğü yedeklemeleri ile birlikte gerçekleştirildiğinde depolama anlık görüntüleri kullanımını SAP HANA yedeklemeler için değerlidir. Depolama anlık görüntü arasındaki zaman aralıklarını karşılamak bu işlem günlüğü yedeklemeleri gerekir. 

Kullanıcılara bir taahhüt 30 günlük bir zaman içinde nokta kurtarma ayarladıysanız, için gerekir:

- Aşırı durumlarda anlık görüntü /hana/data ve 30 gün önce yapılmışsa /hana/shared/SID üzerinde birleşik bir depolama erişim.
- Herhangi bir birleştirilmiş depolama anlık görüntüleri arasında kapsamak bitişik işlem günlüğü yedeklemeleri vardır. Bu nedenle, işlem günlüğü yedekleme biriminin eski anlık görüntü 30 günden daha eski olması gerekir. Azure depolama alanında bulunan başka bir NFS paylaşımına işlem günlüğü yedeklemeleri kopyalarsanız, durum değil. Bu durumda, eski işlem günlüğü yedeklemeleri, NFS paylaşımından çekme.

Depolama anlık görüntüleri ve işlem günlüğü yedeklemeleri nihai depolama çoğaltması yararlanmak için işlem günlüğü yedeklemeleri, SAP HANA yazacağı konum değiştirmeniz gerekir. Bu değişiklik, HANA Studio içinde yapabilirsiniz. SAP HANA tam günlük segmentleri otomatik olarak yedekler rağmen günlüğü yedekleme aralığı belirleyici olarak belirtmeniz gerekir. Olağanüstü durum kurtarma seçeneğini kullandığınızda, genellikle günlük yedeklerinin belirleyici noktayla yürütmek istediğiniz çünkü bu özellikle doğrudur. Aşağıdaki örnekte, 15 dakika günlüğü yedekleme aralığı ayarlanır.

![SAP HANA SAP HANA Studio yedekleme günlük zamanlama](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ayrıca, her 15 dakikadan daha sık yedeklemeler de seçebilirsiniz. Daha sık bir ayar, genellikle olağanüstü durum kurtarma işlevleri HANA büyük örnekleri ile birlikte kullanılır. Bazı müşteriler, işlem günlüğü yedeklemeleri her 5 dakikada gerçekleştirin.  

Veritabanı hiçbir zaman yedeklendiğinden, son yedekleme kataloğunu içinde bulunması gereken tek bir yedekleme girişi oluşturmak için bir dosya tabanlı veritabanı yedeklemesi gerçekleştirmeyi adımdır. Aksi takdirde, SAP HANA, belirtilen günlük yedeklemelerinizi başlatamazsınız.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı bir yedek alın](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


İlk başarılı depolama anlık görüntüleri yürütülen sonra 6. adımda yürütülen test anlık görüntüyü silmek gerekir. Okuma **'Remove test anlık - removeTestStorageSnapshot'** belgedeki [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf) Ayrıntılar için. 


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Sayısı ve disk birimi anlık görüntü boyutunu izleme

Belirli depolama biriminde, anlık görüntüler ve bu anlık görüntü depolama tüketimini sayısını izleyebilirsiniz. `ls` Değildir komut Göster anlık görüntü dizin veya dosya. Ancak, Linux işletim sistemi komut `du` aynı birimlere depolandığından, bu depolama anlık görüntüleri hakkında ayrıntılı bilgiler gösterilir. Komut aşağıdaki seçenekler kullanılabilir:

- `du –sh .snapshot`: Bu seçenek, toplam anlık görüntü dizini içindeki tüm anlık görüntüleri sağlar.
- `du –sh --max-depth=1`: Bu seçenek kaydedilen tüm anlık görüntüleri listeler **.snapshot** klasörü ve her anlık görüntü boyutu.
- `du –hc`: Bu seçenek, tüm anlık görüntüler görüntülenerek kullanılan toplam boyutu sağlar.

Birimlerde tüm depolama alınan ve depolanan anlık görüntüleri kullanan değil emin olmak için şu komutları kullanın.

>[!NOTE]
>Anlık görüntüleri LUN kimliğini önceki komutlara görünmez.

### <a name="getting-details-of-snapshots"></a>Anlık görüntü ayrıntılarını alma
Anlık görüntüleri hakkında daha fazla bilgi almak için de komut dosyasını kullanabilirsiniz `azure_hana_snapshot_details`. Bu betik, olağanüstü durum kurtarma konumunuz etkin bir sunucu ise iki konumdan birinde çalıştırılabilir. Anlık görüntüleri içeren her birime göre ayrılmış aşağıdaki çıktı, komut dosyası sağlar: 
   * Bir birim toplam anlık görüntü boyutu
   * Her birim anlık aşağıdaki ayrıntıları: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme varsa bu anlık görüntü ile ilişkili kimliği

Sözdizimi komut ve çıktı denetiminin **'Listesinde, anlık görüntüler - azure_hana_snapshot_details'** belgedeki [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Bir sunucu üzerinde anlık görüntü sayısını azaltma

Daha önce açıklandığı gibi belirli etiketleri, depolamanın anlık görüntü sayısını azaltabilirsiniz. Son iki anlık görüntü başlatmak için komut etiket ve korumak istediğiniz anlık görüntü sayısını parametrelerdir.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

Önceki örnekte, anlık görüntü etikettir **dailyhana** ve korunması için bu etikete sahip anlık görüntü sayısını **28**. Disk alanı tüketimini için yanıt olarak depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Son parametre kümesine sahip betiği çalıştırmak için 15'e, örneğin, anlık görüntü sayısını azaltmak için en kolay yolu olan **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Bu ayar ile betik çalıştırırsanız, yeni depolama anlık görüntüleri dahil olmak üzere bir anlık görüntü, sayısı 15'tir. 15 en son anlık görüntüleri korunur ve 15 eski anlık görüntüleri silinir.

 >[!NOTE]
 > Bu betik, birden fazla 1 saat öncesine anlık görüntüler varsa anlık görüntü sayısını azaltır. Komut dosyasını kısa 1 saat öncesine aittir anlık görüntüleri silmez. Bu kısıtlamalar, sunulan isteğe bağlı bir olağanüstü durum kurtarma işlevselliği ilgilidir.

Anlık görüntüleri yedekleme ön ekine sahip bir dizi korumak istiyorsanız, **dailyhana** söz dizimi örneklerde, komut dosyasını çalıştırabilirsiniz **0** bekletme sayı olarak. Ardından, bu etiketle eşleşen tüm anlık görüntüler kaldırılır. Ancak, tüm anlık görüntüleri kaldırma, HANA büyük örnekleri olağanüstü durum kurtarma işlevi yeteneklerini etkileyebilir.

Betiği kullanmak için belirli anlık görüntüleri silmek için ikinci bir seçenek olan `azure_hana_snapshot_delete`. Bu betik bir anlık görüntü veya anlık görüntüleri olarak HANA Studio ya da anlık görüntü adı aracılığıyla bulunan HANA yedekleme kimliği kullanarak ya da kümesini silmek için tasarlanmıştır. Şu anda, yedekleme kimliği yalnızca için oluşturulan anlık görüntüleri bağlıdır **hana** anlık görüntü türü. Anlık görüntü türü yedeklerini **günlükleri** ve **önyükleme** bir SAP HANA anlık görüntü işlemleri yapma ve bu nedenle bu anlık görüntüler için bulunması için hiçbir yedekleme kimliği yok. Anlık görüntü adı girildiğinde, tüm anlık görüntüler farklı birimlerde girilen anlık görüntü adı ile eşleşmesi için arar. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Komut dosyası hakkında daha fazla bilgi için bkz **'anlık görüntü - azure_hana_snapshot_delete Delete'** belgedeki [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Kullanıcı olarak komut dosyası yürütme **kök**.

>[!IMPORTANT]
>Yalnızca bir anlık görüntü üzerinde mevcut veriler varsa sildiğiniz, anlık görüntüsü silindikten sonra verilerin devamlı kayıp olduğundan.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Depolama anlık görüntüden dosya düzeyinde geri yükleme

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
Anlık Görüntü türleri için **hana** ve **günlükleri**, anlık görüntüleri birimler üzerinde doğrudan erişebileceğiniz **.snapshot** dizin. Her anlık görüntü için bir alt yoktur. Anlık görüntüden gerçek dizin yapısında, alt noktasında olduğu durumda, her dosya kopyalayabilirsiniz. Betik geçerli sürümünde yoktur **Hayır** (Self Servis DR parçası DR sitede yük devretme sırasında komutlar olarak anlık görüntü geri yükleme gerçekleştirilebilir ancak) anlık görüntü geri yükleme için bir Self Servis sağlanan komut dosyasını geri yükleyin. Microsoft Operasyon ekibinin mevcut kullanılabilir anlık görüntülerden istenen anlık görüntü geri yüklemek için bir hizmet isteği açarak başvurmanız gerekir.

>[!NOTE]
>Tek dosya geri yükleme LUN HANA büyük örneği birimlerinin türünü bağımsız önyükleme anlık görüntüleri için geçerli değildir. **.Snapshot** dizin önyükleme LUN gösterilmez. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüye Kurtar

Bir üretim aşağı senaryosunda, Microsoft Azure desteği ile bir müşteri olayı olarak depolama anlık görüntüden kurtarma işlemi başlatılabilir. Verileri bir üretim sisteminde silindi ve üretim veritabanını geri yüklemek için onu almak tek yolu ise yüksek aciliyet sağlasa da konusudur.

Farklı bir durumda-belirli bir noktaya kurtarma düşük aciliyet olabilir ve gün önceden planlanmış. Yüksek öncelikli bayrağı oluşturma yerine, Azure üzerinde SAP HANA ile bu kurtarma planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılımının yükseltmeyi planlama. Ardından geliştirme paket yükseltmeden önce durumunu temsil eden bir anlık görüntüye geri gerekir.

İstek göndermeden önce hazırlamanız gerekir. Azure ekibi üzerinde SAP HANA isteği ve geri yüklenen birimlere sağlayın. Daha sonra anlık görüntülerine dayalı HANA veritabanını geri yükleyin.

Yeni araç kümesiyle geri anlık görüntüsünü almak için olasılık bölümünde belgelenmiştir **'anlık görüntü geri yükleme'** belgenin [el ile Kurtarma Kılavuzu için azure'da SAP HANA depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Aşağıdaki, istek için hazırlama işlemini göstermektedir:

1. Anlık görüntüyü geri yüklemek için karar verin. Aksi takdirde toplamasını sürece yalnızca hana/veri hacmi geri yüklenir. 

1. HANA örneğini kapatın.

   ![HANA örneğini kapatma](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Veri birimleri üzerinde her HANA veritabanı düğümü çıkarın. Veri birimleri için işletim sistemi bağlıysa, anlık görüntü geri yükleme başarısız olur.
   ![Veri birimleri üzerinde her HANA veritabanı düğümü çıkarın](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Azure destek isteği açın ve belirli bir anlık görüntü geri yükleme hakkında yönergeler içerir.

   - Geri yükleme işlemi sırasında: Azure'da SAP HANA koordinasyon, doğrulama ve doğru depolama anlık görüntünün geri yüklendiğini doğrulama sağlamak için bir konferans katılım için isteyebilir. 

   - Geri yükleme sonrasında: Azure hizmeti üzerinde SAP HANA depolama anlık görüntü geri olduğunda size bildirir.

1. Geri yükleme işlemi tamamlandıktan sonra tüm veri miktarları yeniden bağlayın.

   ![Tüm veri birimleri yeniden bağlayın](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Örneğin, SAP HANA veri dosyalarını storage anlık görüntüden geri almak için başka bir olasılık 7. adımda belgenin belgelenen [el ile Kurtarma Kılavuzu için azure'da SAP HANA depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Belge [el ile Kurtarma Kılavuzu için azure'da SAP HANA depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) anlık görüntü yedeğinden geri yükleme sırasını göstermektedir. Bu belgeleri geri yürütülmesi için kullanın. 

>[!Note]
>7. adım, Microsoft işlemleri tarafından geri anlık geldiyseniz yürütmek gerekli değildir.


### <a name="recover-to-another-point-in-time"></a>Başka bir noktasına geri
Belge [el ile Kurtarma Kılavuzu için azure'da SAP HANA depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf) belirli bir noktaya geri yükleme sırasını sürede bölümünde gösterilmektedir **'aşağıdaki noktasının veritabanına geri'**. Belirli bir noktaya geri yükleme zamanında yürütülmesi için bu belgeleri kullanın. 


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [ilkeleri olağanüstü durum kurtarma ve hazırlık](hana-concept-preparation.md).
