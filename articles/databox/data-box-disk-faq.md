---
title: Microsoft Azure Data Box Disk Hakkında SSS | Microsoft Docs
description: Azure'da büyük miktarlarda veri aktarımı yapmanızı sağlayan bir bulut çözümü olan Azure Data Box Disk hakkında sık sorulan soruları ve yanıtlarını içerir
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/17/2018
ms.author: alkohli
ms.openlocfilehash: 5288e9900c75eae7601b84f7366edf9ac739d5da
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125812"
---
# <a name="what-is-azure-data-box-disk-preview"></a>Azure Data Box Disk nedir? (Önizleme)

Microsoft Azure Data Box Disk bulut istasyonu çözümü, terabaytlarca veriyi Azure'a hızlı, uygun maliyetli ve güvenilir bir şekilde göndermenizi sağlar. Bu SSS belgesinde Azure portaldaki Data Box Disklerini kullanımınızla ilgili sorulara ve yanıtlarına yer verilmiştir. 

Sorular ve yanıtlar aşağıdaki kategorilere ayrılmıştır:

- Hizmet hakkında
- Yapılandırma ve bağlanma 
- Durumu izleme
- Geçiş verileri 
- Verileri doğrulama ve yükleme 

> [!IMPORTANT]
> Data Box Disk önizleme aşamasındadır. Bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="about-the-service"></a>Hizmet hakkında

### <a name="q-what-is-azure-data-box-service"></a>S. Azure Data Box hizmeti nedir? 
A.  Azure Data Box hizmeti, çevrimdışı veri alımı için tasarlanmıştır. Bu hizmet farklı depolama kapasitelerinde veri aktarımı gerçekleştirmek için tasarlanmış olan bir dizi ürünü yönetir. 

### <a name="q-what-are-azure-data-box-disks"></a>S. Azure Data Box Diskleri nedir?
A. Azure Data Box Diskleri terabaytlarca veriyi Azure'da çift yönlü olarak hızlı, uygun maliyetli ve güvenli bir şekilde aktarmanızı sağlar. Microsoft size toplamda maksimum 35 TB depolama kapasitesi sunan 1 ile 5 arasında disk gönderir. Bu diskleri Azure portalda Data Box hizmetiyle kolayca yapılandırabilir, bağlayabilir ve disklerin kilidini açabilirsiniz.  

Diskler, Microsoft BitLocker sürücü şifrelemesi ile şifrelenir ve şifreleme anahtarlarınız, Azure portalda yönetilir. Ardından müşterinin sunucularındaki verileri kopyalarsınız. Microsoft verilerinizi veri merkezinde hızlı ve özel bir ağ yükleme bağlantısı aracılığıyla sürücüden buluta geçirir ve Azure'a yükler.

### <a name="q-when-should-i-use-data-box-disks"></a>S. Data Box Disklerini ne zaman kullanmalıyım?
A. Azure'a aktarmak istediğiniz 35 TB (veya daha az) boyutunda veriniz varsa Data Box Disklerinden faydalanabilirsiniz.

### <a name="q-what-is-the-price-of-data-box-disks"></a>S. Data Box Disklerinin maliyeti nedir?
A. Önizleme sırasında Data Box Diskleri ücretsiz olarak kullanılabilir. Sevkiyat da ücretsizdir ancak Azure depolama ücretleri tahsil edilecektir.

### <a name="q-how-do-i-get-data-box-disks"></a>S. Data Box Disklerini nasıl edinebilirim? 
A.  Azure Data Box Disklerini edinmek için öncelikle [Data Box Disk önizlemesine](http://aka.ms/AzureDataBox) kaydolun. Ardından Azure portalda oturum açın ve diskler için bir Data Box siparişi oluşturun. İletişim bilgilerinizi girin ve bildirim ayarlarını yapın. Sipariş verdikten sonra kullanılabilirlik durumuna göre diskler 10 gün içinde gönderilir.   

### <a name="q-what-is-the-maximum-amount-of-data-i-can-transfer-with-data-box-disks-in-one-instance"></a>S. Data Box Disklerine tek seferde en fazla ne kadar veri aktarabilirim?
A. Her biri 8 TB boyutunda (7 TB kullanılabilir kapasite) 5 disk için kullanabileceğiniz maksimum kapasite 35 TB olacaktır. Dolayısıyla tek seferde en fazla 35 TB veri aktarabilirsiniz.  Daha fazla veri aktarmak için daha fazla disk sipariş etmeniz gerekir.

### <a name="q-how-can-i-check-if-data-box-disks-are-available-in-my-region"></a>S. Data Box Disklerinin bulunduğum bölgede kullanılabilir durumda olup olmadığını nasıl kontrol edebilirim? 
A.  Data Box Diskleri önizleme aşamasında ABD'de ve tüm Avrupa Birliği ülkelerinde kullanılabilir.  

### <a name="q-which-regions-can-i-store-data-in-with-data-box-disks"></a>S. Data Box Diskleri ile hangi bölgelerde veri depolayabilirim?
A. Data Box Disk, önizleme aşamasında ABD, Batı Avrupa ve Kuzey Avrupa'daki tüm bölgelerde kullanılabilir. Yalnızca Azure genel bulut bölgeleri desteklenir. Azure Kamu veya diğer bağımsız bulutlar desteklenmez.

### <a name="q-whom-should-i-contact-if-i-encounter-any-issues--with-data-box-disks"></a>S. Data Box Diskleriyle ilgili sorun yaşamam halinde kiminle iletişim kurmam gerekir?
A. Data Box Diskleriyle ilgili sorun yaşamanız halinde lütfen [Data Box Disk Desteği](mailto:expresspodsupport@microsoft.com) ile iletişime geçin.

## <a name="configure-and-connect"></a>Yapılandırma ve bağlanma
 
### <a name="q-can-i-specify-the-number-of-data-box-disks-in-the-order"></a>S. Siparişteki Data Box Diski sayısını seçebilir miyim?
A.  Hayır. Verilerinizin boyutuna ve disklerin kullanılabilirliğine göre 8 TB boyutunda diskler (en fazla 5 disk) gönderilir.  

### <a name="q-how-do-i-unlock-the-data-box-disks"></a>S. Data Box Disklerinin kilidini nasıl açabilirim? 
A.  Azure portalda Data Box Disk siparişinize gidin ve **Cihaz ayrıntıları**'na gidin. Destek anahtarını kopyalayın. Data Box Disk kilit açma aracını Azure portaldan indirin, ayıklayın ve *DataBoxDiskUnlock.exe* uygulamasını disklere kopyalamak istediğiniz verilerin bulunduğu bilgisayarda çalıştırın. Disklerin kilidini açmak için destek anahtarını girin. Aynı destek anahtarı tüm disklerin kilidini açar.

### <a name="q-can-i-use-a-linux-host-computer-to-connect-and-copy-the-data-on-to-the-data-box-disks"></a>S. Data Box Disklerine bağlanmak ve veri kopyalamak için Linux ana bilgisayarı kullanabilir miyim?
A.  Hayır. Yalnızca Windows bilgisayarlar desteklenir. Daha fazla bilgi için ana bilgisayarınızın [Desteklenen işletim sistemleri](data-box-disk-system-requirements.md) listesine gidin.

### <a name="q-my-disks-are-dispatched-but-now-i-want-to-cancel-this-order-why-is-the-cancel-button-not-available"></a>S. Disklerim yola çıktı ancak şimdi siparişimi iptal etmek istiyorum. Neden iptal düğmesi kullanılamıyor?
A.  Siparişi yalnızca disk siparişi verdikten sonra ancak diskler gönderilmeden önce iptal edebilirsiniz. Diskler yola çıktıktan sonra siparişi iptal edemezsiniz. Önizleme süresi boyunca disklerinizi ücretsiz olarak iade edebilirsiniz ancak bu durum muhtemelen çözüm genel kullanıma sunulduğunda değişecektir. 

### <a name="q-can-i-connect-multiple-data-box-disks-at-the-same-to-the-host-computer-to-transfer-data"></a>S. Veri aktarımı için aynı anda birden fazla Data Box Diskini ana bilgisayara bağlayabilir miyim?
A. Evet. Evet, birden fazla Data Box Diski aynı ana bilgisayara bağlanarak aynı anda birden fazla veri aktarım ve kopyalama işi çalıştırılabilir.

## <a name="track-status"></a>Durumu izleme

### <a name="q-how-do-i-track-the-disks-from-when-i-placed-the-order-to-shipping-the-disks-back"></a>S. Diskleri geri göndermek için sipariş oluşturduktan sonra diskleri nasıl takip edebilirim? 
A.  Data Box Disk siparişinin durumunu Azure portaldan takip edebilirsiniz. Siparişi oluşturduğunuzda bildirimlerin gönderileceği bir e-posta adresi girmeniz de istenir. E-posta adresi girdiyseniz sipariş durumundaki tüm değişiklikler bu adrese gönderilir. [Bilgilendirme e-postalarını yapılandırma](data-box-portal-ui-admin.md#edit-notification-details) hakkında daha fazla bilgi edinin.

### <a name="q-how-do-i-return-the-disks"></a>S. Diskleri nasıl iade edebilirim? 
A.  Microsoft tarafından gönderilen Data Box Diski paketinde iade için kullanabileceğiniz bir sevkiyat etiketi bulunur. Etiketi kutunun üzerine yapıştırın ve kapattığınız paketi sevkiyat şirketine teslim edin. Etiketin hasar görmesi veya kaybolması durumunda **Genel bakış > Sevkiyat etiketini indir** sayfasına gidin ve yeni bir sevkiyat etiketi indirin.

## <a name="migrate-data"></a>Geçiş verileri

### <a name="q-what-is-the-maximum-data-size-that-can-be-used-with-data-box-disks"></a>S. Data Box Diskleri ile kullanılabilecek maksimum veri boyutu nedir?  
A.  Data Box Diskleri çözümünde toplam 35 GB kapasiteye sahip en fazla 5 disk kullanılabilir. Her bir disk 8 TB (7 TB kullanılabilir) boyutundadır. 

### <a name="q-what-are-the-maximum-block-blob-and-page-blob-sizes-supported-by-data-box-disks"></a>S. Data Box Diskleri tarafından desteklenen maksimum blok blobu ve sayfa blobu boyutu nedir? 
A.  Maksimum boyutlar Azure Depolama sınırları ile belirlenir. Maksimum blok blobu yaklaşık 4,768 TiB, maksimum sayfa blobu boyutu ise 8 TiB olarak belirlenmiştir. Daha fazla bilgi için bkz. [Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri](../storage/common/storage-scalability-targets.md). 

### <a name="q-what-is-the-data-transfer-speed-for-data-box-disks"></a>S. Data Box Disklerinin veri aktarım hızı nedir?
A. USB 3.0 bağlantısıyla yapılan testlerde disk performansının 430 MB/sn seviyesine çıkabildiği görülmüştür. Gerçek performans kullanılan dosya boyutuna göre değişiklik gösterecektir. Daha küçük dosyalarda performans daha düşük olabilir.

### <a name="q-how-do-i-know-that-my-data-is-secure-during-transit"></a>S. Verilerimin aktarım sırasında güvende olduğundan nasıl emin olabilirim? 
A.  Data Box Diskleri BitLocker AES-128 bit şifreleme kullanılarak şifrelenir ve destek anahtarına yalnızca Azure portaldan erişilebilir. Destek anahtarını almak için hesap kimlik bilgilerinizi kullanarak Azure portalda oturum açın. Data Box Disk kilit açma aracını çalıştırdığınızda bu destek anahtarını kullanın.

### <a name="q-how-do-i-copy-the-data-to-the-data-box-disks"></a>S. Data Box Disklerine nasıl veri kopyalayabilirim? 
A.  Robocopy, Diskboss veya Windows Dosya Gezgini gibi bir SMB kopyalama aracı kullanarak verileri sürükleyip diske bırakın. 

### <a name="q-are-there-any-tips-to-speed-up-the-data-copy"></a>S. Veri kopyalama işlemini hızlandırmaya yönelik ipuçları var mı?
A.  Kopyalama işlemini hızlandırmak için:

- Birden fazla veri kopyalama akışı kullanın. Örneğin Robocopy'de çok iş parçacıklı seçeneği kullanın. Kullanılan komut hakkında daha fazla bilgi için [Öğretici: Azure Data Box Diskine veri kopyalama ve doğrulama](data-box-disk-deploy-copy-data.md#copy-data-to-disks) sayfasına gidin.
- Birden fazla oturum kullanın.
- Ağ paylaşımı üzerinden kopyalama yapmak yerine (ağ hızları kısıtlayıcı olabilir) verilerin, disklerin bağlı olduğu bilgisayarın yerel depolama alanında bulunduğundan emin olun.
- Kopyalama işlemi boyunca USB 3.0 veya üzeri bağlantı kullandığınızdan emin olun. Bilgisayara bağlı USB denetleyicilerini ve USB cihazlarını tanımlamak için [USBView aracını](https://docs.microsoft.com/windows-hardware/drivers/debugger/usbview) indirin ve kullanın.
- Veri kopyalamak için kullanılan bilgisayarın performansını karşılaştırın. Sunucu donanımının performansını karşılaştırmak için [Bluestop FIO aracını](https://bluestop.org/fio/) indirin ve kullanın.

### <a name="q-how-to-speed-up-the-data-if-the-source-data-has-small-files-kbs-or-few-mbs"></a>S. Kaynak veriler küçük dosyalardan (KB veya birkaç MB) oluşuyorsa veri aktarımını nasıl hızlandırabilirim?
A.  Kopyalama işlemini hızlandırmak için:

- Hızlı depolama alanında yerel bir VHDx oluşturun veya HDD/SSD üzerinde boş bir VHD oluşturun (daha yavaştır).
- Bunu bir sanal makineye takın.
- Dosyaları sanal makinenin diskine kopyalayın.


### <a name="q-can-i-use-multiple-storage-accounts-with-data-box-disks"></a>S. Data Box Diskleriyle birden fazla depolama hesabı kullanabilir miyim?
A.  Hayır. Şu an için Data Box Diskleri ile yalnızca tek bir depolama hesabı (genel veya klasik) kullanılabilir. Hem sık hem de seyrek erişimli bloblar desteklenir. Önizleme sırasında yalnızca Azure genel bulutundaki ABD, Batı Avrupa ve Kuzey Avrupa depolama hesapları desteklenir.

## <a name="verify-and-upload"></a>Doğrulama ve yükleme

### <a name="q-how-soon-can-i-access-my-data-in-azure-once-ive-shipped-the-disks-back"></a>S. Diskleri geri gönderdikten sonra verilerime Azure'da nasıl erişebilirim? 
A.  Veri Kopyalama işlemi için sipariş durumu tamamlandı olarak değiştiğinde verilerinize doğrudan erişim sağlayabilirsiniz.

### <a name="q-where-is-my-data-located-in-azure-after-the-upload"></a>S. Yükleme sonrasında verilerim Azure'da hangi konumda bulunur?
A.  Verileri diskinizdeki *BlockBlob* ve *PageBlob* klasörlerinin altına kopyaladığınızda Azure depolama hesabında *BlockBlob* ve *PageBlob* klasörünün her alt klasörü için bir kapsayıcı oluşturulur. Dosyaları doğrudan *BlockBlob* ve *PageBlob* klasörlerine kopyaladıysanız Azure Depolama hesabında *$root* adlı varsayılan kapsayıcıya yerleştirilir. 

### <a name="q-i-just-noticed-that-i-did-not-follow-the-azure-naming-requirements-for-my-containers-will-my-data-fail-to-upload-to-azure"></a>S. Kapsayıcılarım için Azure adlandırma gereksinimlerine uygun hareket etmediğimi fark ettim. Verilerim yine de Azure'a yüklenir mi?
A. Kapsayıcı adlarında büyük harf kullandıysanız bunlar otomatik olarak küçük harfe dönüştürülür. Adlar diğer kurallara (özel karakterler, diğer diller gibi) uygun değilse yükleme işlemi başarısız olur.

### <a name="q-how-do-i-verify-the-data-i-copied-onto-multiple-data-box-disks"></a>S. Birden fazla Data Box Diskine kopyaladığım verileri nasıl doğrulayabilirim?
A.  Veri kopyalama işlemini tamamladıktan sonra *AzureImportExport* klasöründe bulunan `AzureExpressDiskService.cmd` uygulamasını çalıştırarak doğrulama için sağlama toplamı oluşturabilirsiniz. Birden fazla diskiniz varsa her disk için bir komut penceresi açarak bu komutu çalıştırmanız gerekir. Bu işlemin verilerinizin boyutuna göre uzun sürebileceğini (birkaç saat) unutmayın.

### <a name="q-what-happens-to-my-data-after-i-have-returned-the-disks"></a>S. Diskleri iade ettiğimde verilerime ne olur?
A.  Veriler Azure'a kopyalandıktan sonra disklerdeki veriler NIST SP 800-88 Revision 1 yönergelerine uygun ve güvenli bir şekilde silinir.  

### <a name="q-how-is-my-data-protected-during-transit"></a>S. Verilerim aktarım sırasında nasıl korunur? 
A.  Data Box Diskleri AES-128 Microsoft BitLocker şifrelemesi ile korunur. Tüm disklerin kilidini açmak ve verilere erişmek için tek bir destek anahtarı kullanılması gerekir.

### <a name="q-do-i-need-to-rerun-checksum-validation-if-i-add-more-data-to-the-data-box-disks"></a>S. Data Box Disklerine daha fazla veri eklersem sağlama toplamı doğrulama işlemini tekrar çalıştırmam gerekir mi?
A. Evet. Verilerinizi doğrulamaya karar verirseniz (bunu yapmanızı öneririz!) disklere daha fazla veri eklemeniz durumunda doğrulama işlemini tekrarlamanız gerekir.

### <a name="q-i-used-all-my-disks-to-transfer-data-and-need-to-order-more-disks-is-there-a-way-to-quickly-place-the-order"></a>S. Veri aktarımı için tüm diskleri kullandım ve daha fazla disk sipariş etmem gerekiyor. Siparişi hızlı bir şekilde vermek için kullanabileceğim bir yöntem var mı?
A. Önceki siparişinizi kopyalayabilirsiniz. Kopyalama işlemi, bir öncekiyle aynı bilgilere sahip bir sipariş oluşturur ve adres, iletişim ve bildirim ayrıntılarını yeniden girmenize gerek kalmadan yalnızca sipariş bilgilerini düzenleyebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar

- [Data Box sistem gereksinimlerini](data-box-disk-system-requirements.md) gözden geçirin.
- [Data Box sınırlarını](data-box-disk-limits.md) anlayın.
- [Azure Data Box Diskini](data-box-disk-quickstart-portal.md) Azure portal'da hızlıca dağıtın.
