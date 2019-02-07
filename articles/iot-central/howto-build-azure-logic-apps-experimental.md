---
title: IOT Central Bağlayıcısı Azure Logic apps'te iş akışları oluşturmak | Microsoft Docs
description: IOT Central Bağlayıcısı'nı Azure Logic Apps'te tetikleyici iş akışlarına ve oluşturmak, güncelleştirmek ve iş akışlarında cihazları silme.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 2aced85828e5563b838af327151b2b94bcafb931
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773762"
---
# <a name="build-workflows-with-the-iot-central-connector-in-azure-logic-apps"></a>IOT Central Bağlayıcısı Azure Logic apps'te iş akışları oluşturun

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Azure Logic Apps, cloud services ve şirket içi sistemler arasında uygulama ve verileri tümleştirin otomatik ölçeklenebilir iş akışlarını oluşturmak için kullanın. Azure Logic Apps, çoğu Azure hizmeti, Microsoft Hizmetleri ve diğer hizmet olarak yazılım-A-(SaaS) ürünleri arasında çok sayıda bağlayıcı vardır. Azure Logic Apps'te IOT Central Bağlayıcısı'nı kullanarak IOT Central içinde bir kuralı tetiklendiğinde iş akışları tetikleyebilirsiniz. Bir cihaz oluşturma, bir cihazın özellikleri ve ayarları güncelleştirin veya bir cihazı silmek için IOT Central Bağlayıcısı eylemleri de kullanabilirsiniz. 

Microsoft Flow IOT Central Bağlayıcısı'nı kullanabilirsiniz. Hem Azure Logic Apps ve Microsoft Flow Integration tasarımcıya öncelik veren, ancak biraz farklı hedef kitlelere hizmetleridir. Flow, ihtiyaç duydukları iş akışlarını oluşturmak için türlü ofis çalışanını güçlendirir. Mantıksal uygulamalar, BT uzmanları, veri bağlanmak için ihtiyaç duydukları tümleştirmeler oluşturmak için güç kazandırır. Flow ve Logic Apps karşılaştırması [burada](https://docs.microsoft.com/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs). IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturma hakkında bilgi edinin [burada](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). 

## <a name="prerequisites"></a>Önkoşullar

- Bir Kullandıkça Öde uygulama
- Bir Azure hesabı ve aboneliği oluşturmak ve mantıksal uygulamaları yönetme

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Bir kuralı tetiklendiğinde bir iş akışı tetikleyicisi

Bu bölümde bir kuralı tetiklendiğinde Microsoft Teams için bir ileti göndermek nasıl gösterir. Akışınıza bir olay, olay hub'ına gönderme gibi şeyler, yeni Azure DevOps iş öğesi oluşturma veya SQL Server'da yeni bir satır eklemek için diğer bağlayıcıları kullanmak için yapılandırabilirsiniz.

1. Başlayın [IOT Central içinde bir kural oluşturma](howto-create-telemetry-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json). Kural koşulları kaydettikten sonra tıklayın **Azure Logic Apps** döşeme yeni bir eylem. Tıklayın **Azure portalında oluşturma**. Yeni bir mantıksal uygulama oluşturmak için Azure portalına ulaşabilirsiniz. Azure hesabınızda oturum açmanız gerekebilir.

1. Yeni bir mantıksal uygulama oluşturmak için gerekli bilgileri girin. Yeni mantıksal uygulamanızı sağlamak için bir Azure aboneliği seçebilirsiniz. IOT Central uygulamanızı oluşturulduğu aynı abonelik yok. **Oluştur**’a tıklayın.

    ![Azure portalında mantıksal uygulama oluşturma](./media/howto-build-azure-logic-apps-experimental/createinazureportal.png)

1. Mantıksal uygulamanızı başarıyla oluşturulduktan sonra Logic Apps Tasarımcısı için otomatik olarak yönlendirilir. Tıklayın **boş mantıksal uygulama**. 

    ![Boş mantıksal uygulama oluşturma](./media/howto-build-azure-logic-apps-experimental/blanklogicapp.png)

1. Tasarımcıda "IOT için merkezi" araması yapın ve seçin **bir kuralı ne zaman tetiklenir** tetikleyici. Bağlayıcı ile IOT Central uygulamanıza hesabı ile oturum açın. 

    ![IOT Central Connector'a oturum](./media/howto-build-azure-logic-apps-experimental/addtrigger.png)

1. Başarılı oturum açma işleminden sonra görünen alanları görmeniz gerekir. Seçin **uygulama** ve **kural** pencerelerden.

    ![Uygulama ve kural Seç](./media/howto-build-azure-logic-apps-experimental/pickappandrule.png)

1. Yeni bir eylem ekleyin. Arama **teams'e ileti gönderin** ve **kanalında bir ileti** Microsoft Teams Bağlayıcıdan. Bağlayıcı Microsoft Teams içinde kullandığınız hesap ile oturum açın. 

1. Eylem seçin **takım** ve **kanal**. Doldurun **ileti** her ileti söylemek istediğiniz alanı. Ekleyebileceğiniz *dinamik içerik* , IOT Central kuraldan boyunca cihaz adı ve zaman damgası gibi önemli bilgiler için bildirim geçirme.
    > [!NOTE]
    > Ölçüm ve kuralını tetikleyen özellik değerlerini almak için dinamik içerik penceresi "daha fazla" metni'ı tıklatın.

    ![Mantıksal uygulama düzenleme eylemi dinamik bölmesini açın](./media/howto-build-azure-logic-apps-experimental/buildworkflow.png)

1. İşiniz bittiğinde eyleminizi düzenleme, tıklayın **Kaydet**.

1. IOT Central uygulamanıza geri dönün, bu kural, bir Azure Logic Apps eylem eylemleri alanında içeriyor. görürsünüz.

Her zaman, Logic Apps Azure Portal'da IOT Central Bağlayıcısı'nı kullanarak bir iş akışı oluşturmaya başlayabilirsiniz. Ardından hangi IOT Central uygulaması ve bağlanmak için hangi kural seçebilir ve yine de aynı şekilde çalışır. IOT Central kuralları sayfasından her başlattığınızda gerek yoktur.

## <a name="create-update-and-delete-a-device-in-a-workflow"></a>Oluşturma, güncelleştirme ve bir iş akışında bir cihazı silme

Bir iş akışı mantıksal uygulamanızı oluştururken, IOT Central Bağlayıcısı'nı kullanarak bir eylem ekleyebilirsiniz. Size **bir cihaz oluşturma**, **bir cihaz güncelleştirmesi**, ve **bir cihazı silmek**.

> [!NOTE]
> İçin **bir cihaz güncelleştirmesi** ve **bir cihazı silmek**, güncelleştirmek veya silmek istediğiniz var olan cihazın Kimliğine ihtiyacınız olacak. IOT Central aygıtın Kimliğini alabilirsiniz **Device Explorer**

![IOT Central cihaz Gezgini cihaz kimliği](./media/howto-build-azure-logic-apps-experimental/iotcdeviceid.png)


## <a name="next-steps"></a>Sonraki adımlar
İş akışlarını oluşturmak için Microsoft Flow kullanmayı öğrendiniz, önerilen sonraki adım olarak [cihazları yönetme](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
