---
title: HANA yedekleme ve geri yükleme (büyük örnekler) Azure üzerinde SAP HANA üzerinde | Microsoft Docs
description: HANA yedeklemesi gerçekleştirin ve SAP HANA (büyük örnekler) azure'da üzerinde geri yükleme
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
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e64c243e38c43c5eb543c3e2ec96d7cf8413cb9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709770"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

>[!IMPORTANT]
>Bu makalede bir ardılı yönetim belgelerine SAP HANA veya SAP notları değildir. Düz bir anlayış ve SAP HANA yönetim ve işlemler, yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için özellikle uzmanlığa sahip bekliyoruz. Bu makalede, ekran görüntüleri, SAP HANA Studio gösterilmektedir. SAP HANA'dan yayın için yayın içerik, yapısı ve SAP Yönetim Araçları ve araçları ekranlar yapısını değiştirebilirsiniz.

Adımlar ve ortamınızdaki ve HANA sürümleri ve sürümler ile yapılan işlemler alıştırma önemlidir. Bu makalede açıklanan bazı işlemleri daha iyi bir genel anlamak için basitleştirilmiştir. Bunlar için ayrıntılı adımları için son işlemi el kitapları kullanılması amaçlanmıştır değildir. İşlemi el kitapları yapılandırmalarınız için oluşturmak istiyorsanız, test ve işlemlerinizi çalışma ve belirli yapılandırmalarınız için ilgili işlemleri belgeleyin. 

En önemli yönlerinden işletim veritabanlarının bunları yıkıcı olaylara karşı korur sağlamaktır. Neden bu olayların her şey doğal felaketlere basit kullanıcı hataları olabilir.

Birisi önce silinen kritik veriler sağlayan olarak bir duruma geri yükleme gibi bir veritabanını yedekleme, zaman içinde herhangi bir noktasına geri yükleme özelliği ile önce kesinti sürümündekinden mümkün olduğunca kapatın.

İki tür yedekleme, geri yükleme yeteneği elde etmek için gerçekleştirilmelidir:

- Veritabanı Yedeklemeleri: Tam, artan ya da fark yedekleri
- İşlem günlüğü yedeklemeleri

Bir uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemeleri yanı sıra depolama anlık görüntüleri ile yedeklemelerini gerçekleştirebilir. Depolama anlık görüntüleri, işlem günlüğü yedeklemeleri değiştirin yok. İşlem günlüğü yedeklemeleri önemli veritabanını belirli bir noktaya geri yükleme ya da önceden kaydedilen işlem sayısı günlüklerinden boş kalır. Depolama anlık görüntüleri, veritabanının sarma görüntüsünü hızlı bir şekilde sağlayarak kurtarma hızlandırabilirsiniz. 

SAP HANA (büyük örnekler) azure'da iki yedekleme ve geri yükleme seçeneği sunar:

- **Bunu kendiniz (DIY).** Yeterli disk alanı olduğundan emin olduktan sonra aşağıdaki disk yedekleme yöntemlerden birini kullanarak tam bir veritabanı ve günlük yedekleri gerçekleştirin. Ya da doğrudan HANA büyük örneği birimleri veya bir Azure sanal makinesinde (VM) ayarlanan NFS paylaşımlarını bağlanmış birimler yedekleyebilirsiniz. İkinci durumda, müşterilerin azure'da bir Linux sanal makinesi ayarlama, Azure depolama VM'e ekleyin ve bu sanal makinede yapılandırılan NFS sunucusu üzerinden depolama paylaşmak. HANA büyük örneği birimlerine doğrudan ekleme birimler yedekleme yapıyorsanız, yedeklemelerin bir Azure depolama hesabına kopyalayın. Azure depolama alanına dayalı NFS paylaşımlarını dışarı aktaran bir Azure VM ayarladıktan sonra bunu yapabilirsiniz. Ayrıca, Azure Backup kasası veya Azure soğuk depolama da kullanabilirsiniz. 

   Başka bir seçenek, bir Azure depolama hesabına kopyalanana sonra yedeklemeleri depolamak için bir üçüncü taraf veri koruması aracı kullanmaktır. Kendin YAP yedekleme seçeneği ayrıca, uyumluluk ve denetleme amaçları için uzun süreler için depolamanız gereken veriler için gerekli olabilir. Her durumda, bir sanal makine ve Azure depolama ile temsil edilen NFS paylaşımlarını içine yedeklemeleri kopyalanır.

- **Altyapısını yedekleme ve geri yükleme işlevselliğinin.** Ayrıca Yedekleme ve geri yükleme işlevselliğinin, SAP hana (büyük örnekler) azure'da temel alınan altyapı sağlar. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümün geri kalanında, HANA büyük örnekleri ile sunulan yedekleme ve geri yükleme işlevselliği ele alır. Bu bölüm ayrıca yedekleme ve geri yükleme için olağanüstü durum olması ilişki kapsar kurtarma işlevselliğin sunduğu HANA büyük örnekleri.

> [!NOTE]
>   Altyapının HANA büyük örnekleri tarafından kullanılan anlık görüntü teknoloji, SAP HANA anlık görüntüleri bağımlılığı vardır. Bu noktada, SAP HANA anlık görüntüleri, SAP HANA çok kiracılı veritabanı kapsayıcıların birden fazla Kiracı ile birlikte çalışmaz. Yalnızca tek bir kiracı dağıtılır, SAP HANA anlık görüntüleri iş ve bu yöntemi kullanabilirsiniz.

## <a name="use-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>(Büyük örnekler) Azure üzerinde SAP HANA depolama anlık görüntüleri kullanma

SAP HANA (büyük örnekler) azure'da temel alınan depolama altyapısını birimlerin depolama anlık görüntüleri destekler. Hem yedekleme hem de birimlerin geri yüklenmesi desteklenir, ile aşağıdaki önemli noktalar:

- Tam veritabanı yedeklemeleri yerine, depolama birimi anlık görüntüleri sık kullanılan olarak alınır.
- Bir anlık görüntü üzerinde /hana/data tetiklenir ve /usr/sap, birimler, anlık görüntü teknolojisi içeren /hana/shared başlatan bir SAP HANA önce anlık görüntü depolama anlık görüntü çalıştırır. Bu SAP HANA anlık görüntü Kurulum nihai günlük geri yüklemeler için depolama anlık görüntüsünün kurtarma işleminden sonra noktasıdır. Bir HANA anlık başarılı olması etkin bir HANA örneği gerekir. HSR senaryosunda, depolama anlık bir HANA anlık görüntü burada gerçekleştirilemiyor geçerli bir ikincil düğüm üzerinde desteklenmiyor.
- Depolama anlık görüntüleri başarıyla çalıştıktan sonra SAP HANA anlık görüntüsü silinir.
- İşlem günlüğü yedeklemeleri sık gerçekleştirilen ve /hana/logbackups toplu veya azure'da depolanır. Ayrı olarak anlık görüntüsünü almak için işlem günlüğü yedeklemeleri içeren /hana/logbackups birimi tetikleyebilirsiniz. Bu durumda, bir HANA anlık görüntü çalıştırmanız gerekmez.
- Bir veritabanına belirli bir noktaya zamanında, bir üretim kesinti geri yüklemeniz gerekiyorsa, Microsoft Azure desteği veya SAP HANA Azure geri yükleme isteği için belirli depolama anlık görüntü. Bir planlı bir korumalı alan sistem geri yükleme özgün durumuna buna bir örnektir.
- Depolama anlık görüntüsüne dahil SAP HANA anlık görüntü çalıştırılmış ve depolama anlık görüntü alındıktan sonra depolanan işlem günlüğü yedeklemeleri uygulamak için uzaklık bir noktadır.
- Bu işlem günlüğü yedeklemeleri geri belirli bir noktaya veritabanını geri yönlendirilirsiniz.

Üç sınıf birimlerin hedef depolama anlık görüntüleri gerçekleştirebilirsiniz:

- / Hana/verileri ve/hana/paylaşılan üzerinde /usr/sap içeren birleştirilmiş bir anlık. Bu anlık görüntüyü bir SAP HANA anlık görüntü depolama anlık görüntüleri için hazırlık olarak oluşturulmasını gerektirir. SAP HANA anlık görüntü veritabanı bir depolama açısından tutarlı bir durumda olmasını sağlar. Geri yükleme işlemi için bir noktası üzerinde ayarlanmış olmasıdır.
- Ayrı bir anlık görüntü/hana/logbackups üzerinden.
- Bir işletim sistemi bölümü.

En son anlık görüntü komut dosyaları ve belgeleri almak için bkz: [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Anlık görüntü betik paketi indirme zaman [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0), üç dosyayı alın. Dosyalardan birinde bir PDF sağlanan işlevselliği için belgelenmiştir. Araç kümesinde indirdikten sonra "anlık görüntü araçları edinin." yönergeleri izleyin.

## <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü konuları

>[!NOTE]
>Depolama anlık görüntüleri, HANA büyük örneği birimlerine ayrılan depolama alanını kullanır. Depolama anlık görüntüleri ve kaç depolama anlık görüntüleri tutmak için zamanlama ilgili aşağıdaki noktaları göz önünde bulundurun. 

Belirli mekaniklerini (büyük örnekler) Azure üzerinde SAP HANA depolama anlık görüntüleri içerir:

- Belirli depolama anlık görüntüsünü noktasında ne zaman harcanan süre çok az depolama kullanır.
- Veri içerik değişikliklerini ve SAP HANA veri dosyaları depolama biriminde değiştirin içeriği özgün blok içeriği ve veri değişiklikleri depolamak anlık görüntü gerekir.
- Sonuç olarak, depolama anlık görüntü boyutunu artırır. Uzun süre anlık görüntü var, daha büyük depolama anlık görüntüsü olur.
- SAP HANA veritabanı birimin anlık görüntü depolama alanı tüketimi daha büyük bir depolama anlık ömrü boyunca yapılan değişiklik.

(Büyük örnekler) Azure üzerinde SAP HANA, SAP HANA veri ve günlük birimlerini için sabit bir birim boyutları ile birlikte gelir. Bu birimlerin anlık görüntüleri gerçekleştirme birim alanınızı eats. Şunları yapmanız gerekir:

- Depolama anlık görüntüleri zamanlamak ne zaman belirleyin.
- Depolama birimleri alanı kullanımını izleyin. 
- Depoladığınız anlık görüntü sayısını yönetin. 

Veri masses aktardığınızda veya diğer önemli değişiklikler HANA veritabanına gerçekleştirmek, depolama anlık görüntüleri devre dışı bırakabilirsiniz. 


Aşağıdaki bölümlerde, bu anlık görüntüler gerçekleştirmek için bilgileri sağlayın ve genel öneriler şunlardır:

- Birim başına 255 anlık görüntüleri donanım karşılayabilir olsa da, bu sayının altına iyi kalmasını istiyorsunuz. 250 veya daha az kullanılması önerilir.
- Depolama anlık görüntüleri gerçekleştirmeden önce izlemek ve boş alan izler.
- Boş alan bağlı depolama anlık görüntü sayısını azaltın. Sizi anlık görüntü sayısını azaltabilirsiniz veya birimleri genişletebilirsiniz. Ek depolama alanı 1 terabayt birimleri cinsinden sipariş edebilirsiniz.
- SAP platformu Geçiş Araçları (R3load) ile SAP HANA ile verileri taşıma veya SAP HANA veritabanlarını yedeklerden geri yükleme gibi etkinlikleri sırasında depolama anlık görüntüleri /hana/data birimde devre dışı bırakın. 
- Sırasında daha büyük düzenlemelere tablolarının SAP HANA depolama anlık görüntüleri mümkünse kaçının.
- Depolama anlık görüntüleri, olağanüstü durum kurtarma özellikleri SAP hana (büyük örnekler) Azure'da yararlanarak için bir önkoşuldur.

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Self Servis depolama anlık görüntüleri kullanma önkoşulları

Anlık görüntü betiği başarıyla çalıştığından emin olmak için Perl Linux işletim sisteminde HANA büyük örnekleri sunucuda yüklü olduğundan emin olun. Perl, HANA büyük örneği biriminde önceden yüklenmiş olarak gelir. Perl sürümü denetlemek için aşağıdaki komutu kullanın:

`perl -v`

![Ortak anahtarı, bu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Depolama anlık görüntüleri ayarlama

Depolama anlık görüntüleri HANA büyük örnekleri ile ayarlamak için aşağıdaki adımları izleyin.
1. Perl HANA büyük örnekleri sunucuda Linux işletim sisteminde yüklü olduğundan emin olun.
1. Değiştirme/etc/ssh/ssh\_satır eklemek için yapılandırma _Mac hmac-sha1_.
1. Varsa çalıştırdığınız her bir SAP HANA örneği için ana düğüm üzerinde SAP HANA yedekleme kullanıcı hesabı oluşturun.
1. SAP HANA HDB istemci tüm SAP HANA büyük örnekleri sunucularına yükleyin.
1. İlk SAP HANA büyük örnekleri sunucusunda, her bölgenin anlık görüntü oluşturmayı denetleyen temel alınan depolama altyapıya erişim için ortak bir anahtar oluşturun.
1. Betikler ve yapılandırma dosyasından kopyalayın [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0) konumunu **hdbsql** SAP HANA yükleme.
1. Değiştirme *HANABackupDetails.txt* için uygun müşteriyi belirtimleri gerektiğinde dosya.

En son anlık görüntü betikleri ve belgelerinden almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Daha önce listelenen adımları için bkz. [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

### <a name="consideration-for-mcod-scenarios"></a>MCOD senaryoları için önemli noktalar
Çalıştırırsanız bir [MCOD senaryo](https://launchpad.support.sap.com/#/notes/1681092) bir HANA büyük örneği biriminde birden çok SAP HANA örnekleri ile sağlanan her bir SAP HANA örnekleri için ayrı depolama birimi vardır. MDC ve diğer önemli noktalar hakkında daha fazla bilgi için bkz: "Anımsanması gereken önemli noktalar" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>1\. adım: SAP HANA HDB istemcisini yükleme

Linux işletim sistemi yüklü (büyük örnekler) Azure üzerinde SAP HANA, SAP HANA depolama anlık görüntüleri yedekleme ve olağanüstü durum kurtarma amacıyla çalıştırmak için gereken komut dosyaları ve klasörleri içerir. Daha yeni sürümlerde denetle [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Komut en son sürümü 4.0 ' dir. Farklı komut dosyaları, aynı ana sürümüne ait içinde farklı alt sürümleri olabilir.

SAP HANA'yı yüklerken HANA büyük örneği birimlerine göre SAP HANA HDB istemciyi yüklemek için sizin sorumluluğunuzdur.

### <a name="step-2-change-the-etcsshsshconfig"></a>2\. adım: Değiştirme/etc/ssh/ssh\_yapılandırma

Bu adım "Depolama ile iletişimi etkinleştir" içinde açıklanan [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-3-create-a-public-key"></a>3\. adım: Bir ortak anahtar oluşturma

HANA büyük örneği kiracınızın depolama anlık görüntü arabirimleri erişimi etkinleştirmek için bir ortak anahtar ile oturum açma yordamı oluşturun. 

Kiracınızda Azure (büyük örnekler) sunucusundaki ilk SAP HANA üzerinde depolama altyapıya erişim için ortak bir anahtar oluşturun. Ortak anahtara sahip depolama anlık görüntü arabirimleri oturum açmak için parola gerekli değildir. Parola kimlik genel anahtarıyla korumak gerekmez. 

Bir ortak anahtar oluşturmak için "Depolama ile iletişimi etkinleştir" bkz [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


### <a name="step-4-create-an-sap-hana-user-account"></a>4\. Adım: Bir SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntü oluşturmaya başlamak için depolama anlık görüntü betikleri kullanabileceğiniz SAP HANA'da bir kullanıcı hesabı oluşturun. Bu amaç için SAP HANA Studio içinden bir SAP HANA kullanıcı hesabı oluşturun. Kullanıcı altında SYSTEMDB oluşturulmalıdır ve *değil* MDC için SID veritabanı altında. Tek kapsayıcı ortamında kullanıcı Kiracı veritabanında oluşturulur. Bu hesabın olmalıdır **yedekleme yönetici** ve **kataloğu okuma** ayrıcalıkları. 

Ayarlanmış ve bir kullanıcı hesabı kullanmak için "SAP HANA ile iletişimi etkinleştir" bkz [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0).


### <a name="step-5-authorize-the-sap-hana-user-account"></a>5\. Adım: SAP HANA kullanıcı hesabı yetki

Bu adımda, oluşturduğunuz ve böylece betiklerin çalışma zamanında parolalar göndermek gerekmez SAP HANA kullanıcı hesabı yetkilendirin. SAP HANA komut `hdbuserstore` bir SAP HANA kullanıcı anahtarı oluşturulmasını sağlar. Anahtar, bir veya daha fazla SAP HANA düğümlerinde depolanır. Kullanıcı anahtarı kullanıcı SAP HANA komut dosyası süreci içinde parolalarını yönetmenize gerek kalmadan erişebilir. Betik oluşturma işlemi, bu makalenin sonraki bölümlerinde ele alınmıştır.

>[!IMPORTANT]
>Anlık görüntü komutları çalıştırmak aynı kullanıcı bağlamını içeren bu yapılandırma komutlarını çalıştırın. Aksi takdirde, anlık görüntü komutları düzgün çalışmaz.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>6\. Adım: Anlık görüntü betiklerini alma, anlık görüntüleri yapılandırma ve test yapılandırması ve bağlantı

Betiklerin en son sürümünü indirin [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.0). Komut dosyalarının yüklenme şeklini betikleri 4.0 sürümü ile değiştirildi. Daha fazla bilgi için bkz: "SAP HANA ile iletişimi etkinleştir" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Komutları için kullanılacak tam sırasını "(varsayılan) anlık görüntü araçların kolay yükleme" konusuna bakın. [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). Varsayılan yükleme kullanılmasını öneririz. 

Sürümünden yükseltme 3.x-4.0, bkz: "Var olan yüklemeyi yükseltmek" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 4\.0 araç kümesinde kaldırmak için "Anlık görüntü Araçları'nın kaldırılmasını" bkz [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

"Tam kurulumunda, anlık görüntü Araçları" açıklanan adımları çalıştırmak de unutmayın [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Yüklü olarak amacı farklı betikleri ve dosyaları, "Bu anlık görüntü araçları nedir" açıklanmıştır içinde [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Anlık görüntü araçları yapılandırmadan önce ayrıca HANA yedekleme konum ve ayarlar yapılandırılmış emin olun. doğru. Daha fazla bilgi için bkz: "SAP HANA yapılandırması" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Anlık görüntü araç kümesi yapılandırması "Yapılandırma dosyasında - HANABackupCustomerDetails.txt" bölümünde açıklanan [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-connectivity-with-sap-hana"></a>SAP HANA ile bağlantısını test etme

Tüm yapılandırma verilerini içine yerleştirdiğiniz sonra *HANABackupCustomerDetails.txt* dosya, yapılandırmaları için HANA örneği verileri doğru olup olmadığını denetleyin. Betiği kullanmak `testHANAConnection`, bir SAP HANA ölçek büyütme veya ölçek genişletme yapılandırmasını bağımsız olduğu.

Daha fazla bilgi için bkz: "SAP HANA - testHANAConnection ile bağlantısını denetleyin" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

#### <a name="test-storage-connectivity"></a>Depolama bağlantısını test etme

Sonraki adım yerleştirmiş verileri temel alan depolama bağlantısı denetlemektir *HANABackupCustomerDetails.txt* yapılandırma dosyası. Daha sonra test anlık görüntüsünü çalıştırın. Çalıştırmadan önce `azure_hana_backup` komutu, bu test çalıştırması gerekir. "Depolama - testStorageSnapshotConnection ile bağlantısını denetleyin" Bu test için komutları dizisi için bkz. "içinde [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Sonra bir başarılı oturum açma depolama sanal makine arabirimleri, betik, Aşama 2 ile devam eder ve test anlık görüntüsünü oluşturur. Çıktı, SAP HANA için bir üç düğümlü genişleme yapılandırma burada gösterilmiştir.

Test anlık görüntü komut dosyasını başarıyla çalışırsa, gerçek depolama anlık görüntüleri zamanlayabilirsiniz. Başarılı değilse, İleri taşımadan önce sorunları araştırın. Gerçek ilk anlık görüntülerin, bunlar tamamlanana kadar geçici olarak test anlık görüntü kalmalıdır.


### <a name="step-7-perform-snapshots"></a>7\. Adım: Anlık görüntüleri

Hazırlık adımlarını tamamladığınızda, yapılandırın ve planlayın gerçek depolama anlık görüntüleri başlayabilirsiniz. Zamanlanmış komut dosyası, SAP HANA ölçek büyütme ve ölçek genişletme yapılandırmaları ile çalışır. Düzenli ve normal yedekleme betik yürütme işlemi için cron yardımcı programını kullanarak betiği zamanlayın. 

Tam komut sözdizimi ve işlevselliği için "Gerçekleştirme anlık görüntü yedekleme - azure_hana_backup" bölümüne bakın [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Zaman betik `azure_hana_backup` çalıştığında, aşağıdaki üç aşamaya anlık görüntü depolama oluşturur:

1. Bu işlem, bir SAP HANA anlık görüntü çalışır.
1. Bu işlem, depolama anlık görüntüleri çalışır.
1. Depolama anlık görüntüleri çalıştırılmadan önce oluşturduğunuz SAP HANA anlık görüntü kaldırılır.

Betiği çalıştırmak için kopyalanmış olması HDB yürütülebilir klasöründen çağırın. 

Saklama dönemi komut dosyasını çalıştırdığınızda, bir parametre olarak gönderilen bir anlık görüntü sayısı ile yönetilir. Depolama anlık görüntüleri tarafından kapsanan süreyi, komut dosyası çalıştığında, parametre olarak gönderilen bir anlık görüntü sayısını ve yürütme süresi için kullanılan bir işlevdir. 

Tutulan anlık görüntü sayısını betiğin çağrısında bir parametre olarak adlandırılan sayıyı aşarsa, yeni bir anlık görüntü çalışmadan önce aynı etiketi en eski depolama anlık görüntüsünü silinir. Çağrının son parametre sayısı tutulduğu anlık görüntü sayısını denetlemek için kullanabileceğiniz olarak sayı size sunar. Bu sayı ile de, dolaylı olarak, anlık görüntüler için kullanılan disk alanını denetleyebilirsiniz. 


## <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türleri için anlık görüntü sıklığı, HANA büyük örneği olağanüstü durum kurtarma işlevlerini kullanmasına bağlı olarak değişir. Bu işlev özel öneriler depolama anlık görüntü sıklığı ve yürütme süreleri için yapmanızı şart koşabileceği depolama anlık görüntüleri kullanır. 

Önemli noktalar ve öneriler izleyin yapmanızı varsayılır *değil* HANA büyük örnekleri sağlayan olağanüstü durum kurtarma işlevlerini kullanın. Bunun yerine, son 30 güne ait kurtarma noktasını zamanında sağlayabilir ve yedeklemelere sahip depolama anlık görüntüleri kullanın. Anlık görüntüler ve alan sayısı sınırlamaları göz önünde bulundurulduğunda, aşağıdaki gereksinimleri göz önünde bulundurun:

- Belirli bir noktaya kurtarma için kurtarma zamanı.
- Kullanılan alan.
- Kurtarma noktası ve kurtarma zamanı hedeflerine olası bir olağanüstü durum kurtarma için.
- HANA tam veritabanı yedeklemeleri diskleri karşı nihai yürütülmesi. Tam veritabanı yedeği her diskleri karşı veya **backint** arabirimi gerçekleştirilir, depolama anlık görüntüleri yürütülmesi başarısız oluyor. Tam veritabanı yedeklemeleri depolama anlık görüntüleri üzerinde çalıştırmayı planlıyorsanız, bu süre boyunca depolama anlık görüntüleri yürütülmesini devre dışı bırakıldığından emin olun.
- Anlık görüntü başına 250 ile sınırlı olan birim sayısı.

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

HANA büyük örnekleri olağanüstü durum kurtarma işlevlerini kullanmazsanız, anlık görüntü daha az sıklıkta dönemdir. Bu gibi durumlarda, birleşik anlık görüntüleri /hana/data ve /usr/sap, 12 saat veya 24 saatlik dönem içeren /hana/shared gerçekleştirin. Anlık görüntüleri için bir ay tutun. Aynı günlük yedekleme birimi anlık görüntüleri için geçerlidir. SAP HANA işlem günlüğü yedeklemeleri günlük yedekleme birimi karşı yürütülmesi için 15 dakikalık dönem 5 dakika içinde gerçekleşir.

Zamanlanmış depolama anlık görüntüleri cron kullanarak en iyi şekilde gerçekleştirilir. Tüm yedekleme ve olağanüstü durum kurtarma ihtiyaçlarınızı olarak aynı betiği kullanabilirsiniz. Komut dosyasını değiştirirseniz çeşitli eşleştirilecek girişleri İstenen yedekleme sürelerine. Bu anlık görüntüler tüm farklı cron yürütme zamanları bağlı olarak, zamanlanmış. Saatlik, her 12 saatlik, günlük veya haftalık olabilir. 

Aşağıdaki örnek, bir cron zamanlama içinde /etc/crontab gösterir:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
Önceki örnekte, bir saatlik birleşik anlık görüntü /hana/data ve /usr/sap, konumlarını içeren /hana/shared/SID içeren birimler kapsar. Bu tür bir anlık görüntü için daha hızlı bir-belirli bir noktaya kurtarma son iki gün içinde kullanın. Günlük bir anlık bu birimlerde yoktur. Bu nedenle, iki gün kapsamı saatlik anlık görüntüleri yanı sıra kapsamı dört haftasını tarafından günlük anlık görüntüler görüntülenerek sahip. Ayrıca, işlem günlüğü yedekleme birimi günlük yedek desteklenir. Bu yedeklemeler dört hafta boyunca tutulur. 

Crontab üçüncü satırında gördüğünüz gibi HANA işlem günlüğü yedeklemesi her 5 dakikada bir çalışacak şekilde zamanlanır. Depolama anlık görüntüleri çalıştıran farklı bir cron işlerinin başlangıç zamanları dağılır. Bu şekilde, anlık görüntüler aynı anda zaman belirli bir noktada çalıştırmayın. 

Aşağıdaki örnekte, saatlik olarak konumları /hana/data ve /usr/sap içeren /hana/shared/SID içeren birimlere kapsayan birleşik bir anlık görüntü gerçekleştirin. Bu anlık görüntüler'i iki gün boyunca tutun. İşlem günlüğü yedekleme birimlerin anlık görüntüleri 5 dakikalık olarak çalıştırın ve dört saat boyunca tutulur. Olarak önce HANA işlem günlüğü dosyasının yedeğini 5 dakikada bir çalışacak şekilde zamanlanır. 

İşlem günlüğü yedeklemesi başlatıldıktan sonra işlem günlüğü yedekleme birimin anlık görüntüsünü 2 dakikalık bir gecikmeyle gerçekleştirilir. SAP HANA işlem günlüğü yedeklemesi, normal koşullar altında bu 2 dakika içinde tamamlanır. Olarak önce önyükleme içeren birimi LUN günde bir kez yedekleme depolama anlık görüntü tarafından desteklenir ve dört hafta boyunca tutulur.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

Aşağıdaki grafikte, önceki örnekte dizileri gösterilmektedir. LUN önyükleme çıkarılır.

![Yedekleme ve anlık görüntü arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanına yapılan değişiklikleri belgelemek için /hana/log birime yönelik normal yazma gerçekleştirir. Düzenli olarak, SAP HANA /hana/data birime bir kayıt noktası yazar. Bir SAP HANA işlem günlüğü yedeklemesi crontab içinde olarak belirtilen, her 5 dakikada bir çalışır. 

Ayrıca bir SAP HANA anlık görüntü /hana/data ve /hana/shared/SID birimler üzerinde anlık görüntü birleştirilmiş bir depolama tetikleme sonucunda saatte çalıştığını görürsünüz. HANA anlık görüntünün başarılı olduktan sonra çalıştırmaları birleştirilmiş depolama anlık görüntüsünü alın. Crontab belirtildiği şekilde /hana/logbackup birimin depolama anlık görüntüye her 5 dakika, yaklaşık 2 dakika sonra HANA işlem günlüğü yedeklemesi çalıştırır.

> 

>[!IMPORTANT]
> Anlık görüntüleri SAP HANA işlem günlüğü yedeklemeleri ile birlikte gerçekleştirildiğinde depolama anlık görüntüleri kullanımını SAP HANA yedeklemeler için değerlidir. Depolama anlık görüntü arasındaki zaman aralıklarını karşılamak bu işlem günlüğü yedeklemeleri gerekir. 

Kullanıcılara bir taahhüt 30 günlük bir zaman içinde nokta kurtarma ayarladıysanız, için gerekir:

- Birleştirilmiş depolama anlık görüntü /hana/data ve 30 gün önce yapılmışsa olağanüstü durumlarda /hana/shared/SID üzerinden erişebilirsiniz. 
- Herhangi bir birleştirilmiş depolama anlık görüntüleri arasında kapsamak bitişik işlem günlüğü yedeklemeleri vardır. Bu nedenle, işlem günlüğü yedekleme biriminin eski anlık görüntü 30 günden daha eski olması gerekir. Azure depolama alanında bulunan başka bir NFS paylaşımına işlem günlüğü yedeklemeleri kopyalarsanız, bu durum geçerli değildir. Bu durumda, eski işlem günlüğü yedeklemeleri, NFS paylaşımından çekme.

Depolama anlık görüntüleri ve işlem günlüğü yedeklemeleri nihai depolama çoğaltması avantajlarından yararlanarak, işlem günlüğü yedeklemeleri SAP HANA yazacağı konum değiştirin. Bu değişiklik, HANA Studio içinde yapabilirsiniz. 

SAP HANA tam günlük segmentleri otomatik olarak yedekler olsa da, belirlenimci olarak günlüğü yedekleme aralığı belirtin. Bu, genellikle günlük yedeklerinin belirleyici bir nokta ile çalıştırmak istediğiniz için olağanüstü durum kurtarma seçeneğini kullandığınızda özellikle doğrudur. Aşağıdaki örnekte, 15 dakikada bir günlüğü yedekleme aralığı ayarlanır.

![SAP HANA SAP HANA Studio yedekleme günlük zamanlama](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ayrıca, her 15 dakikadan daha sık yedeklemeler de seçebilirsiniz. Daha sık bir ayar, genellikle olağanüstü durum kurtarma işlevleri HANA büyük örnekleri ile birlikte kullanılır. Bazı müşteriler, işlem günlüğü yedeklemeleri her 5 dakikada gerçekleştirin.

Veritabanı hiçbir zaman yedeklendiğinden, son yedekleme kataloğunu içinde bulunması gereken tek bir yedekleme girişi oluşturmak için bir dosya tabanlı veritabanı yedeklemesi gerçekleştirmeyi adımdır. Aksi takdirde, SAP HANA, belirtilen günlük yedeklemelerinizi başlatamazsınız.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı bir yedek alın](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


İlk başarılı depolama anlık görüntüleri çalıştırdıktan sonra 6. adımda çalıştırdığınız test anlık görüntüyü silin. Daha fazla bilgi için bkz: "Remove test anlık - removeTestStorageSnapshot" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 


### <a name="monitor-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Sayısı ve disk birimi anlık görüntü boyutunu izleyin

Belirli depolama biriminde, anlık görüntüler ve bu anlık görüntü depolama tüketimini sayısını izleyebilirsiniz. `ls` Değildir komut Göster anlık görüntü dizin veya dosya. Linux işletim sistemi komut `du` aynı birimlere depolandıkları olduğundan bu depolama anlık görüntüleri hakkında ayrıntılı bilgiler gösterilir. Komutu aşağıdaki seçenekleri kullanın:

- `du –sh .snapshot`: Bu seçenek, toplam anlık görüntü dizini içindeki tüm anlık görüntüleri sağlar.
- `du –sh --max-depth=1`: Bu seçenek kaydedilen tüm anlık görüntüleri listeler **.snapshot** klasörü ve her anlık görüntü boyutu.
- `du –hc`: Bu seçenek, tüm anlık görüntüler görüntülenerek kullanılan toplam boyutu sağlar.

Anlık görüntü alınır ve depolanan tüm depolama birimlerindeki kullanmayan emin olmak için şu komutları kullanın.

>[!NOTE]
>Anlık görüntüleri LUN kimliğini önceki komutlara görünmez.

### <a name="get-details-of-snapshots"></a>Anlık görüntü ayrıntılarını Al
Anlık görüntüleri hakkında daha fazla bilgi almak için komut dosyasını kullanın `azure_hana_snapshot_details`. Olağanüstü durum kurtarma konumunuz etkin bir sunucu varsa, bu betik iki konumdan birinde çalıştırabilirsiniz. Anlık görüntüleri içeren her birime göre ayrılmış aşağıdaki çıktı, komut dosyası sağlar: 
   * Bir birim toplam anlık görüntü boyutu
   * Her birim anlık aşağıdaki ayrıntıları: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme varsa bu anlık görüntü ile ilişkili kimliği

Çıktıları ve komut söz dizimi için bkz: "Anlık görüntülerini listeleme - azure_hana_snapshot_details" [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 



### <a name="reduce-the-number-of-snapshots-on-a-server"></a>Bir sunucu üzerinde anlık görüntü sayısını azaltın

Daha önce anlatıldığı olarak, belirli etiketleri, depolamanın anlık görüntü sayısını azaltabilirsiniz. Son iki anlık görüntü başlatmak için komut etiket ve korumak istediğiniz anlık görüntü sayısını parametrelerdir.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

Önceki örnekte, anlık görüntü etikettir **dailyhana**. Anlık görüntüleri tutmak bu etikete sahip sayısı **28**. Disk alanı tüketimini için yanıt olarak depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Son parametre kümesine sahip betiği çalıştırmak için 15'e, örneğin, anlık görüntü sayısını azaltmak için kolay bir yol olan **15**:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Bu ayar ile betik çalıştırırsanız, yeni depolama anlık görüntüleri içeren, anlık görüntü, sayısını 15'tir. 15 en son anlık görüntüleri korunur ve 15 eski anlık görüntüleri silinir.

 >[!NOTE]
 > Bu betik, birden fazla bir saat öncesine anlık görüntüler varsa anlık görüntü sayısını azaltır. Komut dosyasını kısa bir saat öncesine aittir anlık görüntüleri silmez. Bu kısıtlamalar, sunulan isteğe bağlı bir olağanüstü durum kurtarma işlevselliği ilgilidir.

Anlık görüntüleri yedekleme ön ekine sahip bir dizi korumak istiyorsanız, **dailyhana** söz dizimi örneklerde, komut dosyasını Çalıştır **0** bekletme sayı olarak. Ardından, bu etiketle eşleşen tüm anlık görüntüler kaldırılır. Tüm anlık görüntüleri kaldırma, HANA büyük örnekleri olağanüstü durum kurtarma işlevi yeteneklerini etkileyebilir.

Betiği kullanmak için belirli anlık görüntüleri silmek için ikinci bir seçenek olan `azure_hana_snapshot_delete`. Bu betik bir anlık görüntü veya anlık görüntüleri bulunan HANA Studio ya da anlık görüntü adı olarak HANA yedekleme kimliği kullanarak ya da kümesini silmek için tasarlanmıştır. Şu anda, yedekleme kimliği yalnızca için oluşturulan anlık görüntüleri bağlıdır **hana** anlık görüntü türü. Anlık görüntü türü yedeklerini **günlükleri** ve **önyükleme** bu anlık görüntüler için bulunması için hiçbir yedekleme kimliği olduğundan anlık görüntü, bir SAP HANA gerçekleştirme. Anlık görüntü adı girildiğinde, tüm anlık görüntüler farklı birimlerde girilen anlık görüntü adı ile eşleşmesi için arar. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

İçinde "Anlık görüntü - azure_hana_snapshot_delete Sil" komut dosyası hakkında daha fazla bilgi için bkz [Microsoft azure'da SAP HANA için araçları snapshot](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).

Kullanıcı olarak komut dosyasını çalıştırmak **kök**.

>[!IMPORTANT]
>Yalnızca üzerinde anlık görüntü mevcut veriler varsa silmek anlık görüntüsü silindikten sonra veriler sonsuza dek kaybolur emin planlayın.


## <a name="file-level-restore-from-a-storage-snapshot"></a>Depolama anlık görüntüden dosya düzeyinde geri yükleme

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
Anlık Görüntü türleri için **hana** ve **günlükleri**, anlık görüntüleri birimler üzerinde doğrudan erişebileceğiniz **.snapshot** dizin. Her anlık görüntü için bir alt yoktur. Anlık görüntüden gerçek dizin yapısında, alt noktasında olduğu durumda her dosyasını kopyalayın. 

Betik'ın geçerli sürümünde yoktur *hiçbir* Self Servis olarak anlık görüntü geri yükleme için sağlanan komut dosyasını geri yükleyin. Anlık görüntü geri yükleme, olağanüstü durum kurtarma sitesinde Self Servis olağanüstü durum kurtarma betiklerini bir parçası olarak yük devretme sırasında gerçekleştirilebilir. Var olan kullanılabilir anlık görüntülerden istenen anlık görüntü geri yüklemek için bir hizmet isteği açarak Microsoft Operasyon ekibinin başvurmanız gerekir.

>[!NOTE]
>Tek dosya geri yükleme LUN HANA büyük örneği birimlerinin türünü bağımsız önyükleme anlık görüntüler için çalışmaz. **.Snapshot** dizin önyükleme LUN açık değil. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüye Kurtar

Bir üretim aşağı senaryosunda, Microsoft Azure desteği ile bir müşteri olayı olarak depolama anlık görüntüden kurtarma işlemi başlatılabilir. Verileri bir üretim sisteminde silindi ve üretim veritabanını geri yüklemek için onu almak tek yolu ise yüksek aciliyet sağlasa da konusudur.

Farklı bir durumda-belirli bir noktaya kurtarma düşük aciliyet olabilir ve gün önceden planlanmış. Yüksek öncelikli bayrağı oluşturma yerine, Azure üzerinde SAP HANA ile bu kurtarma planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılımının yükseltmeyi planladığınız. Ardından geliştirme paket yükseltmeden önce durumunu temsil eden bir anlık görüntüye geri gerekir.

İstek göndermeden önce hazırlamanız gerekir. Azure ekibi üzerinde SAP HANA isteği ve geri yüklenen birimlere sağlayın. Daha sonra anlık görüntülerine dayalı HANA veritabanını geri yükleyin.

Yeni araç kümesiyle geri anlık görüntüsünü almak için olasılık görebileceği için "bir anlık görüntü geri yükleme" [el ile Kurtarma Kılavuzu SAP HANA için'Azure depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

İstek için hazırlamak için aşağıdaki adımları izleyin.

1. Anlık görüntüyü geri yüklemek için karar verin. Aksi takdirde toplamasını sürece yalnızca hana/veri hacmi geri yüklenir. 

1. HANA örneğini kapatın.

   ![HANA örneğini kapatma](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Veri birimleri üzerinde her HANA veritabanı düğümü çıkarın. Veri birimleri için işletim sistemi bağlıysa, anlık görüntü geri yükleme başarısız olur.

   ![Veri birimleri üzerinde her HANA veritabanı düğümü çıkarın](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Azure destek isteği açın ve belirli bir anlık görüntü geri yükleme hakkında yönergeler içerir:

   - Geri yükleme işlemi sırasında: Azure hizmeti üzerinde SAP HANA koordine, doğrulayın ve doğru depolama anlık görüntünün geri yüklendiğini doğrulamak için bir konferans katılım için isteyebilir. 

   - Geri yükleme sonrasında: Azure hizmeti üzerinde SAP HANA depolama anlık görüntü geri olduğunda size bildirir.

1. Geri yükleme işlemi tamamlandıktan sonra tüm veri miktarları yeniden bağlayın.

   ![Tüm veri birimleri yeniden bağlayın](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Adım 7'de, örneğin, depolama anlık görüntüden, kurtarılır SAP HANA veri dosyalarını almak için başka bir olasılık belgelenen [el ile Kurtarma Kılavuzu SAP HANA için'Azure depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf).

Anlık görüntü yedeğinden geri yüklemek için bkz: [el ile Kurtarma Kılavuzu SAP HANA için'Azure depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 

>[!Note]
>Anlık Microsoft işlemleri tarafından geri yüklendiyse, 7. adım gerekmez.


### <a name="recover-to-another-point-in-time"></a>Başka bir noktasına geri
Belirli bir noktaya geri yükleme için "Aşağıdaki noktasının veritabanına kurtarma zamanında" bölümüne bakın [el ile Kurtarma Kılavuzu SAP HANA için'Azure depolama anlık görüntüden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/guides/Manual%20recovery%20of%20snapshot%20with%20HANA%20Studio.pdf). 


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [olağanüstü durum kurtarma ilkeleri ve hazırlık](hana-concept-preparation.md).
