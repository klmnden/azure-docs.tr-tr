---
title: "Azure Storage gelen ve giden veri aktarımı için Azure içeri/dışarı aktarma kullanma | Microsoft Docs"
description: "Alma oluşturma ve işleri Azure portalında Azure Storage veri aktarma için dışarı aktarma hakkında bilgi edinin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: muralikk
<<<<<<< HEAD
ms.openlocfilehash: 221bd7662eb4974395c7f970961d5bfb556417f4
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
=======
ms.openlocfilehash: ffcf0766b89cdab7c79c28dad6bf4c80275e33fc
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-azure-storage"></a>Azure depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma
Bu makalede, sizi güvenli bir şekilde büyük miktarlarda verinin Azure Blob Depolama ve Azure dosyaları için bir Azure veri merkezine sevkiyat disk sürücüleri tarafından aktarımı için Azure içeri/dışarı aktarma hizmeti kullanma hakkında adım adım yönergeler sağlar. Bu hizmet, Azure depolama biriminden sabit disk sürücülerine verileri aktarmak ve şirket içi siteleriniz sevk etmek için de kullanılabilir. Tek bir dahili SATA disk sürücüsü verileri Azure Blob storage veya Azure dosyaları içeri aktarılabilir. 

> [!IMPORTANT] 
> Bu hizmet yalnızca iç SATA HDD veya SSD yalnızca kabul eder. Başka bir aygıtı desteklenir. Bunlar mümkünse ya da başka atılan döndüreceği gibi dış HDD, NAS cihazları, vb. göndermeyin.
>
>

İzleyin aşağıdaki disk üzerindeki verileri Azure depolama alanına aktarılması ise adımları.
### <a name="step-1-prepare-the-drives-using-waimportexport-tool-and-generate-journal-files"></a>1. adım: WAImportExport aracını kullanarak sürücü/s hazırlamak ve günlük dosyası/s oluşturun.

1.  Azure depolama alanına aktarılması verileri tanımlamak. Bu, dizinler ve tek başına dosyalar yerel bir sunucu veya ağ paylaşımında olabilir.
2.  Toplam veri boyutuna bağlı olarak, gerekli 2,5 inç SSD veya 2,5" veya sayısını 3,5" SATA II veya III sabit disk sürücüsü temin edin.
3.  Kullanarak doğrudan SATA sabit sürücüleri eklemek veya bir windows makinesine dış USB bağdaştırıcısı ile.
4.  Her sabit sürücü üzerinde tek bir NTFS birimi oluşturun ve birime bir sürücü harfi atama. Hiçbir bağlama.
5.  NTFS birimi bit kasası şifrelemeyi etkinleştirin. Üzerinde https://technet.microsoft.com/en-us/library/cc731549(v=ws.10).aspx to enable encryption on the windows machine. yönergeleri kullanın
6.  Diskleri kopyalama & Yapıştır veya sürükle & bırak veya Robocopy veya böyle bir araç kullanarak bu şifrelenmiş tek NTFS birimlerine tamamen verileri kopyalayın.
7.  Https://www.microsoft.com/en-us/download/details.aspx?id=42659 WAImportExport V1 indirin
8.  İçin varsayılan klasörü waimportexportv1 sıkıştırmasını açın. Örneğin, C:\WaImportExportV1  
9.  Yönetici olarak çalıştır ve PowerShell veya komut satırı açın ve dizin sıkıştırması açılmış klasöre geçin. Örneğin, cd C:\WaImportExportV1
10. Kopya bir not defteri için komut satırı aşağıda ve komut satırı oluşturmak üzere düzenleyebilirsiniz.
  ./WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session #1 /sk:***/t:D == /bk:*** /srcdir:D: \ /dstdir:ContainerName / /skipwrite
    
    bir dosyanın adını /j: .jrn uzantısı ile günlük dosyası çağrılır. Sürücü bir günlük dosyası oluşturulur ve bu nedenle disk seri numarası günlük dosyası adı olarak kullanmak için önerilir.
    /SK: azure depolama hesabı anahtarı. / t: edilmeye diskinin sürücü harfi. Örneğin, D /bk: sürücü /srcdir bit kasası anahtarıdır: edilmeye diskinin sürücü harfi ve ardından: \. Örneğin, D:\
    /dstdir: Azure Storage veri olduğu içeri aktarılacak kapsayıcısının adı.
    /skipwrite 
    
11. Adım 10 her sevk edilmesi gereken disk için yineleyin.
12. Her komut satırını Çalıştır için /j: parametresiyle belirtilen adla bir günlük dosyası oluşturulur.

### <a name="step-2-create-an-import-job-on-azure-portal"></a>2. adım: Azure Portal'da bir alma işi oluşturun.

1. Günlük https://portal.azure.com/ açın ve daha fazla hizmet altında -> depolama "içeri/dışarı aktarma işleri" ->'ı tıklatın **oluşturma içeri/dışarı aktarma işi**.

2. Temel kavramları bölümünde, "Alma içine Azure" seçin, iş adı bir dize girin, bir abonelik seçin, girin veya bir kaynak grubu seçin. İçe aktarma işi için açıklayıcı bir ad girin. Girdiğiniz ad içerebilir yalnızca küçük harfler, sayılar, tire ve alt çizgi, bir harf ile başlamalı ve boşluk içeremez unutmayın. Devam eden ve bunlar tamamlandığında çalışırken işlerinizi izlemek için seçtiğiniz ad kullanır.

3. İş Ayrıntıları bölümünde sürücü hazırlık adımında elde ettiğiniz sürücü günlük dosyalarını karşıya yükleyin. Waimportexport.exe version1 kullandıysanız hazırlandıktan her sürücü için bir dosyayı karşıya yüklemeyi gerekir. Verileri "Alma hedefi" depolama hesabı bölümünde alınacağı depolama hesabı seçin. Teslim konum, seçilen depolama hesabı bölgeye göre otomatik olarak doldurulur.
   
   ![Adım 3 - içe aktarma işi oluşturma](./media/storage-import-export-service/import-job-03.png)
4. İade bilgileri bölümünden sevkiyat, taşıyıcı aşağı açılan listeden seçin ve o taşıyıcı ile oluşturduğunuz geçerli taşıyıcı hesap numarası girin. Microsoft, içe aktarma işi tamamlandıktan sonra geri için sürücüleri sevk etmek için bu hesabı kullanır. Bir tam ve geçerli kişi adı, telefon, e-posta, adres, şehir, ZIP, durumu/proviince ve ülke/bölge sağlar.
   
5. Özet bölümünde Azure sevkiyat adresini DataCenter Azure DC disklere sevkiyat için kullanılmak üzere sağlanır. İş adı ve tam adresini sevkiyat etikette açıklanan emin olun. 

6. İçeri aktarma işi oluşturma işlemini tamamlamak için Özet sayfasında Tamam'ı tıklatın.

### <a name="step-3-ship-the-drives-to-the-azure-datacenter-shipping-address-provided-in-step-2"></a>3. adım: Sevk adım 2'de sağlanan adresi sevkiyat Azure veri merkezine sürücü/s.
FedEx, UPS veya DHL Azure DC paketi sevk etmek için kullanılabilir.

### <a name="step-4-update-the-job-created-in-step2-with-tracking-number-of-the-shipment"></a>4. adım: İçinde Adım2 sevkiyat sayısı izleme ile oluşturulan iş güncelleştirin.
Diskleri sevkiyat sonra geri dönüp **içeri/dışarı aktarma** sayfa Azure Portal'da aşağıdaki adımları kullanarak izleme numarasını güncelleştirmek için bir) gidin ve alma üzerinde b) tıklatın iş tıklatıldığında **güncelleştirme iş durumunu ve bilgileri izleme sürücüleri sevk sonra**. c) seçin "sevk edilmiş olarak işaretle" onay kutusunu d) taşıyıcı ve izleme numarasını belirtin.
İzleme numarası iş oluşturma 2 hafta içinde güncelleştirilmezse, işin süresi dolar. İş ilerleme durumunu portal panosunda izlenebilir. Önceki bölümdeki her iş durumu tarafından anlamı bkz [işi durumunu görüntüleme](#viewing-your-job-status).

## <a name="when-should-i-use-the-azure-importexport-service"></a>Azure İçeri/Dışarı Aktarma hizmetini ne zaman kullanmalıyım?
Karşıya yükleme veya ağ üzerinden veri indirme çok yavaş olduğunda veya ek ağ bant genişliği alma maliyetli Azure içeri/dışarı aktarma hizmeti kullanmayı düşünün.

Bu hizmet gibi senaryolarda kullanabilirsiniz:

* Bulut için veri geçişi: büyük miktarlarda verinin hızla Azure'a taşımak ve düşük maliyetle.
* İçerik dağıtım: hızla müşteri sitelerinize veri gönderme.
* : Azure Storage'da depolamak için şirket içi verilerinizin yedekleri yedekleyin.
* Veri kurtarma: kurtarmak büyük miktarda veri depolama alanına depolanır ve şirket içi konumunuz teslim sahip.

## <a name="prerequisites"></a>Ön koşullar
Bu bölümde bu hizmeti kullanmak için gereken önkoşulları listeler. Lütfen dikkatle sürücülerinizin göndermeden önce gözden geçirin.

### <a name="storage-account"></a>Depolama hesabı
Var olan bir Azure aboneliği ve içeri/dışarı aktarma hizmetini kullanmak için bir veya daha fazla depolama hesabı olması gerekir. Her iş için veya yalnızca bir depolama hesabından veri aktarmak için kullanılabilir. Diğer bir deyişle, bir tek içeri/dışarı aktarma işi birden çok depolama hesaplarında yayılamaz. Yeni bir depolama hesabı oluşturma hakkında daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](storage-create-storage-account.md#create-a-storage-account).

### <a name="data-types"></a>Veri türleri
Azure içeri/dışarı aktarma hizmeti verileri kopyalamak için kullanabileceğiniz **blok** BLOB'lar, **sayfa** BLOB'lar, veya **dosyaları**. Buna karşılık, yalnızca verebilirsiniz **blok** BLOB'lar, **sayfa** BLOB'lar veya **Append** bu hizmeti kullanarak Azure storage bloblarından. Yalnızca Azure dosyaları içe Azure depolama hizmeti destekler. Azure dosyaları dışarı aktarma şu anda desteklenmiyor.

### <a name="job"></a>İş
Alma işlemini veya depodan dışarı aktarma işlemini başlatmak için önce bir iş oluşturun. Bir iş, bir içeri aktarma işi veya bir dışarı aktarma işinin olabilir:

* Şirket içi Azure depolama hesabınıza olan verileri aktarmak istediğinizde bir alma işi oluşturun.
* Depolama hesabınız için bize gönderilen sabit sürücüler için şu anda depolanan verileri aktarmak istediğinizde bir dışarı aktarma işinin oluşturun. Bir proje oluşturduğunuzda, bir Azure veri merkezi için bir veya daha fazla sabit sürücüler sevkiyat olmalıdır, içeri/dışarı aktarma hizmeti bildirin.

* Bir içeri aktarma işi için verilerinizi içeren sabit sürücüler sevkiyat.
* Bir dışarı aktarma işi için boş sabit disk sürücüler sevkiyat.
* İş başına en fazla 10 sabit disk sürücülerinin gönderebilirsiniz.

Bir alma oluşturma veya Azure portalını kullanarak iş dışarı aktarma veya [Azure depolama içeri/dışarı aktarma REST API](/rest/api/storageimportexport).

### <a name="waimportexport-tool"></a>WAImportExport aracı
Oluşturmanın ilk adımı bir **alma** iş sevk edilen sürücülerinizin alma için hazırlamak için. Sürücülerinizin hazırlamak için yerel bir sunucuya bağlanın ve yerel sunucuda WAImportExport aracını çalıştırın. Bu WAImportExport aracı verilerinizi diske kopyalama, sürücüde BitLocker ile verileri şifrelemek ve sürücü günlük dosyaları oluşturma kolaylaştırır.

Günlük dosyaları, iş ve sürücü sürücü seri numarası ve depolama hesabı adı gibi ilgili temel bilgileri saklar. Bu günlük dosyası sürücüsünde depolanmaz. İçeri aktarma işi oluşturma sırasında kullanılır. Bu makalenin sonraki bölümlerinde iş oluşturma hakkında adım adım ayrıntılar verilmiştir.

WAImportExport aracı yalnızca 64-bit Windows işletim sistemiyle uyumlu değil. Bkz: [işletim sistemi](#operating-system) desteklenen belirli işletim sistemi sürümleri için bölüm.

En son sürümünü indirme [WAImportExport aracı](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). WAImportExport aracını kullanma hakkında daha fazla ayrıntı için bkz: [WAImportExport aracını kullanarak](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Önceki sürümü:** yapabilecekleriniz [WAImportExpot V1 karşıdan](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) aracı sürümü ve başvurmak [WAImportExpot V1 kullanım kılavuzda](storage-import-export-tool-how-to-v1.md). Aracı'nın WAImportExpot V1 sürümü için destek sağlar **veri zaten diske önceden yazıldığında diskleri hazırlama**. Ayrıca kullanılabilir yalnızca anahtar SAS anahtarı ise WAImportExpot V1 aracı kullanmanız gerekecektir.

>

### <a name="hard-disk-drives"></a>Sabit disk sürücüleri
Yalnızca 2,5 inç SSD veya 2,5" veya 3,5" SATA II veya III iç HDD içeri/dışarı aktarma hizmeti ile kullanım için desteklenir. Bir tek içeri/dışarı aktarma işi en fazla 10 HDD/SSD olabilir ve tek tek her HDD/SSD herhangi bir boyutta olabilir. Büyük sayıda sürücüsü birden çok iş arasında yayılabilir ve oluşturulabilir işlerin sayısı sınırı yoktur. 

İçeri aktarma işi için yalnızca ilk veri birimi sürücüde işlenir. Veri birimi NTFS ile biçimlendirilmiş olması gerekir.

> [!IMPORTANT]
> Yerleşik bir USB bağdaştırıcısı ile gelen dış sabit disk sürücülerinin, bu hizmet tarafından desteklenmiyor. Ayrıca, bir dış HDD kasa içinde disk kullanılamaz; Lütfen dış HDD göndermeyin.
> 
> 

İç HDD veri kopyalamak için kullanılan dış USB bağdaştırıcıları listesi aşağıdadır. Anker 68UPSATAA - 02BU Anker 68UPSHHDS BU Startech SATADOCK22UE Orico 6628SUS3 C SİY (6628 serisi) Thermaltake BlacX etkin takas SATA dış sabit sürücü yerleştirme istasyon (USB 2.0 & eSATA)

### <a name="encryption"></a>Şifreleme
BitLocker Sürücü Şifrelemesi'ni kullanarak sürücüdeki verilerin şifrelenmesi gerekir. Bunu esnasında bu verilerinizi korur.

İçeri aktarma işi için şifreleme gerçekleştirmenin iki yolu vardır. İlk veri kümesi CSV dosyası sürücü hazırlanması sırasında WAImportExport Aracı'nı çalıştırırken kullanırken seçeneği belirtmek için yoludur. İkinci el ile sürücüde BitLocker şifrelemesini etkinleştirmek ve şifreleme anahtarını driveset CSV WAImportExport aracı komut satırı sürücü hazırlanması sırasında çalıştırırken belirtmek için bir yoludur.

Verilerinizi sürücülere kopyalandıktan sonra dışarı aktarma işleri için hizmet, size geri göndermeden önce BitLocker'ı kullanarak sürücü şifreleyin. Şifreleme anahtarı için Azure portalı üzerinden sağlanır.  

### <a name="operating-system"></a>İşletim Sistemi
Azure sürücüye göndermeden önce WAImportExport aracını kullanarak sabit sürücü hazırlamak için aşağıdaki 64-bit işletim sistemlerinden birini kullanabilirsiniz:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. BitLocker Sürücü Şifrelemesi bu işletim sistemlerini desteklemez.

### <a name="locations"></a>Konumlar
Azure içeri/dışarı aktarma hizmeti için ve tüm ortak Azure depolama hesapları arasından veri kopyalamayı destekler. Sabit disk sürücülerinin aşağıdaki konumlardan birine gönderebilirsiniz. Depolama hesabınız, burada, Azure portal veya içeri/dışarı aktarma REST API'sini kullanarak işini oluştururken bir alternatif sevkiyat konumu sağlanan belirtilmemiş bir ortak Azure konumda ise.

Sevkiyat konumlar desteklenir:

* Doğu ABD
* Batı ABD
* Doğu ABD 2
* Batı ABD 2
* Orta ABD
* Orta Kuzey ABD
* Orta Güney ABD
* Batı Orta ABD
* Kuzey Avrupa
* Batı Avrupa
* Doğu Asya
* Güneydoğu Asya
* Avustralya Doğu
* Avustralya Güneydoğu
* Japonya Batı
* Japonya Doğu
* Orta Hindistan
* Güney Hindistan
* Batı Hindistan
* Orta Kanada
* Doğu Kanada
* Güney Brezilya
* Kore Orta
* ABD Devleti Virginia
* ABD Devleti Iowa
* US DoD Doğu
* US DoD Orta
* Çin Doğu
* Çin Kuzey
* Birleşik Krallık Güney
* Almanya Orta
* Almanya Kuzeydoğu

### <a name="shipping"></a>Aktarma
**Veri merkezine sevkiyat sürücüler:**

Bir içe veya dışa aktarma işi oluştururken, teslimat adresi sürücülerinizin dağıtmayı desteklenen konumlardan birinin sağlanacaktır. Sağlanan teslimat adresi depolama hesabınız konumuna bağlıdır, ancak depolama hesabı konumu ile aynı olmayabilir.

FedEx, UPS veya DHL sürücülerinizin teslimat adresine sevk etmek için kullanılabilir.

**Veri Merkezi sürücülerden sevkiyat:**

Bir içe veya dışa aktarma işi oluştururken, bir dönüş adresi geri, işi tamamlandıktan sonra sürücüleri sevkiyat yapılırken kullanılacak Microsoft sağlamanız gerekir. Lütfen işlem gecikmelerden kaçınmak için geçerli bir dönüş adresi sağlayın emin olun.

Taşıyıcı zincirini korumak için uygun izleme olması gerekir. Geçerli bir FedEx, UPS veya DHL taşıyıcı sağlamalısınız hesap sürücüleri geri aktarma için Microsoft tarafından kullanılacak numarası. FedEx, UPS veya DHL hesap numarası, sürücüler geri ABD ve Avrupa konumlardan sevkiyat için gereklidir. DHL hesap numarası, sürücüler geri Asya ve Avustralya'da konumlardan sevkiyat için gereklidir. Oluşturabileceğiniz bir [FedEx](http://www.fedex.com/us/oadr/) (için ABD ve Avrupa) veya [DHL](http://www.dhl.com/) bir yoksa (Asya ve Avustralya) taşıyıcı hesap. Bir taşıyıcı hesap numarası zaten varsa, Lütfen geçerli olduğunu doğrulayın.

Paketlerinizi sevkiyat içinde koşulların en izlemelisiniz [Microsoft Azure Hizmet Koşulları'nı](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Lütfen sevkiyat fiziksel ortam uluslararası sınırların arası gerekebilir unutmayın. Fiziksel ortam ve veri içe aktarılan ve/veya ilgili yasalarına uygun olarak dışarı olduğunu sağlamaktan sorumludur. Fiziksel medya göndermeden önce medya ve verilerinizi yasal olarak tanımlanan veri merkezine gönderilebilir olduğunu doğrulamak için advisers danışın. Bu, Microsoft zamanında ulaştığında emin olmaya yardımcı olur. Örneğin, uluslararası sınırların arası herhangi bir paket (dışında Kenarlıklar Avrupa Birliği içinde çapraz) paket eşlik için ticari bir fatura gerekir. Taşıyıcı Web sitesinden ticari fatura doldurulmuş bir kopyasını yazdırmak. Ticari faturalar örnektir [DHL ticari fatura](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) ve [FedEx ticari fatura](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Microsoft dışarı aktarma belirtmedi olduğundan emin olun.
> 
> 

## <a name="how-does-the-azure-importexport-service-work"></a>Azure içeri/dışarı aktarma hizmeti nasıl çalışır?
İşlerini oluşturarak ve sabit disk sürücülerinin bir Azure veri merkezine sevkiyat Azure içeri/dışarı aktarma hizmetini kullanarak Azure depolama ve şirket içi site arasında veri aktarımı. Sevk her sabit diskin tek bir iş ile ilişkilidir. Her bir iş, tek bir depolama hesabıyla ilişkilendirilir. Gözden geçirme [önkoşullar bölümünde](#pre-requisites) dikkatle bu hizmetin desteklenen veri türleri gibi özellikleri hakkında bilgi edinmek için disk türlerini, konumlarını ve sevkiyat.

Bu bölümde, dahil edilen adımları almak ve işleri dışarı aktarma yüksek düzeyde açıklanır. Daha sonra [Hızlı Başlat bölümünde](#quick-start), bir içeri aktarma oluşturmak ve işi vermek için adım adım yönergeler sağlıyoruz.

### <a name="inside-an-import-job"></a>İçe aktarma işi
Yüksek bir düzeyde bir alma işi aşağıdaki adımları içerir:

* İçeri aktarılacak veri ve ihtiyacınız olacak sürücü sayısını belirler.
* Verilerinizi Azure depolama alanında hedef blob veya dosya konumunu belirleyin.
* Bir veya daha fazla sabit disk sürücülerine verilerinizi kopyalamak ve BitLocker ile şifrelemek için WAImportExport aracını kullanın.
* Azure portalı veya içeri/dışarı aktarma REST API kullanarak hedef depolama hesabınızda bir alma işi oluşturun. Azure portalını kullanıyorsanız, sürücü günlük dosyalarını karşıya yükleyin.
* Sürücüleri size geri göndermek için kullanılacak taşıyıcı hesap numarası ve dönüş adresi sağlayın.
* Proje oluşturma sırasında sağlanan sevkiyat adresi sabit disk sürücülerine birlikte.
* İzleme numarası alma işi ayrıntıları teslim güncelleştirin ve içeri aktarma işi gönderin.
* Sürücüleri alınan ve Azure veri merkezinde işlenebilir.
* Sürücüleri, içeri aktarma işi sağlanan dönüş adresi taşıyıcı hesabınıza kullanarak aktarılır.
  
    ![Şekil 1:Import iş akışı](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>İçinde bir dışarı aktarma işinin
> [!IMPORTANT]
> Hizmet verme Azure BLOB destekleyebilir ve Azure dosyaları verilmesini desteklemez.
> 
>

Yüksek bir düzeyde bir dışarı aktarma işinin aşağıdaki adımları içerir:

* Dışa aktarılacak veri ve ihtiyacınız olacak sürücü sayısını belirler.
* Kaynak BLOB veya verilerinizi Blob depolamada kapsayıcı yollarını belirleyin.
* Bir dışarı aktarma işinin Azure portalı veya içeri/dışarı aktarma REST API kullanarak kaynak depolama hesabınızı oluşturun.
* Kaynak BLOB veya kapsayıcısı yollarının verilerinizi dışarı aktarma işi belirtin.
* Sürücüleri size geri göndermek için kullanılacak taşıyıcı hesap numarası ve dönüş adresi sağlayın.
* Proje oluşturma sırasında sağlanan sevkiyat adresi sabit disk sürücülerine birlikte.
* İzleme numarası dışa aktarma işi ayrıntıları teslim güncelleştirin ve dışa aktarma işi gönderin.
* Sürücüleri alınan ve Azure veri merkezinde işlenebilir.
* BitLocker ile şifrelenmiş sürücüleri; Azure portalı üzerinden anahtarları kullanılabilir.  
* Sürücüleri, içeri aktarma işi sağlanan dönüş adresi taşıyıcı hesabınıza kullanarak aktarılır.
  
    ![Şekil 2:Export iş akışı](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>İş ve sürücü durumunu görüntüleme
Alma durumunu izlemek veya Azure portalından işleri verebilirsiniz. Tıklatın **içeri/dışarı aktarma** sekmesi. İşleriniz listesini sayfasında görüntülenir.

![Görünüm iş durumu](./media/storage-import-export-service/jobstate.png)

Sürücünüzü işlemde olduğu bağlı olarak aşağıdaki iş durumlardan birini görürsünüz.

| İş Durumu | Açıklama |
|:--- |:--- |
| Oluşturma | Bir işi oluşturulduktan sonra durumu oluşturma için ayarlanır. Proje oluşturma durumundayken içeri/dışarı aktarma hizmeti sürücüleri veri merkezine sevk edilmiş değil varsayar. Bir işi haftaya kadar iki daha sonra otomatik olarak hizmet tarafından silinir, oluşturma durumda kalabilir. |
| Aktarma | Paketinizi sevk sonra Azure portalında izleme bilgilerini güncelleştirmeniz gerekir.  Bu iş "İçine aktarma" açın. İş için iki haftalık sevkiyat durumunda kalır. 
| Alındı | Tüm sürücüleri veri merkezinde alınmış olan sonra iş durumu için alınan ayarlanır. |
| Aktarma | En az bir sürücü işleme başladıktan sonra iş durumu aktarma için ayarlanacak. Ayrıntılı bilgi için aşağıdaki sürücü durumları bölümüne bakın. |
| Paketleme | Tüm sürücüler işlemeyi tamamladıktan sonra sürücüleri size geri gönderilir kadar iş paketleme durumda yerleştirilir. |
| Tamamlandı | İş bir hata olmadan tamamlandı, müşteri için tüm sürücülerin sevk edilmiş sonra iş tamamlandı durumuna ayarlanır. İş Tamamlandı durumunda otomatik olarak 90 gün sonra silinir. |
| Kapalı | İş işleme sırasında hataları olmuştur, müşteri için tüm sürücülerin sevk edilmiş sonra iş kapalı duruma ayarlanır. İş kapalı durumda otomatik olarak 90 gün sonra silinir. |

Bir içe veya dışa aktarma işi ile geçiş yapıldığı gibi ayrı ayrı bir sürücü yaşam döngüsünü aşağıdaki tabloda açıklanmıştır. Geçerli bir işi her sürücüde şimdi Azure portalından görünür durumda.
Aşağıdaki tabloda her bir iş sürücüde geçirir her durumu açıklar.

| Sürücü durumu | Açıklama |
|:--- |:--- |
| Belirtilen | Azure portalından, iş oluşturulduğunda bir içeri aktarma işi için bir sürücü için ilk durumu belirtilen durumudur. İş oluşturulduğunda, hiçbir sürücü belirtildiği için dışa aktarma işi, ilk sürücü durumu alınan durumudur. |
| Alındı | İçeri/dışarı aktarma hizmeti işleci bir içeri aktarma işi için sevkiyat şirketten alınan sürücüleri işlendiğinde alınan duruma sürücü geçişleri. Bir dışarı aktarma işi için ilk sürücü durumu alınan durumudur. |
| NeverReceived | Sürücü NeverReceived durumuna işi için paket ulaşır ancak paketin sürücü içermiyor taşınır. İki hafta teslimat hizmeti aldı, ancak paket henüz veri merkezinde geldi değil bu yana olması durumunda bir sürücü de bu duruma taşıyabilirsiniz. |
| Aktarma | Windows Azure depolama alanına sürücüden veri aktarmak hizmet başladığında, bir sürücü aktarma durumuna taşınır. |
| Tamamlandı | Hizmet başarıyla hatasız tüm verileri aktarıldığında bir sürücü tamamlandı durumuna taşınır.
| CompletedMoreInfo | Hizmet gelen veya sürücüye veri kopyalanırken bazı sorunlarla karşılaştı, bir sürücü CompletedMoreInfo durumuna taşınır. Hataları, uyarı veya bilgilendirme iletileri BLOB'lar üzerine hakkında bilgiler içerebilir.
| ShippedBack | Sürücü, veri merkezi arkadan dönüş adresi sevk edilmiş zaman ShippedBack durumuna taşınır. |

Bu görüntü Azure portalından bir örnek işi sürücü durumunu görüntüler:

![Sürücü durumunu görüntüle](./media/storage-import-export-service/drivestate.png)

Aşağıdaki tabloda, sürücü hata durumları ve her durum için gerçekleştirilen eylemleri açıklar.

| Sürücü durumu | Olay | Çözümleme / sonraki adım |
|:--- |:--- |:--- |
| NeverReceived | (Bu işin sevkiyat bir parçası olarak alındığından) başka bir sevkiyat NeverReceived ulaştığında işaretlenen sürücü. | İşlemler ekibinin sürücü alınan durumuna taşınır. |
| Yok | Başka bir iş parçası olarak veri merkezindeki herhangi bir işi parçası olmayan bir sürücü ulaşır. | Sürücü fazladan bir sürücü olarak işaretlenir ve özgün paket ile ilişkili iş tamamlandığında müşteriye döndürülür. |

### <a name="time-to-process-job"></a>İşlem işi zaman
Gereken bir içeri/dışarı aktarma işini değişir Sevkiyat Zamanı gibi farklı etkenlere bağlı olarak işlenecek süreyi iş türü, türü ve kopyalanan verilerin boyutunu ve sağlanan disk boyutu. İçeri/dışarı aktarma hizmeti bir SLA yok ancak diskleri alındıktan sonra kopyalama 7-10 gün içinde tamamlamak için hizmet çalışır. REST API daha yakından iş ilerleme durumunu izlemek için kullanabilirsiniz. Kopyalama devam eden bir gösterge sağlayan listesi işleri işlemde yüzde tam parametresi yok. Zaman kritik içeri/dışarı aktarma işlemini tamamlamak için tahmin gerekiyorsa bize ulaşın.

### <a name="pricing"></a>Fiyatlandırma
**İşleme ücret sürücü**

Alma işleminizin bir parçası olarak işlenen her bir sürücü için bir sürücü işleme ücret yoktur veya iş dışa aktarın. Ayrıntıları görmek [Azure içeri/dışarı aktarma fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Sevkiyat maliyetleri**

Diskleri Azure'a sevk ettiğinizde, Sevkiyat taşıyıcı sevkiyat maliyet ücret ödersiniz. Microsoft size sürücüleri döndürdüğünde sevkiyat maliyet işi oluşturma sırasında sağlanan taşıyıcı hesap için ücret kesilir.

**İşlem maliyetleri**

Verileri Azure depolama alanına alırken hiçbir işlem maliyetleri vardır. Blob depolama alanından verileri verildiğinde standart çıkış ücretleri uygulanabilir. İşlem maliyetleri hakkında daha fazla bilgi için bkz: [veri aktarımı fiyatlandırmasını.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="how-to-import-data-into-azure-file-storage-using-internal-sata-hdds-and-ssds"></a>İç SATA HDD ve Ssd'leri kullanarak Azure dosya depolama alanına veri aktarmak nasıl?
İzleyin diskteki verilerin Azure dosya depolama alanına aktarılması ise adımları aşağıda.
Azure içeri/dışarı aktarma hizmetini kullanarak veri aktarırken ilk WAImportExport aracını kullanarak sürücülerinizin hazırlamak için adımdır. Sürücülerinizin hazırlamak için aşağıdaki adımları izleyin.

1. Azure dosya depolama alanına aktarılması verileri tanımlamak. Bu, dizinler ve tek başına dosya yerel sunucuda veya bir ağ paylaşımı olabilir.  
2. Toplam veri boyutuna bağlı olarak gerekir sürücü sayısını belirler. 2,5 inç SSD veya 2,5" ya da 3,5" gereken sayıda SATA II veya III sabit disk sürücüsü temin edin.
4. Dizinler ve/veya her sabit disk sürücüsüne kopyalanacak bağımsız dosyaları belirler.
5. Veri kümesi ve driveset için CSV dosyaları oluşturun.
    
  Azure dosyaları olarak verileri içeri aktarma bir örnek veri kümesi CSV dosyası örneği aşağıdadır:
  
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","fileshare/100M_1.csv.txt",file,rename,"None",None
    "F:\50M_original\","fileshare/",file,rename,"None",None 
    ```
   Yukarıdaki örnekte, "dosya" köke 100M_1.csv.txt kopyalanacak. "Dosya" mevcut değilse tane oluşturulur. Tüm dosya ve klasörlerin 50M_original altında faydalanılması için kopyalanan yinelemeli olacaktır. Klasör yapısı korunur.

    Daha fazla bilgi edinmek [veri kümesi CSV dosyası hazırlama](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    


    **Driveset CSV dosyası**

    Driveset bayrağı sürücü harflerini hazırlıklı olmak için doğru disklerin listesini almak aracı için sırasıyla eşlenmiş disklerin listesini içeren bir CSV dosyası değerdir. 

    Aşağıda driveset CSV dosyası örneği verilmiştir:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    Yukarıdaki örnekte, iki diskleri bağlı ve temel NTFS birimleri birim harfi G:\ ve H:\ ile oluşturulan varsayılır. Aracı biçimlendirmek ve H:\ barındıran değil biçimlendirmek ve birim G:\ barındırma diski şifrelemek disk şifreleyebilirsiniz.

    Daha fazla bilgi edinmek [driveset CSV dosyasını hazırlama](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Kullanım [WAImportExport aracı](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) verilerinizi bir veya daha fazla sabit diskleri kopyalama.
7.  "Şifrele" drivset sabit disk sürücüsünde BitLocker şifrelemesini etkinleştirmek için CSV şifreleme alanında belirtebilirsiniz. Alternatif olarak, de el ile sabit disk sürücüsünde BitLocker şifrelemesini etkinleştirmek "AlreadyEncrypted" belirtin ve Aracı çalıştırırken CSV driveset anahtarı sağlayın.

8. Sabit disk sürücülerini veya günlük dosyası verilerini disk hazırlığı tamamladıktan sonra değişiklik yapmayın.

> [!IMPORTANT]
> Hazırlık her sabit disk sürücüsü bir günlük dosyasında neden olur. Azure Portalı'nı kullanarak içeri aktarma işi oluştururken bu içeri aktarma işinin bir parçası olan sürücüleri tüm günlük dosyalarını yüklemeniz gerekir. Günlük dosyaları işlenmeyecek sürücüleri.
> 
>

Aşağıda komutları ve WAImportExport aracını kullanarak sabit disk sürücüsü hazırlamaya yönelik örnekler verilmektedir.

WAImportExport aracı PrepImport komutu dizinler ve/veya yeni bir kopya oturum dosyaları kopyalamak ilk kopya oturumu için:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**İçeri aktarma örnek 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Aşağıdakileri yapmak için **daha fazla sürücü ekleme**, bir yeni bir driveset dosyası oluşturun ve komutu aşağıdaki gibi çalıştırın. Sonraki kopyalama oturumları belirtilenden farklı disk sürücülerine InitialDriveset .csv dosyanızda için yeni bir driveset CSV dosyası belirtebilir ve değer "AdditionalDriveSet" parametresi olarak sağlayabilirsiniz. Kullanım **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**. AdditionalDriveset CSV dosyasının biçimini InitialDriveSet biçimi olarak aynıdır.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Örnek 2 alma**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

Aynı driveset ek veri eklemek için ek dosyalar/dizine kopyalamak sonraki kopyalama oturumlarına WAImportExport aracı PrepImport komutu çağrılabilir: aynı sabit disk sürücülerine InitialDriveset .csv dosyasında belirtilen sonraki kopyalama oturumları için belirtme **aynı günlük dosyası** adlandırın ve sağlayan bir **yeni oturum kimliği**; depolama hesabı anahtarı sağlamak için gerek yoktur.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Örnek 3 alma**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

WAImportExport aracını kullanma hakkında daha fazla ayrıntı görmek [sabit sürücüler alma işlemi için hazırlanıyor](storage-import-export-tool-preparing-hard-drives-import.md).

Ayrıca, başvurmak [sabit sürücüler içeri aktarma işi için hazırlamak için örnek iş akışı](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) daha ayrıntılı adım adım yönergeler için.  



## <a name="create-an-export-job"></a>Bir dışarı aktarma işi oluşturma
Veri depolama hesabınızdan ve sürücüleri için dışa aktarılan sonra size gönderilen böylece, veri merkezi için bir veya daha fazla boş sürücüleri sevkiyat, içeri/dışarı aktarma hizmeti bildirmek üzere bir dışarı aktarma işinin oluşturun.

### <a name="prepare-your-drives"></a>Sürücülerinizin hazırlama
Aşağıdaki ön denetimleri sürücülerinizin dışa aktarma işi için hazırlamak için önerilir:

1. WAImportExport aracın PreviewExport komutunu kullanarak gerekli disk sayısını denetleyin. Daha fazla bilgi için bkz: [bir dışarı aktarma işi için sürücü kullanımı Önizleme](https://msdn.microsoft.com/library/azure/dn722414.aspx). Seçtiğiniz BLOB tabanlı için kullanacağınız sürücüleri boyutuna sürücü kullanımı Önizleme yardımcı olur.
2. Okuma/dışa aktarma işi için gönderilen sabit sürücü yazabilir olduğunu denetleyin.

### <a name="create-the-export-job"></a>Dışa aktarma işi oluşturma
1. Dışarı aktarma işini oluşturmak için gidin depolama daha fazla hizmet -> Azure portalındaki "içeri/dışarı aktarma işleri" ->. Tıklatın **oluşturma içeri/dışarı aktarma işi**.
2. Adım 1 temel bilgileri, "Azure dışarı aktarma" seçin, iş adı bir dize girin, bir abonelik seçin, girin veya bir kaynak grubu seçin. İçe aktarma işi için açıklayıcı bir ad girin. Girdiğiniz ad içerebilir yalnızca küçük harfler, sayılar, tire ve alt çizgi, bir harf ile başlamalı ve boşluk içeremez unutmayın. Devam eden ve bunlar tamamlandığında çalışırken işlerinizi izlemek için seçtiğiniz ad kullanır. Bu dışarı aktarma işi için sorumlu kişi için kişi bilgilerini sağlayın. 

3. Adım 2 iş ayrıntıları, veri depolama hesabı bölümünde verilecek depolama hesabı seçin. Teslim konumu otomatik olarak olması doldurulur seçilen depolama hesabı bölgeyi temel alan. Depolama hesabınızdan boş sürücü veya sürücüler için vermek istediğiniz hangi blob verileri belirtin. Depolama hesabındaki tüm blob verilerini dışarı aktarmak seçebilirsiniz veya BLOB veya dışarı aktarmak için BLOB'ları ayarlar belirtebilirsiniz.
   
   Dışarı aktarmak için bir blob belirtmek için kullanın **eşit** Seçicisi ve blob kapsayıcı adı ile başlayan, göreli yolunu belirtin. Kullanım *$root* kök kapsayıcısı belirtin.
   
   Bir önek ile başlayan tüm BLOB'lar belirtmek için kullanın **ile başlar** Seçicisi ve eğik çizgiyle başlayan öneki belirtin '/'. Önek kapsayıcı adı, tam kapsayıcı adı veya blob adı öneki tarafından izlenen tüm kapsayıcı adı öneki olabilir.
   
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
   
   Bu ekran görüntüsünde gösterildiği gibi işleme sırasında hataları önlemek için geçerli biçimler blob yollarında sağlamanız gerekir.
   
   ![Adım 3 dışa aktarma işi - oluşturma](./media/storage-import-export-service/export-job-03.png)

4. Adım 3 bilgisi sevkiyat geri dönün, taşıyıcı aşağı açılan listeden seçin ve o taşıyıcı ile oluşturduğunuz bir geçerli taşıyıcı hesap numarası girin. Microsoft, içe aktarma işi tamamlandıktan sonra geri için sürücüleri sevk etmek için bu hesabı kullanır. Bir tam ve geçerli kişi adı, telefon, e-posta, adres, şehir, ZIP, durumu/proviince ve ülke/bölge girin.
   
 5. Özet sayfasında, Azure DC disklere aktarma için kullanılacak Azure veri merkezinde sevkiyat adresi sağlanır. İş adı ve tam adresini sevkiyat etikette açıklanan emin olun. 

6. İçeri aktarma işi oluşturma işlemini tamamlamak için Özet sayfasında Tamam'ı tıklatın

7. Diskleri sevkiyat sonra geri dönüp **içeri/dışarı aktarma** sayfa Azure portalında bir) gidin ve içeri aktarma işi tıklatıldığında b) tıklayın **güncelleştirme iş durumu ve sürücüleri sevk sonra izleme bilgilerinin**. 
     c) seçin "sevk edilmiş olarak işaretle" onay kutusunu d) taşıyıcı ve izleme numarasını belirtin.
    
   İzleme numarası iş oluşturma 2 hafta içinde güncelleştirilmezse, işin süresi dolar.
   
8. Portal panosunda, işin ilerleme durumunu izleyebilirsiniz. Önceki bölümdeki her iş durumu tarafından anlamı bkz [işi durumunu görüntüleme](#viewing-your-job-status).

   > [!NOTE]
   > Dışa aktarılacak blobun sabit sürücüye kopyalama zamanında kullanılıyorsa, Azure içeri/dışarı aktarma hizmeti BLOB anlık görüntü alın ve anlık görüntü kopyalayın.
   > 
   > 
 
9. Sürücüleri, dışarı aktarılan verilerle aldıktan sonra görüntülemek ve sürücünüzün hizmeti tarafından oluşturulan BitLocker anahtarları kopyalayın. Azure portalında iş dışarı ve içeri/dışarı aktarma sekmesini gidin. Listeden, dışa aktarma işi seçin ve BitLocker anahtarları seçeneğini tıklatın. BitLocker anahtarları, aşağıda gösterildiği gibi görünür:
   
   ![BitLocker anahtarları dışa aktarma işi için görüntüleme](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Bu hizmet kullanırken müşterilerin karşılaştığı en yaygın soruları kapsar Lütfen aracılığıyla aşağıdaki SSS bölümüne gidin.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Azure içeri/dışarı aktarma hizmetini kullanarak Azure File storage kopyalayabilir miyim?**

Evet, Azure içeri/dışarı aktarma hizmeti Azure dosya Storge alınacak destekler. Azure dosyaları dışarı aktarma şu anda desteklemiyor.

**Azure içeri/dışarı aktarma hizmeti için CSP abonelikler var mı?**

Azure içeri/dışarı aktarma hizmeti CSP abonelikleri desteklemiyor.

**İçe aktarma işi için sürücü hazırlık adımı atlayabilirsiniz veya kopyalamadan t bir sürücü hazırlayabilirsiniz?**

Azure WAImportExport aracını kullanarak verileri almak için göndermek istediğiniz herhangi bir sürücüye hazırlanması gerekir. Verileri diske kopyalamak için WAImportExport aracını kullanmanız gerekir.

**Dışarı aktarma işini oluştururken herhangi disk hazırlığı gerçekleştirmek gerekiyor mu?**

Yoktur, ancak bazı ön denetimleri önerilir. WAImportExport aracın PreviewExport komutunu kullanarak gerekli disk sayısını denetleyin. Daha fazla bilgi için bkz: [bir dışarı aktarma işi için sürücü kullanımı Önizleme](https://msdn.microsoft.com/library/azure/dn722414.aspx). Seçtiğiniz BLOB tabanlı için kullanacağınız sürücüleri boyutuna sürücü kullanımı Önizleme yardımcı olur. Ayrıca okuma ve dışa aktarma işi için gönderilen sabit sürücü yazma denetleyin.

**I yanlışlıkla uymuyor bir HDD desteklenen gereksinimleri göndermesi durumunda ne olur?**

Azure veri merkezine uymuyor sürücü, desteklenen gereksinimlerine dönecektir. Yalnızca bazı paketindeki sürücülerden biri destek gereksinimlerini bu sürücüleri işlenir ve gereksinimlerini karşılamayan sürücüler için döndürülür.

**İşim iptal edebilir miyim?**

Durumunu oluştururken veya sevkiyat bir işi iptal edebilirsiniz.

**Azure portalında tamamlanmış işlerin durumunu ne kadar süreyle görüntüleyebilirim?**

90 güne kadar tamamlanmış işlerin durumunu görüntüleyebilirsiniz. Tamamlanan İşler 90 gün sonra silinir.

**İçeri veya dışarı aktarma 10'dan fazla sürücüler isterseniz ne yapmalıyım?**

Bir içeri aktarma veya dışarı aktarma işini içeri/dışarı aktarma hizmeti için tek bir işi yalnızca 10 sürücüleri başvuru. 10'dan fazla sürücüleri sevk etmek istiyorsanız, birden çok işleri oluşturabilirsiniz. Aynı işle ilişkili sürücüleri aynı pakette birlikte sevk gerekir.
Microsoft, işler Kılavuzu ve veri kapasitesi birden çok disk yayıldığında Yardım alma sunar. Temasa bulkimport@microsoft.com daha fazla bilgi için

**Hizmet, bunları dönmeden önce sürücüleri biçimlendirmeniz mu?**

Hayır. Tüm sürücüler BitLocker ile şifrelenmiş.

**Sürücüleri içeri/dışarı aktarma işleri için Microsoft'tan satın alabilir?**

Hayır. Her iki içeri aktarma için kendi sürücüleri sevk ve işleri dışarı aktarma gerekecektir.

** Bu hizmet ** tarafından alınan veri nasıl erişebilir mi

Verileri Azure depolama hesabınızın altında Azure portalı üzerinden erişilebilir veya ayrı bir araç kullanarak Depolama Gezgini çağrılır. https://docs.microsoft.com/Azure/VS-Azure-Tools-Storage-Manage-With-Storage-Explorer 

**İçeri aktarma işi tamamlandıktan sonra ne depolama hesabında my veri görünümü ister? Belgelerim dizini hiyerarşi korunur?**

Bir sabit sürücü içeri aktarma işi için hazırlık yaparken, hedef CSV kümesindeki DstBlobPathOrPrefix alan belirtilir. Bu sabit sürücüsünden veri kopyalanır depolama hesabındaki hedef kapsayıcıdır. Bu hedef kapsayıcı içindeki sabit sürücüden klasörler için oluşturulan sanal dizinleri ve blobları dosyaları için oluşturulur. 

**Sürücü depolama Hesabımı zaten mevcut dosyaların varsa, hizmet mevcut BLOB'ları veya depolama Hesabımı dosyaları üzerine yazar?**

Sürücü hazırlanırken hedef dosyaların üzerine ya da yoksayılan alanını dataset CSV dosyası kullanarak adlı değerlendirme belirtebilirsiniz: < yeniden adlandırma | Hayır üzerine | üzerine >. Varsayılan olarak, hizmet yeni dosyaları yeniden adlandırmak yerine mevcut BLOB veya dosyaların üzerine.

**WAImportExport aracının 32-bit işletim sistemleriyle uyumlu mu?**
Hayır. WAImportExport aracı yalnızca 64-bit Windows işletim sistemleriyle uyumlu değil. İşletim sistemleri bölümünde lütfen [ön koşullar](#pre-requisites) desteklenen işletim sistemi sürümleri tam bir listesi.

**My paketinde sabit disk sürücüsü dışında her şey eklemeliyim?**

Lütfen yalnızca sabit sürücülerinizin birlikte. Güç kaynağı kabloları veya USB kablosu gibi öğeleri dahil etmeyin.

**FedEx veya DHL kullanarak my sürücüleri sevk var mı?**

Sürücüleri FedEx, DHL, UPS veya ABD posta hizmeti gibi bilinen bir taşıyıcı kullanarak veri merkezine gönderebilirsiniz. Ancak, veri merkezi sizin sürücülerden sevkiyat için ABD ve AB FedEx hesap numarası veya Asya ve Avustralya'da bölgelerde DHL hesap numarası sağlamanız gerekir.

**Sürücümün uluslararası sevkiyat ile herhangi bir kısıtlama var mı?**

Lütfen sevkiyat fiziksel ortam uluslararası sınırların arası gerekebilir unutmayın. Fiziksel ortam ve veri içe aktarılan ve/veya ilgili yasalarına uygun olarak dışarı olduğunu sağlamaktan sorumludur. Fiziksel medya göndermeden önce medya ve verilerinizi yasal olarak tanımlanan veri merkezine gönderilebilir olduğunu doğrulamak için danışmanlar danışın. Bu, Microsoft zamanında ulaştığında emin olmaya yardımcı olur.

**Bir işi oluştururken, teslimat adresi my depolama hesabı konumdan farklı bir konumdur. Ne yapmalıyım?**

Bazı depolama hesabı konumları alternatif sevkiyat konumlara eşlenir. Daha önce kullanılabilir sevkiyat konumlarını da geçici olarak alternatif konumlara eşlenebilir. Her zaman sürücülerinizin teslim etmeden önce işi oluşturma sırasında sağlanan sevkiyat adresi denetleyin.

**Sürücümün sevkiyat, taşıyıcı veri merkezi iletişim adresi ve telefon numarası için sorar. Ne ı sağlamanız gerekir?**

Telefon numarası ve DC adresi iş oluşturmanın bir parçası sağlanır.

**O365'e PST posta kutuları ve SharePoint verilerini kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?**

Lütfen [alma PST dosyaları veya Office 365 SharePoint verileri](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Azure yedekleme hizmeti yedeklerim çevrimdışı kopyalamak için Azure içeri/dışarı aktarma hizmeti kullanabilir miyim?**

Lütfen [Çevrimdışı Yedekleme iş akışı Azure Yedekleme'de](../../backup/backup-azure-backup-import-export.md).

**HDD için en fazla sayıda bir sevkiyat içinde nedir?**

Herhangi bir sayıda HDD bir sevkiyat olabilir ve diskleri için birden çok iş aitse önerilir bir) karşılık gelen iş adlarıyla etiketli diske sahip. b) -1 ile sonekine izleme numarasıyla işleri güncelleştirme-2 vb.
  
**En fazla blok blobu ve sayfa Blob Disk içeri/dışarı aktarma tarafından desteklenen boyutu nedir?**

En fazla blok blobu boyutudur yaklaşık 4.768TB veya 5,000,000 MB.
En fazla sayfa Blob boyutu 1 TB'tır.

**Disk içeri/dışarı aktarma AES 256 şifrelenmesini destekliyor mu?**

AES 128 bitlocker şifrelemesi ile Azure içeri/dışarı aktarma hizmeti varsayılan olarak şifreler, ancak AES 256 Bu artırılabilir veri kopyalamadan önce el ile bitlocker ile şifreleyerek. 

Kullanıyorsanız [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), aşağıda bir örnek komut verilmiştir
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Kullanıyorsanız [WAImportExport aracı](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) "AlreadyEncrypted" belirtin ve CSV driveset anahtarı sağlayın.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport Aracı'nı ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure alma verme REST API örnek](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

