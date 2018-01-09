---
title: Yedekleme Azure Linux VM'ler | Microsoft Docs
description: "Linux Vm'lerinizi Azure Yedekleme kullanılarak yedekleyerek koruyun."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2eb0958169b175813b0dca775e9250da1cb364d4
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Azure'daki Linux sanal makineleri yedekleyin

Düzenli aralıklarla yedekleme yaparak verilerinizi koruyabilirsiniz. Azure yedekleme coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde tüm VM ya da yalnızca belirli dosyaları geri yükleyebilirsiniz. Bu makalede, bir Linux VM nginx çalıştıran tek bir dosya geri yükleme açıklanmaktadır. Kullanmak için bir VM zaten sahip değilseniz, kullanarak bir tane oluşturabilirsiniz [Linux quickstart](quick-create-cli.md). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM bir yedeğini oluşturun
> * Günlük yedekleme zamanlaması
> * Bir dosyayı bir yedekten geri yükleyin



## <a name="backup-overview"></a>Backup’a genel bakış

Azure Backup hizmeti yedekleme başlattığında, zaman içinde nokta anlık almak için yedekleme uzantısını tetikler. Azure Backup hizmeti kullandığı _VMSnapshotLinux_ Linux uzantı. VM çalışıyorsa, uzantısı ilk VM yedekleme sırasında yüklenir. VM çalışmıyorsa bu yana (hiçbir uygulama yazma VM durdurulduğunda oluşur) yedekleme hizmeti temel alınan depolama anlık görüntü alır.

Varsayılan olarak, Azure Backup dosya sisteminin tutarlı bir yedeklemenin Linux VM için alır ancak yapılacak yapılandırılabilir [uygulama tutarlı yedekleme öncesi betik ve sonrası betik framework kullanarak](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Azure Backup hizmeti anlık görüntüsünü alır sonra verileri kasaya aktarılır. Verimliliği en üst düzeye çıkarmak için hizmet tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri blokları aktarır.

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.


## <a name="create-a-backup"></a>Bir yedekleme oluşturun
Kurtarma Hizmetleri Kasasına basit bir zamanlanmış günlük yedekleme oluşturma. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüden **Sanal makineler**'i seçin. 
3. Listeden yedekleyeceğiniz VM'yi seçin.
4. VM dikey olarak **ayarları** 'yi tıklatın **yedekleme**. **Yedeklemeyi etkinleştir** dikey pencere açılır.
5. İçinde **kurtarma Hizmetleri kasası**, tıklatın **Yeni Oluştur** ve yeni kasa adını sağlayın. Yeni bir kasa aynı kaynak grubunu ve konumu sanal makine olarak oluşturulur.
6. Tıklatın **yedekleme İlkesi**. Bu örnek için varsayılanları tutun ve **Tamam**.
7. Üzerinde **yedeklemeyi etkinleştir** dikey penceresinde tıklatın **yedeklemeyi etkinleştir**. Bu varsayılan zamanlamaya göre günlük yedekleme oluşturur.
10. İlk kurtarma noktası oluşturmak için **yedekleme** dikey penceresinde **Şimdi Yedekle**.
11. Üzerinde **Şimdi Yedekle** dikey penceresinde, takvim simgesini tıklatın, bu kurtarma noktası korunur ve tıklatın son gününü seçmek için Takvim denetimi kullanın **yedekleme**.
12. İçinde **yedekleme** dikey penceresinde, VM için tam kurtarma noktası sayısını görürsünüz.

    ![Kurtarma noktaları](./media/tutorial-backup-vms/backup-complete.png)

İlk yedek yaklaşık 20 dakika sürer. Yedekleme tamamlandıktan sonra bu öğreticinin sonraki bölümüne geçin.

## <a name="restore-a-file"></a>Bir dosya geri yükleme

Yanlışlıkla silme veya bir dosyaya değişiklik, dosya yedekleme Kasası'nı kurtarmak için dosya kurtarma kullanabilirsiniz. Kurtarma noktası olarak yerel bir sürücü bağlama VM'de, çalışan bir komut dosyası kurtarma kullanır. Böylece kurtarma noktasından dosyaları kopyalayın ve VM geri bu sürücüler için 12 saat bağlı kalır.  

Bu örnekte, varsayılan nginx web sayfası /var/www/html/index.nginx-debian.html kurtarmak nasıl gösterir. Bu örnekte bizim VM ortak IP adresi *13.69.75.209*. VM olanağını kullanarak IP adresi bulabilirsiniz:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. Yerel bilgisayarınızda varsayılan nginx web sayfasını görmek için VM ortak IP adresi tarayıcı ve türü açın.

    ![Varsayılan nginx web sayfası](./media/tutorial-backup-vms/nginx-working.png)

1. VM içine SSH.

    ```bash
    ssh 13.69.75.209
    ```
2. /Var/www/HTML/index.nginx-debian.HTML silin.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. Yerel bilgisayarınızda basarsa tarafından CTRL + F5 bu varsayılan nginx sayfası kayboluyor görmek için tarayıcıyı yenileyin.

    ![Varsayılan nginx web sayfası](./media/tutorial-backup-vms/nginx-broken.png)
    
1. Yerel bilgisayarınızda oturum açın [Azure portal](https://portal.azure.com/).
6. Sol taraftaki menüden **Sanal makineler**'i seçin. 
7. Listesinden VM'yi seçin.
8. VM dikey olarak **ayarları** 'yi tıklatın **yedekleme**. **Yedekleme** dikey pencere açılır. 
9. Dikey pencerenin üstündeki menüde seçin **dosya kurtarma**. **Dosya kurtarma** dikey pencere açılır.
10. İçinde **1. adım: kurtarma noktası seçin**, açılan listeden bir kurtarma noktası seçin.
11. İçinde **2. adım: indirme göz atın ve dosyaları kurtarmak için komut dosyası**, tıklatın **karşıdan yürütülebilir** düğmesi. İndirilen dosyayı yerel bilgisayarınıza kaydedin.
7. Tıklatın **karşıdan yükleme komut dosyası** komut dosyasını yerel olarak yüklemek için.
8. Bash istemi açın ve aşağıdaki komutu yazın, değiştirme *Linux_myVM_05 05 2017.sh* doğru yol ve indirdiğiniz, komut dosyası için dosya adı ile *azureuser* veVMiçinkullanıcıadıile*13.69.75.209* , VM için genel IP adresi ile.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. Yerel bilgisayarınızda, bir SSH bağlantısı VM açın.

    ```bash
    ssh 13.69.75.209
    ```
    
10. VM'nizi üzerinde ekleme yürütme izinlerinin komut dosyası.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. VM'nizi üzerinde kurtarma noktası bir dosya sistemi olarak bağlamak için komut dosyasını çalıştırın.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. Komut dosyası çıktısı için bağlama noktası yolunu sağlar. Çıktı şuna benzer:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. VM üzerinde geri burada dosya sildiğiniz için nginx varsayılan web sayfası bağlama noktasını kopyalayın.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. Yerel bilgisayarınızda, nginx varsayılan sayfasını gösteren VM IP adresine bağlı tarayıcısı sekmesi açın. Tarayıcı sayfayı yenilemek için CTRL + F5 tuşuna basın. Varsayılan sayfa yeniden çalıştığını görmelisiniz.

    ![Varsayılan nginx web sayfası](./media/tutorial-backup-vms/nginx-working.png)

18. Yerel bilgisayarınızda Azure portalında hem de tarayıcı sekmesinde geri dönün **3. adım: Kurtarma işleminden sonra diskleri çıkarın** tıklatın **çıkarın diskleri** düğmesi. Bu adımı gerçekleştirmenin unutursanız, mountpoint bağlantısı 12 saat sonra otomatik olarak kapatılır. Bu 12 saat sonra yeni bir başlatma noktası oluşturmak için yeni bir komut dosyası yüklemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VM bir yedeğini oluşturun
> * Günlük yedekleme zamanlaması
> * Bir dosyayı bir yedekten geri yükleyin

Sanal makineler izleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitoring.md)

