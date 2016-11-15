---
title: "Mac OS X’te geliştirme ortamınızı ayarlama | Microsoft Belgeleri"
description: "Çalışma zamanını, SDK&quot;yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Mac OS X üzerinde uygulama derlemek için hazır hale gelirsiniz."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b25afa13010716188eab0623b1d8ea0d525a2b36


---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Mac OS X’te geliştirme ortamınızı ayarlama
> [!div class="op_single_selector"]
> -[ Windows](service-fabric-get-started.md)
> 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

Mac OS X kullanarak Linux kümelerinde çalışacak Service Fabric uygulamaları derleyebilirsiniz. Bu makalede Mac’inizi geliştirme için nasıl ayarlayacağınız ele alınmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Service Fabric, OS X üzerinde yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için Vagrant ve VirtualBox kullanarak önceden yapılandırılmış bir Ubuntu sanal makinesi sağlıyoruz. Başlamadan önce şunlar gereklidir:

* [Vagrant (v1.8.4 veya üzeri)](http://wwww.vagrantup.com/downloads)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Yerel sanal makine oluşturma
5 düğümlü Service Fabric kümesini içeren yerel bir sanal makine oluşturmak için aşağıdakileri yapın:

1. Vagrantfile deposunu kopyalayın
   
    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
2. Deponun yerel kopyasına gidin
   
    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (İsteğe bağlı) Varsayılan sanal makine ayarlarını değiştirin
   
    Varsayılan olarak, yerel sanal makine aşağıdaki gibi yapılandırılır:
   
   * 3 GB ayrılmış bellek
   * Mac ana bilgisayarından trafik geçişini sağlayan 192.168.50.50 numaralı IP’de yapılandırılmış özel ana bilgisayar ağı
     
     Bu ayarların her birini değiştirebilir veya Vagrantfile içinde sanal makineye başka bir yapılandırma ekleyebilirsiniz. Yapılandırma seçeneklerinin tam listesi için bkz. [Vagrant belgeleri](http://www.vagrantup.com/docs).
4. Sanal makine oluşturma
   
    ```bash
    vagrant up
    ```
   
    Bu adım önceden yapılandırılmış sanal makine görüntüsünü indirir, yerel olarak önyükler ve ardından içinde yerel bir Service Fabric kümesi ayarlar. Bu işlem birkaç dakika sürebilir. Kurulum başarıyla tamamlanırsa çıktıda kümenin başlatıldığını belirten bir ileti görürsünüz.
   
    ![Sanal makine hazırlama sonrasında başlayan küme ayarı][cluster-setup-script]
5. http://192.168.50.50:19080/Explorer adresinde (varsayılan özel ağ IP’sini tuttuğunuzu varsayarak) Service Fabric Explorer’a giderek kümenin doğru ayarlanıp ayarlanmadığını sınayın.
   
    ![Ana Mac bilgisayarından görüntülenen Service Fabric Explorer][sfx-mac]

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Eclipse Neon için Service Fabric eklentisini yükleme (isteğe bağlı)
Service Fabric, Eclipse Neon IDE için Java hizmetlerini derleyip dağıtmayı kolaylaştırabilen bir eklenti sağlar.

1. Eclipse'te, Buildship 1.0.17 veya sonraki bir sürümün yüklü olduğundan emin olun. **Yardım > Yükleme Ayrıntıları**’nı seçerek yüklü bileşenlerin sürümlerini denetleyebilirsiniz. Buildship’i güncelleştirmek için [buradaki][buildship-update] yönergeleri kullanabilirsiniz.
2. Service Fabric eklentisini yüklemek için **Yardım > Yeni Yazılım Yükle...** öğesini seçin
3. "Birlikte çalış" metin kutusuna şunu girin: http://dl.windowsazure.com/eclipse/servicefabric.
4. Ekle’ye tıklayın.
   
    ![Service Fabric için Eclipse Neon eklentisi][sf-eclipse-plugin-install]
5. Service Fabric eklentisini seçin ve İleri’ye tıklayın.
6. Yükleme işlemine devam edin ve son kullanıcı lisans sözleşmesini kabul edin.

## <a name="next-steps"></a>Sonraki adımlar
* [Linux için ilk Service Fabric uygulamanızı oluşturma](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

* [Azure portalında bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Azure Resource Manager’ı kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md)
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship



<!--HONumber=Nov16_HO2-->


