---
title: "Bir Linux VM üzerinde PostgreSQL ayarlama | Microsoft Docs"
description: "Yükleme ve PostgreSQL azure'da bir Linux sanal makine yapılandırma hakkında bilgi edinin"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 1a747363-0cc5-4ba3-9be7-084dfeb04651
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 0bccdc1cfdbda06b57da8cd662373ef137768672
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-configure-postgresql-on-azure"></a>Azure’da PostgreSQL yükleme ve yapılandırma
PostgreSQL Oracle ve DB2 için benzer bir Gelişmiş açık kaynak veritabanı yok. Tam ACID uyumluluk, güvenilir işlem işleme ve çoklu sürüm eşzamanlılık denetimi gibi Kurumsal kullanıma hazır özellikler içerir. Ayrıca, ANSI SQL ve SQL/MED (Oracle, MySQL, MongoDB ve diğer birçok için yabancı veri sarmalayıcıları dahil) gibi standartlara destekler. Desteğiyle üzerinde 12 yordam diller, GIN ve GiST dizinleri, uzamsal veri desteği ve birden çok NoSQL benzeri özellikleri JSON veya anahtar-değer tabanlı uygulamalar için yüksek oranda genişletilebilir.

Bu makalede, yüklemek ve bir Azure Linux çalıştıran sanal makine üzerinde PostgreSQL yapılandırmak öğreneceksiniz.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-postgresql"></a>PostgreSQL yükleyin
> [!NOTE]
> Bu öğreticiyi tamamlamak için Linux çalıştıran bir Azure sanal makinesi zaten olmalıdır. Oluşturma ve devam etmeden önce bir Linux VM ayarlamak için bkz: [Azure Linux VM'de Öğreticisi](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

Bu durumda, bağlantı noktası 1999 PostgreSQL bağlantı noktası olarak kullanın.  

PuTTY üzerinden oluşturulan VM Linux bağlayın. Bir Azure Linux VM kullanmakta olduğunuz ilk kez kullanıyorsanız bkz [nasıl kullanmak ile SSH Linux Azure üzerinde](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bir Linux VM bağlanmak için PuTTY kullanın öğrenmek için.

1. (Yönetici) köküne geçmek için aşağıdaki komutu çalıştırın:
   
        # sudo su -
2. Bazı dağıtımları PostgreSQL yüklemeden önce yüklemelisiniz bağımlılıkları vardır. Bu listede, distro denetlemek ve uygun komutu çalıştırın:
   
   * Red Hat temel Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian temel Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Kök dizinine PostgreSQL yükleyin ve paketin sıkıştırmasını açın:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Yukarıdaki örnek verilebilir. Daha ayrıntılı indirme adresi bulabilirsiniz [dizin/pub programlarının/kaynak /](https://ftp.postgresql.org/pub/source/).
4. Yapı başlatmak için şu komutları çalıştırın:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Her şeyi içeren belgeleri (HTML ve ADAM sayfaları) ve ek modülleri (contrib) dahil olmak üzere oluşturulabilir oluşturmak istiyorsanız bunun yerine aşağıdaki komutu çalıştırın:
   
        # gmake install-world
   
    Aşağıdaki onay iletisini almanız gerekir:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL yapılandırın
1. (İsteğe bağlı) Sürüm numarasını içermeyecek şekilde PostgreSQL başvuru kısaltmak için sembolik bağlantı oluşturun:
   
        # ln -s /opt/pgsql9.3.5 /opt/pgsql
2. Veritabanı için bir dizin oluşturun:
   
        # mkdir -p /opt/pgsql_data
3. Kök olmayan kullanıcı oluşturun ve o kullanıcının profilini değiştirin. Ardından, bu yeni bir kullanıcıya geçiş (adlı *postgres* örneğimizde):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Güvenlik nedenleriyle, PostgreSQL kök olmayan kullanıcı başlatmak, başlatma veya veritabanı kapatmak için kullanır.
   > 
   > 
4. Düzen *bash_profile* aşağıdaki komutları girerek dosya. Bu satırlar sonuna eklenecek *bash_profile* dosyası:
   
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
6. Aşağıdaki komutu kullanarak yüklemenizi doğrulama:
   
        $ which psql
   
    Yükleme başarılı olursa, aşağıdaki yanıt görürsünüz:
   
        /opt/pgsql/bin/psql
7. Ayrıca, PostgreSQL sürüm kontrol edebilirsiniz:
   
        $ psql -V
8. Veritabanını başlatılamadı:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Aşağıdaki çıkış almanız gerekir:

![Görüntü](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL ayarlayın
<!--    [postgres@ test ~]$ exit -->

Aşağıdaki komutları çalıştırın:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

/Etc/init.d/postgresql dosyasındaki iki değişkenleri değiştirin. Öneki PostgreSQL yükleme konumunu ayarlayın: **/opt/pgsql**. PGDATA PostgreSQL veri depolama yoluna ayarlayın: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![Görüntü](./media/postgresql-install/no2.png)

Yürütülebilir yapabilmek için dosyayı değiştirin:

    # chmod +x /etc/init.d/postgresql

PostgreSQL başlatın:

    # /etc/init.d/postgresql start

PostgreSQL uç noktası üzerinde olup olmadığını kontrol edin:

    # netstat -tunlp|grep 1999

Şu çıktı görmeniz gerekir:

![Görüntü](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Postgres veritabanına bağlan
Postgres kullanıcıya bir kez daha anahtarı:

    # su - postgres

Postgres veritabanı oluşturun:

    $ createdb events

Olayları veritabanına bağlan:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Oluşturun ve Postgres tablo silme
Veritabanına bağlı, tablo içinde oluşturabilirsiniz.

Örneğin, aşağıdaki komutu kullanarak yeni bir örnek Postgres tablo oluşturun:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Aşağıdaki sütun adları ve kısıtlamaları ile dört sütunlu bir tablo şimdi ayarlamış olduğunuz:

1. "Name" sütun altında 20 karakter uzunluğunda olacak şekilde VARCHAR komutu tarafından sınırlıdır.
2. "Yemek" sütun herkes getirecek yemek öğesi belirtir. VARCHAR altında 30 karakter olması için bu metni sınırlar.
3. "Onaylanan" sütun, kişinin için Yemeğini Getir Partisi RSVP'd olup olmadığını kaydeder. Kabul edilebilir değerler "Y" ve "N" dir.
4. Bunlar olayı için kaydolurken "tarih" sütun gösterir. Tarih yyyy-aa-gg yazılması Postgres gerektirir

Tablonuz başarıyla oluşturulduysa aşağıdakilere bakın:

![Görüntü](./media/postgresql-install/no4.png)

Aşağıdaki komutu kullanarak, tablo yapısı de denetleyebilirsiniz:

![Görüntü](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Bir tabloya veri ekleme
İlk olarak, bilgileri bir satıra Ekle:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Bu çıktı görmeniz gerekir:

![Görüntü](./media/postgresql-install/no6.png)

Birkaç daha fazla kişinin tabloya ekleyebilirsiniz. İşte birkaç seçenek veya kendi oluşturabilirsiniz:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabloları Göster
Bir tablo göstermek için aşağıdaki komutu kullanın:

    select * from potluck;

Çıktısı şöyledir:

![Görüntü](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Bir tablodaki verileri silme
Bir tablodaki verileri silmek için aşağıdaki komutu kullanın:

    delete from potluck where name=’John’;

"John" satırdaki tüm bilgileri siler. Çıktısı şöyledir:

![Görüntü](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Bir tablodaki verileri güncelleştirme
Bir tablodaki verileri güncelleştirmek için aşağıdaki komutu kullanın. Biz "Y" için "N" den kendi RSVP değiştirecek şekilde aynen katılan olmadığını, bu biri için Sandy onayladığından:

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>PostgreSQL hakkında daha fazla bilgi alın
Bir Azure Linux VM'de PostgreSQL yüklemesini tamamladığınıza göre Azure'da kullanarak keyfini çıkarabilirsiniz. PostgreSQL hakkında daha fazla bilgi için [PostgreSQL Web sitesi](http://www.postgresql.org/).

