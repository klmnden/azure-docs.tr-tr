---
title: Azure portalında Linux üzerinde bir işlev uygulaması oluşturma | Microsoft Docs
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: glenga
ms.custom: ''
ms.openlocfilehash: cc99bc4345c388f22e72957590f3917a85e214e0
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126638"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Bir işlev uygulaması Azure App Service planı oluşturma

Azure İşlevleri, işlevlerinizi Linux’ta varsayılan bir Azure App Service kapsayıcısında barındırmanıza olanak sağlar. Bu makalede kullanma hakkında bilgi vermektedir [Azure portalında](https://portal.azure.com) çalışan Linux barındırılan bir işlev uygulaması oluşturmak için bir [App Service planı](functions-scale.md#app-service-plan). Ayrıca [kendi özel kapsayıcınızı getirebilirsiniz](functions-create-function-linux-custom-image.md).

![Azure portalında işlev uygulaması oluşturma](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com> sayfasında oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. Bu makalede, işlev uygulamanızı oluştururken bir App Service planı oluşturun.

1. Seçin **kaynak Oluştur** düğmesi bulunan Azure portalında, sol üst köşesinde ardından seçin **işlem** > **işlev uygulaması**.

    ![Azure portalında işlev uygulaması oluşturma](./media/create-function-app-linux-app-service-plan/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/create-function-app-linux-app-service-plan/function-app-create-flow2.png)

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **OS** | Linux | İşlev uygulamasını Linux üzerinde çalışır. |
    | **Yayımlama** | Kod | İçin varsayılan Linux kapsayıcı uygulamanızı **çalışma zamanı yığını** kullanılır. Sağlamak için ihtiyacınız olan işlev uygulaması proje kodunuzu. Özel bir yayımlamak için başka bir seçenektir [Docker görüntüsü](functions-create-function-linux-custom-image.md). |
    | **[Barındırma planı](functions-scale.md)** | App Service planı | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Bir App Service planında çalıştırdığınızda, denetleyebileceğiniz [işlevi uygulamanızı ölçeklendirme](functions-scale.md).  |
    | **App Service planı/konumu** | Plan oluşturma | Seçin **Yeni Oluştur** ve tedarik bir **App Service planı** adı. Seçin bir **konumu** içinde bir [bölge](https://azure.microsoft.com/regions/) işlevlerinizi yakınınızdaki veya erişeceği diğer hizmetlere erişim. Öğeleri için istediğiniz değerleri seçin  **[fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/linux/)**. <br/>Hem Linux hem de Windows işlev uygulamaları aynı App Service planında çalıştıramazsınız. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. [Python desteği](functions-reference-python.md) şu anda Önizleme aşamasındadır. |
    | **[Depolama](../storage/common/storage-quickstart-create-account.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Dilerseniz [depolama hesabı gereksinimlerini](functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz. |
    | **[Application Insights](functions-monitoring.md)** | Enabled | Application Insights varsayılan olarak devre dışıdır. Artık Application Insights tümleştirmesi etkinleştiriliyor ve, App Service planı konumu yakın barındırma konumu seçme öneririz. Bunu daha sonra yapmak istiyorsanız, bkz. [İzleyici Azure işlevleri](functions-monitoring.md).  |

3. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

4. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/create-function-app-linux-app-service-plan/function-app-create-notification.png)

5. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin.

> [!TIP]
> Portalda işlev uygulamalarınızı bulma konusunda sorun yaşıyorsanız, [Azure portalında İşlev Uygulamalarını sık kullanılanlarınıza eklemeyi](functions-how-to-use-azure-function-app-settings.md#favorite) deneyin.

Ardından, yeni işlev uygulamasında bir işlev oluşturun. İşlev uygulamanızı bile kullanılabilir olduktan sonra tam olarak başlatılması birkaç dakika sürebilir.

## <a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

Bu bölümde bir işlev portalında yeni işlev uygulamanızı oluşturmak nasıl gösterir.

> [!NOTE]
> Portal geliştirme deneyimi için Azure işlevlerini çalışırken yararlı olabilir. Çoğu senaryo için işlevleri yerel olarak geliştirme ve işlev uygulamanızı kullanarak proje yayımlama göz önünde bulundurun [Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) veya [Azure işlevleri çekirdek Araçları](functions-run-local.md#create-a-local-functions-project).  

1. Yeni işlev uygulamanızda seçin **genel bakış** sekmesini tıklatın ve tamamen yüklendikten sonra seçin **+ yeni işlev**.

    ![Genel Bakış sekmesinde yeni bir işlev oluşturma](./media/create-function-app-linux-app-service-plan/overview-create-function.png)

1. İçinde **hızlı** sekmesini, **portal**seçip **devam**.

    ![İşlev geliştirme platformunuzu seçin.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-choose-portal.png)

1. **Web Kancası + API**'yi ve ardından **Oluştur**'u seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-node-webhook.png)

Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde sağ üst kısımdaki **</> İşlev URL'sini al**'a tıklayın, **varsayılan (İşlev anahtarı)** seçeneğini belirleyin ve ardından **Kopyala**'ya tıklayın. 

    ![Azure portalından işlev URL’sini kopyalama](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

2. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesi değerini bu URL’nin sonuna ekleyin ve isteği yürütmek için klavyenizdeki `Enter` tuşuna basın. İşlev tarafından döndürülen yanıtın tarayıcıda gösterildiğini görürsünüz.  

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

3. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütme işleminden alınan izleme çıktısını görmek için, portalda işlevinize geri dönün ve ekranın altındaki oka tıklayarak **Günlükler**’i genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak işlev uygulaması oluşturdunuz.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Daha fazla bilgi için bkz. [Azure İşlevleri HTTP bağlamaları](functions-bindings-http-webhook.md).
