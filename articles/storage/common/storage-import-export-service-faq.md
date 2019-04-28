---
title: Azure içeri/dışarı aktarma hizmeti hakkında SSS | Microsoft Docs
description: Okuma, Azure içeri dışarı aktarma hizmeti hakkında sık sorulan soruların yanıtları.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/13/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: ee2917c64843c8ab137e0122d63a328d6c19fedb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478581"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Azure içeri/dışarı aktarma hizmeti: sık sorulan sorular 
Aşağıda, soruları ve Azure depolama alanına veri aktarmak için Azure içeri/dışarı aktarma hizmeti kullanırken karşılaşabileceğiniz yanıtları verilmiştir. Sorular ve yanıtlar aşağıdaki kategorilere ayrılmıştır:

- İçeri/dışarı aktarma hizmeti hakkında
- İçeri/dışarı aktarma için diskleri hazırlama
- İçeri/dışarı aktarma işleri
- Sevkiyat diskler
- Muhtelif Hükümler 

## <a name="about-importexport-service"></a>İçeri/dışarı aktarma hizmeti hakkında

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmetini kullanarak Azure dosya depolama kopyalayabilirim?

Evet. Azure içeri/dışarı aktarma hizmetini destekler, Azure dosya depolama alanına içeri aktarın. Azure dosyaları dışarı aktarma şu anda desteklemiyor.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>Azure içeri/dışarı aktarma hizmeti, müşterilere CSP abonelikleri kullanılabilir?

Evet. Azure içeri/dışarı aktarma hizmeti, bulut çözümü sağlayıcıları (CSP) abonelikleri destekler.

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>O365'e PST posta kutuları ve SharePoint veri kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?

Evet. Daha fazla bilgi için Git [PST içeri aktarma dosyaları veya Office 365 için SharePoint verilerini](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>Azure yedekleme hizmeti yedeklemelerim çevrimdışı kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?

Evet. Daha fazla bilgi için Git [Çevrimdışı Yedekleme iş akışı Azure backup'taki](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>Sürücüleri içeri/dışarı aktarma işleri için Microsoft'tan satın alabilir?

Hayır. İşleri dışarı aktar ve içeri aktarma için kendi sürücüleri gönderin gerekir.


## <a name="preparing-disks-for-importexport"></a>İçeri/dışarı aktarma için diskleri hazırlama

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>İçeri aktarma işi için sürücü hazırlama adımı atlayarak miyim? Bir sürücü kopyalamadan hazırlama?

Hayır. Verileri içeri aktarmak için kullanılan herhangi bir sürücü, Azure WAImportExport aracını kullanarak hazır olun. Ayrıca sürücüye veri kopyalama aracını kullanın.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>Dışarı aktarma işi oluşturma sırasında herhangi bir disk hazırlama gerçekleştirmek gerekiyor mu?

Hayır. Bazı ön denetimler önerilir. Gerekli disk sayısını denetlemek için WAImportExport Aracı'nın PreviewExport komutunu kullanın. Daha fazla bilgi için [dışarı aktarma işi için sürücü kullanımının önizlemesini](https://msdn.microsoft.com/library/azure/dn722414.aspx). Komutunu kullanmak için oluşturacağınız sürücüleri boyutuna bağlı olarak seçili bloblar için sürücü kullanımının önizlemesini yardımcı olur. Ayrıca, okuma ve yazma dışarı aktarma işi için gönderilen sabit sürücü denetleyin.

## <a name="importexport-jobs"></a>İçeri/dışarı aktarma işleri

### <a name="can-i-cancel-my-job"></a>İşim iptal edebilir miyim?
Evet. Durumu olduğunda, bir işi iptal edebilirsiniz **oluşturma** veya **sevkiyat**. Bu aşamalar ötesinde işi iptal edilemez ve son aşamasına kadar devam eder.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>Ne kadar süreyle tamamlanmış işlerin durumunu Azure portalında görebilir miyim?
90 güne kadar tamamlanmış işlerin durumunu görüntüleyebilirsiniz. Tamamlanan İşler 90 gün sonra silinir.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>İçeri aktarma veya 10'dan fazla sürücüleri dışarı aktarmak istiyorsanız ne yapmalıyım?
Bir içeri aktarma veya dışarı aktarma işi, tek bir iş yalnızca 10 sürücüleri başvurabilir. 10'dan fazla sürücüleri gönderin, birden çok iş oluşturmanız gerekir. Aynı pakette aynı işle ilişkili sürücüleri birlikte sevk gerekir. İşleri daha fazla bilgi ve veri kapasitesi birden çok disk yayıldığında rehberlik almak için Microsoft ile iletişime geçin. bulkimport@microsoft.com. 

### <a name="the-uploaded-blob-shows-status-as-lease-expired-what-should-i-do"></a>Karşıya yüklenen blobun, "Kira süresi dolmuş olarak" durumunu gösterir. Ne yapmalıyım?
"Kiralama süresi doldu" alanı yoksayabilirsiniz. İçeri/dışarı aktarma, başka bir işlem paralel blob güncelleştirebilirsiniz emin olmak için karşıya yükleme sırasında bloba kirası alır. Kiralama süresi doldu, içeri/dışarı aktarma için artık yüklüyor ve blob için kullanabileceğiniz anlamına gelir. 

## <a name="shipping-disks"></a>Sevkiyat diskler

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>HDD için de bir sevkiyat sayısı nedir?
HDD sayısında bir sevk irsaliyesi için sınır yoktur. Diskler için birden çok iş aitse, öneririz: 
- karşılık gelen proje adlarını disklerle etiketleyin.
- işleri izleme numarası -1 ile ve sonra güncelleştir-2 vb.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>Paketimle HDD dışında her şey eklemeliyim?
Sabit sürücülerinizi yalnızca gönderim pakette. Güç kaynağı kabloları veya USB kablosu gibi öğeleri dahil değildir.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>My sürücüleri FedEx veya DHL kullanarak gönderin gerekiyor mu?
Sürücüleri FedEx, DHL, UPS ya da BİZE posta hizmeti gibi bilinen bir taşıyıcı kullanarak bir Azure veri merkezine gönderebilirsiniz. Ancak, sürücüleri, veri merkezinden iade sevkiyat için sağlamanız gerekir:

- ABD ve AB FedEx hesabı veya
- Asya ve Avustralya bölgelerinde DHL hesap numarası.

> [!NOTE]
> Hindistan veri merkezlerinde sürücüleri döndürülecek antetiniz (teslim challan) bir bildirim harfi gerektirir. Gerekli giriş geçişi düzenlemek için Ayrıca seçilen operatörünüz ile çekme kitap ve ayrıntıları sahip bir veri merkezinde paylaşın.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>Uluslararası Sürücümün sevkiyat ile herhangi bir kısıtlama var mıdır?
Sevkiyat fiziksel ortam uluslararası sınırlar arası gerekebileceğini unutmayın. Fiziksel ortam ve verileri içeri aktarılan ve/veya ilgili yasalara uygun olarak dışarı emin olmak sizin sorumluluğunuzdadır. Fiziksel medya göndermeden önce medya ve veri yasal tanımlanan veri merkezine gönderilebilir olduğunu doğrulamak için danışmanları danışın. Bu, Microsoft zamanında ulaşmasını sağlamaya yardımcı olur.

### <a name="are-there-any-special-requirements-for-delivering-my-disks-to-a-datacenter"></a>Bir veri merkezine disklerim sunmaya yönelik herhangi özel bir gereksinimi var mıdır?

Gereksinimleri, belirli bir Azure veri merkezi kısıtlamalarına bağlıdır.
- Bir Microsoft Veri merkezinde paket güvenlik nedenleriyle yazılacak gelen kimlik numarasını gerektiren bazı siteler vardır. Sürücüler veya diskler veri merkezine gönderin önce Azure Data Box Operations başvurun (adbops@microsoft.com) bu numara alınamıyor. Bu sayı paket reddedilir.
- Hindistan veri merkezlerinde kavram No ve kamu kimlik kartı sürücüsü, kişisel ayrıntılarını gerektirir (örneğin, yatay kaydırma, AADHAR DL), ad, kişi ve araba bir ağ geçidi giriş geçişi almak istediğiniz sayıyı blondan. Teslimatı gecikmeleri önlemek için bu gereksinimleri hakkında operatörünüz bildirin.


### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Bir proje oluştururken teslimat adresini my depolama hesabının bulunduğu konumdan farklı bir konumdur. Ne yapmalıyım?

Bazı depolama hesabı konumu alternatif sevkiyat konumlara eşlenir. Daha önce sevk kullanılabilir konumların de geçici olarak alternatif konumlara eşlenebilir. Her zaman sürücülerinizin teslim etmeden önce proje oluşturma sırasında sağlanan gönderme adresini denetleyin.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Sevkiyat Sürücümün, taşıyıcı veri merkezi iletişim adresi ve telefon numarası için sorar. Hangi sağlamalıyım?

Telefon numarası ve DC adresi proje oluşturmanın bir parçası sağlanır.


## <a name="miscellaneous"></a>Muhtelif Hükümler

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>Yanlışlıkla uymuyor bir HDD desteklenen gereksinimlerine gönderebilirim ne olur?

Azure veri merkezi, desteklenen gereksinimlerine uymuyor sürücü döndürür. Paketteki sürücüler bazıları destek gereksinimlerini yalnızca, bu sürücüleri işlenir ve gereksinimlerini karşılamayan sürücüleri için döndürülecek.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>Hizmet, bunları döndürmeden önce sürücüleri biçimlendirmeniz mu?

Hayır. Tüm sürücüler BitLocker ile şifrelenir.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>Bu hizmet tarafından aktarılan verilerin nasıl erişebilirim?

Azure portalı veya [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) Azure depolama hesabınızın altında verilere erişmek için.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>İçeri aktarma tamamlandıktan sonra hangi my veri görünür depolama hesabında? Belgelerim dizini hiyerarşi korunur?

Bir sabit sürücü içeri aktarma işi için hazırlık yaparken, hedef CSV kümesindeki DstBlobPathOrPrefix alan tarafından belirtilir. Sabit sürücüsünden veri kopyalandığı depolama hesabı hedef kapsayıcıda budur. Bu hedef kapsayıcı içindeki sabit sürücüden klasörler için oluşturulan sanal dizinleri ve blobları, dosyalar için oluşturulur. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Bir sürücü depolama Hesabımda bulunan dosyalar varsa, hizmet mevcut BLOB'ları veya dosyalarının üzerine mu?

Bağlıdır. Sürücü hazırlanırken, hedef dosyaların üzerine yazılması ya da adlandırılan değerlendirme yoksayılan alanın veri kümesi CSV dosyasını kullanarak belirtebilirsiniz: < yeniden adlandırma | no üzerine | üzerine >. Varsayılan olarak, yeni dosyalar yeniden adlandırır yerine hizmetini mevcut BLOB'ları veya dosyaların üzerine yaz.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>WAImportExport aracının 32-bit işletim sistemleriyle uyumlu mu?
Hayır. WAImportExport aracın yalnızca 64 bit Windows işletim sistemleriyle uyumludur. Desteklenen işletim sistemi tam listesi için Git [desteklenen işletim sistemleri](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>En fazla blok Blob ve Azure içeri/dışarı aktarma tarafından desteklenen sayfa Blob boyutu nedir?

En fazla blok Blob boyutu yaklaşık 4.768TB'tır veya 5,000,000 MB ile sınırlayın.
En fazla sayfa blobu boyutu 8 TB'tır.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>Azure içeri/dışarı aktarma, AES-256'yı şifrelenmesini destekliyor mu?
Azure içeri/dışarı aktarma hizmeti, AES-128 bitlocker şifreleme varsayılan olarak kullanır. Bu veri kopyalamadan önce el ile bitlocker ile şifreleyerek AES-256 değiştirebilirsiniz. 

- Kullanıyorsanız [WAImportExport V1](https://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), aşağıda bir örnek komut verilmiştir.
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Kullanıyorsanız [WAImportExport V2](https://www.microsoft.com/download/details.aspx?id=55280) "AlreadyEncrypted" belirtin ve CSV driveset anahtarı sağlayın.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma nedir?](storage-import-export-service.md)


