---
title: Verileri Azure Bloblarından dışarı aktarmak için Azure içeri/dışarı aktarma kullanarak | Microsoft Docs
description: Azure portalında Azure Bloblarından veri aktarmak için dışarı aktarma işleri oluşturmayı öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/08/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: e542ad59f6fd64b52aef9438ed0f646e9e36fc4a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209633"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Azure Blob depolama alanından verileri dışarı aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma
Bu makalede Azure içeri/dışarı aktarma hizmeti büyük miktarda veriyi Azure Blob depolama alanından güvenli bir şekilde dışarı aktarmak için nasıl kullanılacağını adım adım yönergeler sağlar. Hizmet, Azure veri merkezine boş sürücüleri gönderin gerektirir. Hizmet veri sürücüleri için depolama hesabınızdan verir ve ardından sürücüleri geri gelir.

## <a name="prerequisites"></a>Önkoşullar

Verileri Azure Blob Depolama dışına aktarmak için dışarı aktarma işi oluşturmadan önce dikkatle gözden geçirin ve aşağıdaki listede yer alan bu hizmet için önkoşulları tamamlayın. Şunları yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti için kullanılabilir bir etkin Azure Aboneliğim var.
- En az bir Azure depolama hesabına sahip. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [bir depolama hesabının nasıl oluşturulacağını](storage-quickstart-create-account.md).
- Diskleri yeterli sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks).
- Bir FedEx/DHL hesabınız vardır. FedEx/DHL dışındaki bir taşıyıcı kullanmak istiyorsanız, Azure veri kutusu işlemleri ekibi ile iletişime geçin `adbops@microsoft.com`. 
    - Hesabın geçerli olmalıdır, Bakiye olmalıdır ve iade sevkiyat özelliklerine sahip olmalı.
    - İzleme numarası için dışarı aktarma işi oluşturur.
    - Her iş ayrı izleme numarası olmalıdır. Birden çok iş aynı izleme numarası ile desteklenmez. 
    - Taşıyıcı hesap yoksa gidin:
        - [FedEX hesabı oluşturma](https://www.fedex.com/en-us/create-account.html), veya 
        - [DHL hesabı oluşturma](http://www.dhl-usa.com/en/express/shipping/open_account.html).

## <a name="step-1-create-an-export-job"></a>1\. adım: Dışarı aktarma işi oluşturma

Azure portalında bir dışarı aktarma işi oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 

    ![İçeri/dışarı aktarma işleri için Git](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Tıklayın **içeri/dışarı aktarma işi oluşturma**.

    ![İçeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. İçinde **Temelleri**:
    
    - Seçin **Azure'dan dışarı aktarma**. 
    - Dışarı aktarma işi için açıklayıcı bir ad girin. İşlerinizi ilerlemesini izlemek için seçtiğiniz adın kullanın. 
        - Ad yalnızca küçük harf, sayı, kısa çizgi ve alt çizgi içerebilir.
        - Ad bir harf ile başlamalı ve boşluk içeremez. 
    - Bir abonelik seçin.
    - Bir kaynak grubu seçin veya girin.

        ![Temel Bilgiler](./media/storage-import-export-data-from-blobs/export-from-blob3.png) 
    
3. İçinde **iş ayrıntıları**:

    - Aktarılacak verilerin bulunduğu depolama hesabını seçin. Bulunduğu yere yakın bir depolama hesabını kullanırsınız.
    - Dropoff konum seçili depolama hesabının bölgeye göre otomatik olarak doldurulur. 
    - Depolama hesabınızdan, boş sürücü veya sürücüler için vermek istediğiniz blob verileri belirtin. 
    - Tercih **tümünü dışarı aktar** blob depolama hesabındaki verileri.
    
         ![Tümünü Dışarı Aktar](./media/storage-import-export-data-from-blobs/export-from-blob4.png) 

    - Hangi kapsayıcıları ve BLOB'ları dışarı aktarmak için belirtebilirsiniz.
        - **Dışarı aktarmak için bir blob belirtmek için**: Kullanım **eşit** Seçici. Blob kapsayıcı adı ile başlayan, göreli yolunu belirtin. Kullanım *$root* kök kapsayıcısı belirtin.
        - **Bir ön Ekle başlayan tüm blobları belirtmek için**: Kullanım **ile başlar** Seçici. Bir eğik çizgi ile başlayan bir öneki belirtin '/'. Ön ek, kapsayıcı adı, tam kapsayıcı adı veya blob adı ön eke göre ve ardından tam kapsayıcı adı ön eki olabilir. Bu ekran görüntüsünde gösterildiği gibi işleme sırasında hataları önlemek için geçerli bir biçimde blob yollarının sağlamanız gerekir. Daha fazla bilgi için [geçerli blob yolu örnekleri](#examples-of-valid-blob-paths). 
   
           ![Seçili kapsayıcılar ve bloblar dışarı aktarma](./media/storage-import-export-data-from-blobs/export-from-blob5.png) 

    - Blob listesi dosyasından dışarı aktarabilirsiniz.

        ![BLOB listesi dosyasından Dışarı Aktar](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  
   
   > [!NOTE]
   > Dışa aktarılacak blob veri kopyalama sırasında kullanılıyorsa, Azure içeri/dışarı aktarma hizmeti blob anlık görüntüsünü alır ve anlık görüntüyü kopyalar.
 

4. İçinde **iade sevkiyat bilgilerini**:

    - Taşıyıcı açılır listeden seçin. Bir taşıyıcı FedEx/DHL dışında kullanmak istiyorsanız, açılır listeden var olan bir seçenek seçin. İlgili Azure veri kutusu işlemleri takım konumundaki `adbops@microsoft.com` ile kullanmayı planladığınız taşıyıcı ilgili bilgileri.
    - Bu operatör ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, dışarı aktarma işi tamamlandıktan sonra geri için sürücüleri göndermeye bu hesabı kullanır. 
    - Bir tam ve geçerli ilgili kişi adı, telefon, e-posta, posta adresi, şehir, posta, eyalet/il ve ülke/bölge belirtin.

        > [!TIP] 
        > Tek bir kullanıcı için bir e-posta adresi belirtmek yerine, Grup e-posta sağlayın. Bu, bir yönetici ayrılsa bile bildirimleri almak sağlar.
   
5. İçinde **özeti**:

    - İşin ayrıntılarını gözden geçirin.
    - Teslimat adresi diskleri azure'a aktarma için sağlanan Azure veri merkezi ve iş adını not edin. 

        > [!NOTE] 
        > Her zaman diskleri Azure Portalı'nda belirtilen veri merkezine gönderin. Diskler hatalı veri merkezine gönderilir, iş işlenmeyecek.

    - Tıklayın **Tamam** dışarı aktarma işi oluşturma işlemini tamamlamak için.

## <a name="step-2-ship-the-drives"></a>2\. adım: Sürücüleri gönderin

İstediğiniz sürücü sayısını bilmiyorsanız, Git [sürücü sayısını kontrol](#check-the-number-of-drives). Sürücü sayısı biliyorsanız, sürücüleri göndermeye devam edin.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>3\. adım: İş izleme bilgilerini güncelleştir

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="step-4-receive-the-disks"></a>4\. Adım: Diskleri
Pano işin tamamlandığından emin bildirdiğinde, diskleri size gönderilir ve sevk irsaliyesi için takip numarasını portalda kullanılabilir.

1. Dışarı aktarılan verileri sürücüleriyle aldıktan sonra sürücüleri kilidini açmak için BitLocker anahtarlarını edinmeniz gerekir. Azure portalında dışarı aktarma işi gidin. Tıklayın **içeri/dışarı aktarma** sekmesi. 
2. Seçin ve listeden, dışarı aktarma işi tıklayın. Git **BitLocker anahtarlarını** ve anahtarları kopyalayın.
   
   ![Dışarı aktarma işi için BitLocker anahtarlarını görüntüle](./media/storage-import-export-service/export-job-bitlocker-keys.png)

3. Diskleri kilidini açmak için BitLocker anahtarları kullanın.

Dışarı aktarma tamamlanmıştır. Şu anda 90 gün sonra otomatik olarak silindiğinde veya iş silebilirsiniz.


## <a name="check-the-number-of-drives"></a>Onay sürücü sayısı

Bu *isteğe bağlı* adım yardımcı dışarı aktarma işi için gerekli sürücü sayısını belirler. Çalıştıran bir Windows sisteminde bu adımı gerçekleştirmeden bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems).

1. [' % S'WAImportExport sürüm 1 indirme](https://aka.ms/waiev1) Windows sistem üzerinde. 
2. Varsayılan klasöre çıkartın `waimportexportv1`. Örneğin, `C:\WaImportExportV1`.
3. Yönetici ayrıcalıklarıyla bir PowerShell veya komut satırı penceresi açın. Sıkıştırması açılmış klasöre dizini değiştirmek için aşağıdaki komutu çalıştırın:
    
    `cd C:\WaImportExportV1`

4. Seçili bloblar için gereken disk sayısını denetlemek için aşağıdaki komutu çalıştırın:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Parametreleri aşağıdaki tabloda açıklanmıştır:
    
    |Komut satırı parametresi|Açıklama|  
    |--------------------------|-----------------|  
    |**/ LOGDIR:**|İsteğe bağlı. Günlük dizini. Ayrıntılı günlük dosyası bu dizine yazılır. Belirtilmezse, geçerli dizin günlük dizini kullanılır.|  
    |**/sn:**|Gereklidir. Dışarı aktarma işi için depolama hesabı adı.|  
    |**/SK:**|Yalnızca bir kapsayıcı SAS belirtilmemişse gereklidir. Dışarı aktarma işi için depolama hesabı için hesap anahtarı.|  
    |**/csas:**|Yalnızca bir depolama hesabı anahtarı belirtilmemişse gereklidir. Dışarı aktarma işi verilecek blobları listeleme kapsayıcısı SAS.|  
    |**/ ExportBlobListFile:**|Gereklidir. XML yolu içeren blob yollarının listesini dosya veya yol önekleri dışarı aktarılacak bloblar için blob. Kullanılan dosya biçimi `BlobListBlobPath` öğesinde [Put işlemini](/rest/api/storageimportexport/jobs) içeri/dışarı aktarma hizmeti REST API işlemi.|  
    |**/ DriveSize:**|Gereklidir. Bir dışarı aktarma işi için kullanılacak sürücüleri boyutunu *örn*, 500 GB, 1,5 TB.|  

    Bkz: bir [PreviewExport komut örneği](#example-of-previewexport-command).
 
5. / Dışarı aktarma işi için gönderilecek sürücülerine okuma onay.

### <a name="example-of-previewexport-command"></a>PreviewExport komut örneği

Aşağıdaki örnek, gösterir `PreviewExport` komutu:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Dışarı aktarma blob listesi dosyası blob adları içeren ve ön ekleri, burada gösterildiği gibi blob:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Azure içeri/dışarı aktarma aracı aktarılabilmesi için tüm blobları listeler ve gerekli tüm ek yükü dikkate alarak, belirtilen boyut sürücülere paketlenecek nasıl hesaplar ve ardından sürücü kullanım bilgilerini ve BLOB'ları beklemesi gereken sürücü sayısını tahmin eder.  
  
Atlanmış bilgilendirici günlükleri ile çıktının bir örneği aşağıda verilmiştir:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```

## <a name="examples-of-valid-blob-paths"></a>Geçerli bir blob yolu örnekleri

Aşağıdaki tabloda, geçerli bir blob yolu örnekleri gösterilmektedir:
   
   | Seçici | BLOB yolu | Açıklama |
   | --- | --- | --- |
   | Şununla başlar |/ |Depolama hesabındaki tüm bloblar dışarı aktarır |
   | Şununla başlar |/$root / |Kök kapsayıcıdaki tüm blob'lara dışarı aktarır |
   | Şununla başlar |/Book |Önek ile başlayan herhangi bir kapsayıcıdaki tüm blob'lara aktarır **rehberi** |
   | Şununla başlar |/Music/ |Kapsayıcıdaki tüm blob'lara aktarır **müzik** |
   | Şununla başlar |/ Müzik/love |Kapsayıcıdaki tüm blob'lara aktarır **müzik** önekiyle başlayan **sevdiğiniz** |
   | Eşittir |$root/logo.bmp |Dışarı aktarmalar blob **logo.bmp** içinde kök kapsayıcı |
   | Eşittir |Videos/Story.mp4 |Dışarı aktarmalar blob **story.mp4** kapsayıcısında **videoları** |

## <a name="next-steps"></a>Sonraki adımlar

* [İşimin ve Sürücümün durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


