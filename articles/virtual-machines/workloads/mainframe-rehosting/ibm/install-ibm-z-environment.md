---
title: IBM zD & T geliştirme/test ortamı Azure'a yükleme | Microsoft Docs
description: IBM Z geliştirme ve Test Ortamı (zD & T) üzerinde Azure sanal makine (VM) altyapı (Iaas) hizmet olarak dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 04/02/2019
tags: ''
keywords: ''
ms.openlocfilehash: ad02ec75dab4cb6971d0467899d80f5f745fd94b
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621305"
---
# <a name="install-ibm-zdt-devtest-environment-on-azure"></a>IBM zD & T geliştirme/test ortamı Azure'a yükleme

IBM Z sistemlerinde anabilgisayar iş yükleri için geliştirme/test ortamı oluşturmak için IBM Z geliştirme ve Test Ortamı (zD & T) üzerinde Azure sanal makine (VM) altyapı (Iaas) hizmet olarak dağıtabilirsiniz.

ZD & T, sağladığı maliyet tasarruflarından x86, uygulayabileceğiniz platformu, daha az kritik olan geliştirme ve test ortamları ve ardından güncelleştirmeleri Z sistem üretim ortamına geri gönderin. Daha fazla bilgi için [IBM ZD & T yükleme yönergeleri](https://www-01.ibm.com/support/docview.wss?uid=swg24044565#INSTALL).

Azure ve Azure Stack aşağıdaki sürümleri destekler:

- zD & T kişisel sürümü
- zD & T paralel Sysplex
- zD & T Enterprise Edition

ZD & T'in tüm sürümlerinde yalnızca x86 üzerinde Linux sistemleri, Windows Server çalıştırın. Enterprise Edition ya da Red Hat Enterprise Linux (RHEL) veya Ubuntu/Debian üzerinde desteklenir. RHEL hem Debian VM görüntüleri, Azure için kullanılabilir.

Bu makalede, zD & T Enterprise Edition'da Azure oluşturulması ve Yönetimi ortamları için zD & T Enterprise Edition web sunucusu kullanabilmeniz için ayarlama işlemini göstermektedir. ZD & T yükleme tüm ortamları yüklemez. Bunlar ayrı olarak yükleme paketleri oluşturmanız gerekir. Örneğin, uygulama geliştiricilerin denetlenen dağıtımları (ADCD) görüntüleridir birim test ortamları. IBM ortam dağıtım zip resimlerdeki bulunur. Bkz. nasıl [Azure üzerinde bir ADCD ortamı ayarlama](demo.md).

Daha fazla bilgi için [zD & T genel bakış](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zdt.overview.gs.doc/topics/c_product_overview.html) IBM bilgi Merkezi'nde.

Bu makalede, Z geliştirme ve Test Ortamı (zD & T) Enterprise Edition azure'da ayarlama işlemini göstermektedir. Ardından, zD & T Enterprise Edition web sunucusu oluşturmak ve azure'da Z tabanlı ortamlarını yönetmek için kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> IBM, zD & T Enterprise Edition'ı yalnızca geliştirme ve test ortamlarında yüklü olmasını sağlar —*değil* üretim ortamları.

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Yalnızca IBM müşteriler ve iş ortakları tarafından kullanılabilir medya erişmeniz gerekir. Daha fazla bilgi için IBM temsilcinize başvurun veya kişi bilgilerini görmek [zD & T](https://www.ibm.com/us-en/marketplace/z-systems-development-test-environment) Web sitesi.

- A [lisans sunucusu](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Bu erişim ortamları için gereklidir. Oluşturduğunuz şekilde nasıl IBM yazılımı lisansı bağlıdır:

     - **Lisans sunucusu donanım tabanlı** yazılım tüm bölümlerini erişmek gerekli rasyonel belirteçler içeren bir USB donanım gerektirir. Bu IBM edinmeniz gerekir.

     - **Yazılım tabanlı lisans sunucusu** lisanslama anahtarları yönetimi için merkezi bir sunucu ayarlamanız gerekir. Bu yöntem, tercih edilir ve yönetim sunucusuna IBM aldığınız anahtarları ayarlamanız gerekir.

## <a name="create-the-base-image-and-connect"></a>Temel görüntü oluşturma ve bağlanma

1. Azure portalında [VM oluşturma](/azure/virtual-machines/linux/quick-create-portal) istediğiniz işletim sistemi yapılandırması ile. Bu makalede bir B4ms VM (4 Vcpu ve 16 GB bellek) varsayılır Ubuntu 16.04 çalıştıran.

2. VM oluşturulduktan sonra SSH için 22, 21 FTP ve web sunucusu için 9443 gelen bağlantı noktalarını açın.

3. Gösterilen SSH kimlik bilgilerini alma **genel bakış** VM dikey penceresinde **Connect** düğmesi. Seçin **SSH** sekmesini ve SSH oturum açma komutunu panoya kopyalayın.

4. Oturum bir [Bash Kabuk](/azure/cloud-shell/quickstart) yerel bilgisayar ve komutu yapıştırın. Biçiminde olacaktır **ssh\<kullanıcı kimliği\>\@\<IP adresi\>** . Kimlik bilgileriniz istendiğinde, giriş dizininizi bir bağlantı kurmak için girin.

## <a name="copy-the-installation-file-to-the-server"></a>Yükleme dosyasını sunucuya kopyalayın.

Web sunucusu için yükleme dosyasıdır **ZDT\_yükleme\_EE\_V12.0.0.1.tgz**. IBM tarafından sağlanan media bulunmaktadır. Bu dosya, Ubuntu VM'ye yüklemeniz gerekir.

1. Komut satırından her şeyi yeni oluşturulan görüntüyü güncel olduğundan emin olmak için aşağıdaki komutu girin:

    ```
    sudo apt-get update
    ```

2. Yüklemek için bir dizin oluşturun:

    ```
    mkdir ZDT
    ```

3. Dosya VM yerel makinenize kopyalayın:

    ```
    scp ZDT_Install_EE_V12.0.0.1.tgz  your_userid@<IP Address /ZDT>   =>
    ```
    
> [!NOTE]
> Bu komut yükleme dosyasını istemcinizi Windows veya Linux çalıştıran bağlı olarak farklılık gösterir, giriş dizini ZDT dizine kopyalar.

## <a name="install-the-enterprise-edition"></a>Enterprise Edition'ı yükleme

1. ZDT dizinine gidin ve ZDT sıkıştırmasını\_yükleme\_EE\_V12.0.0.1.tgz dosyası için aşağıdaki komutları kullanın:

    ```
    cd ZDT
    chmod 755 ZDT\_Install\_EE\_V12.0.0.0.tgz
    ```

2. Yükleyiciyi çalıştırın:

    ```
    ./ZDT_Install_EE_V12.0.0.0.x86_64
    ```

3. Seçin **1** kuruluş sunucusu yüklemek için.

4. Tuşuna **Enter** ve lisans sözleşmelerini dikkatle okuyun. Lisans sonunda girin **Evet** devam etmek için.

5. Yeni oluşturulan kullanıcı parolasını değiştirmek isteyip istemediğiniz sorulduğunda **ibmsys1**, komutunu **sudo parola ibmsys1** ve yeni parolayı girin.

6. Yüklemenin başarılı olup olmadığını doğrulamak için girin

    ```
    dpkg -l | grep zdtapp
    ```

7. Çıkış dizesi içerdiğini doğrulayın **zdtapp 12.0.0.0**gösteren, paket gaz başarıyla yüklendi

### <a name="starting-enterprise-edition"></a>Enterprise Edition'ı başlatma

Web sunucusu başlatır, yükleme işlemi sırasında oluşturulan zD & T kullanıcı kimliği altında çalıştığı olduğunu aklınızda bulundurun.

1. Web sunucusunu başlatmak için aşağıdaki komutu çalıştırmak için kök kullanıcı Kimliğini kullanın:

    ```
    sudo /opt/ibm/zDT/bin/startServer
    ```

2. Şuna benzer komut dosyası tarafından URL çıktısını kopyalayın:

    ```
    https://<your IP address or domain name>:9443/ZDTMC/login.htm
    ```

3. Bir web tarayıcısı zD & T yükleme için yönetim bileşeni açmak için URL'yi yapıştırın.

## <a name="next-steps"></a>Sonraki adımlar

[Yedekleme bir uygulama geliştiricilerin denetlenen dağıtım (ADCD) IBM zD & T v1 ayarlayın](./demo.md)
