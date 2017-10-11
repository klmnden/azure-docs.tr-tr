---
title: "Bir Oracle veritabanına bir Azure VM oluşturma | Microsoft Docs"
description: "Hızla bir Oracle veritabanına 12 c veritabanı ve Azure ortamınızda çalışan alın."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 8683b016c4db2c66fb1dd994405b70c3d137a7fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a>Bir Oracle veritabanına bir Azure VM oluşturma

Bir Azure sanal makineyi dağıtmak için Azure CLI kullanarak bu kılavuzu ayrıntılarını [Oracle Market galeri görüntüsü](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) 12 c Oracle veritabanı oluşturmak için. Sunucu dağıtıldığında, Oracle veritabanını yapılandırmak için SSH yoluyla bağlanır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine (VM) oluşturmak için kullanmak [az vm oluşturma](/cli/azure/vm#create) komutu. 

Aşağıdaki örnekte `myVM` adlı bir VM oluşturulur. Zaten bir varsayılan anahtar konumda yoksa, ayrıca SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM oluşturduktan sonra Azure CLI aşağıdaki örneğe benzer bilgiler görüntüler. Değeri Not `publicIpAddress`. VM erişmek için bu adresi kullanın.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresiyle değiştirin `publicIpAddress` , VM için değer.

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a>Veritabanı oluşturma

Oracle yazılım Market görüntüsü üzerinde zaten yüklü. Bir örnek veritabanı gibi oluşturun. 

1.  Geçiş *oracle* süper kullanıcı sonra günlüğe kaydetme için dinleyici başlatılamadı:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    Çıkış aşağıdakine benzer:

    ```bash
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

2.  Veritabanı oluştur:

    ```bash
    dbca -silent \
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

    Veritabanını oluşturmak için birkaç dakika sürer.

3. Oracle değişkenlerini ayarlama

Bağlanmadan önce iki ortam değişkenleri ayarlamanız gerekir: *ORACLE_HOME* ve *ORACLE_SID*.

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
.Bashrc dosyasına ORACLE_HOME ve ORACLE_SID değişkenlerini de ekleyebilirsiniz. Bu ortam değişkenleri gelecekteki oturum açma işlemleri için kaydeder. Aşağıdaki deyimleri eklenmiştir onaylayın `~/.bashrc` düzenleyiciyi kullanarak dosya.

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a>Oracle EM hızlı bağlantı

Veritabanı keşfetmek için kullanabileceğiniz bir GUI yönetim aracı için Oracle EM Express'i ayarlayın. Oracle EM Express'e bağlanmak için önce Oracle bağlantı noktasına ayarlamanız gerekir. 

1. Sqlplus kullanarak veritabanınızı Bağlan:

    ```bash
    sqlplus / as sysdba
    ```

2. Bağlandıktan sonra bağlantı noktası 5502 EM Express için ayarlama

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. Kapsayıcı PDB1 değilse zaten açık, ancak ilk onay durumu açın:

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    Çıkış aşağıdakine benzer:

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. Varsa için OPEN_MODE `PDB1` okuma PDB1 açmak için aşağıdakilere komutları çalıştırmak yazma, değil:

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

Yazmanız gerekir `quit` türü ve sqlplus oturumu sona erdirmek için `exit` oracle kullanıcının oturum kapatma için.

## <a name="automate-database-startup-and-shutdown"></a>Veritabanı başlatma ve kapatma otomatikleştirme

VM yeniden başlattığınızda Oracle veritabanı varsayılan tarafından otomatik olarak başlamıyor. Oracle veritabanı otomatik olarak başlayacak şekilde ayarlamak için ilk kök olarak oturum açın. Ardından, oluşturun ve bazı sistem dosyaları güncelleştirin.

1. Kök olarak oturum açma
    ```bash
    sudo su -
    ```

2.  Dosyasını düzenleyin, sık kullanılan düzenleyicisini kullanarak `/etc/oratab` ve varsayılan değeri değiştirmek `N` için `Y`:

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  Adlı bir dosya oluşturun `/etc/init.d/dbora` ve aşağıdaki içeriği yapıştırın:

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  İle dosyalarda izinleri değiştirme *chmod* gibi:

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  Sembolik bağlantılar için başlatma ve kapatma gibi oluşturun:

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  Değişikliklerinizi test etmek için VM yeniden başlatın:

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a>Bağlantı için açık bağlantı noktaları

Son görev bazı dış uç noktalar yapılandırmaktır. Azure ağ güvenliği VM koruma grubu ayarlamak için önce SSH oturumunuzun (dışında SSH önceki adımda yeniden başlatıldığı zaman başlayacağı zamana) VM'deki çıkın. 

1.  Oracle veritabanına uzaktan erişmek için kullandığınız uç nokta açmak için bir ağ güvenlik grubu kural oluştururken [az ağ nsg kuralını](/cli/azure/network/nsg/rule#create) gibi: 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  Oracle EM Express uzaktan erişmek için kullandığınız uç nokta açmak için bir ağ güvenlik grubu kural oluştururken [az ağ nsg kuralını](/cli/azure/network/nsg/rule#create) gibi:

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. Gerekirse, VM'yi yeniden ile ortak IP adresi elde [az ağ ortak IP Göster](/cli/azure/network/public-ip#show) gibi:

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  EM Express tarayıcınızdan bağlayın. Tarayıcınız EM (Flash yükleme gereklidir) Express ile uyumlu olduğundan emin olun: 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

Kullanarak oturum açtığınızda **SYS** hesap ve denetleme **SYSDBA'ın olarak** onay kutusu. Parolayı kullanmak **OraPasswd1** yüklemesi sırasında ayarladığınız. 

![Oracle OEM hızlı oturum açma sayfasının ekran görüntüsü](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure üzerinde ilk Oracle veritabanınızı keşfetme tamamladıktan ve VM artık gerekli olmadığında, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) VM, kaynak grubunu kaldırmak için komut ve ilişkili tüm kaynakları.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Diğer hakkında bilgi edinin [Oracle çözümleri Azure üzerinde](oracle-considerations.md). 

Deneyin [yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma](configure-oracle-asm.md) Öğreticisi.