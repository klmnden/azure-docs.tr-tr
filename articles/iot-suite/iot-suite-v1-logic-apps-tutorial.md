---
title: Azure IOT paketi ve Logic Apps | Microsoft Docs
description: Azure IOT Suite için Logic Apps iş süreci için bağlama hakkında bir öğretici.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 4a1db86f4b715533dfea545365eaf66de0574c5e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47107053"
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Öğretici: Mantıksal uygulama için Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümünüzü bağlayın.
[Microsoft Azure IOT paketi] [ lnk-internetofthings] Uzaktan izleme çözümü bir IOT çözümü exemplifies bir uçtan uca özellik kümesiyle hızlı bir şekilde kullanmaya başlamak için harika bir yoludur. Bu öğretici mantıksal uygulama, Microsoft Azure IOT paketi Uzaktan izleme çözümü ekleme konusunda size kılavuzluk eder. İş işleme bağlanarak daha IOT çözümünüzü nasıl alabilir adımları gösterilmektedir.

*Uzaktan izleme çözümüne sağlama hakkında kılavuz arıyorsanız bkz [Öğreticisi: IOT önceden yapılandırılmış çözümleri kullanmaya başlama][lnk-getstarted].*

Bu öğreticiye başlamadan önce şunları yapmalısınız:

* Uzaktan izleme önceden yapılandırılmış çözümü, Azure aboneliğinizdeki sağlayın.
* Sağlamak iş sürecinizin tetikleyen bir e-posta göndermek SendGrid hesabı oluşturun. Ücretsiz bir deneme hesabı için kaydolabilirsiniz [SendGrid](https://sendgrid.com/) tıklayarak **ücretsiz olarak deneyin**. Oluşturmanıza gerek için ücretsiz deneme hesabınızı kaydettikten sonra bir [API anahtarı](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) posta göndermek için izinler veren SendGrid içinde. Öğreticinin ilerleyen bölümlerinde bu API anahtarı gerekir.

Bu öğreticiyi tamamlamak için Visual Studio 2015 veya Visual Studio 2017 önceden yapılandırılmış çözüm arka ucu eylemleri değiştirmeniz gerekir.

Varsayılarak zaten sağladığınız Uzaktan izleme önceden yapılandırılmış çözüm, bu çözüm için kaynak grubuna gidin [Azure portalında][lnk-azureportal]. Kaynak grubu çözüm adı aynı ada sahip, seçtiğiniz zaman Uzaktan izleme çözümünüzü sağladığınız. Kaynak grubunda çözümünüz için sağlanan tüm Azure kaynaklarını görebilirsiniz. Aşağıdaki anlık görüntüde bir örnek gösterilmektedir **kaynak grubu** dikey penceresi, Uzaktan izleme önceden yapılandırılmış çözümü:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Başlamak için önceden yapılandırılmış çözüm ile kullanılacak mantıksal uygulama oluşturun.

## <a name="set-up-the-logic-app"></a>Mantıksal uygulamasını ayarlama
1. Tıklayın **Ekle** Azure portalında kaynak grubu dikey penceresinin üstünde.
2. Arama **mantıksal uygulama**, onu seçin ve ardından **Oluştur**.
3. Doldurun **adı** ve aynı **abonelik** ve **kaynak grubu** , Uzaktan izleme çözümünüzü sağlarken kullanılan. **Oluştur**’a tıklayın.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Dağıtım tamamlandığında, mantıksal uygulama, kaynak grubunuzda bir kaynak olarak listelenip listelenmediğini görebilirsiniz.
5. Mantıksal uygulama dikey penceresine, select gitmek için mantıksal uygulamaya tıklayın **boş mantıksal uygulama** açmak için şablon **Logic Apps Tasarımcısı'nda**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Seçin **istek**. Bu eylem, gelen HTTP isteği ile belirli bir JSON yükü davranır tetikleyici olarak biçimlendirilmiş belirtir.
7. İstek gövdesi JSON Şeması aşağıdaki kodu yapıştırın:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Sonra mantıksal uygulamayı kaydedin, ancak önce bir eylem eklemeniz gerekir, HTTP post için URL'yi kopyalayabilirsiniz.
   > 
   > 
8. Tıklayın **+ yeni adım** el ile tetikleyici altında. Ardından **Eylem Ekle**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Arama **SendGrid - e-postası gönderme** bulup tıklayın.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Bağlantı için bir ad girmeniz **SendGridConnection**, girin **SendGrid API anahtarı** SendGrid hesabınızı ayarlama ve'a tıklayın, oluşturduğunuz **Oluştur**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Sahip olduğunuz her iki e-posta adreslerini ekleme **gelen** ve **için** alanları. Ekleme **Uzaktan izleme uyarısı [cihaz kimliği]** için **konu** alan. İçinde **e-posta gövdesi** Ekle, alan **cihaz [cihaz kimliği], [measurementName] [measuredValue] değeriyle bildirdi.** Ekleyebileceğiniz **[cihaz kimliği]**, **[measurementName]**, ve **[measuredValue]** içinde tıklayarak **önceki adımlardan veri ekleyebilirsiniz** bölümü.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Tıklayın **Kaydet** üst menüdeki.
13. Tıklayın **istek** tetikleyici ve kopyalama **bu URL için Http Post** değeri. Daha sonra Bu öğreticide bu URL'ye ihtiyacınız var.

> [!NOTE]
> Logic Apps çalıştırmanıza olanak [farklı türlerde eylem] [ lnk-logic-apps-actions] Office 365'te Eylemler dahil olmak üzere. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>EventProcessor Web işi ayarlama
Bu bölümde, oluşturduğunuz mantıksal uygulamanızı önceden yapılandırılmış çözümünüzü bağlayın. Bu görevi tamamlamak için mantıksal uygulama bir cihaz sensörü değeri bir eşiği aştığında tetiklenen eylemi tetiklemesine URL'sini ekleyin.

1. En son sürümünü kopyalamak için git istemci kullanmak [azure-IOT-remote-monitoring github deposu][lnk-rmgithub]. Örneğin:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. Visual Studio'da açın **RemoteMonitoring.sln** deposunun yerel kopyasındaki.
3. Açık **ActionRepository.cs** dosyası **altyapı\\depo** klasör.
4. Güncelleştirme **actionIds** sözlük ile **bu URL için Http Post** gibi mantıksal uygulamanızdan Not:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Çözümdeki değişiklikleri kaydetmek ve Visual Studio çıkın.

## <a name="deploy-from-the-command-line"></a>Komut satırından dağıtma
Bu bölümde, Azure'da şu anda çalışan sürümünü değiştirmek için Uzaktan izleme çözümü, güncelleştirilmiş sürümünü dağıtın.

1. Aşağıdaki [geliştirme Kurulum] [ lnk-devsetup] dağıtımı için ortamınızı kurma konusunda yönergeler.
2. Yerel olarak dağıtmak için izlemeniz [yerel dağıtım] [ lnk-localdeploy] yönergeleri.
3. Buluta dağıtın ve mevcut bulut dağıtımınızı güncelleştirmek için izleyin [bulut dağıtımı] [ lnk-clouddeploy] yönergeleri. Dağıtım adı özgün dağıtımınız adını kullanın. Örneğin özgün dağıtım çağrıldı **demologicapp**, aşağıdaki komutu kullanın:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Yapı komut dosyası çalıştığında, aynı Azure hesabı, abonelik, bölge ve Active Directory örneğine çözümü sağladığınız yapılandırırken kullandığınız kullandığınızdan emin olun.

## <a name="see-your-logic-app-in-action"></a>Mantıksal uygulamanızı çalışır halde bakın
Önceden yapılandırılmış Uzaktan izleme çözümüne bir çözüm sağladığınızda, varsayılan olarak ayarlanan iki kurallara sahiptir. Hem kuralları bulunan **SampleDevice001** cihaz:

* Sıcaklık > 38.00
* Nem > 48.00

Sıcaklık kural tetiklendiğinde **yükseltmek Alarm** eylem ve nem kural Tetikleyicileri **SendMessage** eylem. İçin her iki eylem aynı URL'yi kullanılan varsayılarak **ActionRepository** ya da kural için mantıksal uygulama Tetikleyicileri sınıfı. Hem kuralları, bir e-posta göndermek için SendGrid kullanın. **için** uyarının ayrıntılarını adresiyle.

> [!NOTE]
> Mantıksal uygulama eşiğine her zaman tetikleme devam ediyor. Gereksiz e-postaları önlemek için çözüm portalında kurallar devre dışı bırakın veya mantıksal uygulamayı devre dışı [Azure portalında][lnk-azureportal].
> 
> 

Portalda mantıksal uygulama çalıştığında, e-posta alırken yanı sıra de görebilirsiniz:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Sonraki adımlar
Mantıksal uygulama iş süreci için önceden yapılandırılmış çözümü bağlanmak için kullandığınız, önceden yapılandırılmış çözümleri özelleştirme seçenekleri hakkında daha fazla bilgi edinebilirsiniz:

* [Uzaktan izleme önceden yapılandırılmış çözümüyle dinamik telemetri kullanma][lnk-dynamic]
* [Cihaz önceden yapılandırılmış Uzaktan izleme çözümünü bilgi meta veriler][lnk-devinfo]

[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
