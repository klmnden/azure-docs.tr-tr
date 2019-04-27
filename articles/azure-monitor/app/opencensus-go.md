---
title: OpenCensus Git Azure Application Insights ile izleme | Microsoft Docs
description: Yerel ileticisi ve Application Insights ile izleme Git OpenCensus tümleştirmek için yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/15/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cdf01fbbcc8ef1f90b2e0f8973f59c46c5bf70f8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60577907"
---
# <a name="collect-distributed-traces-from-go-preview"></a>Toplama dağıtılmış izlemelerinden Git (Önizleme)

Go uygulamaları ile tümleştirme yoluyla izlemeyi destekleyen dağıtılmış artık application Insights [OpenCensus](https://opencensus.io) ve yeni [yerel ileticisi](./opencensus-local-forwarder.md). Bu makalede, Go için OpenCensus ayarlama ve Application Insights izleme verilerinize alma sürecinde adım adım yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Aboneliğine sahip olmanız gerekir.
- Git yüklenmesi, bu makalede 1.11 sürümü kullanan [Git indirme](https://golang.org/dl/).
- Yüklemek için yönergeleri izleyin [yerel iletici bir Windows hizmeti olarak](./opencensus-local-forwarder.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-application-insights-resource"></a>Application Insights kaynağı oluştur

Öncelikle, bir izleme anahtarını (ikey) oluşturacak bir Application Insights kaynağı oluşturmanız gerekir. İkey sonra Application Insights için izleme eklenmiş OpenCensus uygulamanızdan dağıtılmış izlemeleri göndermek için yerel ileticisi yapılandırmak için kullanılır.   

1. Seçin **kaynak Oluştur** > **Geliştirici Araçları** > **Application Insights**.

   ![Application Insights Kaynağı ekleme](./media/opencensus-Go/0001-create-resource.png)

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

   ![İzleme anahtarı ekran görüntüsü](./media/opencensus-Go/0003-instrumentation-key.png)

2. Düzenleme, `LocalForwarder.config` dosya ve izleme anahtarınızı ekleyin. İçindeki yönergeleri izlediyseniz [önkoşul](./opencensus-local-forwarder.md) dosyası şu konumdadır `C:\LF-WindowsServiceHost`

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

## <a name="opencensus-go-packages"></a>OpenCensus Git paketleri

1. Komut satırından Git için açık Görselleştirmenizdeki paketleri yükleyin:

    ```go
    go get -u go.opencensus.io
    go get -u contrib.go.opencensus.io/exporter/ocagent
    ```

2. .Go dosyaya aşağıdaki kodu ekleyin ve ardından derleyin ve çalıştırın. (Bu örnekte yerel iletici ile tümleştirme kolaylaştıran eklenen kodu resmi OpenCensus yönergeleriyle türetilir)

     ```go
        // Copyright 2018, OpenCensus Authors
        //
        // Licensed under the Apache License, Version 2.0 (the "License");
        // you may not use this file except in compliance with the License.
        // You may obtain a copy of the License at
        //
        //     https://www.apache.org/licenses/LICENSE-2.0
        //
        // Unless required by applicable law or agreed to in writing, software
        // distributed under the License is distributed on an "AS IS" BASIS,
        // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        // See the License for the specific language governing permissions and
        // limitations under the License.
        package main
        
        import (
        
            "bytes"
            "fmt"
            "log"
            "net/http"
            os "os"
            
            ocagent "contrib.go.opencensus.io/exporter/ocagent"
            "go.opencensus.io/plugin/ochttp"
            "go.opencensus.io/plugin/ochttp/propagation/tracecontext"
            "go.opencensus.io/trace"
        
        )
        
        func main() {
            // Register stats and trace exporters to export the collected data.
            serviceName := os.Getenv("SERVICE_NAME")
            if len(serviceName) == 0 {
                serviceName = "go-app"
            }
            fmt.Printf(serviceName)
            agentEndpoint := os.Getenv("OCAGENT_TRACE_EXPORTER_ENDPOINT")

            if len(agentEndpoint) == 0 {
                agentEndpoint = fmt.Sprintf("%s:%d", ocagent.DefaultAgentHost, ocagent.DefaultAgentPort)
            }
        
            fmt.Printf(agentEndpoint)
            exporter, err := ocagent.NewExporter(ocagent.WithInsecure(), ocagent.WithServiceName(serviceName), ocagent.WithAddress(agentEndpoint))
        
            if err != nil {
                log.Printf("Failed to create the agent exporter: %v", err)
            }
        
            trace.RegisterExporter(exporter)
        
            trace.ApplyConfig(trace.Config{DefaultSampler: trace.AlwaysSample()})
        
            client := &http.Client{Transport: &ochttp.Transport{Propagation: &tracecontext.HTTPFormat{}}}
        
            http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                var jsonStr = []byte(`[ { "url": "http://blank.org", "arguments": [] } ]`)
                r, _ := http.NewRequest("POST", "http://blank.org", bytes.NewBuffer(jsonStr))
                r.Header.Set("Content-Type", "application/json")
        
                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }
            })
        
            http.HandleFunc("/call_blank", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                r, _ := http.NewRequest("GET", "http://blank.org", nil)

                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
        
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }        
            })
        
            log.Fatal(http.ListenAndServe(":50030", &ochttp.Handler{Propagation: &tracecontext.HTTPFormat{}}))
        
        }
     ```

3. Basit bir go uygulaması çalıştıran bir kez gidin `http://localhost:50030`. Her yenileme tarayıcı "yerel ileticisi tarafından devralındığında karşılık gelen bir aralık veri eşlik metin hello world" oluşturur.

4. Onaylamak için **yerel ileticisi** izlemeleri denetimi çekme `LocalForwarder.config` dosya. Adımları izlediyseniz [önkoşul](https://docs.microsoft.com/azure/application-insights/local-forwarder), içinde almayacaktır `C:\LF-WindowsServiceHost`.

    Günlük dosyasının içinde görüntü bir verici burada ekledik, ikinci komut çalıştırılmadan önce gördüğünüz gibi `OpenCensus input BatchesReceived` 0. Güncelleştirilmiş betiği çalıştıran başladıktan sonra `BatchesReceived` girdiğimiz değerlerinin sayısı artan eşit:
    
    ![Yeni App Insights kaynağı formu](./media/opencensus-go/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık Application ınsights'ı yeniden açabilirsiniz **genel bakış** şu anda çalışan uygulamanızın hakkında ayrıntıları görüntülemek için Azure portalında sayfası. Seçin **ölçüm Stream canlı**.

   ![Canlı ölçüm akışı kırmızı kutu içinde seçili genel bakış bölmesinin ekran görüntüsü](./media/opencensus-go/0005-overview-live-metrics-stream.png)

2. İkinci bir Go uygulaması'nı yeniden çalıştırın ve başlatmak için tarayıcıyı yenilemeyi `http://localhost:50030`, göreceğiniz Canlı İzleme verilerini Application Insights'da yerel ileticisi hizmetinden gelir.

   ![Canlı ölçüm akışı görüntülenen performans verileri ile ekran görüntüsü](./media/opencensus-go/0006-stream.png)

3. Geri gidin **genel bakış** sayfasından seçim yapıp **Uygulama Haritası** çağrı zamanlama uygulama bileşenleriniz arasındaki bağımlılık ilişkileri ve görsel düzeni için.

    ![Temel Uygulama Haritası ekran görüntüsü](./media/opencensus-go/0007-application-map.png)

    Biz yalnızca bir yöntem çağrısının izleme olduğundan, bizim Uygulama Haritası olarak ilginç değil. Ancak, Uygulama Haritası daha dağıtılmış uygulamalar görselleştirmek için ölçeklendirebilirsiniz:

   ![Uygulama Eşlemesi](media/opencensus-go/application-map.png)

4. Seçin **araştırmak performans** ayrıntılı Performans Analizi gerçekleştirebilir ve performansın kök nedenini belirlemek için.

    ![Performans bölmesinin ekran görüntüsü](./media/opencensus-go/0008-performance.png)

5. Seçme **örnekleri** ve ardından sağ bölmede görüntülenen örnekleri hiçbirinde uçtan uca işlem ayrıntıları deneyimi başlayacaktır. Örnek uygulamamız yalnızca tek bir olay gösterecek, ancak daha karmaşık bir uygulama, tek tek olay çağrı yığını düzeyini aşağı uçtan uca işlem keşfetmek çalıştırmasına olanak tanır.

     ![Uçtan uca işlem arabiriminin ekran görüntüsü](./media/opencensus-go/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-go"></a>Go için OpenCensus izleme

Yalnızca ele aldığımız OpenCensus Go için Application Insights ve yerel iletici ile tümleştirmeye ilişkin temel bilgileri. [Resmi OpenCensus Git Kullanım Kılavuzu](https://godoc.org/go.opencensus.io) daha gelişmiş konuları kapsar.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Haritası](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)
