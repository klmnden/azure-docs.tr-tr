---
title: Yedekleme ve bir Azure Linux sanal makinesinde Oracle Database 12 c veritabanı kurtarma | Microsoft Docs
description: Yedekleme ve kurtarma Azure ortamınızda bir Oracle Database 12 c veritabanı hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: c41f13a6437f69121d3bbb387c96d8e13f2be0b3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60567104"
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Yedekleme ve bir Azure Linux sanal makinesinde Oracle Database 12 c veritabanı kurtarma

Azure CLI, oluşturma ve bir komut isteminde Azure kaynaklarını yönetmek veya betiklerini kullanabilirsiniz. Bu makalede, Azure Market Galerisi görüntüsünden bir Oracle Database 12 c veritabanı dağıtmak için Azure CLI betikleri kullanın.

Başlamadan önce Azure CLI'ın yüklü olduğundan emin olun. Daha fazla bilgi için [Azure CLI yükleme kılavuzundan](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Ortamı hazırlama

### <a name="step-1-prerequisites"></a>1. Adım: Önkoşullar

*   Yedekleme ve kurtarma işlemini gerçekleştirmek için Oracle Database 12 c yüklü örneği olan bir Linux VM oluşturmanız gerekir. VM oluşturmak için kullandığınız Market görüntüsü adlı *Oracle: Oracle-veritabanı-Ee:12.1.0.2:latest*.

    Bir Oracle veritabanına oluşturmayı öğrenmek için bkz: [Oracle veritabanı hızlı başlangıç oluşturmak](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-to-the-vm"></a>2. Adım: VM’ye bağlanma

*   Sanal makine ile bir güvenli Kabuk (SSH) oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresi ve konak adı birlikte değiştirin `publicIpAddress` VM'niz için değer.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a>3. Adım: Veritabanını hazırlama

1.  Bu adım adlı bir VM üzerinde çalıştırılan Oracle örneği (cdb1) sahip olduğunuzu varsayar *myVM*.

    Çalıştırma *oracle* süper kullanıcı kök ve ardından başlatma dinleyicisi:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    The listener supports no services
    The command completed successfully
    ```

2.  (İsteğe bağlı) Veritabanı arşiv günlük modunda olduğundan emin olun:

    ```bash
    $ sqlplus / as sysdba
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG

    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (İsteğe bağlı) İşleme test etmek için bir tablo oluşturun:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Doğrulayın veya yedekleme dosyasının konumunu ve boyutunu değiştirin:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Veritabanını yedeklemek için Oracle kurtarma Yöneticisi'ni (RMAN) kullanın:

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>4. Adım: Linux VM'ler için uygulamayla tutarlı yedekleme

Uygulamayla tutarlı yedeklemeler, Azure Backup, yeni bir özelliktir. Oluşturun ve önce ve sonra VM anlık görüntüsü (anlık görüntü öncesi ve anlık görüntü sonrası) yürütmek için komut dosyaları seçin.

1. JSON dosyasını indirin.

    Gelen VMSnapshotScriptPluginConfig.json indirme https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig. Dosya içeriğini aşağıdakine benzer görünmelidir:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Sanal makinede/etc/Azure klasörü oluşturun:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. JSON dosyasını kopyalayın.

    VMSnapshotScriptPluginConfig.json / etc/Azure klasörüne kopyalayın.

4. JSON dosyasını düzenleyin.

    Dahil edilecek VMSnapshotScriptPluginConfig.json dosyası Düzenle `PreScriptLocation` ve `PostScriptlocation` parametreleri. Örneğin:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Anlık görüntü öncesi ve anlık görüntü sonrası komut dosyalarını oluşturun.

    İşte bir örnek "soğuk yedekleme" anlık görüntü öncesi ve anlık görüntü sonrası betikler (bir çevrimiçi yedekleme, kapatma ve yeniden başlatma ile):

    /Etc/Azure/pre_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    /Etc/Azure/post_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    İşte bir örnek "Sık erişimli yedekleme" anlık görüntü öncesi ve anlık görüntü sonrası betikler (çevrimiçi yedekleme):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    /Etc/Azure/post_script.sh için:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    Gereksinimlerinize göre dosyasının içeriğini /etc/Azure/pre_script.SQL için değiştirin:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    Gereksinimlerinize göre dosyasının içeriğini /etc/Azure/post_script.SQL için değiştirin:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Dosya izinleri değiştirin:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Test betikleri.

    Test betikleri için ilk olarak, kök olarak oturum açın. Ardından, hiçbir hata olmadığından emin olun:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

Daha fazla bilgi için [Linux VM'ler için uygulamayla tutarlı yedekleme](https://azure.microsoft.com/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a>5. Adım: Sanal makineyi yedeklemek için kullanım Azure kurtarma Hizmetleri kasaları

1.  Azure portalında arama **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri kasaları sayfasında](./media/oracle-backup-recovery/recovery_service_01.png)

2.  Üzerinde **kurtarma Hizmetleri kasaları** yeni bir kasa eklemek için dikey **Ekle**.

    ![Kurtarma Hizmetleri kasaları Sayfası Ekle](./media/oracle-backup-recovery/recovery_service_02.png)

3.  Devam etmek için tıklayın **myVault**.

    ![Kurtarma Hizmetleri kasaları Ayrıntı Sayfası](./media/oracle-backup-recovery/recovery_service_03.png)

4.  Üzerinde **myVault** dikey penceresinde tıklayın **yedekleme**.

    ![Kurtarma Hizmetleri kasaları sayfasında yedekleme](./media/oracle-backup-recovery/recovery_service_04.png)

5.  Üzerinde **yedekleme hedefi** dikey penceresinde varsayılan değerleri kullanmak **Azure** ve **sanal makine**. **Tamam** düğmesine tıklayın.

    ![Kurtarma Hizmetleri kasaları Ayrıntı Sayfası](./media/oracle-backup-recovery/recovery_service_05.png)

6.  İçin **yedekleme İlkesi**, kullanın **DefaultPolicy**, ya da seçin **oluşturma yeni ilke**. **Tamam** düğmesine tıklayın.

    ![Kurtarma Hizmetleri kasaları yedekleme İlkesi Ayrıntı Sayfası](./media/oracle-backup-recovery/recovery_service_06.png)

7.  Üzerinde **sanal makineleri** dikey penceresinde seçin **myVM1** onay kutusunu işaretleyin ve ardından **Tamam**. Tıklayın **yedeklemeyi etkinleştir** düğmesi.

    ![Kurtarma Hizmetleri kasaları öğelerini yedekleme Ayrıntı Sayfası](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Tıkladıktan sonra **yedeklemeyi etkinleştir**, yedekleme işlemi zamanlanan süreden süresi dolana kadar başlamaz. Hemen bir yedekleme ayarlamak için sonraki adımda tamamlayın.

8.  Üzerinde **myVault - yedekleme öğeleri** dikey altında **yedekleme öğesi sayısı**, yedekleme öğesi sayısı seçin.

    ![Kurtarma Hizmetleri kasaları myVault Ayrıntı Sayfası](./media/oracle-backup-recovery/recovery_service_08.png)

9.  Üzerinde **yedekleme öğeleri (Azure sanal makine)** dikey penceresinde sayfanın sağ tarafındaki üç nokta simgesine tıklayın (**...** ) düğmesini ve ardından **Şimdi Yedekle**.

    ![Kurtarma Hizmetleri kasaları yedekleme artık komutu](./media/oracle-backup-recovery/recovery_service_09.png)

10. Tıklayın **yedekleme** düğmesi. Yedekleme işleminin tamamlanması için bekleyin. Ardından, Git [adım 6: Veritabanı dosyalarını kaldırma](#step-6-remove-the-database-files).

    Yedekleme işinin durumunu görüntülemek için tıklayın **işleri**.

    ![Kurtarma Hizmetleri kasaları sayfasında iş](./media/oracle-backup-recovery/recovery_service_10.png)

    Yedekleme işinin durumunu aşağıda görünür:

    ![Kurtarma Hizmetleri kasaları iş durumu sayfası](./media/oracle-backup-recovery/recovery_service_11.png)

11. Uygulamayla tutarlı bir yedekleme için günlük dosyasındaki hataları giderir. Günlük dosyası /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0 bulunur.

### <a name="step-6-remove-the-database-files"></a>6. Adım: Veritabanı dosyalarını Kaldır 
Bu makalede kurtarma işlemini test öğreneceksiniz. Kurtarma işlemini test edebilmek için önce veritabanı dosyalarını kaldırmak zorunda.

1.  Belirtilmedi ve yedekleme dosyalarını kaldırın:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (İsteğe bağlı) Oracle örneğini kapatın:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a>Kurtarma Hizmetleri kasaları silinen dosyaları geri yükleme
Silinen dosyaları geri yüklemek için aşağıdaki adımları tamamlayın:

1. Azure portalında arama *myVault* kurtarma Hizmetleri kasaları öğesi. Üzerinde **genel bakış** dikey altında **yedekleme öğeleri**, öğe sayısını seçin.

    ![Kurtarma Hizmetleri kasaları myVault yedekleme öğeleri](./media/oracle-backup-recovery/recovery_service_12.png)

2. Altında **yedekleme öğesi sayısı**, öğe sayısını seçin.

    ![Kurtarma Hizmetleri kasaları Azure sanal makine yedekleme öğesi sayısı](./media/oracle-backup-recovery/recovery_service_13.png)

3. Üzerinde **myvm1** dikey penceresinde tıklayın **dosya kurtarma (Önizleme)**.

    ![Dosya Kurtarma sayfası ekran görüntüsü kurtarma Hizmetleri kasaları](./media/oracle-backup-recovery/recovery_service_14.png)

4. Üzerinde **dosya kurtarma (Önizleme)** bölmesinde tıklayın **betiği indirin**. Ardından, dosyayı indirin (.sh), istemci bilgisayarda bir klasöre kaydedin.

    ![Yükleme komut dosyası kaydetme seçenekleri](./media/oracle-backup-recovery/recovery_service_15.png)

5. VM'ye .sh dosyasını kopyalayın.

    Aşağıdaki örnekte, nasıl dosyayı VM'ye taşımak için güvenli kopya (scp) kullanmak için komut gösterilmektedir. İçeriği panoya kopyalayabilirsiniz ve ardından VM üzerinde ayarlanmış yeni bir dosya içeriği yapıştırın.

    > [!IMPORTANT]
    > Aşağıdaki örnekte, IP adresi ve klasör değerler güncelleştirdiğinizden emin olun. Değerler dosyasının kaydedildiği klasöre eşlemelisiniz.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Dosyanın kök tarafından sahip olunan şekilde değiştirin.

    Aşağıdaki örnekte, kök tarafından sahip olunan dosyayı değiştirin. Ardından, izinleri değiştirin.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    Aşağıdaki örnek, önceki komut dosyasını çalıştırın, sonra görmeniz gereken gösterir. Devam etmek isteyip istemediğiniz sorulduğunda, girin **Y**.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. Bağlı birimleri erişimi onaylandı.

    Çıkmak için girin **q**ve sonra bağlı birimleri için arama yapın. Bir komut isteminde eklenen birimlerin listesini oluşturmak için girin **SD -k**.

    ![The df -k command](./media/oracle-backup-recovery/recovery_service_16.png)

8. Eksik dosyaları klasöre kopyalamak için aşağıdaki betiği kullanın:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. Aşağıdaki betikte RMAN veritabanını kurtarmak için kullanın:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Diski çıkarın.

    Azure portalında, üzerinde **dosya kurtarma (Önizleme)** dikey penceresinde tıklayın **diskleri çıkar**.

    ![Diskleri komutunu çıkarın](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a>Tüm VM'yi geri yükle

Kurtarma Hizmetleri kasaları silinen dosyaları geri yükleme yerine, tüm VM'yi geri yükleyebilirsiniz.

### <a name="step-1-delete-myvm"></a>1. Adım: MyVM Sil

*   Azure portalında Git **myVM1** kasaya ve ardından **Sil**.

    ![Kasayı Sil komutu](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a>2. Adım: Recover SQL

1.  Git **kurtarma Hizmetleri kasaları**ve ardından **myVault**.

    ![myVault giriş](./media/oracle-backup-recovery/recover_vm_02.png)

2.  Üzerinde **genel bakış** dikey altında **yedekleme öğeleri**, öğe sayısını seçin.

    ![Yedekleme öğeleri myVault](./media/oracle-backup-recovery/recover_vm_03.png)

3.  Üzerinde **yedekleme öğeleri (Azure sanal makine)** dikey penceresinde **myvm1**.

    ![Kurtarma VM sayfası](./media/oracle-backup-recovery/recover_vm_04.png)

4.  Üzerinde **myvm1** dikey penceresinde, üç noktaya tıklayın (**...** ) düğmesini ve ardından **VM geri yükleme**.

    ![VM komutu geri yükleme](./media/oracle-backup-recovery/recover_vm_05.png)

5.  Üzerinde **seçin, geri yükleme noktası** dikey penceresinde, geri yüklemek istediğiniz öğeyi seçin ve ardından **Tamam**.

    ![Geri yükleme noktası seçin](./media/oracle-backup-recovery/recover_vm_06.png)

    Uygulamayla tutarlı yedekleme etkinleştirilirse, mavi dikey çubuk görünür.

6.  Üzerinde **geri yükleme Yapılandırması** dikey penceresinde sanal makine adını seçin, kaynak grubunu seçin ve ardından **Tamam**.

    ![Yapılandırma değerlerini geri yükleme](./media/oracle-backup-recovery/recover_vm_07.png)

7.  VM'yi geri yüklemek için **geri** düğmesi.

8.  Geri yükleme işleminin durumunu görüntülemek için tıklayın **işleri**ve ardından **yedekleme işleri**.

    ![Yedekleme işlerinin durumunu komutu](./media/oracle-backup-recovery/recover_vm_08.png)

    Geri yükleme işleminin durumunu aşağıdaki şekilde gösterilmiştir:

    ![Geri yükleme işleminin durumu](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a>3. Adım: Genel IP adresini ayarlama
Sanal makine geri yüklendikten sonra genel IP adresini ayarlayın.

1.  Arama kutusuna **genel IP adresi**.

    ![Genel IP adresleri listesi](./media/oracle-backup-recovery/create_ip_00.png)

2.  Üzerinde **genel IP adresleri** dikey penceresinde tıklayın **Ekle**. Üzerinde **genel IP adresi oluşturma** dikey penceresinde için **adı**, genel IP adı seçin. **Kaynak grubu** olarak **Var olanı kullan**’ı seçin. Sonra, **Oluştur**’a tıklayın.

    ![IP adresi oluşturma](./media/oracle-backup-recovery/create_ip_01.png)

3.  Genel IP adresini sanal makine için ağ arabirimi ile ilişkilendirmek için aramak ve seçmek **myVMip**. ' A tıklayarak **ilişkilendirmek**.

    ![IP adresi ilişkilendirme](./media/oracle-backup-recovery/create_ip_02.png)

4.  İçin **kaynak türü**seçin **ağ arabirimi**. MyVM örneği tarafından kullanılan ağ arabirimi seçin ve ardından **Tamam**.

    ![Kaynak türü ve NIC değerleri seçin](./media/oracle-backup-recovery/create_ip_03.png)

5.  Arayın ve Portalı'ndan verilir myVM örneği açın. VM ile ilişkili IP adresi üzerinde myVM görünür **genel bakış** dikey penceresi.

    ![IP adresi değeri](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a>4. Adım: VM’ye bağlanma

*   VM'ye bağlanmak için aşağıdaki betiği kullanın:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a>5. Adım: Veritabanı erişilebilir olup olmadığını test edin
*   Erişilebilirlik test etmek için aşağıdaki betiği kullanın:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Veritabanı **başlangıç** komutu bir hata oluşturur, veritabanını kurtarmak için bkz: [adım 6: Veritabanını kurtarmak için RMAN kullanın](#step-6-optional-use-rman-to-recover-the-database).

### <a name="step-6-optional-use-rman-to-recover-the-database"></a>6. Adım: (İsteğe bağlı) Veritabanını kurtarmak için RMAN kullanın
*   Veritabanını kurtarmak için aşağıdaki betiği kullanın:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

Yedekleme ve kurtarma Azure Linux sanal makinesinde Oracle Database 12c veritabanı artık tamamlandı.

## <a name="delete-the-vm"></a>VM’yi silin

VM artık ihtiyacınız olmadığında kaynak grubunu, VM'yi ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Yüksek oranda kullanılabilir VM'ler oluşturma](../../linux/create-cli-complete.md)

[VM dağıtımı Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)



