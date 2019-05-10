---
title: Azure Blobları için veri aktarmak için Azure içeri/dışarı aktarma kullanarak | Microsoft Docs
description: İçeri aktarma oluşturma ve dışarı aktarma işleri için ve Azure BLOB veri aktarmak için Azure portalında öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 04/08/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 82672136d6f9af50a3d91da2044f6e0ced4b44a6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409376"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri aktarmak için Azure içeri/dışarı aktarma hizmeti kullanma

Bu makalede Azure içeri/dışarı aktarma hizmeti büyük miktarda veriyi Azure Blob depolama alanına güvenli bir şekilde içeri aktarmak için nasıl kullanılacağını adım adım yönergeler sağlar. Verileri Azure BLOB olarak içeri aktarmak için hizmet, bir Azure veri merkezine verilerinizin bulunduğu disk sürücüleri şifrelenmiş gönderin gerektirir.  

## <a name="prerequisites"></a>Önkoşullar

Azure Blob depolama alanına veri aktarmak için içeri aktarma işine oluşturmadan önce dikkatle gözden geçirin ve aşağıdaki listede yer alan bu hizmet için önkoşulları tamamlayın. Şunları yapmanız gerekir:

- İçeri/dışarı aktarma hizmeti için kullanılabilir bir etkin Azure Aboneliğim var.
- Bir depolama kapsayıcısı ile en az bir Azure depolama hesabı var. Listesine bakın [desteklenen depolama hesapları ve depolama türleri için içeri/dışarı aktarma hizmeti](storage-import-export-requirements.md). 
    - Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz. [bir depolama hesabının nasıl oluşturulacağını](storage-quickstart-create-account.md). 
    - Depolama kapsayıcısı üzerinde daha fazla bilgi için Git [bir depolama kapsayıcısı oluşturma](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Diskleri yeterli sayıda [desteklenen türleri](storage-import-export-requirements.md#supported-disks). 
- Çalıştıran bir Windows sistemine sahip bir [desteklenen işletim sistemi sürümü](storage-import-export-requirements.md#supported-operating-systems). 
- BitLocker Windows sisteminde etkinleştirin. Bkz: [BitLocker'ı etkinleştirme](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [' % S'WAImportExport sürüm 1 indirme](https://aka.ms/waiev1) Windows sistem üzerinde. Varsayılan klasöre çıkartın `waimportexportv1`. Örneğin, `C:\WaImportExportV1`.
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

1.  Disk sürücülerine SATA bağlayıcılar üzerinden Windows sisteme bağlanın.
1.  Her sürücüde tek bir NTFS birimi oluşturun. Birime bir sürücü harfi atayabilirsiniz. Bağlama kullanmayın.
2.  NTFS birimi BitLocker şifrelemesini sağlar. Windows Server sistemi kullanıyorsanız, yönergeleri kullanın [BitLocker'ı Windows Server 2012 R2'de etkinleştirme](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
3.  Şifrelenmiş birime veri kopyalayın. Sürükle bırak veya Robocopy veya böyle bir kopyalama aracını kullanın.
4.  Yönetici ayrıcalıklarıyla bir PowerShell veya komut satırı penceresi açın. Sıkıştırması açılmış klasöre dizini değiştirmek için aşağıdaki komutu çalıştırın:
    
    `cd C:\WaImportExportV1`
5.  Sürücünün BitLocker anahtarı almak için aşağıdaki komutu çalıştırın:
    
    `manage-bde -protectors -get <DriveLetter>:`
6.  Disk hazırlamak için aşağıdaki komutu çalıştırın. **Veri boyutu bağlı olarak bu gün olarak birkaç saat sürebilir.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /sk:<Storage account key> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /skipwrite 
    ```
    Günlük dosyası, Aracı çalıştırdığınız klasörde oluşturulur. İki dosyayı da oluşturulan - bir *.xml* dosyası (aracını çalıştırdığınız klasöre) ve bir *sürücü manifest.xml* dosyası (veri bulunduğu klasörü).
    
    Kullanılan parametreler aşağıdaki tabloda açıklanmıştır:

    |Seçenek  |Açıklama  |
    |---------|---------|
    |/j:     |.Jrn uzantısına sahip bir günlük dosyası adı. Sürücü bir günlük dosyası oluşturulur. Disk seri numarası günlük dosyası adı kullanmanızı öneririz.         |
    |/id:     |Oturum kimliği Benzersiz oturum sayısı komutu her örneği için kullanın.      |
    |/sk:     |Azure depolama hesabı anahtarı.         |
    |/t:     |Gönderilmeye diskinin sürücü harfi. Örneğin, sürücü `D`.         |
    |/bk:     |Sürücüsü için BitLocker anahtarı. Çıktısından sayısal parolası `manage-bde -protectors -get D:`      |
    |/srcdir:     |Ardından gönderilmeye diskinin sürücü harfi `:\`. Örneğin, `D:\`.         |
    |/dstdir:     |Azure depolama alanındaki hedef kapsayıcısının adı.         |
    |/skipwrite:     |Hiçbir kopyalanması gereken yeni verileri ve disk üzerinde var olan veri olduğunu belirten seçeneği hazırlanması sağlamaktır.          |
7. Sevk edilmesi gereken her disk için önceki adımı yineleyin. Her komut satırı çalıştırma için sağlanan ada sahip bir günlük dosyası oluşturulur.
    
    > [!IMPORTANT]
    > - Günlük dosyası ile birlikte bir `<Journal file name>_DriveInfo_<Drive serial ID>.xml` dosyası ayrıca araç bulunduğu aynı klasörde oluşturulur. Günlük dosyası çok büyük ise bir proje oluştururken, .xml dosyasını günlük dosyası yerine kullanılır. 

## <a name="step-2-create-an-import-job"></a>2. Adım: İçeri aktarma işi oluşturma

Azure portalında içeri aktarma işi oluşturmak için aşağıdaki adımları gerçekleştirin.

1. Oturum https://portal.azure.com/.
2. Git **tüm hizmetleri > depolama > içeri/dışarı aktarma işleri**. 
    
    ![İçeri/dışarı aktarma işleri için Git](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Tıklayın **içeri/dışarı aktarma işi oluşturma**.

    ![Oluşturma içeri/dışarı aktarma işini tıklatın](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. İçinde **Temelleri**:

   - Seçin **azure'a aktar**.
   - İçeri aktarma işi için açıklayıcı bir ad girin. İşlerinizi ilerlemesini izlemek için bir ad kullanın.
       - Ad yalnızca küçük harf, sayı ve kısa çizgi içerebilir.
       - Ad bir harf ile başlamalı ve boşluk içeremez.
   - Abonelik seçin.
   - Bir kaynak grubu seçin veya girin.  

     ![1. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. İçinde **iş ayrıntıları**:

    - Sürücü hazırlık adımında elde ettiğiniz sürücü günlük dosyalarını karşıya yükleyin. Varsa `waimportexport.exe version1` olan kullanıldığında, hazırladığınız her sürücü için bir dosya karşıya yükleyin. 2 MB günlük dosyası boyutunu aşıyor sonra kullanabileceğiniz `<Journal file name>_DriveInfo_<Drive serial ID>.xml` ile günlük dosyası da oluşturmuştur. 
    - Verilerin nerede yer alacağını hedef depolama hesabını seçin. 
    - Dropoff konum seçili depolama hesabının bölgeye göre otomatik olarak doldurulur.
   
   ![2. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. İçinde **iade sevkiyat bilgilerini**:

   - Taşıyıcı açılır listeden seçin. Bir taşıyıcı FedEx/DHL dışında kullanmak istiyorsanız, açılır listeden var olan bir seçenek seçin. İlgili Azure veri kutusu işlemleri takım konumundaki `adbops@microsoft.com` ile kullanmayı planladığınız taşıyıcı ilgili bilgileri.
   - Bu operatör ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içeri aktarma işi tamamlandıktan sonra geri için sürücüleri göndermeye bu hesabı kullanır. Bir hesabı yoksa, oluşturun bir [FedEx](https://www.fedex.com/us/oadr/) veya [DHL](https://www.dhl.com/) taşıyıcı hesap.
   - Bir tam ve geçerli ilgili kişi adı, telefon, e-posta, posta adresi, şehir, posta, eyalet/il ve ülke/bölge belirtin. 
        
       > [!TIP] 
       > Tek bir kullanıcı için bir e-posta adresi belirtmek yerine, Grup e-posta sağlayın. Bu, bir yönetici ayrılsa bile bildirimleri almak sağlar.

     ![3. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. İçinde **özeti**:

   - Özet olarak sağlanan proje bilgileri gözden geçirin. İş adı ve teslimat adresi diskleri azure'a geri gönderin için Azure veri merkezi not edin. Bu bilgiler daha sonra sevkiyat etiketi kullanılır.
   - Tıklayın **Tamam** içeri aktarma işi oluşturma.

     ![4. adım - içeri aktarma işi oluşturma](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives"></a>3. adım: Sürücüleri gönderin 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>4. Adım: İş izleme bilgilerini güncelleştir

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>5. Adım: Azure'a verilerin yüklendiğini doğrulama

İş tamamlanana kadar izleyin. İş tamamlandıktan sonra verilerinizi Azure'a karşıya yüklendiğini doğrulayın. Yalnızca, karşıya yükleme başarılı olduğunu doğruladıktan sonra şirket içi verileri silin.

## <a name="next-steps"></a>Sonraki adımlar

* [İşimin ve Sürücümün durumunu görüntüleme](storage-import-export-view-drive-status.md)
* [İçeri/dışarı aktarma gereksinimlerini gözden geçirin](storage-import-export-requirements.md)


