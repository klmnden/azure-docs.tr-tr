---
title: Python uygulamaları Azure Application Insights ile izleme | Microsoft Docs
description: Application Insights ile OpenCensus Python'kurmak wire için yönergeler sağlar
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 07/02/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: 2c043ad793dcf5e59eaf460d1ec4aa7a3b48810d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541447"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Python'dan (Önizleme) dağıtılmış izlemeleri toplamak

Application Insights artık Python uygulamaları ile tümleştirme yoluyla izlemeyi destekleyen dağıtılmış [OpenCensus](https://opencensus.io). Bu makalede, Python için OpenCensus ayarlama ve Application Insights için izleme verilerinizin alma sürecinde adım adım yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Aboneliğine sahip olmanız gerekir.
- Bu makalede, Python yüklenmesi [Python 3.7.0](https://www.python.org/downloads/), ancak önceki sürümleri ile küçük düzeltme çalışma olasılığı yüksektir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-application-insights-resource"></a>Application Insights kaynağı oluşturun

İlk izleme key(ikey) oluşturacak bir Application Insights kaynağı oluşturmak gerekir. İkey sonra Application Insights'a telemetri verileri göndermek için OpenCensus SDK'sını yapılandırmak için kullanılır.

1. Seçin **kaynak Oluştur** > **Geliştirici Araçları** > **Application Insights**.

   ![Application Insights Kaynağı ekleme](./media/opencensus-python/0001-create-resource.png)

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Name**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Location** | East US | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="install-opencensus-azure-monitor-exporters"></a>OpenCensus Azure İzleyici vericiler yükleyin

1. OpenCensus Azure İzleyici vericiler yükleyin:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` bir yol ortam değişkenine Python yüklemenizi kümesi olduğunu varsayar. Bu yapılandırmadıysanız Python yürütülebilir dosyanın, istediğiniz sonuç komutunda bulunduğu için tam dizin yolu vermeniz gerekir: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. İlk bazı izleme verileri yerel olarak şimdi oluşturun. Python boşta veya tercih ettiğiniz düzenleyiciyi, aşağıdaki kodu girin:

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

3. Kod çalıştırma sürekli bir değer girmenizi ister. Kabuk ve karşılık gelen bir değer her bir girdi, yazdırılır **SpanData** OpenCensus Python modülü tarafından oluşturulur. OpenCensus proje tanımlayan bir [ _izleme ağacı yayılma olarak_](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

4. Yararlı tanıtım amacıyla, ancak sonuç olarak Application ınsights'a SpanData yayma istiyoruz. Aşağıdaki kod örneği temel alarak önceki adımdaki kodunuzu değiştirin:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            instrumentation_key='00000000-0000-0000-0000-000000000000',
        ),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```
5. Şimdi Python betiğini yine de size sorulması değerler girmek için çalıştırdığınızda, ancak yalnızca değer Kabuğu'nda şimdi yazdırılıyor.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık Application ınsights'ı yeniden açabilirsiniz **genel bakış** şu anda çalışan uygulamanızın hakkında ayrıntıları görüntülemek için Azure portalında sayfası. Seçin **ölçüm Stream canlı**.

   ![Canlı ölçüm akışı kırmızı kutu içinde seçili genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Geri gidin **genel bakış** sayfasından seçim yapıp **Uygulama Haritası** çağrı zamanlama uygulama bileşenleriniz arasındaki bağımlılık ilişkileri ve görsel düzeni için.

    ![Temel Uygulama Haritası ekran görüntüsü](./media/opencensus-python/0007-application-map.png)

    Biz yalnızca bir yöntem çağrısının izleme olduğundan, bizim Uygulama Haritası olarak ilginç değil. Ancak, Uygulama Haritası daha dağıtılmış uygulamalar görselleştirmek için ölçeklendirebilirsiniz:

   ![Uygulama Eşlemesi](media/opencensus-python/application-map.png)

3. Seçin **araştırmak performans** ayrıntılı Performans Analizi gerçekleştirebilir ve performansın kök nedenini belirlemek için.

    ![Performans bölmesinin ekran görüntüsü](./media/opencensus-python/0008-performance.png)

4. Seçme **örnekleri** ve ardından sağ bölmede görüntülenen örnekleri hiçbirinde uçtan uca işlem ayrıntıları deneyimi başlayacaktır. Örnek uygulamamız yalnızca tek bir olay gösterecek, ancak daha karmaşık bir uygulama, tek tek olay çağrı yığını düzeyini aşağı uçtan uca işlem keşfetmek çalıştırmasına olanak tanır.

     ![Uçtan uca işlem arabiriminin ekran görüntüsü](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="opencensus-for-python"></a>Python için OpenCensus

* [Özelleştirme](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask tümleştirme](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django tümleştirme](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL tümleştirme](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Sonraki adımlar

* [Github'da OpenCensus Python](https://github.com/census-instrumentation/opencensus-python)
* [Uygulama Haritası](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)