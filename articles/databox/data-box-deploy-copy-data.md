---
title: Verilerinizi Microsoft Azure Data Box'a kopyalama | Microsoft Docs
description: Azure Data Box'a veri kopyalamayı öğrenin
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 0204445464a9d61b4e25be1d71373ce8394b32f0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957680"
---
# <a name="tutorial-copy-data-to-azure-data-box"></a>Öğretici: Azure Data Box'a veri kopyalama 

Bu öğreticide yerel web arabirimini kullanarak bağlantı kurma, ana bilgisayarınızdan veri kopyalama ve ardından Data Box'ı göndermeye hazırlama adımları anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama
> * Data Box'ı göndermeye hazırlama.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğretici: Azure Data Box'ı kurma](data-box-deploy-set-up.md) konusunu tamamladınız.
2. Data Box’ınızı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. Data Box üzerinden kopyalamak istediğiniz verileri içeren bir ana bilgisayarınız var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanılabilir ancak kopyalama hızı etkilenecektir. 

## <a name="connect-to-data-box"></a>Data Box'a bağlanma

Seçilen depolama hesabına bağlı olarak, Data Box şunların tamamını veya bir bölümünü oluşturabilir:
- İlişkili her depolama hesabı için üç paylaşım (GPv1 ve GPv2).
- Premium veya blob depolama hesabı için bir paylaşım. 

Blok blobu ve sayfa blobu paylaşımlarının altında birinci düzeydeki varlıklar kapsayıcılar, ikinci düzeydeki varlıklar ise bloblardır. Azure Dosyaları paylaşımlarında birinci düzeydeki varlıklar paylaşımlar, ikinci düzeydeki varlıklar ise dosyalardır.

Aşağıdaki örneği inceleyin. 

- Depolama hesabı: *Mystoracct*
- Blok blobu için paylaşım: *Mystoracct_BlockBlob/my-container/blob*
- Sayfa blobu için paylaşım: *Mystoracct_PageBlob/my-container/blob*
- Dosya için paylaşım: *Mystoracct_AzFile/my-share*

Bağlantı ve veri kopyalama adımları Data Box'ınızın Windows Server veya Linux ana bilgisayara bağlı olma durumuna göre değişebilir.

### <a name="connect-via-smb"></a>SMB ile bağlanma 

Windows Server ana bilgisayarı kullanıyorsanız Data Box'a bağlanmak için aşağıdaki adımları gerçekleştirin.

1. İlk adım kimlik doğrulamasından geçmek ve oturum başlatmaktır. **Bağlan ve kopyala**'ya gidin. Depolama hesabınızla ilişkilendirilmiş paylaşımların erişim kimlik bilgilerini almak için **Kimlik bilgilerini al**'a tıklayın. 

    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. Paylaşıma erişme ve veri kopyalama iletişim kutusunda paylaşıma karşılık gelen **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. **Tamam** düğmesine tıklayın.
    
    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Depolama hesabınızla ilişkilendirilmiş paylaşımlara erişin (aşağıdaki örnekte Mystoracct). Paylaşımlara erişmek için `\\<IP of the device>\ShareName` yolunu kullanın. Veri biçiminize bağlı olarak şu adresleri kullanarak paylaşımlara bağlanın (paylaşım adını kullanın): 
    - *\\<IP address of the device>\Mystoracct_Blob*
    - *\\<IP address of the device>\Mystoracct_Page*
    - *\\<IP address of the device>\Mystoracct_AzFile*
    
    Paylaşımlara ana bilgisayarınızdan bağlanmak için bir komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    İstendiğinde paylaşımın parolasını girin. Aşağıdaki örnekte yukarıdaki komutla paylaşıma bağlanma adımları gösterilmektedir.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>` değerini belirtin. **Tamam** düğmesine tıklayın. Dosya Gezgini açılır.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

5. Artık paylaşımları klasörler olarak görebiliyor olmalısınız. Kopyalamak istediğiniz dosyalar için bir klasör oluşturun (bu örnekte şablonlar). Bazen klasörlerde gri renkli çarpı işareti görünebilir. Bu çarpı işareti hata anlamına gelmez. Klasörler uygulama tarafından durum takibi amacıyla işaretlenir.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) 

### <a name="connect-via-nfs"></a>NFS ile bağlanma 

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

### <a name="copy-data-via-smb"></a>SMB ile veri kopyalama

SMB paylaşımına bağlandıktan sonra verileri kopyalamaya başlayabilirsiniz. 

Verilerinizi kopyalamak için Robocopy gibi SMB uyumlu herhangi bir dosya kopyalama aracını kullanabilirsiniz. Robocopy ile birden fazla kopyalama işlemini başlatabilirsiniz. Aşağıdaki komutu kullanın:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 Öznitelikler aşağıdaki tabloda açıklanmıştır.
    
|Öznitelik  |Açıklama  |
|---------|---------|
|/e     |Boş dizinler dahil olmak üzere alt dizinleri kopyalar.         |
|/r:     |Başarısız kopyalama işlemleri için yeniden deneme sayısını belirtir.         |
|/w:     |Yeniden deneme işlemleri arasındaki bekleme süresini saniye cinsinden belirtir.         |
|/is     |Aynı dosyaları dahil eder.         |
|/nfl     |Dosya adlarının günlüğü alınmayacağını belirtir.         |
|/ndl    |Dizin adlarının günlüğü alınmayacağını belirtir.        |
|/np     |Kopyalama işleminin ilerleme durumunun (kopyalanan dosya veya dizin sayısının) görüntülenmeyeceğini belirtir. İlerleme durumunun gösterilmesi performansı önemli ölçüde düşürür.         |
|/MT     | Çoklu iş parçacığı kullanılır, 32 veya 64 iş parçacığı önerilir. Bu seçenek şifrelenmiş dosyalarla kullanılmaz. Şifrelenmiş ve şifrelenmemiş dosyaları ayırmanız gerekebilir. Ancak tek iş parçacığına sahip kopyalama işlemi performansı önemli ölçüde düşürür.           |
|/fft     | Herhangi bir dosya sistemi için zaman damgası ayrıntı düzeyini azaltmak için kullanın.        |
|/b     | Dosyaları Yedekleme modunda kopyalar.        |
|/z    | Dosyaları Yeniden başlatma modunda kopyalar, kararsız ortamlarda kullanmanız önerilir. Bu işlem ek günlük kaydı nedeniyle aktarım hızını düşürür.      |
| /zb     | Yeniden başlatma modunu kullanır. Erişim reddedilirse bu seçenek Yedekleme modunu kullanır. Bu işlem denetim noktası oluşturma nedeniyle aktarım hızını düşürür.         |
|/efsraw     | Şifrelenmiş dosyaların tümünü EFS ham modunda kopyalar. Yalnızca şifrelenmiş dosyalarda kullanın.         |
|log+:<LogFile>| Çıkışı var olan günlük dosyasına ekler.|    
 
Aşağıdaki örnekte dosyaları Data Box'a kopyalamak için kullanılan Robocopy komutunun çıkışı gösterilmektedir.
    
    C:\Users>robocopy
        -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:53 PM
            Simple Usage :: ROBOCOPY source destination /MIR
    
                    source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                    /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:32
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
            Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
        Options : *.* /DCOPY:DA /COPY:DAT /MT:32 /R:5 /W:60
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                    Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
        Files :        17        17         0         0         0         0
        Bytes :     3.9 k     3.9 k         0         0         0         0          
    C:\Users>
       

Performansı iyileştirmek için veri kopyalama sırasında aşağıdaki Robocopy parametrelerini kullanın.

|    Platform    |    Çoğunlukla küçük dosyalar, 512 KB altı                           |    Çoğunlukla orta büyüklükteki dosyalar, 512 KB-1 MB arası                      |    Çoğunlukla büyük dosyalar, 1 MB üzeri                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy oturumu <br> Oturum başına 16 iş parçacığı    |    3 Robocopy oturumu <br> Oturum başına 16 iş parçacığı    |    2 Robocopy oturumu <br> Oturum başına 24 iş parçacığı    |  |


Robocopy komutu hakkında daha fazla bilgi için bkz. [Robocopy ve birkaç örnek](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için hata dosyalarını indirin.

Veri bütünlüğünü sağlamak için sağlama toplamı veri kopyalama sırasında satır içinde hesaplanır. Kopyalama tamamlandıktan sonra cihazınızdaki kullanılan alanı ve boş alanı doğrulayın.
    
   ![Panoda boş ve kullanılan alanı doğrulama](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)

### <a name="copy-data-via-nfs"></a>NFS ile veri kopyalama

Linux ana bilgisayar kullanıyorsanız Robocopy ile benzer bir kopyalama yardımcı programı kullanabilirsiniz. Linux için kullanabileceğiniz bazı alternatifler: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) veya [Ultracopier](https://ultracopier.first-world.info/).  

cp komutu, dizin kopyalamak için kullanılabilecek en iyi seçeneklerden biridir. Kullanımı hakkında daha fazla bilgi için [cp man sayfalarına](http://man7.org/linux/man-pages/man1/cp.1.html) gidin.

Çok iş parçacığına sahip olan bir kopyalama işlemi için rsync seçeneğini kullanıyorsanız şu yönergeleri izleyin:

 - Linux istemcinizde kullanılan dosya sistemine bağlı olarak **CIFS Utils** veya **NFS Utils** paketini yükleyebilirsiniz.

    `sudo apt-get install cifs-utils` `sudo apt-get install nfs-utils`

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

Son adım cihazı göndermeye hazırlamaktır. Bu adımda tüm cihaz paylaşımları çevrimdışı duruma getirilir. Cihazı göndermeye hazırlamaya başladıktan sonra paylaşımlara erişim sağlayamazsınız.
1. **Göndermeye hazırlama** sayfasına gidip **Hazırlamayı başlat**'a tıklayın. 
   
    ![Göndermeye hazırlama 1](media/data-box-deploy-copy-data/prepare-to-ship1.png)

2. Sağlama toplamı etkinleştirilmediyse bu özelliği etkinleştirme seçeneği sunulur. Veri bütünlüğü açısından sağlama toplamı doğrulamasını gerçekleştirmenizi öneririz. **Sağlama toplamını etkinleştir**'i seçtiğinizde sağlama toplamı hesaplaması başlatılır ve bu işlemin süresi verilerinizin boyutuna göre değişir. **Hazırlamayı başlat**'a tıklayın.
    1. Göndermeye hazırlama aşamasında cihaz paylaşımları çevrimdışı duruma geçer ve cihaz kilitlenir.
        
        ![Göndermeye hazırlama 1](media/data-box-deploy-copy-data/prepare-to-ship2.png) 
   
    2. Cihaz hazırlığı tamamlandıktan sonra cihaz durumu *Göndermeye hazır* olarak değişir. 
        
        ![Göndermeye hazırlama 1](media/data-box-deploy-copy-data/prepare-to-ship3.png)

    3. Bu işlem sırasında kopyalanan dosyaların listesini (bildirim) indirin. Daha sonra bu listeyi kullanarak Azure'a yüklenen dosyaları doğrulayabilirsiniz.
        
        ![Göndermeye hazırlama 1](media/data-box-deploy-copy-data/prepare-to-ship4.png)

3. Cihazı kapatın. **Kapat veya yeniden başlat** sayfasına gidip **Kapat**'a tıklayın. Onayınız istendiğinde devam etmek için **Tamam**'a tıklayın.
4. Kabloları sökün. Bir sonraki adım cihazı Microsoft'a göndermektir.

 
<!--## Appendix - robocopy parameters

This section describes the robocopy parameters used when copying the data to optimize the performance.

|    Platform    |    Mostly small files < 512 KB                           |    Mostly medium  files 512 KB-1 MB                      |    Mostly large files > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy sessions <br> 16 threads per sessions    |    3 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 24 threads per sessions    |  |
|    Data Box Heavy     |    6 Robocopy sessions <br> 24 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |   
|    Data Box Disk         |    4 Robocopy sessions <br> 16 threads per sessions             |    2 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 16 threads per sessions    |   
-->

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box'a bağlanma
> * Data Box'a veri kopyalama
> * Data Box'ı göndermeye hazırlama

Data Box'ınızı kurma ve veri kopyalama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)

