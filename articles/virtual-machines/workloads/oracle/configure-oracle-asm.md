---
title: Bir Azure Linux sanal makinede Oracle ASM ayarlama | Microsoft Docs
description: Hızlı bir şekilde Oracle ASM yukarı ve Azure ortamınızda çalışan alın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/19/2017
ms.author: rclaus
ms.openlocfilehash: cc75235680eeace5107ef6ac0380e8b7a42974fc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656118"
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Bir Azure Linux sanal makinesinde Oracle ASM ayarlayın  

Azure sanal makineleri tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici yükleme ve yapılandırma, Oracle otomatik Depolama Yönetimi (ASM) ile birlikte temel Azure sanal makine dağıtımı kapsar.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oluşturma ve bir Oracle veritabanına VM'ye bağlanın
> * Yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma
> * Oracle kılavuz altyapısını yükleme ve yapılandırma
> * Bir Oracle ASM Yüklemeyi Başlat
> * ASM tarafından yönetilen bir Oracle DB oluştur


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="prepare-the-environment"></a>Ortamı hazırlama

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu oluşturmak için [az group create](/cli/azure/group#az_group_create) komutunu kullanın. Bir Azure kaynak grubu hangi Azure kaynakları dağıtılan yönetilen ve mantıksal bir kapsayıcısıdır. Bu örnekte, bir kaynak grubu adında *myResourceGroup* içinde *eastus* bölge.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>VM oluşturma

Oracle veritabanı görüntüyü temel alarak bir sanal makine oluşturun ve Oracle ASM kullanacak şekilde yapılandırmak için kullanın [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. 

Aşağıdaki örnek, bir Standard_DS2_v2 boyutu 50 GB dört eklenen veri disklerini ile myVM adlı VM oluşturur. Varsayılan anahtar konumunda Bunlar zaten yoksa, ayrıca SSH anahtarları oluşturur.  Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

VM oluşturduktan sonra Azure CLI aşağıdaki örneğe benzer bilgiler görüntüler. Değeri Not `publicIpAddress`. VM erişmek için bu adresi kullanın.

   ```azurecli
   {
     "fqdns": "",
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
     "location": "eastus",
     "macAddress": "00-0D-3A-36-2F-56",
     "powerState": "VM running",
     "privateIpAddress": "10.0.0.4",
     "publicIpAddress": "13.64.104.241",
     "resourceGroup": "myResourceGroup"
   }
   ```

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

VM ile bir SSH oturumu oluşturmak ve diğer ayarları yapılandırmak için aşağıdaki komutu kullanın. IP adresiyle değiştirin `publicIpAddress` , VM için değer.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM yükleyin

Oracle ASM yüklemek için aşağıdaki adımları tamamlayın. 

Oracle ASM yükleme hakkında daha fazla bilgi için bkz: [ASMLib Oracle yüklemeler için Oracle Linux 6](http://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. ASM yüklemeye devam etmek için kök olarak oturum açmanız:

   ```bash
   sudo su -
   ```
   
2. Oracle ASM bileşenleri yüklemek için ek şu komutları çalıştırın:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget http://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Oracle ASM yüklü olduğunu doğrulayın:

   ```bash
   rpm -qa |grep oracleasm
   ```

    Bu komutun çıktısı, aşağıdaki bileşenleri listesi:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM, düzgün çalışabilmesi için belirli kullanıcılar ve roller gerektirir. Aşağıdaki komutları önkoşul kullanıcı hesapları ve grupları oluşturun: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Kullanıcılar ve gruplar oluşturulduğundan doğru doğrulayın:

   ```bash
   id grid
   ```

    Bu komutun çıktısı, aşağıdaki kullanıcılar ve gruplar listesi:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Kullanıcı için bir klasör oluşturun *kılavuz* ve sahibi değiştirin:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle ASM ayarlayın

Bu öğretici için varsayılan kullanıcıdır *kılavuz* ve varsayılan grup *asmadmin*. Emin *oracle* kullanıcı asmadmin grubunun bir parçasıdır. Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. Varsayılan grubu (asmadmin) ve varsayılan kullanıcı (Izgara) tanımlama yanı sıra önyüklemede başlatmak için sürücü yapılandırma gerektirir Oracle ASM kitaplığı sürücüsünü ayarlama (y seçin) ve önyükleme disklerde taramak için (y seçin). Aşağıdaki komutu komut istemlerini yanıtlamanız gerekir:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   Bu komutun çıktısı durdurma ile ister yanıtlanması gereken aşağıdakine benzer görünmelidir.

    ```bash
   Configuring the Oracle ASM library driver.

   This will configure the on-boot properties of the Oracle ASM library
   driver. The following questions will determine whether the driver is
   loaded on boot and what permissions it will have. The current values
   will be shown in brackets ('[]'). Hitting <ENTER> without typing an
   answer will keep that current value. Ctrl-C will abort.

   Default user to own the driver interface []: grid
   Default group to own the driver interface []: asmadmin
   Start Oracle ASM library driver on boot (y/n) [n]: y
   Scan for Oracle ASM disks on boot (y/n) [y]: y
   Writing Oracle ASM library driver configuration: done
   ```

2. Disk yapılandırmasını görüntüleyin:
   ```bash
   cat /proc/partitions
   ```

   Bu komutun çıktısı kullanılabilir diskler aşağıdaki listeye benzer görünmelidir.

   ```bash
   8       16   14680064 sdb
   8       17   14678976 sdb1
   8        0   52428800 sda
   8        1     512000 sda1
   8        2   51915776 sda2
   8       48   52428800 sdd
   8       64   52428800 sde
   8       80   52428800 sdf
   8       32   52428800 sdc
   11       0       1152 sr0
   ```

3. Biçim disk */dev/sdc* aşağıdaki komutu çalıştırarak ve istemleri ile yanıtlama:
   - *n* yeni bölüm için
   - *p* birincil bölüm
   - *1* ilk bölüm seçmek için
   - basın `enter` varsayılan ilk silindir için
   - basın `enter` varsayılan son silindir için
   - basın *w* değişiklikleri bölüm tablosuna yazmak için  

   ```bash
   fdisk /dev/sdc
   ```
   
   Yukarıda verilen yanıtlar kullanarak, çıkış fdisk komutu için aşağıdaki gibi görünmelidir:

   ```bash
   Device contains not a valid DOS partition table, or Sun, SGI or OSF disklabel
   Building a new DOS disklabel with disk identifier 0xf865c6ca.
   Changes will remain in memory only, until you decide to write them.
   After that, of course, the previous content won't be recoverable.

   Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

   The device presents a logical sector size that is smaller than
   the physical sector size. Aligning to a physical sector (or optimal
   I/O) size boundary is recommended, or performance may be impacted.

   WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off the mode (command 'c') and change display units to
           sectors (command 'u').

   Command (m for help): n
   Command action
     e   extended
     p   primary partition (1-4)
   p
   Partition number (1-4): 1
   First cylinder (1-6527, default 1):
   Using default value 1
   Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527):
   Using default value 6527

   Command (m for help): w
   The partition table has been altered!

   Calling ioctl() to re-read partition table.
   Syncing disks.
   ```

4. Önceki fdisk komutu için yineleyin `/dev/sdd`, `/dev/sde`, ve `/dev/sdf`.

5. Disk yapılandırmasını kontrol edin:

   ```bash
   cat /proc/partitions
   ```

   Komutunun çıktısını aşağıdaki gibi görünmelidir:

   ```bash
   major minor  #blocks  name

     8       16   14680064 sdb
     8       17   14678976 sdb1
     8       32   52428800 sdc
     8       33   52428096 sdc1
     8       48   52428800 sdd
     8       49   52428096 sdd1
     8       64   52428800 sde
     8       65   52428096 sde1
     8       80   52428800 sdf
     8       81   52428096 sdf1
     8        0   52428800 sda
     8        1     512000 sda1
     8        2   51915776 sda2
     11       0    1048575 sr0
   ```

6. Oracle ASM hizmet durumunu denetleyin ve Oracle ASM hizmetini başlatın:

   ```bash
   service oracleasm status 
   service oracleasm start
   ```

   Komutunun çıktısını aşağıdaki gibi görünmelidir:
   
   ```bash
   Checking if ASM is loaded: no
   Checking if /dev/oracleasm is mounted: no
   Initializing the Oracle ASMLib driver:                     [  OK  ]
   Scanning the system for Oracle ASMLib disks:               [  OK  ]
   ```

7. Oracle ASM diskleri oluşturun:

   ```bash
   service oracleasm createdisk ASMSP /dev/sdc1 
   service oracleasm createdisk DATA /dev/sdd1 
   service oracleasm createdisk DATA1 /dev/sde1 
   service oracleasm createdisk FRA /dev/sdf1
   ```    

   Komutunun çıktısını aşağıdaki gibi görünmelidir:

   ```bash
   Marking disk "ASMSP" as an ASM disk:                       [  OK  ]
   Marking disk "DATA" as an ASM disk:                        [  OK  ]
   Marking disk "DATA1" as an ASM disk:                       [  OK  ]
   Marking disk "FRA" as an ASM disk:                         [  OK  ]
   ```

8. Oracle ASM diskleri listele:

   ```bash
   service oracleasm listdisks
   ```   

   Komutunun çıktısını aşağıdaki Oracle ASM diskleri listele:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Kök, oracle ve kılavuz kullanıcıların parolalarını değiştirin. **Bu yeni parolalar Not** yükleme sırasında kullandığınız gibi.

   ```bash
   passwd oracle 
   passwd grid 
   passwd root
   ```

10. Klasör iznini değiştirin:

   ```bash
   chmod -R 775 /opt 
   chown grid:oinstall /opt 
   chown oracle:oinstall /dev/sdc1 
   chown oracle:oinstall /dev/sdd1 
   chown oracle:oinstall /dev/sde1 
   chown oracle:oinstall /dev/sdf1 
   chmod 600 /dev/sdc1 
   chmod 600 /dev/sdd1 
   chmod 600 /dev/sde1 
   chmod 600 /dev/sdf1
   ```

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>Karşıdan yükle ve Oracle kılavuz altyapıyı hazırlama

Karşıdan yüklemek ve Oracle kılavuz altyapı yazılımlarını hazırlamak için aşağıdaki adımları tamamlayın:

1. Oracle kılavuz altyapısından karşıdan [Oracle ASM indirme sayfası](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Başlıklı indirme altında **Oracle veritabanı Linux x86-64 12c sürüm 1 kılavuz altyapısına (12.1.0.2.0)**, iki .zip dosyalarını indirin.

2. İstemci bilgisayarınız .zip dosyaları indirdikten sonra VM'nize dosyaları kopyalamak için kopyalama Protokolü (SCP) kullanabilirsiniz:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. .Zip dosyasına taşımak için azure'da, Oracle VM uygulamasına geri SSH / opt klasör. Ardından, dosya sahibini değiştirin:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Dosyaların sıkıştırmasını açın. (Yükleme Linux sıkıştırmasını aracı önceden yüklüyse.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. Değiştirme izni:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Yapılandırılan güncelleştirme takas alanı. Oracle kılavuz bileşenleri en az 6.8 kılavuz yüklemek için GB takas alanı gerekir. Azure Oracle Linux görüntülerinde varsayılan takas dosyası boyutu yalnızca 2048 MB'tır. Artırmaya gereksinim `ResourceDisk.SwapSizeMB` içinde `/etc/waagent.conf` dosya ve güncelleştirilmiş ayarların etkili olması sırayla WALinuxAgent hizmetini yeniden başlatın. Salt okunur bir dosya olduğundan, dosya izinleri yazma erişimine izin verecek biçimde değiştirmeniz gerekir.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Arama `ResourceDisk.SwapSizeMB` değerine değiştirip **8192**. Basın gerekecek `insert` ekleme modu girmek için değerinde yazın **8192** ve tuşuna basarak `esc` komutu moduna geri dönmek için. Değişiklikleri yazma ve dosyayı çıkmak için yazın `:wq` ve basın `enter`.
   
   > [!NOTE]
   > Her zaman kullanmanız önerilir `WALinuxAgent` takas alanı, yerel diskteki kısa ömürlü (geçici disk) en iyi performans için her zaman oluşturulan şekilde yapılandırmak için. Daha fazla bilgi için bkz: [nasıl takas dosyası Linux Azure sanal makinelerde ekleneceğini](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-to-run-x11"></a>Yerel istemci ve VM x11 çalıştırmak için hazırlama
Oracle ASM yapılandırılması, yükleme ve yapılandırmayı tamamlamak için bir grafik arabirim gerektirir. X11 kullanıyoruz bu yüklemeyi kolaylaştırmak için protokol. X11 zaten olan bir istemci sistem (Mac veya Linux) kullanıyorsanız, özellikleri etkinleştirilmiş ve yapılandırılmış - bu yapılandırma ve özel kurulum Windows makinelerine atlayabilirsiniz. 

1. [PuTTY karşıdan](http://www.putty.org/) ve [Xming karşıdan](https://xming.en.softonic.com/) Windows bilgisayarınıza. Devam etmeden önce varsayılan değerlerle bu uygulamaların her ikisi de yüklemesini tamamlamak gerekir.

2. PuTTY yükledikten sonra bir komut istemi açın, PuTTY klasöre (örneğin, C:\Program Files\PuTTY) değiştirin ve çalıştırın `puttygen.exe` bir anahtar oluşturmak için.

3. PuTTY anahtar oluşturma Aracı'nda:
   
   1. Seçerek bir anahtar oluşturmak `Generate` düğmesi.
   2. (Ctrl + C) anahtarının içeriğini kopyalayın.
   3. Seçin `Save private key` düğmesi.
   4. Anahtarı bir parola ile güvenli hale getirme hakkında uyarıyı yok sayın ve ardından `OK`.

   ![PuTTY anahtar oluşturucunun ekran görüntüsü](./media/oracle-asm/puttykeygen.png)

4. VM'nizi, şu komutları çalıştırın:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. Adlı bir dosya oluşturun `authorized_keys`. Bu dosyada anahtar içeriğini yapıştırın ve ardından dosyayı kaydedin.

   > [!NOTE]
   > Anahtar dize içermelidir `ssh-rsa`. Ayrıca, anahtar içeriğini tek satırlık metin olmalıdır.
   >  

6. İstemci sisteminizde Putty'yi başlatın. İçinde **kategori** bölmesinde, Git **bağlantı** > **SSH** > **Auth**. İçinde **kimlik doğrulama için özel anahtar dosyası** kutusunda, daha önce oluşturulan anahtara göz atın.

   ![SSH kimlik doğrulaması seçeneklerinin ekran görüntüsü](./media/oracle-asm/setprivatekey.png)

7. İçinde **kategori** bölmesinde, Git **bağlantı** > **SSH** > **X11**. Seçin **X11 Enable iletimi** onay kutusu.

   ![Ekran görüntüsü X11 SSH seçenekleri iletme](./media/oracle-asm/enablex11.png)

8. İçinde **kategori** bölmesinde, Git **oturum**. Oracle ASM VM girin `<publicIPaddress>` ana bilgisayar adı iletişim kutusunda, yeni bir dolgu `Saved Session` adlandırın ve ardından tıklatın `Save`.  Kaydedildikten sonra tıklayın `open` , Oracle ASM sanal makineye bağlanmak için.  Bağlandığınız ilk kez uzak sistem, kayıt defterinde önbelleğe alınmamış uyarılırsınız. Tıklayın `yes` ekleyin ve devam edin.

   ![PuTTY oturum seçeneklerinin ekran görüntüsü](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle kılavuz altyapısı yükleme

Oracle kılavuz altyapısı yüklemek için aşağıdaki adımları tamamlayın:

1. Olarak oturum açın **kılavuz**. (Sizin için bir parola istenmeden oturum açabilir olmalıdır.) 

   > [!NOTE]
   > Windows çalıştırıyorsanız, yüklemeye başlamadan önce Xming başlatıldığından emin olun.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle kılavuz altyapı 12c sürüm 1 yükleyicisi açılır. (Başlatmak yükleyici için birkaç dakika sürebilir.)

2. Üzerinde **yükleme seçeneği** sayfasında **yüklenir ve tek başına bir sunucu için Oracle kılavuz altyapı yapılandırma**.

   ![Yükleyicisi'nin yükleme seçeneği sayfasının ekran görüntüsü](./media/oracle-asm/install01.png)

3. Üzerinde **ürün dilleri seçin** sayfasında, olun **İngilizce** veya istediğiniz dil seçilmedi.  `next` öğesine tıklayın.

4. Üzerinde **ASM Disk grubu oluşturma** sayfa:
   - Disk grubu için bir ad girin.
   - Altında **artıklık**seçin **dış**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Altında **disk Ekle**seçin **ORCLASMSP**.
   - `next` öğesine tıklayın.

5. Üzerinde **belirtin ASM parola** sayfasında, **bu hesaplar için aynı parolaları kullanıp** seçeneği ve bir parola girin.

   ![Yükleyicinin belirtin ASM parola sayfasının ekran görüntüsü](./media/oracle-asm/install04.png)

6. Üzerinde **yönetimi seçeneklerini belirtin** sayfasında EM bulut denetimini yapılandırma seçeneğiniz vardır. Biz bu seçenek atlanıyor - tıklatın `next` devam etmek için. 

7. Üzerinde **ayrıcalıklı işletim sistemi gruplarına** sayfasında, varsayılan ayarları kullanın. Tıklatın `next` devam etmek için.

8. Üzerinde **yükleme konumu belirtin** sayfasında, varsayılan ayarları kullanın. Tıklatın `next` devam etmek için.

9. Üzerinde **Envanter Oluştur** sayfasında, Envanter dizine geçin `/u01/app/grid/oraInventory`. Tıklatın `next` devam etmek için.

   ![Yükleyicinin Envanter Oluştur sayfasının ekran görüntüsü](./media/oracle-asm/install08.png)

10. Üzerinde **kök komut dosyası yürütme Yapılandırması** sayfasında, **otomatik yapılandırma komut dosyaları çalıştırmak** onay kutusu. Ardından, seçin **"root" kullanıcı kimlik bilgisi kullanma** seçeneği ve kök kullanıcı parolasını girin.

    ![Yükleyicisi'nin kök komut dosyası yürütme yapılandırma sayfasının ekran görüntüsü](./media/oracle-asm/install09.png)

11. Üzerinde **gerçekleştirmek önkoşul denetler** sayfasında, geçerli kurulumu başarısız olur hatalarla. Bu beklenen bir davranıştır. `Fix & Check Again` öğesini seçin.

12. İçinde **düzeltmesi betik** iletişim kutusu, tıklatın `OK`.

13. Üzerinde **Özet** sayfasında, seçtiğiniz ayarları gözden geçirin ve ardından `Install`.

    ![Yükleyicinin Özet sayfasının ekran görüntüsü](./media/oracle-asm/install12.png)

14. Komut dosyaları ayrıcalıklı bir kullanıcı olarak çalıştırılması gereken bildiren, yapılandırma bir uyarı iletişim kutusu görüntülenir. Tıklatın `Yes` devam etmek için.

15. Üzerinde **son** sayfasında, `Close` yüklemenin tamamlanması için.

## <a name="set-up-your-oracle-asm-installation"></a>Oracle ASM yüklemenizi ayarlayın

Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. Hala oturum açmaya olarak olun **kılavuz**, sizin X11 gelen oturum. İsabet gerekebilir `enter` terminal takarsanız için. Ardından Oracle otomatik Depolama Yönetimi yapılandırma Yardımcısı'nı başlatın:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM yapılandırma Yardımcısı'nı açar.

2. İçinde **yapılandırma ASM: Disk gruplarının** iletişim kutusu, tıklatın `Create` düğmesine tıklayın ve ardından `Show Advanced Options`.

3. İçinde **Disk grubu oluşturma** iletişim kutusunda:

   - Disk grubu adı girin **veri**.
   - Altında **üye diskleri Seç**seçin **ORCL_DATA** ve **ORCL_DATA1**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Tıklatın `ok` disk grubu oluşturmak için.
   - Tıklatın `ok` doğrulama penceresini kapatın.

   ![Disk grubu oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm02.png)

4. İçinde **yapılandırma ASM: Disk gruplarının** iletişim kutusu, tıklatın `Create` düğmesine tıklayın ve ardından `Show Advanced Options`.

5. İçinde **Disk grubu oluşturma** iletişim kutusunda:

   - Disk grubu adı girin **FRA**.
   - Altında **artıklık**seçin **dış (hiçbiri)**.
   - Altında **üye diskleri Seç**seçin **ORCL_FRA**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Tıklatın `ok` disk grubu oluşturmak için.
   - Tıklatın `ok` doğrulama penceresini kapatın.

   ![Disk grubu oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm04.png)

6. Seçin **çıkış** ASM yapılandırma Yardımcısı'nı kapatın.

   ![Ekran görüntüsü yapılandırmak ASM: çıkış düğmesi Disk gruplar iletişim kutusu](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>Veritabanı oluşturma

Oracle veritabanı yazılımını Azure Market görüntüsü üzerinde zaten yüklü. Bir veritabanı oluşturmak için aşağıdaki adımları tamamlayın:

1. Kullanıcılar için Oracle süper kullanıcı geçiş ve günlüğe kaydetme için dinleyici başlatılamadı:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Veritabanı yapılandırma Yardımcısı'nı açar.

2. Üzerinde **veritabanı işlemi** sayfasında, `Create Database`.

3. Üzerinde **oluşturma modu** sayfa:

   - Veritabanı için bir ad girin.
   - İçin **depolama türü**, olun **otomatik Depolama Yönetimi (ASM)** seçilir.
   - İçin **veritabanı dosya konumu**, varsayılan kullanmak ASM önerilen konum.
   - İçin **Hızlı Kurtarma alanında**, varsayılan kullanmak ASM önerilen konum.
   - yazın bir **yönetici parolasını** ve **parolayı onaylayın**.
   - olun `create as container database` seçilir.
   - yazın bir `pluggable database name` değeri.

4. Üzerinde **Özet** sayfasında, seçtiğiniz ayarları gözden geçirin ve ardından `Finish` veritabanı oluşturulamıyor.

   ![Özet sayfasının ekran görüntüsü](./media/oracle-asm/createdb03.png)

5. Veritabanı oluşturuldu. Üzerinde **son** sayfasında, bu veritabanı kullanın ve parolaları değiştirmek için ek hesapların kilidini açmak için seçeneği vardır. Bunu yapmak istiyorsanız seçin **parola yönetimi** -Aksi takdirde tıklayın `close`.

## <a name="delete-the-vm"></a>VM’yi silin

Azure Marketi'nden Oracle DB yansımasında Oracle otomatik Depolama Yönetimi başarıyla yapılandırdınız.  Bu VM artık ihtiyacınız olduğunda, kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Oracle DataGuard yapılandırın](configure-oracle-dataguard.md)

[Öğretici: Oracle GoldenGate yapılandırın](Configure-oracle-golden-gate.md)

Gözden geçirme [bir Oracle DB Mimari](oracle-design.md)
