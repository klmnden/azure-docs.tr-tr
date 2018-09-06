---
title: Azure Analysis Services genişleme | Microsoft Docs
description: Azure Analysis Services sunucuları ile ölçek genişletme çoğaltma
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 730b11fb5038e5d6c4f9b00fbc4eb07d673757f9
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43840998"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services ölçeğini genişletme

Ölçeklendirme ile istemci sorguları arasında birden çok dağıtılabilir *sorgu çoğaltmalarını* bir sorgu havuzundaki yüksek sorgu iş yükleri sırasında yanıt sürelerini azaltma. Ayrıca ayırabilirsiniz sorgu havuzundan işleme, sağlama işleme ile istemci sorguları olumsuz etkilenmez. Ölçeklendirme, Azure portalında veya Analysis Services REST API kullanılarak yapılandırılabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir normal server dağıtımında, işlem sunucusu ve sorgu sunucusu bir sunucusu işlevi görür. İstemci sorguları sunucunuzdaki modelleri karşı sorgu işleme birimi (QPU), sunucunuzun planının aşıyor veya yüksek sorgu iş yükleri ile aynı anda model işlemesi, performansı düşürebilir. 

Ölçeklendirme ile en fazla yedi ek sorgu çoğaltması kaynakları (sizin sunucunuzla birlikte Toplam sekiz) ile bir sorgu havuzu oluşturabilirsiniz. Kritik zamanlarda QPU taleplerini karşılamak üzere sorgu yinelemelerinin sayısı ölçeklendirilebilir ve herhangi bir anda bir işlem sunucusu sorgu havuzundan ayırabilirsiniz. Tüm sorgu çoğaltmaları, sunucunuzla aynı bölgede oluşturulur.

Bir sorgu havuzundaki sahip sorgu çoğaltmaları sayısından bağımsız olarak, işleme iş yükleri arasında sorgu çoğaltmaları dağıtılmadı. Tek bir sunucu işlem sunucusu olarak görev yapar. Sorgu çoğaltmaları, yalnızca sorgu havuzundaki her sorgu çoğaltma arasında eşitlenen modelleri karşı sorgular işlevi görür. 

Ölçek genişletme, yeni sorgu çoğaltmaları artımlı olarak sorgu havuzuna eklenir. Bu sorgu havuza eklenecek yeni sorgu çoğaltması kaynaklar için beş dakikaya kadar sürebilir; istemci bağlantıları ve sorguları almaya hazır. Ne zaman tüm yeni sorgu çoğaltmaları hazır ve çalışır hale geldiğinde, yeni istemci bağlantılarını tüm sorgu havuzu kaynaklar arasında Yük Dengelemesi yapılıyor. Mevcut istemci bağlantıları, şu anda bağlı oldukları kaynaktan değiştirilmez.  Ölçek artırma, tüm mevcut istemci bağlantıları için sorgu havuzdan kaldırılmadan bir sorgu havuzu kaynak sonlandırılır. Ölçek işlemi tamamlandıktan sonra kalan sorgu kaynak havuzuna bağlanır.

İşlemleri tamamlandıktan sonra modeller, işleme sırasında işlem sunucusu ve sorgu çoğaltmaları eşitleme gerçekleştirilmelidir. İşlemleri otomatikleştirme, işlem işleme başarıyla tamamlandıktan sonra bir eşitleme işlemi yapılandırılması önemlidir. Eşitleme, portalda veya PowerShell veya REST API kullanarak el ile gerçekleştirilebilir. 

Hem işlem hem de sorgu işlemleri için en yüksek performans için işlem sunucunuzun sorgu havuzundan ayırmak seçebilirsiniz. Ayrılmış, mevcut ve yeni istemci bağlantılarını sorgu çoğaltmaları yalnızca sorgu havuzundaki atanır. İşlemleri yalnızca kısa bir süre alırsa, sorgu havuzu, işleme ve eşitleme işlemleri ve ardından sorgu havuza eklemek için gereken süre, işlem sunucusundan ayrı seçebilirsiniz. 

> [!NOTE]
> Ölçek genişletme standart fiyatlandırma katmanı sunucuları için kullanılabilir. Her sorgu çoğaltma sunucunuz ile aynı fiyat üzerinden faturalandırılır.

> [!NOTE]
> Ölçek genişletme, sunucunuzun kullanılabilir bellek miktarını artırmaz. Bellek için planınızı yükseltmek gerekir.

## <a name="region-limits"></a>Bölge sınırları

Sunucunuz bulunduğu bölgeyi yapılandırabileceğiniz sorgu yinelemelerinin sayısı sınırlıdır. Daha fazla bilgi için bkz. [bölgelere göre kullanılabilirliği](analysis-services-overview.md#availability-by-region).

## <a name="monitor-qpu-usage"></a>QPU kullanımı izleme

 Sunucunuz gerekli için ölçek genişletme belirlemek için Azure portalında sunucunuzu ölçümleri kullanarak izleyin. Çıkış, QPU düzenli olarak yükselir, Modellerinizi karşı sorgu sayısı, planınız için QPU sınırını aştığından anlamına gelir. Sorgu iş parçacığı havuzu kuyruğundaki sorgusu sayısı üzerinden kullanılabilir QPU aştığında sorgu havuzu iş kuyruğu uzunluğu ölçüm da artırır. Daha fazla bilgi için bkz. [Sunucu ölçümlerini izleme](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Ölçek genişletmeyi yapılandırma

### <a name="in-azure-portal"></a>Azure portalında

1. Portalında **genişleme**. Sorgu çoğaltma sunucu sayısını seçmek için kaydırıcıyı kullanın. Mevcut sunucunuz yanı sıra seçtiğiniz yinelemeler sayısıdır.

2. İçinde **işleme sunucusunu sorgulama havuzundan ayırın**seçin işleme sunucunuza sorgu sunuculardan hariç tutmak için Evet.

   ![Ölçek genişletme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Tıklayın **Kaydet** , yeni sorgu çoğaltma sunucuları sağlamak için. 

Tablosal modeller, birincil sunucuda çoğaltma sunucuları ile eşitlenir. Eşitleme tamamlandığında, gelen sorguları çoğaltma sunucusu arasında dağıtma sorgu havuzu başlar. 


## <a name="synchronization"></a>Eşitleme 

Yeni sorgu çoğaltmaları sağladığınızda, Azure Analysis Services Modellerinizi genelinde tüm çoğaltma otomatik olarak çoğaltır. Ayrıca, el ile eşitleme portalı veya REST API'yi kullanarak gerçekleştirebilirsiniz. Modellerinizi işlediğinizde, güncelleştirmeleri, sorgu yinelemeleri arasında eşitlenen için bir eşitleme gerçekleştirmeniz gerekir.

### <a name="in-azure-portal"></a>Azure portalında

İçinde **genel bakış** > model > **Eşitle modeli**.

![Ölçek genişletme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST API
Kullanım **eşitleme** işlemi.

#### <a name="synchronize-a-model"></a>Bir modeli Eşitle   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Eşitleme durumunu Al  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
PowerShell'i kullanarak önce [yükleme veya en son AzureRM modülünü güncelleştirme](https://github.com/Azure/azure-powershell/releases). 

Sorgu yinelemelerinin sayısı ayarlamak için kullanın [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). İsteğe bağlı belirtin `-ReadonlyReplicaCount` parametresi.

Eşitleme çalıştırmak için kullandığınız [eşitleme AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).



## <a name="connections"></a>Bağlantılar

Sunucunuzun genel bakış sayfasında, iki sunucu adları vardır. Ölçeği genişletilmiş bir sunucu için henüz yapılandırmadıysanız, her iki sunucu adları aynı şekilde işler. Ölçeği genişletilmiş bir sunucu için yapılandırdıktan sonra bağlantı türüne bağlı olarak uygun sunucu adını belirtmek gerekir. 

Power BI Desktop, Excel ve özel uygulamalar kullanma gibi son kullanıcı istemci bağlantıları için **sunucu adı**. 

Azure işlev uygulamaları ve ÇYN, SSMS, SSDT ve PowerShell bağlantı dizelerini kullanma **yönetim sunucusu adı**. Özel bir yönetim sunucusu adı içerir `:rw` (okuma-yazma) niteleyicisi. Yönetim sunucusu üzerindeki tüm işleme faaliyetlerinden oluşur.

![Sunucu adları](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>İlgili bilgiler

[Sunucu ölçümlerini izleme](analysis-services-monitor.md)   
[Azure Analysis Services'ı yönetme](analysis-services-manage.md) 

