---
title: Öğrenciler için Azure'ı kullanarak bir işlev oluşturma | Microsoft Docs
description: Azure Öğrenci başlangıç aboneliği içinde bir Azure işlevi oluşturma hakkında bilgi edinin
Customer intent: As a student, I want to be able to create a HTTP triggered Function App within the Student Starter plan so that I can easily add APIs to any project.
services: functions
documentationcenter: na
author: alexkarcher-msft
manager: ggailey777
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 02/22/2019
ms.author: alkarche
ms.openlocfilehash: 860fedb13e84054e8ba264116be4e452445b7e9b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143091"
---
# <a name="create-a-function-using-azure-for-students-starter"></a>Öğrenciler için Azure'ı kullanarak bir işlev oluşturma

Bu öğreticide, Azure Öğrenci başlangıç aboneliği için bir hello world HTTP işlevde oluşturacağız. Ayrıca bu abonelik türü Azure işlevleri'nde kullanılabilir aracılığıyla alacağız.

Microsoft *Öğrenciler için Azure başlangıç* , size hiçbir ücret ödemeden bulutta geliştirmeniz gereken Azure ürünlerini kullanmaya başlamanızı sağlar. [Burada Bu teklif hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/offers/ms-azr-0144p/)

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda yürütmenize olanak tanır. [Burada işlevleri hakkında bilgi edinin.](./functions-overview.md)

## <a name="create-a-function"></a>Bir işlev oluşturma

 Bu konu başlığında, Azure portalında bir "Merhaba Dünya" HTTP ile tetiklenen işlev oluşturma için işlevleri kullanmayı öğrenin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-student-starter/function-app-in-portal-editor.png)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com> sayfasında oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. 

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın, ardından **İşlem** > **İşlev Uygulaması** seçeneğini belirleyin.

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-create-student-starter/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    <img src="./media/functions-create-student-starter/Function-create-start.png" width="315">

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
   | **[App Service planı/konumu](./functions-scale.md)** | Yeni | Barındırma planı denetleyen işlev uygulamanız için dağıtılan hangi bölge ve kaynaklarınızın yoğunluğu. Birden fazla işlev uygulaması aynı planı olarak dağıtılan tüm ücretsiz aynı tek örnek paylaşır. Öğrenci başlangıç planın bir sınırlama budur. Tam barındırma seçenekleri [burada açıklanmıştır.](./functions-scale.md)|
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. |
    |**[Application Insights](./functions-monitoring.md)**| Enabled | Application Insights, işlev uygulamanızın günlüklerini çözümlemek ve depolamak için kullanılır. Application Insights'ı destekleyen bir konuma seçerseniz, varsayılan olarak etkindir. Application Insights herhangi bir işlev için el ile yakın bir bölge seçerek Application Insights'ı dağıtmak için etkinleştirilebilir. Application Insights, yalnızca canlı akış günlüklerini görüntülemek mümkün olmayacak.

3. Seçin **App Service planı/konumu** üst farklı bir konum seçin

4. Seçin **Yeni Oluştur** ve ardından planınızı benzersiz bir ad verin.

5. Size en yakın konumu seçin. [Burada Azure bölgelerinin tam bir Haritası'na bakın.](https://azure.microsoft.com/global-infrastructure/regions/) 

    <img src="./media/functions-create-student-starter/Create-ASP.png" width="800">

6. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

    <img src="./media/functions-create-student-starter/Function-create-end.png" width="315">

7. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-student-starter/function-app-create-notification.png)

8. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin.

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

1. Yeni işlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesini, **Portalda**'yı ve **Devam**'ı seçin.

    ![İşlevler hızlı başlangıcı platform seçimi.](./media/functions-create-student-starter/function-app-quickstart-choose-portal.png)

1. **Web Kancası + API**'yi ve ardından **Oluştur**'u seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/functions-create-student-starter/function-app-quickstart-node-webhook.png)

Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde sağ üst kısımdaki **</> İşlev URL'sini al**'a tıklayın, **varsayılan (İşlev anahtarı)** seçeneğini belirleyin ve ardından **Kopyala**'ya tıklayın. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-student-starter/function-app-develop-tab-testing.png)

2. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesi değerini bu URL’nin sonuna ekleyin ve isteği yürütmek için klavyenizdeki `Enter` tuşuna basın. İşlev tarafından döndürülen yanıtın tarayıcıda gösterildiğini görürsünüz.  

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-student-starter/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

3. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütme işleminden alınan izleme çıktısını görmek için, portalda işlevinize geri dönün ve ekranın altındaki oka tıklayarak **Günlükler**’i genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-student-starter/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="supported-features-in-azure-for-students-starter"></a>Öğrenciler için Azure üzerinde desteklenen özellikler

Azure Öğrenci başlangıç için Azure işlevleri çalışma zamanı özelliklerinin çoğu aşağıda listelenen birkaç anahtar kısıtlamalarla erişimi vardır:

* HTTP tetikleyicisi, desteklenen tek tetikleyici türüdür.
    * Tüm giriş ve bağlamaları desteklenen tüm çıktı! [Burada tam listesine bakın.](functions-triggers-bindings.md)
* Desteklenen diller: 
    * C# (.NET Core 2)
    * JavaScript (8 ve 10 Node.js)
    * F# (.NET Core 2)
    * [Burada daha yüksek planlarında desteklenen dilleri bakın](supported-languages.md)
* Windows tek desteklenen işletim sistemi ' dir.
* Ölçek sınırlı olduğundan [bir ücretsiz katmanı örneği](https://azure.microsoft.com/pricing/details/app-service/windows/) 60 dakika her gün çalışan. Otomatik olarak HTTP trafiğini alındı, ancak başka 0 ile 1 örneğine serverlessly ölçeklenir.
* Yalnızca [2.x çalışma zamanı](functions-versions.md) desteklenir.
* Tüm geliştirici araçları, düzenleme ve işlevleri yayımlama için desteklenmiyor. Bu, VS Code, Visual Studio, Azure CLI ve Azure portalı içerir. Portal dışında herhangi bir şey kullanmak istiyorsanız, önce portalda bir uygulama oluşturun ve ardından bu uygulama, tercih edilen aracında dağıtım hedefi olarak gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP ile tetiklenen işlevi bir işlev uygulaması oluşturdunuz! Şimdi yerel araç kullanımı, daha fazla dil, izleme ve tümleştirmeler keşfedebilirsiniz.

 * [Visual Studio kullanarak ilk işlevinizi oluşturma](./functions-create-your-first-function-visual-studio.md)
 * [Visual Studio Code kullanarak ilk işlevinizi oluşturma](./functions-create-first-function-vs-code.md)
 * [Azure işlevleri JavaScript Geliştirici Kılavuzu](./functions-reference-node.md)
 * [Bir Azure SQL veritabanı'na bağlanmak için Azure işlevleri'ni kullanın](./functions-scenario-database-table-cleanup.md)
 * [Azure işlevleri HTTP bağlama hakkında daha fazla bilgi](./functions-bindings-http-webhook.md).
 * [Azure işlevlerini izleme](./functions-monitoring.md)
