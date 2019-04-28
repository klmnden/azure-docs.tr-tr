---
title: Azure içeri/dışarı aktarma için ve Azure Depolama'dan veri aktarımı kullanarak | Microsoft Docs
description: İçeri aktarma oluşturma ve dışarı aktarma işleri için ve Azure Depolama'dan veri aktarma için Azure portalında öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 02/14/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 4850dd82ca52a060c921569433035256f5b74cce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478796"
---
# <a name="what-is-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti nedir?

Azure içeri/dışarı aktarma hizmeti, güvenli bir şekilde büyük miktarda veriyi Azure Blob Depolama ve Azure dosyaları için bir Azure veri merkezine bir sürücü sevkiyat tarafından içeri aktarmak için kullanılır. Bu hizmet, verileri Azure Blob depolama alanından disk sürücülerine aktarmak ve şirket içi sitelerinize teslim etme için de kullanılabilir. Bir veya daha fazla disk sürücüsü verileri Azure Blob Depolama veya Azure dosyaları içeri aktarılabilir. 

Kendi disk sürücüleri sağlayın ve Azure içeri/dışarı aktarma hizmeti ile veri aktarma. Microsoft tarafından sağlanan disk sürücüler de kullanabilirsiniz. 

Microsoft tarafından sağlanan disk sürücüleri kullanarak veri aktarmak istiyorsanız, kullanabileceğiniz [Azure Data Box Disk](../../databox/data-box-disk-overview.md) verileri Azure'a aktarmak için. Microsoft, 40 TB toplam kapasiteye bölgesel bir taşıyıcı aracılığıyla veri merkeziniz için bir sipariş başına en fazla 5 şifrelenmiş katı hal disk sürücüleri (SSD'ler) gelir. Hızlı disk sürücülerini yapılandırır, bir USB 3.0 bağlantısı üzerinden disk sürücülerine veri kopyalama ve disk sürücüleri azure'a geri gönderin. Daha fazla bilgi için Git [Azure Data Box Disk genel bakış](../../databox/data-box-disk-overview.md).

## <a name="azure-importexport-usecases"></a>Azure içeri/dışarı aktarma usecases

Azure içeri/dışarı aktarma hizmeti veri yükleme veya ağ üzerinden indirme çok yavaş olduğunda veya ek ağ bant genişliği alma maliyetle kullanmayı düşünün. Bu hizmet aşağıdaki senaryolarda kullanın:

* **Bulutta veri taşıma**: Büyük miktarlarda verinin hızla Azure'a taşıyın ve hesaplı.
* **İçerik dağıtım**: Hızlı bir şekilde veri müşteri sitelerinize gönderin.
* **Yedekleme**: Azure depolamada depolamak için şirket içi verilerinizin yedeklerini uygulayın.
* **Veri Kurtarma**: Büyük Depolama'da depolanan veriler miktarda kurtarın ve şirket içi konumunuza teslim edilmesini.

## <a name="importexport-components"></a>İçeri/dışarı aktarma bileşenleri

İçeri/dışarı aktarma hizmeti, aşağıdaki bileşenleri kullanır:

- **İçeri/dışarı aktarma hizmeti**: Azure portalında kullanılabilen bu hizmet, kullanıcı oluşturma ve veri alma (yükleme) izlemek ve (indirme) işleri dışarı aktar yardımcı olur.  

- **WAImportExport aracı**: Bu aşağıdakileri yapan bir komut satırı aracıdır: 
    - Disk sürücülerinizden gönderilir ve içeri aktarma için hazırlar.
    - Sürücüye veri kopyalama işlemini kolaylaştırır.
    - Bir sürücüde BitLocker ile verileri şifreler.
    - İçeri aktarma oluşturma sırasında kullanılan sürücü günlük dosyalarını oluşturur.
    - Dışarı aktarma işleri için gerekli sürücüler sayıda tanımlamanıza yardımcı olacak.
    
> [!NOTE]
> İki sürümleri, sürüm 1 ve 2 WAImportExport aracı kullanılabilir. Kullanmanızı öneririz:
> - Azure Blob Depolama içine 1 sürümü'içeri / dışarı aktarma için. 
> - Azure dosyalarını içe veri aktarma için 2 sürümü.
>
> WAImportExport aracın yalnızca 64 bit Windows işletim sistemi ile uyumludur. Desteklenen belirli işletim sistemi sürümleri için Git [Azure içeri/dışarı aktarma gereksinimleri](storage-import-export-requirements.md#supported-operating-systems).

- **Disk sürücüleri**: Katı hal sürücüleri (SSD'ler) veya bir Azure veri merkezine sabit disk sürücülerinin (HDD'ler) sevk edebilir. İçeri aktarma işi oluşturma, disk, verilerini içeren sürücüler gönderin. Dışarı aktarma işi oluşturma, Azure veri merkezine boş sürücüleri gönderin. Belirli disk türleri için Git [desteklenen disk türleri](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>İçeri/dışarı aktarma nasıl çalışır?

Azure içeri/dışarı aktarma hizmeti işleri oluşturarak Azure Blobları ve Azure dosyaları ile veri aktarımı sağlar. İşleri oluşturmak için Azure portal veya Azure Resource Manager REST API'si kullanın. Her bir iş, tek bir depolama hesabıyla ilişkilidir. 

İşleri, içeri aktarma olabilir veya dışarı aktarma işleri. İçeri aktarma işi dışarı aktarma işi verileri Azure Bloblarından dışarı aktarılmasına izin verir ancak verileri Azure BLOB'ları veya Azure dosyaları almak sağlar. İçeri aktarma işi için verilerinizi içeren sürücüler gönderin. Dışarı aktarma işi oluşturduğunuzda, bir Azure veri merkezine boş sürücüleri gönderin. Her durumda, iş başına en fazla 10 sürücü gönderebilirsiniz.

### <a name="inside-an-import-job"></a>İçinde içeri aktarma işi

İçeri aktarma işi, yüksek düzeyde, aşağıdaki adımları içerir:

1. Alınacak ihtiyacınız sürücü sayısı hedef blob konum verilerinizi Azure depolama için verileri belirler.
2. Disk sürücülerine veri kopyalamak için WAImportExport Aracı'nı kullanın. Disk sürücüleri BitLocker ile şifreleyin.
3. İçeri aktarma işi, hedef depolama hesabında Azure Portalı'nda oluşturun. Sürücü günlük dosyalarını karşıya yükleyin.
4. Taşıyıcı hesap numarası ve dönüş adresi sürücüler size geri sevkiyat için sağlar.
5. Disk sürücülerini işi oluşturma işlemi sırasında sağlanan sevkiyat adresine gönderin.
6. Teslimat izleme numarası içeri aktarma işi ayrıntıları güncelleştirin ve içeri aktarma işi Gönder.
7. Sürücüleri aldı ve Azure veri merkezinde işlenir.
8. Sürücüleri içeri aktarma işinin sağlanan dönüş adresi taşıyıcı hesabınızı kullanarak aktarılır.

> [!NOTE]
> Yerel (içinde veri merkezi ülke) sevk irsaliyesi için lütfen bir yurt dışı taşıyıcı hesap paylaşın 
>
> (Ülke dışında bir veri merkezi) abroad sevk irsaliyesi için lütfen bir uluslararası taşıyıcı hesap paylaşın

 ![Şekil 1:Import iş akışı](./media/storage-import-export-service/importjob.png)

Adım adım yönergeler veri alma, gidin:

- [Azure Bloblarınızdan veri alma](storage-import-export-data-to-blobs.md)
- [Azure dosyaları ile verileri içeri aktar](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>İçinde dışarı aktarma işi

> [!IMPORTANT]
> Hizmet, yalnızca Azure BLOB'ları dışarı aktarmayı destekler. Azure dosyaları dışarı aktarma desteklenmiyor.

Dışarı aktarma işi, yüksek düzeyde, aşağıdaki adımları içerir:

1. Dışa aktarılacak veri belirlemek için size gereksinimi, kaynak BLOB veya kapsayıcı yollarını verilerinizin Blob Depolama alanında sürücüsü sayısı.
3. Dışarı aktarma işi, Azure portalında kaynak depolama hesabınızdaki oluşturun.
4. Kaynak BLOB veya kapsayıcı yolları verilerin dışarı belirtin.
5. Taşıyıcı hesap numarası ve dönüş adresi sürücüler size geri sevkiyat için sağlar.
6. Disk sürücülerini işi oluşturma işlemi sırasında sağlanan sevkiyat adresine gönderin.
7. Teslimat izleme numarası dışarı aktarma işi ayrıntıları güncelleştirin ve dışarı aktarma işi gönderin.
8. Sürücüleri aldı ve Azure veri merkezinde işlenir.
9. Sürücüler BitLocker ile şifrelenir ve anahtarları, Azure portalı üzerinden kullanılabilir.  
10. Sürücüleri içeri aktarma işinin sağlanan dönüş adresi taşıyıcı hesabınızı kullanarak aktarılır.

> [!NOTE]
> Yerel (içinde veri merkezi ülke) sevk irsaliyesi için lütfen bir yurt dışı taşıyıcı hesap paylaşın 
>
> (Ülke dışında bir veri merkezi) abroad sevk irsaliyesi için lütfen bir uluslararası taşıyıcı hesap paylaşın
  
 ![Şekil 2:Export iş akışı](./media/storage-import-export-service/exportjob.png)

Verileri dışarı aktarma ile ilgili adım adım yönergeler için Git [Azure Bloblarından veri dışarı aktarma](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Bölge kullanılabilirliği 

Azure içeri/dışarı aktarma hizmeti, için ve tüm Azure depolama hesaplarından veri kopyalamayı destekler. Disk sürücüleri listelenen konumlardan birine gönderebilirsiniz. Depolama hesabınızı burada belirtilmeyen bir Azure konumunda ise, işi oluşturduğunuzda bir alternatif sevkiyat konumu sağlanır.

### <a name="supported-shipping-locations"></a>Desteklenen sevkiyat konumları


|Ülke  |Ülke  |Ülke  |Ülke  |
|---------|---------|---------|---------|
|Doğu ABD    | Kuzey Avrupa        | Orta Hindistan        |US Gov Iowa         |
|Batı ABD     |Batı Avrupa         | Güney Hindistan        | US DoD Doğu        |
|Doğu ABD 2    | Doğu Asya        |  Batı Hindistan        | US DoD Orta        |
|Batı ABD 2     | Güneydoğu Asya        | Orta Kanada        | Çin Doğu         |
|Orta ABD     | Avustralya Doğu        | Doğu Kanada        | Çin Kuzey        |
|Orta Kuzey ABD     |  Avustralya Güneydoğu       | Güney Brezilya        | Birleşik Krallık Güney        |
|Orta Güney ABD     | Japonya Batı        |Kore Orta         | Almanya Orta        |
|Batı Orta ABD     |  Japonya Doğu       | ABD Devleti Virginia        | Almanya Kuzeydoğu        |


## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Veri sürücüsünde BitLocker Sürücü Şifrelemesi ile şifrelenir. Bu şifreleme, aktarım sırasında verilerinizi korur.

İçeri aktarma işlerinde için sürücüleri iki yolla şifrelenir.  


- Seçeneğini kullanırken belirtin *dataset.csv* sürücü hazırlanırken WAImportExport aracı çalıştırılırken dosya. 

- El ile sürücüde BitLocker şifrelemesini sağlar. Şifreleme anahtarı belirtin *driveset.csv* sürücü hazırlanırken WAImportExport aracı komut satırı çalışırken.


Verilerinizi bu sürücülere kopyalandıktan sonra dışarı aktarma işleri için hizmet, size geri göndermeden önce BitLocker'ı kullanarak sürücüyü şifreler. Şifreleme anahtarı için Azure portalı üzerinden sağlanır.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Fiyatlandırma

**İşleme ücreti sürücü**

İçeri aktarma işleminizin bir parçası olarak işlenen her sürücü için sürücü işleme ücret ödemeniz gerekmez veya dışarı aktarma işi. İlgili ayrıntılara [Azure içeri/dışarı aktarma fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Sevkiyat maliyetleri**

Diskleri Azure'a gönderin, Sevkiyat taşıyıcısı için sevkiyat ücreti ödersiniz. Microsoft sürücüler size geri döndüğünde, sevkiyat ücreti, proje oluşturma sırasında sağladığınız taşıyıcı hesap için ücretlendirilir.

**İşlem maliyetleri**

Azure Depolama'ya veriler içeri aktarılırken standart depolama işlem maliyetleri yanı sıra işlem ücreti alınmaz vardır. Blob depolama alanından verileri dışarı aktarılırken standart çıkış ücretleri uygulanabilir. İşlem maliyetleri hakkında daha fazla bilgi için bkz. [veri aktarımı fiyatlandırması.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Sonraki adımlar

İçeri/dışarı aktarma hizmeti için kullanmayı öğrenin:
* [Azure BLOB'ları için verileri içeri aktar](storage-import-export-data-to-blobs.md)
* [Azure Bloblarından veri dışarı aktarma](storage-import-export-data-from-blobs.md)
* [Azure dosyaları için verileri içeri aktar](storage-import-export-data-to-files.md)

