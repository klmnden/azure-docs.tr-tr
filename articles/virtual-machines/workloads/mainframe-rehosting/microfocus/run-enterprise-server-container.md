---
title: Micro odak Enterprise Server 4.0 bir Docker kapsayıcısında Azure sanal makineler üzerinde çalıştırın.
description: IBM z/OS ana iş yüklerinizi Micro odak kuruluş sunucusu bir Docker kapsayıcısında Azure sanal Makineler'de çalıştırarak yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: sread
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 30d99c3f4767eb50361f7074c0d508fcf309faca
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896603"
---
# <a name="run-micro-focus-enterprise-server-40-in-a-docker-container-on-azure"></a>Micro odak Enterprise Server 4.0, Azure'da bir Docker kapsayıcısında çalıştırma

Azure'da bir Docker kapsayıcısında Micro odak Enterprise Server 4.0 çalıştırabilirsiniz. Bu öğretici, nasıl gösterir. Kuruluş Sunucusu (müşteri bilgilerini denetleme sistemi) Windows CICS acctdemo sunum kullanır.

Docker uygulamaları için taşınabilirlik ve yalıtım ekler. Örneğin, bir Docker görüntüsü başka bir, çalıştırmak için bir Windows VM veya Docker ile bir Windows sunucusuna bir depodan dışarı aktarabilirsiniz. Aynı yapılandırmaya sahip yeni konumdaki Docker görüntüsünü çalıştırır: kuruluş sunucusu yüklemek zorunda kalmadan. Görüntü gereksinimlerimizim bir parçasıdır. Lisans değerlendirmeleri hala geçerlidir.

Bu öğreticide yükler **Windows 2016 Datacenter with Containers** Azure Market'ten sanal makine (VM). Bu VM'yi içeren **Docker 18.09.0**. Aşağıdaki adımlarda kapsayıcıyı dağıtmak, çalıştırmak ve 3270 bir öykünücü ile bağlanabilirsiniz gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şu önkoşulların denetleyin:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Micro odak yazılım ve bir geçerli lisans (veya deneme lisansı). Mevcut bir Micro odak müşterisi iseniz Micro odak temsilcinize başvurun. Aksi takdirde, [bir deneme sürümü isteyin](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

     > [!NOTE]
     > Docker Demo dosyaları ile Kurumsal Server 4.0 dahil edilir. Bu öğreticide ent\_sunucu\_dockerfile'ları\_4.0\_windows.zip. Kuruluş sunucusu yükleme dosyası erişilen aynı yerden erişmek veya Git *Micro odak* kullanmaya başlamak için.

- Belgelerine [Enterprise Server ve kurumsal Geliştirici](https://www.microfocus.com/documentation/enterprise-developer/#").

## <a name="create-a-vm"></a>VM oluşturma

1. Üst medyanın korunmasına\_sunucu\_dockerfile'ları\_4.0\_windows.zip dosya. ES-Docker-Prod-XXXXXXXX.mflic lisans dosyası (Docker görüntülerinizi oluşturmak için gereklidir) güvenli hale getirin.

2. Bir VM oluşturun. Bunu yapmak için Azure portal, select açın **kaynak Oluştur** sol üst ve filtre tarafından *windows server*. Sonuçlarda seçin **Windows Server 2016 Datacenter – kapsayıcılarla**.

     ![Azure portalı arama sonuçları](media/01-portal.png)

3. Sanal Makinenin özelliklerini yapılandırmak için örnek ayrıntıları seçin:

    1. Bir VM boyutu seçin. Bu öğretici için kullanmayı bir **standart DS2\_v2** 2 Vcpu ve 7 GB bellek ile VM.

    2. Seçin **bölge** ve **kaynak grubu** dağıtmak istiyor.

    3. İçin **kullanılabilirlik seçeneklerini**, varsayılan ayarı kullanın.

    4. İçin **kullanıcıadı**, kullanmak istediğiniz yönetici hesabı ve parola yazın.

    5. Emin **3389 RDP bağlantı noktası** açıktır. VM'de oturum açmak için genel olarak açığa için yalnızca bu bağlantı noktası gerekir. Ardından, tüm varsayılan değerleri kabul edin ve tıklayın **gözden geçir + Oluştur**.

     ![Bir sanal makine bölmesinde oluşturma](media/container-02.png)

4. (Birkaç dakika) dağıtımın tamamlanmasını bekleyin. Bir ileti, sanal makinenizin oluşturulduğunu belirtir.

5. Tıklayın **kaynağa Git** gitmek için **genel bakış** dikey penceresinde VM'niz için.

6. Sağ tarafta tıklayın **Connect** düğmesi. **Sanal makineye bağlanma** seçenekler sağ tarafta görüntülenir.

7. Tıklayın **RDP dosyasını indir** VM'ye olanak tanıyan RDP dosyasını İndir düğmesini.

8. Dosya indirme tamamlandıktan sonra kullanıcı adı ve parola sanal makine için oluşturduğunuz ve tür açın.

     > [!NOTE]
     > Oturum açmak için Kurumsal kimlik bilgilerinizi kullanmayın. (Bu kullanmak isteyebilirsiniz RDP istemcisinin varsayar. Bunu yapmadığınız.)

9.  Seçin **diğer seçenekler**, VM kimlik bilgilerinizi'ı seçin.

Bu noktada, sanal makine çalışır ve RDP aracılığıyla bağlı. Ve kullanıma hazır bir sonraki adım için oturumunuz.

## <a name="create-a-sandbox-directory-and-upload-the-zip-file"></a>Bir sanal dizin oluşturun ve ZIP dosyasını karşıya yükleme

1.  Burada demo ve lisans dosyaları karşıya yükleyebilir VM'de bir dizin oluşturun. Örneğin, **C:\\korumalı alan**.

2.  Karşıya yükleme **ent\_sunucu\_dockerfile'ları\_4.0\_windows.zip** ve **ES-Docker-Prod-XXXXXXXX.mflic** dosyasını, oluşturduğunuz bir dizine.

3.  İçin ZIP dosyasının içeriğini ayıklayın **ent\_sunucu\_dockerfile'ları\_4.0\_windows** ayıklama işlemi tarafından oluşturulan dizin. Bu dizin, bir benioku dosyası (olarak, .html ve .txt dosyası) ve iki alt dizinleri içerir **EnterpriseServer** ve **örnekler**.

4.  Kopyalama **ES-Docker-Prod-XXXXXXXX.mflic** c:\\korumalı alan\\ent\_sunucu\_dockerfile'ları\_4.0\_windows\\ EnterpriseServer ve C:\\korumalı alan\\ent\_sunucu\_dockerfile'ları\_4.0\_windows\\örnekler\\CICS dizinleri.

> [!NOTE]
> Lisans dosyası her iki dizini için de kopyaladığınızdan emin olun. Görüntüleri düzgün şekilde lisanslandığından emin olmak Docker derleme adımı için gereklidirler.

## <a name="check-docker-version-and-create-base-image"></a>Docker sürümünü denetleyin ve temel görüntü oluşturma

> [!IMPORTANT]
> Uygun bir Docker görüntüsü oluşturma iki adımlı bir işlemdir. İlk olarak kuruluş Server 4.0 temel görüntü oluşturun. Ardından x64 için başka bir görüntü oluşturma platformu. (32-bit) bir x86 oluşturabilirseniz görüntü, 64-bit görüntüyü kullanın.

1. Bir komut istemi açın.

2. Docker'ın yüklü olduğunu denetleyin. Komut istemine şunları yazın:

    ```
        docker version
    ```

     Örneğin, bu yazılırken sürümü 18.09.0 oluştu.

3. Dizini değiştirmek için şunu yazın **cd \Sandbox\ent_server_dockerfiles_4.0_windows\EnterpriseServer**.

4. Tür **bld.bat IacceptEULA** ilk temel görüntüsü için derleme işlemini başlatmak için. Çalıştırmak bu işlem birkaç dakika bekleyin. Sonuçlarda iki görüntüleri bildirim oluşturuldu: biri x64, diğeri x86 için:

     ![Görüntüleri gösteren komut penceresi](media/container-04.png)

5. CICS Tanıtım amaçlı son görüntü oluşturmak için CICS dizine geçin yazarak **cd\\korumalı alan\\ent\_sunucu\_dockerfile'ları\_4.0\_windows\\ Örnekler\\CICS**.

6. Görüntüyü oluşturmak için şunu yazın **bld.bat x64**. Çalıştırılacak işlem birkaç dakika bekleyin ve olduğunu belirten bir ileti görüntü oluşturuldu.

7. Tür **docker görüntüleri** VM'de yüklü Docker görüntüleri bir listesini görüntüleyin. Emin **microfocus/es-acctdemo** , bunlardan biridir.

     ![Komut penceresi ES acctdemo görüntü gösterme](media/container-05.png)

## <a name="run-the-image"></a>Görüntü çalıştırma 

1.  Başlatma Enterprise Server 4.0 ve acctdemo uygulamayı, komut isteminde şunu yazın:

    ```
         docker run -p 16002:86/tcp -p 16002:86/udp -p 9040-9050:9040-9050 -p 9000-9010:9000-9010 -ti --network="nat" --rm microfocus/es-acctdemo:win_4.0_x64
    ```

2.  3270 terminal öykünücüsünü gibi yükleyen [x3270](http://x3270.bgp.nu/) ve çalışmakta olan görüntünün 9040, bağlantı noktası üzerinden eklemek için kullanın.

3.  Docker kapsayıcıları yönettiği için DHCP sunucusu olarak davranıp şekilde acctdemo kapsayıcının IP adresini alın:

    1.  Çalışmakta olan kapsayıcıyı Kimliğini alın. Tür **Docker ps** komut istemi ve kimliği Not (**22a0fe3159d0** Bu örnekte). Sonraki adım için kaydedin.

    2.  Acctdemo kapsayıcı için IP adresini almak için önceki adımdan gelen kapsayıcı kimliği şu şekilde kullanın:

    ```
       docker inspect <containerID> --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```

       Örneğin:

    ```   
        docker inspect 22a0fe3159d0 --format="{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}"
    ```
4. Acctdemo görüntüsü için IP adresini not edin. Örneğin, aşağıdaki çıktıda 172.19.202.52 adresidir.

     ![IP adresi gösteren komut penceresi](media/container-06.png)

5. Öykünücüyü görüntüsünü bağlayın. Öykünücü acctdemo görüntü ve bağlantı noktasını 9040 adresini kullanmak için yapılandırın. Burada, sahip **172.19.202.52:9040**. Size ait benzer olacaktır. **CICS için oturum açma** ekranı açılır.

    ![Oturum açma ekranına CICS](media/container-07.png)

6. CICS bölgesiyle girerek oturum açın **SYSAD** için **UserID** ve **SYSAD** için **parola**.

7. Öykünücü'nın keymap kullanarak ekrandaki temizleyin. X3270 için seçin **Keymap** menü seçeneği.

8. Acctdemo uygulamayı başlatmak için yazın **ACCT**. Uygulamanın ilk ekranı görüntülenir.

     ![Hesap tanıtım ekran](media/container-08.png)

9. Görüntü hesap türleri ile denemeler yapın. Örneğin **D** isteği için ve **11111** için **hesabı**. Denemek için diğer hesap 22222 33333 ve benzeri sayılardır.

     ![Hesap tanıtım ekran](media/container-09.png)

10. Enterprise Server Yönetim Konsolu'nu görüntülemek için komut istemi türü için gidin ve **http:172.19.202.52:86 Başlat**

     ![Enterprise Server Yönetim Konsolu](media/container-010.png)

İşte bu kadar! Artık çalışan ve bir Docker kapsayıcısında bir CICS uygulama yönetme.

## <a name="next-steps"></a>Sonraki adımlar

- [Micro odak Enterprise Server 4.0 ve kurumsal Geliştirici 4.0 Azure'a yükleme](./set-up-micro-focus-azure.md)
- [Ana bilgisayar uygulaması geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
