---
title: Verilerinizi Microsoft Azure Data Box Disk'e kopyalama | Microsoft Docs
description: Azure Data Box Disk'inizi nasıl veri kopyalayacağınızı öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 75a78e303991e5426c97b8ceb0eb1375e03be2a2
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56868196"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Öğretici: Veri için Azure Data Box Disk kopyalama ve doğrulayın

Bu öğreticide ana bilgisayarınızdan veri kopyalama ve veri bütünlüğünü doğrulamak için sağlama toplamı oluşturma adımları anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Verileri doğrulama

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:
- Tamamladığınızda [Öğreticisi: Yükleme ve yapılandırma, Azure Data Box Disk](data-box-disk-deploy-set-up.md).
- Disklerinizin kilitleri açılır ve diskler bir istemci bilgisayara bağlanır.
- Disklere veri kopyalamak için kullanılan istemci bilgisayar [Desteklenen işletim sistemi](data-box-disk-system-requirements.md##supported-operating-systems-for-clients) çalıştırmalıdır.
- Verileriniz için hedeflenen depolama türünün [Desteklenen depolama türleri](data-box-disk-system-requirements.md#supported-storage-types) ile eşleştiğinden emin olun.


## <a name="copy-data-to-disks"></a>Disklere veri kopyalama

Bilgisayarınızla Data Box Disk arasında bağlantı kurmak ve veri kopyalamak için aşağıdaki adımları gerçekleştirin.

1. Kilidi açılan sürücünün içeriğini görüntüleyin.

    ![Sürücü içeriğini görüntüleme](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Blok blobu olarak içeri aktarılması gereken verileri BlockBlob klasörüne kopyalayın. Benzer şekilde VHD/VHDX gibi verileri de PageBlob klasörüne kopyalayın. 

    BlockBlob ve PageBlob klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. BlockBlob ve PageBlob klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır. `$root` kapsayıcısındaki tüm dosyalar her zaman blok blobu olarak yüklenir.

    Kök dizinde dosya ve klasörler varsa veri kopyalama işlemine başlamadan önce bunları farklı bir klasöre taşımanız gerekir.

    Kapsayıcı ve blob adlarıyla ilgili Azure adlandırma gereksinimlerine bakın.

    #### <a name="azure-naming-conventions-for-container-and-blob-names"></a>Kapsayıcı ve blob adları için Azure adlandırma kuralları
    |Varlık   |Kurallar  |
    |---------|---------|
    |Blok blobu ve sayfa blobu için kapsayıcı adları     |Bir harf veya rakamla başlamalıdır ve yalnızca küçük harf, rakam ve kısa çizgi (-) içerebilir. Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. Adlarda kısa çizgiler art arda kullanılamaz. <br>3 ila 63 karakter uzunluğunda geçerli bir DNS adı olmalıdır.          |
    |Blok blobu ve sayfa blobu için blob adları    |Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. <br>Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır.<br>Ayrılmış URL karakterleri doğru şekilde atlanmalıdır.<br>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir.         |

    > [!IMPORTANT] 
    > Tüm kapsayıcıların ve blobların [Azure adlandırma kurallarına](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions) uygun olması gerekir. Bu kurallara uyulmaması halinde veriler Azure'a yüklenemez.

3. Dosyaları kopyalarken dosya boyutlarının blok blobları için en fazla ~4,7 TiB, sayfa blobları için ise en fazla ~8 TiB olduğundan emin olun. 
4. Verileri kopyalamak için Veri Gezgini'nde sürükle ve bırak komutlarını kullanabilirsiniz. Verilerinizi kopyalamak için Robocopy gibi SMB uyumlu herhangi bir dosya kopyalama aracını da kullanabilirsiniz. Aşağıdaki Robocopy komutunu kullanarak birden fazla kopyalama işlemini başlatabilirsiniz:

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Komut parametreleri ve seçenekleri aşağıda belirtilmiştir:
    
    |Parametreler/Seçenekler  |Açıklama |
    |--------------------|------------|
    |Kaynak            | Kaynak dizin yolunu belirtir.        |
    |Hedef       | Hedef dizin yolunu belirtir.        |
    |/E                  | Boş dizinler dahil olmak üzere alt dizinleri kopyalar. |
    |/MT[:N]             | N iş parçacığına sahip çoklu iş parçacıklı kopyalama işlemleri oluşturur ve burada N, 1 ile 128 arasında bir tam sayıdır. <br>N için varsayılan değer 8 olarak belirlenmiştir.        |
    |/R: <N>             | Başarısız kopyalama işlemleri için yeniden deneme sayısını belirtir. N için varsayılan değer 1.000.000 (bir milyon yeniden deneme) olarak belirlenmiştir.        |
    |/W: <N>             | Yeniden deneme işlemleri arasındaki bekleme süresini saniye cinsinden belirtir. N için varsayılan değer 30 (30 saniyelik bekleme süresi) olarak belirlenmiştir.        |
    |/NFL                | Dosya adlarının günlüğü alınmayacağını belirtir.        |
    |/NDL                | Dizin adlarının günlüğü alınmayacağını belirtir.        |
    |/FFT                | FAT dosya sürelerini (iki saniyelik duyarlık) kullanır.        |
    |/Log:<Log File>     | Durum çıkışını günlük dosyasına yazar (var olan günlük dosyasının üzerine yazar).         |

    Her birinde birden fazla işin çalıştığı birden fazla disk kullanılabilir. 

6. İş devam ederken kopyalama durumunu kontrol edin. Aşağıdaki örnekte dosyaları Data Box Disk'e kopyalamak için kullanılan Robocopy komutunun çıkışı gösterilmektedir.

    ```
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
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
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
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    Performansı iyileştirmek için veri kopyalama sırasında aşağıdaki Robocopy parametrelerini kullanın.

    |    Platform    |    Çoğunlukla küçük dosyalar, 512 KB altı                           |    Çoğunlukla orta büyüklükteki dosyalar, 512 KB-1 MB arası                      |    Çoğunlukla büyük dosyalar, 1 MB üzeri                             |   
    |----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
    |    Data Box Disk        |    4 Robocopy oturumları * <br> Oturum başına 16 iş parçacığı    |    2 Robocopy oturumları * <br> Oturum başına 16 iş parçacığı    |    2 Robocopy oturumları * <br> Oturum başına 16 iş parçacığı    |  |
    
    **Her Robocopy oturum 7000 dizinleri ve dosyaları 150 milyon en fazla olabilir.*
    
    >[!NOTE]
    > Yukarıdaki önerilen parametreleri inhouse sınama aşamasında kullanılan ortam temel alır.
    
    Robocopy komutu hakkında daha fazla bilgi için bkz. [Robocopy ve birkaç örnek](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

6. Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için günlük dosyalarını indirin. Günlük dosyaları robocopy komutunu belirtildiği yer alır.
 
> [!IMPORTANT]
> - Verilerin uygun dosya biçimine karşılık gelen klasörlere kopyalandığından emin olmak sizin sorumluluğunuzdur. Örneğin blok blobu verilerinin blok blobu klasörlerine kopyalanması gerekir. Veri biçimi uygun klasörle (depolama türü) eşleşmiyorsa veriler Azure'a yüklenemez.
> -  Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box Disk sınırları](data-box-disk-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun.
> - Data Box Disk tarafından yüklenen verilerin Data Box Disk haricinde başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.

### <a name="split-and-copy-data-to-disks"></a>Verileri bölme ve disklere kopyalama

Birden fazla disk kullanıyorsanız ve bölünerek tüm disklere kopyalanması gereken büyük bir veri kümesine sahipseniz bu isteğe bağlı yordamı kullanabilirsiniz. Data Box Split Copy aracı Windows bilgisayarlarda veri bölme ve kopyalama işlemlerini gerçekleştirmenize yardımcı olur.

>[!IMPORTANT]
> Veri kutusu bölünmüş kopyalama aracı verilerinizi de doğrular. Verileri kopyalamak için veri kutusu bölünmüş kopyalama aracı kullanmanız durumunda atlayabilirsiniz [doğrulama adımını](#validate-data).

1. Data Box Split Copy aracını Windows bilgisayarınıza indirip yerel bir klasöre ayıkladığınızdan emin olun. Bu araç Windows için Data Box Disk araç takımıyla birlikte indirilmiştir.
2. Dosya Gezgini'ni açın. Veri kaynağı sürücüsünü ve Data Box Disk'e atanmış olan sürücü harflerini not edin. 

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-1.png)
 
3. Kopyalanacak veri kaynağını belirleyin. Örneğin, bu durumda:
    - Aşağıdaki blok blobu tanımlanmıştır.

         ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-2.png)    

    - Aşağıdaki sayfa blobu tanımlanmıştır.

         ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-3.png)
 
4. Yazılımı ayıkladığınız klasöre gidin. Bulun `SampleConfig.json` klasördeki bir dosya. Bu değiştirip kaydedebileceğiniz bir salt okunur dosyadır.

   ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-4.png)
 
5. Değiştirme `SampleConfig.json` dosya.
 
    - İş adı girin. Bunu yaptığınızda Data Box Disk'te bir klasör oluşturulur ve aynı ad Azure depolama hesabında bu disklerle ilişkilendirilmiş olan kapsayıcı için de kullanılır. İş adının Azure kapsayıcı adlandırma kurallarına uygun olması gerekir. 
    - Yol biçiminde Not yaparak kaynak yolunu sağlamanız `SampleConfigFile.json`. 
    - Hedef disklere karşılık gelen sürücü harflerini girin. Veriler kaynak yoldan alınarak birden fazla diske kopyalanır.
    - Günlük dosyaları için bir yol belirtin. Varsayılan olarak, bunu geçerli dizine gönderilen burada `.exe` bulunur.

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-5.png)

6. Dosya biçimi doğrulamak için Git `JSONlint`. Dosyayı Farklı Kaydet `ConfigFile.json`. 

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-6.png)
 
7. Bir komut istemi penceresi açın. 

8. Çalıştırma `DataBoxDiskSplitCopy.exe`. Type

    `DataBoxDiskSplitCopy.exe PrepImport /config:<Your-config-file-name.json>`

     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-7.png)
 
9. Betiği sürdürmek için Enter tuşuna basın.

    ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-8.png)
  
10. Veri kümesi bölünüp kopyalandıktan sonra kopyalama oturumuna ait Split Copy aracı özeti görüntülenir. Örnek çıktı aşağıda gösterilmiştir.

    ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-9.png)
 
11. Verilerin hedef disklere bölündüğünü doğrulayın. 
 
    ![Veri kopyalama bölme](media/data-box-disk-deploy-copy-data/split-copy-10.png)
    ![bölünmüş veri kopyalama](media/data-box-disk-deploy-copy-data/split-copy-11.png)
     
    İçeriğini incelerseniz `n:` daha fazla sürücü, iki alt klasör için blok blobu ve sayfa blobu biçim verilerine karşılık gelen oluşturulduğunu görürsünüz.
    
     ![Verileri bölme ve kopyalama](media/data-box-disk-deploy-copy-data/split-copy-12.png)

12. Kopyalama oturumu başarısız olursa kurtarıp sürdürmek için şu komutu kullanın:

    `DataBoxDiskSplitCopy.exe PrepImport /config:<configFile.json> /ResumeSession`

Veri kopyalama işlemi tamamlandıktan sonra verilerinizi doğrulamak için geçebilirsiniz. Bölünmüş kopyalama aracı kullandıysanız, (bölme de aracı doğrulama kopya) doğrulamasını atlayın ve sonraki öğreticiye ilerleyin.


## <a name="validate-data"></a>Verileri doğrulama

Verileri kopyalamak için bölünmüş kopyalama aracı kullanmadıysanız, verilerinizi doğrulamak gerekir. Verileri doğrulamak için aşağıdaki adımları uygulayın.

1. Sağlama toplamı doğrulaması için sürücünüzün *DataBoxDiskImport* klasöründe `DataBoxDiskValidation.cmd` komutunu çalıştırın.
    
    ![Data Box Diski doğrulama aracı çıktısı](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Uygun olanı seçin. **Her zaman 2. seçeneği işaretleyerek dosyaları doğrulamanızı ve sağlama toplamları oluşturmanızı öneririz**. Bu adım verilerinizin boyutuna bağlı olarak uzun sürebilir. Betik tamamlandığında komut penceresinden çıkın. Doğrulama ve sağlama toplamı alma sırasında herhangi bir hata olursa size bildirilir ve hata günlüklerine bir bağlantı sunulur.

    ![Sağlama toplamı çıktısı](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - İki çalıştırma arasında aracı sıfırlayın.
    > - Seçenek 1 küçük dosyaları içeren, büyük bir veri kümesiyle ilgili kullanırsanız (~ KB'leri). Bu seçenek, yalnızca sağlama toplamı oluşturulması çok uzun zaman alabilir ve performansının çok yavaş olabilir dosyaları doğrular.

3. Birden çok disk kullanıyorsanız, komutu her disk için çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Veri bütünlüğünü doğrulama

Data Box Disk'i iade etme ve Azure'a yüklenen verileri doğrulama adımları hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a geri gönderme](./data-box-disk-deploy-picked-up.md)