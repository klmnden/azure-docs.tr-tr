---
title: Azure içeri/dışarı aktarma kullanarak Azure dosyalara veri aktarmak | Microsoft Docs
description: Azure portalında Azure dosyalara veri aktarmak için içeri aktarma işi oluşturmayı öğrenin.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 4349b471f960e7844511c473bffcd2177a34e055
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661023"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Azure dosyaları için verileri almak için Azure içeri/dışarı aktarma hizmeti kullanma

Bu makalede, büyük miktarlarda verinin Azure dosyalarıyla güvenli bir şekilde içeri aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma hakkında adım adım yönergeler sağlar. Veri almak için hizmet bir Azure veri merkezi verilerinizi içeren desteklenen disk sürücülerini dağıtmayı gerektirir.  

İçeri/dışarı aktarma hizmeti destekler, Azure depolama alanına Azure dosyaları yalnızca içeri aktarın. Azure dosyaları dışarı aktarma desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Azure dosyaları içine veri aktarmak için bir alma işi oluşturmadan önce dikkatle gözden geçirin ve aşağıdaki önkoşul listesini tamamlayın. Yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti ile kullanmak için etkin bir Azure aboneliğinizin olması.
- En az bir Azure depolama hesabına sahip. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](storage-create-storage-account.md#create-a-storage-account).
- Diskleri yetecek sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks). 
- Çalıştıran bir Windows sistemine sahip bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems).
- [Sürüm 2 WAImportExport karşıdan](https://www.microsoft.com/download/details.aspx?id=55280) Windows sisteminde. Varsayılan klasöre sıkıştırmasını `waimportexport`. Örneğin, `C:\WaImportExport`.


## <a name="step-1-prepare-the-drives"></a>1. adım: sürücüleri hazırlama

Bu adım, bir günlük dosyası oluşturur. Günlük dosyası sürücü seri numarası, şifreleme anahtarı ve depolama hesabı ayrıntıları gibi temel bilgileri depolar.

Sürücüleri hazırlamak için aşağıdaki adımları gerçekleştirin.

1. Bizim disk sürücüleri Windows sistemi SATA bağlayıcıları üzerinden bağlanır.
2. Tek bir NTFS birimi her sürücüde oluşturun. Birime bir sürücü harfini atayın. Bağlama kullanmayın.
3. Değiştirme *dataset.csv* aracı bulunduğu kök klasörü içinde dosya. Bir dosya veya klasör ya da her ikisini de almak isteyip istemediğinizi bağlı olarak, giriş eklemek *dataset.csv* aşağıdaki örneklere benzer dosya.  

    - **Bir dosyayı içe aktarmak için**: Aşağıdaki örnekte, C: sürücüsüne kopyalamak için verileri bulunur. Dosyanızı *MyFile1.txt* kök dizinine kopyalanır *MyAzureFileshare1*. Varsa *MyAzureFileshare1* yok, Azure depolama hesabında oluşturulur. Klasör yapısı korunur.

        ```
            BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
        ```
    - **Bir klasör almak için**: tüm dosyalar ve klasörler altında *MyFolder2* faydalanılması için kopyalanan yinelemeli. Klasör yapısı korunur.

        ```
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
        ```
    Birden çok giriş klasörleri veya içe aktarılan dosyaları karşılık gelen aynı dosyada yapılabilir. 

        ```
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            "F:\MyFolder3\MyFile3.txt","MyAzureFileshare2/",file,rename,"None",None 
            
        ```
    Daha fazla bilgi edinmek [veri kümesi CSV dosyası hazırlama](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Değiştirme *driveset.csv* aracı bulunduğu kök klasörü içinde dosya. Giriş eklemek *driveset.csv* aşağıdaki örneklere benzer dosya. Böylece aracı doğru hazırlanması için disk listesi seçebilirsiniz driveset dosya diskleri ve karşılık gelen sürücü harflerini listesine sahiptir.

    Bu örnek iki diskleri bağlı ve temel NTFS birimleri G:\ ve H:\ oluşturulduğunu varsayar. H:\is G: zaten şifrelendiyse ancak şifrelenmez. Aracı biçimlendirir ve H:\ yalnızca barındıran disk şifreler (ve değil G:\).

    - **Şifrelenmemiş bir disk için**: belirtin *şifrele* disk BitLocker şifrelemesini etkinleştirmek için.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        H,Format,SilentMode,Encrypt,
        ```
    
    - **Zaten şifrelenmiş bir disk için**: belirtin *AlreadyEncrypted* ve BitLocker anahtarını belirtin.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
        ```

    Birden çok giriş için birden çok sürücü karşılık gelen aynı dosyada yapılabilir. Daha fazla bilgi edinmek [driveset CSV dosyasını hazırlama](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5.  Kullanım `PrepImport` kopyalayın ve disk sürücüsüne verileri hazırlamak için seçeneği. Dizinler ve/veya yeni bir kopya oturum dosyaları kopyalamak ilk kopyalama oturumu için aşağıdaki komutu çalıştırın:

        ```
        .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
        ```

    İçeri aktarma örneği aşağıda verilmiştir.
  
        ```
        .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
        ```
 
6. İle sağlanan bir günlük dosyası adı ile `/j:` parametresi, her komut satırını Çalıştır için oluşturulur. Hazırlık her bir sürücü içeri aktarma işi oluşturduğunuzda, karşıya yüklenmelidir. bir günlük dosyası vardır. Günlük dosyaları işlenmez sürücüleri.

    > [!IMPORTANT]
    > - Disk sürücülerini veya günlük dosyası verileri disk hazırlığı tamamladıktan sonra değişiklik yapmayın.

Ek örnekler için Git [günlük dosyaları için örnek](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>2. adım: bir içeri aktarma işi oluşturma 

Azure portalında bir alma işi oluşturmak için aşağıdaki adımları gerçekleştirin.
1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 

    ![İçeri/dışarı aktarma gidin](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Tıklatın **oluşturma içeri/dışarı aktarma işi**.

    ![İçeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. İçinde **Temelleri**:

    - Seçin **Azure içe**.
    - İçe aktarma işi için açıklayıcı bir ad girin. Devam eden ve bunlar tamamlandığında çalışırken işlerinizi izlemek için bu adı kullanın.
        -  Bu ad yalnızca küçük harfler, sayılar, tire ve alt çizgi içerebilir.
        -  Adı bir harf ile başlamalı ve boşluk içeremez. 
    - Bir abonelik seçin.
    - Kaynak grubu seçin. 

        ![1. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. İçinde **iş ayrıntıları**:
    
    - Yukarıdaki sırasında oluşturulan günlük dosyalarını karşıya [1. adım: sürücüleri hazırlama](#step-1-prepare-the-drives). 
    - Verileri içeri aktarılan depolama hesabı seçin. 
    - Teslim konum, seçilen depolama hesabı bölgeye göre otomatik olarak doldurulur.
   
       ![2. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. İçinde **dönüş Sevkiyat bilgisi**:

    - Taşıyıcı aşağı açılan listeden seçin.
    - Bu taşıyıcı ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içe aktarma işi tamamlandıktan sonra geri için sürücüleri sevk etmek için bu hesabı kullanır. 
    - Bir tam ve geçerli kişi adı, telefon, e-posta, adres, şehir, ZIP, bölge ve ülke/bölge sağlar.

       ![Adım 3 - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. İçinde **Özet**:

    - Azure veri merkezinde Azure geri disklere sevkiyat adresi sevkiyat sağlar. İş adı ve tam adresini sevkiyat etikette açıklanan emin olun.
    - Tıklatın **Tamam** alma işi oluşturma işlemini tamamlamak için.

        ![4. adım - içe aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>3. adım: Azure veri merkezine sürücüleri sevk 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Adım 4: izleme bilgilerini iş güncelleştirin.

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="samples-for-journal-files"></a>Günlük dosyaları için örnek

İçin **daha fazla sürücü ekleme**, yeni bir driveset dosyası oluşturun ve komutu aşağıdaki gibi çalıştırın. 

İçinde belirtilenden farklı disk sürücüleri için sonraki kopyalama oturumlarının *InitialDriveset .csv* dosya, yeni driveset belirtin *.csv* dosya ve parametre değeri olarak sağlayın `AdditionalDriveSet`. Kullanım **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**. AdditionalDriveset CSV dosyasının biçimini InitialDriveSet biçimi olarak aynıdır.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

İçeri aktarma örneği aşağıda verilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Aynı driveset ek veri eklemek için ek dosyalar/dizine kopyalamak için Kopyala sonraki oturumlar için PrepImport komutunu kullanın.

Aynı sabit disk sürücülerine belirtilen sonraki kopyalama oturumları için *InitialDriveset.csv* dosya, belirtin **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**; Depolama hesabı anahtarı sağlamak için gerek yoktur.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

İçeri aktarma örneği aşağıda verilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


