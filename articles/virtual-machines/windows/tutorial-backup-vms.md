---
title: Yedekleme Azure Windows VM | Microsoft Docs
description: Windows Vm'lerinizi Azure Yedekleme kullanılarak yedekleyerek koruyun.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 12859bf967cf8de1b57ab9dfd5c0bd080806f2eb
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makineleri yedekleyin

Düzenli aralıklarla yedekleme yaparak verilerinizi koruyabilirsiniz. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yükleme yaptığınızda VM’nin tamamını veya belirli dosyaları geri yükleyebilirsiniz. Bu makalede Windows Server ve IIS çalıştıran bir VM'de tek bir dosya geri yükleme açıklanmaktadır. Kullanmak için bir VM zaten sahip değilseniz, kullanarak bir tane oluşturabilirsiniz [Windows Hızlı Başlangıç](quick-create-portal.md). Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir VM’nin yedeğini oluşturma
> * Günlük yedekleme zamanlama
> * Bir yedeklemeden bir dosyayı geri yükleme




## <a name="backup-overview"></a>Backup’a genel bakış

Azure Backup hizmeti yedekleme işini başlattığında, zaman içinde nokta anlık almak için yedekleme uzantısını tetikler. Azure Backup hizmeti kullandığı _VMSnapshot_ uzantısı. Uzantı, VM’nin çalışıyor olması durumunda ilk VM yedeklemesi sırasında yüklenir. VM çalışmıyorsa Backup hizmeti, temel alınan depolamanın anlık görüntüsünü alır (VM durduğunda herhangi bir uygulama yazma işlemi gerçekleşmediği için).

Windows VM görüntüsünü alırken, Backup hizmeti Birim Gölge Kopyası Hizmeti (sanal makine disklerin tutarlı bir anlık görüntü almak için VSS ile) düzenler. Azure Backup hizmeti anlık görüntüyü aldıktan sonra veriler kasaya aktarılır. Verimliliği maksimuma çıkarmak için hizmet yalnızca bir önceki yedeklemeden itibaren değişmiş olan veri bloklarının aktarımını yapar.

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.


## <a name="create-a-backup"></a>Yedekleme oluşturma
Kurtarma Hizmetleri Kasasına basit bir zamanlanmış günlük yedekleme oluşturma. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol taraftaki menüden **Sanal makineler**'i seçin. 
3. Listeden yedekleyeceğiniz VM'yi seçin.
4. VM dikey penceresinde, **Ayarlar** bölümünden **Yedekleme**’ye tıklayın. **Yedeklemeyi etkinleştir** dikey penceresi açılır.
5. **Kurtarma Hizmetleri kasası**’nda **Yeni oluştur**’a tıklayıp yeni kasa için ad belirtin. Sanal makineyle aynı Kaynak Grubunda ve konumda yeni bir kasa oluşturulur.
6. **Yedekleme ilkesi**’ne tıklayın. Bu örnek için varsayılanları tutun ve **Tamam**’a tıklayın.
7. **Yedeklemeyi etkinleştir** dikey penceresinde **Yedeklemeyi Etkinleştir** seçeneğine tıklayın. Bu işlem, varsayılan zamanlamaya göre günlük bir yedekleme oluşturur.
10. Başlangıç kurtarma noktası oluşturmak için **Yedekleme** dikey penceresinde **Şimdi yedekle** seçeneğine tıklayın.
11. **Şimdi Yedekle** dikey penceresinde takvim simgesine tıklayın, bu kurtarma noktasının korunduğu son günü seçmek için takvim denetimini kullanın ve **Yedekleme**’ye tıklayın.
12. VM’nize yönelik **Yedekleme** dikey penceresinde tamamlanmış kurtarma noktalarının sayısını görürsünüz.

    ![Kurtarma noktaları](./media/tutorial-backup-vms/backup-complete.png)
    
İlk yedekleme yaklaşık olarak 20 dakika sürer. Yedeklemeniz tamamlandıktan sonra bu öğreticinin bir sonraki kısmına geçin.

## <a name="recover-a-file"></a>Bir dosya kurtarma

Bir dosyayı yanlışlıkla siler veya dosyada yanlışlıkla değişiklik yaparsanız dosyayı yedekleme kasanızdan kurtarmak için Dosya Kurtarma’yı kullanabilirsiniz. Kurtarma noktası yerel sürücü olarak bağlamak için VM'de, çalışan bir komut dosyası kurtarma kullanır. Bu sürücüler, kurtarma noktasından dosyaları kopyalayıp VM’ye geri yükleyebilmeniz için 12 saat boyunca takılı kalır.  

Bu örnekte, varsayılan web sayfasını, IIS için kullanılan görüntü dosyasını kurtarmak nasıl gösterir. 

1. Bir tarayıcı açın ve varsayılan IIS sayfasını göstermek için VM IP adresine bağlanın.

    ![Varsayılan IIS web sayfası](./media/tutorial-backup-vms/iis-working.png)

2. VM'ye bağlanın.
3. VM üzerinde açmak **dosya Gezgini** için \inetpub\wwwroot gidin ve dosyayı silin **iisstart.png**.
4. Yerel bilgisayarınızda varsayılan IIS sayfasında görüntü kayboluyor görmek için tarayıcıyı yenileyin.

    ![Varsayılan IIS web sayfası](./media/tutorial-backup-vms/iis-broken.png)

5. Yerel bilgisayarınızda yeni bir sekme açın ve gidin [Azure portal](https://portal.azure.com).
6. Soldaki menüde seçin **sanal makineleri** ve VM listeden seçin.
8. VM dikey penceresinde, **Ayarlar** bölümünden **Yedekleme**’ye tıklayın. **Yedekleme** dikey penceresi açılır. 
9. Dikey pencerenin üst tarafındaki menüde **Dosya Kurtarma** seçeneğini belirleyin. **Dosya Kurtarma** dikey penceresi açılır.
10. **1. Adım: Kurtarma noktasını seçme** bölümünde, açılır menüden bir kurtarma noktası seçin.
11. **2. Adım: Dosyalara göz atmak ve kurtarmak için betiği indirme indirme** bölümünde **Yürütülebilir Dosyayı İndir** düğmesine tıklayın. Dosyaya kaydedin, **indirmeleri** klasör.
12. Yerel bilgisayarınızda açın **dosya Gezgini** gidin, **indirmeleri** klasörü ve indirilen .exe dosyasını kopyalayın. Dosya adı, VM adıyla önek alacaktır. 
13. VM'nizi (üzerinden RDP bağlantı) üzerinde VM Masaüstü .exe dosyasına yapıştırın. 
14. VM masaüstüne gidin ve üzerinde .exe dosyasını çift tıklatın. Bir komut istemi başlatın ve sonra kurtarma noktası erişebileceğiniz bir dosya paylaşımı bağlayın. Bu tamamlandığında paylaşımı oluşturma, yazın **q** komut istemini kapatın.
15. VM'nizi üzerinde açık **dosya Gezgini** ve dosya paylaşımı için kullanılan sürücü harfi gidin.
16. \İnetpub\wwwroot ve kopyalama **iisstart.png** dosyasından paylaşma ve \inetpub\wwwroot yapıştırabilirsiniz. Örneğin, F:\inetpub\wwwroot\iisstart.png kopyalayın ve dosyayı kurtarmak için c:\inetpub\wwwroot yapıştırın.
17. Yerel bilgisayarınızda, IIS varsayılan sayfasını gösteren VM IP adresine bağlı tarayıcısı sekmesi açın. Tarayıcı sayfasını yenilemek için CTRL + F5 tuşlarına basın. Görüntünün geri yüklendiğini görmelisiniz.
18. Yerel bilgisayarınızda Azure portalı için tarayıcı sekmesine geri dönün ve **3. Adım: Kurtarma işleminden sonra diskleri çıkarma** bölümünde **Diskleri Çıkar** düğmesine tıklayın. Bu adımı gerçekleştirmenin unutursanız, mountpoint bağlantısı 12 saat sonra otomatik olarak kapat. Bu 12 saatin ardından yeni takma noktası oluşturmak için yeni bir betik indirmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir VM’nin yedeğini oluşturma
> * Günlük yedekleme zamanlama
> * Bir yedeklemeden bir dosyayı geri yükleme

Sanal makineleri izleme hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal makineleri yönetme](tutorial-govern-resources.md)









