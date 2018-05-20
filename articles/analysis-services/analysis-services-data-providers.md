---
title: Azure Analysis Services'a bağlanmak için gereken istemci kitaplığı | Microsoft Docs
description: Azure Analysis Services bağlanmak istemci uygulamaları ve araçları için gerekli istemci kitaplıkları açıklar
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c6c92bdd2461a0f1147f15b5c1134c189c55a37c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Azure Analysis Services'a bağlanmak için istemci kitaplıkları

İstemci kitaplıkları, Analysis Services sunucularına bağlanmak için istemci uygulamaları ve araçları için gereklidir. 

## <a name="download-the-latest-client-libraries-windows-installer"></a>Son istemci kitaplıkları (Windows Installer) yükle  

|Karşıdan Yükle  |Ürün sürümü  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.0.1.395      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    15.0.1.395      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   15.0.4     |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    15.0.4     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO ve ADOMD (NuGet paketleri)

Analysis Services yönetim nesneleri (AMO) ve ADOMD istemci kitaplıkları yüklenebilir paketleri olarak kullanılabilir [NuGet.org](https://www.nuget.org/). Windows Installer kullanmak yerine NuGet başvurularını geçiş yapmanız önerilir. 

|Paket  | Ürün sürümü  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    15.0.2.0      |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   15.0.2.0      |

NuGet paket derlemeleri AssemblyVersion izleyin anlamsal sürüm oluşturma: önemli. KÜÇÜK. DÜZELTME EKİ. Olsa bile farklı bir sürümünü (MSI yükle sonuçlanır) GAC'de NuGet başvurularını beklenen sürümü yüklenemiyor. Düzeltme eki her sürüm için artırılır. AMO ve ADOMD sürümleri eşitleme tutulur.

## <a name="understanding-client-libraries"></a>İstemci kitaplıkları anlama

Analysis Services üç istemci kitaplıkları, olarak da bilinen veri sağlayıcıları kullanır. ADOMD.NET ve Çözümleme Hizmetleri yönetim nesneleri (AMO) yönetilen istemci kitaplıkları ' dir. Analysis Services OLE DB sağlayıcısı (MSOLAP DLL) yerel istemci Kitaplığı ' dir. Genellikle, tüm üç aynı anda yüklenir. **Azure Analysis Services, tüm üç kitaplıklarının en son sürümlerini gerektirir**. 

Microsoft Power BI Desktop ve Excel gibi istemci uygulamaları, tüm üç istemci kitaplıkları yükleyin ve yeni sürümler kullanılabilir olduğunda bunları güncelleştirin. Sürüm veya güncelleştirmelerinin sıklığını bağlı olarak, bazı istemci kitaplıkları Azure Analysis Services tarafından gerekli en son sürümlerini olmayabilir. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar, el ile veya programlama kitaplıkları yükleme gerektirir. İstemci kitaplıkları el ile yükleme için SQL Server özellik paketlerinde dağıtılabilir paketleri dahil edilir. Ancak, bu istemci kitaplıkları, SQL Server sürümüne bağlıdır ve en son olmayabilir.  

İstemci bağlantıları için istemci kitaplıkları, Azure Analysis Services sunucusundan bir veri kaynağına bağlanmak için gereken veri sağlayıcıları farklıdır. Veri kaynağı bağlantıları hakkında daha fazla bilgi için bkz: [veri kaynağı bağlantıları](analysis-services-datasource.md).

## <a name="client-library-types"></a>İstemci Kitaplığı türleri

### <a name="analysis-services-ole-db-provider-msolap"></a>Analysis Services OLE DB sağlayıcısı (MSOLAP) 

 Analysis Services OLE DB sağlayıcısı (MSOLAP) Analysis Services veritabanı bağlantıları için yerel istemci Kitaplığı ' dir. Ayrıca, ADOMD.NET ve AMO bağlantı istekleri için veri sağlayıcısı için temsilci seçme, tarafından dolaylı olarak kullanılır. Ayrıca, OLE DB sağlayıcısı doğrudan uygulama kodunuzdan çağırabilirsiniz.  
  
 Analysis Services OLE DB sağlayıcısı, çoğu araçları ve Analysis Services veritabanları erişmek için kullanılan istemci uygulamaları tarafından otomatik olarak yüklenir. Analysis Services verilerine erişmek için kullanılan bilgisayarlara yüklenmesi gerekir.  
  
 OLE DB sağlayıcıları genellikle bağlantı dizeleri belirtilir. OLE DB sağlayıcısı için başvurmak için farklı bir terminoloji bir Analysis Services bağlantı dizesini kullanır: MSOLAP. \<sürüm > .dll.

### <a name="amo"></a>AMO  

 AMO sunucu yönetimi ve veri tanımı için kullanılan bir yönetilen istemci kitaplıktır. Yüklü ve araçları ve istemci uygulamaları tarafından kullanılan. Örneğin, SQL Server Management Studio (SSMS) AMO Analysis Services'a bağlanmak için kullanır. AMO kullanarak bağlantı oluşan genellikle, en alt düzeydedir `“data source=\<servername>”`. Bağlantı kurulduktan sonra veritabanı koleksiyonları ve büyük nesneler ile çalışmak için API kullanın. SSDT ve SSMS AMO Analysis Services örneğine bağlanın.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET Analysis Services veri sorgulama için kullanılan bir yönetilen veri istemci kitaplıktır. Yüklü ve araçları ve istemci uygulamaları tarafından kullanılan. 
  
 Bir veritabanına bağlanırken, tüm üç kitaplıkları için bağlantı dizesi özellikleri benzerdir. Tanımladığınız için ADOMD.NET kullanarak neredeyse her bağlantı dizesi [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) AMO ve Analysis Services OLE DB sağlayıcısı (MSOLAP) için de çalışır. Daha fazla bilgi için bkz: [bağlantı dizesi özellikleri &#40;Analysis Services&#41;](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a> İstemci kitaplığı sürümü belirleme   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Git ' C:\Program Files\Microsoft Analysis Services\AS OLEDB\. Birden fazla klasör varsa, en yüksek sayıyı seçin.
  
2.  Sağ **msolap.dll** > **özellikleri** > **ayrıntıları**. Dosya adı msolap140.dll ise, eski en son sürüme ve yükseltilmelidir.
    
    ![İstemci Kitaplığı ayrıntıları](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\` kısmına gidin. Birden fazla klasör varsa, en yüksek sayıyı seçin.
2. Sağ **Microsoft.AnalysisServices** > **özellikleri** > **ayrıntıları**.  

### <a name="adomd"></a>ADOMD

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\` kısmına gidin. Birden fazla klasör varsa, en yüksek sayıyı seçin.
2. Sağ **Microsoft.AnalysisServices.AdomdClient** > **özellikleri** > **ayrıntıları**.  


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)
