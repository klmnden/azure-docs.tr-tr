---
title: Azure Analysis Services genişleme | Microsoft Docs
description: Azure Analysis Services sunucuları ile ölçek genişletme çoğaltma
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6a69d8d60b2e588ded9ccca20521195ae11ff136
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449413"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services ölçeğini genişletme

Ölçeklendirme ile istemci sorguları arasında birden çok dağıtılabilir *sorgu çoğaltmalarını* içinde bir *sorgu havuzu*, yüksek sorgu iş yükleri sırasında yanıt sürelerini azaltma. Ayrıca ayırabilirsiniz sorgu havuzundan işleme, sağlama işleme ile istemci sorguları olumsuz etkilenmez. Ölçeklendirme, Azure portalında veya Analysis Services REST API kullanılarak yapılandırılabilir.

Ölçek genişletme standart fiyatlandırma katmanı sunucuları için kullanılabilir. Her sorgu çoğaltma sunucunuz ile aynı fiyat üzerinden faturalandırılır. Tüm sorgu çoğaltmaları, sunucunuzla aynı bölgede oluşturulur. Sunucunuz bulunduğu bölgeyi yapılandırabileceğiniz sorgu yinelemelerinin sayısı sınırlıdır. Daha fazla bilgi için bkz. [bölgelere göre kullanılabilirliği](analysis-services-overview.md#availability-by-region). Ölçek genişletme, sunucunuzun kullanılabilir bellek miktarını artırmaz. Bellek için planınızı yükseltmek gerekir. 

## <a name="why-scale-out"></a>Neden genişleme?

Bir normal server dağıtımında, işlem sunucusu ve sorgu sunucusu bir sunucusu işlevi görür. İstemci sorguları sunucunuzdaki modelleri karşı sorgu işleme birimi (QPU), sunucunuzun planının aşıyor veya yüksek sorgu iş yükleri ile aynı anda model işlemesi, performansı düşürebilir. 

Ölçeklendirme ile en fazla yedi ek sorgu çoğaltması kaynaklarla bir sorgu havuzu oluşturabilirsiniz (sekiz toplam dahil olmak üzere, *birincil* sunucusu). Kritik zamanlarda QPU taleplerini karşılamak üzere sorgu havuzundaki çoğaltmalar sayısını ölçeklendirebilirsiniz ve herhangi bir anda bir işlem sunucusu sorgu havuzundan ayırabilirsiniz. 

Bir sorgu havuzundaki sahip sorgu çoğaltmaları sayısından bağımsız olarak, işleme iş yükleri arasında sorgu çoğaltmaları dağıtılmadı. Birincil sunucu işlem sunucusu olarak görev yapar. Sorgu çoğaltmaları birincil sunucu ve sorgu havuzundaki her çoğaltma arasında eşitlenen modeli veritabanları sorguları yalnızca hizmet. 

Ölçek genişletme, artımlı olarak sorgu havuzuna eklenecek yeni sorgu çoğaltmaları için beş dakikaya kadar sürebilir. Tüm yeni sorgu çoğaltmaları hazır ve çalışır hale geldiğinde, yeni istemci bağlantılarını sorgu Havuzu'ndaki kaynakları arasında Yük Dengelemesi. Mevcut istemci bağlantıları, şu anda bağlı oldukları kaynaktan değiştirilmez. Ölçek artırma, tüm mevcut istemci bağlantıları için sorgu havuzdan kaldırılmadan bir sorgu havuzu kaynak sonlandırılır. İstemciler, kalan sorgu kaynak havuzuna bağlanabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Ölçek genişletme ilk kez yapılandırırken, modeli veritabanları, birincil sunucuda olan *otomatik olarak* yeni yinelemede yeni bir sorgu havuzu ile eşitlenir. Otomatik eşitleme yalnızca bir kez gerçekleşir. Otomatik eşitleme sırasında birincil sunucunun veri dosyaları (blob depolama alanındaki bekleyen şifreli) de blob depolama alanındaki bekleyen şifreli ikinci bir konuma kopyalanır. Sorgu havuzundaki çoğaltmalar değişkenler, *hydrated* verilerle ikinci dosya kümesi. 

Yalnızca, bir ilk kez genişleme sunucusu, bir otomatik eşitleme gerçekleştirilirken, el ile eşitleme de gerçekleştirebilirsiniz. Eşitleme, veriler üzerinde sorgu havuzundaki çoğaltmalar birincil sunucudaki eşleşen sağlar. Birincil sunucuda (yenileme) modelleri işlerken bir eşitleme gerçekleştirilmelidir *sonra* işlemleri gerçekleştirilir. Bu eşitleme güncelleştirilmiş verileri birincil sunucunun dosyaları blob depolama alanında ikinci dosyalar kümesine kopyalar. Sorgu havuzundaki çoğaltmalar dosyaları blob depolama alanında ikinci kümesinden güncelleştirilen verilerle hydrated. 

Sonraki bir ölçeklendirme işlemi gerçekleştirirken, örneğin, iki beş, sorgu havuzundaki çoğaltmalar sayısını artırmak yeni çoğaltmaları dosyaları blob depolama alanında ikinci kümesini verilerle hydrated. Eşitleme yoktur. Daha sonra gerçekleştirmeyi olsaydı ölçeği genişletme, sorgu havuzu yeni yinelemede olacaktır sonra eşitleme yedekli hidrasyonu iki kez - hydrated. Sonraki bir ölçeklendirme işlemi gerçekleştirirken, göz önünde bulundurmanız önemlidir:

* Bir eşitleme gerçekleştirmeniz *ölçeklendirme işleminden önce* eklenen çoğaltmaların yedek hidrasyonu önlemek için.

* Her iki işlem otomatikleştirirken *ve* ölçek genişletme işlemleri önemlidir ilk olarak birincil sunucuda, veri işleme sonra bir eşitleme gerçekleştirin ve sonra genişleme işlemi gerçekleştirin. Bu sıra QPU ve bellek kaynakları üzerinde en az etki sağlar.

* Eşitleme bile sorgu havuzundaki hiçbir çoğaltması olduğunda izin verilir. Sıfırdan yeni verilerle bir veya daha fazla çoğaltma birincil sunucudaki bir işleme işlemi ölçeği genişletmeyi, sorgu havuzundaki hiçbir çoğaltması ile ilk eşitleme gerçekleştirin ve sonra ölçek genişletme. Ölçeği genişletme önce eşitleme, yeni eklenen çoğaltmaların yedek hidrasyonu önler.

* Bir model veritabanı birincil sunucudan silinirken, otomatik olarak sorgu havuzundaki çoğaltmaların silinebilir değil. Bir eşitleme işlemi kullanarak gerçekleştirmeniz gerektiğini [eşitleme AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) bu veritabanı için dosya/sn yinelemenin paylaşılan blob depolama konumundan kaldırır ve ardından modeli siler PowerShell komutu Sorgu havuzundaki çoğaltmalarındaki veritabanı.

* Birincil sunucuda bir veritabanını yeniden adlandırma, veritabanı için tüm çoğaltmaları doğru eşitlendiğinden emin olmak için gereken ek bir adım yoktur. Yeniden adlandırdıktan sonra eşitleme kullanarak gerçekleştirin. [eşitleme AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) komut belirtme `-Database` parametresi eski bir veritabanı adı. Bu eşitleme, eski adıyla dosya ve veritabanı tüm çoğaltmaları kaldırır. Ardından başka bir eşitleme belirtme gerçekleştirin `-Database` yeni veritabanı adı ile parametre. İkinci eşitleme, yeni adlandırılmış veritabanı dosyaları ikinci kümesine kopyalar ve tüm çoğaltmaları hydrates. Portalda Eşitle modeli komutunu kullanarak bu eşitleme gerçekleştirilemiyor.

### <a name="separate-processing-from-query-pool"></a>Sorgu havuzundan ayrı işleme

Hem işlem hem de sorgu işlemleri için en yüksek performans için işlem sunucunuzun sorgu havuzundan ayırmak seçebilirsiniz. Ayrılmış, mevcut ve yeni istemci bağlantılarını sorgu çoğaltmaları yalnızca sorgu havuzundaki atanır. İşlemleri yalnızca kısa bir süre alırsa, sorgu havuzu, işleme ve eşitleme işlemleri ve ardından sorgu havuza eklemek için gereken süre, işlem sunucusundan ayrı seçebilirsiniz. 

## <a name="monitor-qpu-usage"></a>QPU kullanımı izleme

Sunucunuz gerekli için ölçek genişletme belirlemek için Azure portalında sunucunuzu ölçümleri kullanarak izleyin. Çıkış, QPU düzenli olarak yükselir, Modellerinizi karşı sorgu sayısı, planınız için QPU sınırını aştığından anlamına gelir. Sorgu iş parçacığı havuzu kuyruğundaki sorgusu sayısı üzerinden kullanılabilir QPU aştığında sorgu havuzu iş kuyruğu uzunluğu ölçüm da artırır. 

İzlemek için başka bir iyi ölçüm ServerResourceType göre ortalama QPU ' dir. Bu ölçüm, sorgu havuzu birincil sunucusu için ortalama QPU karşılaştırır. 

### <a name="to-configure-qpu-by-serverresourcetype"></a>QPU ServerResourceType tarafından yapılandırmak için
1. Ölçümleri çizgi grafiğine tıklayın **ölçüm Ekle**. 
2. İçinde **kaynak**, sunucunuzun sonra seçin **ÖLÇÜM ad alanı**seçin **Analysis Services standart ölçüm**, ardından **ÖLÇÜM**, seçin **QPU**ve ardından **toplama**seçin **ortalama**. 
3. Tıklayın **bölme uygulamak**. 
4. İçinde **değerleri**seçin **ServerResourceType**.  

Daha fazla bilgi için bkz. [Sunucu ölçümlerini izleme](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Ölçek genişletmeyi yapılandırma

### <a name="in-azure-portal"></a>Azure portalında

1. Portalında **genişleme**. Sorgu çoğaltma sunucu sayısını seçmek için kaydırıcıyı kullanın. Mevcut sunucunuz yanı sıra seçtiğiniz yinelemeler sayısıdır.

2. İçinde **işleme sunucusunu sorgulama havuzundan ayırın**seçin işleme sunucunuza sorgu sunuculardan hariç tutmak için Evet. İstemci [bağlantıları](#connections) varsayılan bağlantı dizesini kullanarak (olmadan `:rw`) sorgu havuzundaki çoğaltmalar yönlendirilirsiniz. 

   ![Ölçek genişletme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Tıklayın **Kaydet** , yeni sorgu çoğaltma sunucuları sağlamak için. 

Ölçeği genişletilmiş bir sunucu için ilk kez yapılandırırken, modelleri, birincil sunucuda sorgu havuzundaki çoğaltmalar ile otomatik olarak eşitlenir. Ölçeği genişletilmiş bir veya daha fazla çoğaltma için ilk yapılandırırken sonra otomatik eşitleme yalnızca gerçekleşir. Aynı sunucuda çoğaltma sayısı sonraki değişiklikler *başka bir otomatik eşitleme tetiklemez*. Çoğaltmaları sıfır ve daha sonra tekrar çoğaltmaları herhangi bir sayıda genişleme sunucuya ayarlasanız bile otomatik eşitleme yeniden gerçekleşmez. 

## <a name="synchronize"></a>Eşitleme 

El ile veya REST API'yi kullanarak eşitleme işlemlerinin gerçekleştirilmesi gerekir.

### <a name="in-azure-portal"></a>Azure portalında

İçinde **genel bakış** > model > **Eşitle modeli**.

![Ölçek genişletme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST API

Kullanım **eşitleme** işlemi.

#### <a name="synchronize-a-model"></a>Bir modeli Eşitle   

`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Eşitleme durumunu Al  

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

Döndürülen durum kodları:


|Kod  |Açıklama  |
|---------|---------|
|-1     |  Geçersiz       |
|0     | Çoğaltılıyor        |
|1     |  Dolduruluyor       |
|2     |   Tamamlandı       |
|3     |   Başarısız      |
|4     |    Sonlandırılıyor     |
|||


### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i kullanarak önce [yükleme veya en son Azure PowerShell modülü güncelleştirme](/powershell/azure/install-az-ps). 

Eşitleme çalıştırmak için kullandığınız [eşitleme AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance).

Sorgu yinelemelerinin sayısı ayarlamak için kullanın [kümesi AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). İsteğe bağlı belirtin `-ReadonlyReplicaCount` parametresi.

Sorgu havuzu işleme sunucudan ayırmak için kullanın [kümesi AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). İsteğe bağlı belirtin `-DefaultConnectionMode` kullanılacak parametreyi `Readonly`.

## <a name="connections"></a>Bağlantılar

Sunucunuzun genel bakış sayfasında, iki sunucu adları vardır. Ölçeği genişletilmiş bir sunucu için henüz yapılandırmadıysanız, her iki sunucu adları aynı şekilde işler. Ölçeği genişletilmiş bir sunucu için yapılandırdıktan sonra bağlantı türüne bağlı olarak uygun sunucu adını belirtmek gerekir. 

Power BI Desktop, Excel ve özel uygulamalar kullanma gibi son kullanıcı istemci bağlantıları için **sunucu adı**. 

Azure işlev uygulamaları ve ÇYN, SSMS, SSDT ve PowerShell bağlantı dizelerini kullanma **yönetim sunucusu adı**. Özel bir yönetim sunucusu adı içerir `:rw` (okuma-yazma) niteleyicisi. (Birincil) yönetim sunucusu üzerindeki tüm işleme faaliyetlerinden oluşur.

![Sunucu adları](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="troubleshoot"></a>Sorun giderme

**Sorun:** Kullanıcılar alma hatası **sunucusu bulunamıyor '\<sunucusunun adı >' bağlantı modunda 'ReadOnly' örneği.**

**Çözüm:** Seçerken **işleme sunucusunu sorgulama havuzundan ayırın** seçeneği, varsayılan bağlantı dizesini kullanarak istemci bağlantıları (olmadan `:rw`) sorgu havuzu kopyaya yönlendirilirsiniz. Eşitleme değildir, çünkü sorgu havuzundaki çoğaltmalar henüz çevrimiçi henüz tamamlanmamış, yeniden yönlendirilen istemci bağlantıları başarısız olabilir. Başarısız bağlantılar önlemek için olmalıdır en az iki sunucu sorgu havuzundaki bir eşitleme yaparken. Her sunucu, tek tek diğer çevrimiçi kalırken eşitlenir. İşleme sunucusunu sorgu havuzu işleme sırasında yok. isterseniz, işleme için havuzundan kaldırın ve ardından havuza geri işleme tamamlandıktan sonra ancak eşitleme öncesindeki ekleyin seçebilirsiniz. Eşitleme durumunu izlemek için bellek ve QPU ölçümlerini kullanın.

## <a name="related-information"></a>İlgili bilgiler

[Sunucu ölçümlerini izleme](analysis-services-monitor.md)   
[Azure Analysis Services'ı yönetme](analysis-services-manage.md) 
