---
title: Avere vFXT depolama - Azure'ı yapılandırma
description: Bir arka uç depolama sistemi için Azure, Avere vFXT için ekleme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 6d35d5cdeafb80a36f910d71393802a3affb4df8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515894"
---
# <a name="configure-storage"></a>Depolama alanını yapılandırma

Bu adım bir arka uç depolama sistemi vFXT kümeniz için ayarlar.

> [!TIP]
> Avere vFXT küme birlikte yeni bir Azure Blob kapsayıcısı oluşturduysanız, bu kapsayıcı zaten kullanılmak üzere ayarlanmış ve depolama alanı eklemek gerekmez.

Yeni bir Blob kapsayıcısı ile kümenizi oluşturmadıysanız, ya da bir ek donanım veya bulut tabanlı depolama sistemi eklemek istiyorsanız aşağıdaki yönergeleri izleyin.

İki ana görevi vardır:

1. [Bir çekirdek dosyalayıcı oluşturma](#create-a-core-filer), mevcut bir depolama sistemi ya da bir Azure depolama hesabı vFXT kümenizi bağlanır.

1. [Ad alanı birleşim oluşturma](#create-a-junction), istemcilerin bağlar yolunu tanımlar.

Bu adımları Avere Denetim Masası'nı kullanın. Okuma [vFXT kümeye erişmek](avere-vfxt-cluster-gui.md) nasıl kullanılacağını öğrenin.

## <a name="create-a-core-filer"></a>Bir çekirdek dosyalayıcı oluşturma

"Çekirdek dosyalayıcı", bir arka uç depolama sistemi vFXT bir terimdir. Depolama, bir donanım NAS Gereci NetApp veya Isilon gibi veya Bulut nesne deposu olabilir. Çekirdek filtreleri hakkında daha fazla bilgi bulunabilir [Avere içinde küme ayarları Kılavuzu](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#managing-core-filers).

Bir çekirdek dosyalayıcı eklemek için iki ana tür çekirdek filtrelerin birini seçin:

  * [NAS çekirdek dosyalayıcı](#nas-core-filer) -NAS çekirdek dosyalayıcı eklemeyi açıklar 
  * [Azure depolama bulut çekirdek dosyalayıcı](#azure-storage-cloud-core-filer) -bulut çekirdek dosyalayıcı bir Azure depolama hesabı eklemeyi açıklar

### <a name="nas-core-filer"></a>NAS çekirdek dosyalayıcı

Bir şirket içi NetApp NAS çekirdek dosyalayıcı olabilir veya Isilon ya da bulutta bir NAS uç noktası. Depolama sistemine güvenilir bir yüksek hızlı bağlantıyı Avere vFXT kümeye - Örneğin, 1 GB/sn ExpressRoute bağlantı (VPN değil) - olmalıdır ve kullanılan NAS dışarı aktarmaları için küme kök erişim vermelisiniz.

Aşağıdaki adımlar bir NAS çekirdek gösterecek şekilde ekleyin:

1. Avere Denetim Masası'ndan tıklayın **ayarları** en üstteki sekmedeki.

1. Tıklayın **çekirdek dosyalayıcı** > **yönetme çekirdek filtrelerin** soldaki.

1. **Oluştur**’a tıklayın.

   ![Oluştur düğmesine imleci ile Ekle yeni çekirdek dosyalayıcı sayfasının ekran görüntüsü](media/avere-vfxt-add-core-filer-start.png)

1. Sihirbaz gerekli bilgileri doldurun: 

   * Çekirdek gösterecek şekilde adlandırın.
   * Varsa bir tam etki alanı adı (FQDN) belirtin. Aksi takdirde, bir IP adresi veya ana bilgisayar adı, çekirdek dosyalayıcı için çözümler sağlar.
   * Dosyalayıcı sınıfınıza listeden seçin. Emin değilseniz, belirleyin **diğer**.

     ![Çekirdek dosyalayıcı adı ve tam etki alanı adı ekleme yeni çekirdek dosyalayıcı sayfasının ekran görüntüsü](media/avere-vfxt-add-core-filer.png)
  
   * Tıklayın **sonraki** ve önbellek İlkesi'ni seçin. 
   * Tıklayın **dosyalayıcı ekleme**.
   * Daha ayrıntılı bilgi için bkz [yeni bir NAS ekleme çekirdek dosyalayıcı](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html) Avere içinde küme ayarları Kılavuzu.

Ardından, devam [bir birleşim oluşturma](#create-a-junction).  

### <a name="azure-storage-cloud-core-filer"></a>Azure depolama bulut çekirdek dosyalayıcı

VFXT kümenizin arka uç depolama alanı olarak Azure Blob Depolama kullanmak için boş bir kapsayıcı bir çekirdek dosyalayıcı eklemeniz gerekir.

> [!TIP] 
> Bir blob kapsayıcısı Avere vFXT küme oluşturma aynı anda oluşturmayı seçerseniz, dağıtım şablonu veya betik bir depolama kapsayıcısı oluşturur, bir çekirdek dosyalayıcı tanımlar ve ad alanı birleşim vFXT kümesi oluşturmanın bir parçası oluşturur. Şablon, ayrıca kümenin sanal ağ içindeki depolama hizmet uç noktası oluşturur. 

BLOB Depolama, kümeye ekleme, bu görevleri gerektirir:

* Depolama hesabı oluşturma (aşağıda 1 adım)
* (Adım 2-3) boş bir Blob kapsayıcısı oluşturma
* Depolama erişim anahtarı vFXT kümesi (4-6. adım) için bir bulut kimlik bilgisi olarak Ekle
* Blob kapsayıcısı çekirdek dosyalayıcı vFXT kümenin (adım 7-9) olarak Ekle
* Çekirdek dosyalayıcı bağlamak için istemcilerin kullandığı bir ad alanı birleşim oluşturun ([bir birleşim oluşturma](#create-a-junction), aynı donanım ve bulut depolama için)

Küme oluşturulduktan sonra BLOB Depolama eklemek için aşağıdaki adımları izleyin. 

1. Bu ayarlar ile bir genel amaçlı V2 depolama hesabı oluşturun:

   * **Abonelik** - vFXT kümeyle aynı
   * **Kaynak grubu** - (isteğe bağlı) vFXT küme grubu olarak aynı
   * **Konum** - vFXT kümeyle aynı
   * **Performans** - standart (Premium depolama desteklenmiyor)
   * **Hesap türü** -genel amaçlı V2 (depolama v2)
   * **Çoğaltma** -yerel olarak yedekli depolama (LRS)
   * **Erişim katmanı** - sık erişimli
   * **Güvenli aktarım gereklidir** -(varsayılan olmayan değer) bu seçeneği devre dışı
   * **Sanal ağlar** - gerekli değil

   Azure portalını kullanın veya "Azure'a dağıtın" düğmeye tıklayın.

   [![Depolama hesabı oluşturmak için](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fstorageaccount%2Fazuredeploy.json)

1. Hesap oluşturulduktan sonra depolama hesabı sayfasına göz atın.

   ![Azure portalında yeni depolama hesabı](media/avere-vfxt-new-storage-acct.png)

1. Tıklayarak bir blob kapsayıcısı oluşturursunuz **Blobları** genel bakış sayfasında ve ardından **+ kapsayıcı**. Herhangi bir kapsayıcı adı kullanın ve erişim ayarlandığından emin olun **özel**.

   ![Depolama BLOB'ları sayfası mevcut kapsayıcı yok](media/avere-vfxt-blob-no-container.png)

1. Azure depolama hesabı anahtarını tıklayarak **erişim anahtarları** altında **ayarları**:

   ![Anahtar kopyalama için azure portalında GUI](media/avere-vfxt-copy-storage-key.png) 

1. Kümenizin Avere Denetim Masası'nı açın. Tıklayın **ayarları**ve daha sonra **küme** > **bulut kimlik bilgileri** sol gezinti bölmesinde. Bulut kimlik bilgileri sayfasında tıklayın **kimlik bilgileri Ekle**.

   ![Bulut kimlik bilgileri yapılandırma sayfasında kimlik bilgileri Ekle düğmesine tıklayın](media/avere-vfxt-new-credential-button.png)

1. Bulut çekirdek Fili Oluşturucu için bir kimlik bilgisi oluşturmak için aşağıdaki bilgileri doldurun: 

   | Alan | Değer |
   | --- | --- |
   | Kimlik bilgisi adı | açıklayıcı bir ad |
   | Hizmet türü | (Azure depolama erişim anahtarını seçin) |
   | Kiracı | Depolama hesabı adı |
   | Abonelik | Abonelik kimliği |
   | Depolama erişim anahtarı | Azure depolama hesabı anahtarı (önceki adımda kopyaladığınız) | 

   **Gönder**'e tıklayın.

   ![Bulut kimlik bilgisi form Avere Denetim Masası'ndaki tamamlandı](media/avere-vfxt-new-credential-submit.png)

1. Ardından, çekirdek dosyalayıcı oluşturun. Avere Denetim Masası'ndaki sol tarafındaki tıklayın **çekirdek dosyalayıcı** >  **yönetme çekirdek filtrelerin**. 

1. Tıklayın **Oluştur** düğmesini **yönetme çekirdek filtrelerin** Ayarları sayfası.

1. Sihirbaz doldurun:

   * Dosya türünü seçin **bulut**. 
   * Yeni çekirdek dosyalayıcı adlandırın ve tıklatın **sonraki**.
   * Varsayılan önbellek ilkesini kabul edin ve üçüncü sayfasına devam edin.
   * İçinde **hizmet türünü**, seçin **Azure depolama**. 
   * Daha önce oluşturduğunuz kimlik bilgilerini seçin.
   * Ayarlama **demetine içeriği** için **boş**
   * Değişiklik **sertifika doğrulama** için **devre dışı**
   * Değişiklik **sıkıştırma modu** için **yok**  
   * **İleri**’ye tıklayın.
   * Dördüncü sayfada kapsayıcıda adını **demet adı** olarak *storage_account_name*/*container_name*.
   * İsteğe bağlı olarak, **şifreleme türü** için **hiçbiri**.  Azure depolama, varsayılan olarak şifrelenir.
   * Tıklayın **dosyalayıcı ekleme**.

   Daha ayrıntılı bilgi için okuma [yeni bir bulut çekirdek dosyalayıcı ekleme](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html>) Avere küme yapılandırma kılavuzu. 

Sayfa yenilenir ve yeni çekirdek gösterecek şekilde görüntülemek için sayfayı yenileyebilirsiniz.

Ardından, için ihtiyacınız [bir birleşim oluşturma](#create-a-junction).

## <a name="create-a-junction"></a>Bir bağlantı oluşturun

Bir birleşim istemciler için oluşturduğunuz bir yoludur. İstemciler, bağlama yolunu ve seçtiğiniz hedefe ulaşır.

Örneğin, aşağıdakileri oluşturabilirsiniz `/avere/files` , NetApp çekirdek gösterecek şekilde eşlemek için `/vol0/data` dışarı aktarma ve `/project/resources` alt.

Merkezleriyle hakkında daha fazla bilgi bulunabilir [Avere küme yapılandırma Kılavuzu'nun ad alanı bölümünde](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html).

Avere Denetim Masası Ayarları arabirimi aşağıdaki adımları izleyin:

* Tıklayın **VServer** > **Namespace** sol üst.
* İle başlayan bir ad alanı yolu sağlamak / (eğik çizgi) gibi ``/avere/data``.
* Çekirdek dosyalayıcı seçin.
* Çekirdek dosyalayıcı Dışa Aktar'ı seçin.
* **İleri**’ye tıklayın.

  ![Birleşim, çekirdek dosyalayıcı ve dışarı aktarma tamamlandı alanlarla "yeni birleşim Ekle" sayfasının ekran görüntüsü](media/avere-vfxt-add-junction.png)

Birleşim birkaç saniye sonra görünür. Ek merkezleriyle gerektiği şekilde oluşturun.

Birleşim oluşturulduktan sonra istemciler için [Avere vFXT küme bağlama](avere-vfxt-mount-clients.md) dosya sistemine erişebilir.
