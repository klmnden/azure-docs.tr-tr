---
title: Azure Data Box NFS aracılığıyla veri kopyalamak için öğretici | Microsoft Docs
description: NFS aracılığıyla Azure Data Box için veri kopyalama hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: alkohli
ms.openlocfilehash: 55c880d0e795873a759ba121b6de0c98c5249061
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924481"
---
# <a name="tutorial-copy-data-to-azure-data-box-via-nfs"></a>Öğretici: Azure Data Box NFS aracılığıyla veri kopyalayın

Bu öğreticide bağlanın ve yerel web UI aracılığıyla ana bilgisayardan veri kopyalama işlemini açıklamaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladığınızda [Öğreticisi: Azure Data Box ' ayarlamak](data-box-deploy-set-up.md).
2. Data Box’ınızı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Data Box üzerinden kopyalamak istediğiniz verileri içeren bir ana bilgisayarınız var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı kullanılabilir değilse, 1 GbE veri bağlantısı kullanılabilir, ancak kopyalama hızı etkilenir. 

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

Seçilen depolama hesabına bağlı olarak, Data Box kadar oluşturur:
- İlişkili her depolama hesabına GPv1 ve GPv2 için üç paylaşım.
- Premium depolama için bir paylaşım. 
- Blob depolama hesabı için bir paylaşım. 

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları paylaşımlarında birinci düzeydeki varlıklar paylaşımlar, ikinci düzeydeki varlıklar ise dosyalardır.

Aşağıdaki tabloda UNC yolunu paylaşımları burada veriler karşıya yüklendikten, Data Box'ı ve Azure depolama yolu URL üzerinde gösterir. Son Azure depolama yolu URL'si UNC paylaşımı yolundan elde edilebilir.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure blok BLOB'ları | <li>Paylaşımları UNC yolu: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure sayfa blobları  | <li>Paylaşımları UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımları UNC yolu: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

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
    
    Mac istemcileri için ek bir seçenek aşağıdaki gibi eklemeniz gerekecektir: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

    **Her zaman kopyalamayı düşündüğünüz dosyalar için paylaşımda bir klasör oluşturun ve ardından dosyaları bu klasöre kopyalayın**. Blok blobu altında klasör oluşturulur ve sayfa blob paylaşımları veriler BLOB olarak karşıya bir kapsayıcıyı temsil eder. Dosyaları doğrudan kopyalanamıyor *kök* depolama hesabında klasör.

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Data Box paylaşımlarına bağlandıktan sonra veri kopyalamaya başlayabilirsiniz. Veri kopyalama başlamadan önce aşağıdaki konuları gözden geçirin:

- Verilerin uygun dosya biçimine karşılık gelen paylaşımlara kopyalandığından emin olun. Örneğin blok blobu verilerinin blok blobu paylaşımına kopyalanması gerekir. VHD'ler sayfa BLOB'ları için kopyalayın. Veri biçimi uygun paylaşım türüyle eşleşmiyorsa verilerin Azure'a yüklenmesi başarısız olur.
-  Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box sınırları](data-box-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun. 
- Data Box tarafından yüklenen verilerin Data Box haricinde başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.
- Aynı anda hem SMB hem de NFS kullanmamanızı veya aynı verileri Azure'daki aynı uç hedefe kopyalamamanızı öneririz. Bu gibi durumlarda nihai sonucu kestirmek mümkün olmayabilir.
- **Her zaman kopyalamayı düşündüğünüz dosyalar için paylaşımda bir klasör oluşturun ve ardından dosyaları bu klasöre kopyalayın**. Blok blobu altında klasör oluşturulur ve sayfa blob paylaşımları veriler BLOB olarak karşıya bir kapsayıcıyı temsil eder. Dosyaları doğrudan kopyalanamıyor *kök* depolama hesabında klasör.

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

> [!IMPORTANT]
> Aşağıdaki Linux dosya türlerinde desteklenmez: sembolik bağlantılar, karakter dosyası, blok dosyaları, yuva ve kanallar. Bu dosya türlerini hatalarda sonuçlanır **göndermeye hazırlama** adım.

- Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
   ![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama


Data Box'ınızı Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

