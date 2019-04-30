---
title: Microsoft Azure veri kutusu ağ geçidi kullanım örnekleri | Microsoft Docs
description: Azure veri kutusu ağ geçidi, verilerinizi Azure'a aktarmanıza imkan sağlayan bir sanal gereç depolama çözümü için kullanım durumlarından açıklar.
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 03/2/2019
ms.author: alkohli
ms.openlocfilehash: 37ec1d05d07f33343b9ff21380a277d00b242b7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754190"
---
# <a name="use-cases-for-azure-data-box-gateway"></a>Azure veri kutusu ağ geçidi için kullanım örnekleri

Azure veri kutusu ağ geçidi, şirket içi bulunan ve Azure'a görüntünüzün, medya ve diğer veri gönderen bir bulut depolama ağ geçidi cihazı var. Bu bulut depolama, Hiper yöneticide sağlanan bir sanal makinenin ağ geçididir. Daha sonra Azure'a gönderir NFS ve SMB protokolleri kullanarak bu sanal cihaza veri yazma. Bu makalede, bu cihaz dağıtabileceğiniz senaryoları için ayrıntılı bir açıklaması sunulmaktadır.

Veri kutusu ağ geçidi aşağıdaki senaryolar için kullanın:

- Büyük miktarda verileri için sürekli olarak alın.
- Bulut için güvenli ve verimli bir şekilde veri arşivleme.
- İlk toplu sonra ağ üzerinden artımlı veri aktarımı için Aktarım işlemlerinden Data Box'ı kullanarak.

Bu senaryoların her biri, sonraki bölümlerde ayrıntılı olarak açıklanmıştır.


## <a name="continuous-data-ingestion"></a>Sürekli veri alımı

Veri kutusu ağ geçidi birincil avantajı sürekli olarak veri boyutundan bağımsız olarak bulut kopyalamak için bir aygıt halinde veri alma becerisini biridir.

Veri ağ geçidi cihazı için yazıldıkça cihaz verileri Azure Depolama'ya yükler. Cihaz, dosyaları yerel olarak belirli bir eşiğe ulaştığında meta verilerini korurken kaldırarak depolama otomatik olarak yönetir. Meta veriler yerel bir kopyasının saklanması dosya güncelleştirildiğinde değişiklikleri yalnızca karşıya yüklemek ağ geçidi cihazı sağlar. Ağ geçidi cihazınıza yüklenen verilere ilişkin yönergeleri olmalıdır [Veril uyarılar](data-box-gateway-limits.md#data-upload-caveats).

Cihaz veriyle dolduğunda, verileri buluta yüklenen oranı eşleştirmek için (gereken şekilde) giriş oranı azaltma başlatır. Cihazda sürekli alımı izlemek için uyarıları kullanın. Azaltma başlatır ve azaltma durduktan sonra temizlenir sonra bu uyarılar oluşturulur.

## <a name="cloud-archival-of-data"></a>Bulut veri arşivleme

Verilerinizi bulutta uzun süreli saklamak istediğiniz veri kutusu ağ geçidi kullanın. Kullanabileceğiniz **arşiv** uzun süreli saklama için depolama katmanı.

Arşiv katmanı, nadiren erişilen verileri depolamak için en az 180 gün için optimize edilmiştir. **Arşiv** katmanı en düşük depolama maliyetini sunar ancak en yüksek erişim maliyetine sahiptir. Daha fazla bilgi için Git [arşiv erişim katmanı](/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier).

### <a name="move-data-to-archive-tier"></a>Arşiv katmanı için veri taşıma

Başlamadan önce çalışan bir veri kutusu ağ geçidi aygıtı olduğundan emin olun. Ayrıntılı adımları [Öğreticisi: Azure veri kutusu ağ geçidi dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md) ve sonraki öğreticiye işletimsel bir cihaz bulunana kadar ilerledikten tutun.

- Her zamanki aktarımı yordam boyunca açıklanan şekilde yüklemek için veri kutusu ağ geçidi cihazı kullanın [veri kutusu ağ geçidi üzerinden veri aktarımı](data-box-gateway-deploy-add-shares.md).
- Veriler karşıya yüklendikten sonra arşiv katmanına taşımak gerekir. Blob katmanı iki şekilde ayarlayabilirsiniz: Azure PowerShell Betiği veya bir Azure depolama yaşam döngüsü yönetim ilkesi.  
    - Azure PowerShell kullanarak izleyin, bunlar [adımları](/azure/databox/data-box-how-to-set-data-tier#use-azure-powershell-to-set-the-blob-tier) arşiv katmanı verileri taşımak için.
    - Azure Yaşam Döngüsü Yönetimi'ni kullanarak, veriler için arşiv katmanını taşımak için şu adımları izleyin.
        - [Kayıt](/azure/storage/common/storage-lifecycle-management-concepts#register-for-preview) katmanı arşiv kullanılacak Blob yaşam döngüsü yönetimi hizmetinin önizlemesi.
        - Aşağıdaki ilkeyi kullanmak [arşiv verilerini alma](/azure/storage/blobs/storage-lifecycle-management-concepts#archive-data-at-ingest).
- BLOB arşiv işaretlenmiş bir kez sıcak veya soğuk katmanı sürece bunlar artık ağ geçidi tarafından değiştirilebilir. Yerel depolamada bir dosya ise (silmeler dahil) Yerel kopyaya yapılan değişiklikler katmanı arşiv olarak karşıya değil.
- Arşiv depolama birimindeki verileri okumak için sık erişimli veya seyrek erişimli blob katmanını değiştirerek rehydrated gerekir. [Paylaşım yenileme](data-box-gateway-manage-shares.md#refresh-shares) ağ geçidinde blob yeniden doldurma değil.

Daha fazla bilgi için nasıl hakkında daha fazla bilgi [Azure Blob Depolama yaşam döngüsünü yönetme](/azure/storage/common/storage-lifecycle-management-concepts).

## <a name="initial-bulk-transfer-followed-by-incremental-transfer"></a>Artımlı aktarım tarafından izlenen ilk toplu aktarımı

Data Box ve kutusu ağ geçidiyle birlikte büyük miktarda veri artımlı aktarımları tarafından izlenen bir toplu karşıya yükleme yapmak istediğinizde kullanın. Data Box toplu aktarımı bir çevrimdışı moda (ilk çekirdek) ve veri kutusu ağ geçidi için artımlı aktarımları (devam eden akış) için ağ üzerinden kullanın.

### <a name="seed-the-data-with-data-box"></a>Data Box ile verileri temel

Data Box'ı ve Azure Depolama'ya yükleme verileri kopyalamak için aşağıdaki adımları izleyin.

1. [Data Box'ınızı sipariş](/azure/databox/data-box-deploy-ordered).
2. [Veri kutusu ayarlama](/azure/databox/data-box-deploy-set-up).
3. [Data Box SMB üzerinden veri kopyalamak](/azure/databox/data-box-deploy-copy-data).
4. [Data Box döndürür, Azure veri yükleme doğrulayın](/azure/databox/data-box-deploy-picked-up).
5. Azure için verileri karşıya yükleme tamamlandıktan sonra tüm verileri Azure depolama kapsayıcıları olmalıdır. Data Box depolama hesabında tüm verileri kopyalanır emin olmak için Blob (ve dosya) kapsayıcısına gidin. Bu adı daha sonra kullanacağınız kapsayıcı adını not edin. Örneğin, aşağıdaki ekran görüntüsünde `databox` kapsayıcı Artımlı aktarım için kullanılır.

    ![Data Box hakkında veriler kapsayıcı](media/data-box-gateway-use-cases/data-container1.png)

İlk dengeli dağıtım aşaması bu toplu aktarımı tamamlanır.

### <a name="ongoing-feed-with-data-box-gateway"></a>Veri kutusu ağ geçidi ile akış devam eden

Veri kutusu ağ geçidi tarafından sürekli alımı için şu adımları izleyin.

1. Veri kutusu ağ geçidinde bir bulut paylaşımı oluşturun. Bu paylaşım, tüm veriler Azure depolama hesabına otomatik olarak yükler. Git **paylaşımları** tıklayıp, veri kutusu ağ geçidi kaynağı **+ Ekle paylaşımı**.

    ![Paylaşım Ekle + tıklayın](media/data-box-gateway-use-cases/add-share1.png)

2. Bu paylaşımı çekirdeği oluşturulmuş veri içeren bir kapsayıcıya eşler emin olun. İçin **Select blob kapsayıcısı**, seçin **var olanı kullan** ve nerede Data Box verilerden aktarılan kapsayıcıya göz atın.

    ![Paylaşım ayarları](media/data-box-gateway-use-cases/share-settings-select-existing-container1.png)

3. Paylaşım oluşturulduktan sonra paylaşıma yenileyin. Bu işlem, içerik azure'dan şirket içi paylaşımıyla yeniler.

    ![Paylaşımı Yenile](media/data-box-gateway-use-cases/refresh-share1.png)

    Paylaşım eşitlendiğinde, istemci üzerinde dosyaları değiştirildiyse veri kutusu ağ geçidi artımlı değişiklikler yükleyeceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Gateway sistem gereksinimlerini](data-box-gateway-system-requirements.md) gözden geçirin.
- [Data Box Gateway sınırlarını](data-box-gateway-limits.md) anlayın.
- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.