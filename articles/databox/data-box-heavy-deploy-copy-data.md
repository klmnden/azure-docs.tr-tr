---
title: Azure veri kutusu ağır SMB aracılığıyla veri kopyalamak için öğretici | Microsoft Docs
description: İçin Azure veri kutusu ağır SMB üzerinden veri kopyalama hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 05/28/2019
ms.author: alkohli
ms.openlocfilehash: 8ee96f2e06071d60eb97596687387fd80ba14cc3
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496270"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-smb-preview"></a>Öğretici: Azure veri kutusu ağır SMB (Önizleme) üzerinden verileri kopyalayın

Bu öğreticide bağlanın ve yerel web UI aracılığıyla ana bilgisayardan veri kopyalama işlemini açıklamaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Veri kutusu ağır için Bağlan
> * Veri kutusu ağır için veri kopyalama


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. Tamamladınız [Öğreticisi: Azure veri kutusu ağır kümesi](data-box-deploy-set-up.md).
2. Aldığınız, veri kutusu yoğun ve sipariş durumu Portalı'nda **teslim edildi**.
3. Üzerinde veri kutusu ağır kopyalamak istediğiniz verileri içeren bir konak bilgisayar var. Ana bilgisayarınız:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. Hızlı kopya hızları için iki 40 GbE bağlantılar (düğüm başına bir adet) paralel olarak yararlanılabilir. 40 GbE bağlantı kullanılabilir değilse, en az iki 10 GbE bağlantılar (düğüm başına bir tane) sahip olmasını öneririz.

## <a name="connect-to-data-box-heavy-shares"></a>Veri kutusu ağır paylaşımlara

Seçilen depolama hesabına bağlı olarak, veri kutusu ağır kadar oluşturur:
- İlişkili her depolama hesabına GPv1 ve GPv2 için üç paylaşım.
- Premium depolama için bir paylaşım.
- Blob depolama hesabı için bir paylaşım.

Bu paylaşımlar cihazın her iki düğümde oluşturulur.

Blok blob ve sayfa blob paylaşımlar altında':
- Birinci düzey varlıkları kapsayıcılardır.
- İkinci düzey, BLOB'ları varlıklardır.

Azure dosyaları paylaşımlarını altında:
- Birinci düzey, paylaşımları varlıklardır.
- İkinci düzey varlıkları dosyalarıdır.

Aşağıdaki tabloda UNC yolunu paylaşımları burada veriler karşıya yüklendikten, veri kutusu yoğun ve Azure depolama yolu URL üzerinde gösterir. Son Azure depolama yolu URL'si UNC paylaşımı yolundan elde edilebilir.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure blok BLOB'ları | <li>Paylaşımları UNC yolu: `\\<DeviceIPAddress>\<StorageAccountName_BlockBlob>\<ContainerName>\files\a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure sayfa blobları  | <li>Paylaşımları UNC yolu: `\\<DeviceIPAddres>\<StorageAccountName_PageBlob>\<ContainerName>\files\a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Dosyaları       |<li>Paylaşımları UNC yolu: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>\files\a.txt`</li><li>Azure depolama URL'si: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |      

Bir Windows veya Linux istemcisi bağlanma adımları farklıdır.

> [!NOTE]
> Düğümlere paralel cihazın bağlanmak için aynı adımları izleyin.

### <a name="connect-on-a-windows-system"></a>Bir Windows sistemine bağlanın

Bir Windows Server ana bilgisayar kullanıyorsanız, veri kutusu ağır için bağlanmak için aşağıdaki adımları izleyin.

1. İlk adım kimlik doğrulamasından geçmek ve oturum başlatmaktır. **Bağlan ve kopyala**'ya gidin. Depolama hesabınızla ilişkilendirilmiş paylaşımların erişim kimlik bilgilerini almak için **Kimlik bilgilerini al**'a tıklayın.

    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-heavy-deploy-copy-data/get-share-credentials-1.png)

2. Paylaşıma erişme ve veri kopyalama iletişim kutusunda paylaşıma karşılık gelen **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. **Tamam**'ı tıklatın.
    
    ![Paylaşım kimlik bilgilerini alma 1](media/data-box-heavy-deploy-copy-data/get-share-credentials-2.png)

3. Depolama hesabınızla ilişkili paylaşımlara erişmek için (*databoxe2etest* aşağıdaki örnekte), ana bilgisayardan bir komut penceresi açın. Komut istemine şunları yazın:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Veri biçimine bağlı olarak paylaşım yolları şu şekilde olacaktır:
    - Azure blok blobu- `\\10.100.10.100\databoxe2etest_BlockBlob`
    - Azure sayfa blobu- `\\10.100.10.100\databoxe2etest_PageBlob`
    - Azure dosyaları- `\\10.100.10.100\databoxe2etest_AzFile`
    
4. İstendiğinde paylaşımın parolasını girin. Aşağıdaki örnekte yukarıdaki komutla paylaşıma bağlanma adımları gösterilmektedir.

    ```
    C:\Users\Databoxuser>net use \\10.100.10.100\databoxe2etest_BlockBlob /u:databoxe2etest
    Enter the password for 'databoxe2etest' to connect to '10.100.10.100':
    The command completed successfully.
    ```

4. Windows + R tuşlarına basın. **Çalıştır** penceresinde `\\<device IP address>` değerini belirtin. Tıklayın **Tamam** dosya Gezgini'ni açın.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-heavy-deploy-copy-data/connect-shares-file-explorer-1.png)

    Artık klasör paylaşımları görmeniz gerekir.
    
    ![Paylaşıma Dosya Gezgini ile bağlanma 2](media/data-box-heavy-deploy-copy-data/connect-shares-file-explorer-2.png)

    **Her zaman kopyalamayı düşündüğünüz dosyalar için paylaşımda bir klasör oluşturun ve ardından dosyaları bu klasöre kopyalayın**. Blok blobu altında klasör oluşturulur ve sayfa blob paylaşımları veriler BLOB olarak karşıya bir kapsayıcıyı temsil eder. Dosyaları doğrudan kopyalanamıyor *kök* depolama hesabında klasör.
    
### <a name="connect-on-a-linux-system"></a>Bir Linux sisteminde bağlanma

Bir Linux istemcisi kullanıyorsanız, SMB paylaşımını bağlaması için aşağıdaki komutu kullanın.

```
sudo mount -t nfs -o vers=2.1 10.126.76.172:/databoxe2etest_BlockBlob /home/databoxubuntuhost/databox
```

`vers` Linux konağınız destekleyen SMB sürümü parametredir. Yukarıdaki komutta uygun sürümünü takın.

Veri kutusu ağır destekleyen SMB sürümleri için bkz: [Linux istemcileri için desteklenen dosya sistemleri](data-box-heavy-system-requirements.md#supported-file-systems-for-linux-clients).

## <a name="copy-data-to-data-box-heavy"></a>Veri kutusu ağır için veri kopyalama

Veri kutusu ağır paylaşımlarına bağlandıktan sonra sonraki adıma veri kopyalamaktır.

### <a name="copy-considerations"></a>Konuları kopyalama

Veri kopyalama başlamadan önce aşağıdaki konuları gözden geçirin:

- Uygun veri biçimine karşılık gelen paylaşımlarına veri kopyaladığınızdan emin olun. Örneğin blok blobu verilerinin blok blobu paylaşımına kopyalanması gerekir. VHD'ler sayfa blob'olarak kopyalayın.

    Uygun paylaşım türü veri biçimi eşleşmiyor, daha sonraki bir adımda, Azure için verileri karşıya yükleme başarısız olur.
-  Veri kopyalama sırasında veri boyutu için açıklanan boyutu sınırları uyduğundan emin olun [Azure depolama ve veri kutusu ağır sınırları](data-box-heavy-limits.md).
- Ardından veri kutusu ağır tarafından karşıya yükleniyor, veri, veri kutusu ağır dışında başka uygulamalar tarafından eşzamanlı olarak yüklenirse, bu karşıya yükleme işi hataları ve veri bozulmasına neden olabilir.
- Olmasını öneririz:
    - Aynı anda hem SMB hem de NFS kullanmayın.
    - Azure'da aynı son hedefine aynı verileri kopyalayın.
     
  Bu gibi durumlarda, nihai sonucu belirlenemiyor.
- Her zaman bir paylaşım kapsamında kopyalayın ve ardından dosyaları klasöre kopyalayın istediğiniz dosyalar için bir klasör oluşturun. Blok blobu altında klasör oluşturulur ve sayfa blob paylaşımları, verilerin bloblar halinde karşıya bir kapsayıcı temsil eder. Dosyaları doğrudan kopyalanamıyor *kök* depolama hesabında klasör.

SMB paylaşımı bağlandıktan sonra veri kopyalama başlar.

1. Verilerinizi kopyalamak için Robocopy gibi SMB uyumlu herhangi bir dosya kopyalama aracını kullanabilirsiniz. Robocopy ile birden fazla kopyalama işlemini başlatabilirsiniz. Aşağıdaki komutu kullanın:
    
    ```
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile>
    ```
    Öznitelikler aşağıdaki tabloda açıklanmıştır.
    
    |Öznitelik  |Açıklama  |
    |---------|---------|
    |/e      |Boş dizinler dahil olmak üzere alt dizinleri kopyalar.         |
    |/r:     |Başarısız kopyalama işlemleri için yeniden deneme sayısını belirtir.         |
    |/w:     |Yeniden deneme işlemleri arasındaki bekleme süresini saniye cinsinden belirtir.         |
    |/is     |Aynı dosyaları dahil eder.         |
    |/nfl    |Dosya adları oturumunuz açık olduğunu belirtir.         |
    |/ndl    |Dizin adları oturumunuz açık olduğunu belirtir.        |
    |/np     |Kopyalama işleminin ilerleme durumunun (kopyalanan dosya veya dizin sayısının) görüntülenmeyeceğini belirtir. İlerleme durumunun gösterilmesi performansı önemli ölçüde düşürür.         |
    |/MT     | Çoklu iş parçacığı kullanılır, 32 veya 64 iş parçacığı önerilir. Bu seçenek şifrelenmiş dosyalarla kullanılmaz. Şifrelenmiş ve şifrelenmemiş dosyaları ayırmanız gerekebilir. Ancak tek iş parçacığına sahip kopyalama işlemi performansı önemli ölçüde düşürür.           |
    |/fft    | Herhangi bir dosya sistemi için zaman damgası ayrıntı düzeyini azaltmak için kullanın.        |
    |/b      | Dosyaları Yedekleme modunda kopyalar.        |
    |/z      | Dosyaları Yeniden başlatma modunda kopyalar, kararsız ortamlarda kullanmanız önerilir. Bu işlem ek günlük kaydı nedeniyle aktarım hızını düşürür.      |
    | /zb    | Yeniden başlatma modunu kullanır. Erişim reddedilirse bu seçenek Yedekleme modunu kullanır. Bu işlem denetim noktası oluşturma nedeniyle aktarım hızını düşürür.         |
    |/efsraw | Şifrelenmiş dosyaların tümünü EFS ham modunda kopyalar. Yalnızca şifrelenmiş dosyalarda kullanın.         |
    |Günlük +:\<günlük dosyası >| Çıkışı var olan günlük dosyasına ekler.|
    
 
    Aşağıdaki örnek veri kutusu ağır için dosyaları kopyalamak için robocopy komutunu çıkışını gösterir.

    ```   
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.100.10.100\devicemanagertest1_AzFile\templates /MT:24
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
        Started : Thursday, April 4, 2019 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
        Dest : \\10.100.10.100\devicemanagertest1_AzFile\templates\
        Files : *.*
        Options : *.* /DCOPY:DA /COPY:DAT /MT:24 /R:5 /W:60
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
    ```       

2. Performansı iyileştirmek için veri kopyalama sırasında aşağıdaki Robocopy parametrelerini kullanın. (En iyi örneği senaryoları numaralarını temsil eder.)

    | Platform    | Çoğunlukla küçük dosyalar, 512 KB altı    | Orta dosyaları çoğunlukla 512 KB - 1 MB  | Çoğunlukla büyük dosyalar, 1 MB üzeri                             |
    |-------------|--------------------------------|----------------------------|----------------------------|
    | Data Box Heavy | 6 Robocopy oturumu <br> Oturum başına 24 iş parçacığı | 6 Robocopy oturumu <br> Oturum başına 16 iş parçacığı | 6 Robocopy oturumu <br> Oturum başına 16 iş parçacığı |


    Robocopy komutu hakkında daha fazla bilgi için bkz. [Robocopy ve birkaç örnek](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx).

3. Kopyalanan dosyaları görüntülemek ve doğrulamak için hedef klasörü açın.

    ![Kopyalanan dosyalar görüntüleyin](media/data-box-heavy-deploy-copy-data/view-copied-files-1.png)


4. Veriler kopyalanırken:

    - Bu Azure nesne ve depolama limitleri yanı sıra Azure dosya ve kapsayıcı adlandırma kurallarını karşıladığından emin olmak dosya adları, boyutları ve biçimini doğrulandı.
    - Veri bütünlüğünü sağlamak için sağlama toplamı da hesaplanan satır içidir.

    Kopyalama işlemi sırasında hatayla karşılaşırsanız sorun giderme için hata dosyalarını indirin. Hata dosyalarını indirmek için ok simgesini seçin.

    ![Hata dosyalarını indirme](media/data-box-heavy-deploy-copy-data/download-error-files.png)

    Daha fazla bilgi için [veri kutusu yoğun veri kopyalama sırasında hata günlüklerini görüntülemek](data-box-logs.md#view-error-log-during-data-copy). Veri kopyalama sırasında hatalar ayrıntılı bir listesi için bkz. [veri kutusu ağır sorun giderme sorunları](data-box-troubleshoot.md).

5. Hata dosyasını Not Defteri'nde açın. Aşağıdaki hata dosyasını verileri doğru şekilde düşmeyen gösterir.

    ![Dosya açma hatası](media/data-box-heavy-deploy-copy-data/open-error-file.png)
    
    Bir sayfa blob'u için veri 512 bayt hizalı olması gerekir. Bu verileri kaldırıldıktan sonra aşağıdaki ekran görüntüsünde gösterildiği gibi hata çözümler.

    ![Hata çözümlendi](media/data-box-heavy-deploy-copy-data/error-resolved.png)

6. Kopyalama tamamlandıktan sonra Git **panoyu görüntüle** sayfası. Kullanılan alan ve boş alan Cihazınızda doğrulayın.
    
    ![Panoda boş ve kullanılan alanı doğrulama](media/data-box-heavy-deploy-copy-data/verify-used-space-dashboard.png)

İkinci düğümü cihazın açın veri kopyalama için yukarıdaki adımları yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri kutusu ağır konuları hakkında gibi öğrendiniz:

> [!div class="checklist"]
> * Veri kutusu ağır için Bağlan
> * Veri kutusu ağır için veri kopyalama


Veri kutusu ağır Microsoft'a göndermeye hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Microsoft, Azure veri kutusu ağır gönderin](./data-box-heavy-deploy-picked-up.md)

