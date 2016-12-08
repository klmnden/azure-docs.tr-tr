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
ms.date: 11/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: ae5837b4fce52aad4c8b39434c27c450aafc1310
ms.openlocfilehash: 6f42f79abed80df12148463e25935893a4bbdcde


---
# <a name="create-your-first-azure-function"></a>İlk Azure İşlevinizi oluşturma
## <a name="overview"></a>Genel Bakış
Azure İşlevleri, diğer Azure hizmetlerinde, SaaS ürünlerinde ve şirket içi sistemlerde gerçekleşen olaylar tarafından tetiklenen kodları uygulama işlevine sahip var olan Azure uygulaması platformunu genişleten olay denetimli bir istendiğinde işlem deneyimidir. Azure İşlevleri ile uygulamalarınız isteğe göre ölçeklenir ve yalnızca kullandığınız kaynaklar için ödeme yaparsınız. Azure İşlevleri, çeşitli programlama dillerinde uygulanan zamanlanmış veya tetiklenmiş kod birimleri oluşturmanızı sağlar. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konu başlığında, bir HTTP tetikleyicisi tarafından çağrılan basit bir "merhaba dünya" Node.js işlevi oluşturmak için portaldaki Azure İşlevleri hızlı başlangıcının nasıl kullanılacağı gösterilmektedir. Bu adımların portalda nasıl gerçekleştirildiğini görmek için kısa bir video da izleyebilirsiniz.

## <a name="watch-the-video"></a>Videoyu izleme
Aşağıdaki video bu öğreticideki temel adımların nasıl gerçekleştirileceğini gösterir. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Hızlı başlangıçtan bir işlev oluşturma
Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Yeni işlevle bir işlev uygulaması oluşturmak için bu adımları uygulayın. İşlev uygulaması, varsayılan yapılandırma ile oluşturulur. İşlev uygulamanızın açıkça nasıl oluşturulacağına ilişkin bir örnek görmek için bkz. [diğer Azure İşlevleri hızlı başlangıç öğreticisi](functions-create-first-azure-function-azure-portal.md).

İlk işlevinizin oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/).

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın.
2. Yeni işlev uygulamanız için benzersiz bir **Ad** yazın veya oluşturulan adı kabul edin, tercih ettiğiniz **Region (Bölge)** seçeneğini belirleyin ve ardından **Create + get started (Oluştur + başla)** düğmesine tıklayın. 
3. **Hızlı Başlangıç** sekmesinde, **Web Kancası + API** ve **JavaScript**'e tıklayın ve ardından **İşlev oluştur**'a tıklayın. Yeni bir önceden tanımlanmış Node.js işlevi oluşturulur. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (İsteğe bağlı) Hızlı başlangıcın bu noktasında portaldaki Azure İşlevleri özelliklerinin hızlı turuna bakmayı seçebilirsiniz. Turu tamamladıktan veya atladıktan sonra, HTTP tetikleyicisini kullanarak yeni işlevinizi test edebilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme
Azure İşlevleri hızlı başlangıçlarında işlevsel kod olduğundan, yeni işlevinizi hemen test edebilirsiniz.

1. **Develop (Geliştir)** sekmesinde **Code (Kod)** penceresini inceleyin ve bu Node.js kodunun, ileti gövdesinde veya bir sorgu dizesinde geçirilen bir *ad* değerine sahip bir HTTP isteğini beklediğine dikkat edin. İşlev çalıştığında bu değer yanıt iletisinde döndürülür.
   
2. İşleve ait yerleşik HTTP test isteği bölmesini görüntülemek için **Test**’e tıklayın.
 
    ![](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. **Request body (İstek gövdesi)** metin kutusunda *ad* özelliğinin değerini adınızla değiştirin ve **Run (Çalıştır)** düğmesine tıklayın. Bir deneme HTTP isteği tarafından yürütmenin tetiklendiğini, bilgilerin akış günlüklerine yazıldığını ve **Çıktıda** "merhaba" yanıtının görüntülendiğini göreceksiniz.
 
3. Aynı işlevin başka bir tarayıcı penceresi veya sekmesinden yürütülmesini tetiklemek için, **Develop (Geliştir)** sekmesindeki **Function URL (İşlev URL'si)** değerini kopyalayın ve bir tarayıcının adres çubuğuna yapıştırın. URL’ye `&name=yourname` sorgu dizesi değerini ekleyin ve Enter'a basın. Aynı bilgiler günlüklere yazılır ve tarayıcı önceki "hello" yanıtını görüntüler.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıç, temel bir HTTP tetiklemeli işlevin basit bir yürütmesini gösterir. Uygulamalarınızda Azure İşlevleri’ni kullanma hakkında daha fazla bilgi almak için aşağıdaki konulara bakın:

* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  İşlevleri kodlamak ve tetikleyicileri ve bağlamaları tanımlamak için programcı başvurusu
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar. 
* [Azure Uygulama Hizmeti nedir?](../app-service/app-service-value-prop-what-is.md)  
  Azure İşlevleri; dağıtım, ortam değişkenleri ve tanılama gibi temel işlevler için Azure Uygulama Hizmeti platformunu kullanır. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO4-->


