---
title: "Bir Azure Linux VM'de Oracle Golden kapısı uygulamak | Microsoft Docs"
description: "Hızla bir Oracle Golden kapısı yukarı ve Azure ortamınızda çalışan alın."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: c99023d794dfb3b78b26ef721d89302e126f5cb1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Bir Azure Linux VM'de Oracle Golden kapısı uygulama 

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda Azure Market galeri görüntüsü 12 c Oracle veritabanından dağıtmak için Azure CLI kullanma ayrıntıları verilmektedir. 

Bu belge, adım adım oluşturmak, yükleme ve Oracle Golden kapısı Azure VM temelinde yapılandırma gösterilmektedir.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-the-environment"></a>Ortamı hazırlama

Oracle Golden kapısı yüklemeyi gerçekleştirmek için aynı kullanılabilirlik kümesinde iki Azure sanal makineleri oluşturmanız gerekir. Sanal makineleri oluşturmak için kullandığınız Market görüntü **Oracle: Oracle-veritabanı-Ee:12.1.0.2:latest**.

Ayrıca UNIX Düzenleyicisi VI ile ilgili bilgi sahibi olmanız ve x11 (X Windows) temel bir anlayış gerekir.

Ortam yapılandırma özetini verilmiştir:
> 
> |  | **Birincil site** | **Çoğaltma sitesi** |
> | --- | --- | --- |
> | **Oracle Sürüm** |Oracle 12c sürüm 2 – (12.1.0.2) |Oracle 12c sürüm 2 – (12.1.0.2)|
> | **Makine adı** |myVM1 |myVM2 |
> | **İşletim Sistemi** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle SID** |CDB1 |CDB1 |
> | **Çoğaltma şeması** |TEST|TEST |
> | **Golden kapısı sahibi/çoğaltılır** |C##GGADMIN |REPUSER |
> | **Golden kapısı işlemi** |EXTORA |REPORA|


### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

Oturum açtığınızda, Azure aboneliğinizle [az oturum açma](/cli/azure/reference-index#az_login) komutu. Ardından izleyin ekrandaki yönergeleri.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Bir Azure kaynak grubu bir mantıksal hangi Azure kaynaklarını dağıtılan içine ve hangi bunlar yönetilebilir bir kapsayıcısıdır. 

Aşağıdaki örnek `westus` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Aşağıdaki isteğe bağlı ancak önerilen bir adımdır. Daha fazla bilgi için bkz: [Azure kullanılabilirlik kümeleri Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#az_vm_create) komutuyla bir sanal makine oluşturun. 

Aşağıdaki örnek adlı iki VM'ler oluşturur `myVM1` ve `myVM2`. Zaten bir varsayılan anahtar konumda yoksa, SSH anahtarları oluşturma. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

#### <a name="create-myvm1-primary"></a>MyVM1 (birincil) oluşturun:
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

VM oluşturulduktan sonra Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir. (Not edin `publicIpAddress`. Bu adresi VM erişmek için kullanılır.)

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

#### <a name="create-myvm2-replicate"></a>MyVM2 oluşturun (Çoğaltma):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Not edin `publicIpAddress` de oluşturulduktan sonra.

### <a name="open-the-tcp-port-for-connectivity"></a>Bağlantı için TCP bağlantı noktasını açın

Sonraki adım, Oracle veritabanı uzaktan erişim sağlayan dış uç noktalar yapılandırmaktır. Dış uç noktalar yapılandırmak için aşağıdaki komutları çalıştırın.

#### <a name="open-the-port-for-myvm1"></a>MyVM1 için bağlantı noktası açın:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

Sonuçları aşağıdaki yanıta benzer görünmelidir:

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

#### <a name="open-the-port-for-myvm2"></a>MyVM2 için bağlantı noktası açın:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin `publicIpAddress` ile değiştirin.

```bash 
ssh <publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a>Veritabanı üzerinde myVM1 (birincil) oluşturma

Veritabanını yüklemek için sonraki adım olacak şekilde Oracle yazılım Market görüntüsü üzerinde zaten yüklü. 

Yazılım 'oracle' süper kullanıcı çalıştırın:

```bash
sudo su - oracle
```

Veritabanı oluştur:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Çıktı aşağıdaki yanıta benzer görünmelidir:

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

ORACLE_SID ve ORACLE_HOME değişkenleri ayarlayın.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

İsteğe bağlı olarak, böylece bu ayarlar gelecekteki oturum açma işlemleri için kaydedilir .bashrc dosyasına ORACLE_HOME ve ORACLE_SID ekleyebilirsiniz:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle dinleyicisini başlatmak
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-the-database-on-myvm2-replicate"></a>MyVM2 üzerinde veritabanı oluşturma (Çoğaltma)

```bash
sudo su - oracle
```
Veritabanı oluştur:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
ORACLE_SID ve ORACLE_HOME değişkenleri ayarlayın.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Böylece bu ayarlar gelecekteki oturum açma işlemleri için kaydedilir isteğe bağlı olarak, eklenen ORACLE_HOME ve ORACLE_SID .bashrc dosyasına kullanabilirsiniz.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Oracle dinleyicisini başlatmak
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Golden kapısı yapılandırın 
Golden kapısı yapılandırmak için bu bölümdeki adımları uygulayın.

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>MyVM1 (birincil) arşiv günlük modunu etkinleştir

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
```
Zorla günlüğe yazılmasını etkinleştirmek ve en az bir günlük dosyası bulunduğundan emin olun.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Golden kapısı yazılımları indirme
Karşıdan yüklemek ve Oracle Golden kapısı yazılımlarını hazırlamak için aşağıdaki adımları tamamlayın:

1. Karşıdan **fbo_ggs_Linux_x64_shiphome.zip** dosya [Oracle Golden kapısı indirme sayfası](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html). İndirme başlığı altında **Oracle Linux x86-64 için Oracle GoldenGate 12.x.x.x**, .zip dosyalarını indirmek için bir dizi olmalıdır.

2. İstemci bilgisayarınız .zip dosyaları indirdikten sonra güvenli kopyalama Protokolü (SCP) VM'nize dosyaları kopyalamak için kullanın:

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. .Zip dosyalarını taşımak **/ opt** klasör. Ardından dosyaları sahibi aşağıdaki gibi değiştirin:

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. (Yükleme Linux sıkıştırmasını yardımcı programı henüz yüklü değilse) dosyaları sıkıştırmasını açın:

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. Değiştirme izni:

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a>İstemci ve VM x11 (yalnızca Windows istemcileri için) çalıştırmak için hazırlama
Bu isteğe bağlı bir adımdır. Bir Linux istemcisi kullanarak veya x11 zaten varsa bu adımı atlayabilirsiniz kurulumu.

1. PuTTY ve Xming Windows bilgisayarınıza indirin:

  * [PuTTY indirin](http://www.putty.org/)
  * [Xming indirin](https://xming.en.softonic.com/)

2.  PuTTY, PuTTY klasöründe (örneğin, C:\Program Files\PuTTY) yükledikten sonra puttygen.exe (PuTTY anahtar Oluşturucu) çalıştırın.

3.  PuTTY anahtar oluşturma Aracı'nda:

  - Bir anahtar oluşturmak için seçin **Generate** düğmesi.
  - Anahtar içeriğini kopyalayın (**Ctrl + C**).
  - Seçin **özel anahtarı Kaydet** düğmesi.
  - Görünür ve ardından uyarı Yoksay **Tamam**.

    ![PuTTY anahtarı Oluşturucu sayfasının ekran görüntüsü](./media/oracle-golden-gate/puttykeygen.png)

4.  VM'nizi, şu komutları çalıştırın:

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. Adlı bir dosya oluşturun **authorized_keys**. Bu dosyada anahtar içeriğini yapıştırın ve ardından dosyayı kaydedin.

  > [!NOTE]
  > Anahtar dize içermelidir `ssh-rsa`. Ayrıca, anahtar içeriğini tek satırlık metin olmalıdır.
  >  

6. PuTTY’yi başlatın. İçinde **kategori** bölmesinde, **bağlantı** > **SSH** > **Auth**. İçinde **kimlik doğrulama için özel anahtar dosyası** kutusunda, daha önce oluşturulan anahtara göz atın.

  ![Özel anahtarı ayarlama sayfasının ekran görüntüsü](./media/oracle-golden-gate/setprivatekey.png)

7. İçinde **kategori** bölmesinde, **bağlantı** > **SSH** > **X11**. Ardından **X11 Enable iletimi** kutusu.

  ![Etkinleştirme X11 sayfasının ekran görüntüsü](./media/oracle-golden-gate/enablex11.png)

8. İçinde **kategori** bölmesinde, Git **oturum**. Ana bilgisayar bilgilerini girin ve ardından **açık**.

  ![Oturum sayfasının ekran görüntüsü](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Golden kapısı yazılımı yükleyin

Oracle Golden kapısı yüklemek için aşağıdaki adımları tamamlayın:

1. Oracle oturum açın. (Sizin için bir parola istenmeden oturum açabilir olmalıdır.) Yüklemeye başlamadan önce Xming çalıştığından emin olun.
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. 'Oracle veritabanı 12 c için Oracle GoldenGate' seçin. Ardından **sonraki** devam etmek için.

  ![Yükleyici seçin yükleme sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Yazılım konumunu değiştirebilirsiniz. Ardından **Yöneticisi'ni başlatın** kutusunda ve veritabanı konumu girin. Devam etmek için **İleri**’yi seçin.

  ![Yükleme Seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Stok dizini değiştirin ve ardından **sonraki** devam etmek için.

  ![Yükleme Seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_03.png)

5. Üzerinde **Özet** ekran, select **yükleme** devam etmek için.

  ![Yükleyici seçin yükleme sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_04.png)

6. Bir komut dosyası 'kök' olarak çalıştıracak istenebilir. Bu durumda, ayrı bir oturum ssh sudo kök, VM açın ve sonra komut dosyasını çalıştırın. Seçin **Tamam** devam edin.

  ![Yükleme Seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_05.png)

7. Yükleme tamamlandığında seçin **Kapat** işlemini tamamlayın.

  ![Yükleme Seç sayfasının ekran görüntüsü](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>MyVM1 hizmette (birincil) ayarlayın

1. Oluşturma veya güncelleştirme tnsnames.ora dosyası:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Golden kapısı sahip ve kullanıcı hesapları oluşturun.

  > [!NOTE]
  > Sahip hesabı C ## önekine sahip olmalıdır.
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA to C##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. Golden kapısı test kullanıcı hesabı oluşturun:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. Extract parametre dosyası yapılandırın.

 Altın kapısı komut satırı arabirimi (ggsci) başlatın:

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. Aşağıdaki (VI komutlarını kullanarak) EXTRACT parametre dosyasına ekleyin. Esc tuşuna basın, ': wq!' dosyayı kaydetmek için. 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. YAZMAÇ ayıklayın--tümleşik Ayıkla:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. Extract denetim noktaları ayarlama ve gerçek zamanlı extract başlatın:

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request to MANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
Bu adımda, daha sonra farklı bir bölümde kullanılacak başlangıç SCN bulun:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a>MyVM2 hizmette ayarlayın (Çoğaltma)


1. Oluşturma veya güncelleştirme tnsnames.ora dosyası:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Bir çoğaltma hesabı oluşturun:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba to repuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. Bir Golden kapısı test kullanıcı hesabı oluşturun:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. Değişiklikleri çoğaltmak için REPLICAT parametre dosyası: 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  REPORA parametresi dosyasının içeriği:

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. Replicat denetim noktası ayarlayın:

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-the-replication-myvm1-and-myvm2"></a>(MyVM1 ve myVM2) çoğaltmayı ayarlama

#### <a name="1-set-up-the-replication-on-myvm2-replicate"></a>1. Çoğaltma myVM2 ayarlayın (Çoğaltma)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
Dosya aşağıdaki satırla güncelleştirin:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
Yöneticisi hizmetini durdurup yeniden başlatın:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-the-replication-on-myvm1-primary"></a>2. MyVM1 çoğaltmayı (birincil) ayarlayın

İlk yükleme ve hataları başlatın:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-the-replication-on-myvm2-replicate"></a>3. Çoğaltma myVM2 ayarlayın (Çoğaltma)

Değişiklik SCN önce elde numarasıyla numarası:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
Çoğaltma başladıktan ve testi tabloları yeni kayıtları ekleyerek test edebilirsiniz.


### <a name="view-job-status-and-troubleshooting"></a>İş durumunu görüntüleme ve sorunlarını giderme

#### <a name="view-reports"></a>Raporları görüntüleme
Üzerinde myVM1 raporları görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
Üzerinde myVM2 raporları görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Görünüm durumu ve geçmişi
Üzerinde myVM1 durumunu ve geçmişini görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

Üzerinde myVM2 durumunu ve geçmişini görüntülemek için aşağıdaki komutları çalıştırın:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Bu, yükleme ve Oracle linux Golden kapısı yapılandırmasına tamamlar.


## <a name="delete-the-virtual-machine"></a>Sanal makineyi silin

Artık gerekli olduğunda, kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek oranda kullanılabilir sanal makine oluşturma öğreticisi](../../linux/create-cli-complete.md)

[VM dağıtımı CLI örneklerini keşfedin](../../linux/cli-samples.md)
