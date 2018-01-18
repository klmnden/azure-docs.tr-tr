---
title: "Uzaktan izleme çözümü - Azure özelleştirme | Microsoft Docs"
description: "Bu makalede önceden yapılandırılmış Uzaktan izleme çözümü için kaynak kodunu nasıl erişebileceğiniz hakkında bilgi sağlar."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/17/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: f5d38091b59110859d4376a5cd16a19f24dad65b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="customize-the-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış Uzaktan izleme çözümü özelleştirme

Bu makale, çözümü önceden yapılandırılmış nasıl kaynak koduna erişim ve Uzaktan izleme özelleştirme hakkında bilgi sağlar. Makalede açıklanır:

* Kaynak kodu ve önceden yapılandırılmış çözümü oluşturan mikro kaynakları içeren GitHub depolarının.
* Yeni bir cihaz ekleme gibi genel özelleştirme senaryolarını yazın.

Aşağıdaki videoda önceden yapılandırılmış Uzaktan izleme çözümü özelleştirme seçeneklerine genel bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

## <a name="project-overview"></a>Proje genel bakış

### <a name="implementations"></a>Uygulamaları

Uzaktan izleme çözümü .NET ve Java uygulamaları vardır. Her iki uygulamaları benzer işlevselliği sağlamak ve aynı temel Azure Hizmetleri'ni kullanır. Üst düzey GitHub depolarının burada bulabilirsiniz:

* [.NET çözümü](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java çözümü](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

### <a name="microservices"></a>Mikro hizmetler

Çözüm belirli bir özellik ilgileniyorsanız, tek tek her mikro hizmet için GitHub depolarının erişebilir. Her mikro hizmet çözümünün işlevselliği farklı bir kısmını uygular. Genel mimarisi hakkında daha fazla bilgi için bkz: [önceden yapılandırılmış çözüm mimarisi Uzaktan izleme](iot-suite-remote-monitoring-sample-walkthrough.md).

Bu tablo her mikro hizmet her dil için geçerli kullanılabilirliğini özetlenmektedir:

<!-- please add links for each of the repos in the table, you can find them here https://github.com/Azure/azure-iot-pcs-team/wiki/Repositories-->

| Microservice      | Açıklama | Java | .NET |
| ----------------- | ----------- | ---- | ---- |
| Web UI            | Uzaktan izleme çözümü için Web uygulaması. UI React.js framework kullanarak uygular. | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) |
| IOT Hub Yöneticisi   | IOT Hub ile iletişim işler.        | [Kullanılabilir](https://github.com/Azure/iothub-manager-java) | [Kullanılabilir](https://github.com/Azure/iothub-manager-dotnet)   |
| Kimlik Doğrulaması    |  Azure Active Directory Tümleştirme yönetir.  | Henüz kullanılamıyor | [Kullanılabilir](https://github.com/Azure/pcs-auth-dotnet)   |
| Cihaz benzetimi | Sanal cihazlar havuzu yönetir. | Henüz kullanılamıyor | [Kullanılabilir](https://github.com/Azure/device-simulation-dotnet)   |
| Telemetri         | Cihaz telemetrisi UI için kullanılabilir hale getirir. | [Kullanılabilir](https://github.com/Azure/device-telemetry-java) | [Kullanılabilir](https://github.com/Azure/device-telemetry-dotnet)   |
| Telemetri Aracısı   | Telemetri akışına çözümler, Azure IOT Hub iletilerden depolar ve tanımlı kurallara göre uyarılar oluşturur.  | [Kullanılabilir](https://github.com/Azure/telemetry-agent-java) | [Kullanılabilir](https://github.com/Azure/telemetry-agent-dotnet)   |
| Kullanıcı Arabirimi yapılandırma         | Yapılandırma verilerini kullanıcı Arabiriminden yönetir. | [Kullanılabilir](https://github.com/azure/pcs-ui-config-java) | [Kullanılabilir](https://github.com/azure/pcs-ui-config-dotnet)   |
| Depolama bağdaştırıcısı   |  Depolama Birimi hizmeti ile etkileşimlerini yönetir.   | [Kullanılabilir](https://github.com/azure/pcs-storage-adapter-java) | [Kullanılabilir](https://github.com/azure/pcs-storage-adapter-dotnet)   |
| Ters proxy     | Özel kaynaklar benzersiz bir uç noktası aracılığıyla yönetilen bir şekilde kullanıma sunar. | Henüz kullanılamıyor | [Kullanılabilir](https://github.com/Azure/reverse-proxy-dotnet)   |

Java çözümü şu anda .NET kimlik doğrulaması, benzetimi ve ters proxy mikro kullanır. Kullanılabilir durumda olduklarında hemen bu mikro Java sürümleri tarafından değiştirilecek.

## <a name="presentation-and-visualization"></a>Sunu ve görselleştirme

Aşağıdaki bölümlerde, Uzaktan izleme çözümü sunu ve görselleştirmeleri katmanda özelleştirmek için seçenekleri açıklanmaktadır:

### <a name="change-the-logo-in-the-ui"></a>Kullanıcı arabiriminde logosunu değiştirme

Varsayılan dağıtım logo ve Contoso şirket adını kullanıcı Arabiriminde kullanır. Şirketinizin adını ve logo görüntülemek için bu kullanıcı Arabirimi öğeleri değiştirmek için:

1. Web kullanıcı arabirimini depo kopyalamak için aşağıdaki komutu kullanın:

    ```cmd/sh
    git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
    ```

1. Şirket adını değiştirmek için açın `src/common/lang.js` dosyasını bir metin düzenleyicisinde.

1. Dosyasında aşağıdaki satırı bulun:

    ```js
    CONTOSO: 'Contoso',
    ```

1. Değiştir `Contoso` şirket adı. Örneğin:

    ```js
    CONTOSO: 'YourCo',
    ```

1. Dosyayı kaydedin.

1. Logo güncelleştirmek için yeni bir SVG dosyasına ekleyin `assets/icons` klasör. Varolan logo durumunda `assets/icons/Contoso.svg` dosya.

1. Açık `src/components/layout/leftNav/leftNav.js` dosyasını bir metin düzenleyicisinde.

1. Dosyasında aşağıdaki satırı bulun:

    ```js
    import ContosoIcon from '../../../assets/icons/Contoso.svg';
    ```

1. Değiştir `Contoso.svg` logosu dosyanızın adı. Örneğin:

    ```js
    import ContosoIcon from '../../../assets/icons/YourCo.svg';
    ```

1. Dosyasında aşağıdaki satırı bulun:

    ```js
    alt="ContosoIcon"
    ```

1. Değiştir `ContosoIcon` ile `alt` metin. Örneğin:

    ```js
    alt="YourCoIcon"
    ```

1. Dosyayı kaydedin.

1. Değişiklikleri test etmek için güncelleştirilmiş çalıştırabilirsiniz `webui` yerel makinenizde. Derleme ve çalıştırma hakkında bilgi edinmek için `webui` yerel olarak çözüm bkz [yapı, çalıştırmak ve yerel olarak test](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/README.md#build-run-and-test-locally) içinde `webui` GitHub depo Benioku dosyası.

1. Değişikliklerinizi dağıtmak için bkz: [Geliştirici Başvurusu Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide).

<!--

### Add a new KPI to the Dashboard page

The following steps describe how to add a new KPI to display on the **Dashboard** page. The new KPI shows information about the number of alarms with specific status values as a pie chart:

1. Step 1

1. Step 2
-->

### <a name="customize-the-map"></a>Harita özelleştirme

Bkz: [Özelleştir harita](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) çözümündeki eşleme bileşenleri ayrıntılarını için github sayfası.

<!--
### Customize the telemetry chart

See the [Customize telemetry chart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of the telemetry chart components in the solution.

### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

### Duplicate an existing control

To duplicate an existing UI element such as a chart or alert, see the [Duplicate a control](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub.

-->

### <a name="other-customization-options"></a>Diğer özelleştirme seçenekleri

Daha fazla Uzaktan izleme çözümü sunu ve görselleştirmeleri katmanda değiştirmek için kod düzenleyebilirsiniz. İlgili GitHub depolarının şunlardır:

* [UIConfig (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [UIConfig (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [Azure bilgisayarları uzaktan izleme WebUI](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="device-connectivity-and-streaming"></a>Cihaz bağlantısı ve akış

Aşağıdaki bölümlerde cihaz bağlantısı ve Uzaktan izleme çözümü akış katmanda özelleştirmek için seçenekleri açıklanmaktadır. [Aygıt modelleri](https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models) cihaz türleri ve telemetri çözümdeki açıklanmaktadır. Sanal ve fiziksel cihazlar için aygıt modelleri kullanın.

Bir fiziksel aygıt uygulama örneği için bkz: [Cihazınızı Uzaktan izleme önceden yapılandırılmış çözümü arasında bağlantı](iot-suite-connecting-devices-node.md).

Kullanıyorsanız bir _fiziksel aygıt_, cihaz meta verilerini ve telemetri belirtimi içeren cihaz modeli ile istemci uygulaması sağlamanız gerekir.

Aşağıdaki bölümlerde, aygıt modelleri ile sanal cihazlar kullanarak açıklanmaktadır:

### <a name="add-a-telemetry-type"></a>Telemetri türü ekleme

Cihaz türleri Contoso demo çözümdeki her cihaz türü gönderdiği telemetri belirtin. Ek telemetri türlerini belirtmek için bir aygıt telemetri tanımları olarak meta veriler çözüme gönderebilir. Bu biçimi kullanıyorsanız, Pano cihaz telemetri ve kullanılabilir yöntemlerin dinamik olarak tüketir ve kullanıcı arabirimini değiştirmenize gerek yoktur. Alternatif olarak, çözümü cihaz tür tanımında değiştirebilirsiniz.

İçinde özel telemetri eklemeyi öğrenmek için _aygıt benzeticisi_ mikro hizmet, bkz: [çözümünüzü sanal cihazlar ile Test](iot-suite-remote-monitoring-test.md).

### <a name="add-a-device-type"></a>Bir aygıt türü ekleme

Contoso demo çözüm bazı örnek cihaz türlerini tanımlar. Çözüm, belirli bir uygulama gereksinimlerinizi karşılamak için özel cihaz türleri tanımlamanızı sağlar. Örneğin, şirketinizin endüstriyel bir ağ geçidi çözüme bağlı birincil cihaz olarak kullanabilir.

Cihazınızı doğru bir temsilini oluşturmak için aygıt gereksinimlerini karşılamak için cihazda çalışan uygulama değiştirmeniz gerekir.

Yeni bir aygıt türü eklemeyi öğrenmek için _aygıt benzeticisi_ mikro hizmet, bkz: [çözümünüzü sanal cihazlar ile Test](iot-suite-remote-monitoring-test.md).

### <a name="define-custom-methods-for-simulated-devices"></a>Sanal cihazlar için özel yöntemler tanımlayın

Sanal cihazlar için özel yöntemler uzaktan izleme çözümünde tanımlamak öğrenmek için bkz: [aygıt modelleri](https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Device-Models) GitHub deposunda.

<!--
#### Using the simulator service

TODO: add steps for the simulator microservice here
-->

#### <a name="using-a-physical-device"></a>Fiziksel bir aygıtı kullanma

Yöntemleri ve işleri fiziksel aygıtlarınızda uygulamak için aşağıdaki IOT hub'ı makalelere bakın:

* [Anlama ve IOT hub'ı doğrudan yöntemleri çağırma](../iot-hub/iot-hub-devguide-direct-methods.md).
* [Zamanlama işlerini birden çok aygıta](../iot-hub/iot-hub-devguide-jobs.md).

### <a name="other-customization-options"></a>Diğer özelleştirme seçenekleri

Daha fazla cihaz bağlantısı ve Uzaktan izleme çözümü akış katmanda değiştirmek için kod düzenleyebilirsiniz. İlgili GitHub depolarının şunlardır:

* [Cihaz Telemetrisi (.NET)](https://github.com/Azure/device-telemetry-dotnet)
* [Cihaz Telemetrisi (Java)](https://github.com/Azure/device-telemetry-java)
* [Telemetri aracısını (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetri aracısını (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

<!--
The following sections describe options to customize the data processing and analytics layer in the remote monitoring solution:

### Rules and actions

See the [Customize rules and actions](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to customize the rules and actions in solution.


### Other customization options
-->

Veri işleme ve analizi Uzaktan izleme çözümü katmanda değiştirmek için kod düzenleyebilirsiniz. İlgili GitHub depolarının şunlardır:

* [Telemetri aracısını (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetri aracısını (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="infrastructure"></a>Altyapı

<!--
The following sections describe options for customizing the infrastructure services in the remote monitoring solution:

### Change storage

The default storage service for the remote monitoring solution is Cosmos DB. See the [Customize storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses.

### Change log storage

The default storage service for logs is Cosmos DB. See the [Customize log storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses for logging.

### Other customization options
-->

Uzaktan izleme çözümü altyapısında değiştirmek için kod düzenleyebilirsiniz. İlgili GitHub depolarının şunlardır:

* [Iothub Yöneticisi'ni (.NET)](https://github.com/Azure/iothub-manager-dotnet)
* [Iothub Yöneticisi'ni (Java)](https://github.com/Azure/iothub-manager-java)
* [Depolama bağdaştırıcısı (.NET)](https://github.com/Azure/pcs-storage-adapter-dotnet)
* [Depolama bağdaştırıcısı (Java)](https://github.com/Azure/pcs-storage-adapter-java)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, önceden yapılandırılmış çözümü özelleştirme yardımcı olmak için kullanılabilir kaynaklar hakkında öğrendiniz.

Önceden yapılandırılmış Uzaktan izleme çözümü hakkında daha fazla kavramsal bilgi için bkz: [uzak mimarisi izleme](iot-suite-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Geliştirici Sorun Giderme Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->