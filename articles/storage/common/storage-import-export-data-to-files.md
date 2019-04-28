---
title: Azure dosyaları'na veri aktarmak için Azure içeri/dışarı aktarma kullanarak | Microsoft Docs
description: Azure dosyaları'na veri aktarmak için Azure portalında içeri aktarma işlerinde oluşturmayı öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/08/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 28026a429643c62434ddfd7591126169857a7371
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61479090"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Azure dosyaları'na veri almak için Azure içeri/dışarı aktarma hizmetini kullanma

Bu makalede Azure içeri/dışarı aktarma hizmeti büyük miktarda veriyi Azure dosyalarına güvenli bir şekilde içeri aktarmak için nasıl kullanılacağını adım adım yönergeler sağlar. Verileri içeri aktarmak için hizmet, desteklenen disk sürücüleri içeren bir Azure veri merkezi için veri gönderin gerektirir.  

İçeri/dışarı aktarma hizmetini destekler, yalnızca Azure dosyaları Azure Depolama'ya aktarın. Azure dosyaları dışarı aktarma desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

Azure dosyalarına veri aktarmak için içeri aktarma işine oluşturmadan önce dikkatle gözden geçirin ve aşağıdaki listede yer alan önkoşulları tamamlayın. Şunları yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti ile kullanmak için etkin bir Azure aboneliğine sahip.
- En az bir Azure depolama hesabına sahip. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [bir depolama hesabının nasıl oluşturulacağını](storage-quickstart-create-account.md).
- Diskleri yeterli sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks). 
- Çalıştıran bir Windows sistemine sahip bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems).
- [' % S'WAImportExport sürüm 2 indirme](https://aka.ms/waiev2) Windows sistem üzerinde. Varsayılan klasöre çıkartın `waimportexport`. Örneğin, `C:\WaImportExport`.
- Bir FedEx/DHL hesabınız vardır. FedEx/DHL dışındaki bir taşıyıcı kullanmak istiyorsanız, Azure veri kutusu işlemleri ekibi ile iletişime geçin `adbops@microsoft.com`.  
    - Hesabın geçerli olmalıdır, Bakiye olmalıdır ve iade sevkiyat özelliklerine sahip olmalı.
    - İzleme numarası için dışarı aktarma işi oluşturur.
    - Her iş ayrı izleme numarası olmalıdır. Birden çok iş aynı izleme numarası ile desteklenmez.
    - Taşıyıcı hesap yoksa gidin:
        - [FedEX hesabı oluşturma](https://www.fedex.com/en-us/create-account.html), veya 
        - [DHL hesabı oluşturma](http://www.dhl-usa.com/en/express/shipping/open_account.html).
 


## <a name="step-1-prepare-the-drives"></a>1. Adım: Sürücüleri hazırlama

Bu adım, bir günlük dosyası oluşturur. Günlük dosyası sürücü seri numarası, şifreleme anahtarını ve depolama hesabı ayrıntıları gibi temel bilgileri depolar.

Sürücüleri hazırlamak için aşağıdaki adımları gerçekleştirin.

1. Bizim disk sürücüleri Windows sistemi SATA bağlayıcılar üzerinden bağlanır.
2. Her sürücüde tek bir NTFS birimi oluşturun. Birime bir sürücü harfi atayabilirsiniz. Bağlama kullanmayın.
3. Değiştirme *dataset.csv* aracı bulunduğu kök klasöründeki dosya. Bir dosya veya klasör ya da her ikisini de almak isteyip istemediğinizi bağlı olarak, girişler ekleyin *dataset.csv* aşağıdaki örneklere benzer dosya.  

   - **Bir dosyayı içe aktarmak için**: Aşağıdaki örnekte, kopyalanacak verileri C: sürücüsünde yer alıyor. Dosyanızı *MyFile1.txt* kök dizinine kopyalanır *MyAzureFileshare1*. Varsa *MyAzureFileshare1* yok, Azure depolama hesabı oluşturulur. Klasör yapısı korunur.

       ```
           BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
       ```
   - **Bir klasörü içeri aktarmak için**: Tüm dosya ve klasörlerin altındaki *MyFolder2* yinelemeli dosya paylaşımına kopyalanır. Klasör yapısı korunur.

       ```
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
       ```
     Aynı dosyada içe aktarılan dosyaları veya klasörleri karşılık gelen birden çok girişi yapılabilir. 

       ```
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
                        
       ```
     Daha fazla bilgi edinin [veri kümesi CSV dosyası hazırlanıyor](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Değiştirme *driveset.csv* aracı bulunduğu kök klasöründeki dosya. Girdileri ekleme *driveset.csv* aşağıdaki örneklere benzer dosya. Aracı düzgün listesi disklerin hazırlanması seçebilir böylece driveset dosya diskleri ve karşılık gelen sürücü harflerini listesine sahiptir.

    Bu örnekte, iki disk eklenir ve temel NTFS birimleri G:\ ve H:\ oluşturduğunuz varsayılır. H:\is G: zaten şifrelendiyse ancak şifrelenmez. Aracı biçimlendirir ve yalnızca H:\ barındıran disk şifreler (ve değil G:\).

   - **Şifrelenmemiş bir disk için**: Belirtin *şifrele* diskte BitLocker şifrelemesini etkinleştirmek için.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       H,Format,SilentMode,Encrypt,
       ```
    
   - **Zaten şifrelenmiş bir disk için**: Belirtin *AlreadyEncrypted* ve BitLocker anahtarı sağlayın.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
       ```

     Birden çok giriş için birden çok sürücü karşılık gelen aynı dosyada yapılabilir. Daha fazla bilgi edinin [driveset CSV dosyası hazırlanıyor](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5. Kullanım `PrepImport` kopyalayın ve disk sürücüsüne verileri hazırlamak için seçeneği. Dizinleri ve/veya yeni bir kopya oturumu dosyaları kopyalamak ilk kopyalama oturumu için aşağıdaki komutu çalıştırın:

       ```
       .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
       ```

   İçeri aktarma örneği aşağıda gösterilmiştir.
  
       ```
       .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
       ```
 
6. İle sağlanan ada sahip bir günlük dosyası `/j:` parametresi, her komut satırı çalıştırma için oluşturulur. Hazırlama, her bir sürücü içeri aktarma işi oluşturma zaman karşıya yüklenmelidir bir günlük dosyası vardır. Günlük dosyaları işlenmez beraberinde getirir.

    > [!IMPORTANT]
    > - Disk sürücülerini veya günlük dosyası veri diski hazırlama tamamladıktan sonra değişiklik yapmayın.

Ek örnekler için Git [günlük dosyaları için örnekleri](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>2. Adım: İçeri aktarma işi oluşturma 

Azure portalında içeri aktarma işi oluşturmak için aşağıdaki adımları gerçekleştirin.
1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 

    ![İçeri/dışarı aktarma gidin](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Tıklayın **içeri/dışarı aktarma işi oluşturma**.

    ![İçeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. İçinde **Temelleri**:

    - Seçin **azure'a aktar**.
    - İçeri aktarma işi için açıklayıcı bir ad girin. Bu ad, devam eden ve tamamlandıktan sonra oldukları işlerinizi izlemek için kullanabilirsiniz.
        -  Bu ad yalnızca küçük harf, sayı, kısa çizgi ve alt çizgi içerebilir.
        -  Ad bir harf ile başlamalı ve boşluk içeremez. 
    - Bir abonelik seçin.
    - Kaynak grubunu seçin. 

        ![1. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. İçinde **iş ayrıntıları**:
    
    - Önceki sırasında oluşturulan günlük dosyalarını karşıya yükleme [1. adım: Sürücüleri hazırlamak](#step-1-prepare-the-drives). 
    - Verileri içeri aktarılan depolama hesabını seçin. 
    - Dropoff konum seçili depolama hesabının bölgeye göre otomatik olarak doldurulur.
   
       ![2. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. İçinde **iade sevkiyat bilgilerini**:

    - Taşıyıcı aşağı açılan listeden seçin. Bir taşıyıcı FedEx/DHL dışında kullanmak istiyorsanız, açılır listeden var olan bir seçenek seçin. İlgili Azure veri kutusu işlemleri takım konumundaki `adbops@microsoft.com` ile kullanmayı planladığınız taşıyıcı ilgili bilgileri.
    - Bu operatör ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içeri aktarma işi tamamlandıktan sonra geri için sürücüleri göndermeye bu hesabı kullanır. 
    - Bir tam ve geçerli ilgili kişi adı, telefon, e-posta, posta adresi, şehir, posta, eyalet/il ve ülke/bölge belirtin.

        > [!TIP] 
        > Tek bir kullanıcı için bir e-posta adresi belirtmek yerine, Grup e-posta sağlayın. Bu, bir yönetici ayrılsa bile bildirimleri almak sağlar.

       ![3. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. İçinde **özeti**:

    - Teslimat adresi diskleri azure'a geri sevkiyat için Azure veri merkezi sunar. İş adı ve tam adresini sevkiyat etiketi açıklanan emin olun.
    - Tıklayın **Tamam** içeri aktarma işi oluşturma işlemini tamamlamak için.

        ![4. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>3. Adım: Sürücüleri Azure veri merkezine gönderin 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>4. Adım: İş izleme bilgilerini güncelleştir

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>5. Adım: Azure'a verilerin yüklendiğini doğrulama

İş tamamlanana kadar izleyin. İş tamamlandıktan sonra verilerinizi Azure'a karşıya yüklendiğini doğrulayın. Yalnızca, karşıya yükleme başarılı olduğunu doğruladıktan sonra şirket içi verileri silin.

## <a name="samples-for-journal-files"></a>Günlük dosyaları için örnekleri

İçin **daha fazla sürücü ekleme**driveset yeni bir dosya oluşturun ve komut aşağıdaki gibi çalıştırın. 

Sonraki kopyalama oturumlarını içinde belirtilenden farklı disk sürücülerinde *InitialDriveset .csv* dosya, yeni driveset belirtin *.csv* dosya ve parametre değeri olarak sağlayın `AdditionalDriveSet`. Kullanım **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**. InitialDriveSet biçiminde AdditionalDriveset CSV dosya biçimi aynıdır.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

İçeri aktarma örneği aşağıda gösterilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Ek veriler için aynı driveset eklemek için ek dosyalar/dizin kopyalamak için kopyalama sonraki oturumlar için PrepImport komutu kullanın.

Aynı sabit disk sürücülerini belirtilen sonraki kopyalama oturumları için *InitialDriveset.csv* dosya, belirtin **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**; Depolama hesabı anahtarını sağlamaya gerek yoktur.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

İçeri aktarma örneği aşağıda gösterilmiştir.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [İşimin ve Sürücümün durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


