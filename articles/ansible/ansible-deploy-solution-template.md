---
title: Hızlı Başlangıç - Azure'da CentOS for Ansible çözüm şablonu dağıtma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure ile çalışacak şekilde yapılandırılmış araçları ile birlikte Azure'da barındırılan bir CentOS sanal makineye Ansible çözüm şablonu dağıtmayı öğrenirsiniz.
keywords: ansible'ı, azure, devops, çözüm şablonu, sanal makine, azure kaynakları, centos ve red hat için yönetilen kimlikleri
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 58f28d5cf7d31a3fbddc8e1ca18be4dbcf617f61
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65231008"
---
# <a name="quickstart-deploy-the-ansible-solution-template-for-azure-to-centos"></a>Hızlı Başlangıç: Azure'da CentOS for Ansible çözüm şablonu dağıtma

Azure için Ansible çözüm şablonu, ansible'ı ve Azure ile çalışacak şekilde yapılandırılmış Araçları Paketi ile birlikte bir CentOS sanal makinede Ansible örneği yapılandırmak için tasarlanmıştır. Araçlar şunları içerir:

- **Azure modülleri Ansible** - [Azure modülleri Ansible](./ansible-matrix.md) oluşturmak ve azure'da altyapınızı yönetmenize olanak sağlayan modülleri dizisi olan. Bu modülleri en son sürümünü varsayılan olarak dağıtılır. Ancak, çözüm şablonu dağıtım işlemi sırasında ortamınız için uygun bir sürüm numarası belirtebilirsiniz.
- **Azure komut satırı arabirimi (CLI) 2.0** - [Azure CLI 2.0](/cli/azure/?view=azure-cli-latest) Azure kaynaklarını yönetmek için bir platformlar arası komut satırı deneyimidir. 
- **Kimlikler Azure kaynakları için yönetilen** - [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) özellik bulut uygulama kimlik bilgilerinin güvenli tutma sorunu giderir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="deploy-the-ansible-solution-template"></a>Ansible çözüm şablonu Dağıt

1. Gözat [Ansible çözüm şablonu Azure Market'te](https://azuremarketplace.microsoft.com/en-%20%20us/marketplace/apps/azure-oss.ansible?tab=Overview).

1. Seçin **alma şimdi**.

1. Kullanım koşullarını, gizlilik ilkesi ve kullanım Azure Market koşulları ayrıntılı bir pencere görüntülenir. Seçin **devam**.

1. Azure portalında görünür ve çözüm şablonu açıklar Ansible sayfasını görüntüler. **Oluştur**’u seçin.

1. İçinde **oluşturma Ansible** sayfasında, birden çok sekme görürsünüz. Üzerinde **Temelleri** sekmesinde, gerekli bilgileri girin:

   - **Ad** -Ansible örneğinizin adını belirtin. Tanıtım amaçlı olarak adı `ansiblehost` kullanılır.
   - **Kullanıcı adı:** -Ansible örneğine erişimi olan kullanıcı adını belirtin. Tanıtım amaçlı olarak adı `ansibleuser` kullanılır.
   - **Kimlik doğrulaması türü:** -seçin **parola** veya **SSH ortak anahtarı**. Tanıtım amaçlı olarak **SSH ortak anahtarı** seçilir.
   - **Parola** ve **parolayı onayla** - seçerseniz **parola** için **kimlik doğrulama türü**, bu değerler için parolanızı girin.
   - **SSH ortak anahtarı** - seçerseniz **SSH ortak anahtarı** için **kimlik doğrulama türü**, RSA ortak anahtarını tek satırlı biçimde başlayarak - girin `ssh-rsa`.
   - **Abonelik** -açılan listeden Azure aboneliğinizi seçin.
   - **Kaynak grubu** - açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** ve yeni bir kaynak grubu için bir ad belirtin. Tanıtım amacıyla, yeni bir kaynak grubu adında `ansiblerg` kullanılır.
   - **Konum** -senaryonuz için uygun olan aşağı açılan listeden konumu seçin.

     ![Azure portal Ansible temel ayarları sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-1.png)

1. **Tamam**’ı seçin.

1. İçinde **ek ayarlar** sekmesinde, gerekli bilgileri girin:

   - **Boyutu** -Azure portal varsayılan olarak standart boyutu. Kendi senaryonuza karşılar farklı bir boyut belirlemek için farklı boyutlarının listesini görüntülemek için oku seçin.
   - **VM disk türü** -seçin **SSD** (Premium Solid-State sürücü) veya **HDD** (Sabit Disk sürücüsü). Tanıtım amaçlı olarak **SSD** performans avantajlarından için seçilir. Disk depolama türlerinin her biri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
       - [VM’ler için yüksek performanslı Premium Depolama ve yönetilen diskler](/azure/virtual-machines/windows/premium-storage)
       - [Standart SSD yönetilen diskler için Azure sanal makine iş yükleri](/azure/virtual-machines/windows/disks-standard-ssd)
   - **Genel IP adresi** -sanal makine dışında sanal makine ile iletişim kurmak istiyorsanız bu ayarı belirtin. Varsayılan ada sahip yeni bir ortak IP adresi olan `ansible-pip`. Farklı bir IP adresi belirtmek için bu IP adresi atamasının adı ve SKU gibi öznitelikleri - oku seçip belirtin. 
   - **Etki alanı adı etiketi** -genel kullanıma yönelik sanal makine etki alanı adını girin. Ad benzersiz ve karşılayan adlandırma gereksinimlerini olmalıdır. Sanal makine için ad belirtme hakkında daha fazla bilgi için bkz. [Azure kaynakları için adlandırma kuralları](/azure/architecture/best-practices/naming-conventions).
   - **Ansible sürüm** -bir sürüm numarası ya da değer belirtmeniz `latest` en son sürümünü dağıtmak için. Bilgi simgesi seçin **Ansible sürüm** kullanılabilir sürümler hakkında daha fazla bilgi için.

     ![Azure portal Ansible ek ayarlar sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-2.png)

1. **Tamam**’ı seçin.

1. İçinde **Ansible Tümleştirme ayarlarını** sekmesinde, kimlik doğrulama türünü belirtin. Azure kaynaklarını güvenli hale getirme hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](/azure/active-directory/managed-identities-azure-resources/overview).

    ![Azure portal Ansible tümleştirme ayarları sekmesinde](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-3.png)

1. **Tamam**’ı seçin.

1. **Özeti** doğrulama işlemini gösteren ve Ansible dağıtımı için belirtilen ölçütleri listeleme sayfasını görüntüler. Sekmesinin altındaki bir bağlantısını sağlar **şablon ve parametreleri indir** Azure desteklenen diller ve platformlar ile kullanmak için. 

     ![Azure portal sekmesini Ansible Özet sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-4.png)

1. **Tamam**’ı seçin.

1. Zaman **Oluştur** sekmesi görüntülenirse, seçin **Tamam** ansible'ı dağıtmak için.

1. Seçin **bildirimleri** Ansible dağıtımını izlemek için portal sayfasının üst simge. Dağıtım tamamlandıktan sonra seçin **kaynak grubuna gidin**. 

     ![Azure portal sekmesini Ansible Özet sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-complete.png)

1. Kaynak grubu sayfasındaki Ansible ana bilgisayarınızın IP adresini alın ve ansible'ı kullanarak Azure kaynaklarınızı yönetmek oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Hızlı Başlangıç: Ansible'ı kullanarak Azure'da bir Linux sanal makinesi yapılandırma](/azure/virtual-machines/linux/ansible-create-vm)