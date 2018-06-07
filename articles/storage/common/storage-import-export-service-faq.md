---
title: Azure içeri/dışarı aktarma hizmeti hakkında SSS | Microsoft Docs
description: Okuma Azure içe aktarma dışarı aktarma hizmeti hakkında sık sorulan sorulara yanıtlar.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: alkohli
ms.openlocfilehash: ed928452946b871ee9192bda82fcbf205b96e6e0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661024"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Azure içeri/dışarı aktarma hizmeti: sık sorulan sorular 
Aşağıdaki sorular ve Azure depolama alanına veri aktarmak için Azure içeri/dışarı aktarma hizmetini kullandığınızda karşılaşabileceğiniz yanıtlarını geçerlidir. Sorular ve yanıtlar Aşağıdaki kategorilerde düzenlenir:

- İçeri/dışarı aktarma hizmeti hakkında
- Diskleri içeri/dışarı aktarma için hazırlama
- İşlerini içeri/dışarı aktarma
- Diskleri aktarma
- Muhtelif Hükümler 

## <a name="about-importexport-service"></a>İçeri/dışarı aktarma hizmeti hakkında

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmetini kullanarak Azure File storage kopyalayabilir miyim?

Evet. Azure içeri/dışarı aktarma hizmeti destekler Azure File Storage alın. Azure dosyaları dışarı aktarma şu anda desteklemiyor.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>Azure içeri/dışarı aktarma hizmeti için CSP abonelikler var mı?

Evet. Azure içeri/dışarı aktarma hizmeti, bulut çözüm sağlayıcıları (CSP) abonelikleri destekler.

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>O365'e PST posta kutuları ve SharePoint verilerini kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?

Evet. Daha fazla bilgi için Git [alma PST dosyaları veya Office 365 SharePoint verileri](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Azure yedekleme hizmeti yedeklerim çevrimdışı kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?

Evet. Daha fazla bilgi için Git [Çevrimdışı Yedekleme iş akışı Azure Yedekleme'de](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Sürücüleri içeri/dışarı aktarma işleri için Microsoft'tan satın alabilir?

Hayır. Kendi sürücüler içeri aktarılacak sevk ve işler dışarı gerekir.


## <a name="preparing-disks-for-importexport"></a>Diskleri içeri/dışarı aktarma için hazırlama

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>İçe aktarma işi için sürücü hazırlık adımı atlayabilirsiniz? Bir sürücüyü kopyalamadan hazırlama?

Hayır. Azure WAImportExport aracını kullanarak veri almak için kullanılan herhangi bir sürücü hazırlanması gerekir. Ayrıca verileri diske kopyalamak için Aracı'nı kullanın.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>Dışarı aktarma işini oluştururken herhangi disk hazırlığı gerçekleştirmek gerekiyor mu?

Hayır. Bazı prechecks önerilir. Gerekli disk sayısını denetlemek için WAImportExport aracın PreviewExport komutunu kullanın. Daha fazla bilgi için bkz: [bir dışarı aktarma işi için sürücü kullanımı Önizleme](https://msdn.microsoft.com/library/azure/dn722414.aspx). Komut, kullanacaksanız sürücüleri boyutuna göre seçilen BLOB'lar için sürücü kullanımı önizlemesini yardımcı olur. Ayrıca okuma ve dışa aktarma işi için gönderilen sabit sürücü yazma denetleyin.

## <a name="importexport-jobs"></a>İşlerini içeri/dışarı aktarma

### <a name="can-i-cancel-my-job"></a>İşim iptal edebilir miyim?
Evet. Durumunu olduğunda bir işi iptal edebilirsiniz **oluşturma** veya **sevkiyat**. Bu aşamalar ötesinde iş iptal edilemiyor ve son aşaması kadar devam eder.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Azure portalında tamamlanmış işlerin durumunu ne kadar süreyle görüntüleyebilirim?
90 güne kadar tamamlanmış işlerin durumunu görüntüleyebilirsiniz. Tamamlanan İşler 90 gün sonra silinir.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>İçeri veya dışarı aktarma 10'dan fazla sürücüler isterseniz ne yapmalıyım?
Bir içeri aktarma veya dışarı aktarma işini tek bir İşte yalnızca 10 sürücüleri başvuru. 10'dan fazla sürücüleri sevk etmek için birden çok iş oluşturmanız gerekir. Aynı işle ilişkili sürücüleri aynı pakette birlikte sevk gerekir. Daha fazla bilgi ve veri kapasitesi birden çok disk yayıldığında Kılavuzu işleri içeri aktarmak için Microsoft'a başvurun bulkimport@microsoft.com.                                                              

## <a name="shipping-disks"></a>Diskleri aktarma

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>En fazla sayısını HDD için bir sevkiyat nedir?
Bir sevkiyat HDD sayısında bir sınır yoktur. Diskleri için birden çok iş aitse, öneririz: 
- karşılık gelen iş adları disklerle etiketleyin.
- -1 ile sonekine izleme numarasıyla işleri güncelleştirme-2 vb.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>My paketinde HDD dışında her şey eklemeliyim?
Yalnızca sabit sürücülerinizin sevkiyat paketinde birlikte. Güç kaynağı kabloları veya USB kablosu gibi öğeleri dahil etmeyin.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>FedEx veya DHL kullanarak my sürücüleri sevk var mı?
Sürücüleri FedEx, DHL, UPS veya ABD posta hizmeti gibi bilinen bir taşıyıcı kullanarak Azure veri merkezine gönderebilirsiniz. Ancak, veri merkezi size sürücülerden dönüş sevkiyat için sağlamanız gerekir:

- ABD ve AB FedEx hesap numarası veya
- Asya ve Avustralya'da bölgelerde DHL hesap numarası.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Sürücümün uluslararası sevkiyat ile herhangi bir kısıtlama var mı?
Lütfen sevkiyat fiziksel ortam uluslararası sınırların arası gerekebilir unutmayın. Fiziksel ortam ve veri içe aktarılan ve/veya ilgili yasalarına uygun olarak dışarı olduğunu sağlamaktan sorumludur. Fiziksel medya göndermeden önce medya ve verilerinizi yasal olarak tanımlanan veri merkezine gönderilebilir olduğunu doğrulamak için danışmanlar danışın. Bu, Microsoft zamanında ulaştığında emin olmaya yardımcı olur.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Bir işi oluştururken, teslimat adresi my depolama hesabı konumdan farklı bir konumdur. Ne yapmalıyım?

Bazı depolama hesabı konumları alternatif sevkiyat konumlara eşlenir. Daha önce kullanılabilir sevkiyat konumlarını da geçici olarak alternatif konumlara eşlenebilir. Her zaman sürücülerinizin teslim etmeden önce işi oluşturma sırasında sağlanan sevkiyat adresi denetleyin.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Sürücümün sevkiyat, taşıyıcı veri merkezi iletişim adresi ve telefon numarası için sorar. Ne ı sağlamanız gerekir?

Telefon numarası ve DC adresi iş oluşturmanın bir parçası sağlanır.


## <a name="miscellaneous"></a>Muhtelif Hükümler

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>I yanlışlıkla uymuyor bir HDD desteklenen gereksinimleri göndermesi durumunda ne olur?

Azure veri merkezine uymuyor sürücü, desteklenen gereksinimlerine dönecektir. Yalnızca bazı paketindeki sürücülerden biri destek gereksinimlerini bu sürücüleri işlenir ve gereksinimlerini karşılamayan sürücüler için döndürülür.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>Hizmet, bunları dönmeden önce sürücüleri biçimlendirmeniz mu?

Hayır. Tüm sürücüler BitLocker ile şifrelenmiş.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Bu hizmeti tarafından aktarılan verilerin nasıl erişebilir mi?

Azure Portalı'nı kullanın veya [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) Azure depolama hesabınızın altında verilere erişmek için.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Alma işlemi tamamlandıktan sonra ne my veri görünür depolama hesabında? Belgelerim dizini hiyerarşi korunur?

Bir sabit sürücü içeri aktarma işi için hazırlık yaparken, hedef CSV kümesindeki DstBlobPathOrPrefix alan belirtilir. Bu sabit sürücüsünden veri kopyalanır depolama hesabındaki hedef kapsayıcıdır. Bu hedef kapsayıcı içindeki sabit sürücüden klasörler için oluşturulan sanal dizinleri ve blobları dosyaları için oluşturulur. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Depolama Hesabımı zaten mevcut dosyaların bir sürücüye sahipse, hizmetin varolan BLOB veya dosyaların üzerine yazmaz?

Bağlıdır. Sürücü hazırlanırken hedef dosyaların üzerine ya da yoksayılan alanını dataset CSV dosyası kullanarak adlı değerlendirme belirtebilirsiniz: < yeniden adlandırma | Hayır üzerine | üzerine >. Varsayılan olarak, hizmet yeni dosyaları yeniden adlandırır yerine mevcut BLOB veya dosyaların üzerine yazın.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>WAImportExport aracının 32-bit işletim sistemleriyle uyumlu mu?
Hayır. WAImportExport aracı yalnızca 64-bit Windows işletim sistemleriyle uyumlu değil. Desteklenen işletim sistemi tam bir listesi için Git [desteklenen işletim sistemleri](). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>En fazla blok blobu ve sayfa Blob Azure içeri/dışarı aktarma tarafından desteklenen boyutu nedir?

En fazla blok blobu boyutudur yaklaşık 4.768TB veya 5,000,000 MB.
En fazla sayfa Blob boyutu 1 TB'tır.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>Azure içeri/dışarı aktarma AES 256 şifrelenmesini destekliyor mu?
Azure içeri/dışarı aktarma hizmeti varsayılan olarak AES-128 bitlocker şifrelemesi kullanır. Bu veri kopyalamadan önce el ile bitlocker ile şifreleyerek için AES 256 değiştirebilirsiniz. 

- Kullanıyorsanız [WAImportExport V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), aşağıda bir örnek komut verilmiştir
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Kullanıyorsanız [WAImportExport V2](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) "AlreadyEncrypted" belirtin ve CSV driveset anahtarı sağlayın.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma nedir?](storage-import-export-service.md)


