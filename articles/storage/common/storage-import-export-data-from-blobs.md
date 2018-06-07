---
title: Verileri Azure Bloblarından dışarı aktarmak için Azure içeri/dışarı aktarma kullanarak | Microsoft Docs
description: Azure portalında Azure Bloblarından veri aktarmak için dışarı aktarma işleri oluşturmayı öğrenin.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: eb41708c7446b3139758678c9247ffbb11da8b40
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661005"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>Verileri Azure Blob depolama alanından dışarı aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma
Bu makalede, büyük miktarlarda verinin Azure Blob depolama alanından güvenli bir şekilde dışarı aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma hakkında adım adım yönergeler sağlar. Hizmet Azure veri merkezine boş sürücüleri sevk gerektirir. Hizmet veri sürücülerine depolama hesabınızdan verir ve sürücüleri geri gelir.

## <a name="prerequisites"></a>Önkoşullar

Verileri Azure Blob Storage dışına aktarmak için bir dışarı aktarma işinin oluşturmadan önce dikkatle gözden geçirin ve bu hizmet için Önkoşullar aşağıdaki listesini tamamlayın. Yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti için kullanılabilir etkin bir Azure aboneliğinizin olması.
- En az bir Azure depolama hesabına sahip. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](storage-create-storage-account.md#create-a-storage-account).
- Diskleri yetecek sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks).

## <a name="step-1-create-an-export-job"></a>1. adım: bir dışa aktarma işi oluşturma

Azure portalında bir dışarı aktarma işini oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 

    ![İçeri/dışarı aktarma işleri gidin](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Tıklatın **oluşturma içeri/dışarı aktarma işi**.

    ![İçeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. İçinde **Temelleri**:
    
    - Seçin **verme Azure'dan**. 
    - Dışa aktarma işi için açıklayıcı bir ad girin. İşleriniz ilerlemesini izlemek için seçtiğiniz ad kullanın. 
        - Ad yalnızca küçük harfler, sayılar, tire ve alt çizgi içerebilir.
        - Adı bir harf ile başlamalı ve boşluk içeremez. 
    - Bir abonelik seçin.
    - Bir kaynak grubu seçin veya girin.

        ![Temel Bilgiler](./media/storage-import-export-data-from-blobs/export-from-blob3.png) 
    
3. İçinde **iş ayrıntıları**:

    - Dışa aktarılacak veri bulunduğu depolama hesabı seçin. 
    - Teslim konum, seçilen depolama hesabı bölgeye göre otomatik olarak doldurulur. 
    - Depolama hesabınızdan boş sürücü veya sürücüler için vermek istediğiniz blob verileri belirtin. 
    - Tercih **tüm verme** blob depolama hesabınızdaki verileri.
    
         ![Tüm dışarı aktarma](./media/storage-import-export-data-from-blobs/export-from-blob4.png) 

    - Hangi kapsayıcılar ve bloblar dışarı aktarmak için belirtebilirsiniz.
        - **Dışarı aktarmak için bir blob belirtmek için**: kullanım **eşit** Seçici. Kapsayıcı adı ile başlayan blob göreli yolunu belirtin. Kullanım *$root* kök kapsayıcısı belirtin.
        - **Bir önek ile başlayan tüm BLOB'lar belirtmek için**: kullanım **ile başlar** Seçici. Eğik çizgiyle başlayan öneki belirtin '/'. Önek kapsayıcı adı, tam kapsayıcı adı veya blob adı öneki tarafından izlenen tüm kapsayıcı adı öneki olabilir. Bu ekran görüntüsünde gösterildiği gibi işleme sırasında hataları önlemek için geçerli biçim blob yollarında sağlamanız gerekir. Daha fazla bilgi için bkz: [geçerli blob yolu örnekleri](#examples-of-valid-blob-paths). 
   
           ![Seçili kapsayıcıları ve blobları dışarı aktarma](./media/storage-import-export-data-from-blobs/export-from-blob5.png) 

    - Blob listesi dosyasını dışarı aktarabilirsiniz.

        ![BLOB listesi dosyasından dışarı aktarma](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  
   
   > [!NOTE]
   > Dışa aktarılacak blobun veri kopyalama sırasında kullanımdaysa, Azure içeri/dışarı aktarma hizmeti BLOB bir anlık görüntüsünü alır ve anlık görüntü kopyalar.
 

4. İçinde **dönüş Sevkiyat bilgisi**:

    - Taşıyıcı açılır listeden seçin.
    - Bu taşıyıcı ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içe aktarma işi tamamlandıktan sonra geri için sürücüleri sevk etmek için bu hesabı kullanır. 
    - Bir tam ve geçerli kişi adı, telefon, e-posta, adres, şehir, ZIP, bölge ve ülke/bölge sağlar.
   
5. İçinde **Özet**:

    - İş ayrıntılarını gözden geçirin.
    - Azure diskleri sevkiyat için iş adını ve sağlanan Azure veri merkezi teslimat adresi unutmayın. 
    - Tıklatın **Tamam** dışa aktarma işi oluşturma işlemini tamamlamak için.

## <a name="step-2-ship-the-drives"></a>2. adım: sürücüleri sevk

Gereksinim duyduğunuz sürücü bilmiyorsanız, Git [sürücü sayısını kontrol](#check-the-number-of-drives). Sürücü biliyorsanız, sürücüleri sevk etmek için devam edin.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>3. adım: iş izleme bilgilerini güncelleştir

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="step-4-receive-the-disks"></a>4. adım: diskleri alma
İş tamamlandıktan panoyu raporlar için diskleri sevk ve sevkiyat izleme numarası portalında kullanılabilir.

1. Dışarı aktarılan verileri sürücülerle aldıktan sonra sürücüleri kilidini açmak için BitLocker anahtarları almanız gerekir. Azure portalında dışarı aktarma işini gidin. Tıklatın **içeri/dışarı aktarma** sekmesi. 
2. Ve sırayla dışa aktarma işi listeden seçin. Git **BitLocker anahtarları** ve anahtarları kopyalayın.
   
   ![BitLocker anahtarları dışa aktarma işi için görüntüleme](./media/storage-import-export-service/export-job-bitlocker-keys.png)

3. Diskleri kilidini açmak için BitLocker tuşlarını kullanın.

Dışa aktarma işlemi tamamlanmış olur. Şu anda işi silebilir veya 90 gün sonra otomatik olarak silinir.


## <a name="check-the-number-of-drives"></a>Onay sürücü sayısı

Bu *isteğe bağlı* adım yardımcı olur dışa aktarma işi için gerekli sürücü sayısını belirler. Çalıştıran bir Windows sisteminde bu adımı gerçekleştirmek bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems).

1. [Sürüm 1 WAImportExport karşıdan](https://www.microsoft.com/en-us/download/details.aspx?id=42659) Windows sisteminde. 
2. Varsayılan klasöre sıkıştırmasını `waimportexportv1`. Örneğin, `C:\WaImportExportV1`.
3. Yönetici ayrıcalıklarıyla bir PowerShell veya komut satırı penceresi açın. Dizin sıkıştırması açılmış klasör olarak değiştirmek için aşağıdaki komutu çalıştırın:
    
    `cd C:\WaImportExportV1`

4. Seçili BLOB'lar için gerekli disk sayısını denetlemek için aşağıdaki komutu çalıştırın:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    Parametreler aşağıdaki tabloda açıklanmıştır:
    
    |Komut satırı parametresi|Açıklama|  
    |--------------------------|-----------------|  
    |**/ LOGDIR:**|İsteğe bağlı. Günlük dosyası dizini. Ayrıntılı günlük dosyalarını bu dizine yazılır. Belirtilmezse, geçerli dizin günlük dizini olarak kullanılır.|  
    |**/sn:**|Gereklidir. Dışa aktarma işi için depolama hesabı adı.|  
    |**/SK:**|Yalnızca bir kapsayıcı SAS belirtilmemişse gereklidir. Dışa aktarma işi için depolama hesabı için hesap anahtarı.|  
    |**/csas:**|Yalnızca bir depolama hesabı anahtarı belirtilmemişse gereklidir. BLOB'ları dışarı aktarma işinin dışarı aktarılmasına izin listesi için kapsayıcı SAS.|  
    |**/ ExportBlobListFile:**|Gereklidir. XML yolu içeren blob yollar listesi dosya veya yol önekleri verilecek BLOB'ları için blob. Kullanılan dosya biçimi `BlobListBlobPath` öğesinde [Put işlemini](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) içeri/dışarı aktarma hizmeti REST API'si işlemi.|  
    |**/ DriveSize:**|Gereklidir. Bir dışarı aktarma işi için kullanılacak sürücüleri boyutunu *ör*, 500 GB, 1,5 TB.|  

    Bkz: bir [PreviewExport komut örneği](#example-of-previewexport-command).
 
5. Okuma/dışa aktarma işi için gönderilen sürücülerinde yazabilir denetleyin.

### <a name="example-of-previewexport-command"></a>PreviewExport komut örneği

Aşağıdaki örnekte gösterilmiştir `PreviewExport` komutu:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Dışarı aktarma blob listeyi dosyası blob adları içeren ve önekleri, aşağıda gösterildiği gibi blob olabilir:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Azure içeri/dışarı aktarma aracı verilecek tüm BLOB'ları listeler ve gerekli tüm ek yükü dikkate alarak belirtilen boyutu, sürücü halinde paketlemek nasıl hesaplar, sonra BLOB'ları ve sürücü kullanım bilgilerini tutmak için gerekli sürücüleri sayısını tahmin eder.  
  
Atlanmış bilgilendirme günlükleriyle çıktısı örneği şöyledir:  
  
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

## <a name="examples-of-valid-blob-paths"></a>Geçerli blob yolu örnekleri

Aşağıdaki tabloda, geçerli blob yolu örnekleri gösterilmektedir:
   
   | Seçici | BLOB yolu | Açıklama |
   | --- | --- | --- |
   | İle başlar |/ |Depolama hesabındaki tüm BLOB'lar verir |
   | İle başlar |/$root / |Kök kapsayıcıdaki tüm blob'lara verir |
   | İle başlar |/Book |Önek ile başlayan tüm kapsayıcıdaki tüm blob'lara aktarır **rehberi** |
   | İle başlar |/Music/ |Kapsayıcıdaki tüm blob'lara aktarır **müzik** |
   | İle başlar |/ Müzik/love |Kapsayıcıdaki tüm blob'lara aktarır **müzik** önek ile başlayan **memnuniyet** |
   | Eşit |$root/logo.bmp |Dışarı aktarma blob **logo.bmp** kök kapsayıcısında |
   | Eşit |Videos/Story.mp4 |Dışarı aktarma blob **story.mp4** kapsayıcısında **videolar** |

## <a name="next-steps"></a>Sonraki adımlar

* [İş ve sürücü durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


