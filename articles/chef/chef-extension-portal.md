---
title: Azure portalından Chef İstemcisi'ni yükleme
description: Azure portalından Chef istemcinizi yapılandırma ve dağıtma hakkında bilgi edinin
keywords: Azure, chef, devops, istemci, yükleme, portalı
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: cf7afb50006fb273b4d685f9e4259be1cb60fe4e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60563889"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Azure portalından Chef İstemcisi'ni yükleme
Azure portalından Chef istemci uzantısını doğrudan üzerine bir Linux veya Windows makinesi ekleyebilirsiniz. Bu makalede yeni bir Linux sanal makine kullanarak işlemi boyunca size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Chef**: Chef etkin bir hesabınız yoksa, oturum açmak için bir [ücretsiz deneme sürümü barındırılan Chef](https://manage.chef.io/signup). Bu makaledeki yönergeleri takip etmek için aşağıdaki değerleri Chef hesabınızdan gerekir:
  - organization_validation anahtarı
  - RB
  - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Yeni bir Linux sanal makinesinde Chef uzantısını yükle
Bu bölümde, bir Linux makine oluşturmak için öncelikle Azure portalını kullanacaksınız. İşlem sırasında yeni bir sanal makine üzerinde Chef uzantıyı yüklemek nasıl görürsünüz.

1. [Azure portala](https://portal.azure.com) gidin.

1. Sol taraftaki menüden **sanal makineler** seçeneği. Varsa **sanal makineler** seçeneği mevcut seçeneğini değil **tüm hizmetleri** seçip **sanal makineler**.

1. Üzerinde **sanal makineler** sekmesinde **Ekle**.

    ![Azure portalında yeni bir sanal makine ekleyin](./media/chef-extension-portal/add-vm.png)

1. Üzerinde **işlem** sekmesinde, istenen işletim sistemi seçin. Bu Tanıtım için **Ubuntu Server** seçilir.

1. Üzerinde **Ubuntu Server** sekmesinde **Ubuntu Server 16.04 LTS**.

    ![Bir Ubuntu sanal makinesi oluştururken ihtiyacınız olan sürümü belirtin.](./media/chef-extension-portal/ubuntu-server-version.png)

1. Üzerinde **Ubuntu Server 16.04 LTS** sekmesinde **Oluştur**.

    ![Ubuntu, ürün hakkında ek bilgi sağlar.](./media/chef-extension-portal/create-vm.png)

1. Üzerinde **sanal makine oluşturma** sekmesinde **Temelleri**.

1. Üzerinde **Temelleri** sekmesinde, aşağıdaki değerleri belirtin ve ardından **Tamam**.

   - **Ad** -yeni bir sanal makine için bir ad girin.
   - **VM disk türü** -seçeneklerinden birini belirtin **SSD** veya **HDD** depolama disk türü. Azure'da sanal makine disk türleri hakkında daha fazla bilgi için bkz [bir disk türü seçin](../virtual-machines/windows/disks-types.md).
   - **Kullanıcı adı** -sanal makinede yönetici ayrıcalıkları verilmiş bir kullanıcı adı girin.
   - **Kimlik doğrulama türü** - seçin **parola**. Belirleyebilirsiniz **SSH ortak anahtarı**ve bir SSH ortak anahtar değeri sağlayın. Bu Tanıtım (ve ekran görüntüleri), amacıyla **parola** seçilir.
   - **Parola** ve **parolayı onayla** -kullanıcı için bir parola girin.
   - **Azure Active Directory ile oturum aç** - seçin **devre dışı bırakılmış**.
   - **Abonelik** -birden fazla aboneliğiniz varsa, istediğiniz Azure aboneliğini seçin.
   - **Kaynak grubu** -kaynak grubunuz için bir ad girin.
   - **Konum** - seçin **Doğu ABD**.

     ![Bir sanal makine oluşturmak için sekmesinde temelleri](./media/chef-extension-portal/add-vm-basics.png)

1. Üzerinde **bir boyut seçin** sekmesinde, sanal makine boyutu seçin ve ardından **seçin**.

1. Üzerinde **ayarları** önceki sekmeleri seçtiğiniz değerlere göre için sekmesinde değerleri çoğunu doldurulur. **Uzantılar**'ı seçin.

     ![Ayarlar sekmesi aracılığıyla sanal makine uzantıları eklenir](./media/chef-extension-portal/add-vm-select-extensions.png)

1. Üzerinde **uzantıları** sekmesinde **uzantısı ekleme**.

     ![Bir sanal makine için bir uzantı eklemek için Add uzantı seçin](./media/chef-extension-portal/add-vm-add-extension.png)

1. Üzerinde **yeni kaynak** sekmesinde **Linux Chef uzantısı (1.2.3-Beta)** .

     ![Chef, Linux ve Windows sanal makineleri için uzantıları sahip](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Üzerinde **Linux Chef uzantısı** sekmesinde **Oluştur**.

1. Üzerinde **uzantı yükleme** sekmesinde, aşağıdaki değerleri belirtin ve ardından **Tamam**.

    - **Chef sunucu URL'si** -Örneğin, kuruluş adını içeren Chef sunucu URL'si girin *https://api.chef.io/organization/mycompany* .
    - **Chef düğüm adı** -Chef düğüm adı girin. Bu, herhangi bir değer olabilir.
    - **Çalıştırma listesi** -makineye eklenir Chef çalıştırma listesi girin. Bu boş bırakılabilir.
    - **İstemci adı doğrulama** -Chef doğrulama istemci adı girin. Örneğin, *tarcher Doğrulayıcı*.
    - **Doğrulama anahtarı** -makinelerinizin önyükleme yaparken kullanılan doğrulama anahtarı içeren bir dosya seçin.
    - **İstemci yapılandırma dosyası** -chef istemci için bir yapılandırma dosyası seçin. Bu boş bırakılabilir.
    - **Chef istemci sürümü** -yüklemek için chef istemci sürümü girin. Bu boş bırakılabilir. Boş bir değer, en son sürümünü yükler.
    - **SSL doğrulama modu** -seçin **hiçbiri** veya **eş**. *Hiçbiri* Tanıtıma seçilmedi.
    - **Şef ortamının** -bu düğüm, bir üyesi olmalıdır Şef ortamının girin. Bu boş bırakılabilir.
    - **Databag gizlilik şifrelenmiş** -bu makine şifrelenmiş Databag erişimi olması gereken gizli dizi içeren bir dosya seçin. Bu boş bırakılabilir.
    - **Chef sunucu SSL sertifikası** -Chef sunucunuza atanan SSL sertifikasını seçin. Bu boş bırakılabilir.

      ![Bir Linux sanal makinesinde Chef sunucusu yükleme](./media/chef-extension-portal/install-extension.png)

1. İçin döndürülürken **uzantıları** sekmesinde **Tamam**.

1. İçin döndürülürken **ayarları** sekmesinde **Tamam**.

1. İçin döndürülürken **Oluştur** sekme (Bu temsil eder, seçili ve girilen seçeneklerin özeti), bilgilerin doğrulayın yanı sıra **kullanım koşullarını**seçip **Oluştur**.

Oluşturma ve Chef uzantısı ile sanal makine dağıtma işlemi tamamlandığında, başarı veya başarısızlık işlemin bir bildirim gösterir. Ayrıca, oluşturulduktan sonra yeni bir sanal makine için kaynak sayfasında Azure Portalı'nda otomatik olarak açılır.

![Bir Linux sanal makinesinde Chef sunucusu yükleme](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)
