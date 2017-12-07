---
title: "Visual Studio’yu kullanarak Azure’da ilk işlevinizi oluşturma | Microsoft Docs"
description: "Visual Studio için Azure İşlevleri Araçları’nı kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve yayımlayın."
services: functions
documentationcenter: na
author: ggailey777
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
ms.date: 12/1/2017
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 7f71ecb2b58728f466371c7aa6d2aac965177863
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="create-your-first-function-using-visual-studio"></a>Visual Studio kullanarak ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir ortamda yürütmenize olanak tanır.

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

Bu konuda, yerel olarak bir “merhaba dünya” işlevini oluşturmak ve test etmek amacıyla Azure İşlevleri için Visual Studio 2017 araçlarını nasıl kullanacağınızı öğreneceksiniz. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar, Visual Studio 2017 sürüm 15,3 veya üzeri bir sürümde Azure geliştirme iş yükünün parçası olarak kullanılabilir.

![Visual Studio projesinde Azure İşlevleri kodu](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yükleyin:

* **Azure geliştirme** iş yükü dahil [Visual Studio 2017 sürüm 15.4](https://www.visualstudio.com/vs/) veya sonraki bir sürüm.

    ![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Visual Studio'da bir Azure İşlevleri projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Projeyi oluşturduğunuza göre, artık ilk işlevinizi oluşturabilirsiniz.

## <a name="create-the-function"></a>İşlevi oluşturma

1. **Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. **Azure İşlevi**’ni seçin, **Ad** için `HttpTriggerCSharp.cs` girin ve **Ekle**’ye tıklayın.

2. **HttpTrigger**’ı seçin, **Erişim hakları** için **Anonim**’i seçin ve **Tamam**’a tıklayın. Oluşturulan işleve, herhangi bir istemciden bir HTTP isteği erişir. 

    ![Yeni bir Azure İşlevi oluşturma](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    İşlev kodunuzu uygulayan bir sınıfı içeren bir kod dosyası projenize eklenir. Bu kod bir ad değeri alıp geri yansıtan bir şablonu temel alır. **FunctionName** özniteliği işlevinizin adını ayarlar. **HttpTrigger** özniteliği işlevi tetikleyen iletiyi gösterir. 

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

