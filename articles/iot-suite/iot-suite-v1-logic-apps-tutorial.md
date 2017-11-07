---
title: Azure IOT paketi ve Logic Apps | Microsoft Docs
description: "Azure IOT paketi için Logic Apps için iş sürecini bağlanacağını konusunda bir öğretici."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: f4457b44c97fadc58406430fc0f31b3e0bac6682
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Öğretici: Mantıksal uygulama, Azure IOT paketi uzaktan önceden yapılandırılmış izleme çözümüne bağlama
[Microsoft Azure IOT paketi] [ lnk-internetofthings] IOT çözümünü exemplifies bir uçtan uca özellik kümesini hızlıca başlamak için harika bir uzaktan izleme çözümü yoludur. Bu öğreticide, mantıksal uygulama, Microsoft Azure IOT paketi Uzaktan izleme çözümü ekleme konusunda açıklanmaktadır. Bu adımlarda, bir iş sürecini bağlanarak daha IOT çözümünüzü nasıl yararlanabileceğinizi gösterilmektedir.

*Önceden yapılandırılmış çözümü Uzaktan izleme sağlama hakkında bir kılavuz arıyorsanız bkz [Öğreticisi: IOT önceden yapılandırılmış çözümleri kullanmaya başlama][lnk-getstarted].*

Bu öğretici başlamadan önce aşağıdakileri yapmanız gerekir:

* Uzaktan izleme önceden yapılandırılmış çözümü Azure aboneliğinize sağlayın.
* İş süreciniz tetikleyen bir e-posta göndermesini sağlamak için bir SendGrid hesabı oluşturun. Ücretsiz bir deneme hesabı için kaydolabilirsiniz [SendGrid](https://sendgrid.com/) tıklayarak **ücretsiz deneyin**. Ücretsiz deneme hesabınız için kaydedilen sonra oluşturmak gereken bir [API anahtarı](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) posta göndermek için izinler veren SendGrid içinde. Öğreticide daha sonra bu API anahtarı gerekir.

Bu öğreticiyi tamamlamak için Visual Studio 2015 veya Visual Studio 2017 önceden yapılandırılmış çözüm arka ucu Eylemler değiştirmeniz gerekir.

Önceden yapılandırılmış Çözüm zaten sağladığınız Uzaktan izleme varsayıldığında, bu çözüm için kaynak grubuna gidin [Azure portal][lnk-azureportal]. Kaynak grubu çözüm adı ile aynı ada sahip seçtiğiniz zaman Uzaktan izleme çözümünüz sağlandı. Kaynak grubu, Azure Klasik Portalı'nda bulabilirsiniz Azure Active Directory Uygulama dışında çözümünüz için tüm sağlanmış Azure kaynaklarını görebilirsiniz. Aşağıdaki ekran görüntüsünde bir örneği gösterir **kaynak grubu** Uzaktan izleme için dikey önceden yapılandırılmış çözüm:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Başlamak için önceden yapılandırılmış çözümü ile kullanmak için mantıksal uygulama ayarlayın.

## <a name="set-up-the-logic-app"></a>Mantıksal uygulama ayarlama
1. Tıklatın **Ekle** Azure Portal'daki kaynak grubu dikey pencerenizi üstünde.
2. Arama **mantıksal uygulama**, onu seçin ve ardından **oluşturma**.
3. Doldurmak **adı** ve aynı **abonelik** ve **kaynak grubu** Uzaktan izleme çözümünüz sağladığında kullanılır. **Oluştur**'a tıklayın.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Dağıtımınız tamamlandığında, mantıksal uygulama kaynağı, kaynak grubu olarak listelendiğini görebilirsiniz.
5. Mantıksal uygulama dikey penceresine, select gitmek için mantıksal uygulamaya tıklayın **boş mantıksal uygulama** açmak için şablon **Logic Apps Tasarımcısı**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Seçin **isteği**. Bu eylem, gelen HTTP isteğiyle belirli bir JSON yükü davranır tetikleyici olarak biçimlendirilmiş belirtir.
7. İstek gövdesi JSON şemaya aşağıdaki kodu yapıştırın:
   
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
   > Mantıksal uygulama kaydedebilir, ancak öncelikle bir eylem eklemeniz gerekir sonra HTTP post için URL kopyalayabilirsiniz.
   > 
   > 
8. Tıklatın **+ yeni adım** el ile tetikleyici altında. Ardından **Eylem Ekle**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Arama **SendGrid - e-postası gönderme** ve tıklatın.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Bağlantı için bir ad girin **SendGridConnection**, girin **SendGrid API anahtarı** SendGrid hesabınızı ayarlama'ı tıklattığınızda, oluşturduğunuz **oluşturma**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Sahip olduğunuz her iki e-posta adreslerini ekleyin **gelen** ve **için** alanları. Ekleme **Uzaktan izleme uyarı [DeviceID]** için **konu** alan. İçinde **e-posta gövdesi** alanında, eklemek **aygıt [DeviceID], [measurementName] [measuredValue] değerle bildirdi.** Ekleyebileceğiniz **[DeviceID]**, **[measurementName]**, ve **[measuredValue]** konumunda öğesini tıklatarak **veri önceki adımları ekleyebilirsiniz** bölümü.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Tıklatın **kaydetmek** üst menüde.
13. Tıklatın **isteği** tetikleyici ve kopyalama **bu URL için Http Post** değeri. Bu URL'yi daha sonra Bu öğreticide gerekir.

> [!NOTE]
> Logic Apps etkinleştirmek, çalıştırmak [farklı türlerde eylem] [ lnk-logic-apps-actions] Office 365'te Eylemler dahil olmak üzere. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>EventProcessor Web işi ayarlayın
Bu bölümde, önceden yapılandırılmış çözümünüzün oluşturduğunuz mantıksal uygulama bağlayın. Bu görevi tamamlamak için bir aygıt algılayıcı değeri bir eşiği aştığında harekete eyleme mantıksal uygulama tetiklemek için URL'sini ekleyin.

1. En son sürümünü kopyalamak için git istemciniz kullanmak [azure-iot-remote-monitoring github deposunu][lnk-rmgithub]. Örneğin:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. Visual Studio'da açın **RemoteMonitoring.sln** depo yerel kopyadan.
3. Açık **ActionRepository.cs** dosyasını **altyapı\\depo** klasör.
4. Güncelleştirme **actionIds** sözlüğü ile **bu URL için Http Post** mantığı uygulamanızdan gibi Not:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Çözümde değişiklikleri kaydetmek ve Visual Studio çıkın.

## <a name="deploy-from-the-command-line"></a>Komut satırından dağıtma
Bu bölümde, Azure'da şu anda çalışan sürümü değiştirmek için Uzaktan izleme çözümü güncelleştirilmiş sürümüne dağıtın.

1. Aşağıdaki [geliştirme Kurulum] [ lnk-devsetup] dağıtım için ortamınızı ayarlamak üzere yönergeler.
2. Yerel olarak dağıtmak için izlemeniz [yerel dağıtım] [ lnk-localdeploy] yönergeler.
3. Buluta dağıtmak ve var olan bulut dağıtımınızı güncelleştirmek için izleyin [bulut dağıtımı] [ lnk-clouddeploy] yönergeler. Dağıtım adı olarak özgün dağıtımınızın adı kullanın. Örneğin özgün dağıtım çağrıldı **demologicapp**, aşağıdaki komutu kullanın:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Yapı komut dosyası çalıştığında, aynı Azure hesabınız, abonelik, bölgeye ve çözüm sağladığında, kullanılan Active Directory örneğini kullandığınızdan emin olun.

## <a name="see-your-logic-app-in-action"></a>Mantıksal uygulamanızı eylem bakın
Önceden yapılandırılmış Uzaktan izleme çözümü bir çözüm sağladığınızda, varsayılan olarak ayarlanan iki kuralları vardır. Her iki kural bulunan **SampleDevice001** aygıt:

* Sıcaklık > 38.00
* Nem > 48.00

Sıcaklık kural Tetikleyicileri **yükseltmek Alarm** eylem ve nem kural Tetikleyicileri **SendMessage** eylem. Her iki eylemler için aynı URL'ye kullanılan varsayılarak **ActionRepository** sınıfı, her iki kural için logic app Tetikleyicileri. E-posta göndermek için SendGrid hem kuralları kullanın **için** uyarı ayrıntılarını adresiyle.

> [!NOTE]
> Mantıksal uygulama eşiğine her zaman tetiklemek devam eder. Gereksiz e-postaları önlemek için çözüm Portalı'nda kuralları devre dışı bırakmak veya mantıksal uygulama devre dışı [Azure portal][lnk-azureportal].
> 
> 

Portalda mantıksal uygulama çalıştığında, e-posta alırken yanı sıra da görebilirsiniz:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Sonraki adımlar
Önceden yapılandırılmış çözümü bir iş sürecini bağlanmak için bir mantıksal uygulama kullandıysanız, önceden yapılandırılmış çözümleri özelleştirme seçenekleri hakkında daha fazla bilgi edinebilirsiniz:

* [Önceden yapılandırılmış Uzaktan izleme çözümü ile dinamik telemetri kullanın][lnk-dynamic]
* [Önceden yapılandırılmış Uzaktan izleme çözümü cihaz bilgileri meta veriler][lnk-devinfo]

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
