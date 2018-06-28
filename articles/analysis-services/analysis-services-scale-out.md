---
title: Azure Analysis Services genişleme | Microsoft Docs
description: Azure Analysis Services sunucuları ölçeklendirme ile aynı
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5d1d55a1cf29d6dc3574099cd468c42ccfc72f5b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34597136"
---
# <a name="azure-analysis-services-scale-out"></a>Azure Analysis Services genişletme

Genişletme ile istemci sorguları arasında birden çok dağıtılabilir *sorgu çoğaltmaları* bir sorgu havuzda yüksek sorgu iş yükleri sırasında yanıt sürelerini azaltma. Ayrıca ayırabilirsiniz sorgu havuzundan işleme, işlemlerinde tarafından istemci sorguları olumsuz etkilenmez sağlama. Genişletme, Azure portalında veya Analysis Services REST API kullanarak yapılandırılabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Tipik sunucu dağıtımında, bir sunucu işleme hem sorgu sunucusu görev yapar. Sorgu işleme birimleri (QPU) sunucunuzun planının modelleri sunucunuzda istemci sorguları sayısını aşıyor ya da yüksek sorgu iş yükleri aynı zamanda model işlemesi, performansı düşürebilir. 

Genişletme ile en çok yedi ek sorgu çoğaltmaları (sekiz toplam sunucunuz dahil) olan bir sorgu havuzu oluşturabilirsiniz. Kritik zamanlarda QPU taleplerini karşılamak üzere sorgu çoğaltmaların sayısı ölçeklendirebilirsiniz ve herhangi bir anda bir işlem sunucusu sorgu havuzundan ayırabilirsiniz. Tüm sorgu çoğaltmaları sunucunuz ile aynı bölgede oluşturulur.

Sorgu havuzda sahip sorgu çoğaltmaları sayısından bağımsız olarak, işleme iş yüklerinin sorgu çoğaltmalar arasında dağıtılmadı. Tek bir sunucu işlem sunucusu olarak görev yapar. Sorgu çoğaltmaları yalnızca sorgu havuzundaki her çoğaltma arasında eşitlenen modelleri sorguları hizmet. 

İşlemleri tamamlandığında işlem sunucusu ve sorgu çoğaltma sunucusu arasında bir eşitleme gerçekleştirilmelidir. İşlemleri otomatik hale getirme, işlemler işleme başarıyla tamamlandıktan sonra bir eşitleme işlemi yapılandırmak önemlidir. Eşitleme portalında veya PowerShell veya REST API kullanarak el ile gerçekleştirilebilir.

> [!NOTE]
> Genişleme standart fiyatlandırma katmanı sunucuları için kullanılabilir. Her sorgu çoğaltma sunucunuz ile aynı hızda faturalandırılır.

> [!NOTE]
> Genişleme sunucunuz için kullanılabilir bellek miktarını artırın değil. Bellek artırmak için planınızı yükseltme yapmanız gerekir.

## <a name="region-limits"></a>Bölge sınırları

Yapılandırabileceğiniz sorgu çoğaltmaların sayısı, sunucunun bulunduğu bölgeye göre sınırlıdır. Aşağıdaki sınırları Uygula:

|Bölge  |Yineleme sayısı üst sınırı  |
|---------|---------|
|Doğu ABD 2    |    7     |
|Batı Orta ABD     |    7     |
|Batı Avrupa     |    7     |
|Batı ABD     |     7    |
|Orta ABD     |     3    |
|Güneydoğu Asya    |     3    |
|Diğer tüm bölgeler  |   1    |



## <a name="monitor-qpu-usage"></a>QPU kullanımını izleme

 Sunucunuz gereklidir genişleme varsa belirlemek için ölçümleri kullanarak Azure portalında sunucunuz izleyin. QPU düzenli olarak kullanıma maxes, Modellerinizi sorguları sayısı planınıza QPU sınırını aşan anlamına gelir. Sorgu sorgu iş parçacığı havuzu sırasındaki sayısı kullanılabilir QPU aştığında sorgu havuzu iş kuyruğu uzunluğu ölçüm da artırır. Daha fazla bilgi için bkz. [Sunucu ölçümlerini izleme](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Genişleme yapılandırın

### <a name="in-azure-portal"></a>Azure portalında

1. Portalı'nda tıklatın **genişleme**. Sorgu çoğaltma sunucularının sayısını seçmek için kaydırıcıyı kullanın. Varolan sunucunuzu ek olarak seçtiğiniz çoğaltmaları sayısıdır.

2. İçinde **sorgulanırken havuzu işleme sunucusundan ayrı**seçin işleme sunucunuz sorgu sunucularından dışlamak için Evet.

   ![Genişleme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Tıklatın **kaydetmek** , yeni sorgu çoğaltma sunucuları sağlamak için. 

Birincil sunucunuzda tablolu modeller çoğaltma sunucuları ile eşitlenir. Eşitleme tamamlandığında, gelen sorguları çoğaltma sunucusu arasında dağıtma sorgu havuzu başlar. 


## <a name="synchronization"></a>Eşitleme 

Yeni sorgu çoğaltmaları sağladığınızda, Azure Analysis Services Modellerinizi genelinde tüm çoğaltma otomatik olarak çoğaltır. Portalı veya REST API'yi kullanarak el ile eşitleme de gerçekleştirebilirsiniz. Modellerinizi işlerken güncelleştirmeleri sorgu çoğaltmalarınızı arasında eşitlenir şekilde bir eşitleme gerçekleştirmeniz gerekir.

### <a name="in-azure-portal"></a>Azure portalında

İçinde **genel bakış** > modeli > **Eşitle modeli**.

![Genişleme kaydırıcı](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST API
Kullanım **eşitleme** işlemi.

#### <a name="synchronize-a-model"></a>Bir model Eşitle   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Eşitleme durumunu Al  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
PowerShell, kullanmadan önce [veya son AzureRM Modülü güncelleştirmesini](https://github.com/Azure/azure-powershell/releases). 

Sorgu çoğaltmaların sayısı ayarlamak için kullanın [kümesi AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). İsteğe bağlı belirtin `-ReadonlyReplicaCount` parametresi.

Eşitleme çalıştırmak için kullandığınız [eşitleme AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).



## <a name="connections"></a>Bağlantılar

Sunucunuzun genel bakış sayfasında, iki sunucu adları vardır. Henüz bir sunucu için genişleme yapılandırmadıysanız, her iki sunucu adları aynı çalışır. Bir sunucu için genişleme yapılandırdıktan sonra bağlantı türüne bağlı olarak uygun sunucu adı belirtmeniz gerekir. 

Power BI Desktop, Excel ve özel uygulamalar kullanma gibi son kullanıcı istemci bağlantıları için **sunucu adı**. 

Azure işlev uygulamalarının ve AMO, SSMS, SSDT ve PowerShell bağlantı dizeleri için kullanma **yönetim sunucusu adı**. Özel bir yönetim sunucusu adını içeren `:rw` (okuma-yazma) niteleyicisi. Tüm işlemleri yönetim sunucu üzerinde meydana gelir.

![Sunucu adları](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>İlgili bilgiler

[İzleyici Sunucusu ölçümleri](analysis-services-monitor.md)   
[Azure Analysis Services yönetme](analysis-services-manage.md) 

