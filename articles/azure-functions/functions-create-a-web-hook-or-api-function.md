---
title: "Bir web kancası veya API Azure İşlevi oluşturma | Microsoft Belgeleri"
description: "Bir Web Kancası veya API çağrısı ile çağrılan işlev oluşturmak için Azure İşlevlerini kullanın."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/30/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9484a637a1876f2cae644e43986bd5ef3201da1e


---
# <a name="create-a-webhook-or-api-azure-function"></a>Bir web kancası veya API Azure İşlevi oluşturma
Azure İşlevleri, çeşitli programlama dillerinde uygulanan zamanlanan veya tetiklenen kod birimleri oluşturmanızı sağlayan olay odaklı, isteğe bağlı bir hesaplama deneyimidir. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konuda bir GitHub web kancası tarafından çağrılan yeni bir Node.js işlevinin nasıl oluşturulacağı gösterilmektedir. Yeni işlev, Azure İşlevleri portalında önceden tanımlanmış bir şablon temel alınarak oluşturulur. Bu adımların portalda nasıl gerçekleştirildiğini görmek için kısa bir video da izleyebilirsiniz.

## <a name="watch-the-video"></a>Videoyu izleme
Aşağıdaki video bu öğreticideki temel adımların nasıl gerçekleştirileceğini gösterir 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="create-a-webhooktriggered-function-from-the-template"></a>Şablondan web kancası ile tetiklenen bir işlev oluşturma
Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. Bir işlev oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/). 

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın.
2. Kullanılacak bir işlev uygulamanız varsa **İşlev uygulamalarınızdan** seçin ve ardından **Aç**’a tıklayın. Yeni bir işlev uygulaması oluşturmak için yeni işlev uygulamanıza ait benzersiz bir **Ad** yazın veya oluşturulan adı kabul edin, tercih ettiğiniz **Bölge** seçeneğini belirleyin ve ardından **Oluştur + başla** düğmesine tıklayın. 
3. İşlev uygulamanızda **+ Yeni İşlev** > **GitHub Web Kancası - Düğüm** > **Oluştur** öğesine tıklayın. Bunun yapılması belirtilen şablonu temel alan varsayılan ada sahip bir işlev oluşturur. 
   
    ![Yeni GitHub web kancası işlevi oluşturma](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 
4. **Geliştir** sekmesinde **Kod** penceresindeki örnek express.js işlevine dikkat edin. Bu işlev bir sorun açıklaması web kancasından GitHub isteği alır, sorun metnini günlüğe kaydeder ve web kancasına `New GitHub comment: <Your issue comment text>` olarak bir yanıt gönderir.

    ![Yeni GitHub web kancası işlevi oluşturma](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. **İşlev URL’si** ve **GitHub Gizli Anahtarı** değerlerini kopyalayın. GitHub içinde web kancasını oluşturduğunuzda bunlar gerekli olacaktır. 
2. **Çalıştır** öğesine inin, İstek gövdesindeki sorun açıklamasının önceden tanımlanmış JSON gövdesini not edin, ardından **Çalıştır**’a tıklayın. 
   
    Beklenen bir gövde JSON verisi belirtip **Çalıştır** düğmesine tıklayarak **Geliştir** sekmesinde yeni bir şablon temelli işlevi her zaman test edebilirsiniz. Bu durumda, şablonun sorun açıklaması için önceden tanımlanmış bir gövdesi olur. 

Ardından, GitHub deponuzda gerçek web kancasını oluşturursunuz.

## <a name="configure-the-webhook"></a>Web kancası yapılandırma
1. GitHub’da sahip olduğunuz bir depoya gidin; buna çatallanmış tüm depolar dahildir.
2. **Ayarlar** > **Web kancaları ve hizmetler** > **Web kancası ekle** öğesine tıklayın.
   
    ![Yeni GitHub web kancası işlevi oluşturma](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   
3. İşlevinizin URL’sini ve gizli anahtarını **Yük URL’si** ve **Gizli Anahtar** alanına yapıştırın, ardından **Tek olayları seçmeme izin ver** öğesine tıklayın, **Sorun açıklaması**’nı seçin ve **Web kancası ekle**’ye tıklayın.
   
    ![Yeni GitHub web kancası işlevi oluşturma](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Bu noktada, GitHub web kancası yeni bir sorun açıklaması eklendiğinde işlevinizi tetikleyecek şekilde yapılandırılır.  
Bundan sonra sınama yapılır.

## <a name="test-the-function"></a>İşlevi test etme
1. GitHub deponuzda **Sorunlar** sekmesini yeni bir tarayıcı penceresinde açın, **Yeni Sorun**’a tıklayın, bir başlık yazın ve ardından **Yeni sorunu gönder**’e tıklayın. Ayrıca var olan bir sorunu açabilirsiniz.
2. Sorunda bir açıklama yazın ve **Açıklama**’ya tıklayın. Bu noktada, GitHub’daki yeni web kancanıza geri dönebilir ve **Son Teslimatlar** altında bir web kancası isteğinin gönderildiğini ve yanıt gövdesinin `New GitHub comment: <Your issue comment text>` olduğunu görebilirsiniz.
3. İşlevler portalına geri dönerek günlüklere inin ve işlevin tetiklendiğini ve akış günlüklerine `New GitHub comment: <Your issue comment text>` değerinin yazıldığını görün.

## <a name="next-steps"></a>Sonraki adımlar
Azure İşlevleri hakkında daha fazla bilgi edinmek için bu konulara göz atın.

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Kodlama işlevleri için programlayıcı başvurusu.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Dinamik hizmet planı dahil olmak üzere Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


