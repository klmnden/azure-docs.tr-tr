---
title: "İlk Azure İşlevinizi oluşturma | Microsoft Belgeleri"
description: "İki dakikadan kısa bir süre içinde sunucusuz bir uygulama olan ilk Azure İşlevinizi oluşturun."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/09/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 7743bfb98552f7fa2334d8daa6a6f6935969f393
ms.openlocfilehash: d76630e0be4a021720d88e6d7e64cf2258f84266


---
# <a name="create-your-first-azure-function"></a>İlk Azure İşlevinizi oluşturma
## <a name="overview"></a>Genel Bakış
Azure İşlevleri, diğer Azure hizmetlerinde, SaaS ürünlerinde ve şirket içi sistemlerde gerçekleşen olaylar tarafından tetiklenen kodları uygulama işlevine sahip var olan Azure uygulaması platformunu genişleten olay denetimli bir istendiğinde işlem deneyimidir. Azure İşlevleri ile uygulamalarınız isteğe göre ölçeklenir ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Azure İşlevleri, çeşitli programlama dillerinde uygulanan zamanlanmış veya tetiklenmiş kod birimleri oluşturmanızı sağlar. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konu başlığında, bir HTTP tetikleyicisi tarafından çağrılan basit bir "merhaba dünya" JavaScript işlevi oluşturmak için portaldaki Azure İşlevleri hızlı başlangıcının nasıl kullanılacağı gösterilmektedir. Bu adımların portalda nasıl gerçekleştirildiğini görmek için kısa bir video da izleyebilirsiniz.

## <a name="watch-the-video"></a>Videoyu izleme
Aşağıdaki video bu öğreticideki temel adımların nasıl gerçekleştirileceğini gösterir. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Hızlı başlangıçtan bir işlev oluşturma
Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Yeni işlevle bir işlev uygulaması oluşturmak için bu adımları uygulayın. İşlev uygulaması, varsayılan yapılandırma ile oluşturulur. İşlev uygulamanızın açıkça nasıl oluşturulacağına ilişkin bir örnek görmek için bkz. [diğer Azure İşlevleri hızlı başlangıç öğreticisi](functions-create-first-azure-function-azure-portal.md).

İlk işlevinizin oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/).

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın.
2. Yeni işlev uygulamanız için benzersiz bir **Ad** yazın veya otomatik olarak oluşturulan adı kabul edin, tercih ettiğiniz **Region (Bölge)** seçeneğini belirleyin ve ardından **Create + get started (Oluştur + başla)** düğmesine tıklayın. Yalnızca harf, rakam ve tire içeren geçerli bir ad girmeniz gerektiğini unutmayın. Alt çizgi (**_**) izin verilen bir karakter değildir.
3. **Hızlı Başlangıç** sekmesinde, **Web Kancası + API** ve **JavaScript**'e tıklayın ve ardından **İşlev oluştur**'a tıklayın. Yeni bir önceden tanımlanmış JavaScript işlevi oluşturulur. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (İsteğe bağlı) Hızlı başlangıcın bu noktasında portaldaki Azure İşlevleri özelliklerinin hızlı turuna bakmayı seçebilirsiniz. Turu tamamladıktan veya atladıktan sonra, HTTP tetikleyicisini kullanarak yeni işlevinizi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO2-->


