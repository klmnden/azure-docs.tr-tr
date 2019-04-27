---
title: Azure sanal makinelere TmaxSoft OpenFrame yükleyin
description: Azure sanal makinelerinde (VM'ler) TmaxSoft OpenFrame ortamı kullanarak IBM z/OS ana iş yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 78a8b5e7a1c5512f81315519210bc7759dd15342
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60800415"
---
# <a name="install-tmaxsoft-openframe-on-azure"></a>TmaxSoft OpenFrame Azure'a yükleme

Bir OpenFrame ortamı azure'da geliştirme, tanıtımlar, test veya üretim iş yükleri için uygun ayarlama konusunda bilgi edinin. Bu öğreticide, her adımda açıklanmaktadır.

OpenFrame Azure'da anabilgisayar öykünmesi ortam oluşturma, birden çok bileşeni içerir. Örneğin, anabilgisayar ara yazılım IBM müşteri bilgileri denetim sistemi (CICS) gibi çevrimiçi hizmetlere OpenFrame değiştirin ve OpenFrame Batch, kendi TJES bileşeni ile IBM anabilgisayar kişinin iş giriş alt sistem (JES) değiştirir.

OpenFrame Oracle veritabanı, Microsoft SQL Server, IBM Db2 ve MySQL gibi bir ilişkisel veritabanı ile çalışır. Bu yüklemeyi OpenFrame TmaxSoft Tibero ilişkisel veritabanı kullanır. Bir Linux işletim sisteminde OpenFrame hem Tibero çalıştırın. Desteklenen diğer Linux dağıtımları kullanabilirsiniz, ancak bu öğretici, CentOS 7.3 yükler. Bir sanal makinede (VM) OpenFrame uygulama sunucusu ve Tibero veritabanı yüklenir.

Öğreticiyi OpenFrame suite bileşenlerini yükleme adımları. Bazı ayrıca yüklenmelidir.

Ana OpenFrame bileşenler:

- Gerekli yükleme paketleri.
- Tibero veritabanı.
- Açık veritabanı bağlantısı (ODBC) OpenFrame uygulamaların Tibero veritabanıyla iletişim kurmak için kullanılır.
- OpenFrame temeli, tüm sistemin yöneten bir ara yazılım.
- OpenFrame toplu işlem, ana bilgisayar'ın toplu sistemler değiştirir çözüm.
- TACF, sistemleri ve kaynaklara kullanıcı erişimini denetleyen bir hizmeti modülü.
- ProSort, toplu işlem için bir sıralama Aracı'nı tıklatın.
- OFCOBOL, ana bilgisayar'ın COBOL programlar yorumlar bir derleyici.
- OFASM, ana bilgisayar'ın assembler programlar yorumlar bir derleyici.
- OpenFrame sunucu türü C (OSC), ana bilgisayar'ın Ara yazılım ve IBM CICS değiştirir çözüm.
- Java Enterprise kullanıcı çözümü (JEUS), bir web uygulaması sunucusunun Java Enterprise Edition 6 için sertifikalı.
- OFGW, 3270 dinleyici sağlayan OpenFrame ağ geçidi bileşeni.
- OFManager, web ortamında OpenFrame'nın işlem ve yönetim işlevleri sağlayan bir çözüm.

Diğer OpenFrame bileşenler gereklidir:

- OSI, ana bilgisayar ara yazılım ve IMS DC değiştirir çözüm.
- TJES, ana bilgisayar'ın JES ortamı sağlayan çözümü.
- OFTSAM, açık sistemde kullanılacak (V) SAM dosyaları sağlayan çözümü.
- OFHiDB, ana bilgisayar değiştirir çözüm IMS DB güçlendirin.
- OFPLI, PL anabilgisayar yorumlar bir derleyici kullanıcının / ı programları.
- PROTRIEVE, CA-Easytrieve anabilgisayar dili yürüten bir çözüm.
- OFMiner, bir çözüm anabilgisayar varlıkları analiz eder ve ardından bunları Azure'a geçirir.

## <a name="architecture"></a>Mimari

Aşağıdaki şekil, bu öğreticide OpenFrame 7.0 mimari bileşenler genel bir bakış sağlar:

![OpenFrame bileşenleri](media/openframe-02.png)

## <a name="azure-system-requirements"></a>Azure sistem gereksinimleri

Aşağıdaki tabloda, Azure yükleme gereksinimleri listelenmektedir.
<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Gereksinim</th><th>Açıklama</th></tr>
</thead>
<tbody>
<tr><td>Azure'da desteklenen Linux dağıtımları
</td>
<td>
Linux x86 2.6 (32-bit, 64-bit)<br/>
Red Hat 7.x<br/>
CentOS 7.x<br/>
</td>
</tr>
<tr><td>Donanım
</td>
<td>Çekirdek: 2 (minimum)<br/>
Bellek: 4 GB (minimum)<br/>
Değiştirme alanı: 1 GB (minimum)<br/>
Sabit disk: 100 GB (minimum)<br/>
</td>
</tr>
<tr><td>Windows kullanıcıları için isteğe bağlı yazılım
</td>
<td>PuTTY: VM özelliklerini yapılandırmak için bu kılavuzda kullanılan<br/>
WinSCP: Popüler SFTP istemci ve FTP istemcisi kullanabilirsiniz<br/>
Windows için Eclipse: TmaxSoft tarafından desteklenen bir geliştirme platformu<br/>
(Microsoft Visual Studio şu anda desteklenmiyor)
</td>
</tr>
</tbody>
</table>

<!-- markdownlint-enable MD033 -->

## <a name="prerequisites"></a>Önkoşullar

Gerekli olan tüm yazılımların bir araya getirin ve el ile gerçekleştirilen tüm işlemleri tamamlamak için birkaç gün harcamayı planlayın.

Başlamadan önce aşağıdakileri yapın:

- OpenFrame yükleme medyasını TmaxSoft alın. Varolan bir TmaxSoft müşteri varsa, lisanslı kopyasını TmaxSoft temsilcinize başvurun. Aksi takdirde, deneme sürümünden istek [TmaxSoft](http://www.tmaxsoft.com/contact/).

- E-posta göndererek OpenFrame belgeleri istek <support@tmaxsoft.com>.

- Zaten yoksa, bir Azure aboneliği edinme. Ayrıca oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

- İsteğe bağlı. Siteden siteye VPN tüneli veya izin verilen kullanıcılara, kuruluşunuzdaki Azure VM erişimi kısıtlayan bir Sıçrama kutusu ayarlayın. Bu adım gerekli değildir, ancak en iyi bir uygulamadır.

## <a name="set-up-a-vm-on-azure-for-openframe-and-tibero"></a>Azure VM'de OpenFrame ve Tibero için ayarlayın

Çeşitli dağıtım modelleri kullanarak OpenFrame ortamı ayarlayabilirsiniz, ancak aşağıdaki yordamı OpenFrame uygulama sunucusu ve bir VM üzerinde Tibero veritabanı dağıtma işlemi gösterilmektedir. Daha büyük ortamlarda ve büyük iş yükleri için en iyi yöntem daha iyi performans için kendi VM veritabanında ayrı olarak dağıtmaktır.

**Bir VM oluşturmak için**

1. Adresinden Azure portalında Git <http://portal.azure.com> ve hesabınızda oturum açın.

2. Tıklayın **sanal makineler**.

    ![Azure portalında kaynak listesi](media/vm-01.png)

3. **Ekle**'ye tıklayın.

    ![Azure portalında seçeneği ekleme](media/vm-02.png)

4. Sağındaki **işletim sistemleri**, tıklayın **daha fazla**.

     ![Azure Portalı'nda daha fazla seçenek](media/vm-03.png)

5. Tıklayın **CentOS tabanlı 7.3** bu gözden geçirme tam olarak takip etmek veya başka bir desteklenen Linux dağıtımı seçebilirsiniz.

     ![Azure Portalı'nda işletim sistemi seçenekleri](media/vm-04.png)

6. İçinde **Temelleri** ayarlarını girin **adı**, **kullanıcı adı**, **kimlik doğrulama türü**, **abonelik** (Kullandıkça Öde değer ödeme AWS stilini) ve **kaynak grubu** (var olanı kullanın veya TmaxSoft grubu oluşturma).

7. Tamamlandığında (için ortak/özel anahtar çifti dahil olmak üzere **kimlik doğrulama türü**), tıklayın **Gönder**.

> [!NOTE]
> SSH ortak anahtarı için kullanılıyorsa **kimlik doğrulama türü**, ortak/özel anahtar çifti oluşturmak için sonraki bölümde adımlara bakın, sonra da buradaki adımları devam.

### <a name="generate-a-publicprivate-key-pair"></a>Bir ortak/özel anahtar çifti oluşturma

Bir Windows işletim sistemi kullanıyorsanız, bir ortak/özel anahtar çifti oluşturmak için PuTTYgen gerekir.

Ortak anahtar serbestçe paylaşılabilir, ancak özel anahtarı tamamen gizli tutulmalıdır ve hiçbir zaman başka bir tarafla paylaşılmaz. Anahtarları oluşturduktan sonra yapıştırmak zorunda **SSH ortak anahtarı** yapılandırmanın içine — Linux VM'ye aslında karşıya yükleniyor. Depolandığını iç yetkili\_içindeki anahtarları \~/.ssh dizini kullanıcı hesabınızın giriş dizini. Linux VM tanıması ve ilişkili sağladığınızda bağlantıyı doğrulama ise **SSH özel anahtarı** de SSH istemcisi (bizim durumumuzda, PuTTY).

Yeni kişiler erişim VM yaparken: 

- Her yeni puttygen araçlarını kullanarak kendi ortak/özel anahtarları oluşturur.
- Kişiler, kendi özel anahtarları ayrı ayrı depolayın ve sanal makinenin yönetici için ortak anahtar bilgilerini gönder.
- Yönetici için ortak anahtar içeriğini yapıştırır \~/.ssh/authorized\_anahtarları dosyası.
- Yeni tek PuTTY üzerinden bağlanır.

**Bir ortak/özel anahtar çifti oluşturmak için**

1.  PuTTYgen'nden indirin <https://www.putty.org/> ve tüm varsayılan ayarları kullanarak yükleyin.

2.  PuTTYgen açmak için PuTTY yükleme dizini C: bulun\\Program dosyaları\\PuTTY.

    ![PuTTY arabirimi](media/puttygen-01.png)

3.  Tıklayın **oluşturmak**.

    ![PuTTY anahtar Oluşturucu iletişim kutusu](media/puttygen-02.png)

4.  Oluşturduktan sonra ortak anahtar ve özel anahtarı Kaydet. Ortak anahtar içeriğini yapıştırın **SSH ortak anahtarı** bölümünü **sanal makine oluşturma \> Temelleri** bölmesinde (6 ve 7 önceki bölümdeki adımları gösterilen).

    ![PuTTY anahtar Oluşturucu iletişim kutusu](media/puttygen-03.png)

### <a name="configure-vm-features"></a>VM özelliklerini yapılandırma

1. Azure portalında, **bir boyut seçin** dikey penceresinde istediğiniz Linux makine donanım ayarlarını seçin. *Minimum* Tibero hem OpenFrame yüklemeye ilişkin gereksinimleri, 2 CPU ve 4 GB RAM Bu örnek yükleme gösterildiği verilmiştir:

    ![Temel bilgileri - sanal makine oluşturma](media/create-vm-01.png)

2. Tıklayın **3 ayar** ve isteğe bağlı özellikleri yapılandırmak için varsayılan ayarları kullanın.
3. Ödeme bilgilerinizi gözden geçirin.

    ![Satın alma - sanal makine oluşturma](media/create-vm-02.png)

4. Seçimlerinizi gönderin. Azure VM dağıtmaya başlar. Bu işlem genellikle birkaç dakika sürer.

5. VM dağıtıldıktan sonra yapılandırma sırasında seçilen tüm ayarları gösteren panosunu görüntülenir. Not **genel IP adresi**.

    ![Azure panosunda tmax](media/create-vm-03.png)

6. PuTTY’yi açın.

7. İçin **ana bilgisayar adı**, kullanıcı adınızı yazın ve genel IP adresinin kopyalanır. Örneğin, **kullanıcıadı\@publicıp**.

    ![PuTTY yapılandırması iletişim kutusu](media/putty-01.png)

8. İçinde **kategori** kutusunun **bağlantı \> SSH \> Auth**. Yolunu belirtin, **özel anahtarı** dosya.

    ![PuTTY yapılandırması iletişim kutusu](media/putty-02.png)

9. Tıklayın **açık** PuTTY penceresini açın. Başarılı olursa, Azure üzerinde çalışan yeni CentOS VM'nin bağlandığı.

10. Kök kullanıcı olarak oturum açmak için şunu yazın **sudo bash**.

    ![Kök kullanıcı oturum açma komut penceresi](media/putty-03.png)

## <a name="set-up-the-environment-and-packages"></a>Ortamı ve paketleri ayarlama

VM oluşturulduktan ve oturum açmış göre birkaç Kurulum adımı gerçekleştirmek ve önyükleme gerekli paketleri yükleyin.

1. Eşleştirmek **ofdemo** hosts dosyasını düzenlemek için olduğu gibi vi kullanarak yerel IP adresi (`vi /etc/hosts`). Örneğimizde IP varsayılarak olan 192.168.96.148 ofdemo, bu, değişiklikten önce:

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    <IP Address>    <your hostname>
    ```

     Bu değişiklikten sonra.

    ```vi
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4 
    ::1              localhost localhost.localdomain localhost6 localhost6.localdomain 
    192.168.96.148   ofdemo
    ```

2. Kullanıcı ve gruplar oluşturun:

    ```vi
    [root@ofdemo ~]# adduser -d /home/oframe7 oframe7 
    [root@ofdemo ~]# passwd oframe7
    ```

3. Kullanıcı oframe7 parolasını değiştirin:

    ```vi
    New password: 
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    ```

4. /Etc/sysctl.conf çekirdek parametrelerinde güncelleştirin:

    ```vi
    [root@ofdemo ~]# vi /etc/sysctl.conf
    kernel.shmall = 7294967296 
    kernel.sem = 10000 32000 10000 10000
    ```

5. Yeniden başlatma olmadan dinamik olarak çekirdek parametreleri Yenile:

    ```vi
    [root@ofdemo ~]# /sbin/sysctl -p
    ```

6. Gerekli paketleri alın: Sunucu Internet'e bağlı olduğundan emin olun, aşağıdaki paketleri indirin ve yükleyin:

     - dos2unix
     - Glibc
     - glibc.i686 glibc.x86\_64
     - libaio
     - ncurses

          > [!NOTE]
          > Ncurses paket yüklendikten sonra aşağıdaki simgesel bağlantılar oluştur:
         ```
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so
         ln -s /usr/lib64/libncurses.so.5.9 /usr/lib/libtermcap.so.2
         ```

     - gcc
     - gcc-c ++
     - libaio devel.x86\_64
     - strace
     - ltrace
     - GDB

7. Java RPM yükleme gerçekleştirildiğinde, aşağıdakileri yapın:

```
root@ofdemo ~]# rpm -ivh jdk-7u79-linux-x64.rpm
[root@ofdemo ~]# vi .bash_profile

# JAVA ENV
export JAVA_HOME=/usr/java/jdk1.7.0_79/
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$CLASSPATH:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

[root@ofdemo ~]# source /etc/profile
[root@ofdemo ~]# java –version

java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

[root@ofdemo ~]# echo $JAVA_HOME /usr/java/jdk1.7.0_79/
```

## <a name="install-the-tibero-database"></a>Tibero veritabanını yükleme

Tibero Azure'da OpenFrame ortamında birkaç temel işlevleri sağlar:

- Tibero OpenFrame iç veri deposu olarak çeşitli sistem işlevleri için kullanılır.
- KSDS RRDS ve ESDS, dahil VSAM dosyaları Tibero veritabanı veri depolama için dahili olarak kullanın.
- TACF veri deposu Tibero içinde depolanır.
- OpenFrame katalog bilgileri Tibero içinde depolanır.
- Tibero veritabanı IBM Db2 bir ardılı olarak uygulama verilerini depolamak için kullanılabilir.

**Tibero yüklemek için**

1. Tibero ikili yükleyici dosyanın var olduğundan emin olun ve sürüm numarasını gözden geçirin.
2. Tibero yazılım Tibero kullanıcı hesabına (oframe) kopyalayın. Örneğin:

    ```
    [oframe7@ofdemo ~]$ tar -xzvf tibero6-bin-6_rel_FS04-linux64-121793-opt-tested.tar.gz 
    [oframe7@ofdemo ~]$ mv license.xml /opt/tmaxdb/tibero6/license/
    ```

3. .Bash açın\_VI profilinde (`vi .bash_profile`) aşağıdaki yapıştırın:

    ```
    # Tibero6 ENV
    export TB_HOME=/opt/tmaxdb/tibero6 
    export TB_SID=TVSAM export TB_PROF_DIR=$TB_HOME/bin/prof 
    export LD_LIBRARY_PATH=$TB_HOME/lib:$TB_HOME/client/lib:$LD_LIBRARY_PATH 
    export PATH=$TB_HOME/bin:$TB_HOME/client/bin:$PATH
    ```

4. Bash profili, komut isteminde çalıştırmak için:

    ```
    source .bash_profile
    ```

5. İpucu dosyası (Tibero için bir yapılandırma dosyası) oluşturur ve ardından içinde olduğu gibi vi açın. Örneğin:

    ```
    [oframe7@ofdemo ~]$ sh $TB_HOME/config/gen_tip.sh
    [oframe7@ofdemo ~]$ vi $TB_HOME/config/$TB_SID.tip
    ```

6. Değiştirme \$TB\_HOME/client/config/tbdsn.tbr ve 127.0.0.1 yerine put gösterildiği oflocalhost:

    ```
    TVSAM=( 
    (INSTANCE=(HOST=127.0.0.1)
        (PT=8629)
    (DB_NAME=TVSAM)
          )
     )
    ```

7. Veritabanı oluşturun. Aşağıdaki çıktı görünür:

    ```
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NOMOUNT mode).
     /--------------------- newmount sql ------------------------/
    create database character set MSWIN949 national character set UTF16;
    /-----------------------------------------------------------/
    Database created.
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof.
    Listener port = 8629
    Tibero 6
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    /opt/tmaxdb/tibero6/bin/tbsvr
    ………………………..
    Creating agent table...
    Done.
    For details, check /opt/tmaxdb/tibero6/instance/TVSAM/log/system_init.log.
    ************************************************** 
    * Tibero Database TVSAM is created successfully on Fri Aug 12 19:10:43 UTC 2016.
    *     Tibero home directory ($TB_HOME) =
    *         /opt/tmaxdb/tibero6
    *     Tibero service ID ($TB_SID) = TVSAM
    *     Tibero binary path =
    *         /opt/tmaxdb/tibero6/bin:/opt/tmaxdb/tibero6/client/bin
    *     Initialization parameter file =
    *         /opt/tmaxdb/tibero6/config/TVSAM.tip
    * 
    * Make sure that you always set up environment variables $TB_HOME and
    * $TB_SID properly before you run Tibero.
     ******************************************************************************
    ```

8. Tibero geri dönüştürmek için ilk kullanarak kapatabilirler `tbdown` komutu. Örneğin:

    ```
    [oframe7@ofdemo ~]$$ tbdown 
    Tibero instance terminated (NORMAL mode).
    ```

9. Artık Tibero kullanarak önyükleme `tbboot`. Örneğin:

    ```
    [oframe7@ofdemo ~]$ tbboot
    Change core dump dir to /opt/tmaxdb/tibero6/bin/prof. Listener port = 8629

    Tibero 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Tibero instance started up (NORMAL mode).
    ```

10. Bir tablo oluşturmak için SYS kullanarak veritabanına erişim (sys/tmax) kullanıcı ardından TACF ve varsayılan birimi için gereken tablo oluşturun:

    ```
    [oframe7@ofdemo ~]$ tbsql tibero/tmax
    tbSQL 6  
    TmaxData Corporation Copyright (c) 2008-. All rights reserved.
    Connected to Tibero.
    ```

11. Şimdi aşağıdaki SQL komutlarını yazın:

    ```
    SQL> create tablespace "DEFVOL" datafile 'DEFVOL.dbf' size 500M autoextend on; create tablespace "TACF00" datafile 'TACF00.dbf' size 500M autoextend on; create tablespace "OFM_REPOSITORY" datafile 'ofm_repository.dbf' size 300M autoextend on;
    SQL> Tablespace 'DEFVOL' created.
    SQL> Tablespace 'TACF00' created.
    SQL> Tablespace ' OFM_REPOSITORY ' created.
    SQL> SQL> Disconnected.
    ```

12. Tibero önyükleme ve Tibero işlemlerin çalıştığını doğrulayın:

    ```
    [oframe7@ofdemo ~]$ tbboot 
    ps -ef | egrep tbsvr
    ```

Çıkış:

![Tibero çıkış](media/tibero-01.png)

## <a name="install-odbc"></a>ODBC yükleme

OpenFrame uygulamalarda, açık kaynaklı unixODBC proje tarafından sağlanan ODBC API kullanarak Tibero veritabanı ile iletişim kurar.

ODBC yüklemek için:

1. UnixODBC 2.3.4.tar.gz yükleyici dosyanın var olduğundan emin olun veya kullanın `wget unixODBC-2.3.4.tar.gz` komutu. Örneğin:

     ```
     [oframe7@ofdemo ~]$ wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.4.tar.gz
     ```

2. İkili dosya sıkıştırmasını açın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf unixODBC-2.3.4.tar.gz
     ```

3. UnixODBC 2.3.4 dizine gidin ve denetimi makine bilgileri kullanarak derleme görevleri dosyası oluşturur. Örneğin:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ ./configure --prefix=/opt/tmaxapp/unixODBC/ --sysconfdir=/opt/tmaxapp/unixODBC/etc
     ```

     Varsayılan olarak, unixODBC / usr/local, böylece yüklü `--prefix` konumu değiştirmek için bir değer geçirir. Benzer şekilde, yapılandırma dosyalarını/etc içinde varsayılan olarak, bu nedenle yüklü `--sysconfdir` istenen konuma değerini geçirir.

4. Derleme görevleri dosyası yürütün: `[oframe7@ofdemo unixODBC-2.3.4]$ make`

5. Yürütülebilir dosyayı derledikten sonra program dizinine kopyalayın. Örneğin:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ make install
     ```

6. Bash profili düzenlemek için olduğu gibi vi kullanın (`vi ~/.bash_profile`) ve aşağıdakileri ekleyin:

     ```
     # UNIX ODBC ENV 
     export ODBC_HOME=$HOME/unixODBC 
     export PATH=$ODBC_HOME/bin:$PATH 
     export LD_LIBRARY_PATH=$ODBC_HOME/lib:$LD_LIBRARY_PATH 
     export ODBCINI=$HOME/unixODBC/etc/odbc.ini 
     export ODBCSYSINI=$HOME
     ```

7. ODBC için geçerlidir. Aşağıdaki dosyaları uygun şekilde düzenleyin. Örneğin:

     ```
     [oframe7@ofdemo unixODBC-2.3.4]$ source ~/.bash_profile

     [oframe7@ofdemo ~]$ cd

     [oframe7@ofdemo ~]$ odbcinst -j unixODBC 2.3.4
     DRIVERS............: /home/oframe7/odbcinst.ini
     SYSTEM DATA SOURCES: /home/oframe7/odbc.ini
     FILE DATA SOURCES..: /home/oframe7/ODBCDataSources
     USER DATA SOURCES..: /home/oframe7/unixODBC/etc/odbc.ini
     SQLULEN Size.......: 8
     SQLLEN Size........: 8
     SQLSETPOSIROW Size.: 8

     [oframe7@ofdemo ~]$ vi odbcinst.ini

     [Tibero]
     Description = Tibero ODBC driver for Tibero6
     Driver = /opt/tmaxdb/tibero6/client/lib/libtbodbc.so
     Setup = 
     FileUsage = 
     CPTimeout = 
     CPReuse = 
     Driver Logging = 7

     [ODBC]
     Trace = NO 
     TraceFile = /home/oframe7/odbc.log 
     ForceTrace = Yes 
     Pooling = No 
     DEBUG = 1

     [oframe7@ofdemo ~]$ vi odbc.ini

     [TVSAM]
     Description = Tibero ODBC driver for Tibero6 
     Driver = Tibero 
     DSN = TVSAM 
     SID = TVSAM 
     User = tibero 
     password = tmax
     ```

8. Sembolik bağlantı oluşturun ve Tibero veritabanı bağlantısını doğrulama:

     ```
     [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbc.so $ODBC_HOME/lib/libodbc.so.1 [oframe7@ofdemo ~]$ ln $ODBC_HOME/lib/libodbcinst.so 
     $ODBC_HOME/lib/libodbcinst.so.1

     [oframe7@ofdemo lib]$ isql TVSAM tibero tmax
     ```

Aşağıdaki çıktı görüntülenir:

![SQL için ODBC çıkış gösteren bağlı](media/odbc-01.png)

## <a name="install-openframe-base"></a>OpenFrame tabanı yükleme

Temel uygulama sunucusu, önce OpenFrame işlem sunucusu süreçleri de dahil olmak üzere azure'da sistemini yönetmek için kullandığı ayrı ayrı hizmetlere yüklenir.

**OpenFrame temel yüklemek için**

1. Tibero yükleme başarılı olduğundan emin olun ve ardından doğrulayın aşağıdaki OpenFrame\_Base7\_0\_Linux\_x86\_64. bin yükleyici dosya ve base.properties yapılandırma dosyası yok.

2. Bash profili aşağıdaki Tibero özgü bilgilerle güncelleştirin:

     ```bash
     alias ofhome='cd $OPENFRAME_HOME'
     alias ulog='cd $OPENFRAME_HOME/log/tmax/ulog'
     alias sysjcl='cd $OPENFRAME_HOME/volume_default/SYS1.JCLLIB'
     alias sysload='cd $OPENFRAME_HOME/volume_default/SYS1.LOADLIB'
     alias sysproc='cd $OPENFRAME_HOME/volume_default/SYS1.PROCLIB'
     alias oscsrc='cd $OPENFRAME_HOME/osc/oivp'
     alias osisrc='cd $OPENFRAME_HOME/osi/oivp'
     alias defvol='cd $OPENFRAME_HOME/volume_default'
     ```

3. Bash profili yürütün:`[oframe7@ofdemo ~]$ . .bash_profile`
4. Tibero işlemleri çalıştığından emin olun. Örneğin:

     ```linux
     [oframe7@ofdemo ~]$ ps -ef|grep tbsvr
     ```

    ![Taban](media/base-01.png)

     > [!IMPORTANT]
     > Yüklemeden önce Tibero başlattığınız emin olun.

5. Lisans oluşturmak [technet.tmaxsoft.com](https://technet.tmaxsoft.com/en/front/main/main.do) ve OSC lisansları OpenFrame Base, Batch, TACF yerleştirin, uygun bir klasörde:

     ```
     [oframe7@ofdemo ~]$ cp license.dat /opt/tmaxapp/OpenFrame/core/license/
     [oframe7@ofdemo ~]$ cp lictjes.dat lictacf.dat licosc.dat $OPENFRAME_HOME/license/
     ```

6. İkili ve base.properties OpenFrame temel dosyalarını indirin:

     ```
     [oframe7@ofdemo ~]$ vi base.properties
     OPENFRAME_HOME= <appropriate location for installation> ex. /opt/tmaxapp/OpenFrame TP_HOST_NAME=<your IP Hostname> ex. ofdemo 
     TP_HOST_IP=<your IP Address> ex. 192.168.96.148 
     TP_SHMKEY=63481 
     TP_TPORTNO=6623 
     TP_UNBLOCK_PORT=6291 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     MASCAT_NAME=SYS1.MASTER.ICFCAT 
     MASCAT_CREATE=YES 
     DEFAULT_VOLSER=DEFVOL 
     VOLADD_DEFINE=YES TSAM_USERNAME=tibero 
     TSAM_PASSWORD=tmax 
     TSAM_DATABASE=oframe 
     DATASET_SHMKEY=63211 
     DSLOCK_DATA=SYS1.DSLOCK.DATA 
     DSLOCK_LOG=SYS1.DSLOCK.LOG 
     DSLOCK_SEQ=dslock_seq.dat 
     DSLOCK_CREATE=YES 
     OPENFRAME_LICENSE_PATH=/opt/tmaxapp/license/OPENFRAME TMAX_LICENSE_PATH=/opt/tmaxapp/license/TMAX
     ```

7. Base.properties dosyası kullanan yükleyici yürütün. Örneğin:

    ```
    [oframe7@ofdemo ~]$ chmod a+x OpenFrame_Base7_0_Linux_x86_64.bin 
    [oframe7@ofdemo ~]$ ./OpenFrame_Base7_0_Linux_x86_64.bin -f base.properties
    ```

    İşlem tamamlandığında yükleme tamamlandı iletisini etkinleştirirseniz ' dir.

8. OpenFrame temel dizin yapısını kullanarak doğrulama `ls -ltr` komutu. Örneğin:

     ```
     [oframe7@ofdemo OpenFrame]$ ls -ltr
     total 44

     drwxrwxr-x. 4 oframe7 oframe7 61 Nov 30 16:57 UninstallerData 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 bin 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 cpm drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 data 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:57 include 
     drwxrwxr-x. 2 oframe7 oframe7 8192 Nov 30 16:57 lib 
     drwxrwxr-x. 6 oframe7 oframe7 48 Nov 30 16:57 log 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 profile 
     drwxrwxr-x. 7 oframe7 oframe7 62 Nov 30 16:57 sample 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 schema 
     drwxrwxr-x. 2 oframe7 oframe7 6 Nov 30 16:57 temp 
     drwxrwxr-x. 3 oframe7 oframe7 16 Nov 30 16:57 shared 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 license 
     drwxrwxr-x. 23 oframe7 oframe7 4096 Nov 30 16:58 core 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 config 
     drwxrwxr-x. 2 oframe7 oframe7 4096 Nov 30 16:58 scripts 
     drwxrwxr-x. 2 oframe7 oframe7 25 Nov 30 16:58 volume_default
     ```

9. Başlangıç OpenFrame tabanı:

     ```
     [oframe7@ofdemo ~]$ cp /usr/lib/libtermcap.so.2 $TMAXDIR/lib
     Startup Tmax Server
     [oframe7@ofdemo ~]$ tmboot
     ```

     ![tmboot komut çıktısı](media/base-02.png)

10. İşlem durumunu sı tmadmin komutu kullanarak hazır olduğunu doğrulayın. RDY görüntülendiği **durumu** işlemlerin her biri için sütun:

     ![tmadmin komut çıktısı](media/base-03.png)

11. OpenFrame temel kapatın:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(vtammgr:43) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLH downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: CLL downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TLM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMM downed: Wed Sep  7 15:37:21 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-openframe-batch"></a>OpenFrame toplu yükleme

OpenFrame Batch anabilgisayar batch ortamının benzetimini yapan birçok bileşenden oluşur ve toplu işlerini Azure'da çalıştırmak için kullanılır.

**Batch yüklemek için**

1. Temel yükleme başarılı olduğundan emin olun ve ardından doğrulayın OpenFrame\_Batch7\_0\_Fix2\_MVS\_Linux\_x86\_64. bin yükleyicisini ve Batch.Properties yapılandırma dosyası var:

2. Komut isteminde `vi batch.properties` VI kullanarak batch.properties dosyayı düzenlemek için.

3. Parametreleri aşağıdaki gibi değiştirin:

     ```
     OPENFRAME_HOME = /opt/tmaxapp/OpenFrame
     DEFAULT_VOLSER=DEFVOL 
     TP_NODE_NAME=NODE1 
     TP_NODE_LIST=NODE1 
     RESOURCE_SHMKEY=66991 
     #JOBQ_DATASET_CREATE=YES 
     #OUTPUTQ_DATASET_CREATE=YES 
     DEFAULT_JCLLIB_CREATE=YES 
     DEFAULT_PROCLIB_CREATE=YES 
     DEFAULT_USERLIB_CREATE=YES 
     TJES_USERNAME=tibero 
     TJES_PASSWORD=tmax 
     TJES_DATABASE=oframe 
     BATCH_TABLE_CREATE=YES
     ```

4. Komut isteminde şunu yazın batch yükleyiciyi çalıştırmak için:

     ```
     ./OpenFrame_Batch7_0_Fix2_MVS_Linux_x86_64.bin -f batch.properties
     ```

5. Yükleme tamamlandığında, yüklenen OpenFrame paketlerini yazarak başlatın `tmboot` komut isteminde.

    ![tmboot çıkış](media/tmboot-01.png)

6. Tür `tmadmin` OpenFrame işlemi denetlemek için komut isteminde.

    ![Tmax yönetici ekran](media/tmadmin-01.png)

7. Aşağıdaki komutları yürütün:

     ```
     $$2 NODE1 (tmadm): quit 
     ADM quit for node (NODE1)
     ```

8. Kullanım `tmdown` başlangıç yukarı ve Batch kapatmak için komut:

     ```
     [oframe7@ofdemo ~]$tmdown
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:44) downed: Wed Sep  7 16:01:46 2016
     TMDOWN: SERVER(vtammgr: 43) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:45) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:46) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjinit:55) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:49) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:51) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:52) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjmsvr:53) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofruisvr:41) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: SERVER(ofrsmlog:42) downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLL downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TLM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: CLH downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMM downed: Wed Sep  7 16:01:46 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-tacf"></a>TACF yükleyin

TACF, sistemleri ve RACF güvenlik kaynaklarına kullanıcı erişimini denetleyen bir OpenFrame hizmeti modülü yöneticisidir.

**TACF yüklemek için**

1. Doğrulayın OpenFrame\_Tacf7\_0\_Fix2\_Linux\_x86\_64. bin yükleyici dosya ve tacf.properties yapılandırma dosyası yok.
2. Toplu yükleme başarılı olduğundan emin olun ve ardından tacf.properties dosyasını açmak için olduğu gibi vi kullanın (`vi tacf.properties`).
3. TACF parametreleri değiştirin:

     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame 
     USE_OS_AUTH=NO 
     TACF_USERNAME=tibero 
     TACF_PASSWORD=tmax 
     TACF_DATABASE=oframe 
     TACF_TABLESPACE=TACF00 
     TACF_TABLE_CREATE=YES
     ```

4. TACF yükleyici tamamladıktan sonra TACF ortam değişkenleri uygulanır. Komut istemine şunları yazın:

     ```
     source \~/.bash\_profile
     ```

5. TACF yükleyici yürütün. Komut istemine şunları yazın:

     ```
     ./OpenFrame_Tacf7_0_Fix2_Linux_x86_64.bin -f tacf.properties
     ```

     Çıktı aşağıdakine benzer olacaktır:

     ```
     Wed Dec 07 17:36:42 EDT 2016
     Free Memory: 18703 kB 
     Total Memory: 28800 kB

     4 Command Line Args: 
     0:  -f 1:  tacf.properties 
     2:  -m 
     3:  SILENT 
     java.class.path: 
     /tmp/install.dir.41422/InstallerData 
     /tmp/install.dir.41422/InstallerData/installer.zip 
     ZGUtil.CLASS_PATH: 
     /tmp/install.dir.41422/InstallerData 
     tmp/install.dir.41422/InstallerData/installer.zip 
     sun.boot.class.path: 
     /tmp/install.dir.41422/Linux/resource/jre/lib/resources.jar /tmp/install.dir.41422/Linux/resource/jre/lib/rt.jar /tmp/install.dir.41422/Linux/resource/jre/lib/sunrsasign.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jsse.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jce.jar /tmp/install.dir.41422/Linux/resource/jre/lib/charsets.jar /tmp/install.dir.41422/Linux/resource/jre/lib/jfr.jar /tmp/install.dir.41422/Linux/resource/jre/classes
     ```

6. Komut isteminde `tmboot` OpenFrame yeniden başlatmak için. Çıktı aşağıdakine benzer olacaktır:

     ```
     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/7 
     TMBOOT: TMM is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLL is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: CLH is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: TLM(tlm) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsasvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrlhsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrdsedt) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrcmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofruisvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrsmlog) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(vtammgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjschd) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjinit) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjhist) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmjspbk) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(ofrpmsvr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(obmtsmgr) is starting: Wed Sep  7 17:48:53 2016 
     TMBOOT: SVR(tmsvr) is starting: Wed Sep  7 17:48:53 2016
     ```

7. İşlem durumunu kullanarak hazır olduğunu doğrulamak `tmadmin` içinde `si` komutu. Örneğin:

     ```
     [oframe7\@ofdemo \~]\$ tmadmin
     ```

     İçinde **durumu** RDY sütununda görünür:

    ![Durum sütununda RDY](media/tmboot-02.png)

8. Aşağıdaki komutları yürütün:

     ```
     $$2 NODE1 (tmadm): quit 
     DM quit for node (NODE1)

     [oframe7@ofdemo ~]$ tacfmgr 
     Input USERNAME  : ROOT 
     Input PASSWORD  : SYS1

     TACFMGR: TACF MANAGER START!!!
     QUIT TACFMGR: TACF MANAGER END!!!

     [oframe7@ofdemo ~]$ tmdow
     ```

9. Kullanarak sunucuyu kapatma `tmdown` komutu. Çıktı aşağıdakine benzer olacaktır:

     ```
     [oframe7@ofdemo ~]$ tmdown 
     Do you really want to down whole Tmax? (y : n): y

     TMDOWN for node(NODE1) is starting: 
     TMDOWN: SERVER(ofrlhsvr:37) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdsedt:39) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjschd:54) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:47) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:48) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrdmsvr:38) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjmsvr:50) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjhist:56) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrsasvr:36) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrcmsvr:40) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmjspbk:57) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(tmsvr:60) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(ofrpmsvr:58) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: SERVER(obmtsmgr:59) downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLL downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: CLH downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TLM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMM downed: Wed Sep  7 17:50:50 2016 
     TMDOWN: TMAX is down
     ```

## <a name="install-prosort"></a>ProSort yükleme

ProSort, toplu işlem veri sıralama için kullanılan bir yardımcı programdır.

**ProSort yüklemek için**

1. Batch yüklemenin başarılı olduğundan emin olun ve ardından doğrulayın **bin prosort prosort\_linux64 2123 opt.tar.gz 2sp3** yükleyici dosya mevcut.

2. Özellikler dosyası kullanan yükleyici yürütün. Komut istemine şunları yazın:

     ```
     tar -zxvf prosort-bin-prosort\_2sp3-linux64-2123-opt.tar.gz
     ```

3. Prosort dizin giriş konuma taşıyın. Komut istemine şunları yazın:

     ```
     mv prosort /opt/tmaxapp/prosort
     ```

4. Bir lisans alt dizin oluşturma ve lisans dosyasını kopyalayın. Örneğin:

     ```
     cd /opt/tmaxapp/prosort 
     mkdir license 
     cp /opt/tmaxsw/oflicense/prosort/license.xml /opt/tmaxapp/prosort/license
     ```

5. İçinde olduğu gibi vi bash.Profile açın (`vi .bash_profile`) ve şu şekilde güncelleştirin:

     ```bash
     #       PROSORT

     PROSORT_HOME=/opt/tmaxapp/prosort 
     PROSORT_SID=gbg 
     PATH=$PATH:$PROSORT_HOME/bin LD_LIBRARY_PATH=$PROSORT_HOME/lib:$LD_LIBRARY_PATH LIBPATH$PROSORT_HOME/lib:$LIBPATH 
     export PROSORT_HOME PROSORT_SID 
     PATH LD_LIBRARY_PATH LIBPATH 
     PATH=$PATH:$OPENFRAME_HOME/shbin 
     export PATH
     ```

6. Komut isteminde, bir bash profili yürütmek için aşağıdakileri yazın: `. .bash_profile`

7. Yapılandırma dosyası oluşturun. Örneğin:

     ```
     oframe@oframe7: cd /opt/tmaxapp/prosort/config 
     oframe@oframe7: ./gen_tip.sh 
     Using PROSORT_SID "gbg"
      /home/oframe7/prosort/config/gbg.tip generated
     ```

8. Sembolik bağlantısını oluşturun. Örneğin:

     ```
     oframe@oframe7: cd /opt/tmaxapp/OpenFrame/util/ 
     oframe@oframe7home/oframe7/OpenFrame/util :  ln -s DFSORT SORT
     ```

9. Yürüterek ProSort yüklemesini doğrulama `prosort -h` komutu. Örneğin:

     ```
     oframe@oframe7: prosort -h

     Usage: prosort [options] [sort script files]
     options ------
     -h             Display this information 
     -v             Display version information 
     -s             Display state information 
     -j             Display profile information 
     -x             Use SyncSort compatible mode
     ```

## <a name="install-ofcobol"></a>OFCOBOL yükleyin

OFCOBOL ana bilgisayar'ın COBOL programlar yorumlar OpenFrame derleyicisidir. 

**OFCOBOL yüklemek için**

1. Toplu iş/çevrimiçi yükleme başarılı olduğundan emin olun ve ardından doğrulayın OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64. bin yükleyici dosya mevcut.

2. Komut isteminde OFCOBOL yükleyiciyi çalıştırmak için aşağıdakileri yazın:

     ```
      ./OpenFrame\_COBOL3\_0\_40\_Linux\_x86\_64.bin
     ```

3. Lisans sözleşmesini okuyun ve devam etmek için Enter tuşuna basın.

4. Lisans sözleşmesini kabul edin. Yükleme tamamlandığında aşağıdakiler görünür:

     ```
     Choose Install Folder 
     --------------------
     Where would you like to install?
     Default Install Folder: /home/oframe7/OFCOBOL

     ENTER AN ABSOLUTE PATH, OR PRESS <ENTER> TO ACCEPT THE DEFAULT : /opt/tmaxapp/OFCOBOL

     INSTALL FOLDER IS: /opt/tmaxapp/OFCOBOL 
     IS THIS CORRECT? (Y/N): Y[oframe7@ofdemo ~]$ vi .bash_profile

     ============================================================================ Installing... 
     ------------
     [==================|==================|==================|==================] 
     [------------------|------------------|------------------|------------------]

     =============================================================================== Installation Complete 
     --------------------
     Congratulations. OpenFrame_COBOL has been successfully installed
     PRESS <ENTER> TO EXIT THE INSTALLER
     ```

5. Bash profili içinde olduğu gibi vi açın (`vi .bash_profile`) ve OFCOBOL değişkenlerle güncelleştirilmiş doğrulayın.
6. Bash profili yürütün. Komut istemine şunları yazın:

     ```
      source ~/.bash_profile
     ```

7. OFCOBOL lisans yüklü klasöre kopyalayın. Örneğin:
     ```
     mv licofcob.dat $OFCOB_HOME/license
     ```
8. OpenFrame tjclrun.conf yapılandırma dosyasına gidin ve içinde olduğu gibi vi açın. Örneğin:
     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

   Değişiklik SYSLIB bölümü aşağıda verilmiştir:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib
     ```
   Değişiklikten sonra SYSLIB bölüm şöyledir:
     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bin LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${COBDIR}/lib:/ usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib :${ODBC_HOME}/lib 
     :${OFCOB_HOME}/lib
     ```
9. OpenFrame gözden\_COBOL\_InstallLog.log dosya içinde olduğu gibi vi ve herhangi bir hata olmadığını doğrulayın. Örneğin:
     ```
     [oframe7@ofdemo ~]$ vi $OFCOB_HOME/UninstallerData/log/OpenFrame_COBOL_InstallLog.log 
     …….. 
     Summary 
     ------
     Installation: Successful. 
     131 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalError
     ```
10. Kullanım `ofcob --version` komut ve yüklemeyi doğrulamak için sürüm numarasını gözden geçirin. Örneğin:

     ```
     [oframe7@ofdemo ~]$ ofcob --version 
     OpenFrame COBOL Compiler 3.0.54 
     CommitTag:: 645f3f6bf7fbe1c366a6557c55b96c48454f4bf
     ```

11. OpenFrame kullanarak yeniden `tmdown/tmboot` komutu.

## <a name="install-ofasm"></a>OFASM yükleyin

OFASM ana bilgisayar'ın assembler programlar yorumlar OpenFrame derleyicisidir.

**OFASM yüklemek için**

1. Toplu iş/çevrimiçi yükleme başarılı olduğundan emin olun ve ardından doğrulayın OpenFrame\_ASM3\_0\_Linux\_x86\_64. bin yükleyici dosya mevcut.

2. Yükleyiciyi çalıştırın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ ./OpenFrame_ASM3_0_Linux_x86_64.bin
     ```

3. Lisans sözleşmesini okuyun ve devam etmek için Enter tuşuna basın.
4. Lisans sözleşmesini kabul edin.
5. Bash profili OFASM değişkenlerle güncellenir doğrulayın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ source .bash_profile
     [oframe7@ofdemo ~]$ ofasm --version 
     # TmaxSoft OpenFrameAssembler v3 r328 
     (3ff35168d34f6e2046b96415bbe374160fcb3a34)

     [oframe7@ofdemo OFASM]$ vi .bash_profile

     # OFASM ENV 
     export OFASM_HOME=/opt/tmaxapp/OFASM 
     export OFASM_MACLIB=$OFASM_HOME/maclib/free_macro 
     export PATH="${PATH}:$OFASM_HOME/bin:" 
     export LD_LIBRARY_PATH="./:$OFASM_HOME/lib:$LD_LIBRARY_PATH"
     ```

6. İçinde olduğu gibi vi OpenFrame tjclrun.conf yapılandırma dosyasını açın ve şu şekilde düzenleyin:

     ```
     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/config 
     [oframe7@ofdemo ~]$ vi tjclrun.conf
     ```

     [SYSLIB] bölümü işte *önce* Değiştir:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib
     ```

     [SYSLIB] bölümü işte *sonra* Değiştir:

     ```
     [SYSLIB] BIN_PATH=${OPENFRAME_HOME}/bin:${OPENFRAME_HOME}/util:${COBDIR}/bin:/usr/local/bin:/bi n:${OPENFRAME_HOME}/volume_default/SYS1.LOADLIB LIB_PATH=${OPENFRAME_HOME}/lib:${OPENFRAME_HOME}/core/lib:${TB_HOME}/client/lib:${CO BDIR}/lib:/usr/lib:/lib:/lib/i686:/usr/local/lib:${PROSORT_HOME}/lib:/opt/FSUNbsort/lib:${OFCOB_HOM E}/lib:${ODBC_HOME}/lib:${OFPLI_HOME}/lib:${OFASM_HOME}/lib
     ```

7. OpenFrame açın\_ASM\_InstallLog.log dosya içinde olduğu gibi vi ve herhangi bir hata olmadığını doğrulayın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ vi 
     $OFASM_HOME/UninstallerData/log/OpenFrame_ASM_InstallLog.log 
     …….. 
     Summary 
     ------

     Installation: Successful.

     55 Successes 
     0 Warnings 
     0 NonFatalErrors 
     0 FatalErrors
     ```

8. Aşağıdaki komutlardan birini göndererek OpenFrame yeniden:

     ```
     tmdown / tmboot
     ```

     — veya —

     ```
     oscdown / oscboot
     ```

## <a name="install-osc"></a>OSC yükleyin

OSC hızlı OLTP işlemleri ve diğer yönetim işlevlerini desteklediği IBM CICS benzer OpenFrame ortamıdır.

**OSC yüklemek için**

1. Temel yükleme başarılı olduğundan emin olun ve ardından doğrulayın OpenFrame\_OSC7\_0\_Fix2\_Linux\_x86\_64. bin yükleyici dosya ve osc.properties yapılandırma dosyası mevcut.
2. Aşağıdaki parametreleri osc.properties dosyasındaki düzenleyin:
     ```
     OPENFRAME_HOME=/opt/tmaxapp/OpenFrame OSC_SYS_OSC_NCS_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_NCS OSC_APP_OSC_TC_PATH=/opt/tmaxapp/OpenFrame/temp/OSC_TC
     ```

3. Gösterildiği gibi özellikler dosyası kullanan yükleyici yürütün:

     ```
     [oframe7@ofdemo ~]$ chmod a+x OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin [oframe7@ofdemo ~]$ ./OpenFrame_OSC7_0_Fix2_Linux_x86_64.bin -f osc.properties
     ```

     İşiniz bittiğinde "Kurulum Tamamlandı" iletisi görüntülenir.

4. Bash profili OSC değişkenlerle güncelleştirildiğini doğrulayın.
5. OpenFrame gözden\_OSC7\_0\_Fix2\_InstallLog.log dosya. Şuna benzer şekilde görünecektir:

     ```
     Summary 
     ------ 
     Installation: Successful.

     233 Successes
     0 Warnings
     0 NonFatalErrors
     0 FatalError
     ```

6. VI ofsys.seq yapılandırma dosyası kullanın. Örneğin:

     ```
     vi $OPENFRAME_HOME/config/ofsys.seq
     ```

7. İçinde \#temel ve \#toplu olarak bölümlerde, parametreleri gösterildiği gibi düzenleyin.

     ```
     Before changes
     #BASE
     ofrsasvr
     ofrlhsvr
     ofrdmsvr
     ofrdsedt
     ofrcmsvr
     ofruisvr
     ofrsmlog
     vtammgr
     TPFMAGENT

     #BATCH 
     #BATCH#obmtsmgr
     #BATCH#ofrpmsvr
     #BATCH#obmjmsvr
     #BATCH#obmjschd
     #BATCH#obmjinit
     #BATCH#obmjhist
     #BATCH#obmjspbk
     #TACF #TACF#tmsvr

     After changes  #BATCH
     #BASE          obmtsmgr 
     ofrsasvr       ofrpmsvr
     ofrlhsvr       obmjmsvr
     ofrdmsvr       obmjschd
     ofrdsedt       obmjinit
     ofrcmsvr       obmjhist
     ofruisvr       obmjspbk
     ofrsmlog
     vtammgr        #TACF
     TPFMAGENT      tmsvr
    ```

8. Lisans dosyasını kopyalayın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ cp /home/oframe7/oflicense/ofonline/licosc.dat 

     $OPENFRAME_HOME/license

     [oframe7@ofdemo ~]$ cd $OPENFRAME_HOME/license 
     oframe@oframe7/OpenFrame/license / ls -l 
     -rwxr-xr-x. 1 oframe mqm 80 Sep 12 01:37 licosc.dat 
     -rwxr-xr-x. 1 oframe mqm 80 Sep  8 09:40 lictacf.dat 
     -rwxrwxr-x. 1 oframe mqm 80 Sep  3 11:54 lictjes.da
     ```

9. Başlangıç yukarı ve OSC kapatmak için CICS bölge paylaşılan bellek yazarak başlatın `osctdlinit OSCOIVP1` komut isteminde.

10. Çalıştırma `oscboot` OSC önyüklemesi için. Çıktı aşağıdakine benzer olacaktır:

     ```
     OSCBOOT : pre-processing       [ OK ]

     TMBOOT for node(NODE1) is starting: 
     Welcome to Tmax demo system: it will expire 2016/11/4 
     Today: 2016/9/12 
          TMBOOT: TMM is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLL is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: CLH is starting: Mon Sep 12 01:40:25 2016 
          TMBOOT: TLM(tlm) is starting: Mon Sep 12 01:40:25 2016 
     ```

11. İşlem durumunu hazır olduğunu doğrulamak için `tmadmin` sı komutunu. Tüm işlemler de RDY görüntülenmelidir **durumu** sütun.

    ![İşlemler RDY görüntüleme](media/tmadmin-02.png)

12. Kullanarak OSC kapatma `oscdown` komutu.

## <a name="install-jeus"></a>JEUS yükleyin

Sunu katmanı OpenFrame web uygulama sunucusunun JEUS (Java Enterprise kullanıcı çözümü) sağlar.

JEUS yüklemeden önce JEUS yüklemek için gereken komut satırı araçları ve kitaplıkları sunan Apache Ant paketi yükleyin.

**Apache Ant'ı yüklemek için**

1. Ant yükleme ikili kullanarak `wget` komutu. Örneğin:

     ```
     wget http://apache.mirror.cdnetworks.com/ant/binaries/apacheant-1.9.7-bin.tar.gz
     ```

2. Kullanım `tar` ikili dosyasını ayıklayın ve uygun bir konuma taşımak için yardımcı program. Örneğin:

     ```
     tar -xvzf apache-ant-1.9.7-bin.tar.gz
     ```

3. Verimlilik için sembolik bağlantı oluşturun:

     ```
     ln -s apache-ant-1.9.7 ant
     ```

4. Bash profili içinde olduğu gibi vi açın (`vi .bash_profile`) ve aşağıdaki değişkenleri ile güncelleştirin:

     ```
     # Ant ENV
     export ANT_HOME=$HOME/ant 
     export PATH=$HOME/ant/bin:$PATH
     ```

5.  Değiştirilmiş ortam değişkenini uygulayın. Örneğin:

     ```
     [oframe7\@ofdemo \~]\$ source \~/.bash\_profile
     ```

**JEUS yüklemek için**

1. Kullanarak yükleyici genişletin `tar` yardımcı programı. Örneğin:

     ```
     [oframe7@ofdemo ~]$ tar -zxvf jeus704.tar.gz
     ```

2. Oluşturma bir **jeus** klasörü (`mkdir jeus7`) ve ikili sıkıştırmasını açın.
3. Değiştirin **Kurulum** dizini (veya kendi ortamınıza JEUS parametresini kullanın). Örneğin:

     ```
     [oframe7@ofdemo ~]$ cd jeus7/setup/
     ```

4. Yürütme `ant clean-all` derleme gerçekleştirmeden önce. Çıktı aşağıdakine benzer olacaktır:

     ```
     Buildfile: /home/oframe7jeus7/setup/build.xml

     clean-bin:
     delete-domain:
     [echo] Deleting a domain configuration: domain = jeus_domain
     delete-nodesxml:
     clean-config:
     clean-all:
     BUILD SUCCESSFUL
     Total time: 0 seconds
     ```

5.  Etki alanı yapılandırma template.properties dosyasının yedeğini alın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ cp domain-config-template.properties domain-configtemplate.properties.bkp
     ```

6. Etki alanı yapılandırma template.properties dosya içinde olduğu gibi vi açın:

     ```
     [oframe7\@ofdemo setup]\$ vi domain-config-template.properties
     ```

7. Değişiklik `jeus.password=jeusadmin nodename=Tmaxsoft` için `jeus.password=tmax1234 nodename=ofdemo`

8. Yürütme `ant install` JEUS oluşturmak için komutu.
9.  .bash güncelleştirme\_gösterildiği JEUS değişkenleriyle profili dosyası:

     ```
     # JEUS ENV 
     export JEUS_HOME=/opt/tmaxui/jeus7 PATH="/opt/tmaxui/jeus7/bin:/opt/tmaxui/jeus7/lib/system:/opt/tmaxui/jeus7/webserver/bin:$ {PATH}" 
     export PATH
     ```

10. Bash profili yürütün. Örneğin:

     ```
     [oframe7@ofdemo setup]$ . .bash_profile
     ```

11. *İsteğe bağlı*. Kolay kapatma ve JEUS bileşenlerin önyükleme için bir diğer adı oluşturun:

     ```     
     # JEUS alias

     alias dsboot='startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin'
     alias msboot='startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin' 
     alias msdown=‘jeusadmin -u administrator -p tmax1234 "stop-server server1“’ 
     alias dsdown=‘jeusadmin -domain jeus_domain -u administrator -p tmax1234 "local-shutdown“’
     ```

12. Yüklemeyi doğrulamak için etki alanı yönetim sunucusu gösterildiği gibi başlatın:

     ```
     [oframe7@ofdemo ~]$ startDomainAdminServer -domain jeus_domain -u administrator -p jeusadmin
     ```

13. Söz dizimini kullanarak web oturum açmasına göre doğrulayın:

     ```
     http://<IP>:<port>/webadmin/login
     ```

     Örneğin, <http://192.168.92.133:9736/webadmin/login.> oturum açma ekranı görüntülenir:
    
     ![JEUS WebAdmin oturum açma ekranı](media/jeus-01.png)

     > [!NOTE]
     > Bağlantı noktası güvenliği ile herhangi bir sorun yaşarsanız, bağlantı noktası 9736 açın veya Güvenlik Duvarı'nı devre dışı bırakın (`systemctl stop firewall`).

14. Sunucu1 için ana bilgisayar adını değiştirmek için tıklayın **kilitleme & Düzen**, ardından **Sunucu1**. Sunucu penceresinde, ana bilgisayar adı gibi değiştirin:

    1.  Değişiklik **Nodename** için **ofdemo**.
    2.  Tıklayın **Tamam** penceresinin sağ tarafındaki.
    3.  Tıklayın **Değişiklikleri Uygula** alt sol tarafında penceresinin ve açıklama girin *ana bilgisayar adı değişikliği*.

    ![JEUS WebAdmin ekranı](media/jeus-02.png)

15. Yapılandırma onay ekranında başarılı olduğunu doğrulayın.

    ![jeus_domain sunucuyu ekranı](media/jeus-03.png)

16. Aşağıdaki komutu kullanarak yönetilen sunucu işlemi "SUNUCU1" başlatın:

     ```
     [oframe7@ofdemo ~]$ startManagedServer -domain jeus_domain -server server1 -u administrator -p jeusadmin
     ```

## <a name="install-ofgw"></a>OFGW yükleyin

OFGW 3270 terminal öykünücü OSI temel arasındaki iletişimi destekler ve terminal öykünücüsünü ve OSI arasındaki oturumları yöneten OpenFrame geçididir.

**OFGW yüklemek için**

1. JEUS başarıyla yüklendiğinden emin olun ve ardından doğrulayın OFGW7\_0\_1\_Generic.bin yükleyici dosya mevcut.
2. Yükleyiciyi çalıştırın. Örneğin:

     ```
     [oframe7@ofdemo ~]$ ./OFGW7_0_1_Generic.bin
     ````

3. Aşağıdaki konumlar için karşılık gelen yönergeleri kullanın:
     -   JEUS giriş dizini
     -   JEUS etki alanı adı
     -   JEUS sunucu adı
     -   Tibero sürücü
     -   Ofdemo Tmax düğüm kimliği

4. Varsayılan değerleri kabul edin ve ardından Yükleyiciden çıkmak için Enter tuşuna basın.

5. OFGW URL'sini beklendiği gibi çalıştığını doğrulayın:

     ```
     Type URL 
     http://192.168.92.133:8088/webterminal/ and press enter
      < IP >               :8088/webterminal/
     ```

     Aşağıdaki ekran görünür:

    ![OpenFrame WebTerminal](media/ofgw-01.png)

## <a name="install-ofmanager"></a>OFManager yükleyin

OFManager web ortamında OpenFrame için işlemi ve Yönetimi işlevleri sağlar.

**OFManager yüklemek için**

1. Doğrulayın OFManager7\_Generic.bin yükleyici dosya mevcut.
2. Yükleyiciyi çalıştırın. Örneğin:

     ```
     OFManager7_Generic.bin
     ```

3.  Devam etmek ve lisans sözleşmesini kabul etmek için Enter tuşuna basın.
4.  Yükleme klasörü seçin.
5.  Varsayılanları kabul edin.
6.  Tibero veritabanını seçin.
7.  Yükleyiciden çıkmak için Enter tuşuna basın.
8.  OFManager URL'sini beklendiği gibi çalıştığını doğrulayın:

     ```
     Type URL http://192.168.92.133:8088/ofmanager and press enter <  IP >  : < PORT >  ofmanager Enter ID:   ROOT 
     Password: SYS1
     ```

Başlangıç ekran görünür:

![Tmax OpenFrame Yöneticisi oturum açma ekranı](media/ofmanager-01.png)

OpenFrame bileşenlerin yüklenmesi tamamlanan.

## <a name="next-steps"></a>Sonraki adımlar

Bir ana bilgisayar geçişi düşünüyorsanız, genişleyen iş ortağı ekosistemimiz yardımcı olması kullanılabilir. Bir iş ortağı çözümü seçme hakkında ayrıntılı yönergeler için bkz [Platform Modernizasyonu Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

-   [Azure’ı kullanmaya başlama](https://docs.microsoft.com/azure/)
-   [Host Integration Server (HIS) belgeleri](https://docs.microsoft.com/host-integration-server/)
-   [Azure sanal veri merkezi Lift-and-Shift ile Taşıma Kılavuzu](https://blogs.msdn.microsoft.com/azurecat/2018/03/12/new-whitepaper-azure-virtual-datacenter-lift-and-shift-guide/)
