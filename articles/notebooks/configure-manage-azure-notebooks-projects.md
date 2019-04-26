---
title: Yapılandırma ve Azure not defteri projeleri yönetme
description: Proje meta veri, proje dosyaları, projenin ortam ve kurulum adımları Azure not defterleri UI ve terminal doğrudan erişim aracılığıyla nasıl yönetileceği.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 35dd6ff1-a14a-4a2e-b173-6d8467de3e89
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: d1f94c5fd774b51f57da2885d1ccd8eb909cd3c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60234995"
---
# <a name="manage-and-configure-projects"></a>Projeleri yönetme ve yapılandırma

Bir Azure not defterlerinde temelde bir Jupyter not defterleri, dosya klasörü ve açıklayıcı meta verileri ile birlikte çalıştığı temel Linux sanal makine yapılandırmasını projesidir. Proje Panosu Azure not defterlerinde dosyaları yönetmek ve aksi takdirde proje özelliklerini yapılandırmanıza olanak sağlar:

- Ücretsiz katman veya bir Azure sanal makinesi üzerinde projesi çalıştırır, bilgi işlem katmanı.
- Bir ad, açıklama, proje paylaşırken ve projenin genel veya özel olup kullanılan tanımlayıcı içeren proje meta verileri.
- Projenin not defteri, verileri ve diğer dosyaları herhangi bir dosya sistemi gibi yönetme.
- Bir projenin ortam başlatma komut dosyaları veya terminal yoluyla doğrudan yönetin.
- Günlükleri, terminal erişin.

> [!Note]
> Burada açıklanan yönetim ve yapılandırma özellikleri, projeyi başlangıçta oluşturan yalnızca proje sahibi için kullanılabilir. Ancak, proje kendi dikkate sahibi ve proje istediğiniz gibi yapılandırabilirsiniz. Bu durumda kopyalama olabilir.

Azure not defterleri, her bir not defteri veya başka bir dosyaya çalıştırdığınızda temel sanal makine başlar. Sunucu otomatik olarak dosyaları kaydeder ve 60 dakika işlem yapılmadığında kapanır. Sunucu ile dilediğiniz zaman durdurabilirsiniz **kapatma** komut (klavye kısayolu: h).

## <a name="compute-tier"></a>İşlem katmanı

**Çalıştırma** proje panosu açılır listede olan proje üzerinde çalıştığı işlem katmanı seçtiğiniz. Varsayılan olarak, projeler çalıştıracağınız **ücretsiz işlem** katmanı, kötüye kullanımı önlemek için 4 GB bellek ve veri 1 GB ile sınırlıdır:

![Katmanı aşağı açılan liste proje panosundaki işlem](media/project-compute-tier-list.png)

Bir Azure aboneliğinde sağladıktan farklı bir sanal makine kullanarak bu kısıtlamaları devre dışı bırakabilir. Yüklemeli ve JupyterHub bu sanal makine üzerinde çalıştırın. Veri bilimi sanal makinesi görüntülerini (herhangi bir işletim sistemini) iyi seçimler olduklarından varsayılan olarak JupyterHub içerirler.

Uygun şekilde yapılandırılmış bir Azure sanal makine oluşturduktan sonra seçin **doğrudan işlem** (listesinde gösterilecek) ad, sanal makinenin IP adresi ve bağlantı noktası (genellikle 8000, varsayılan bağlantı noktası, ister aşağı açılan listede seçeneği JupyterHub dinlediği) ve VM kimlik bilgileri:

![Doğrudan işlem seçeneği sunucu bilgilerini toplamak için sor](media/project-compute-tier-direct.png)

Aşağıdaki koşullar doğruysa, açılır listede de gösterilir [veri bilimi sanal makinesi (DSVM)](/azure/machine-learning/data-science-virtual-machine) örnekleri. (Herhangi biri bu koşullar karşılanmadığı takdirde hala doğrudan işlem seçeneğini kullanarak ve Azure Portalı'ndan elde edilen değerleri girerek DSVM bağlanabilirsiniz.)

- Azure Active Directory (AAD), bir şirket hesabı gibi kullanan bir hesapla Azure not defterlerine oturumunuz.
- Hesabınız bir Azure aboneliğine bağlı.
- Bu Abonelikteki bir veya daha fazla sanal makineler ile en az sahip olduğunuz veri bilimi sanal makinesi için Linux (Ubuntu) görüntüsü kullanan okuyucu erişimi.)

![Proje panosu açılır listede veri bilimi sanal makine örnekleri](media/project-compute-tier-dsvm.png)

Azure not defterleri DSVM örneği seçtiğinizde, sanal Makineyi oluştururken kullanılan belirli bir makine kimlik bilgilerini isteyebilir.

Yeni bir DSVM örneği oluşturmak için yönergeleri takip edin [Ubuntu veri bilimi sanal makinesi oluşturma](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Kullanım **Linux (Ubuntu) için veri bilimi sanal makinesi** DSVM Azure not defterleri aşağı açılan listede görünmesini istiyorsanız, görüntü.  Windows veya CentOS görüntüsü kullanması gereken diğer nedenlerle kullandığınız **doğrudan işlem** el ile değerini DSVM Örneğinize bağlanmak için seçeneği.

> [!IMPORTANT]
> Doğrudan işlem veya veri bilimi sanal makineleri kullanırken, bunları üzerinde çalıştırdığınız not defterlerini tamamen müstakil olmalıdır. Şu anda yalnızca Azure not defterleri kopyalar *.ipynb* VM dosyasına ancak tüm diğer dosyalar projesinde kopyalamaz. Sonuç olarak, diğer proje dosyaları bulmak diğer Vm'lerde çalışan not defterlerini başarısız.
>
> Bu davranış, iki yolla geçici çözüm bulabilirsiniz:
>
> 1. Proje dosyaları sanal Makineye el ile kopyalayın.
>
> 2. Bir kurulum not defteri içindeki dosyalar ekleme, önce birincil not defterini ilk çalıştırma. Dosya içeriğini burada hücresi her dosya için bir kod hücresi Kurulum not defteri oluşturun. Ardından her hücre üst kısmında Ekle komutu `%%writefile <filename>`burada `<filename>` içeriği almak için dosyanın adıdır. Not defterini çalıştırdığınızda, bu sanal makine üzerindeki tüm dosyaları oluşturur. Bir örnek için bkz. [Microsoft evcil hayvan algılayıcısı tanıtım setup.ipynb dosyasında](https://github.com/Microsoft/connect-petdetector/blob/master/setup.ipynb) (GitHub).
>
>     ![Kullanarak bir %% kodu hücreyi başındaki writefile komutu](media/setup-notebook-writefile-command.png)

## <a name="edit-project-metadata"></a>Proje meta verilerini düzenleme

Proje Panosu üzerinde seçin **proje ayarları**, ardından **bilgi** sekmesinde, aşağıdaki tabloda açıklandığı gibi proje meta verileri içerir. Proje meta verileri dilediğiniz zaman değiştirebilirsiniz.

| Ayar | Açıklama |
| --- | --- |
| Proje adı | Azure not defterlerini görüntüleme amacıyla kullanır. projeniz için bir kolay ad. Örneğin, "Hello World içinde Python". |
| Proje Kimliği | Bir proje paylaşmak için kullandığınız URL parçası haline gelir özel tanımlayıcısı. Bu kimlik yalnızca harf, rakam ve kısa çizgiler kullanabilirsiniz, 30 karakterle sınırlıdır ve olamaz bir [proje kimliği ayrılmış](create-clone-jupyter-notebooks.md#reserved-project-ids). Ne kullanılacağını emin değilseniz, genel bir kural projenizin adına bir küçük harfli sürümünü burada "my-Not-(gerekirse, uzunluk sınırını uyacak şekilde kesildi) proje" gibi kısa çizgi içine boşluk açık kullanmaktır. |
| Genel proje | Varsa ayarlama, proje erişmek için bağlantıya kimseyle sağlar. Özel bir proje oluştururken, bu seçeneği temizleyin. |
| Klonları Gizle | Ayarlanırsa, kullanıcılar bu proje için yapılmış kopyalar listesini göremiyorsanız. Klonları gizleme aynı kuruluştaki bir parçası gibi olmayan birçok kişilerle paylaşılan projeler için kullanışlı bir not defteri kullanırken bir sınıf ders için. |

> [!Important]
>
> Proje kimliği değiştiriliyor herhangi bir bağlantı, daha önce paylaştığınız proje geçersiz kılar.

## <a name="manage-project-files"></a>Proje dosyalarını yönetme

Proje Panosu proje klasörü sistem içeriğini gösterir. Bu dosyaları yönetmek için çeşitli komutlara kullanabilirsiniz.

### <a name="create-new-files-and-folders"></a>Yeni dosyalar ve klasörler oluşturma

**+ Yeni** komut (klavye kısayolu: n) yeni dosya veya klasörleri oluşturur. İlk komutunu kullanırken, oluşturulacak öğesi türünü seçin:

| Öğe türü | Açıklama | Komut davranışı |
| --- | --- | --- |
| **Not Defteri** | Jupyter not defteri | Not defterinin dosya adı ve dil belirtin açılır pencere görüntüler. |
| **Klasör** | Bir alt klasör | Klasör adı girin, projenin dosya listesinde bir düzenleme alanı oluşturur. |
| **Boş dosya** | Bir dosya metin, veri, vb. gibi herhangi bir içeriği saklayabilirsiniz. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |
| **Markdown** | Bir Markdown dosyası. | Bir düzenleme alanı dosya adını girin projenin dosya listesini oluşturur. |

### <a name="upload-files"></a>Dosyaları karşıya yükleme

**Karşıya** komut diğer konumlardan veri aktarmaya yönelik iki seçenek sağlar: **URL'den** ve **bilgisayardan**. Daha fazla bilgi için [veri dosyalarını Azure not defteri projelerde çalışabilirsiniz](work-with-project-data-files.md).

### <a name="select-file-specific-commands"></a>Dosya özel komutları seçin

Projenin dosya listesindeki her öğeyi sağ bağlam menüsü komutları sağlar:

![Bir dosya bağlam menüsü komutları](media/project-file-commands.png)

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Çalıştırın | r (veya tıklayın) | Bir not defteri dosyası çalıştırır. Diğer dosya türleri görüntüleme için açılır.  |
| Bağlantıyı Kopyala | Y | Panoya bir dosyaya bir bağlantı kopyalar. |
| Jupyter laboratuar ortamında çalıştırma | j | Bir not defteri olan Jupyter normalde sağladığından bir geliştirici odaklı daha arabirimi JupyterLab içinde çalıştırır. |
| Önizleme | p | Dosyanın bir HTML önizlemesini açılır; Not defterleri için not defterinin salt okunur bir işleme bir önizlemedir. Daha fazla bilgi için [Önizleme](#preview) bölümü. |
| Dosyasını düzenleyin | Ben | Dosya düzenleme için açar. |
| İndirme | g | Dosya veya klasör içeriğini içeren bir zip dosyası indirir. |
| Yeniden Adlandır | a | Dosya veya klasör için yeni bir ad sorar. |
| Sil | x | Onay ister ve ardından dosyayı projeden kalıcı olarak kaldırır. Silme işlemleri geri alınamaz. |
| Taşı | m | Bir dosyayı, aynı projede farklı bir klasöre taşır. |

#### <a name="preview"></a>Önizleme

Bir dosya veya dizüstü bilgisayar İçindekiler salt okunur görünüm önizlemesidir; Not Defteri hücreleri çalıştıran devre dışı bırakıldı. Not Defteri ve dosya için bir bağlantı vardır, ancak Azure not defterleri açmadı herkes tarafından önizlemesi gösterilmektedir. Bir kullanıcı oturum açtıktan sonra kendi hesabı not defterini kopyalayabilirsiniz veya not defteri kendi yerel bilgisayarlarına yükleyebilirsiniz.

Önizleme sayfası klavye kısayollarıyla birkaç araç çubuğu komutlarını destekler:

| Komut | Klavye kısayolu | Eylem |
| --- | --- | --- |
| Paylaş | s | İçinden bağlantısını almak, sosyal medyada paylaşın, HTML eklemek için alabilir ve bir e-posta paylaşım açılan pencere görüntüler. |
| Kopyala | c  | Not defterini hesabınıza kopyalayın. |
| Çalıştırın | r | Bunu yapmak izin verilmez, Not defterini çalıştırılır. |
| İndirme | g | Not defterini bir kopyasını yükler. |

## <a name="configure-the-project-environment"></a>Proje ortamını yapılandırma

Not defterlerinizi içinde çalıştığı temel sanal makine ortamı yapılandırmak için üç yolu vardır:

- Bir kez başlatma komut dosyası Ekle
- Kurulum adımları belirtmek için projenin ortam ayarlarını kullan
- Sanal makine, bir terminal erişin.

Sanal makine başlatılır ve bu nedenle proje içindeki tüm not defterlerinin etkiler her proje yapılandırması biçimlerinin uygulanır.

### <a name="one-time-initialization-script"></a>Tek seferlik başlatma betiği

Bir sunucu projesi için ilk kez Azure not defterleri oluşturur, adlı projedeki bir dosyayı arar *aznbsetup.sh*. Bu dosya varsa, bu işlem Azure not defterleri çalışır. Betik çıktısı proje klasörünüzde depolanır *. aznbsetup.log*.

### <a name="environment-setup-steps"></a>Ortam Kurulumu adımları

Ortam yapılandırma her bir adımı oluşturmak için projenin ortam ayarlarını kullanabilirsiniz.

Proje Panosu üzerinde seçin **proje ayarları**, ardından **ortam** Burada, ekleme, kaldırma ve kurulum adımlarını proje için değiştirmek sekmesinde:

![Ortam sekmesi seçili proje ayarları açılır](media/project-settings-environment-steps.png)

Bir adım eklemek için önce seçin **+ Ekle**, bir adım türü seçip **işlemi** aşağı açılan listesi:

![Yeni ortam Kurulum adımı için işlem Seçici](media/project-settings-environment-details.png)

Ardından Proje bilgi işlemi, seçtiğiniz türüne bağlıdır:

- **Requirements.txt**: İkinci açılan listesinde seçin bir *requirements.txt* projede zaten var olan dosyayı. Ardından görüntülenen üçüncü aşağı açılan listeden bir Python sürümünü seçin. Kullanarak bir *requirements.txt* dosya, Azure not defterleri çalıştırmaları `pip install -r` ile *requirements.txt* bir not defteri sunucusu başlatma sırasında dosya. Açıkça içinde notebook paketleri yüklemeniz gerekmez.

- **Kabuk betiği**: Projedeki bir bash Kabuk betiği ikinci aşağı açılan listeden seçin (genellikle bir dosyayla *.sh* uzantısı) ortamı başlatmak için çalıştırmak istediğiniz herhangi bir komut içerir.

- **Environment.yml**: İkinci açılan listesinde seçin bir *environments.yml* conda ortamı kullanarak Python projeleri için dosya.

Adımları eklemeyi tamamladığınızda, seçin **Kaydet**.

### <a name="use-the-terminal"></a>Terminal kullanma

Proje Panosu üzerinde **Terminal** komut sunucuya doğrudan erişim sağlayan bir Linux terminal açar. Terminal içinde verileri indirmek, düzenleyebilir veya dosyalarını yönetme, işlemleri incelemek ve hatta VI ve nano gibi araçları kullanın.

> [!Note]
> Projenizin ortamında başlangıç betikleriniz varsa, terminal açma Kurulum'un devam ediyor belirten bir ileti görüntüleyebilir.

Terminalde tüm standart Linux komutlarını gönderebilir. Ayrıca `ls` gibi sanal makinede mevcut farklı ortamları görmek için giriş klasörü içinde *anaconda2_501*, *anaconda3_420*, *anaconda3_501*, *IfSharp*, ve *R*, ile birlikte bir *proje* projeyi içeren klasörü:

![Proje terminalde Azure Not Defterleri](media/project-terminal.png)

Belirli bir ortama etkilemek için bu ortam klasöre dizinleri önce değiştirin.

Python ortamları için bulabileceğinizi `pip` ve `conda` içinde *bin* her bir ortamın klasör. Yerleşik diğer adlar ortamları için de kullanabilirsiniz:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Sunucuya yapılan değişiklikleri uygulamak yalnızca dosya ve klasörleri oluşturduğunuz dışında geçerli oturum için *proje* kendisini klasör. Örneğin, proje klasöründeki bir dosyayı düzenleme oturumları arasında kalıcıdır ancak ile paketleri `pip install` değil.

> [!Note]
> Kullanırsanız `python` veya `python3`, not defterleri için kullanılmaz Python, sistem tarafından yüklenen sürümleri çağırır. İster işlemleri için izniniz yok `pip install` ya da, bu nedenle sürüme özgü diğer adlar kullandığınızdan emin olun.

## <a name="access-notebook-logs"></a>Not Defteri günlüklerine erişme

Bir not defteri çalışırken sorun yaşarsanız, jupyter tarafından sağlanan çıkış adlı bir klasörde depolanır *. nb.log*. Bu günlükler aracılığıyla erişebileceğiniz **Terminal** komut veya proje Panosu.

Jupyter yerel olarak çalıştırırken genellikle, bu bir terminal penceresinden başlatılmamış olabilir. Terminal penceresinde, çekirdek durumu gibi bir çıktı gösterir.

Günlükleri görüntülemek için terminalde aşağıdaki komutu kullanın:

```bash
cat .nb.log
```

Ayrıca, bir kod hücresine bir Python not defteri gelen komutunu da kullanabilirsiniz:

```bash
!cat .nb.log
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: Proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Bir not defteri bulut verilerine erişim](access-data-resources-jupyter-notebooks.md)
