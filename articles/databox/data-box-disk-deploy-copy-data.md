---
title: Verilerinizi Microsoft Azure Data Box Disk'e kopyalama | Microsoft Docs
description: Azure Data Box Disk'inizi nasıl veri kopyalayacağınızı öğrenmek için bu öğreticiyi kullanın
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
ms.date: 07/12/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: b0769ba70f495728df5c38b43bae4059b27de88b
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010829"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Öğretici: Verileri Azure Data Box Disk'e kopyalama ve doğrulama

Bu öğreticide ana bilgisayarınızdan veri kopyalama ve veri bütünlüğünü doğrulamak için sağlama toplamı oluşturma adımları anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Veri bütünlüğünü doğrulama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:
- [Öğretici: Azure Data Box Disk'inizi takma ve yapılandırma](data-box-disk-deploy-set-up.md) adımlarını tamamladınız.
- Disklerinizi paketten çıkarıp açtınız.
- Disklere veri kopyalamak için kullanacağınız bir ana bilgisayarınız var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-disk-system-requirements.md) çalıştırılmalıdır.
    - [Windows PowerShell 4 yüklü](https://www.microsoft.com/download/details.aspx?id=40855) olmalıdır.
    - [.NET Framework 4.5 yüklü](https://www.microsoft.com/download/details.aspx?id=30653) olmalıdır.


## <a name="copy-data-to-disks"></a>Disklere veri kopyalama

Bilgisayarınızla Data Box Disk arasında bağlantı kurmak ve veri kopyalamak için aşağıdaki adımları gerçekleştirin.

1. Kilidi açılan sürücünün içeriğini görüntüleyin. 

    ![Sürücü içeriğini görüntüleme](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Blok blobu olarak içeri aktarılması gereken verileri BlockBlob klasörüne kopyalayın. Benzer şekilde VHD/VHDX gibi verileri de PageBlob klasörüne kopyalayın. 

    BlockBlob ve PageBlob klasörlerinin altındaki her klasör için Azure depolama hesabında bir kapsayıcı oluşturulur. BlockBlob ve PageBlob klasörlerinin altındaki tüm dosyalar Azure Depolama hesabındaki varsayılan `$root` kapsayıcısına kopyalanır. `$root` kapsayıcısındaki tüm dosyalar her zaman blok blobu olarak yüklenir.

    Kök dizinde dosya ve klasörler varsa veri kopyalama işlemine başlamadan önce bunları farklı bir klasöre taşımanız gerekir.

    Kapsayıcı ve blob adlarıyla ilgili Azure adlandırma gereksinimlerine bakın.

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
    |<Source>            | Kaynak dizin yolunu belirtir.        |
    |<Destination>       | Hedef dizin yolunu belirtir.        |
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
 
    
7. Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın. Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için günlük dosyalarını indirin. Günlük dosyaları Robobopy komutunda belirtilen dizine kaydedilir.
 


> [!IMPORTANT]
> - Verilerin uygun dosya biçimine karşılık gelen klasörlere kopyalandığından emin olmak sizin sorumluluğunuzdur. Örneğin blok blobu verilerinin blok blobu klasörlerine kopyalanması gerekir. Veri biçimi uygun klasörle (depolama türü) eşleşmiyorsa veriler Azure'a yüklenemez.
> -  Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box Disk sınırları](data-box-disk-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun. 
> - Data Box Disk tarafından yüklenen verilerin Data Box Disk haricinde başka bir uygulama tarafından da yüklenmesi durumunda yükleme işinde hata oluşabilir ve veri bozulması yaşanabilir.

## <a name="verify-data-integrity"></a>Veri bütünlüğünü doğrulama

Veri bütünlüğünü doğrulamak için aşağıdaki adımları gerçekleştirin.

1. Sağlama toplamı doğrulaması için `AzureExpressDiskService.ps1` dosyasını çalıştırın. Dosya Gezgini'nde sürücünün *AzureImportExport* klasörüne gidin. Sağ tıklayın ve **PowerShell ile Çalıştır**'ı seçin. 

    ![Sağlama toplamını çalıştırma](media/data-box-disk-deploy-copy-data/data-box-disk-checksum.png)

2. Bu adım verilerinizin boyutuna bağlı olarak uzun sürebilir. Betik çalıştırıldığında veri bütünlüğü denetimi işleminin özetinin yanı sıra işlemin tamamlanması için kalan süre görüntülenir. Komut penceresini kapatmak için **Enter** tuşuna basabilirsiniz.

    ![Sağlama toplamı çıktısı](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

3. Birden çok disk kullanıyorsanız, komutu her disk için çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Verileri Data Box Disk'e kopyalama
> * Veri bütünlüğünü doğrulama

Data Box Disk'i iade etme ve Azure'a yüklenen verileri doğrulama adımları hakkında bilgi edinmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a geri gönderme](./data-box-disk-deploy-picked-up.md)

