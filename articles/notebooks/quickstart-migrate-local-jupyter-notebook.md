---
title: Yerel Jupyter not defterini Azure not defterleri için geçirme
description: Hızlı bir şekilde Azure not defterleri için yerel bilgisayarınıza veya bir web URL'si bir Jupyter not defteri aktarılması, sonra işbirliği için paylaşın.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 2e935425-3923-4a33-89b2-0f2100b0c0c4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 5ee9970a255928bca9d08111229be6298f20d7b3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59279287"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook"></a>Hızlı Başlangıç: Yerel Jupyter not defterini geçirme

Kendi bilgisayarınızda yerel olarak oluşturduğunuz Jupyter not defterleri yalnızca sizin erişilebilir. Dosyalarınızı anlamına gelir, çeşitli paylaşabilirsiniz, ancak ardından alıcılar not defteri kendi yerel kopyasına sahip ve neden olabilecek tüm değişiklikleri birleştirmek için zordur. GitHub gibi paylaşılan çevrimiçi depoda not defterlerini depolayabilir, ancak bunun yapılması yine de her bir ortak çalışanı kendi yerel Jupyter yükleme sizinkiyle aynı yapılandırmaya sahip olmasını gerektirir.

Azure not defterleri için yerel ya da depo tabanlı not defterlerinizi geçiş yaparak, bunları içinden anında, ortak çalışanlarla paylaşmanız bulutta depolar. Bu ortak çalışanlar görüntüleyip, Not Defteri çalıştırmak için yalnızca bir tarayıcı gereklidir ve bunların [oturum](quickstart-sign-in-azure-notebooks.md) Azure not defterleri için bunlar da değişiklik yapabilirsiniz.

Bu hızlı başlangıçta bir not defteri yerel bilgisayarınıza veya başka bir ulaşılabilir dosya URL'si geçirme işlemi gösterilmektedir. Bir GitHub deposundan not defterlerini geçirmek için bkz [hızlı başlangıç: Bir not defteri kopyalama](quickstart-clone-jupyter-notebook.md).

## <a name="create-a-project-on-azure-notebooks"></a>Azure not defterleri ile ilgili bir proje oluşturun

1. Git [Azure not defterleri](https://notebooks.azure.com) ve oturum açın. (Ayrıntılar için bkz [hızlı başlangıç - Azure not defterleri için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Genel profil sayfanızdan seçin **Projelerim** sayfanın üstündeki:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. Üzerinde **Projelerim** sayfasında **+ yeni proje** (klavye kısayolu: n); yalnızca olarak görüntülenebilir **+** tarayıcı penceresini darsa:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. İçinde **yeni proje oluştur** görüntülenen açılan, geçiş yaptığınız Not defteri için uygun değerleri girin **proje adı** ve **proje kimliği** alanları Temizle Seçenekler **genel proje** ve **bir Benioku.MD oluşturma**, ardından **Oluştur**.

## <a name="upload-the-local-notebook"></a>Yerel not defterini karşıya yükleme

1. Proje sayfasında **karşıya** (yukarı ok yalnızca Eğer öyleyse, tarayıcı pencerenizi görünebilir küçük), 1'i seçin. Görünen açılır seçin **bilgisayardan** defterinizin, yerel dosya sisteminde bulunuyorsa veya **URL'den** defterinizin çevrimiçi bulunuyorsa:

    ![Bir URL veya yerel bilgisayar bir not defterini karşıya yüklemek için komutu](media/quickstarts/upload-from-computer-url-command.png)

   (Not defterinizin bir GitHub deposunda ise, yeniden yönergeleri takip [hızlı başlangıç: Bir not defteri kopyalama](quickstart-clone-jupyter-notebook.md) yerine.)

   - Kullanıyorsanız **bilgisayarı**, sürükle ve bırak, *.ipynb* seçin veya açılan dosyalarıyla **dosya seçin**, ardından gözatın ve içeri aktarmak istediğiniz dosyaları seçin. Ardından **karşıya**. Karşıya yüklenen dosyalar olan yerel dosyalarla aynı adı verilir. (Herhangi bir içeriği karşıya yükleme gerekmez *.ipynb_checkpoints* klasörleri.)

     ![Bilgisayar açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-computer-popup.png)

   - Kullanıyorsanız **URL'den**, kaynak adresi girerek **dosya URL'si** alan ve projenizde not defterine atamak için dosya adı **dosya adı** alan. Ardından **karşıya**. Ayrı URL'ler ile birden çok dosyanız varsa, kullanmak **+ Dosya Ekle** sonra ve açılan yeni alanlar için başka bir dosyaya sağlar, girdiğiniz ilk URL denetlemek için komutu.

     ![URL açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-url-popup.png)

1. Açın ve içeriğini ve işlem doğrulamak için yeni yüklenen defterinizin çalıştırın. İşiniz bittiğinde **dosya** > **durdurmak ve Kapat** not defterini kapatmak için.

1. Karşıya yüklenen defterinizin bağlantısını paylaşmak için projeyi seçip dosyaya sağ tıklayın **bağlantıyı Kopyala** (klavye kısayolu: y), bu bağlantıyı uygun iletisine yapıştırın. Alternatif olarak, tüm kullanarak olarak proje paylaşabilirsiniz **paylaşmak** proje sayfasında denetimi.

1. Not defterlerini dışında dosyalarını düzenlemek için seçin ve proje dosyasında sağ **dosya Düzenle** (klavye kısayolu: Ben). Varsayılan eylem **çalıştırma** (klavye kısayolu: r), yalnızca dosya içeriğini gösterir ve düzenlemeye izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
