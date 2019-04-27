---
title: Azure Application Insights ile OpenCensus Python izleme | Microsoft Docs
description: Yerel ileticisi ve Application Insights ile izleme OpenCensus Python'kurmak wire için yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 22e58f31e2f891eb09c3d42a01763c68cdcd11a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60577702"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Python'dan (Önizleme) dağıtılmış izlemeleri toplamak

Application Insights artık Python uygulamaları ile tümleştirme yoluyla izlemeyi destekleyen dağıtılmış [OpenCensus](https://opencensus.io) ve yeni [yerel ileticisi](./../../azure-monitor/app/opencensus-local-forwarder.md). Bu makalede adım adım OpenCensus ' için Python ayarlama ve Application Insights izleme verilerinize alma sürecinde size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Aboneliğine sahip olmanız gerekir.
- Bu makalede, Python yüklenmesi [Python 3.7.0](https://www.python.org/downloads/), ancak önceki sürümleri ile küçük düzeltme çalışma olasılığı yüksektir.
- Yüklemek için yönergeleri izleyin [bir Windows hizmeti olarak yerel ileticisi](./../../azure-monitor/app/opencensus-local-forwarder.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-application-insights-resource"></a>Application Insights kaynağı oluştur

İlk izleme key(ikey) oluşturacak bir Application Insights kaynağı oluşturmak gerekir. İkey sonra Application Insights için izleme eklenmiş OpenCensus uygulamanızdan dağıtılmış izlemeleri göndermek için yerel ileticisi yapılandırmak için kullanılır.   

1. Seçin **kaynak Oluştur** > **Geliştirici Araçları** > **Application Insights**.

   ![Application Insights Kaynağı ekleme](./media/opencensus-python/0001-create-resource.png)

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Uygulama Türü** | Genel | İzlemekte olduğunuz uygulamanın türü |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="configure-local-forwarder"></a>Yerel ileticisi yapılandırma

1. **Genel Bakış** > **Temel Bilgiler**’i seçin > Uygulamanızın **İzleme Anahtarı**’nı kopyalayın.

   ![İzleme anahtarı ekran görüntüsü](./media/opencensus-python/0003-instrumentation-key.png)

2. Düzenleme, `LocalForwarder.config` dosya ve izleme anahtarınızı ekleyin. İçindeki yönergeleri izlediyseniz [önkoşul](./../../azure-monitor/app/opencensus-local-forwarder.md) dosyası şu konumdadır `C:\LF-WindowsServiceHost`

    ```xml
      <OpenCensusToApplicationInsights>
        <!--
          Instrumentation key to track telemetry to.
          -->
        <InstrumentationKey>{enter-instrumentation-key}</InstrumentationKey>
      </OpenCensusToApplicationInsights>
    
      <!-- Describes aspects of processing Application Insights telemetry-->
      <ApplicationInsights>
        <LiveMetricsStreamInstrumentationKey>{enter-instrumentation-key}</LiveMetricsStreamInstrumentationKey>
      </ApplicationInsights>
    </LocalForwarderConfiguration>
    ```

3. Uygulamayı yeniden **yerel ileticisi** hizmeti.

## <a name="opencensus-python-package"></a>OpenCensus Python paketi

1. Açık Görselleştirmenizdeki paketi için Python pip ya da komut satırından pipenv ile yükleyin:

    ```python
    python -m pip install opencensus
    # pip env install opencensus
    ```

    > [!NOTE]
    > `python -m pip install opencensus` bir yol ortam değişkenine Python yüklemenizi kümesi olduğunu varsayar. Bu yapılandırmadıysanız Python yürütülebilir dosyanın, istediğiniz sonuç komutunda bulunduğu için tam dizin yolu vermeniz gerekir: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus`.

2. İlk bazı izleme verileri yerel olarak şimdi oluşturun. Python boşta veya tercih ettiğiniz düzenleyiciyi, aşağıdaki kodu girin:

    ```python
    from opencensus.trace.tracer import Tracer
    
    def main():
        while True:
            valuePrompt()
    
    def valuePrompt():
        tracer = Tracer()
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)
    
    if __name__ == "__main__":
        main()
    
    ```

3. Kod çalıştırma sürekli bir değer girmenizi ister. Kabuk ve karşılık gelen bir değer her bir girdi, yazdırılır **SpanData** OpenCensus Python modülü tarafından oluşturulur. OpenCensus proje tanımlayan bir [ _izleme ağacı yayılma olarak_](https://opencensus.io/core-concepts/tracing/).
    
    ```python
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=1f07f062ac394c50925f2ae61e635e14, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='5c17a4ad6ba14299', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:15.847292Z', end_time='2018-09-15T20:42:17.615664Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=c71b4e88a22a495da61df52ce3eee3e1, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='51547c0af5f046eb', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:17.615664Z', end_time='2018-09-15T20:48:11.160314Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=b4cdcc9e6df44a8fbb6e8ddeccc1351c, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f2caacf7892744d1', parent_span_id=None, attributes={}, start_time='2018-09-15T20:48:11.175931Z', end_time='2018-09-15T20:48:12.629178Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

4. Tanıtım amacıyla yararlı olsa da sonuçta SpanData bunu tarafından çekilmesi bir şekilde yayma istiyoruz bizim **yerel iletici hizmeti** ve gönderilen Application ınsights'ı açın. Kodunuzu önceki adımdan aşağıdaki değiştirin:

    ```python
    from opencensus.trace.tracer import Tracer
    from opencensus.trace import config_integration
    from opencensus.trace.exporters.ocagent import trace_exporter
    from opencensus.trace import tracer as tracer_module
    
    import os
    
    def main():        
        while True:
            valuePrompt()
    
    def valuePrompt():
        export_LocalForwarder = trace_exporter.TraceExporter(
        service_name=os.getenv('SERVICE_NAME', 'python-service'),
        endpoint=os.getenv('OCAGENT_TRACE_EXPORTER_ENDPOINT'))
        
        tracer = Tracer(exporter=export_LocalForwarder)
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)
    
    if __name__ == "__main__":
        main()
    ```

5. Kaydet ve yukarıdaki modülünü çalıştıran deneyin, alabileceğiniz bir `ModuleNotFoundError` için `grpc`. Yüklemek için aşağıdakini çalıştırın bu meydana gelirse [grpcio paket](https://pypi.org/project/grpcio/) ile:

    ```
    python -m pip install grpcio
    ```

6. Artık yukarıda Python betiğini çalıştırdığınızda, yine de değerleri girin istenir, ancak yalnızca değeri yazdırılır Kabuğu'nda şimdi.

7. Onaylamak için **yerel ileticisi** izlemeleri denetimi çekme `LocalForwarder.config` dosya. Adımları izlediyseniz [önkoşul](https://docs.microsoft.com/azure/application-insights/local-forwarder), içinde almayacaktır `C:\LF-WindowsServiceHost`.

    Günlük dosyasının içinde görüntü bir verici burada ekledik, ikinci komut çalıştırılmadan önce gördüğünüz gibi `OpenCensus input BatchesReceived` 0. Güncelleştirilmiş betiği çalıştıran başladıktan sonra `BatchesReceived` girdiğimiz değerlerinin sayısı artan eşit:
    
    ![Yeni App Insights kaynağı formu](./media/opencensus-python/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık Application ınsights'ı yeniden açabilirsiniz **genel bakış** şu anda çalışan uygulamanızın hakkında ayrıntıları görüntülemek için Azure portalında sayfası. Seçin **ölçüm Stream canlı**.

   ![Canlı ölçüm akışı kırmızı kutu içinde seçili genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Başlangıç değerleri girme ve ikinci Python betiğini yeniden çalıştırırsanız, göreceğiniz Canlı İzleme verilerini Application Insights'da yerel ileticisi hizmetinden gelir.

   ![Canlı ölçüm akışı görüntülenen performans verileri ile ekran görüntüsü](./media/opencensus-python/0006-stream.png)

3. Geri gidin **genel bakış** sayfasından seçim yapıp **Uygulama Haritası** çağrı zamanlama uygulama bileşenleriniz arasındaki bağımlılık ilişkileri ve görsel düzeni için.

    ![Temel Uygulama Haritası ekran görüntüsü](./media/opencensus-python/0007-application-map.png)

    Biz yalnızca bir yöntem çağrısının izleme olduğundan, bizim Uygulama Haritası olarak ilginç değil. Ancak, Uygulama Haritası daha dağıtılmış uygulamalar görselleştirmek için ölçeklendirebilirsiniz:

   ![Uygulama Eşlemesi](media/opencensus-python/application-map.png)

4. Seçin **araştırmak performans** ayrıntılı Performans Analizi gerçekleştirebilir ve performansın kök nedenini belirlemek için.

    ![Performans bölmesinin ekran görüntüsü](./media/opencensus-python/0008-performance.png)

5. Seçme **örnekleri** ve ardından sağ bölmede görüntülenen örnekleri hiçbirinde uçtan uca işlem ayrıntıları deneyimi başlayacaktır. Örnek uygulamamız yalnızca tek bir olay gösterecek, ancak daha karmaşık bir uygulama, tek tek olay çağrı yığını düzeyini aşağı uçtan uca işlem keşfetmek çalıştırmasına olanak tanır.

     ![Uçtan uca işlem arabiriminin ekran görüntüsü](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-python"></a>Python için OpenCensus izleme

Yalnızca ele aldığımız kablolama OpenCensus'kurmak Python için Application Insights ve yerel iletici ile ilgili temel kavramları. Resmi kullanım kılavuzu gibi daha gelişmiş konular ele alınmaktadır:

* [Örnekleyici](https://opencensus.io/api/python/trace/usage.html#samplers)
* [Flask tümleştirme](https://opencensus.io/api/python/trace/usage.html#flask)
* [Django tümleştirme](https://opencensus.io/api/python/trace/usage.html#django)
* [MySQL tümleştirme](https://opencensus.io/api/python/trace/usage.html#service-integration)
* [PostgreSQL](https://opencensus.io/api/python/trace/usage.html#postgresql)
  
## <a name="next-steps"></a>Sonraki adımlar

* [OpenCensus Python Kullanım Kılavuzu](https://opencensus.io/api/python/trace/usage.html)
* [Uygulama Haritası](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)
