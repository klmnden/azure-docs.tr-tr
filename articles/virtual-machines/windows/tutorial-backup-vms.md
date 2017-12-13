---
title: Yedekleme Azure Windows VM | Microsoft Docs
description: "Windows Vm'lerinizi Azure Yedekleme kullanılarak yedekleyerek koruyun."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 20760650b093216a2929de580f5971c45e0534a8
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makineleri yedekleyin

Düzenli aralıklarla yedekleme yaparak verilerinizi koruyabilirsiniz. Azure yedekleme coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde tüm VM ya da yalnızca belirli dosyaları geri yükleyebilirsiniz. Bu makalede Windows Server ve IIS çalıştıran bir VM'de tek bir dosya geri yükleme açıklanmaktadır. Kullanmak için bir VM zaten sahip değilseniz, kullanarak bir tane oluşturabilirsiniz [Windows Hızlı Başlangıç](quick-create-portal.md). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM bir yedeğini oluşturun
> * Günlük yedekleme zamanlaması
> * Bir dosyayı bir yedekten geri yükleyin




## <a name="backup-overview"></a>Backup’a genel bakış

Azure Backup hizmeti yedekleme işini başlattığında, zaman içinde nokta anlık almak için yedekleme uzantısını tetikler. Azure Backup hizmeti kullandığı _VMSnapshot_ uzantısı. VM çalışıyorsa, uzantısı ilk VM yedekleme sırasında yüklenir. VM çalışmıyorsa bu yana (hiçbir uygulama yazma VM durdurulduğunda oluşur) yedekleme hizmeti temel alınan depolama anlık görüntü alır.

Windows VM görüntüsünü alırken, Backup hizmeti Birim Gölge Kopyası Hizmeti (sanal makine disklerin tutarlı bir anlık görüntü almak için VSS ile) düzenler. Azure Backup hizmeti anlık görüntüsünü alır sonra verileri kasaya aktarılır. Verimliliği en üst düzeye çıkarmak için hizmet tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri blokları aktarır.

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

## <a name="recover-a-file"></a>Bir dosya kurtarma

Yanlışlıkla silme veya bir dosyaya değişiklik, dosya yedekleme Kasası'nı kurtarmak için dosya kurtarma kullanabilirsiniz. Kurtarma noktası yerel sürücü olarak bağlamak için VM'de, çalışan bir komut dosyası kurtarma kullanır. Böylece kurtarma noktasından dosyaları kopyalayın ve VM geri bu sürücüler için 12 saat bağlı kalır.  

Bu örnekte, varsayılan web sayfasını, IIS için kullanılan görüntü dosyasını kurtarmak nasıl gösterir. 

1. Bir tarayıcı açın ve varsayılan IIS sayfasını göstermek için VM IP adresine bağlanın.

    ![Varsayılan IIS web sayfası](./media/tutorial-backup-vms/iis-working.png)

2. VM'ye bağlanın.
3. VM üzerinde açmak **dosya Gezgini** için \inetpub\wwwroot gidin ve dosyayı silin **iisstart.png**.
4. Yerel bilgisayarınızda varsayılan IIS sayfasında görüntü kayboluyor görmek için tarayıcıyı yenileyin.

    ![Varsayılan IIS web sayfası](./media/tutorial-backup-vms/iis-broken.png)

5. Yerel bilgisayarınızda yeni bir sekme açın ve gidin [Azure portal](https://portal.azure.com).
6. Soldaki menüde seçin **sanal makineleri** ve VM listeden seçin.
8. VM dikey olarak **ayarları** 'yi tıklatın **yedekleme**. **Yedekleme** dikey pencere açılır. 
9. Dikey pencerenin üstündeki menüde seçin **dosya kurtarma**. **Dosya kurtarma** dikey pencere açılır.
10. İçinde **1. adım: kurtarma noktası seçin**, açılan listeden bir kurtarma noktası seçin.
11. İçinde **2. adım: indirme göz atın ve dosyaları kurtarmak için komut dosyası**, tıklatın **karşıdan yürütülebilir** düğmesi. Dosyaya kaydedin, **indirmeleri** klasör.
12. Yerel bilgisayarınızda açın **dosya Gezgini** gidin, **indirmeleri** klasörü ve indirilen .exe dosyasını kopyalayın. Dosya adı, VM adıyla önek alacaktır. 
13. VM'nizi (üzerinden RDP bağlantı) üzerinde VM Masaüstü .exe dosyasına yapıştırın. 
14. VM masaüstüne gidin ve üzerinde .exe dosyasını çift tıklatın. Bir komut istemi başlatın ve sonra kurtarma noktası erişebileceğiniz bir dosya paylaşımı bağlayın. Bu tamamlandığında paylaşımı oluşturma, yazın **q** komut istemini kapatın.
15. VM'nizi üzerinde açık **dosya Gezgini** ve dosya paylaşımı için kullanılan sürücü harfi gidin.
16. \İnetpub\wwwroot ve kopyalama **iisstart.png** dosyasından paylaşma ve \inetpub\wwwroot yapıştırabilirsiniz. Örneğin, F:\inetpub\wwwroot\iisstart.png kopyalayın ve dosyayı kurtarmak için c:\inetpub\wwwroot yapıştırın.
17. Yerel bilgisayarınızda, IIS varsayılan sayfasını gösteren VM IP adresine bağlı tarayıcısı sekmesi açın. Tarayıcı sayfayı yenilemek için CTRL + F5 tuşuna basın. Görüntünün geri yüklendiğini görmelisiniz.
18. Yerel bilgisayarınızda Azure portalında hem de tarayıcı sekmesinde geri dönün **3. adım: Kurtarma işleminden sonra diskleri çıkarın** tıklatın **çıkarın diskleri** düğmesi. Bu adımı gerçekleştirmenin unutursanız, mountpoint bağlantısı 12 saat sonra otomatik olarak kapat. Bu 12 saat sonra yeni bir başlatma noktası oluşturmak için yeni bir komut dosyası yüklemeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VM bir yedeğini oluşturun
> * Günlük yedekleme zamanlaması
> * Bir dosyayı bir yedekten geri yükleyin

Sanal makineler izleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitoring.md)









