---
title: Oluşturma ve Azure üzerinde Jupyter Notebook kopyalama | Microsoft Docs
description: Azure not defterleri projeler not defterlerini ve yeni oluştur veya başka bir kaynaktan kopyalama ilişkili dosyaları koleksiyonu yönetin.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9b6a49e2-1d71-4c0b-9e5d-16e059427e38
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 36de7ce0cbbc1c3a2c12682b6882dc4d5491eb10
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856732"
---
# <a name="create-and-clone-projects"></a>Oluşturma ve projeleri kopyalama

Azure not defterleri ilgili dosyaları ve Jupyter Notebook olarak adlandırılan mantıksal gruplar halinde düzenler *projeleri*. Bir kapsayıcı olarak ilk proje oluşturma sonra oluşturduğunuzda veya diğer proje dosyalarının yanı sıra bir klasördeki bir veya daha fazla not defterlerini kopyalayın. (Bu işlem gösterilmiştir [öğretici](tutorial-create-run-jupyter-notebook.md).)

Bir proje, meta verileri ve özel kurulum adımları ve paket yükleme dahil olmak üzere çalıştırmak, hangi Not sunucuyu etkileyen diğer yapılandırma ayarlarını da korur. Daha fazla bilgi için [yönetme ve projeleri yapılandırma](configure-manage-azure-notebooks-projects.md).

## <a name="use-the-my-projects-dashboard"></a>Projelerim panosunu kullanma

**Projelerim** Panosu'nda `https://notebooks.azure.com/<userID>/projects` burada görüntüleyin, yönetin ve projeleri oluşturma:

[![](media/my-projects-dashboard.png "Azure not defterlerinde projeleri Panom")](media/my-projects-dashboard.png#lightbox)

Panoda yapabilecekleriniz kullanıcı kimliği sahip bir hesapla oturum mi oturum açmadıysanız bağlıdır:

| Komut | Kimler kullanabilir? | Açıklama |
| --- | --- | --- |
| **Çalıştırma** | Sahip | Project server başlar ve Jupyter'de proje klasörünü açar. (Daha yaygın olarak, ilk olarak bir proje klasörüne gidin ve sonra bir not defteri oradan başlatın.) |
| **İndir** | Herkes | Seçili projenin bir kopyasını ZIP dosyası olarak indirir. |
| **Paylaş** | Herkes | Paylaşım açılan menüsü üzerinden Seçili projeye bir URL alabilir, sosyal medyada paylaşın, URL içeren bir e-posta gönderin ve almak için hem HTML veya Markdown kodu ile bir "başlatma Not" rozet görüntüler (bkz [başlatma rozet elde](#obtain-a-launch-badge)) URL ile. |
| **Silme** | Sahip | Seçili proje siler. Bu işlem geri alınamaz. |
| **Terminal** | Sahip | Project server başlar ve ardından bu sunucu için terminal bash ile yeni bir tarayıcı penceresi açılır. |
| **+ Yeni Proje** | Sahip | Yeni bir proje oluşturur. Bkz: [yeni bir proje oluşturma](#create-a-new-project). |
| **Github deposunu karşıya yükleme** | Sahip | Bir projeyi Github'dan içeri aktarır. [Bir projeyi Github'dan alma](#import-a-project-from-github). |
| **Kopya** | Herkes | Seçilen proje kendi hesabına kopyalar. Oturum imzalamanızı ister. Bkz: [bir projesini kopyalama](#clone-a-project). |

### <a name="obtain-a-launch-badge"></a>Başlatma rozet alın

Kullanırken **paylaşımı** seçin ve komut **ekleme** sekmesi, HTML kod veya "dizüstü başlatma" rozet oluşturan Markdown kopyalayabilir:

    ![Launch notebook badge ](https://notebooks.azure.com/launch.png)

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
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. Örneğin, "Ny not defteri proje". |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL parçası haline gelir özel tanımlayıcısı. Bu kimlik, yalnızca harf, rakam ve kısa çizgiler kullanabilirsiniz ve 30 karakterle sınırlıdır. Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Bu Benioku projeyle Başlat | Varsa ayarlayın, bir varsayılan oluşturur *README.md* proje dosyasında. A *README.md* dosyasıdır projeniz için belgeleri sağlarsınız isterseniz. |

## <a name="import-a-project-from-github"></a>Bir projeyi Github'dan alma

Tüm genel bir GitHub deposuna tüm veriler dahil olmak üzere bir proje olarak kolayca içeri aktarabilirsiniz ve *README.md* dosyaları. Kullanım **karşıya GitHub deposunu** komutu, açılan aşağıdaki ayrıntıları sağlayın ve ardından seçin **alma**:

| Alan | Açıklama |
| --- | --- |
| GitHub deposu | Github.com kaynak depoda adı. Örneğin, Azure Bilişsel hizmetler için Jupyter not defterlerini kopyalamak için [ https://github.com/Microsoft/cognitive-services-notebooks ](https://github.com/Microsoft/cognitive-services-notebooks), "Microsoft/bilişsel-services-dizüstü bilgisayarlar" girin.  |
| Yinelemeli olarak kopyalama | GitHub depoları, birden çok alt deposu içerebilir. Üst deponun ve tüm alt öğelerini kopyalamak istiyorsanız bu seçeneği ayarlayın. Çok sayıda alt öğeleri bir depo için mümkün olduğu için bu seçeneği temizleyin ihtiyaç duymadıkça bırakın. |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL parçası haline gelir özel tanımlayıcısı. Bu kimlik, yalnızca harf, rakam ve kısa çizgiler kullanabilirsiniz. |
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
- [Nasıl yapılır: projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: yükleme paketleri içinde bir not defteri](install-packages-jupyter-notebook.md)
- [Nasıl yapılır: bir slayt gösterisi sunar](present-jupyter-notebooks-slideshow.md)
- [Nasıl yapılır: veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Nasıl yapılır: veri kaynaklarına erişim](access-data-resources-jupyter-notebooks.md)
- [Nasıl yapılır: Azure Machine Learning Hizmetleri kullanma](use-machine-learning-services-jupyter-notebooks.md)
