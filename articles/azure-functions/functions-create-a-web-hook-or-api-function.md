---
title: "Bir web kancası veya API Azure İşlevi oluşturma | Microsoft Belgeleri"
description: "Bir Web Kancası veya API çağrısı ile çağrılan sunucusuz işlev oluşturmak için Azure İşlevlerini kullanın."
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
ms.date: 02/02/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: 3e12b8c988b8971574352e976ad88e2e47f47660
ms.lasthandoff: 03/16/2017


---
# <a name="create-a-webhook-or-api-azure-function"></a>Bir web kancası veya API Azure İşlevi oluşturma
Azure İşlevleri, çeşitli programlama dillerinde uygulanmış, zamanlanmış veya tetiklenmiş kod birimlerini oluşturmanızı sağlayan olay odaklı bir isteğe bağlı işlem deneyimidir. Azure İşlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure İşlevlerine Genel Bakış](functions-overview.md).

Bu konu başlığında, bir GitHub web kancası tarafından çağrılan bir JavaScript işlevinin nasıl oluşturulacağı gösterilmektedir. Yeni işlev, Azure İşlevleri portalında önceden tanımlanmış bir şablon temel alınarak oluşturulur. Bu adımların portalda nasıl gerçekleştirildiğini görmek için kısa bir video da izleyebilirsiniz.

Bu öğreticideki genel adımlar JavaScript yerine C# veya F# dilinde bir işlev oluşturmak için de kullanılabilir. 

## <a name="watch-the-video"></a>Videoyu izleme
Aşağıdaki video, bu öğreticideki temel adımların nasıl gerçekleştirileceğini gösterir 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

+ Etkin bir Azure hesabı. Henüz bir hesabınız yoksa [bir ücretsiz Azure hesabına kaydolabilirsiniz](https://azure.microsoft.com/free/).  
 Ayrıca [İşlevler’i Deneyin](https://functions.azure.com/try) deneyimini kullanarak bu öğreticiyi bir Azure hesabı olmadan tamamlayabilirsiniz.
+ GitHub hesabı. Henüz yoksa [ücretsiz GitHub hesabına kaydolabilirsiniz](https://github.com/join). 

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Şablondan web kancası ile tetiklenen bir işlev oluşturma
Azure'da işlevlerinizin yürütülmesini bir işlev uygulaması barındırır. 

1. [Azure İşlevleri portalına](https://functions.azure.com/signin) gidin ve Azure hesabınız ile oturum açın.

2. Kullanılacak bir işlev uygulamanız varsa **İşlev uygulamalarınızdan** seçin ve ardından **Aç**’a tıklayın. Bir işlev uygulaması oluşturmak için yeni işlev uygulamanıza ait benzersiz bir **Ad** yazın veya oluşturulan adı kabul edin, tercih ettiğiniz **Bölge** seçeneğini belirleyin ve ardından **Oluştur + başla** düğmesine tıklayın. 

3. İşlev uygulamanızda **+ Yeni İşlev** > **GitHub Web Kancası - JavaScript** > **Oluştur** öğesine tıklayın. Bu adım, belirtilen şablonu temel alan varsayılan ada sahip bir işlev oluşturur. Alternatif olarak bir C# veya F# işlevi oluşturabilirsiniz.
   
    ![GitHub web kancası ile tetiklenen bir işlev oluşturma](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. **Geliştir** sekmesinde **Kod** penceresindeki örnek express.js işlevine dikkat edin. Bu işlev bir sorun açıklaması web kancasından GitHub isteği alır, sorun metnini günlüğe kaydeder ve web kancasına `New GitHub comment: <Your issue comment text>` olarak bir yanıt gönderir.

    ![İşlev kodunu gözden geçirme](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. **İşlev URL’si** ve **GitHub Gizli Anahtarı** değerlerini kopyalayıp kaydedin. Bu değerleri sonraki bölümde web kancasını GitHub’da yapılandırırken kullanacaksınız. 

2. **Test**'e tıklayın, **İstek gövdesindeki** sorun açıklamasının önceden tanımlanmış JSON gövdesini not edin, ardından **Çalıştır**'a tıklayın. 

    ![Web kancası işlevini portalda test etme](./media/functions-create-a-web-hook-or-api-function/functions-test-webhook-in-portal.png)
   
    > [!NOTE]
    > Beklenen bir gövde JSON verisi belirtip **Çalıştır** düğmesine tıklayarak **Geliştir** sekmesinde yeni bir şablon temelli işlevi her zaman test edebilirsiniz. Bu durumda, şablonun sorun açıklaması için önceden tanımlanmış bir gövdesi olur. 

Ardından, GitHub deponuzda gerçek web kancasını oluşturursunuz.

## <a name="configure-the-webhook"></a>Web kancası yapılandırma
1. GitHub’da sahip olduğunuz bir depoya gidin. Varsa çatallandırdığınız depoları da kullanabilirsiniz.
 
2. **Ayarlar** > **Web kancaları ve hizmetler** > **Web kancası ekle** öğesine tıklayın.
   
    ![GitHub web kancası ekleme](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   

3. İşlevinizin URL’sini ve gizli anahtarını **Yük URL’si** ve **Gizli Anahtar** alanına yapıştırıp **İçerik türü** için **uygulama/json** öğesini seçin.

4. **Olayları tek tek seçmeme izin ver**’e tıklayın, **Sorun Açıklaması**’nı seçin ve **Web kancası ekle**’ye tıklayın.
   
    ![Web kancası URL'sini ve parolasını ayarlama](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Bu noktada, GitHub web kancası yeni bir sorun açıklaması eklendiğinde işlevinizi tetikleyecek şekilde yapılandırılır.  
Bundan sonra sınama yapılır.

## <a name="test-the-function"></a>İşlevi test etme
1. GitHub deposunda **Sorunlar** sekmesini yeni bir tarayıcı penceresinde açın.

2. Yeni pencerede **Yeni Sorun**’a tıklayın, bir başlık yazın ve ardından **Yeni sorunu gönder**’e tıklayın. Ayrıca var olan bir sorunu açabilirsiniz.

2. Sorunda bir açıklama yazın ve **Açıklama**’ya tıklayın. 

3. Diğer GitHub penceresinde, yeni web kancanızın yanındaki **Düzenle** seçeneğine tıklayın, aşağı kaydırarak **Son Teslimler**’e gelin, ardından bir web kancası isteğinin gönderildiğini ve yanıt gövdesinin `New GitHub comment: <Your issue comment text>` olduğunu doğrulayın.

3. İşlevler portalına geri dönerek günlüklere inin ve işlevin tetiklendiğini ve akış günlüklerine `New GitHub comment: <Your issue comment text>` değerinin yazıldığını görün.

## <a name="next-steps"></a>Sonraki adımlar
Azure İşlevleri hakkında daha fazla bilgi edinmek için bu konulara göz atın.

* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Kodlama işlevleri için programlayıcı başvurusu.
* [Azure İşlevlerini test etme](functions-test-a-function.md)  
  İşlevlerinizi test etmek için çeşitli araçları ve teknikleri açıklar.
* [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


