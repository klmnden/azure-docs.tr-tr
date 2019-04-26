---
title: Azure için Avere vFXT taşıma verileri
description: Azure için kullanılacak yeni depolama birimi Avere vFXT ile veri ekleme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: a3d6cb745c782d2a7166208f2a8dd1202a330b15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60410134"
---
# <a name="moving-data-to-the-vfxt-cluster---parallel-data-ingest"></a>Veri taşımayı vFXT kümeye - paralel veri alma 

Yeni bir vFXT küme oluşturduktan sonra verileri yeni depolama biriminin üzerine taşımak için ilk göreviniz olabilir. Ancak, veri taşıma, her zamanki yöntemi bir istemciden bir basit kopyalama komutu veriyor, yavaş kopyalama performansı büyük olasılıkla görürsünüz. Tek iş parçacıklı kopyalama Avere vFXT kümenin arka uç depolama alanına veri kopyalamak için iyi bir seçenek değil.

Avere vFXT küme ölçeklenebilir çok istemci önbelleğini veri kopyalamak için kullanılabilecek en hızlı ve en verimli şekilde birden fazla istemciyle olduğu. Bu teknik, dosya ve nesne alımı parallelizes.

![Birden çok istemci, çok iş parçacıklı veri taşıma gösteren diyagram: Sol üst kısmında, şirket içi donanım depolama için bir simge kendisinden gelen birden çok oklar içerir. Oklar, dört istemci makinelerine gelin. Her istemci makinesinden üç oklar doğru Avere vFXT gelin. Avere vFXT birden çok oklar Blob depolama alanına gelin.](media/avere-vfxt-parallel-ingest.png) 

``cp`` Veya ``copy`` bir depolama sistemden veri aktarmak için kullanılması için yaygın olarak kullanılan komutları olan tek iş parçacıklı işlemler, aynı anda yalnızca bir dosyasını kopyalayın. Bu, dosya sunucusu küme kaynaklarının boşa harcanmasına olan aynı anda - yalnızca bir dosya almak anlamına gelir.

Bu makalede, birden çok istemci, çok iş parçacıklı dosya sistemi Avere vFXT kümeye verileri taşımak için kopyalama oluşturmaya yönelik stratejiler açıklanmaktadır. Bu, dosya aktarımı kavramları ve birden çok istemci ve basit kopyalama komutları kullanarak etkin veri kopyalamak için kullanılan karar noktaları açıklar.

Ayrıca, yardımcı olabilecek bazı yardımcı programlar açıklar. ``msrsync`` Yardımcı programı, kısmen demetlerin içine bir veri kümesi bölme ve rsync komutlarını kullanarak işlemini otomatik hale getirmek için kullanılabilir. ``parallelcp`` Betiğidir kaynak dizini okuyan başka bir yardımcı program ve sorunları komutları otomatik olarak kopyalayın.  

Bir bölüme atlamak için bağlantıya tıklayın:

* [El ile kopyalama örnek](#manual-copy-example) - kopyalama komutları kullanarak eksiksiz bir açıklaması
* [Kısmen otomatik (msrsync) örneği](#use-the-msrsync-utility-to-populate-cloud-volumes) 
* [Örnek denemeleri kopyalayarak paralel](#use-the-parallel-copy-script)

## <a name="data-ingestor-vm-template"></a>Veri çıkışlara VM şablonu

Resource Manager şablonu otomatik olarak bu makalede değinilen paralel veri alma araçları ile bir VM oluşturmak için GitHub üzerinde kullanılabilir. 

![birden çok oklarla her blob depolama, donanım depolama ve Azure kaynaklarını gösteren diyagram. Oklar "veri çıkışlara VM" ve buradan Avere vFXT için birden çok oklar noktası noktası](media/avere-vfxt-ingestor-vm.png)

VM veri çıkışlara burada yeni oluşturulan VM'ye Avere vFXT küme bağlar ve kümeden, önyükleme betik indirir bir öğretici bir parçasıdır. Okuma [veri çıkışlara VM önyükleme](https://github.com/Azure/Avere/blob/master/docs/data_ingestor.md) Ayrıntılar için.

## <a name="strategic-planning"></a>Stratejik planlama

Paralel olarak veri kopyalamak için bir strateji oluştururken, dosya boyutu, dosya sayısı ve dizin derinliği ödün anlamanız gerekir.

* Dosyaları küçük olduğunda, ilgi ölçüm, saniye başına dosya sayısıdır.
* Dosyalar olduğunda büyük (10MiBi veya üzeri), ölçüm ilgi Saniyedeki bayt sayısıdır.

Aktarım hızı ve Kopyala komut uzunluğunu zamanlama ve dosya sayısı ve boyutunu hesaba katacak şekilde ölçülebilir bir aktarılan dosyaların oranda, her bir kopyalama işlemine sahiptir. Bu belgenin kapsamı dışında olan ücretler ölçmek nasıl açıklayan, ancak, küçük veya büyük dosyalarla uğraşıyor olup olmadığını anlamak için zorunludur.

## <a name="manual-copy-example"></a>El ile kopyalama örnek 

Çok iş parçacıklı bir kopyasını bir istemci üzerinde birden fazla kopya komutu aynı anda arka planda önceden tanımlanmış kümesi dosyaları veya yolları karşı çalıştırarak el ile oluşturabilirsiniz.

Linux/UNIX ``cp`` komutu bağımsız değişken içeren ``-p`` sahipliği ve mtime meta verileri korumak için. Bu bağımsız değişken eklemek için aşağıdaki komutları isteğe bağlıdır. (Bağımsız değişken ekleyerek hedef dosya sistemi meta veri değişikliği için istemci tarafından gönderilen dosya sistemi çağrılarının sayısını artırır.)

Bu basit örnekte, iki dosya paralel olarak kopyalar:

```bash
cp /mnt/source/file1 /mnt/destination1/ & cp /mnt/source/file2 /mnt/destination1/ &
```

Bu komutu gönderdikten sonra `jobs` komutu iki iş parçacığı çalıştığını gösterir.

### <a name="predictable-filename-structure"></a>Tahmin edilebilir dosya yapısı 

Dosya adları, tahmin edilebilir olduğu durumlarda, paralel kopyalama iş parçacığı oluşturmak için ifadeleri kullanabilirsiniz. 

Örneğin, dizininize sırayla numaralandırılmış 1000 dosyalar varsa `0001` için `1000`, her 100 dosya kopyalayın on paralel iş parçacığı oluşturmak için aşağıdaki ifadeleri kullanabilirsiniz:

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination1/ & \
cp /mnt/source/file2* /mnt/destination1/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination1/ & \
cp /mnt/source/file5* /mnt/destination1/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination1/ & \
cp /mnt/source/file8* /mnt/destination1/ & \
cp /mnt/source/file9* /mnt/destination1/
```

### <a name="unknown-filename-structure"></a>Bilinmeyen dosya yapısı

Dosya adlandırma yapınızı tahmin edilebilir değilse, dizin adları ile dosyaları gruplandırabilirsiniz. 

Bu örnekte göndermek için tüm dizinleri toplar ``cp`` arka plan görevleri komutları çalıştırın:

```bash
/root
|-/dir1
| |-/dir1a
| |-/dir1b
| |-/dir1c
   |-/dir1c1
|-/dir1d
```

Dosyaları toplandıktan sonra paralel bir kopya çalıştırabilirsiniz komutları için yinelemeli olarak alt dizinleri ve tüm içerikleri kopyalayın:

```bash
cp /mnt/source/* /mnt/destination/
mkdir -p /mnt/destination/dir1 && cp /mnt/source/dir1/* mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ & 
cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ & # this command copies dir1c1 via recursion
cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
```

### <a name="when-to-add-mount-points"></a>Bağlama noktaları eklemek ne zaman

Tek hedef dosya sistemi bağlama noktası karşı giderek yeterli paralel iş parçacığı sonra burada daha fazla iş parçacığı ekleyerek daha fazla aktarım hızı sağlamazsa bir noktası olacaktır. (Aktarım hızı, saniye başına dosyaları veya bayt/saniye, veri türüne bağlı olarak ölçülecektir.) Ya da daha da kötüsü, aşırı iş parçacığı bazen bir performans düşüşüne neden olabilir.  

Bu durumda, aynı uzak dosya sistemi bağlama yolu kullanarak diğer vFXT küme IP adresleri için istemci tarafı bağlama noktaları ekleyebilirsiniz:

```bash
10.1.0.100:/nfs on /mnt/sourcetype nfs (rw,vers=3,proto=tcp,addr=10.1.0.100)
10.1.1.101:/nfs on /mnt/destination1type nfs (rw,vers=3,proto=tcp,addr=10.1.1.101)
10.1.1.102:/nfs on /mnt/destination2type nfs (rw,vers=3,proto=tcp,addr=10.1.1.102)
10.1.1.103:/nfs on /mnt/destination3type nfs (rw,vers=3,proto=tcp,addr=10.1.1.103)
```

İstemci tarafı bağlama noktaları ek ek kopyalama komutları devre dışı çatal sağlar ekleme `/mnt/destination[1-3]` bağlama noktaları, daha fazla paralellik elde edin.  

Örneğin, dosyalarınızı çok büyükse, paralel kopyalama gerçekleştirmenin istemcisinden daha fazla komutları gönderme farklı hedef yolları kullanmak için kopyalama komutları tanımlayabilirsiniz.

```bash
cp /mnt/source/file0* /mnt/destination1/ & \
cp /mnt/source/file1* /mnt/destination2/ & \
cp /mnt/source/file2* /mnt/destination3/ & \
cp /mnt/source/file3* /mnt/destination1/ & \
cp /mnt/source/file4* /mnt/destination2/ & \
cp /mnt/source/file5* /mnt/destination3/ & \
cp /mnt/source/file6* /mnt/destination1/ & \
cp /mnt/source/file7* /mnt/destination2/ & \
cp /mnt/source/file8* /mnt/destination3/ & \
```

Yukarıdaki örnekte, tüm üç hedef bağlama noktaları istemci dosya kopyalama işlemleri tarafından hedeflenen.

### <a name="when-to-add-clients"></a>Zaman istemcileri eklemek için

Son olarak, ne zaman istemci özellikleri, daha fazla kopyasını iş parçacığı ekleme sınırına ulaştınız veya ek bağlama noktaları herhangi ek dosyaları/sn verim değil veya bayt/sn artırır. Böyle bir durumda, başka bir istemci aynı dosya kopyalama işlemleri kendi kümesi çalıştıran bağlama noktaları kümesi ile dağıtabilirsiniz. 

Örnek:

```bash
Client1: cp -R /mnt/source/dir1/dir1a /mnt/destination/dir1/ &
Client1: cp -R /mnt/source/dir2/dir2a /mnt/destination/dir2/ &
Client1: cp -R /mnt/source/dir3/dir3a /mnt/destination/dir3/ &

Client2: cp -R /mnt/source/dir1/dir1b /mnt/destination/dir1/ &
Client2: cp -R /mnt/source/dir2/dir2b /mnt/destination/dir2/ &
Client2: cp -R /mnt/source/dir3/dir3b /mnt/destination/dir3/ &

Client3: cp -R /mnt/source/dir1/dir1c /mnt/destination/dir1/ &
Client3: cp -R /mnt/source/dir2/dir2c /mnt/destination/dir2/ &
Client3: cp -R /mnt/source/dir3/dir3c /mnt/destination/dir3/ &

Client4: cp -R /mnt/source/dir1/dir1d /mnt/destination/dir1/ &
Client4: cp -R /mnt/source/dir2/dir2d /mnt/destination/dir2/ &
Client4: cp -R /mnt/source/dir3/dir3d /mnt/destination/dir3/ &
```

### <a name="create-file-manifests"></a>Dosya bildirimleri oluşturma

Sonra anlama (birden çok kopya-iş parçacığı hedef, istemci başına birden çok hedefe, ağ üzerinden erişilebilen kaynak dosya sistemi başına birden çok istemci başına), yukarıdaki yaklaşımları bu öneriyi dikkate alın: Dosya bildirimleri oluşturmak ve bunları birden fazla istemci genelinde ile kopya komutları kullanın.

Bu senaryo kullanan UNIX ``find`` komut dosyaları veya dizinleri bildirimleri oluşturmak için:

```bash
user@build:/mnt/source > find . -mindepth 4 -maxdepth 4 -type d
./atj5b55c53be6-01/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-01/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-01/support/trace/rolling
./atj5b55c53be6-03/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-03/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-03/support/trace/rolling
./atj5b55c53be6-02/support/gsi/2018-07-22T21:12:06EDT
./atj5b55c53be6-02/support/pcap/2018-07-23T01:34:57UTC
./atj5b55c53be6-02/support/trace/rolling
```

Bu sonuç, bir dosyaya yönlendirin: `find . -mindepth 4 -maxdepth 4 -type d > /tmp/foo`

Dosya sayısı ve alt dizinleri boyutunu belirlemek için BASH komutları kullanma bildirimi aracılığıyla yineleyebilirsiniz sonra:

```bash
ben@xlcycl1:/sps/internal/atj5b5ab44b7f > for i in $(cat /tmp/foo); do echo " `find ${i} |wc -l`    `du -sh ${i}`"; done
244    3.5M    ./atj5b5ab44b7f-02/support/gsi/2018-07-18T00:07:03EDT
9      172K    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.8M    ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T01:01:00UTC
131    13M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    6.2M    ./atj5b5ab44b7f-02/support/gsi/2018-07-20T21:59:41UTC
134    12M     ./atj5b5ab44b7f-02/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
7      16K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:12:19UTC
8      83K     ./atj5b5ab44b7f-02/support/pcap/2018-07-18T17:17:17UTC
575    7.7M    ./atj5b5ab44b7f-02/support/cores/armada_main.2000.1531980253.gsi
33     4.4G    ./atj5b5ab44b7f-02/support/trace/rolling
281    6.6M    ./atj5b5ab44b7f-01/support/gsi/2018-07-18T00:07:03EDT
15     182K    ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-18T05:01:00UTC
244    17M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-19T01:01:01UTC
299    31M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T01:01:00UTC
256    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T21:59:41UTC_partial
889    7.7M    ./atj5b5ab44b7f-01/support/gsi/2018-07-20T21:59:41UTC
262    29M     ./atj5b5ab44b7f-01/support/gsi/stats_2018-07-20T22:22:55UTC_vfxt_catchup
11     248K    ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:12:19UTC
11     88K     ./atj5b5ab44b7f-01/support/pcap/2018-07-18T17:17:17UTC
645    11M     ./atj5b5ab44b7f-01/support/cores/armada_main.2019.1531980253.gsi
33     4.0G    ./atj5b5ab44b7f-01/support/trace/rolling
244    2.1M    ./atj5b5ab44b7f-03/support/gsi/2018-07-18T00:07:03EDT
9      158K    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-18T05:01:00UTC
124    5.3M    ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-19T01:01:01UTC
152    15M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T01:01:00UTC
131    12M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T21:59:41UTC_partial
789    8.4M    ./atj5b5ab44b7f-03/support/gsi/2018-07-20T21:59:41UTC
134    14M     ./atj5b5ab44b7f-03/support/gsi/stats_2018-07-20T22:25:58UTC_vfxt_catchup
7      159K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:12:19UTC
7      157K    ./atj5b5ab44b7f-03/support/pcap/2018-07-18T17:17:17UTC
576    12M     ./atj5b5ab44b7f-03/support/cores/armada_main.2013.1531980253.gsi
33     2.8G    ./atj5b5ab44b7f-03/support/trace/rolling
```

Son olarak, istemcilere gerçek dosya kopyalama komutları çalışıyorlardı gerekir.  

Dört istemcileriniz varsa, bu komutu kullanın:

```bash
for i in 1 2 3 4 ; do sed -n ${i}~4p /tmp/foo > /tmp/client${i}; done
```

Beş istemcileriniz varsa, aşağıdaki gibi kullanın:

```bash
for i in 1 2 3 4 5; do sed -n ${i}~5p /tmp/foo > /tmp/client${i}; done
```

Ve altı için... Gerektiği şekilde tahmin.

```bash
for i in 1 2 3 4 5 6; do sed -n ${i}~6p /tmp/foo > /tmp/client${i}; done
```

Erişmenizi sağlayacak *N* ortaya çıkan dosyalar, her biri için *N* yol adları çıktısını bir parçası olarak elde edilen düzeyi dört dizinlerine sahip istemciler `find` komutu. 

Her dosya kopyalama komutu oluşturmak için kullanın:

```bash
for i in 1 2 3 4 5 6; do for j in $(cat /tmp/client${i}); do echo "cp -p -R /mnt/source/${j} /mnt/destination/${j}" >> /tmp/client${i}_copy_commands ; done; done
```

Yukarıdaki erişmenizi *N* dosyaları, her bir BASH komut dosyası olarak istemci üzerinde çalışabilen satır başına bir kopyalama komutu. 

Bu komut dosyaları istemci başına eşzamanlı olarak birden çok iş parçacığı, paralel olarak birden çok istemcide çalıştırılacak hedeftir.

## <a name="use-the-msrsync-utility-to-populate-cloud-volumes"></a>Bulut birimleri doldurmak için msrsync yardımcı programını kullanın

``msrsync`` Aracı ayrıca Avere küme için bir arka uç çekirdek dosyalayıcı veri taşımak için kullanılabilir. Bu aracı, birden çok paralel çalıştırarak bant genişliği kullanımını iyileştirmek için tasarlanmıştır ``rsync`` işlemler. Adresindeki github'dan edinilebilir https://github.com/jbd/msrsync.

``msrsync`` ayrı "buckets" kaynak dizinine'kurmak keser ve bağımsız çalışan ``rsync`` her bir demete işlemleri.

Dört çekirdekli VM kullanarak ön test 64 işlem kullanırken en iyi verimlilik gösterilmiştir. Kullanım ``msrsync`` seçeneği ``-p`` işlemlerin sayısı 64'e ayarlamak için.

Unutmayın ``msrsync`` yalnızca yerel birimler gelen ve giden yazabilirsiniz. Kaynak ve hedef kümenin sanal ağda yerel takar olarak erişilebilir olması gerekir.

Bir Azure bulut birimi Avere kümesiyle doldurmak için msrsync kullanmak için bu yönergeleri izleyin:

1. Msrsync ve (rsync ve Python 2.6 veya sonraki sürümleri) ön yükleme
1. Dosyalar ve dizinler kopyalanacak toplam sayısını belirler.

   Örneğin, Avere yardımcı programını kullanın ``prime.py`` bağımsız değişkenlerle ```prime.py --directory /path/to/some/directory``` (url indirerek kullanılabilir https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py).

   Değil kullanıyorsanız ``prime.py``, Gnu öğeleriyle sayısını hesaplayabilir ``find`` aracını aşağıda gösterilen şekilde:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. İşlem başına öğe sayısını belirlemek için 64 öğe sayısını bölün. İle bu numarayı kullanmak ``-f`` komutu çalıştırdığınızda, demet boyutunu ayarlamak için seçeneği.

1. Dosyaları kopyalamak için msrsync komutu yürütün:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Örneğin, bu komut 64 işlemlerde 11.000 dosyaları için /mnt/vfxt/repository /test/source-repository taşımak için tasarlanmıştır:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/vfxt/repository``

## <a name="use-the-parallel-copy-script"></a>Paralel kopyalama komut dosyası kullan

``parallelcp`` Betiği ayrıca vFXT kümenizin arka uç depolama alanına veri taşımak için kullanışlı olabilir. 

Aşağıdaki komut dosyası yürütülebilir dosya ekleyecek `parallelcp`. (Bu betik Ubuntu için tasarlanmıştır; başka bir dağıtım kullanıyorsanız yüklemelisiniz ``parallel`` ayrı ayrı.)

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM 
#!/bin/bash

display_usage() { 
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n" 
} 

if [  \$# -le 1 ] ; then 
    display_usage
    exit 1
fi 
 
if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then 
    display_usage
    exit 0
fi 

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

### <a name="parallel-copy-example"></a>Örnek denemeleri kopyalayarak paralel

Bu örnek, derlemek için paralel kopyalama betiği kullanır. ``glibc`` Avere küme kaynak dosyalarını kullanma. 
<!-- xxx what is stored where? what is 'the avere cluster mount point'? xxx -->

Kaynak dosyaları Avere küme bağlama noktasında depolanır ve nesne dosyaları yerel sabit sürücüsünde depolanır.

Bu betik, yukarıdaki paralel kopyalama betiği kullanır. Seçenek ``-j`` ile kullanılan ``parallelcp`` ve ``make`` paralelleştirme elde etmek için.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/node1 avere
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```

