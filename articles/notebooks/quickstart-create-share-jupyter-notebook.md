---
title: Oluşturma ve Azure üzerinde Jupyter notebook paylaşma
description: Hızlı bir şekilde oluşturun ve Azure not defterleri ile ilgili bir Jupyter not defteri çalıştırın, sonra bu not defterini başkalarıyla paylaşın.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 5add60ad-0b4b-4fd5-adf5-eb50ce072d00
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b1618e2ac997445606ce98fc72a1ec35ca1280be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60239948"
---
# <a name="quickstart-create-and-share-a-notebook"></a>Hızlı Başlangıç: Not defteri oluşturma ve paylaşma

1. Git [Azure not defterleri](https://notebooks.azure.com) ve oturum açın. (Ayrıntılar için bkz [hızlı başlangıç - Azure not defterleri için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Genel profil sayfanızdan seçin **Projelerim** sayfanın üstündeki:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. Üzerinde **Projelerim** sayfasında **+ yeni proje** (klavye kısayolu: n); yalnızca olarak görüntülenebilir **+** tarayıcı penceresini darsa:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. İçinde **yeni proje oluştur** görünen açılan girin veya aşağıdaki bilgileri ayarlayın ve ardından **Oluştur**:

   - **Proje adı**: Python dilinde Merhaba Dünya
   - **Proje kimliği**: hello-world-python
   - **Genel proje**: (Seçili)
   - **Bir Benioku.MD oluşturma**: (Seçili)

     ![Yeni Proje açılır penceresi ile doldurulmuş ayrıntıları](media/quickstarts/new-project-popup.png)

1. Birkaç dakika sonra Azure not defterleri yeni projeye gider. Bir not defteri seçerek projeye ekleyin. **+ yeni** açılan (yalnızca görünen **+**), sonra seçerek **not defteri**:

    [![](media/quickstarts/empty-project-new-notebook-button.png "Yeni bir proje boş ve not defteri komut ekleme")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. İçinde **Yeni Not Defteri Oluştur** görünen açılan bir dosya adı gibi not defteri için girin *HelloWorldInPython.ipynb* (*.ipynb* IronPython (Jupyter) not defteri anlamına gelir. ) seçip **Python 3.6** dil (olarak da adlandırılan *çekirdek*):

    ![Yeni Not Defteri Oluştur açılan menüsü](media/quickstarts/new-notebook-popup.png)

1. Seçin **yeni** ardından projenizin dosya listesinde görünür not defteri oluşturmayı tamamlamak için:

    ![Projenin dosya listesinde görünen yeni not defteri](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Not defterini çalıştırma

1. Yeni Not Defteri Düzenleyicisi'nde çalıştırmak için seçin. (Bu örnekte Python 3.6) seçtiğiniz çekirdek bu not defteri için otomatik olarak etkinleştirilir:

    ![Azure not defterleri yeni bir not defteri görünümü](media/quickstarts/create-notebook-first-open.png)

1. Varsayılan olarak, Not defterini bir boş kodu hücreyi vardır. Hücre değiştirir **Markdown**, hücresi türü açılan listeyi kullanarak **Markdown**:

    ![Yeni bir not defterinde hücresi türü değiştirme](media/quickstarts/create-notebook-cell-type.png)

1. Hücreye aşağıdaki başlık metnini yapıştırın veya girin:

    ```markdown
    # Hello World in Python
    ```

1. Markdown düzenlediğiniz çünkü metin "#" ile bir üstbilgi görünür. Markdown HTML'e işlenecek seçin **çalıştırma** düğmesi. Azure not defterleri sonra otomatik olarak oluşturur Yeni bir kod hücresi daha sonra:

    ![Bir hücreyi ve işlenmiş Markdown Çalıştır düğmesi](media/quickstarts/run-cell-markdown-render.png)

1. Kod hücresine aşağıdaki Python kodu girin:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Seçin **çalıştırma** (klavye kısayolu: Shift + kodu çalıştırmak için Enter). Bir hücrede aşağıdaki metne benzer başarılı bir çıktı görmeniz gerekir:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Ve kayıt simgesini çalışmanızı kaydetmek için:

    ![Jupyter not defteri araç Kaydet simgesine](media/quickstarts/hello-results-save-icon.png)

1. Seçin **dosya** > **Kapat ve Durdur** tarayıcı penceresini kapatın ve sunucuyu durdurmak için menü komutu.

## <a name="share-the-notebook"></a>Not defterini paylaşın

Dizüstü bilgisayarınızı paylaşmak için gerekirse proje sayfasına geçin, not defteri dosyasını sağ tıklayın, **bağlantıyı Kopyala** (klavye kısayolu: y), bu bağlantıyı uygun bir iletisine yapıştırın (e-posta, anlık ileti, vs.).

De kullanabilirsiniz proje sayfasında **paylaşımı** bağlantısını almak, bağlantı ile bir e-posta iletisi oluşturun veya HTML ve Markdown elde etmek için menü ekleme kodu:

![Proje paylaşımı komutu](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
