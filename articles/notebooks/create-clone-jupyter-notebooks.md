---
title: Oluşturma ve Azure üzerinde Jupyter Notebook kopyalayın
description: Azure not defterleri projeler not defterlerini ve yeni oluştur veya başka bir kaynaktan kopyalama ilişkili dosyaları koleksiyonu yönetin.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9b6a49e2-1d71-4c0b-9e5d-16e059427e38
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: 0ee0c7162e26b875c74796b6d5379b414981e2d5
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59282330"
---
# <a name="create-and-clone-projects"></a>Projeleri oluşturma ve kopyalama

Azure not defterleri ilgili dosyaları ve Jupyter Notebook olarak adlandırılan mantıksal gruplar halinde düzenler *projeleri*. Bir kapsayıcı olarak ilk proje oluşturma sonra oluşturduğunuzda veya diğer proje dosyalarının yanı sıra bir klasördeki bir veya daha fazla not defterlerini kopyalayın. (Bu işlem gösterilmiştir [öğretici](tutorial-create-run-jupyter-notebook.md).)

Bir proje, meta verileri ve özel kurulum adımları ve paket yükleme dahil olmak üzere çalıştırmak, hangi Not sunucuyu etkileyen diğer yapılandırma ayarlarını da korur. Daha fazla bilgi için [yönetme ve projeleri yapılandırma](configure-manage-azure-notebooks-projects.md).

## <a name="use-the-my-projects-dashboard"></a>Projelerim panosunu kullanma

**Projelerim** Panosu'nda `https://notebooks.azure.com/<userID>/projects` burada görüntüleyin, yönetin ve projeleri oluşturma:

[![Azure not defterlerinde projeleri Panom](media/my-projects-dashboard.png)](media/my-projects-dashboard.png#lightbox)

Panoda yapabilecekleriniz kullanıcı kimliği sahip bir hesapla oturum mi oturum açmadıysanız bağlıdır:

| Komut | Kimler kullanabilir? | Açıklama |
| --- | --- | --- |
| **Çalıştırma** | Sahip | Project server başlar ve Jupyter'de proje klasörünü açar. (Daha yaygın olarak, ilk olarak bir proje klasörüne gidin ve sonra bir not defteri oradan başlatın.) |
| **İndir** | Herkes | Seçili projenin bir kopyasını ZIP dosyası olarak indirir. |
| **Paylaş** | Herkes | Paylaşım açılan menüsü üzerinden Seçili projeye bir URL alabilir, sosyal medyada paylaşın, URL içeren bir e-posta gönderin ve almak için hem HTML veya Markdown kodu ile bir "başlatma Not" rozet görüntüler (bkz [başlatma rozet elde](#obtain-a-launch-badge)) URL ile. |
| **Silme** | Sahip | Seçili proje siler. Bu işlem geri alınamaz. |
| **Terminal** | Sahip | Project server başlar ve ardından bu sunucu için terminal bash ile yeni bir tarayıcı penceresi açılır. |
| **+ Yeni Proje** | Sahip | Yeni bir proje oluşturur. Bkz: [yeni bir proje oluşturma](#create-a-new-project). |
| **GitHub deposunu karşıya yükleme** | Sahip | Bir projeyi Github'dan içeri aktarır. [Bir projeyi Github'dan alma](#import-a-project-from-github). |
| **Kopya** | Herkes | Seçilen proje kendi hesabına kopyalar. Oturum imzalamanızı ister. Bkz: [bir projesini kopyalama](#clone-a-project). |

### <a name="obtain-a-launch-badge"></a>Başlatma rozet alın

Kullanırken **paylaşımı** seçin ve komut **ekleme** sekmesi, HTML kod veya "dizüstü başlatma" rozet oluşturan Markdown kopyalayabilir:

![Not Defteri rozet başlatın](https://notebooks.azure.com/launch.png)

Bir Azure not defterleri projesi yoksa, uygun kullanıcı adı ve depo adları değiştirerek aşağıdaki şablonlarını kullanarak doğrudan Github'dan klonlar bir bağlantı oluşturabilirsiniz:

```html
<a href="https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>"><img src="https://notebooks.azure.com/launch.png" /></a>
```

```markdown
[![Azure Notebooks](https://notebooks.azure.com/launch.png)](https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>)
```

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Kullanırken **+ yeni proje** komutu, Azure not defterleri görüntüler bir **yeni proje oluştur** açılır. Bu açılan penceresinde, aşağıdaki bilgileri girin ve ardından **Oluştur**:

| Alan | Açıklama |
| --- | --- |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. Örneğin, "Not Defteri Projem". |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL'nin bir parçası haline gelir özel bir tanımlayıcı (form `https://notebooks.azure.com/<user_id>/projects/<project_id>`). Bu kimlik yalnızca harf, rakam ve kısa çizgiler kullanabilirsiniz, 30 karakterle sınırlıdır ve olamaz bir [proje kimliği ayrılmış](#reserved-project-ids). Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Bu Benioku projeyle Başlat | Varsa ayarlayın, bir varsayılan oluşturur *README.md* proje dosyasında. A *README.md* dosyasıdır projeniz için belgeleri sağlarsınız isterseniz. |

### <a name="reserved-project-ids"></a>Ayrılmış proje kimlikleri

Aşağıdaki ayrılmış sözcükler başlarına proje kimlikleri kullanılamaz. Bu ayrılmış sözcükler ancak olabilir daha uzun proje kimlikleri bir parçası olarak kullanılır.

| | | | | | |
| --- | --- | --- | --- | --- | --- |
| hakkında | account | Yönetim | api | blogu | sınıf |
| content | pano | Keşfedin | SSS | Yardım | html |
| giriş sayfası | içeri aktarma | Kitaplık | yönetim | yeni | Not Defteri |
| Not Defterleri | PDF | önizleme | fiyatlandırma | profil | ara |
| durum | destek | test | | | |

Bu sözcükler birini bir proje kimliği kullanmayı denerseniz **yeni proje oluştur** ve **proje ayarları** açılan pencereler gösterir, "kitaplık kimliği ayrılmış bir tanımlayıcı değil."

Bir proje kimliği de bir projenin URL'SİNİN bir parçası olduğundan, ad engelleyici yazılım "Duyurusu." gibi bazı anahtar sözcükler kullanımını engelleyebilir Bu gibi durumlarda, farklı bir sözcük proje kimliği kullanın.

## <a name="import-a-project-from-github"></a>Bir projeyi Github'dan alma

Tüm genel bir GitHub deposuna tüm veriler dahil olmak üzere bir proje olarak kolayca içeri aktarabilirsiniz ve *README.md* dosyaları. Kullanım **karşıya GitHub deposunu** komutu, açılan aşağıdaki ayrıntıları sağlayın ve ardından seçin **alma**:

| Alan | Açıklama |
| --- | --- |
| GitHub deposu | Github.com kaynak depoda adı. Örneğin, Azure Bilişsel hizmetler için Jupyter not defterlerini kopyalamak için [ https://github.com/Microsoft/cognitive-services-notebooks ](https://github.com/Microsoft/cognitive-services-notebooks), "Microsoft/bilişsel-services-dizüstü bilgisayarlar" girin.  |
| Yinelemeli olarak kopyalama | GitHub depoları, birden çok alt deposu içerebilir. Üst deponun ve tüm alt öğelerini kopyalamak istiyorsanız bu seçeneği ayarlayın. Çok sayıda alt öğeleri bir depo için mümkün olduğu için bu seçeneği temizleyin ihtiyaç duymadıkça bırakın. |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL'nin bir parçası haline gelir özel bir tanımlayıcı (form `https://notebooks.azure.com/<user_id>/projects/<project_id>`). Bu kimlik yalnızca harf, rakam ve kısa çizgiler kullanabilirsiniz, 30 karakterle sınırlıdır ve olamaz bir [proje kimliği ayrılmış](#reserved-project-ids). Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |

Bir depo Github'dan alma geçmişini alır. Yeni değişiklikleri, Github'dan değişiklikleri çekme ve benzeri terminalden standart Git komutlarını kullanabilirsiniz.

## <a name="clone-a-project"></a>Bir projesini kopyalama

Kopyalama, ardından çalıştırın ve herhangi bir not defteri veya diğer proje dosyasında değişiklik kendi hesabınız mevcut bir projeyi bir kopyasını oluşturur. Hangi deneyleri veya diğer iş özgün proje bozmadan bunu kendi projeleriniz kopyalarını için kopyalama de kullanabilirsiniz.

Bir proje kopyalamak için:

1. Üzerinde **Projelerim** Pano, istenen proje sağ tıklayıp **kopya** (klavye kısayolu: c).

    ![Proje bağlam menüsünde kopyalama komutu](media/clone-command.png)

1. İçinde **kopya proje** açılan, bir kopya için adı ve kimliği girin ve kopya genel olup olmadığını belirtin. Bu ayarları aynı olan bir [yeni proje](#create-a-new-project).

    ![Kopya proje açılan menüsü](media/clone-project.png)

1. Seçtikten sonra **kopya** Kopyala düğmesini Azure not defterleri gider.

## <a name="next-steps"></a>Sonraki adımlar

- [Örnek Not Defterleri keşfedin](azure-notebooks-samples.md)
- [Nasıl yapılır: Yapılandırma ve projeleri yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: İçinde bir not defteri paketleri yükleme](install-packages-jupyter-notebook.md)
- [Nasıl yapılır: Mevcut bir slayt gösterisi](present-jupyter-notebooks-slideshow.md)
- [Nasıl yapılır: Veri dosyaları ile çalışma](work-with-project-data-files.md)
- [Nasıl yapılır: Veri kaynaklarına erişim](access-data-resources-jupyter-notebooks.md)
- [Nasıl yapılır: Azure Machine Learning Hizmetleri kullanma](use-machine-learning-services-jupyter-notebooks.md)
