---
title: "Azure Service Fabric ile çalışmak için Mac OS X’te geliştirme ortamınızı ayarlama | Microsoft Docs"
description: "Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Mac OS X üzerinde uygulama derlemek için hazır hale gelirsiniz."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2017
ms.author: saysa
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 8b4fc0ab9034263418cac42ced203035e0a8fcad
ms.contentlocale: tr-tr
ms.lasthandoff: 08/23/2017

---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Mac OS X’te geliştirme ortamınızı ayarlama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Mac OS X kullanarak Linux kümelerinde çalışacak Service Fabric uygulamaları derleyebilirsiniz. Bu makalede Mac’inizi geliştirme için nasıl ayarlayacağınız ele alınmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Service Fabric, OS X üzerinde yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için Vagrant ve VirtualBox kullanarak önceden yapılandırılmış bir Ubuntu sanal makinesi sağlıyoruz. Başlamadan önce şunlar gereklidir:

* [Vagrant (v1.8.4 veya üzeri)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Birbirini destekleyen Vagrant ve VirtualBox sürümleri kullanmanız gerekir. Vagrant desteklenmeyen bir VirtualBox sürümünde kararsız davranabilir.
>

## <a name="create-the-local-vm"></a>Yerel sanal makine oluşturma
5 düğümlü Service Fabric kümesini içeren yerel bir sanal makine oluşturmak için aşağıdaki adımları uygulayın:

1. `Vagrantfile` deposunu kopyalama

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Bu adımlar VM yapılandırması ile birlikte VM’nin indirildiği konumu içeren `Vagrantfile` dosyasını getirir.

2. Deponun yerel kopyasına gidin

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (İsteğe bağlı) Varsayılan sanal makine ayarlarını değiştirin

    Varsayılan olarak, yerel sanal makine aşağıdaki gibi yapılandırılır:

   * 3 GB ayrılmış bellek
   * Mac ana bilgisayarından trafik geçişini sağlayan 192.168.50.50 numaralı IP’de yapılandırılmış özel ana bilgisayar ağı

     Bu ayarların her birini değiştirebilir veya `Vagrantfile` içinde VM’ye başka bir yapılandırma ekleyebilirsiniz. Yapılandırma seçeneklerinin tam listesi için bkz. [Vagrant belgeleri](http://www.vagrantup.com/docs).
4. Sanal makine oluşturma

    ```bash
    vagrant up
    ```

   Bu adım önceden yapılandırılmış sanal makine görüntüsünü indirir, yerel olarak önyükler ve ardından içinde yerel bir Service Fabric kümesi ayarlar. Bu işlem birkaç dakika sürebilir. Kurulum başarıyla tamamlanırsa çıktıda kümenin başlatıldığını belirten bir ileti görürsünüz.

    ![Sanal makine hazırlama sonrasında başlayan küme ayarı][cluster-setup-script]

    >[!TIP]
    > VM’yi indirmek uzun sürüyorsa, wget veya curl kullanarak ya da bir tarayıcı üzerinden `Vagrantfile` dosyasındaki **config.vm.box_url** tarafından belirtilen bağlantıya giderek indirebilirsiniz. Yerel olarak indirdikten sonra, `Vagrantfile` öğesini, görüntüyü indirdiğiniz yerel yolu işaret edecek şekilde düzenleyin. Örneğin, görüntüyü /home/users/test/azureservicefabric.tp8.box dizinine indirdiyseniz, **config.vm.box_url** öğesini bu yola ayarlayın.
    >

5. http://192.168.50.50:19080/Explorer adresinde (varsayılan özel ağ IP’sini tuttuğunuzu varsayarak) Service Fabric Explorer’a giderek kümenin doğru ayarlanıp ayarlanmadığını sınayın.

    ![Ana Mac bilgisayarından görüntülenen Service Fabric Explorer][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Yeoman kullanarak Mac uygulaması oluşturma
Service Fabric, Yeoman şablon oluşturucu kullanarak terminalden Service Fabric uygulaması oluşturmanıza yardımcı olacak yapı iskelesi araçları sağlar. Lütfen makinenizde çalışan bir Service Fabric yeoman şablon oluşturucu olduğundan emin olmak için aşağıdaki adımları izleyin.

1. Mac’inizde Node.js ve NPM yüklü olması gerekir. Yüklü değilse, Node.js ve NPM’yi Homebrew kullanarak aşağıdaki gibi yükleyebilirsiniz. Mac'inizde yüklü Node.js ve NPM sürümlerini denetlemek için ``-v`` seçeneğini kullanabilirsiniz.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. NPM’den makinenize [Yeoman](http://yeoman.io/) şablon oluşturucuyu yükleme

  ```bash
  npm install -g yo
  ```
3. Başlarken [belgelerinde](service-fabric-get-started-linux.md) bulunan adımları izleyerek kullanmak istediğiniz Yeoman oluşturucuyu yükleyin. Yeoman kullanarak Service Fabric uygulamaları oluşturmak için adımları takip edin.

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. Mac üzerinde bir Service Fabric Java uygulaması oluşturmak için makinenizde JDK 1.8 ve Gradle yüklü olmalıdır.


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Eclipse Neon için Service Fabric eklentisini yükleme

Service Fabric, **Java IDE için Eclipse Neon** için Java hizmetlerini oluşturma, derleme ve dağıtmayı kolaylaştırabilen bir eklenti sağlar. Service Fabric Eclipse eklentisini yükleme veya güncelleştirme hakkındaki bu genel [belgede](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) bahsedilen yükleme adımlarını izleyebilirsiniz.

>[!TIP]
> Varsayılan olarak, oluşturulan uygulamada ``Local.json`` dosyasındaki ``Vagrantfile`` değerinde belirtilen varsayılan IP’yi destekliyoruz. Bunu değiştirir ve Vagrant’ı farklı bir IP ile dağıtırsanız lütfen uygulamanızdaki ``Local.json`` dosyasında da IP’yi güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar
<!-- Links -->
* [Linux üzerinde Yeoman kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-create-your-first-linux-application-with-java.md)
* [Linux üzerinde Eclipse için Service Fabric Eklentisi kullanarak ilk Service Fabric Java uygulamanızı oluşturma ve dağıtma](service-fabric-get-started-eclipse.md)
* [Azure portalında bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Azure Resource Manager’ı kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md)
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

