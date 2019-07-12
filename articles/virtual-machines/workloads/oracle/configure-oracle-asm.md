---
title: Bir Azure Linux sanal makinesinde Oracle ASM ayarlayın | Microsoft Docs
description: Oracle ASM ve Azure ortamınızda çalışan hızla alın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
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
ms.openlocfilehash: a2f6eab495680b3f32246488af5b7bbe5263d93a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707696"
---
# <a name="set-up-oracle-asm-on-an-azure-linux-virtual-machine"></a>Bir Azure Linux sanal makinesinde Oracle ASM ayarlayın  

Azure sanal makineleri tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici, temel Azure sanal makine dağıtımını yükleme ve yapılandırma, Oracle otomatik Depolama Yönetimi (ASM) ile birlikte kullanıldığında kapsar.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oluşturma ve bir Oracle veritabanı VM'ye bağlanma
> * Yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma
> * Yükleme ve Oracle kılavuz altyapısını yapılandırma
> * Bir Oracle ASM Yüklemeyi Başlat
> * ASM tarafından yönetilen bir Oracle DB oluşturma


[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="prepare-the-environment"></a>Ortamı hazırlama

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın. Bir Azure kaynak grubu, Azure kaynaklarını dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bu örnekte, adlı bir kaynak grubu *myResourceGroup* içinde *eastus* bölge.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

### <a name="create-a-vm"></a>VM oluşturma

Oracle Database görüntüyü temel alarak sanal makine oluşturma ve Oracle ASM kullanacak şekilde yapılandırmak için kullanın [az vm oluşturma](/cli/azure/vm) komutu. 

Aşağıdaki örnek, bir Standard_DS2_v2 boyutu 50 GB'ın dört bağlı veri diskleri ile myVM adlı bir VM oluşturur. Bunlar varsayılan anahtar konumunda zaten yoksa, aynı zamanda SSH anahtarları oluşturulur.  Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  

   ```azurecli-interactive
   az vm create --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50 50 50 50
   ```

VM oluşturduktan sonra Azure CLI aşağıdaki örneğe benzer bilgiler görüntüler. Değerini not edin `publicIpAddress`. Sanal Makineye erişmek için bu adresi kullanın.

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

Sanal makine ile bir SSH oturumu oluşturmak ve ek ayarları yapılandırmak için aşağıdaki komutu kullanın. IP adresi ile değiştirin `publicIpAddress` VM'niz için değer.

```bash 
ssh <publicIpAddress>
```

## <a name="install-oracle-asm"></a>Oracle ASM'yi yükleyin

Oracle ASM yüklemek için aşağıdaki adımları tamamlayın. 

Oracle ASM yükleme hakkında daha fazla bilgi için bkz. [Oracle ASMLib indirmeleri için Oracle Linux 6](https://www.oracle.com/technetwork/server-storage/linux/asmlib/ol6-1709075.html).  

1. ASM yüklemeye devam etmek üzere kök olarak oturum açmanız gerekir:

   ```bash
   sudo su -
   ```
   
2. Oracle ASM bileşenleri yüklemek için ek şu komutları çalıştırın:

   ```bash
    yum list | grep oracleasm 
    yum -y install kmod-oracleasm.x86_64 
    yum -y install oracleasm-support.x86_64 
    wget https://download.oracle.com/otn_software/asmlib/oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    yum -y install oracleasmlib-2.0.12-1.el6.x86_64.rpm 
    rm -f oracleasmlib-2.0.12-1.el6.x86_64.rpm
   ```

3. Oracle ASM yüklendiğini doğrulayın:

   ```bash
   rpm -qa |grep oracleasm
   ```

    Bu komutun çıktısı, aşağıdaki bileşenler listesinde:

    ```bash
   oracleasm-support-2.1.10-4.el6.x86_64
   kmod-oracleasm-2.0.8-15.el6_9.x86_64
   oracleasmlib-2.0.12-1.el6.x86_64
    ```

4. ASM, düzgün çalışabilmesi için belirli kullanıcıları ve rolleri gerektirir. Aşağıdaki komutlar, önkoşul kullanıcı hesapları ve grupları oluşturun: 

   ```bash
    groupadd -g 54345 asmadmin 
    groupadd -g 54346 asmdba 
    groupadd -g 54347 asmoper 
    useradd -u 3000 -g oinstall -G dba,asmadmin,asmdba,asmoper grid 
    usermod -g oinstall -G dba,asmdba,asmadmin oracle
   ```

5. Kullanıcılar ve gruplar oluşturulmuş doğru şekilde doğrulayın:

   ```bash
   id grid
   ```

    Bu komutun çıktısı, aşağıdaki kullanıcı ve grupları listesi:

    ```bash
    uid=3000(grid) gid=54321(oinstall) groups=54321(oinstall),54322(dba),54345(asmadmin),54346(asmdba),54347(asmoper)
    ```
 
6. Kullanıcı için bir klasör oluşturun *kılavuz* ve sahibini değiştirin:

   ```bash
   mkdir /u01/app/grid 
   chown grid:oinstall /u01/app/grid
   ```

## <a name="set-up-oracle-asm"></a>Oracle ASM ayarlayın

Bu öğretici için varsayılan kullanıcıdır *kılavuz* ve varsayılan grup *asmadmin*. Emin *oracle* kullanıcı asmadmin grubun bir parçasıdır. Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. Oracle ASM kitaplığı sürücüsünü ayarı varsayılan kullanıcı (Kılavuz) ve varsayılan grup (asmadmin) tanımlama yanı sıra sürücü önyüklemede başlatılacak yapılandırma içerir (y seçin) ve önyükleme disklerde taraması (y seçin). Aşağıdaki komutu istemlerini yanıtlayın yapmanız gerekir:

   ```bash
   /usr/sbin/oracleasm configure -i
   ```

   Bu komutun çıktısı ile durdurma ister yanıtlanması için aşağıdakine benzer görünmelidir.

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

   Bu komutun çıktısı, kullanılabilir disklerin aşağıdaki listeye benzer görünmelidir

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

3. Diski Biçimlendir */dev/sdc* aşağıdaki komutu çalıştırarak ve ile istemleri yanıtlayarak:
   - *n* yeni bölümü için
   - *p* için birincil bölüm
   - *1* ilk bölümü seçin
   - basın `enter` için varsayılan ilk silindir
   - basın `enter` için varsayılan son silindir
   - basın *w* bölüm tablosuna değişiklikleri yazmak için  

   ```bash
   fdisk /dev/sdc
   ```
   
   Yukarıda verilen yanıtları kullanarak fdisk komutu için çıktı aşağıdaki gibi görünmelidir:

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

   Komut çıktısı, aşağıdaki gibi görünmelidir:

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

   Komut çıktısı, aşağıdaki gibi görünmelidir:
   
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

   Komut çıktısı, aşağıdaki gibi görünmelidir:

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

   Komut çıktısı şu Oracle ASM disklerin listelemesi gerekir:

   ```bash
    ASMSP
    DATA
    DATA1
    FRA
   ```

9. Kök, oracle ve kılavuz kullanıcıların parolalarını değiştirin. **Yeni bu parolaları Not** yükleme sırasında kullanıyorsanız.

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

## <a name="download-and-prepare-oracle-grid-infrastructure"></a>İndirin ve Oracle kılavuz altyapıyı hazırlama

İndirip kılavuz altyapı Oracle yazılımlarını hazırlamak için aşağıdaki adımları tamamlayın:

1. Oracle kılavuz Altyapıdan indirme [Oracle ASM indirme sayfası](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html). 

   Başlıklı indirme altında **Oracle Database 12c sürüm 1 kılavuz altyapısına (12.1.0.2.0) Linux x86-64**, iki .zip dosyasını indirin.

2. .Zip dosyalarını, istemci bilgisayara indirdikten sonra sanal makinenizde dosyaları kopyalamak için kopyalama Protokolü (SCP) kullanabilirsiniz:

   ```bash
   scp *.zip <publicIpAddress>:.
   ```

3. Geri .zip dosyasına taşımak amacıyla azure'da Oracle VM'ye SSH / opt klasör. Sonra dosya sahibini değiştirin:

   ```bash
   ssh <publicIPAddress>
   sudo mv ./*.zip /opt
   cd /opt
   sudo chown grid:oinstall linuxamd64_12102_grid_1of2.zip
   sudo chown grid:oinstall linuxamd64_12102_grid_2of2.zip
   ```

4. Dosyaların sıkıştırmasını açın. (Yükleme Linux sıkıştırmasını açın aracı zaten yüklü değilse.)
   
   ```bash
   sudo yum install unzip
   sudo unzip linuxamd64_12102_grid_1of2.zip
   sudo unzip linuxamd64_12102_grid_2of2.zip
   ```

5. İzni değiştir:
   
   ```bash
   sudo chown -R grid:oinstall /opt/grid
   ```

6. Yapılandırılan güncelleştirme takas alanı. Oracle kılavuz bileşenleri en az 6.8 Kılavuzu yüklemek için GB takas alanı gerekir. Azure'da Oracle Linux görüntüleri için varsayılan takas dosyası boyutu yalnızca 2048 MB'dir. Artırmanız gerekirse `ResourceDisk.SwapSizeMB` içinde `/etc/waagent.conf` dosya ve etkili olması güncelleştirilmiş ayarlarının WALinuxAgent hizmeti yeniden başlatın. Salt okunur bir dosya olduğu için yazma erişimi etkinleştirmek için dosyanın izinlerini değiştirmek gerekir.

   ```bash
   sudo chmod 777 /etc/waagent.conf  
   vi /etc/waagent.conf
   ```
   
   Arama `ResourceDisk.SwapSizeMB` ve değere değiştirin **8192**. Tuşuna basmanız gerekir `insert` INSERT moduna girmek için şunu yazın değerinde **8192** ve tuşuna `esc` komut moduna geri dönmek için. Değişiklikleri yazma ve dosya çıkmak için şunu yazın `:wq` basın `enter`.
   
   > [!NOTE]
   > Her zaman kullanmanızı öneririz `WALinuxAgent` takas alanı en iyi performans için yerel geçici disk (geçici disk) üzerinde her zaman oluşturulan şekilde yapılandırmak için. Hakkında daha fazla bilgi için bkz. [Linux Azure sanal makineler'de takas dosyası ekleme](https://support.microsoft.com/en-us/help/4010058/how-to-add-a-swap-file-in-linux-azure-virtual-machines).

## <a name="prepare-your-local-client-and-vm-to-run-x11"></a>VM ve yerel istemci x11 çalıştırmak için hazırlama
Oracle ASM yapılandırılması, yükleme ve yapılandırmayı tamamlamak için bir grafik arabirim gerektirir. X11 kullanıyoruz bu yüklemeyi kolaylaştırmak için protokol. X11 zaten bir istemci sistemi (Mac veya Linux) kullanıyorsanız özellikleri etkinleştirilmiş ve yapılandırılmış - bu yapılandırma ve Kurulum özel Windows makineleri için atlayabilirsiniz. 

1. [Putty'yi indirin](https://www.putty.org/) ve [Xming indirme](https://xming.en.softonic.com/) Windows bilgisayarınıza. Hem varsayılan değerlerle devam etmeden önce bu uygulamaların yüklenmesini tamamlamak için ihtiyacınız olacaktır.

2. PuTTY yükledikten sonra bir komut istemi açın, PuTTY klasöre (örneğin, C:\Program Files\PuTTY) değiştirin ve çalıştırın `puttygen.exe` bir anahtar oluşturmak için.

3. PuTTY anahtar oluşturucu içinde:
   
   1. Bir anahtar seçerek oluşturmak `Generate` düğmesi.
   2. (Ctrl + C) anahtarının içeriğini kopyalayın.
   3. `Save private key` düğmesini seçin.
   4. Anahtarı bir parola ile güvenli hale getirme hakkında uyarıyı yok sayın ve ardından `OK`.

   ![PuTTY anahtar Oluşturucu ekran görüntüsü](./media/oracle-asm/puttykeygen.png)

4. Sanal makinenize şu komutları çalıştırın:

   ```bash
   sudo su - grid
   mkdir .ssh 
   cd .ssh
   ```

5. `authorized_keys` adlı bir dosya oluşturun. Anahtarın içeriğini bu dosyaya yapıştırın ve dosyayı kaydedin.

   > [!NOTE]
   > Anahtar dize içermelidir `ssh-rsa`. Ayrıca, anahtarın içeriğini tek satırlık bir metin olmalıdır.
   >  

6. İstemci sisteminizde Putty'yi başlatın. İçinde **kategori** bölmesinde, Git **bağlantı** > **SSH** > **Auth**. İçinde **kimlik doğrulaması için özel anahtar dosyası** kutusunda, daha önce oluşturduğunuz anahtara göz atın.

   ![SSH kimlik doğrulaması seçeneklerinin ekran görüntüsü](./media/oracle-asm/setprivatekey.png)

7. İçinde **kategori** bölmesinde, Git **bağlantı** > **SSH** > **X11**. Seçin **etkinleştir X11 iletme** onay kutusu.

   ![Ekran görüntüsü X11 SSH iletme seçenekleri](./media/oracle-asm/enablex11.png)

8. İçinde **kategori** bölmesinde, Git **oturumu**. Oracle ASM sanal makinenize girin `<publicIPaddress>` konak adı iletişim kutusunda, yeni bir dolgu `Saved Session` adlandırın ve ardından `Save`.  Kaydedildikten sonra tıklayarak `open` Oracle ASM sanal makinenize bağlanmak için.  İlk kez bağlandığınızda uzak sistem kayıt defterinizde önbelleğe alınmamış uyarılırsınız. Tıklayarak `yes` ekleyin ve devam etmek için.

   ![PuTTY oturumuna seçeneklerinin ekran görüntüsü](./media/oracle-asm/puttysession.png)

## <a name="install-oracle-grid-infrastructure"></a>Oracle kılavuz altyapısı yükleme

Oracle kılavuz altyapı yüklemek için aşağıdaki adımları tamamlayın:

1. Olarak oturum **kılavuz**. (Sizin için bir parola istenmeden oturum açabilir olmalıdır.) 

   > [!NOTE]
   > Windows çalıştırıyorsanız, yüklemeye başlamadan önce Xming başlatıldığından emin olun.

   ```bash
   cd /opt/grid
   ./runInstaller
   ```

   Oracle kılavuz altyapı 12c sürüm 1 yükleyici açılır. (Başlatmak yükleyici için birkaç dakika sürebilir.)

2. Üzerinde **yükleme seçenek** sayfasında **yükleme ve yapılandırma Oracle kılavuz altyapı için bir tek başına sunucu**.

   ![Yükleyicisi'nin yükleme seçenek sayfasının ekran görüntüsü](./media/oracle-asm/install01.png)

3. Üzerinde **ürün dilleri seçin** sayfasında, olun **İngilizce** veya istediğiniz dili seçilir.  Tıklatın `next`.

4. Üzerinde **ASM Disk grubu oluşturma** sayfası:
   - Disk grubu için bir ad girin.
   - Altında **Yedeklilik**seçin **dış**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Altında **diskleri ekleme**seçin **ORCLASMSP**.
   - Tıklatın `next`.

5. Üzerinde **ASM parola belirtin** sayfasında **bu hesaplar için aynı parolaları kullanıp** seçeneği ve bir parola girin.

   ![Yükleyicinin belirtin ASM parola sayfasının ekran görüntüsü](./media/oracle-asm/install04.png)

6. Üzerinde **yönetim seçeneklerini belirtin** sayfasında EM bulut denetimi yapılandırma seçeneğiniz vardır. Biz bu seçeneği atlanıyor - tıklayın `next` devam etmek için. 

7. Üzerinde **ayrıcalıklı işletim sistemi gruplarına** sayfasında, varsayılan ayarları kullanın. Tıklayın `next` devam etmek için.

8. Üzerinde **yükleme konumu belirtin** sayfasında, varsayılan ayarları kullanın. Tıklayın `next` devam etmek için.

9. Üzerinde **Envanter Oluştur** sayfasında, Envanter dizinine `/u01/app/grid/oraInventory`. Tıklayın `next` devam etmek için.

   ![Yükleyicinin Envanter Oluştur sayfasının ekran görüntüsü](./media/oracle-asm/install08.png)

10. Üzerinde **kök betik yürütme Yapılandırması** sayfasında **otomatik olarak yapılandırma betiklerini çalıştırmak** onay kutusu. Ardından, **"Kök" kullanıcının kimlik bilgilerini** seçenek ve kök kullanıcı parolası girin.

    ![Yükleyicinin kök betik yürütme yapılandırma sayfasının ekran görüntüsü](./media/oracle-asm/install09.png)

11. Üzerinde **önkoşul denetimleri gerçekleştirmek** sayfasında, geçerli Kurulum hatalarla yapılamayacak. Bu beklenen bir davranıştır. `Fix & Check Again` öğesini seçin.

12. İçinde **düzeltme komut dosyası** iletişim kutusu, tıklayın `OK`.

13. Üzerinde **özeti** sayfasında seçtiğiniz ayarları gözden geçirin ve ardından `Install`.

    ![Yükleyicinin Özet sayfasının ekran görüntüsü](./media/oracle-asm/install12.png)

14. Betikleri ayrıcalıklı bir kullanıcısı çalıştırılması gereken bildiren, yapılandırma bir uyarı iletişim kutusu görünür. Tıklayın `Yes` devam etmek için.

15. Üzerinde **son** sayfasında `Close` yüklemeyi bitirmek için.

## <a name="set-up-your-oracle-asm-installation"></a>Oracle ASM yüklemenizi ayarlayın

Oracle ASM yüklemenizi ayarlamak için aşağıdaki adımları tamamlayın:

1. Hala oturum açmaya olarak sağlamak **kılavuz**, öğesinden, X11 oturumu. İsabet gerekebilir `enter` terminal canlandırılması mümkün. Ardından Oracle otomatik Depolama Yönetimi yapılandırma Yardımcısı'nı başlatın:

   ```bash
   cd /u01/app/grid/product/12.1.0/grid/bin
   ./asmca
   ```

   Oracle ASM yapılandırma Yardımcısı'nı açar.

2. İçinde **ASM yapılandırın: Disk grupları** iletişim kutusu, tıklayın `Create` düğmesine ve ardından `Show Advanced Options`.

3. İçinde **Disk grubu oluşturma** iletişim kutusunda:

   - Disk grubu adı girin **veri**.
   - Altında **üye diskleri seçin**seçin **ORCL_DATA** ve **ORCL_DATA1**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Tıklayın `ok` disk grubu oluşturmak için.
   - Tıklayın `ok` doğrulama penceresini kapatmak için.

   ![Disk grubu oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm02.png)

4. İçinde **ASM yapılandırın: Disk grupları** iletişim kutusu, tıklayın `Create` düğmesine ve ardından `Show Advanced Options`.

5. İçinde **Disk grubu oluşturma** iletişim kutusunda:

   - Disk grubu adı girin **FRA**.
   - Altında **Yedeklilik**seçin **dış (hiçbiri)** .
   - Altında **üye diskleri seçin**seçin **ORCL_FRA**.
   - Altında **ayırma birimi boyutu**seçin **4**.
   - Tıklayın `ok` disk grubu oluşturmak için.
   - Tıklayın `ok` doğrulama penceresini kapatmak için.

   ![Disk grubu oluştur iletişim kutusunun ekran görüntüsü](./media/oracle-asm/asm04.png)

6. Seçin **çıkış** ASM yapılandırma Yardımcısı'nı kapatın.

   ![Ekran görüntüsü ASM yapılandırın: Çıkış düğmesi disk gruplar iletişim kutusu](./media/oracle-asm/asm05.png)

## <a name="create-the-database"></a>Veritabanı oluşturma

Oracle veritabanı yazılımını Azure Market görüntüsü üzerinde zaten yüklü. Bir veritabanı oluşturmak için aşağıdaki adımları tamamlayın:

1. Kullanıcılar geçiş için Oracle süper kullanıcı ve günlüğe kaydetme için dinleyici'ı başlatın:

   ```bash
   su - oracle
   cd /u01/app/oracle/product/12.1.0/dbhome_1/bin
   ./dbca
   ```
   Veritabanı yapılandırma Yardımcısı'nı açar.

2. Üzerinde **veritabanı işlemi** sayfasında `Create Database`.

3. Üzerinde **oluşturma modu** sayfası:

   - Veritabanı için bir ad girin.
   - İçin **depolama türü**, olun **otomatik Depolama Yönetimi (ASM)** seçilir.
   - İçin **veritabanı dosyaları konumu**, varsayılan ASM önerilen konum.
   - İçin **Hızlı Kurtarma alanında**, varsayılan ASM önerilen konum.
   - yazın bir **yönetici parolasını** ve **parolayı onaylayın**.
   - olun `create as container database` seçilir.
   - yazın bir `pluggable database name` değeri.

4. Üzerinde **özeti** sayfasında seçtiğiniz ayarları gözden geçirin ve ardından `Finish` veritabanını oluşturmak için.

   ![Özet sayfasının ekran görüntüsü](./media/oracle-asm/createdb03.png)

5. Veritabanı oluşturuldu. Üzerinde **son** sayfasında, bu veritabanını kullanacak ve parolaları değiştirmek için ek hesapların kilidini açmak için seçeneğiniz vardır. Bunu yapmak istiyorsanız seçin **parola yönetimi** -Aksi takdirde tıklayarak `close`.

## <a name="delete-the-vm"></a>VM’yi silin

Azure Marketi Oracle DB görüntüden üzerinde Oracle otomatik Depolama Yönetimi başarıyla yapılandırdınız.  Bu VM artık ihtiyacınız olmadığında kaynak grubunu, VM'yi ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Configure Oracle DataGuard](configure-oracle-dataguard.md)

[Öğretici: Oracle Goldengate'i yapılandırma](Configure-oracle-golden-gate.md)

Gözden geçirme [bir Oracle DB mimarisi hazırlama](oracle-design.md)
