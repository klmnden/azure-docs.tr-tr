---
title: Azure veri Gezgini'nde verileri analiz için Jupyter not defteri kullanın
description: Bu konuda bir Jupyter not defteri ve Kqlmagic uzantısını kullanarak Azure veri Gezgini'nde verilerini nasıl çözümleyeceğinizi gösterilmektedir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 896a5d13279c15f0035f214da3d5a7d7e6f1861f
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045074"
---
# <a name="use-a-jupyter-notebook-and-kqlmagic-extension-to-analyze-data-in-azure-data-explorer"></a>Azure veri Gezgini'nde verileri çözümlemek için bir Jupyter not defteri ve Kqlmagic uzantısı kullanma

Jupyter not defteri oluşturmak ve Canlı kod, denklemleri, görselleştirmeler ve anlatım metnini içeren belgeleri paylaşmak olanak tanıyan bir açık kaynaklı web uygulamasıdır. Kullanım, veri temizleme ve dönüştürme, sayısal bir simülasyon, modelleme, veri Görselleştirme ve makine öğrenimi içerir.
[Jupyter not defteri](https://jupyter.org/) ek komutlar destekleyerek çekirdek yeteneklerini genişletmek Sihirli işlevleri destekler. KQL Sihirli Kusto dil sorguları yerel olarak çalıştırabilmeniz için Jupyter not defteri Python çekirdek yeteneklerini genişleten bir komuttur. Sorgulamak ve tümleşik Plot.ly kitaplığı ile zengin kullanarak verileri görselleştirmek için Python ve Kusto sorgu dilini kolayca birleştirebilirsiniz `render` komutları. Sorguları çalıştırmak için veri kaynakları desteklenir. Bu veri kaynakları, Azure Veri Gezgini, Azure İzleyici günlüklerine yanı sıra günlük ve telemetri verilerini, hızlı ve yüksek düzeyde ölçeklenebilir veri keşif hizmeti ve Application Insights ekleyin. KQL Sihirli Azure not defterleri, Jupyter Laboratuvar ve Visual Studio kod Jupyter uzantısı ile de çalışır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Active Directory (AAD) bir üyesi olan Kuruluş e-posta hesabı.
- Jupyter not defteri yerel makinenizde yüklü Azure not defterleri kullanma veya örneği kopyalayın [Azure not defteri](https://kustomagicsamples-manojraheja.notebooks.azure.com/j/notebooks/Getting%20Started%20with%20kqlmagic%20on%20Azure%20Data%20Explorer.ipynb)

## <a name="install-kql-magic-library"></a>KQL Sihirli kitaplığını yükle

1. KQL Sihirli yükleyin:

    ```python
    !pip install Kqlmagic --no-cache-dir  --upgrade
    ```
    > [!NOTE]
    > Azure not defterlerini kullanarak, bu adım gerekli değildir.

1. KQL Sihirli yükle:

    ```python
    reload_ext Kqlmagic
    ```

## <a name="connect-to-the-azure-data-explorer-help-cluster"></a>Azure Veri Gezgini yardımcı kümeye bağlanma

Bağlanmak için aşağıdaki komutu kullanın *örnekleri* veritabanı barındırılmasına *yardımcı* kümesi. Microsoft AAD kullanıcıları için Kiracı adını değiştirin `Microsoft.com` AAD Kiracınız ile.

```python
%kql AzureDataExplorer://tenant="Microsoft.com";code;cluster='help';database='Samples'
```

## <a name="query-and-visualize"></a>Sorgulayın ve görselleştirin

Sorgu kullanarak verileri [işleci işleme](/azure/kusto/query/renderoperator) ve ploy.ly kitaplığını kullanarak verileri görselleştirin. Bu sorgu ve görselleştirme yerel KQL kullanan tümleşik bir deneyim sağlar. Kqlmagic dışında çoğu grafikleri destekler `timepivot`, `pivotchart`, ve `ladderchart`. İşleme tüm öznitelikleri ile desteklenen `kind`, `ysplit`, ve `accumulate`. 

### <a name="query-and-render-piechart"></a>Sorgu ve işleme pasta grafiği görüntüsü

```python
%%kql
StormEvents
| summarize statecount=count() by State
| sort by statecount 
| limit 10
| render piechart title="My Pie Chart by State"
```

### <a name="query-and-render-timechart"></a>Sorgu ve işleme zaman grafiğini

```python
%%kql
StormEvents
| summarize count() by bin(StartTime,7d)
| render timechart
```

> [!NOTE]
> Bu grafik etkileşimlidir. Belirli bir zaman yakınlaştırmak için bir zaman aralığı seçin.

### <a name="customize-the-chart-colors"></a>Grafik renkleri özelleştirme

Varsayılan renk paletindeki beğenmezseniz palet seçeneklerini kullanarak grafikleri özelleştirin. Kullanılabilir paletleri burada bulunabilir: [KQL Sihirli sorgu grafik sonucunuz için renk paleti seçin](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)

1. Paletleri listesi için:

    ```python
    %kql --palettes -popup_window
    ```

1. Seçin `cool` renk paletini ve sorguyu yeniden oluşturun:

    ```python
    %%kql -palette_name "cool"
    StormEvents
    | summarize statecount=count() by State
    | sort by statecount
    | limit 10
    | render piechart title="My Pie Chart by State"
    ```

## <a name="parameterize-a-query-with-python"></a>Python ile sorgu Parametreleştirme

Kusto sorgu dili ve Python arasında basit bir değişim KQL Sihirli sağlar. Daha fazla bilgi için: [Python ile KQL Sihirli sorgunuzu Parametreleştirme](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb)

### <a name="use-a-python-variable-in-your-kql-query"></a>Bir Python değişkeni KQL sorgunuzda kullanın

Verileri filtrelemek için sorgu Python değişkeninin değerini kullanabilirsiniz:

```python
statefilter = ["TEXAS", "KANSAS"]
```

```python
%%kql
let _state = statefilter;
StormEvents 
| where State in (_state) 
| summarize statecount=count() by bin(StartTime,1d), State
| render timechart title = "Trend"
```

### <a name="convert-query-results-to-pandas-dataframe"></a>Sorgu sonuçları için Pandas DataFrame Dönüştür

Pandas DataFrame KQL sorguda sonuçlarını erişebilirsiniz. Son yürütülen sorgu sonuçları değişkeni tarafından erişim `_kql_raw_result_` ve kolayca sonuçları Pandas DataFrame şu şekilde dönüştürebilirsiniz:

```python
df = _kql_raw_result_.to_dataframe()
df.head(10)
```

### <a name="example"></a>Örnek

Birçok analiz senaryolarda, çok sayıda sorgu içeren ve sonuçları bir sorgudan sonraki sorgulara akışı yeniden kullanılabilir not defterlerini oluşturmak isteyebilirsiniz. Aşağıdaki örnek Python değişkenini kullanır `statefilter` verilere filtre uygulamak için.

1. En üst 10 durumlarını görüntülemek için bir sorgu çalıştırdığınızda `DamageProperty`:

    ```python
    %%kql
    StormEvents
    | summarize max(DamageProperty) by State
    | order by max_DamageProperty desc
    | limit 10
    ```

1. Üst durum ayıklayın ve Python değişkene ayarlamak için bir sorgu çalıştırın:

    ```python
    df = _kql_raw_result_.to_dataframe()
    statefilter =df.loc[0].State
    statefilter
    ```

1. Kullanarak bir sorgu çalıştırın `let` deyimi ve Python değişkeni:

    ```python
    %%kql
    let _state = statefilter;
    StormEvents 
    | where State in (_state)
    | summarize statecount=count() by bin(StartTime,1d), State
    | render timechart title = "Trend"
    ```

1. Yardım komutu çalıştırın:

    ```python
    %kql --help "help"
    ```

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen tüm özellikleri içeren aşağıdaki örnek not defterleri keşfetmek için Yardım komutu çalıştırın:
- [Azure için Veri Gezgini ile KQL Sihirli kullanmaya başlayın](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStart.ipynb) 
- [İçin Application Insights ile KQL Sihirli kullanmaya başlayın](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartAI.ipynb) 
- [Azure İzleyici günlüklerine için KQL Sihirli kullanmaya başlama](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartLA.ipynb) 
- [Python ile parametrize KQL Sihirli sorgunuzu](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 
- [KQL Sihirli sorgu grafik sonucunuz için renk paleti seçin](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)
