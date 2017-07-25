---
title: "Visual Studio’yu kullanarak Azure’da ilk işlevinizi oluşturma | Microsoft Docs"
description: "Visual Studio için Azure İşlevleri Araçları’nı kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve yayımlayın."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: rachelap, glenga
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: be7a9979ba7e6aa26c60b24bcc892ca35af3c1fc
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---
# <a name="create-your-first-function-using-visual-studio"></a>Visual Studio kullanarak ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu sunucusuz bir ortamda yürütmenize olanak tanır.

> [!IMPORTANT]
> Bu konu başlığı altında, adımları tamamlamak için Visual Studio’nun Önizleme Sürümü kullanılır. Devam etmeden önce lütfen [Visual Studio 2017 Önizleme sürümü 15.3](https://www.visualstudio.com/vs/preview/)’ü yüklediğinizden emin olun.

Bu konuda, yerel olarak bir “merhaba dünya” işlevini oluşturmak ve test etmek amacıyla Visual Studio 2017 için Azure İşlevleri Araçları’nı nasıl kullanacağınızı öğreneceksiniz. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Visual Studio projesinde Azure İşlevleri kodu](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yükleyin:

* **Azure geliştirme** iş yükü dahil [Visual Studio 2017 Önizleme sürümü 15.3](https://www.visualstudio.com/vs/preview/).

    ![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-azure-functions-tools-for-visual-studio-2017"></a>Visual Studio 2017 için Azure İşlevleri Araçları’nı yükleyin

Başlamadan önce, Visual Studio 2017 için Azure İşlevleri Araçları’nı indirip yüklemeniz gerekir. Bu araçlar, yalnızca Visual Studio 2017 Önizleme sürümü 15.3 veya sonraki bir sürümüyle kullanılabilir. Azure İşlevleri Araçları’nı zaten yüklediyseniz bu bölümü atlayabilirsiniz.

[!INCLUDE [Install the Azure Functions Tools for Visual Studio](../../includes/functions-install-vstools.md)]   

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Visual Studio'da bir Azure İşlevleri projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Projeyi oluşturduğunuza göre, artık ilk işlevinizi oluşturabilirsiniz.

## <a name="create-the-function"></a>İşlevi oluşturma

**Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. **Azure İşlevi**’ni seçin ve **Ekle**’ye tıklayın.

**HttpTrigger**’ı seçin, **İşlev Adı** yazın, **Erişim Hakları** için **Anonim**’i seçin ve **Oluştur**’a tıklayın. Oluşturulan işleve, herhangi bir istemciden bir HTTP isteği erişir. 

![Yeni bir Azure İşlevi oluşturma](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

[!INCLUDE [Test the function locally](../../includes/functions-vstools-test.md)]

Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.  

![Azure yerel çalışma zamanı](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

 HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

Hata ayıklamayı durdurmak için Visual Studio araç çubuğunda **Durdur** düğmesine tıklayın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamanızın olması gerekir. Visual Studio'dan bir işlev uygulaması oluşturabilirsiniz.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

Yayımlama profili sayfasından işlev uygulamasının temel URL'sini kopyalayın. İşlevi yerel olarak test ederken kullandığınız URL’nin `localhost:port` kısmını, yeni temel URL ile değiştirin. Daha önce olduğu gibi, `&name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütmeyi unutmayın.

HTTP ile tetiklenen işlevinizi çağıran URL şunun gibi görünür:

    http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü uzak GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

![Tarayıcıdaki işlev yanıtı](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak C# işlev uygulaması oluşturmak için Visual Studio’yu kullandınız. 

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Azure İşlevleri Temel Araçları ile yerel test ve hata ayıklama hakkında daha fazla bilgi için bkz. [Kod ve Azure İşlevleri’nin yerel olarak test edilmesi](functions-run-local.md). .NET sınıf kitaplıkları olarak işlevleri geliştirme hakkında daha fazla bilgi için bkz. [.NET sınıf kitaplıklarını Azure İşlevleri ile kullanma](functions-dotnet-class-library.md). 


