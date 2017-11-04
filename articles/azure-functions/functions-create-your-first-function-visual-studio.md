---
title: "Visual Studio’yu kullanarak Azure’da ilk işlevinizi oluşturma | Microsoft Docs"
description: "Visual Studio için Azure İşlevleri Araçları’nı kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve yayımlayın."
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2017
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 016179372d69dc63f5e5226723d87ac6e74b31fd
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="create-your-first-function-using-visual-studio"></a>Visual Studio kullanarak ilk işlevinizi oluşturma

Azure işlevleri sağlar, kodunuzda yürütme bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) önce bir VM oluşturun veya bir web uygulaması yayımlamak zorunda kalmadan ortamı.

Bu konuda, oluşturma ve bir "hello world" işlevi yerel olarak test etmek için Azure işlevleri için Visual Studio 2017 araçlarını kullanmayı öğrenin. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar, Visual Studio 2017 sürüm 15,3 veya üzeri bir sürümde Azure geliştirme iş yükünün parçası olarak kullanılabilir.

![Visual Studio projesinde Azure İşlevleri kodu](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yükleyin:

* [Visual Studio 2017 sürüm 15.3](https://www.visualstudio.com/vs/preview/) veya sonraki bir sürümü de dahil olmak üzere **Azure geliştirme** iş yükü.

    ![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-install-note.md)] 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Visual Studio'da bir Azure İşlevleri projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Projeyi oluşturduğunuza göre, artık ilk işlevinizi oluşturabilirsiniz.

## <a name="create-the-function"></a>İşlevi oluşturma

1. **Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. **Azure İşlevi**’ni seçin ve **Ekle**’ye tıklayın.

2. **HttpTrigger**’ı seçin, **İşlev Adı** yazın, **Erişim Hakları** için **Anonim**’i seçin ve **Oluştur**’a tıklayın. Oluşturulan işleve, herhangi bir istemciden bir HTTP isteği erişir. 

    ![Yeni bir Azure İşlevi oluşturma](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    Kod dosyası projenize işlevi kodunuzu uygulayan bir sınıf içerir eklenir. Bu kod adı değeri ve geri kırmak alan bir şablona temel alır. **FunctionName** özniteliği, işlevin adını ayarlar. **HttpTrigger** öznitelik işlevi tetikler iletisi gösterir. 

    ![İşlev kod dosyası](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.  

1. İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin.  Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.  

    ![Azure yerel çalışma zamanı](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Hata ayıklamayı durdurmak için Visual Studio araç çubuğunda **Durdur** düğmesine tıklayın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamanızın olması gerekir. Visual Studio'dan bir işlev uygulaması oluşturabilirsiniz.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. Yayımlama profili sayfasından işlev uygulamasının temel URL'sini kopyalayın. İşlevi yerel olarak test ederken kullandığınız URL’nin `localhost:port` kısmını, yeni temel URL ile değiştirin. Daha önce olduğu gibi, `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütmeyi unutmayın.

    HTTP ile tetiklenen işlevinizi çağıran URL şunun gibi görünür:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü uzak GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak C# işlev uygulaması oluşturmak için Visual Studio’yu kullandınız. 

+ Projenizi diğer tetikleyici ve bağlama türlerini destekleyecek şekilde yapılandırma hakkında bilgi almak için, [Visual Studio için Azure İşlevleri Araçları](functions-develop-vs.md) makalesindeki [Yerel geliştirme için proje yapılandırma](functions-develop-vs.md#configure-the-project-for-local-development) bölümüne bakın.
+ Azure İşlevleri Temel Araçları ile yerel test ve hata ayıklama hakkında daha fazla bilgi için bkz. [Kod ve Azure İşlevleri’nin yerel olarak test edilmesi](functions-run-local.md). 
+ .NET sınıf kitaplıkları olarak işlevleri geliştirme hakkında daha fazla bilgi için bkz. [.NET sınıf kitaplıklarını Azure İşlevleri ile kullanma](functions-dotnet-class-library.md). 

