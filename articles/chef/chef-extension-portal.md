---
title: Azure portalından Chef İstemcisi'ni yükleme
description: Azure portalından Chef istemcinizi yapılandırmak ve dağıtmak hakkında bilgi edinin
keywords: Azure, chef, devops, istemci, yükleme, portal
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 52f34361d7c1f3dff47f2571a714b8be7764cc6f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Azure portalından Chef İstemcisi'ni yükleme
Bir Linux veya Windows sanal makine Azure portalından oluşturmak veya Chef uzantı sanal makineye ekleyebilirsiniz. Bu makalede yeni bir Linux sanal makine kullanarak bu işleminde size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar
- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Chef**: etkin bir Chef hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü barındırılan Chef](https://manage.chef.io/signup). Bu makaledeki yönergeleri birlikte izlemek için aşağıdaki değerleri Chef hesabınızdan gerekir: 
    - organization_validation anahtarı
    - RB
    - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Yeni bir Linux sanal makine üzerinde Chef uzantısını yükleyin
Bu bölümde, ilk Azure Portalı'nı Linux makine oluşturmak için kullanmanız. İşlemi sırasında yeni sanal makineye Chef uzantıyı yüklemek nasıl görürsünüz.

1. Gözat [Azure portal](http://portal.azure.com).

1. Sol taraftaki menüden seçin **sanal makineleri** seçeneği. Varsa **sanal makineleri** seçeneği mevcut, select değil **tüm hizmetleri** ve ardından **sanal makineleri**.

1. Üzerinde **sanal makineleri** sekmesine **Ekle**.

    ![Azure portalında yeni bir sanal makine ekleyin](./media/chef-extension-portal/add-vm.png)

1. Üzerinde **işlem** sekmesinde, istenen işletim sistemi seçin. Bu Tanıtım için **Ubuntu Server** seçilir.

1. Üzerinde **Ubuntu Server** sekmesine **Ubuntu Server 16.04 LTS**.

    ![Ubuntu sanal makine oluştururken ihtiyacınız olan sürümü belirtin](./media/chef-extension-portal/ubuntu-server-version.png)

1. Üzerinde **Ubuntu Server 16.04 LTS** sekmesine **oluşturma**.

    ![Ubuntu ürünlerinin hakkında ek bilgiler sağlar](./media/chef-extension-portal/create-vm.png)

1. Üzerinde **sanal makine oluşturma** sekmesine **Temelleri**.

1. Üzerinde **Temelleri** sekmesinde, aşağıdaki değerleri belirtin ve ardından **Tamam**.

    - **Ad** -yeni bir sanal makine için bir ad girin.
    - **VM disk türü** -belirtin **SSD** veya **HDD** depolama disk türü. Azure sanal makine disk türleri hakkında daha fazla bilgi için bkz: [yüksek performanslı Premium depolama ve VM'ler için yönetilen diskleri](/azure/virtual-machines/windows/premium-storage).
    - **Kullanıcı adı** -sanal makinede yönetici ayrıcalıkları verilmiş bir kullanıcı adı girin.
    - **Kimlik doğrulama türü** - seçin **parola**. Öğesini de seçebilirsiniz **SSH ortak anahtarını**ve SSH ortak anahtar değeri sağlayın. Bu demo (ve ekran görüntüleri içinde) amacıyla **parola** seçilir.
    - **Parola** ve **parolayı onayla** -kullanıcı için bir parola girin.
    - **Azure Active Directory ile oturum aç** - seçin **devre dışı**.
    - **Abonelik** -birden fazla varsa desired Azure aboneliğini seçin.
    - **Kaynak grubu** -kaynak grubunuz için bir ad girin.
    - **Konum** - seçin **Doğu ABD**.

    ![Sanal makine oluşturmak için temel kavramları sekmesi](./media/chef-extension-portal/add-vm-basics.png)

1. Üzerinde **bir boyutu seçin** sekmesinde, sanal makine için boyutu seçin ve ardından **seçin**.

1. Üzerinde **ayarları** sekmesi, değerleri çoğunu doldurulmuş önceki sekmeleri seçtiğiniz değerlere dayalı için. Seçin **uzantıları**.

    ![Uzantıları ayarları sekmesi aracılığıyla sanal makineleri ekleniyor](./media/chef-extension-portal/add-vm-select-extensions.png)

1. Üzerinde **uzantıları** sekmesine **uzantısı Ekle**.

    ![Uzantı sanal makineye eklemek için Add uzantı Seç](./media/chef-extension-portal/add-vm-add-extension.png)

1. Üzerinde **yeni kaynak** sekmesine **Linux Chef uzantısı (1.2.3)**.

    ![Linux ve Windows sanal makineler için Uzantılar Chef sahip](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Üzerinde **Linux Chef uzantısı** sekmesine **oluşturma**.

1. Üzerinde **yükleme uzantısı** sekmesinde, aşağıdaki değerleri belirtin ve ardından **Tamam**.

    - **Chef sunucu URL'si** -Chef sunucu kuruluş adını içeren bir URL girin. Kullandım *https://api.chef.io/organization/hessco* gösteri için.
    - **Chef düğüm adı** -Chef düğüm adı girin. Bu, herhangi bir değer olabilir.
    - **Liste çalıştırmak** -makineye eklenir çalıştırmak Chef listesini girin. Bu boş olabilir.
    - **Doğrulama istemci adı** -Chef doğrulama istemci adını girin. Kullandım *tarcher Doğrulayıcı* gösteri için.
    - **Doğrulama anahtarı** -makinelerinizi önyükleme sırasında kullanılan doğrulama anahtarı içeren bir dosya seçin. 
    - **İstemci yapılandırma dosyası** -chef istemci için bir yapılandırma dosyası seçin. Bu boş olabilir.
    - **Chef istemci sürümü** -yüklemek için chef istemci sürümü girin. Boş bir değer, en son sürümünün yüklü neden olur. Bu boş olabilir.
    - **SSL doğrulama modu** -seçin **hiçbiri** veya **eş**. Seçtiğim *hiçbiri* gösteri için.
    - **Chef ortamı** -bu düğüm, bir üyesi olmalıdır Chef ortamı girin. Bu boş olabilir.
    - **Databag gizlilik şifrelenmiş** -şifrelenmiş Databag bu makineye erişimi için gizli anahtarı içeren bir dosya seçin. Bu boş olabilir.
    - **Chef sunucu SSL sertifikası** -Chef sunucunuza atanan SSL sertifikasını seçin. Bu boş olabilir.

    ![Linux sanal bir makinede Chef sunucusu yükleme](./media/chef-extension-portal/install-extension.png)

1. Döndürülen zaman **uzantıları** sekmesine **Tamam**.

1. Döndürülen zaman **ayarları** sekmesine **Tamam**.

1. Döndürülen zaman **oluşturma** (seçili ve girilen seçenekleri özetini temsil eden) sekmesini bilgileri doğrulayın yanı sıra **kullanım koşulları**seçip **oluşturma**.

Oluşturma ve dağıtma Chef uzantılı sanal makine işlemi tamamlandığında, başarı veya başarısızlık işleminin bir bildirim gösterir. Ayrıca, bir kez oluşturulduğunda yeni bir sanal makine için kaynak sayfası Azure Portalı'nda otomatik olarak açılır.

![Linux sanal bir makinede Chef sunucusu yükleme](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Chef kullanarak Azure'da Windows sanal makine oluşturma](/azure/virtual-machines/windows/chef-automation)