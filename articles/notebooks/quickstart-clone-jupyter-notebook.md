---
title: Azure not defterleri ile Jupyter not defteri github'dan kopyalama
description: Hızla bir GitHub deposundan bir Jupyter not defterine kopyalayın ve Azure not defterleri hesabınızdaki çalıştırın.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: d7122b78-6daa-4bea-883b-ff832cfecef3
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 30625423553b71e848d27d047d4b7bc3add6eaff
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59265160"
---
# <a name="quickstart-clone-a-notebook"></a>Hızlı Başlangıç: Not defterini kopyalama

Birçok veri bilimcilerine ve geliştiricilere, not defterlerinde depolamak [GitHub depoları](https://github.com), ücretsiz depolama ve sürüm denetimi için birçok farklı proje türleri sağlayan hizmet. GitHub, genellikle yerel olarak çalıştırılan Jupyter notebooks işbirliği olarak kullanılır. Böyle durumlarda, her bir ortak çalışanı depoyu yerel bir kopyasını tutar ve Not Defterleri bu kopyasından çalışır.

Kopyalama kopyasını bir GitHub Not Defteri, bunun yerine Azure not defterleri hesabınızdaki oluşturur. Bu kopyası, özgün depodan bağımsızdır; değişiklikler yalnızca Azure not defterleri hesabınızda depolanan ve özgün etkilemez. Bulutta, kopya olduğu için proje değil herhangi bir yerel kopyalarını veya hatta Jupyter kendi bilgisayarlarında yüklü olan diğer ortak çalışanlar ile paylaşabilirsiniz. Ayrıca, yalnızca bir başlangıç noktası kendi veya veri dosyalarını almak için olarak bir proje için bir not defteri kopyalama.

## <a name="clone-azure-cognitive-services-notebooks"></a>Azure Bilişsel hizmetler not defterlerini kopyalama

1. Git [Azure not defterleri](https://notebooks.azure.com) ve oturum açın. (Ayrıntılar için bkz [hızlı başlangıç - Azure not defterleri için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Genel profil sayfanızdan seçin **Projelerim** sayfanın üstündeki:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. Üzerinde **Projelerim** sayfasında, yukarı ok düğmesini seçin (klavye kısayolu: U; düğme olarak görünen **karşıya GitHub deposunu** tarayıcı penceresini olduğunda yeterince geniş):

    ![GitHub deposunu komut Projelerim sayfasında karşıya yükleme](media/quickstarts/upload-github-repo-command.png)

1. İçinde **karşıya GitHub deposu** görüntülenir, girin veya aşağıdaki bilgileri ayarlayın ve ardından seçin **alma**:

   - **GitHub deposu**: Microsoft/bilişsel-services-not defterlerini (Bu ad, Azure Bilişsel hizmetler için Jupyter not defterlerini klonlar [ https://github.com/Microsoft/cognitive-services-notebooks ](https://github.com/Microsoft/cognitive-services-notebooks)).
   - **Yinelemeli olarak kopyalama**: (Seçili)
   - **Proje adı**: Bilişsel hizmetler kopyalama
   - **Proje kimliği**: bilişsel-services-kopya
   - **Genel**: (Seçili)

     ![Depo bilgilerini toplamak için GitHub deposunu açılan karşıya yükleme](media/quickstarts/upload-github-repo-popup.png)

1. İşlem tamamlanırken arkanıza endişelenmeyin; bir depo kopyalama, birkaç dakika sürebilir.

1. Kopyalama tamamlandıktan sonra Azure not defterleri, yeni projeye görebileceğiniz tüm dosyaların kopyalarını alır.

    [![](media/quickstarts/completed-clone.png "Tamamlanmış bir kopya görünümü")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Bir not defteri paylaşın

1. Kopyalanan projenin kopyasını paylaşmak için kullanmak **paylaşmak** denetlemek veya bağlantısını almak, bağlantısını içeren bir HTML veya Markdown kodunu edinmek ya da bağlantı ile bir e-posta iletisi oluşturun:

    ![Proje paylaşımı komutu](media/quickstarts/share-project-command.png)

1. Temizlenmiş, çünkü **genel** kopya projenin kopyalarken seçeneği özel. Kopyanızı genel hale getirmek için seçin **proje ayarları**ayarlayın **genel proje** açılan pencerede seçeneğini ve ardından **Kaydet**.

1. Bir not defteri çalıştırmak için projeyi seçin. Örneğin, Azure Bilişsel hizmetler deposundaki her bir not defteri kendi müstakil Hızlı Başlangıç ' dir. Aşağıdaki resimde bir Bilişsel hizmetler API'si abonelik anahtarı ekleme ve "bunnies" için arama terimi "Yavru köpekler" değiştirme sonra BingImageSearchAPI not defteri kullanmanın sonucunu göstermektedir:

    ![Github'dan kopyaladığınız Jupyter not defteri çalıştırma](media/quickstarts/clone-notebook-result.png)

1. Bitirdiğinizde not defteri çalıştırmak, seçin **dosya** > **Kapat ve Durdur** not defteri ve tarayıcı penceresini kapatın.

1. Projedeki tek tek bir not defteri paylaşmak için Not defterini sağ tıklayıp **bağlantıyı Kopyala** (klavye kısayolu: y):

    ![Bağlantı için ayrı bir not defteri kopyalamak için bağlam menüsü komutu](media/quickstarts/copy-link-to-individual-notebook.png)

1. Not defterlerini dışında dosyalarını düzenlemek için seçin ve proje dosyasında sağ **dosya Düzenle** (klavye kısayolu: Ben). Varsayılan eylem **çalıştırma** (klavye kısayolu: r), yalnızca dosya içeriğini gösterir ve düzenlemeye izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
