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
ms.date: 04/01/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 69417551c1c8d410f75e74a8164c8b8a223ab835
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58805338"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

>[!IMPORTANT]
>Bu belge, yönetim belgeleri SAP HANA veya SAP notları hiçbir yerini almaktadır. Okuyucu düz bir anlayış ve SAP HANA yönetimi ve işlemleri, özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları uzmanlığa sahip beklenmektedir. Bu belgede, SAP HANA Studio ekran görüntüleri gösterilmektedir. SAP HANA'dan yayın için yayın içerik, yapısı ve SAP Yönetim Araçları ve araçları ekranlar yapısını değiştirebilirsiniz.

Adımlar ve ortamınızdaki ve HANA sürümleri ve sürümler ile yapılan işlemler alıştırma önemlidir. Bu belgede açıklanan bazı işlemler, daha iyi bir genel anlamak için Basitleştirilmiş ve nihai işlemi el kitapları için ayrıntılı adımları olarak kullanılmak üzere tasarlanmamıştır. İşlemi el kitapları yapılandırmalarınız için oluşturmak istiyorsanız, test ve işlemlerinizi çalışma ve belirli yapılandırmalarınız için ilgili işlemleri belge gerekir. 

İşletim veritabanları için en önemli yönlerinden bunları yıkıcı olaylara karşı korur sağlamaktır. Neden bu olayların her şey doğal felaketlere basit kullanıcı hataları olabilir.

Bir veritabanını, zamanda herhangi bir noktasına geri yükleme olanağıyla yedekleme (gibi birisi kritik verileri silinmeden önceki), mümkün olduğunca yakın önce kesinti olduğu şekilde bir duruma geri yükleme sağlar.

İki tür yedeklemeleri en iyi sonuçlar için gerçekleştirilmesi gerekir:

- Veritabanı Yedeklemeleri: Tam, artan ya da fark yedekleri
- İşlem günlüğü yedeklemeleri

Bir uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemeleri yanı sıra depolama anlık görüntüleri ile yedeklemelerini gerçekleştirebilir. Depolama anlık görüntüleri, işlem günlüğü yedeklemeleri değiştirmeyin. İşlem günlüğü yedeklemeleri önemli veritabanını belirli bir noktaya geri yükleme ya da önceden kaydedilen işlem sayısı günlüklerinden boş kalır. Ancak, depolama anlık görüntüleri, veritabanının sarma görüntüsünü hızlı bir şekilde sağlayarak kurtarma hızlandırabilirsiniz. 

SAP HANA (büyük örnekler) azure'da iki yedekleme ve geri yükleme seçeneği sunar:

- Yazılanları (DIY). Yeterli disk alanı olduğundan emin olmak için hesaplamak sonra aşağıdaki disk yedekleme yöntemlerden birini kullanarak tam bir veritabanı ve günlük yedekleri gerçekleştirin. Ya da doğrudan HANA büyük örneği birimlerine veya ağ dosya paylaşımları'için (NFS), bir Azure sanal makinesinde (VM) ayarlama bağlı birimleri yedekleyebilirsiniz. İkinci durumda, müşterilerin azure'da bir Linux sanal makinesi ayarlama, Azure depolama VM'e ekleyin ve bu sanal makinede yapılandırılan NFS sunucusu üzerinden depolama paylaşmak. HANA büyük örneği birimlerine doğrudan ekleme birimler yedekleme yapıyorsanız, (Azure depolama alanına dayalı NFS paylaşımlarını dışarı aktaran bir Azure VM ayarladıktan sonra), Azure depolama hesabınız için yedeklemeleri kopyalamanız gerekir. Ayrıca, Azure yedekleme kasası veya Azure soğuk depolama da kullanabilirsiniz. 

   Başka bir seçenek, bir Azure depolama hesabına kopyaladıktan sonra yedeklemeleri depolamak için üçüncü taraf veri koruma aracını kullanmaktır. Kendin YAP yedekleme seçeneği ayrıca, uyumluluk ve denetleme amaçları için uzun süreler için depolamanız gereken veriler için gerekli olabilir. Her durumda, bir sanal makine ve Azure depolama ile temsil edilen NFS paylaşımlarını içine yedeklemeleri kopyalanır.

- Altyapısını yedekleme ve geri yükleme işlevselliğinin. Yedekleme ve geri yükleme işlevselliğinin, SAP hana (büyük örnekler) azure'da temel alınan altyapı sağlar. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümün geri kalanında, HANA büyük örnekleri ile sunulan yedekleme ve geri yükleme işlevselliği ele alır. Bu bölüm ayrıca ilişki yedekleme kapsar ve geri yükleme sahip için olağanüstü durum kurtarma işlevselliğin sunduğu HANA büyük örnekleri.

> [!NOTE]
>   Altyapının HANA büyük örnekleri tarafından kullanılan anlık görüntü teknoloji, SAP HANA anlık görüntüleri bağımlılığı vardır. Bu noktada, SAP HANA anlık görüntüleri SAP HANA çok kiracılı veritabanı kapsayıcıların birden fazla Kiracı ile birlikte çalışmaz. Yalnızca tek bir kiracı dağıtılır, bu yöntem kullanılabilir ve SAP HANA anlık görüntüleri çalışır.

## <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>(Büyük örnekler) Azure üzerinde SAP hana depolama anlık görüntüleri kullanma

SAP HANA (büyük örnekler) azure'da temel alınan depolama altyapısını birimlerin depolama anlık görüntüleri destekler. Hem yedekleme hem de birimlerin geri yüklenmesi desteklenir, ile aşağıdaki önemli noktalar:

- Tam veritabanı yedeklemeleri yerine, depolama birimi anlık görüntüleri sık kullanılan olarak alınır.
- Anlık görüntü /hana/data ve /hana/shared (/usr/sap içerir) üzerinden tetiklerken birimler, anlık görüntü teknoloji bir SAP HANA depolama anlık görüntüleri yürütülmeden önce anlık görüntü başlatır. Bu SAP HANA anlık görüntü Kurulum nihai günlük geri yüklemeler için depolama anlık görüntüsünün kurtarma işleminden sonra noktasıdır. HANA anlık görüntü başarılı olması etkin bir HANA örneği gerekir.  HSR senaryosunda, depolama anlık görüntüleri HANA anlık görüntü burada gerçekleştirilemiyor geçerli ikincil düğüm üzerinde desteklenmiyor.
- Depolama anlık görüntüleri başarıyla yürütüldükten sonra SAP HANA anlık görüntüsü silinir.
- İşlem günlüğü yedeklemeleri sık alınır ve /hana/logbackups toplu ya da azure'da depolanır. Ayrı olarak anlık görüntüsünü almak için işlem günlüğü yedeklemeleri içeren /hana/logbackups birimi tetikleyebilirsiniz. Bu durumda, bir HANA anlık görüntüsü yürütme gerekmez.
- Bir veritabanını belirli bir noktaya geri gerekir, bu Microsoft Azure desteği (bir üretim kesinti) veya SAP HANA Azure geri yükleme isteği için belirli depolama anlık görüntü. Bir planlı bir korumalı alan sistem geri yükleme özgün durumuna buna bir örnektir.
- Depolama anlık görüntüsüne dahil SAP HANA anlık görüntü yürütülen ve depolama anlık görüntü alındıktan sonra depolanan işlem günlüğü yedeklemeleri uygulamak için uzaklık bir noktadır.
- Bu işlem günlüğü yedeklemeleri geri belirli bir noktaya veritabanını geri yönlendirilirsiniz.

Depolama anlık görüntüleri birimlerin üç sınıf hedefleyen gerçekleştirebilirsiniz:

- Birleşik bir anlık görüntü/hana/veri ve /hana/shared (/ usr/sap içerir). Bu anlık görüntüyü bir SAP HANA anlık görüntü depolama anlık görüntüleri için hazırlık olarak oluşturulmasını gerektirir. SAP HANA anlık görüntü veritabanı bir depolama açısından tutarlı bir durumda olduğundan emin olur. Ve geri yüklemek için diğer bir deyişle bir noktaya ayarlama işlemi üzerinde çalışır.
- Ayrı bir anlık görüntü/hana/logbackups üzerinden.
- Bir işletim sistemi bölümü.

En son anlık görüntü betikleri ve belgelerinden almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Anlık görüntü betik paketi indirme zaman [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts), komut dosyaları için PDF belgeleri betik paketinin bir parçası olarak ayrıca Al. Her betik paketi kendi PDF belgeleri vardır.

## <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü konuları

>[!NOTE]
>Depolama anlık görüntüleri, HANA büyük örneği birimlerine ayrılan depolama alanını kullanır. Depolama anlık görüntüleri ve kaç depolama anlık görüntüleri tutmak için zamanlama ilgili aşağıdaki noktaları göz önünde bulundurmanız gerekir. 

Belirli mekaniklerini (büyük örnekler) Azure üzerinde SAP HANA depolama anlık görüntüleri içerir:

- Belirli depolama anlık görüntü (noktada zaman alınır) az depolama kullanır.
- Veri içerik değişikliklerini ve SAP HANA veri dosyaları depolama biriminde değiştirin içeriği anlık görüntü veri değişikliklerini yanı sıra, özgün blok içeriği depolamak gerekir.
- Sonuç olarak, depolama anlık görüntü boyutunu artırır. Uzun süre anlık görüntü var, daha büyük depolama anlık görüntüsü olur.
- SAP HANA veritabanı birimin anlık görüntü depolama alanı tüketimi daha büyük bir depolama anlık ömrü boyunca yapılan değişiklik.

(Büyük örnekler) Azure üzerinde SAP HANA, SAP HANA veri ve günlük birimlerini için sabit bir birim boyutları ile birlikte gelir. Bu birimlerin anlık görüntüleri gerçekleştirme birim alanınızı eats. Depolama anlık görüntüleri zamanlamak ne zaman belirlemek gerekir. Ayrıca, depolamanın anlık görüntü sayısını yönetme yanı sıra depolama birimleri alanı tüketimini izlemek gerekir. Veri masses aktardığınızda veya diğer önemli değişiklikler HANA veritabanına gerçekleştirmek, depolama anlık görüntüleri devre dışı bırakabilirsiniz. 


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
1. Betikler ve yapılandırma dosyasından kopyalayın [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) konumunu **hdbsql** SAP HANA yükleme.
1. Değiştirme *HANABackupDetails.txt* için uygun müşteriyi belirtimleri gerektiğinde dosya.

En son anlık görüntü betikleri ve belgelerinden almak [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Anlık görüntü betik paketi indirme zaman [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts), komut dosyaları için PDF belgeleri betik paketinin bir parçası olarak ayrıca Al. Her betik paketi kendi PDF belgeleri vardır.

### <a name="consideration-for-mcod-scenarios"></a>MCOD senaryoları için önemli noktalar
Çalıştırıyorsanız bir [MCOD senaryo](https://launchpad.support.sap.com/#/notes/1681092) bir HANA büyük örneği biriminde birden çok SAP HANA örnekleri ile sağlanan her bir SAP HANA örnekleri için ayrı depolama birimi vardır. Self Servis anlık görüntü Otomasyonu geçerli sürümünde her HANA örneği sistemde kimliği (SID) ayrı anlık görüntüleri başlatamazsınız. İşlevi, denetimleri için sunucu yapılandırma dosyasında (Bu makalenin devamındaki bakın) kayıtlı SAP HANA örnekleri sunar ve eşzamanlı bir anlık görüntü birimi kayıtlı tüm örnekleri hacimdeki yürütür.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>1. Adım: SAP HANA HDB istemcisini yükleme

Linux işletim sistemi yüklü (büyük örnekler) Azure üzerinde SAP HANA, SAP HANA depolama anlık görüntüleri yedekleme ve olağanüstü durum kurtarma amacıyla yürütmek gereken komut dosyaları ve klasörleri içerir. Daha yeni sürümlerde denetle [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Komut en son sürümünü 3.x ' dir. Farklı komut dosyaları, aynı ana sürümüne ait içinde farklı alt sürümleri olabilir.

>[!IMPORTANT]
>Sürümünden 2.1 sürümüne taşırken 3.x betiklerinin yapısını yapılandırma dosyası ve bazı söz dizimleri değiştirildiğine dikkat edin. Belirli bölümlerde belirtme konusuna bakın. 

SAP HANA yüklerken HANA büyük örneği birimlerine göre SAP HANA HDB istemciyi yüklemek için sizin sorumluluğunuzdur.

### <a name="step-2-change-the-etcsshsshconfig"></a>2. Adım: Değiştirme/etc/ssh/ssh\_yapılandırma

Değişiklik `/etc/ssh/ssh_config` ekleyerek _Mac hmac-sha1_ satır burada gösterildiği gibi:
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

### <a name="step-3-create-a-public-key"></a>3. Adım: Bir ortak anahtar oluşturma

Depolama anlık görüntü arabirimler HANA büyük örneği kiracınızın erişimi etkinleştirmek için bir ortak anahtar ile oturum açma yordamı yapmanız gerekir. Kiracınızda Azure (büyük örnekler) sunucusundaki ilk SAP HANA üzerinde depolama altyapıya erişim için kullanılacak ortak bir anahtar oluşturun. Ortak anahtar, parola depolama anlık görüntü arabirimleri oturum açmak için gerekli değildir sağlar. Bir ortak anahtar oluşturma ayrıca parola kimlik bilgilerini korumak gerekmez anlamına gelir. Linux'ta SAP HANA büyük örnekleri sunucuda, ortak anahtarı oluşturmak için aşağıdaki komutu yürütün:
```
  ssh-keygen -t rsa –b 5120 -C ""
```

Yeni konum **_/root/.ssh/id\_rsa.pub**. Gerçek bir parola girmeyin, aksi takdirde oturumunuzu her defasında parolayı girmek için gereklidir. Bunun yerine, seçin **Enter** iki kez oturum açmak için "parolayı girmeniz" gereksinimini kaldırmak için.

Ortak anahtar klasörlere değiştirerek beklendiği gibi düzeltilmiştir emin **/root/.ssh/** ve ardından yürütmeyi `ls` komutu. Anahtarı varsa, aşağıdaki komutu çalıştırarak kopyalayabilirsiniz:

![Ortak anahtarı, bu komutu çalıştırarak kopyalanır.](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Bu noktada, Azure üzerinde SAP HANA başvurun ve ortak anahtar ile verin. Hizmet temsilcisi, HANA büyük örneği kiracınız için gerekmez temel alınan depolama altyapısındaki kaydetmek için ortak anahtarı kullanır.

### <a name="step-4-create-an-sap-hana-user-account"></a>4. Adım: Bir SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntüleri oluşturulmasını başlatmak için depolama anlık görüntü betikleri kullanabileceğiniz SAP HANA'da bir kullanıcı hesabı oluşturmanız gerekir. Bu amaç için SAP HANA Studio içinden bir SAP HANA kullanıcı hesabı oluşturun. Kullanıcı için MDC SYSTEMDB SID veritabanı altında değildir ve oluşturulmalıdır. Tek kapsayıcı ortamında, Kiracı veritabanı altında Kurulum kullanıcıdır. Bu hesap aşağıdaki ayrıcalıklara sahip olmalıdır: **Yedekleme Yönetim** ve **okuma katalog**. Bu örnekte, kullanıcı adı: **SCADMIN**. HANA Studio'da oluşturulan kullanıcı hesabı adı büyük/küçük harfe duyarlıdır. Seçtiğinizden emin olun **Hayır** , sonraki oturum açma parolasını değiştirmesine izin gerektirme.

![HANA Studio kullanıcı oluşturma](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

MCOD dağıtımları, bir birim üzerinde birden çok SAP HANA örnekleri ile kullanırsanız, her SAP HANA örneği için bu adımı yinelemeniz gerekir.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>5. Adım: SAP HANA kullanıcı hesabı yetki

Bu adımda, betiklerin çalışma zamanında parolalar göndermek gerekmez, sizin oluşturduğunuz, SAP HANA kullanıcı hesabı yetkilendirin. SAP HANA komut `hdbuserstore` bir veya daha fazla SAP HANA düğümde depolanan bir SAP HANA kullanıcı anahtarı oluşturulmasını sağlar. Kullanıcı anahtarı kullanıcı SAP HANA komut dosyası süreci içinde parolalarını yönetmenize gerek kalmadan erişebilir. Betik oluşturma işlemi, bu makalenin sonraki bölümlerinde ele alınmıştır.

>[!IMPORTANT]
>Yürütülecek komut planlanan kullanıcı altında aşağıdaki komutu çalıştırın. Aksi takdirde, komut dosyası düzgün çalışamaz.

Girin `hdbuserstore` komutuyla şu şekilde:

**MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**MDC HANA Kurulumu**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

Aşağıdaki örnekte, kullanıcının olduğu **SCADMIN01**, ana bilgisayar adı **lhanad01**, ve örneğini sayıdır **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Birden çok SAP HANA örnekleri bir birim üzerinde bulunan bir HANA MCOD dağıtım kullanırsanız, her SAP HANA örneği ve ilişkili yedekleme kullanıcı birimi için yineleyin gerekir.

Bir SAP HANA ölçeklendirme yapılandırmanız varsa, tek bir sunucudan tüm betik yönetmek gerekir. Bu örnekte, SAP HANA anahtar **SCADMIN01** her ana bilgisayarın hangi konak anahtarının ilişkili olduğunu gösterir şekilde değiştirilmesi gerekir. HANA veritabanı örneği sayısı ile SAP HANA yedekleme hesabı düzeltin. Anahtar, hangi atandıktan ve genişleme yapılandırmaları için Yedekleme kullanıcı tüm SAP HANA örnekleri erişim hakları olmalıdır konak üzerindeki yönetim ayrıcalıkları olmalıdır. Üç genişleme düğümü varsayılarak adlarına sahip **lhanad01**, **lhanad02**, ve **lhanad03**, bir dizi komut şöyle görünür:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>6. Adım: Anlık görüntü betiklerini alma, anlık görüntüleri yapılandırma ve test yapılandırması ve bağlantı

Betiklerin en son sürümünü indirin [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Çalışma dizini için indirilmiş betikler ve metin dosyasına kopyalayın **hdbsql**. Geçerli HANA yüklemeler için bu dizin şu biçimdedir: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Perl betikleri ile ilgilenirken: 

- Hiçbir zaman komut dosyalarını, Microsoft Operations tarafından belirtilmediği sürece değiştirin.
- Komut dosyası veya bir parametre dosyası değiştirmek isteyip istemediğiniz sorulduğunda "olduğu gibi vi" gibi Linux metin düzenleyicisi ve Not Defteri gibi bir Windows Düzenleyicisi değil her zaman kullanın. Bir Windows Düzenleyicisi'ni kullanarak dosya biçimi bozulmasına neden olabilir.
- Her zaman en son komut dosyalarını kullanın. Github'dan en son sürümünü indirebilirsiniz.
- Betikleri sürümüyle aynı sürümü arasında yatay kullanın.
- Test betikleri ve gerekli parametreleri ve betiğin çıktısını ile üretim sisteminde kullanmadan önce doğrudan deneyim kazanın.
- Microsoft Operations tarafından sağlanan sunucunun bağlama noktası adını değiştirmeyin. Bu standart bağlama noktaları, başarılı bir yürütme için kullanılabilir olması için bu betikleri dayanır.


Farklı betikleri ve dosyaları amacı aşağıdaki gibidir:

- **Azure\_hana\_backup.pl**: Bu betik Linux Cron zamanlama yardımcı programıyla depolama anlık görüntüleri HANA verileri ve paylaşılan birimler, /hana/logbackups birimin ya da işletim sistemi üzerinde yürütmek için zamanlanır.
- **Azure\_hana\_çoğaltma\_status.pl**: Bu betik, üretim site çoğaltma durumu olağanüstü durum kurtarma sitesine etrafında temel ayrıntıları sağlar. Çoğaltma gerçekleşen ve bu öğelerin boyutunu gösterir emin olmak için komut dosyası izleyicileri çoğaltılmıyor. Bir çoğaltma çok uzun sürüyorsa veya bağlantı kapalı ise ayrıca rehberlik sağlar.
- **azure\_hana\_snapshot\_details.pl**: Bu betik, ortamınızda mevcut tüm anlık görüntüler, birim başına hakkında temel ayrıntılar bir listesini sağlar. Bu betik, birincil sunucuda veya sunucu birim olağanüstü durum kurtarma konumunuz olarak çalıştırılabilir. Betik anlık görüntüleri içeren her bir birimi tarafından ayrılmış aşağıdaki bilgileri sağlar:
   * Bir birim toplam anlık görüntü boyutu
   * Her birim anlık aşağıdaki ayrıntıları: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme varsa bu anlık görüntü ile ilişkili kimliği
- **azure\_hana\_snapshot\_delete.pl**: Bu betik bir depolama anlık görüntü veya anlık görüntü kümesini siler. SAP HANA yedekleme kimliği HANA Studio bulunan veya depolama anlık görüntü adını kullanabilirsiniz. Şu anda, yedekleme kimliği yalnızca HANA veri/log/paylaşılan birimler için oluşturulan anlık görüntüleri bağlıdır. Aksi takdirde, girilen anlık görüntü kimliği eşleşen tüm anlık görüntüleri arayan anlık görüntü kimliği girilirse,  
- **testHANAConnection.pl**: Bu betik, SAP HANA örneği bağlantısını test eder ve depolama anlık görüntüleri için gerekli.
- **testStorageSnapshotConnection.pl**: Bu betik iki amacı vardır. İlk olarak, bu betikleri çalıştırır HANA büyük örneği birim erişim atanan depolama sanal makineye ve HANA büyük örnekleri depolama anlık görüntü arabirimine sahip olmasını sağlar. İkinci amacı, sınamakta olduğunuz HANA örneği için geçici bir anlık görüntü oluşturmaktır. Bu betik her HANA örneği için yedekleme betikleri beklendiği gibi çalışmasını sağlamak için bir sunucu üzerinde çalıştırmanız gerekir.
- **removeTestStorageSnapshot.pl**: Bu betik komut dosyasını oluşturduğunuz test anlık görüntüsü silinmektedir **testStorageSnapshotConnection.pl**.
- **azure\_hana\_dr\_failover.pl**: Bu betik, başka bir bölgeye DR yük devretme başlatır. Yük devretme istediğiniz birime veya DR bölgesinde HANA büyük örneği birim üzerinde yürütülecek betiğin gerekir. Bu betik, birincil taraftan depolama çoğaltması ikincil tarafa durdurur, DR birimlerde en son anlık görüntüyü geri yükler ve bağlama birimleri için DR sağlar.
- **azure\_hana\_test\_dr\_failover.pl**: Bu betik, DR sitesine yük devretme testi gerçekleştirir. Azure_hana_dr_failover.pl betik bu yürütme depolama çoğaltmayı birincil sunucudan ikincil kesintiye uğratmaz. Bunun yerine, çoğaltılan depolama birimleri klonlarını DR tarafındaki oluşturulur ve kopyalanan birim bağlama sağlanır. 
- **HANABackupCustomerDetails.txt**: Bu dosya, SAP HANA yapılandırmanızı uyum sağlamak için değiştirmeniz gerekir değiştirilebilir yapılandırma dosyasıdır. *HANABackupCustomerDetails.txt* dosya, depolama anlık görüntüleri çalışacak olan betiğe için Denetim ve yapılandırma dosyasıdır. Dosya amaçları ve kurulum için ayarlayın. Aldığınız **depolama yedekleme adı** ve **depolama IP adresi** örneklerinizi dağıtırken azure'da SAP HANA öğesinden. Sıralı sıralama ya da bu dosyadaki değişkenlerden herhangi birini aralığı değiştiremezsiniz. Bunu yaparsanız, komut dosyaları düzgün çalışmaz. Ayrıca, IP adresini ölçek artırma düğümünde veya ana düğüm (Ölçek genişletme varsa) Azure üzerinde SAP HANA alırsınız. Ayrıca SAP HANA'ın yüklenmesi sırasında size HANA örnek numarasını bilmeniz. Şimdi, bir yedekleme adı yapılandırma dosyasına eklemeniz gerekir.

HANA büyük örneği birim ve sunucu IP adresi sunucu adını doldurduktan sonra ölçek büyütme veya genişleme dağıtımı için yapılandırma dosyasını aşağıdaki örnekteki gibi görünür. Her SAP HANA yedeklemek veya kurtarmak istediğiniz SID için tüm gerekli alanları doldurun.

Ayrıca, gerekli bir alan önüne "#" ekleyerek bir süreliğine yedekleme istemediğiniz örneklerinin satırları açıklama. Tüm SAP HANA yedeklemek veya o belirli bir örneğine kurtarmak gerek yoksa, bir sunucuda bulunan örnekleri girmeniz gerekmez. Tüm alanları için biçim tutulması gerekir veya tüm betikleri throw bir hata iletisi ve betik sonlandırır. Son SAP HANA örneği kullanımda sonra kullanmadığınız tüm SID bilgi ayrıntıları ek gerekli satırlarını silebilirsiniz. Tüm satırları doldurulmuş, yorum veya silindi.

>[!IMPORTANT]
>Dosya yapısı değiştirildi sürüm 2.1 sürümündeki Git ile 3.x. 3.x sürümünü komut dosyalarını kullanmak istiyorsanız, yapılandırma dosya yapısı uyum gerekir. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

HANA büyük örneği biriminde yapılandırdığınız her örneği için veya yapılandırmayı genişletmek için verileri aşağıdaki şekilde tanımlamak gerekir:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
Bu yapılandırma her düğüm genişleme ve HANA sistem çoğaltması yapılandırmaları için yineleyin. Bu ölçü başarısız durumda, yedeklemeleri ve nihai depolama çoğaltma çalışmaya devam etmesini sağlar.   

Tüm yapılandırma verilerini içine yerleştirdiğiniz sonra *HANABackupCustomerDetails.txt* dosya, yapılandırmaları için HANA örneği verileri doğru olup olmadığını denetleyin. Betiği kullanmak `testHANAConnection.pl`, bir SAP HANA ölçek büyütme veya ölçek genişletme yapılandırmasını bağımsız olduğu.

```
testHANAConnection.pl
```

Bir SAP HANA ölçeklendirme yapılandırmanız varsa, ana HANA örneği gerekli HANA sunucuları ve örnekleri erişimi olduğundan emin olun. Test betiği için herhangi bir parametre yok, ancak verilerinizi eklemelisiniz *HANABackupCustomerDetails.txt* düzgün çalıştırılacak betik için yapılandırma dosyası. Betik hata denetlemek için her örnek mümkün değildir, dolayısıyla yalnızca kabuk komutu hata kodlarını döndürülür. Betik, bazı faydalı yorumlar, denetleyin Bu halde bile sağlar.

Betiği çalıştırmak için aşağıdaki komutu girin:
```
 ./testHANAConnection.pl
```
Betik başarıyla HANA örneği durumu alırsa, HANA bağlantısı başarılı bir ileti görüntüler.


Sonraki adım yerleştirmiş verileri temel alan depolama bağlantısı denetlemektir *HANABackupCustomerDetails.txt* yapılandırma dosyası ve bir test anlık görüntü yürütebilirsiniz. Yürütmeden önce `azure_hana_backup.pl` komut dosyası, bu test çalıştırması gerekir. Bir birim anlık görüntü yok içeriyorsa, birim boş olup olmadığını veya anlık görüntü ayrıntılarını almak için bir SSH hatası olursa belirlemek mümkün değildir. Bu nedenle, iki adımı betik yürütür:

- Kiracının sanal makine depolama ve arabirimleri anlık görüntüleri yürütmek komut dosyaları için erişilebilir olduğunu doğrular.
- Her birim için bir test veya dummy, anlık görüntü tarafından HANA örneği oluşturur.

Bu nedenle, HANA örneği bağımsız değişken olarak dahildir. Yürütme başarısız olursa hata için depolama bağlantı denetimini sağlamak mümkün değildir. Betik, olsa bile hata denetleme, faydalı ipucu sağlar.

1. Bu test gerçekleştirmek için komutları dizisini yürütün:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   Depolama kullanıcı adı hem de depolama IP adresi, HANA büyük örneği birim devreden MultiPath sırasında sağlanmıştır.

1. Test betiği çalıştırın:
   ```
    ./testStorageSnapshotConnection.pl
   ```

Önceki kurulum adımları ve yapılandırılmış verileri ile sağlanan ortak anahtar kullanarak depolama oturum açmak komut dosyası çalıştığında *HANABackupCustomerDetails.txt* dosya. Oturum açma başarılı olursa, aşağıdaki içeriği gösterilmektedir:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Depolama konsola bağlanmada sorunlar meydana gelirse, çıktı şuna benzer:

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

Sonra bir başarılı oturum açma depolama sanal makine arabirimleri, betik, Aşama 2 ile devam eder ve test anlık görüntüsünü oluşturur. Çıktı, SAP HANA için bir üç düğümlü genişleme yapılandırması aşağıda gösterilmiştir:

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

Test anlık görüntü ile betiği başarıyla yürütüldü, gerçek depolama anlık görüntüleri yapılandırmaya devam edebilirsiniz. Başarılı olmazsa, geçmeden önce sorunları araştırın. Gerçek ilk anlık görüntülerin, bunlar tamamlanana kadar geçici olarak test anlık görüntü kalmalıdır.


### <a name="step-7-perform-snapshots"></a>7. Adım: Anlık görüntüleri

Hazırlık adımlarını tamamladığınızda, depolamanın anlık görüntü yapılandırmasını başlayabilirsiniz. Zamanlanmış komut dosyası, SAP HANA ölçek büyütme ve ölçek genişletme yapılandırmaları ile çalışır. Düzenli ve normal yedekleme betik yürütme işlemi için cron yardımcı programını kullanarak betiği zamanlayın. 

Üç tür anlık görüntüsü yedekleri oluşturabilirsiniz:
- **HANA**: / Hana/veri içeren birimler ve hana paylaşılan (/usr/sap de içeren) / eşgüdümlü bir anlık görüntü tarafından kapsanan toplam anlık görüntü bir yedekleme. Tek dosya geri yükleme, bu anlık görüntüden mümkündür.
- **Günlükleri**: / Hana/logbackups birimin anlık görüntü yedekleme. HANA anlık görüntü yok, bu depolama anlık görüntüleri yürütmek için tetiklenir. Bu depolama birimi, SAP HANA işlem günlüğü yedeklemeleri içerecek şekilde tasarlanmıştır. Bu olası veri kaybını önlemeye ve günlük büyümesini sınırlamak için daha sık gerçekleştirilir. Tek dosya geri yükleme, bu anlık görüntüden mümkündür. 3 dakikadan sıklığını azaltın yok.
- **Önyükleme**: HANA büyük örneği önyükleme mantıksal birim numarası (LUN) içeren birim anlık görüntüsünü. Bu anlık görüntü yedekleme, yalnızca tür ı SKU'ları, HANA büyük örnekleri ile mümkündür. LUN önyükleme içeren birim anlık görüntüden geri yükleyen tek dosyalı gerçekleştirilemiyor.


>[!NOTE]
> Bu üç tür MCOD dağıtımlarını desteklemek sürüm 3.x kodları taşıma ile değiştirilen anlık görüntüleri için arama söz dizimi. Artık bir örneğinin HANA SID belirtmek için gerek yoktur. SAP HANA örnekleri birim yapılandırma dosyasında yapılandırıldığından emin olmak gereken *HANABackupCustomerDetails.txt*.

>[!NOTE]
> Komut dosyası ilk kez çalıştırdığınızda, çoklu SID ortama bazı beklenmeyen hatalar gösterebilir. Komut dosyasını yeniden çalıştırmak sorunu giderir.



Depolama anlık görüntüleri betiğiyle yürütmeye yönelik yeni çağrı sözdizimini *azure_hana_backup.pl* şöyle görünür:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Parametrelerin ayrıntıları aşağıdaki gibidir: 

- İlk parametre, anlık görüntü yedekleme türünü belirtir. İzin verilen değerler: **hana**, **günlükleri**, ve **önyükleme**. 
- Parametre  **\<HANA büyük örnek türü >** yalnızca önyükleme birimi yedeklemeler için gereklidir. HANA büyük örneği birime bağımlı iki geçerli değerler "TypeI" veya "TypeII" vardır. Birim türü çıkış bulmak için bkz [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- Parametre  **\<snapshot_prefix >** bir anlık görüntü veya anlık görüntü türü için yedekleme etiketi. İki amacı vardır: biridir sizin için bir ad vermek böylece bu anlık görüntüler hakkında olduğunu bilirsiniz. Betik için ikinci amaçtır *azure\_hana\_backup.pl* belirli bir etiket altında korunur depolama anlık görüntü sayısını belirlemek için. Aynı türdeki iki depolama anlık görüntüsü yedekleri, zamanlama (gibi **hana**), iki farklı etiketleri ve 30 anlık görüntüleri saklanır her biri için etkilenen birim 60 depolama anlık görüntülerle son tanımlayın. Yalnızca alfa sayısal ("A-Z, a-z, 0-9"), alt çizgi ("_") ve tire ("-") karakterlere izin verilir. 
- Parametre  **\<snapshot_frequency >** için gelecek gelişmeler ayrılmıştır ve herhangi bir etkisi yoktur. "3 dk" türü yedeklerini yürütülürken ayarlamak **günlük**ve "bir yedekleme türleri yürütülürken 15 dakika".
- Parametre  **\<anlık görüntüleri korunur sayısı >** aynı anlık görüntü önek (etiketi) ile anlık görüntü sayısını tanımlayarak anlık görüntü saklama dolaylı olarak tanımlar. Bu parametre, cron aracılığıyla zamanlanan yürütme için önemlidir. Anlık görüntü sayısı ile aynı snapshot_prefix Bu parametre tarafından sağlanan sayıyı aşarsa, yeni bir depolama anlık görüntü yürütmeden önce eski anlık görüntü silindi.

Bir ölçek genişletme söz konusu olduğunda, betik tüm HANA sunucuları erişebildiğinden emin olmak için ek denetimi yapar. Betik ayrıca bir SAP HANA anlık görüntüsünü oluşturmadan önce tüm HANA örnekleri örnekleri uygun durumunu döndürmek denetler. SAP HANA anlık görüntü depolama anlık görüntü tarafından izlenir.

Betik yürütme işlemi `azure_hana_backup.pl` aşağıdaki üç aşamaya anlık görüntü depolama oluşturur:

1. SAP HANA anlık yürütür
1. Depolama anlık görüntüleri yürütür
1. Depolama anlık görüntünün yürütmeden önce oluşturulan SAP HANA anlık görüntü kaldırır

Betiği çalıştırmak için kopyalanmış olması HDB yürütülebilir klasöründen çağırın. 

Saklama dönemi komut yürütürken, parametre olarak gönderilen bir anlık görüntü sayısı ile yönetilir. Depolama anlık görüntüleri tarafından kapsanan süreyi betiği yürütülürken, bir parametre olarak gönderilen bir anlık görüntü sayısını ve yürütme süresi bir işlevdir. Tutulan anlık görüntü sayısını betiğin çağrısında bir parametre olarak adlandırılan sayıyı aşarsa, yeni bir anlık görüntü yürütülmeden önce aynı etiketi en eski depolama anlık görüntüsünü silinir. Çağrının son parametre sayısı tutulduğu anlık görüntü sayısını denetlemek için kullanabileceğiniz olarak sayı size sunar. Bu sayı ile de, dolaylı olarak, anlık görüntüler için kullanılan disk alanını denetleyebilirsiniz. 

> [!NOTE]
>Etiketini değiştirme hemen sonra sayım yeniden başlatır. Anlık görüntülerin yanlışlıkla silinmez etiketleme, katı olması gerekir.

## <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türleri için anlık görüntü sıklığı, HANA büyük örneği olağanüstü durum kurtarma işlevlerini kullanmasına bağlı olarak değişir. Bu işlev özel öneriler depolama anlık görüntü sıklığı ve yürütme süreleri için yapmanızı şart koşabileceği depolama anlık görüntüleri kullanır. 

Önemli noktalar ve öneriler izleyin yapmanızı varsayılır *değil* HANA büyük örnekleri sağlayan olağanüstü durum kurtarma işlevlerini kullanın. Bunun yerine, son 30 güne ait kurtarma noktasını zamanında sağlayabilir ve yedeklemelere sahip depolama anlık görüntüleri kullanın. Anlık görüntüler ve alan sayısı sınırlamaları göz önünde bulundurulduğunda, müşterilerin aşağıdaki gereksinimleri göz önünde bulundurduğunuzdan:

- Belirli bir noktaya kurtarma için kurtarma zamanı.
- Kullanılan alan.
- Kurtarma noktası ve kurtarma zamanı hedeflerine olası bir olağanüstü durum kurtarma için.
- HANA tam veritabanı yedeklemeleri diskleri karşı nihai yürütülmesi. Tam veritabanı yedeği her diskleri karşı veya **backint** arabirimi gerçekleştirilir, depolama anlık görüntüleri yürütülmesi başarısız oluyor. Depolama anlık görüntüleri üzerinde tam veritabanı yedeklemeleri yürütme planlıyorsanız, depolama anlık görüntüleri yürütülmesini bu süre boyunca devre dışı bırakıldığından emin olun.
- Anlık görüntü sayısı (250 ile sınırlı) birim başına.


HANA büyük örnekleri olağanüstü durum kurtarma işlevlerini kullanma müşteriler, anlık görüntü daha az sıklıkta dönemdir. Böyle durumlarda, müşteriler birleşik anlık görüntüleri /hana/data ve (/usr/sap içerir) /hana/shared 12 saat veya 24 saatlik dönem içinde gerçekleştirmek ve bunlar bir ay için anlık görüntüleri tutmak. Aynı günlük yedekleme birimi anlık görüntüleri ile geçerlidir. Bununla birlikte, SAP HANA işlem günlüğü yedeklemeleri günlük yedekleme birimi karşı yürütülmesi için 15 dakikalık dönem 5 dakika içinde gerçekleşir.

Zamanlanmış depolama anlık görüntüleri cron kullanarak en iyi şekilde gerçekleştirilir. Tüm yedeklemeleri ve olağanüstü durum kurtarma gereksinimleri için aynı komut dosyasını kullanın ve yedekleme sürelerine istenen komut dosyası girişleri çeşitli eşleşecek şekilde değiştirin. Bu anlık görüntüler tüm farklı cron yürütme zamanları bağlı olarak, zamanlanmış: saat, 12 saatlik, günlük veya haftalık. 

/ Etc/crontab bir cron zamanlama örneği verilmiştir:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
22 12 * * *  ./azure_hana_backup.pl logs dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
Önceki örnekte, yok/hana/verileri içeren birimlerin ve (/ usr/sap içerir) /hana/shared kapsayan birleştirilmiş bir saatlik anlık konumları. Bu tür bir anlık görüntü için daha hızlı bir-belirli bir noktaya kurtarma son iki gün içinde kullanın. Ayrıca, bu birimlerde günlük anlık görüntü yok. Bu nedenle, iki gün kapsamı saatlik anlık görüntüleri yanı sıra kapsamı dört haftasını tarafından günlük anlık görüntüler görüntülenerek sahip. Ayrıca, işlem günlüğü yedekleme birimi günlük yedek desteklenir. Bu yedeklemeler de dört hafta boyunca tutulur. Crontab üçüncü satırında gördüğünüz gibi HANA işlem günlüğü yedeklemesi her 5 dakikada yürütülecek şekilde zamanlanır. Depolama anlık görüntüleri yürütülen farklı cron işlerinin başlangıç zamanları dağılır, böylece bu anlık görüntülerin belirli bir noktada aynı anda zaman yürütülmez. 

Aşağıdaki örnekte, / hana/veri ve hana paylaşılan / (dahil olmak üzere/usr/sap) konumları saatlik olarak içeren birimlere kapsayan birleşik bir anlık görüntü gerçekleştirin. Bu anlık görüntüler'i iki gün boyunca tutun. İşlem günlüğü yedekleme birimlerin anlık görüntüleri 5 dakikalık olarak yürütülen ve 4 saat boyunca tutulur. Olarak önce HANA işlem günlüğü dosyasının yedeğini 5 dakikada yürütülecek şekilde zamanlanır. İşlem günlüğü yedeklemesi başlatıldıktan sonra işlem günlüğü yedekleme birimin anlık görüntüsünü 2 dakikalık bir gecikmeyle gerçekleştirilir. Bu 2 dakika içinde normal koşullarda SAP HANA işlem günlüğü yedeklemesinin tamamlanması gerekir. Olarak önce önyükleme içeren birimi LUN günde bir kez yedekleme depolama anlık görüntü tarafından desteklenir ve dört hafta boyunca tutulur.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup.pl logs regularlogback 3min 28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl logs logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

Aşağıdaki grafikte, önceki örnekte, LUN önyükleme hariç dizileri gösterilmektedir:

![Yedekleme ve anlık görüntü arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanına yapılan değişiklikleri belgelemek için /hana/log birime yönelik normal yazma gerçekleştirir. Düzenli olarak, SAP HANA /hana/data birime bir kayıt noktası yazar. Bir SAP HANA işlem günlüğü yedeklemesi crontab içinde olarak belirtilen, her 5 dakikada bir çalıştırılır. Ayrıca, bir SAP HANA anlık görüntü /hana/data ve /hana/shared birimler üzerinde birleşik depolama anlık görüntü tetikleme sonucunda saatte yürütülür bakın. Birleştirilmiş depolama anlık görüntüleri, HANA anlık görüntünün başarılı olduktan sonra yürütülür. Crontab belirtildiği gibi depolama anlık görüntüleri /hana/logbackup birimdeki her 5 dakika, yaklaşık 2 dakika sonra HANA işlem günlüğü yedeklemesi yürütülür.

> 

>[!IMPORTANT]
> Anlık görüntüleri SAP HANA işlem günlüğü yedeklemeleri ile birlikte gerçekleştirildiğinde depolama anlık görüntüleri kullanımını SAP HANA yedeklemeler için değerlidir. Depolama anlık görüntü arasındaki zaman aralıklarını karşılamak bu işlem günlüğü yedeklemeleri gerekir. 

Kullanıcılara bir taahhüt 30 günlük bir zaman içinde nokta kurtarma ayarladıysanız, için gerekir:

- Aşırı durumlarda anlık görüntü /hana/data ve 30 gün önce yapılmışsa /hana/shared üzerinde birleşik bir depolama erişim.
- Herhangi bir birleştirilmiş depolama anlık görüntüleri arasında kapsamak bitişik işlem günlüğü yedeklemeleri vardır. Bu nedenle, işlem günlüğü yedekleme biriminin eski anlık görüntü 30 günden daha eski olması gerekir. Azure depolama alanında bulunan başka bir NFS paylaşımına işlem günlüğü yedeklemeleri kopyalarsanız, durum değil. Bu durumda, eski işlem günlüğü yedeklemeleri, NFS paylaşımından çekme.

Depolama anlık görüntüleri ve işlem günlüğü yedeklemeleri nihai depolama çoğaltması yararlanmak için işlem günlüğü yedeklemeleri, SAP HANA yazacağı konum değiştirmeniz gerekir. Bu değişiklik, HANA Studio içinde yapabilirsiniz. SAP HANA tam günlük segmentleri otomatik olarak yedekler rağmen günlüğü yedekleme aralığı belirleyici olarak belirtmeniz gerekir. Olağanüstü durum kurtarma seçeneğini kullandığınızda, genellikle günlük yedeklerinin belirleyici noktayla yürütmek istediğiniz çünkü bu özellikle doğrudur. Aşağıdaki örnekte, 15 dakika günlüğü yedekleme aralığı ayarlanır.

![SAP HANA SAP HANA Studio yedekleme günlük zamanlama](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ayrıca, her 15 dakikadan daha sık yedeklemeler de seçebilirsiniz. Daha sık bir ayar, genellikle olağanüstü durum kurtarma işlevleri HANA büyük örnekleri ile birlikte kullanılır. Bazı müşteriler, işlem günlüğü yedeklemeleri her 5 dakikada gerçekleştirin.  

Veritabanı hiçbir zaman yedeklendiğinden, son yedekleme kataloğunu içinde bulunması gereken tek bir yedekleme girişi oluşturmak için bir dosya tabanlı veritabanı yedeklemesi gerçekleştirmeyi adımdır. Aksi takdirde, SAP HANA, belirtilen günlük yedeklemelerinizi başlatamazsınız.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı bir yedek alın](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


İlk başarılı depolama anlık görüntüleri yürütülen sonra 6. adımda yürütülen test anlık görüntüsünü silebilirsiniz. Bunu yapmak için komut dosyasını çalıştırmak `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl
```

Betik çıktının bir örneği verilmiştir:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Sayısı ve disk birimi anlık görüntü boyutunu izleme

Belirli depolama biriminde, anlık görüntüler ve bu anlık görüntü depolama tüketimini sayısını izleyebilirsiniz. `ls` Değildir komut Göster anlık görüntü dizin veya dosya. Ancak, Linux işletim sistemi komut `du` aynı birimlere depolandığından, bu depolama anlık görüntüleri hakkında ayrıntılı bilgiler gösterilir. Komut aşağıdaki seçenekler kullanılabilir:

- `du –sh .snapshot`: Bu seçenek, toplam anlık görüntü dizini içindeki tüm anlık görüntüleri sağlar.
- `du –sh --max-depth=1`: Bu seçenek kaydedilen tüm anlık görüntüleri listeler **.snapshot** klasörü ve her anlık görüntü boyutu.
- `du –hc`: Bu seçenek, tüm anlık görüntüler görüntülenerek kullanılan toplam boyutu sağlar.

Birimlerde tüm depolama alınan ve depolanan anlık görüntüleri kullanan değil emin olmak için şu komutları kullanın.

>[!NOTE]
>Anlık görüntüleri LUN kimliğini önceki komutlara görünmez.

### <a name="getting-details-of-snapshots"></a>Anlık görüntü ayrıntılarını alma
Anlık görüntüleri hakkında daha fazla bilgi almak için de komut dosyasını kullanabilirsiniz `azure_hana_snapshot_details.pl`. Bu betik, olağanüstü durum kurtarma konumunuz etkin bir sunucu ise iki konumdan birinde çalıştırılabilir. Anlık görüntüleri içeren her birime göre ayrılmış aşağıdaki çıktı, komut dosyası sağlar: 
   * Bir birim toplam anlık görüntü boyutu
   * Her birim anlık aşağıdaki ayrıntıları: 
      - Anlık görüntü adı 
      - Oluşturma zamanı 
      - Anlık görüntü boyutu
      - Anlık görüntü sıklığı
      - HANA yedekleme varsa bu anlık görüntü ile ilişkili kimliği

Betik yürütme söz dizimi örneği verilmiştir:

```
./azure_hana_snapshot_details.pl 
```

HANA yedekleme Kimliği almak betiği çalışacağından, SAP HANA örneğine bağlanmak gerekir. Yapılandırma dosyasında bu bağlantı gerektiren *HANABackupCustomerDetails.txt* doğru şekilde ayarlanacak. İki anlık görüntü bir birimde bir çıktı aşağıdaki gibi görünebilir:

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



### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Bir sunucu üzerinde anlık görüntü sayısını azaltma

Daha önce açıklandığı gibi belirli etiketleri, depolamanın anlık görüntü sayısını azaltabilirsiniz. Son iki anlık görüntü başlatmak için komut etiket ve korumak istediğiniz anlık görüntü sayısını parametrelerdir.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

Önceki örnekte, anlık görüntü etikettir **dailyhana** ve korunması için bu etikete sahip anlık görüntü sayısını **28**. Disk alanı tüketimini için yanıt olarak depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Son parametre kümesine sahip betiği çalıştırmak için 15'e, örneğin, anlık görüntü sayısını azaltmak için en kolay yolu olan **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Bu ayar ile betik çalıştırırsanız, yeni depolama anlık görüntüleri dahil olmak üzere bir anlık görüntü, sayısı 15'tir. 15 en son anlık görüntüleri korunur ve 15 eski anlık görüntüleri silinir.

 >[!NOTE]
 > Bu betik, birden fazla 1 saat öncesine anlık görüntüler varsa anlık görüntü sayısını azaltır. Komut dosyasını kısa 1 saat öncesine aittir anlık görüntüleri silmez. Bu kısıtlamalar, sunulan isteğe bağlı bir olağanüstü durum kurtarma işlevselliği ilgilidir.

Yedek etiketli anlık görüntüleri bir dizi korumak istiyorsanız, **hanadaily** söz dizimi örneklerde, komut dosyasını çalıştırabilirsiniz **0** bekletme sayı olarak. Ardından, bu etiketle eşleşen tüm anlık görüntüler kaldırılır. Ancak, tüm anlık görüntüleri kaldırma, HANA büyük örnekleri olağanüstü durum kurtarma işlevi yeteneklerini etkileyebilir.

Betiği kullanmak için belirli anlık görüntüleri silmek için ikinci bir seçenek olan `azure_hana_snapshot_delete.pl`. Bu betik bir anlık görüntü veya anlık görüntüleri olarak HANA Studio ya da anlık görüntü adı aracılığıyla bulunan HANA yedekleme kimliği kullanarak ya da kümesini silmek için tasarlanmıştır. Şu anda, yedekleme kimliği yalnızca için oluşturulan anlık görüntüleri bağlıdır **hana** anlık görüntü türü. Anlık görüntü türü yedeklerini **günlükleri** ve **önyükleme** bir SAP HANA anlık görüntü işlemleri yapma ve bu nedenle bu anlık görüntüler için bulunması için hiçbir yedekleme kimliği yok. Anlık görüntü adı girildiğinde, tüm anlık görüntüler farklı birimlerde girilen anlık görüntü adı ile eşleşmesi için arar. 

Betik çağrısı söz dizimini kullanarak HANA örneği SID'si belirtmenize gerek betik çağırın:

```
./azure_hana_snapshot_delete.pl <SID>

```

Kullanıcı olarak komut dosyası yürütme **kök**.

Bir anlık görüntüsü seçerseniz her anlık görüntü tek tek silebilirsiniz. Anlık görüntü içeren birimi ilk girin ve sonra anlık görüntü adını sağlayın. Anlık görüntü, birimde var ve birden fazla 1 saat öncesine, silinir. Anlık görüntü adı ve birim adları yürüterek bulabilirsiniz `azure_hana_snapshot_details` betiği. 

>[!IMPORTANT]
>Yalnızca bir anlık görüntü üzerinde mevcut veriler varsa sildiğiniz, anlık görüntüsü silindikten sonra verilerin devamlı kayıp olduğundan.

  
## <a name="file-level-restore-from-a-storage-snapshot"></a>Depolama anlık görüntüden dosya düzeyinde geri yükleme
Anlık Görüntü türleri için **hana** ve **günlükleri**, anlık görüntüleri birimler üzerinde doğrudan erişebileceğiniz **.snapshot** dizin. Her anlık görüntü için bir alt yoktur. Anlık görüntüden gerçek dizin yapısında, alt noktasında olduğu durumda, her dosya kopyalayabilirsiniz. Betik geçerli sürümünde yoktur **Hayır** (Self Servis DR parçası DR sitede yük devretme sırasında komutlar olarak anlık görüntü geri yükleme gerçekleştirilebilir ancak) anlık görüntü geri yükleme için bir Self Servis sağlanan komut dosyasını geri yükleyin. Microsoft Operasyon ekibinin mevcut kullanılabilir anlık görüntülerden istenen anlık görüntü geri yüklemek için bir hizmet isteği açarak başvurmanız gerekir.

>[!NOTE]
>Tek dosya geri yükleme LUN HANA büyük örneği birimlerinin türünü bağımsız önyükleme anlık görüntüleri için geçerli değildir. **.Snapshot** dizin önyükleme LUN gösterilmez. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüye Kurtar

Bir üretim aşağı senaryosunda, Microsoft Azure desteği ile bir müşteri olayı olarak depolama anlık görüntüden kurtarma işlemi başlatılabilir. Verileri bir üretim sisteminde silindi ve üretim veritabanını geri yüklemek için onu almak tek yolu ise yüksek aciliyet sağlasa da konusudur.

Farklı bir durumda-belirli bir noktaya kurtarma düşük aciliyet olabilir ve gün önceden planlanmış. Yüksek öncelikli bayrağı oluşturma yerine, Azure üzerinde SAP HANA ile bu kurtarma planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılımının yükseltmeyi planlama. Ardından geliştirme paket yükseltmeden önce durumunu temsil eden bir anlık görüntüye geri gerekir.

İstek göndermeden önce hazırlamanız gerekir. Azure ekibi üzerinde SAP HANA isteği ve geri yüklenen birimlere sağlayın. Daha sonra anlık görüntülerine dayalı HANA veritabanını geri yükleyin. 

Aşağıdaki, istek için hazırlama işlemini göstermektedir:

>[!NOTE]
>Kullanmakta olduğunuz SAP HANA yayın bağlı olarak, aşağıdaki ekran görüntüleri, kullanıcı arabirimi farklı olabilir.

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

1. SAP HANA Studio aracılığıyla HANA veritabanı yeniden bağlandığınızda otomatik olarak ortaya değil SAP HANA Studio içinde kurtarma seçeneklerini seçin. Aşağıdaki örnek, son HANA anlık görüntüye geri yüklemeyi gösterir. Depolama anlık bir HANA anlık görüntü katıştırır. En son depolama anlık görüntüye geri yüklerseniz, en son HANA anlık görüntü olmalıdır. (Daha eski bir depolama anlık görüntüye geri yüklerseniz, depolama anlık görüntünün alındığı zamana dayalı HANA anlık görüntü bulmak gerekir.)

   ![SAP HANA Studio içinde kurtarma seçeneklerini seçin](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

1. Seçin **veritabanını belirli bir veri yedekleme veya depolama anlık görüntü kurtarma**.

   ![Kurtarma türünü belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

1. Seçin **belirtin yedekleme kataloğu olmadan**.

   ![Yedekleme konumu belirtin penceresi](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

1. İçinde **hedef türü** listesinden **anlık görüntü**.

   ![Yedekleme Kurtar penceresine belirtin](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

1. Seçin **son** kurtarma işlemini başlatmak için.

    ![Kurtarma işlemini başlatmak için "son" düğmesini seçin](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

1. HANA veritabanı geri ve depolama anlık görüntüsüne dahil HANA anlık görüntüye, kurtarılır.

    ![HANA veritabanı geri ve HANA anlık görüntüye kurtarıldı](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>En son durumuna Kurtar

Aşağıdaki işlem, depolama anlık görüntüsüne dahil HANA anlık görüntüye geri yükler. Bunun ardından işlem günlüğü yedeklemeleri veritabanının en son durumu için depolama anlık görüntü geri yüklemeden önce geri yükler.

>[!IMPORTANT]
>Devam etmeden önce hareket günlüğü yedekleri tam ve bitişik zincirine sahip olduğunuzdan emin olun. Bu yedeklemeler geçerli durumu veritabanının geri yükleyemezsiniz.

1. Adımları 1-6 kurtarma en son HANA anlık görüntüye tamamlayın.

1. Seçin **en son durumuna veritabanını Kurtar**.

   !["En son durumuna veritabanını Kurtar" seçin](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

1. En son HANA günlük yedeklerinin konumunu belirtin. Konum tüm HANA işlem günlüğü yedeklemeleri HANA anlık görüntüden en son durumuna içermesi gerekir.

   ![En son HANA günlük yedeklerinin konumunu belirtin](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

1. Veritabanını kurtarmak kendisinden temel olarak bir yedekleme seçin. Bu örnekte, ekran görüntüsünde HANA anlık görüntü depolama anlık dahil HANA anlık görüntüsüdür. 

   ![Veritabanını kurtarmak kendisinden temel olarak bir yedekleme seçin](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

1. NET **Delta yedeklemelerini kullanın** deltaları en son durumu ve HANA anlık görüntüsünün zaman arasında yoksa, kutuyu.

   ![Hiçbir deltaları varsa "Delta yedeklemelerini kullanın" onay kutusunu temizleyin.](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

1. Özet ekranında, seçin **son** geri yükleme yordamı başlatmak için.

   ![Özet ekranında "Son" tıklayın](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Başka bir noktasına geri
(Depolama anlık görüntüsüne dahil) HANA anlık görüntü ve HANA anlık görüntü noktası-ın-time kurtarma sonraki bir arasında zaman içinde bir noktaya kurtarmak için aşağıdaki adımları gerçekleştirin:

1. HANA anlık görüntüden tüm işlem günlüğü yedeklemeleri için kurtarmak istediğiniz kez olduğundan emin olun.
1. En son durumuna altında kurtarma yordamı başlayın.
1. Yordamı, 2'de adımda **kurtarma türünü belirtin** penceresinde **aşağıdaki noktasının veritabanına geri**ve ardından noktası süreyi belirtin. 
1. 3-6. adımları tamamlayın.

## <a name="monitor-the-execution-of-snapshots"></a>İzleyici yürütme anlık görüntüleri

HANA büyük örnekleri depolama anlık görüntüleri kullanma gibi Ayrıca bu anlık görüntülerin yürütülmesini izlemek için gerekir. Depolama anlık görüntüleri yürüten betik çıkışı bir dosyaya yazar ve Perl betikleri aynı konuma kaydeder. Her Depolama anlık görüntü için ayrı bir dosyaya yazılır. Her dosya çıktısı, anlık görüntü komut yürütülüp yürütülmediği çeşitli aşamaları gösterir:

1. Bir anlık görüntüsünü oluşturmak için gereken birimleri bulur.
1. Bu birimlerdeki alınan anlık görüntülere bulur.
1. Belirtilen anlık görüntü sayısını eşleştirilecek nihai mevcut anlık görüntüleri siler.
1. Bir SAP HANA anlık görüntüsünü oluşturur.
1. Depolama anlık görüntüleri birimleri oluşturur.
1. SAP HANA anlık görüntüsünü siler.
1. En son anlık görüntüye yeniden adlandırır **.0**.

Belirtilen betik cab en önemli bir parçası bir bu parçasıdır:
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
Bu örnekten nasıl HANA anlık görüntü oluşturma betiği kayıtları görebilirsiniz. Genişleme durumunda, ana düğüm üzerinde bu işlemi başlatılır. Ana düğüm her çalışan düğümü üzerinde SAP HANA anlık görüntüleri zaman uyumlu oluşturulmasını başlatır. Depolama anlık görüntüsü daha sonra alınır. Depolama anlık görüntüleri başarılı yürütmenin ardından, HANA anlık görüntüsü silinir. Ana düğüm HANA anlık görüntüyü silme işlemi başlatılır.


## <a name="next-steps"></a>Sonraki adımlar
- Bkz: [ilkeleri olağanüstü durum kurtarma ve hazırlık](hana-concept-preparation.md).
