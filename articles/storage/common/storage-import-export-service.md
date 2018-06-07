---
title: Azure Storage gelen ve giden veri aktarımı için Azure içeri/dışarı aktarma kullanma | Microsoft Docs
description: Alma oluşturma ve işleri Azure portalında Azure Storage veri aktarma için dışarı aktarma hakkında bilgi edinin.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 83ba437e699eb150e86e6c89e478377394966419
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809551"
---
# <a name="what-is-azure-importexport-service"></a>Azure içeri/dışarı aktarma hizmeti nedir?

Azure içeri/dışarı aktarma hizmeti, güvenli bir şekilde büyük miktarlarda verinin Azure Blob Depolama ve Azure dosyaları için bir Azure veri merkezi disk sürücülerine sevkiyat tarafından içeri aktarmak için kullanılır. Bu hizmet, verileri Azure Blob depolama alanından disk sürücülerine aktarmak ve şirket içi siteleriniz sevk etmek için de kullanılabilir. Bir veya daha fazla disk verileri Azure Blob storage veya Azure dosyaları içeri aktarılabilir. 

## <a name="azure-importexport-usecases"></a>Azure içeri/dışarı aktarma usecases

Karşıya yükleme veya ağ üzerinden veri indirme çok yavaş olduğunda veya ek ağ bant genişliği alma maliyetli Azure içeri/dışarı aktarma hizmeti kullanmayı düşünün. Bu hizmet aşağıdaki senaryolarda kullanın:

* **Veri geçişi buluta**: büyük miktarlarda verinin hızla Azure'a taşımak ve düşük maliyetle.
* **İçerik dağıtım**: hızla müşteri sitelerinize veri gönderme.
* **Yedekleme**: Azure Storage'da depolamak için şirket içi verilerinizin yedekleri alın.
* **Veri Kurtarma**: kurtarmak büyük miktarda veri depolama alanına depolanır ve şirket içi konumunuz teslim sahip.

## <a name="importexport-components"></a>Bileşenleri içeri/dışarı aktarma

İçeri/dışarı aktarma hizmeti aşağıdaki bileşenleri kullanır:

- **İçeri/dışarı aktarma**hizmet: Bu hizmet Azure portalında kullanılabilir oluşturmak ve içeri aktarma izlemek ve işler dışarı kullanıcı yardımcı olur.  

- **WAImportExport aracı**: Bu aşağıdaki yapan bir komut satırı aracıdır: 
    - Sağlanan sürücülerinizin alma için hazırlar.
    - Verilerinizi diske kopyalama kolaylaştırır.
    - BitLocker'ı içeren sürücüde verileri şifreler.
    - İçeri aktarma oluşturma sırasında kullanılan sürücü günlük dosyaları oluşturur.
    - Dışarı aktarma işleri için gerekli sürücüleri sayıda tanımlamanıza yardımcı olacak.

    Bu araç, iki sürümleri, sürüm 1 ve 2 kullanılabilir. Kullanmanızı öneririz:

    - Sürüm 1 içeri/dışarı aktarma için Azure Blob depolama alanına. 
    - Azure dosyaları içe veri aktarma sürüm 2.

    WAImportExport aracı yalnızca 64-bit Windows işletim sistemiyle uyumlu değil. Desteklenen belirli işletim sistemi sürümleri için Git [Azure içeri/dışarı aktarma gereksinimleri](storage-import-export-requirements.md#supported-operating-systems).

- **Diskleri**: katı hal sürücüleri (SSD) sevk edebilir veya sabit disk sürücüleri (HDD) için Azure veri merkezi. İçe aktarma işi oluştururken, disk, verilerini içeren sürücüler birlikte. Dışarı aktarma işini oluştururken, Azure veri merkezine boş sürücüleri birlikte. Belirli bir disk türleri için Git [desteklenen disk türleri](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>İçeri/dışarı aktarma nasıl çalışır?

Azure içeri/dışarı aktarma hizmeti işleri oluşturarak Azure BLOB'ları ve Azure dosyaları halinde veri aktarımı sağlar. İşleri oluşturmak için Azure portal veya Azure Resource Manager REST API kullanın. Her bir iş, tek bir depolama hesabıyla ilişkilendirilir. 

İşlerini işler dışarı veya içeri aktarma olabilir. İçe aktarma işi dışarı aktarma işinin verilerin Azure Bloblarından dışarı aktarılmasına olanak tanır ancak verileri Azure BLOB'ları veya Azure dosyaları almak sağlar. Bir içeri aktarma işi için verilerinizi içeren sürücüler birlikte. Bir dışarı aktarma işinin oluşturduğunuzda, Azure veri merkezinde boş sürücülere birlikte. Her durumda, iş başına en fazla 10 disk sürücüleri gönderebilirsiniz.

> [!IMPORTANT]
> Verileri Azure dosyasına dışarı aktarma desteklenmiyor.

Bu bölümde yüksek düzey adımları söz konusu içeri aktarma ve dışarı aktarma işleri açıklanmıştır. 


### <a name="inside-an-import-job"></a>İçe aktarma işi

Yüksek bir düzeyde bir alma işi aşağıdaki adımları içerir:

1. İçeri aktarılan, verilerin ihtiyacınız sürücü sayısı, verileriniz Azure depolama alanında için hedef blob konum belirleyin.
2. Disk sürücülerine verileri kopyalamak için WAImportExport aracını kullanın. BitLocker ile diskleri şifreleyin.
3. İçe aktarma işi hedef depolama hesabınız Azure portalında oluşturun. Sürücü günlük dosyalarını karşıya yükleyin.
2. Sürücüleri size geri sevkiyat için taşıyıcı hesap numarası ve dönüş adresi sağlayın.
3. Proje oluşturma sırasında sağlanan sevkiyat adresi disk sürücülerine birlikte.
4. İzleme numarası alma işi ayrıntıları teslim güncelleştirin ve içeri aktarma işi gönderin.
5. Sürücüleri alınan ve Azure veri merkezinde işlenebilir.
6. Sürücüleri, içeri aktarma işi sağlanan dönüş adresi taşıyıcı hesabınıza kullanarak aktarılır.
  
    ![Şekil 1:Import iş akışı](./media/storage-import-export-service/importjob.png)

Veri adım adım yönergeler için içeri aktarma, gidin:

- [Azure BLOB verilerini alma](storage-import-export-data-to-blobs.md)
- [Azure dosyalarına veri al](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>İçinde bir dışarı aktarma işinin

> [!IMPORTANT]
> Hizmeti yalnızca Azure BLOB'ları dışarı aktarmayı destekler. Azure dosyaları verilmesini desteklenmiyor.

Yüksek bir düzeyde bir dışarı aktarma işinin aşağıdaki adımları içerir:

1. Dışa aktarılacak veri belirlemek için, gereksinim, kaynak BLOB'ları veya kapsayıcısı yollarının verilerinizi Blob depolama alanına sürücüsü sayısı.
3. Bir dışarı aktarma işinin kaynak depolama hesabınız Azure portalında oluşturun.
4. Kaynak BLOB veya kapsayıcısı yollarının verilerin aktarılması belirtin.
5. Sürücüleri size geri sevkiyat için taşıyıcı hesap numarası ve dönüş adresi sağlayın.
6. Proje oluşturma sırasında sağlanan sevkiyat adresi disk sürücülerine birlikte.
7. İzleme numarası dışa aktarma işi ayrıntıları teslim güncelleştirin ve dışa aktarma işi gönderin.
8. Sürücüleri alınan ve Azure veri merkezinde işlenebilir.
9. BitLocker ile şifrelenmiş sürücüleri ve anahtarları Azure portalı üzerinden kullanılabilir.  
10. Sürücüleri, içeri aktarma işi sağlanan dönüş adresi taşıyıcı hesabınıza kullanarak aktarılır.
  
    ![Şekil 2:Export iş akışı](./media/storage-import-export-service/exportjob.png)

Veri aktarma hakkında adım adım yönergeler için Git [dışarı aktarma veri Azure Bloblarından](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Bölge kullanılabilirliği 

Azure içeri/dışarı aktarma hizmeti için ve tüm Azure depolama hesapları arasından veri kopyalamayı destekler. Disk sürücüleri listelenen konumlardan birine gönderebilirsiniz. Depolama hesabınızı burada belirtilmemiş Azure bir konumdaysa işi oluşturduğunuzda bir alternatif sevkiyat konumu sağlanır.

### <a name="supported-shipping-locations"></a>Sevkiyat konumlar desteklenir


|Ülke  |Ülke  |Ülke  |Ülke  |
|---------|---------|---------|---------|
|Doğu ABD    | Kuzey Avrupa        | Orta Hindistan        |ABD Devleti Iowa         |
|Batı ABD     |Batı Avrupa         | Güney Hindistan        | US DoD Doğu        |
|Doğu ABD 2    | Doğu Asya        |  Batı Hindistan        | US DoD Orta        |
|Batı ABD 2     | Güneydoğu Asya        | Orta Kanada        | Çin Doğu         |
|Orta ABD     | Avustralya Doğu        | Doğu Kanada        | Çin Kuzey        |
|Orta Kuzey ABD     |  Avustralya Güneydoğu       | Güney Brezilya        | Birleşik Krallık Güney        |
|Orta Güney ABD     | Japonya Batı        |Kore Orta         | Almanya Orta        |
|Batı Orta ABD     |  Japonya Doğu       | ABD Devleti Virginia        | Almanya Kuzeydoğu        |


## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Veri sürücüsünde BitLocker Sürücü Şifrelemesi kullanılarak şifrelenir. Bunu esnasında bu şifreleme verilerinizi korur.

İçeri aktarma işi için iki yolla sürücüleri şifrelenir.  


- Seçeneğini kullanırken belirtin *dataset.csv* sürücü hazırlanması sırasında WAImportExport Aracı'nı çalıştırırken dosya. 

- El ile sürücüde BitLocker şifrelemesini etkinleştirmek. Şifreleme anahtarı belirtmek *driveset.csv* sürücü hazırlanması sırasında WAImportExport aracı komut satırı çalışırken.


Verilerinizi sürücülere kopyalandıktan sonra dışarı aktarma işleri için bunu size geri göndermeden önce BitLocker'ı kullanarak sürücü hizmet şifreler. Şifreleme anahtarı için Azure portalı üzerinden sağlanır.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Fiyatlandırma

**İşleme ücret sürücü**

Alma işleminizin bir parçası olarak işlenen her bir sürücü için bir sürücü işleme ücret yoktur veya iş dışa aktarın. Ayrıntıları görmek [Azure içeri/dışarı aktarma fiyatlandırma](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Sevkiyat maliyetleri**

Diskleri Azure'a sevk ettiğinizde, Sevkiyat taşıyıcı sevkiyat maliyet ücret ödersiniz. Microsoft size sürücüleri döndürdüğünde sevkiyat maliyet işi oluşturma sırasında sağlanan taşıyıcı hesap için ücret kesilir.

**İşlem maliyetleri**

Verileri Azure depolama alanına alırken standart depolama işlem maliyetleri yanı sıra hiçbir işlem maliyetleri vardır. Blob depolama alanından verileri verildiğinde standart çıkış ücretleri uygulanabilir. İşlem maliyetleri hakkında daha fazla bilgi için bkz: [veri aktarımı fiyatlandırmasını.](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Sonraki adımlar

İçeri/dışarı aktarma hizmeti kullanmayı öğrenin:
* [Azure BLOB Veri Al](storage-import-export-data-to-blobs.md)
* [Verileri Azure BLOB'ları dışa](storage-import-export-data-from-blobs.md)
* [Azure dosyaları veri al](storage-import-export-data-to-files.md)

