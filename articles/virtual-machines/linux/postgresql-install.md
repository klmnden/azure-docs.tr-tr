---
title: Bir Linux sanal makinesi üzerinde PostgreSQL ayarlama | Microsoft Docs
description: Yükleme ve azure'da bir Linux sanal makinesi üzerinde PostgreSQL yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 086b36b347f214e1e9cdf44e4fb5a29fe501fa8b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667114"
---
# <a name="install-and-configure-postgresql-on-azure"></a>Azure’da PostgreSQL yükleme ve yapılandırma
PostgreSQL için Oracle ve DB2 benzer bir Gelişmiş açık kaynak veritabanı ' dir. Bu, tam ACID uyumluluk ve güvenilir bir işlem tabanlı işleme çok sürümlü eşzamanlılık denetimi gibi Kurumsal kullanıma hazır özellikler içerir. Ayrıca, ANSI SQL ve SQL/MED (Oracle, MySQL, MongoDB ve diğer birçok için yabancı veri sarmalayıcıları dahil) gibi standartları destekler. 12'den yordam diller, GIN ve GiST dizinleri, uzamsal veri desteği ve birden çok NoSQL benzeri özellikler için destek JSON veya anahtar-değer tabanlı uygulamalar için yüksek oranda genişletilebilir.

Bu makalede, yükleme ve Linux çalıştıran Azure sanal makinesinde PostgreSQL yapılandırma öğreneceksiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>PostgreSQL yükleme
> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Azure sanal makinesi zaten olmalıdır. Oluşturma ve devam etmeden önce bir Linux VM ayarlamak için bkz: [Azure Linux VM öğretici](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

Bu durumda, bağlantı noktası 1999 PostgreSQL bağlantı noktası olarak kullanın.  

PuTTY üzerinden oluşturulan VM Linux bağlanın. Bu bir Azure Linux VM kullandığınız ilk kez olup olmadığını [nasıl azure'da Linux ile SSH kullanma](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bir Linux VM'ye bağlanmak için PuTTY kullanma hakkında bilgi edinmek için.

1. (Yönetici) köküne geçmek için aşağıdaki komutu çalıştırın:
   
        # sudo su -
2. Bazı dağıtımlar, PostgreSQL yüklemeden önce yüklemelisiniz bağımlılıkları vardır. Bu listede, distro denetlemek ve uygun komutu çalıştırın:
   
   * Red Hat taban Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian taban Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. PostgreSQL kök dizine indirin ve ardından paketi sıkıştırmasını açın:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Yukarıdaki örnek verilebilir. Ayrıntılı yükleme adresini bulabilirsiniz [/pub programlarının/kaynak / dizin](https://ftp.postgresql.org/pub/source/).
4. Derlemeyi başlatmak için şu komutları çalıştırın:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Belge (HTML ve ADAM sayfaları) ve ek modüller (contrib) dahil olmak üzere oluşturulabilir her şeyi oluşturmak istiyorsanız aşağıdaki komutu yerine çalıştırın:
   
        # gmake install-world
   
    Şu onaylama iletisini almanız gerekir:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL yapılandırın
1. (İsteğe bağlı) Sürüm numarasını içermeyecek şekilde PostgreSQL başvuru kısaltmak için sembolik bir bağlantısını oluşturun:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Veritabanı için bir dizin oluşturun:
   
        # mkdir -p /opt/pgsql_data
3. Kök olmayan kullanıcı bu kullanıcının profili oluşturup değiştirmek. Ardından, bu yeni bir kullanıcıya geçiş (adlı *postgres* örneğimizde):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Güvenlik nedenleriyle, PostgreSQL kök olmayan kullanıcı başlatmak, başlatma veya kapatma veritabanı için kullanır.
   > 
   > 
4. Düzen *bash_profile* aşağıdaki komutları girerek dosya. Bu satır sonuna kadar eklenecektir *bash_profile* dosyası:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Yürütme *bash_profile* dosyası:
   
        $ source .bash_profile
6. Aşağıdaki komutu kullanarak yüklemenizi doğrulayın:
   
        $ which psql
   
    Yükleme işleminiz başarılı olursa, şu yanıtı görürsünüz:
   
        /opt/pgsql/bin/psql
7. PostgreSQL sürümü de göz atabilirsiniz:
   
        $ psql -V

8. Veritabanı başlatın:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Aşağıdaki çıktıyı almalısınız:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL ayarlayın
<!--    [postgres@ test ~]$ exit -->

Aşağıdaki komutları çalıştırın:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

/Etc/init.d/postgresql dosyasında iki değişkenleri değiştirin. Önek PostgreSQL yükleme yoluna ayarlanır: **/opt/pgsql**. PGDATA PostgreSQL veri depolama yoluna ayarlanmış: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Dosyayı yürütülebilir hale getirmek için değiştirin:

    # chmod +x /etc/init.d/postgresql

PostgreSQL başlatın:

    # /etc/init.d/postgresql start

Üzerinde PostgreSQL uç noktası olup olmadığını kontrol edin:

    # netstat -tunlp|grep 1999

Aşağıdaki çıktıyı görmeniz gerekir:

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Postgres veritabanı'na bağlanma
Postgres kullanıcıya yeniden anahtarı:

    # su - postgres

Postgres veritabanı oluşturun:

    $ createdb events

Yeni oluşturduğunuz olayları veritabanına bağlan:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Oluşturma ve Postgres tablo silme
Veritabanına bağlandıktan sonra içinde tablolar oluşturabilirsiniz.

Örneğin, aşağıdaki komutu kullanarak yeni bir örnek Postgres tablo oluşturun:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Artık aşağıdaki sütun adları ve kısıtlamalar dört sütunlu bir tablo ayarladıysanız:

1. "Name" sütunu altındaki 20 karakter uzunluğunda olacak şekilde VARCHAR komutu tarafından sınırlıdır.
2. "Yemek" sütunu her kişi getirecek Gıda öğeyi belirtir. VARCHAR logoyu 30 karakter olması için bu metni sınırlar.
3. "Onaylandı" sütun, kişinin Yemeğini Getir için Partisi yanıtladığı kaydeder. Kabul edilebilir değerler şunlardır: "Y" ve "N".
4. Olay için RMS'ye kaydolurken "tarih" sütun gösterir. Tarih yyyy-aa-gg yazılması Postgres gerektirir

Tablonuzu başarıyla oluşturulduysa aşağıdaki görmeniz gerekir:

![image](./media/postgresql-install/no4.png)

Ayrıca, aşağıdaki komutu kullanarak tablo yapısı denetleyebilirsiniz:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Tabloya veri ekleme
İlk olarak, bilgileri bir satır ekleyin:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Bu bir çıktı görmeniz gerekir:

![image](./media/postgresql-install/no6.png)

Birkaç kişi tabloya ekleyebilirsiniz. İşte bazı seçenekler veya kendi oluşturabilirsiniz:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabloları Göster
Bir tablo göstermek için aşağıdaki komutu kullanın:

    select * from potluck;

Çıktı.

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Bir tablodaki verileri Sil
Bir tablodaki verileri silmek için aşağıdaki komutu kullanın:

    delete from potluck where name=’John’;

Bu, tüm bilgileri "John" satır siler. Çıktı.

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Bir tablodaki verileri güncelleştirme
Bir tablodaki verileri güncelleştirmek için aşağıdaki komutu kullanın. "Y" öğesinden "N" Davetiyeyi değiştireceğiz. Bu nedenle bunlar katılan olduğunu, bu biri için Kumlu onaylamıştır:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>PostgreSQL hakkında daha fazla bilgi edinin
Azure Linux VM'de PostgreSQL yüklemesini tamamladığınızda, Azure'da kullanmaya keyfini çıkarabilirsiniz. PostgreSQL hakkında daha fazla bilgi edinmek için [PostgreSQL Web sitesi](https://www.postgresql.org/).

