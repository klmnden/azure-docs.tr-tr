---
title: 'Öğretici: Blob storage - Azure depolama üzerinde statik Web sitesi barındırma'
description: Statik Web sitesi barındırmak için bir depolama hesabı yapılandırma hakkında bilgi edinin ve Azure depolama için statik bir Web sitesi dağıtın.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: tamram
ms.custom: seodec18
ms.openlocfilehash: eb472465c0d35150f2a13563058905751219411d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61428581"
---
<!---Customer intent: I want to host files for a static website in Blob storage and access the website from an Azure endpoint.--->

# <a name="tutorial-host-a-static-website-on-blob-storage"></a>Öğretici: Blob Depolama üzerinde statik Web sitesi barındırma

Bu öğretici, bir dizinin birinci bölümüdür. İçinde oluşturun ve Azure depolama için statik bir Web sitesi dağıtma hakkında bilgi edinin. İşlemi tamamladığınızda, kullanıcıların herkese açık şekilde erişebileceği statik bir Web sitesi olacaktır. 

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Statik Web sitesi barındırma yapılandırın
> * Hello World Web sitesi dağıtma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Bu öğreticide [Visual Studio Code](https://code.visualstudio.com/download), statik Web sitesi oluşturma ve bir Azure depolama hesabına dağıtma programcıları için ücretsiz bir araç.

Visual Studio Code'u yükledikten sonra Azure depolama Önizleme uzantıyı yükleyin. Bu uzantı Visual Studio Code ile Azure depolama yönetim işlevini tümleştirir. Uzantı, Azure Depolama'ya statik Web sitenizi dağıtmak için kullanır. Uzantıyı yüklemek için:

1. Visual Studio Code'u başlatın.
2. Araç çubuğunda **uzantıları**. Arama *Azure depolama*seçip **Azure depolama** listeden uzantısı. Ardından **yükleme** uzantıyı yüklemek için düğme.

    ![VS Code'da Azure depolama uzantıyı yükleme](media/storage-blob-static-website-host/install-extension-vs-code.png)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum [Azure portalında](https://portal.azure.com/) kullanmaya başlamak için.

## <a name="configure-static-website-hosting"></a>Statik Web sitesi barındırma yapılandırın

İlk adım, Azure portalında statik bir Web sitesi barındırmak için depolama hesabınızın yapılandırmaktır. Azure depolama hesabınızı statik Web site barındırması için yapılandırdığınızda, otomatik olarak adlı bir kapsayıcı oluşturur. *$web*. *$Web* kapsayıcı statik Web siteniz için dosyaları içerir. 

1. Açık [Azure portalında](https://portal.azure.com/) web tarayıcınızda. 
1. Depolama hesabınızı bulun ve hesabına genel bakış görüntüler.
1. Seçin **statik Web sitesi** statik Web siteleri için yapılandırma sayfasını görüntüleyin.
1. Seçin **etkin** depolama hesabı için statik Web sitesi barındırma olanağı.
1. İçinde **dizin belgesi adı** alan, bir varsayılan dizin sayfasını belirtmek *index.html*. Bir kullanıcı, statik Web sitesi köküne gittiğinde varsayılan dizin sayfası görüntülenir.  
1. İçinde **hata belgesi yolu** alanında, varsayılan hata sayfasının belirtin *404. html*. Statik Web sitenize var olmayan bir sayfaya gitmek bir kullanıcı çalıştığında varsayılan hata sayfası görüntülenir.
1. **Kaydet**’e tıklayın. Azure portalında statik Web sitesi uç noktanızı artık görüntüler. 

    ![Bir depolama hesabı için statik Web sitesi barındırma etkinleştir](media/storage-blob-static-website-host/enable-static-website-hosting.png)

## <a name="deploy-a-hello-world-website"></a>Hello World Web sitesi dağıtma

Ardından, Visual Studio Code ile bir Hello World web sayfası oluşturabilir ve barındırılan Azure depolama hesabınızdaki statik Web sitesi dağıtın.

1. Adlı bir boş klasör oluşturun *numaralı* yerel dosya sisteminize. 
1. Visual Studio Code'u başlatın ve yeni oluşturduğunuz gelen klasörü açın **Gezgini** paneli.

    ![Visual Studio code'da klasörü aç](media/storage-blob-static-website-host/open-folder-vs-code.png)

1. Varsayılan dizin dosyasını içinde oluşturmak *numaralı* klasörünü adlandırın *index.html*.

    ![Visual Studio Code'da varsayılan dizin dosyası oluşturma](media/storage-blob-static-website-host/create-index-file-vs-code.png)

1. Açık *index.html* düzenleyicisinde, aşağıdaki metni dosyaya yapıştırın ve kaydedin:

    ```
    <h1>Hello World!</h1>
    ```

1. Varsayılan hata dosyasını oluşturun ve adlandırın *404. html*.
1. Açık *404. html* düzenleyicisinde, aşağıdaki metni dosyaya yapıştırın ve kaydedin:

    ```
    <h1>404</h1>
    ```

1. Altındaki sağ *numaralı* klasöründe **Gezgini** paneli ve seçin **statik Web sitesi Dağıt...**  Web sitenizi dağıtın. Aboneliklerin listesini almak için Azure'da oturum açarken istenir.

1. Statik Web sitesi barındırma etkinleştirdiğiniz depolama hesabını içeren aboneliği seçin. Ardından, istendiğinde depolama hesabını seçin.

Visual Studio Code şimdi web uç noktanıza dosyalarınızı karşıya yükleyin ve başarı durum çubuğunu göster. Azure'da görüntülemek için Web sitesini başlatın.

![Azure'da statik Web sitesi dağıtımı görüntüle](media/storage-blob-static-website-host/view-static-website-endpoint.png)

Başarıyla öğreticisini tamamladığınıza ve statik bir Web sitesi Azure'a dağıtılan.

## <a name="next-steps"></a>Sonraki adımlar

Serinin Bu öğretici, Azure depolama hesabınızı statik Web sitesi barındırma için yapılandırma ve oluşturma ve bir Azure uç noktası için statik bir Web sitesi dağıtma öğrendiniz.

Şimdi özel bir etki alanı SSL ile Azure CDN ile statik bir Web siteniz için yapılandırdığınız iki kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Statik bir Web sitesi için SSL ile özel etki alanı etkinleştirmek için Azure CDN'yi kullanma](storage-blob-static-website-custom-domain.md)
