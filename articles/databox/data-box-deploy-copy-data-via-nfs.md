---
title: NFS aracılığıyla Microsoft Azure Data Box için veri kopyalama | Microsoft Docs
description: Azure Data Box'a veri kopyalamayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 11/20/2018
ms.author: alkohli
ms.openlocfilehash: 646acca7eeb2e811f8683a1d35ff8c6efae130da
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319020"
---
# <a name="tutorial-copy-data-to-azure-data-box-via-nfs"></a>Öğretici: Azure Data Box NFS aracılığıyla veri kopyalayın 

Bu öğreticide yerel web arabirimini kullanarak bağlantı kurma, ana bilgisayarınızdan veri kopyalama ve ardından Data Box'ı göndermeye hazırlama adımları anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama
> * Data Box'ı göndermeye hazırlama.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladığınızda [Öğreticisi: Azure Data Box ' ayarlamak](data-box-deploy-set-up.md).
2. Data Box’ınızı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Data Box üzerinden kopyalamak istediğiniz verileri içeren bir ana bilgisayarınız var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanılabilir ancak kopyalama hızı etkilenecektir. 

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

Seçilen depolama hesabına bağlı olarak, Data Box kadar oluşturur:
- İlişkili her depolama hesabına GPv1 ve GPv2 için üç paylaşım.
- Premium veya blob depolama hesabı için bir paylaşım. 

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları paylaşımlarında birinci düzeydeki varlıklar paylaşımlar, ikinci düzeydeki varlıklar ise dosyalardır.

Aşağıdaki örneği inceleyin. 

- Depolama hesabı: *Mystoracct*
- Blok blobu için paylaşımı: *Mystoracct_BlockBlob/my-kapsayıcı/blob*
- Sayfa blobu için paylaşımı: *Mystoracct_PageBlob/my-container/blob*
- Dosya Paylaşımı için: *Mystoracct_AzFile/my-paylaşma*

Linux ana bilgisayarı kullanıyorsanız aşağıdaki adımları gerçekleştirerek Data Box'ı NFS istemcilerine izin verecek şekilde yapılandırın.

1. Paylaşıma erişmesine izin verilen istemcilerin IP adreslerini sağlayın. Yerel web arabiriminde **Bağlan ve kopyala** sayfasına gidin. **NFS ayarları** bölümünde **NFS istemci erişimi**'ne tıklayın. 

    ![NFS istemci erişimini yapılandırma 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. NFS istemcisinin IP adresini girin ve **Ekle**'ye tıklayın. Bu adımı tekrarlayarak birden fazla NFS istemcisi için erişim sağlayabilirsiniz. **Tamam** düğmesine tıklayın.

    ![NFS istemci erişimini yapılandırma 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Linux ana bilgisayarında NFS istemcisinin [desteklenen sürümünün](data-box-system-requirements.md) yüklü olduğundan emin olun. Linux dağıtımınıza uygun sürümü kullanın. 

3. NFS istemcisi yüklendikten sonra, Data Box cihazınızdaki NFS paylaşımını bağlamak için aşağıdaki komutu kullanın:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Aşağıdaki örnekte, Data Box paylaşımına NFS yoluyla nasıl bağlanılacağı gösterilir. Data Box cihaz IP'si `10.161.23.130` şeklindedir, `Mystoracct_Blob` ubuntuVM'ye bağlanmıştır ve bağlama noktası `/home/databoxubuntuhost/databox` ile belirtilmiştir.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Data Box paylaşımlarına bağlandıktan sonra veri kopyalamaya başlayabilirsiniz. Veri kopyalamaya başlamadan önce aşağıdaki noktaları gözden geçirin:

- Verilerin uygun dosya biçimine karşılık gelen paylaşımlara kopyalandığından emin olun. Örneğin blok blobu verilerinin blok blobu paylaşımına kopyalanması gerekir. Veri biçimi uygun paylaşım türüyle eşleşmiyorsa verilerin Azure'a yüklenmesi başarısız olur.
-  Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box sınırları](data-box-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun. 
- Data Box tarafından yüklenen verilerin Data Box haricinde başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.
- Aynı anda hem SMB hem de NFS kullanmamanızı veya aynı verileri Azure'daki aynı uç hedefe kopyalamamanızı öneririz. Bu gibi durumlarda nihai sonucu kestirmek mümkün olmayabilir.

Linux ana bilgisayar kullanıyorsanız Robocopy ile benzer bir kopyalama yardımcı programı kullanabilirsiniz. Linux için kullanabileceğiniz bazı alternatifler: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) veya [Ultracopier](https://ultracopier.first-world.info/).  

`cp` komutu, dizin kopyalamak için en iyi seçeneklerden biridir. Kullanımı hakkında daha fazla bilgi için [cp man sayfalarına](http://man7.org/linux/man-pages/man1/cp.1.html) gidin.

Çok iş parçacığına sahip olan bir kopyalama işlemi için rsync seçeneğini kullanıyorsanız şu yönergeleri izleyin:

 - Linux istemcinizde kullanılan dosya sistemine bağlı olarak **CIFS Utils** veya **NFS Utils** paketini yükleyebilirsiniz.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  **Rsync** ve **Parallel** uygulamalarını yükleyin (Linux dağıtımınıza göre değişir).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Bağlama noktası oluşturun.

    `sudo mkdir /mnt/databox`

 - Birimi bağlayın.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

 - Klasör dizin yapısını kopyalayın.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

 - Dosyaları kopyalayın. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     Burada j paralelleştirme sayısını, X ise paralel kopya sayısını belirtir

     16 paralel kopyayla başlamanızı ve kullanılabilir kaynak durumuna göre iş parçacığı sayısını artırmanızı öneririz.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama
> * Data Box'ı göndermeye hazırlama

Data Box'ınızı Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

