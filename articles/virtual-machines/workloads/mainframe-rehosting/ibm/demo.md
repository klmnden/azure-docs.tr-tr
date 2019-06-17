---
title: Yedekleme bir uygulama geliştiricilerin denetlenen dağıtım (ADCD) IBM zD & T v1 ayarlama | Microsoft Docs
description: Bir IBM Z geliştirme ve Test Ortamı (zD & T) ortamı, Azure sanal makinelerinde (VM'ler) çalıştırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: c6fcb345b49ce6354a24408ebe163fb928990252
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925685"
---
# <a name="set-up-an-application-developers-controlled-distribution-adcd-in-ibm-zdt-v1"></a>Yedekleme bir uygulama geliştiricilerin denetlenen dağıtım (ADCD) IBM zD & T v1 ayarlayın

Azure sanal makinelerinde (VM'ler) bir IBM Z geliştirme ve Test Ortamı (zD & T) ortamında çalıştırabilirsiniz. Bu ortam, IBM Z serisi mimarisini benzetirken. Bunu çeşitli Z serisi işletim sistemleri veya IBM uygulama geliştiricilerin denetlenen dağıtımları'nı (ADCDs) olarak adlandırılan özel paketler kullanıma sunulan yüklemeleri (Ayrıca Z örnekleri veya paketleri olarak adlandırılır) barındırabilir.

Bu makalede nasıl zD & T ortamı azure'da ADCD örneği ayarlanacağı gösterilmektedir. ADCDs tam Z serisi işletim sistemi uygulamaları geliştirme ve zD & t çalıştırılan test ortamları oluşturun

ZD & T gibi ADCDs yalnızca IBM müşteriler ve iş ortakları için kullanılabilir ve yalnızca geliştirme ve test amaçlıdır. Bunlar, üretim ortamları için kullanılamaz. Çok sayıda IBM yükleme paketleri indirme kullanılabilen [Passport avantajı](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.guide.adcd.doc/topics/installation_ps.html) veya [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [ZD & T ortam] [ ibm-install-z] Azure'da daha önce ayarlamış. Bu makalede, daha önce oluşturduğunuz aynı Ubuntu 16.04 VM görüntüsü kullandığınızı varsayar.

- IBM PartnerWorld veya Passport avantajı aracılığıyla ADCD medya erişim.

- A [lisans sunucusu](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Bu IBM zD & t çalıştırmak için gereklidir Oluşturduğunuz şekilde nasıl IBM yazılımı lisansı bağlıdır:

  - **Lisans sunucusu donanım tabanlı** yazılım tüm bölümlerini erişmek gerekli rasyonel belirteçler içeren bir USB donanım gerektirir. Bu IBM edinmeniz gerekir.

  - **Yazılım tabanlı lisans sunucusu** lisanslama anahtarları yönetimi için merkezi bir sunucu ayarlamanız gerekir. Bu yöntem, tercih edilir ve yönetim sunucusuna IBM aldığınız anahtarları ayarlamanız gerekir.

## <a name="download-the-installation-packages-from-passport-advantage"></a>Passport Avantajı ' yükleme paketleri indirin

ADCD medyaya erişimi gereklidir. Aşağıdaki adımlarda, IBM müşteriler ve Passport avantajı kullanabilirsiniz varsayılır. IBM iş ortakları kullanma [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

> [!NOTE]
> Bu makalede, Azure portalına erişmek için ve IBM medya indirmek için bir Windows PC kullanıldığı varsayılmaktadır. Bir Mac veya Ubuntu Masaüstü kullanıyorsanız komutları ve IBM medya alma işlemi biraz farklı olabilir.

1. Oturum [Passport avantajı](https://www.ibm.com/software/howtobuy/passportadvantage/paocustomer).

2. Seçin **yazılım indirme işlemleri** ve **medya erişim**.

3. Seçin **programı teklifi ve anlaşma numarası**, tıklatıp **devam**.

4. Parça numarası ve bölüm açıklama girin ve tıklayın **Bulucu**.

5. İsteğe bağlı olarak alfabetik düzende ürün adına göre görüntüleme listeye tıklayın.

6. Seçin **tüm işletim sistemleri** içinde **işletim sistemi alanı**, ve **tüm diller** içinde **dilleri alan**. ' A tıklayarak **Git**.

7. Tıklayın **tek tek dosyaları seçin** için listeyi genişletin ve indirmek için bireysel ortam görüntülemek için.

8. İndirme, seçmek istediğiniz paket doğrulayın **indirme**ve ardından istediğiniz dizine dosyalarını indirin.

## <a name="upload-the-adcd-packages"></a>ADCD paket karşıya yükleme

Paket olduğuna göre sanal makinenizde Azure için yüklemeniz gerekir.

1. Azure portalında başlatmak bir **ssh** oluşturduğunuz Ubuntu VM ile oturum. VM'nizi seçin Git **genel bakış** dikey penceresine tıklayın ve ardından **Connect**.

2. Seçin **SSH** sekmesine ve ardından kopyalama ssh panoya komutu.

3. VM'nizi kimlik bilgilerinizi kullanarak oturum açın ve [SSH istemcisi](/azure/virtual-machines/linux/use-remote-desktop) tercih ettiğiniz. Bu Tanıtım için Windows komut istemi bir bash kabuğunda ekler Windows 10 için Linux Uzantıları'nı kullanır. PuTTY da çalışır.

4. Oturum açtığında, IBM paketleri yüklemek için bir dizin oluşturun. Linux etkilenebileceğini büyük/küçük harfe duyarlıdır. Örneğin, bu tanıtımda paketler yüklenir varsayılır:

        /home/MyUserID/ZDT/adcd/nov2017/volumes

5. Gibi bir SSH istemcisi kullanarak dosyaları karşıya yükleme[WinSCP](https://winscp.net/eng/index.php). SCP SSH bir parçası olduğundan, ne SSH kullanan olan bağlantı noktası 22'yi kullanır. Yerel bilgisayarınızda Windows değilse yazabilirsiniz [scp komutunun](http://man7.org/linux/man-pages/man1/scp.1.html) SSH oturumunuzda.

6. Resim depolama zD & t olur, oluşturduğunuz Azure VM dizine yükleme başlatmak

    > [!NOTE]
    > Emin olun **ADCDTOOLS. XML** yükleme dahil **giriş/MyUserID/ZDT/adcd/nov2017** dizin. Buna daha sonra ihtiyacınız olacak.

7. Azure'a bağlantınızı bağlı olarak biraz zaman alabilir, dosyaları karşıya yüklemek bekleyin.

8. Karşıya yükleme tamamlandığında, birimleri dizine gidin ve tüm sıkıştırmasını **gz** birimleri:

    ```
        gunzip \*.gz
    ```
    
![Dosya Gezgini gösteren gz birimleri açıldı](media/01-gunzip.png)

## <a name="configure-the-image-storage"></a>Resim depolama yapılandırma

Sonraki adım, zD & T, karşıya yüklenen paket kullanılacak yapılandırmaktır. ZD & T içinde görüntü depolama işlemi, bağlama ve görüntüleri kullanmanıza olanak sağlar. SSH veya FTP kullanabilirsiniz.

1. Başlangıç **zDTServer**. Bunu yapmak için kök düzeyinde olması gerekir. Aşağıdaki iki komutları sırayla girin:
    ```
        sudo su -
        /opt/ibm/zDT/bin/startServer
    ```
2. URL çıkış komutu tarafından not edin ve web sunucusuna erişmek için bu URL'yi kullanın. Benzer şekilde görünür:
     > https://(Your VM name or IP Address):9443/ZDTMC/index.HTML
     >
     > Unutmayın, bağlantı noktası 9443, web access kullanır. Web sunucusuna oturum açmak için bunu kullanın. Kullanıcı Kimliği ZD & T **zdtadmin** ve parola **parola**.

    ![IBM zD & T Enterprise Edition Hoş Geldiniz ekranı](media/02-welcome.png)

3. Üzerinde **Hızlı Başlangıç** sayfasındaki **yapılandırma**seçin **resim depolama**.

     ![IBM zD & T Enterprise Edition hızlı başlangıç ekranı](media/03-quickstart.png)

4. Üzerinde **resim depolama yapılandırma** sayfasında **SSH Dosya Aktarım Protokolü**.

5. İçin **ana bilgisayar adı**, türü **Localhost** ve görüntüleri yüklediğiniz için dizin yolunu girin. Örneğin, /home/MyUserID/ZDT/adcd/nov2017/volumes.

6. Girin **kullanıcı kimliği** ve **parola** VM için. ZD & T kullanıcı kimliği ve parola kullanmayın.

7. Erişimi ve ardından emin olmak için bağlantıyı sınayın **Kaydet** yapılandırmayı kaydetmek için.

## <a name="configure-the-target-environments"></a>Hedef ortamları yapılandırmak

Sonraki adım, zD & T hedef ortam yapılandırmaktır. Bu benzetilmiş barındırılan görüntülerinizi çalıştırdığı ortamıdır.

1. Üzerinde **Hızlı Başlangıç** sayfasındaki **yapılandırma**seçin **hedef ortamlarında**.

2. Üzerinde **hedef ortamları yapılandırmak** sayfasında **ekleme hedef**.

3. Seçin **Linux**. İki tür ortamlarda, Linux ve Cloud(OpenStack), IBM destekler, ancak bu tanıtımda, Linux üzerinde çalışır.

4. Üzerinde **Ekle hedef ortam** sayfası için **ana bilgisayar adı**, girin **localhost**. Tutun **SSH bağlantı noktası** kümesine **22**.

5. İçinde **hedef ortam etiketi** kutusunda, aşağıdaki gibi bir etiket girin **MyCICS.**

     ![Hedef ortam ekranı ekleme](media/04-add-target.png)

## <a name="configure-adcd-and-deploy"></a>ADCD yapılandırın ve dağıtın

Önceki yapılandırma adımlarını tamamladıktan sonra zD & T, paketleri ve hedef ortamı kullanmak için yapılandırmanız gerekir. Yeniden görüntü depolama işlemi zD & T, bağlama ve görüntüleri kullanmanıza olanak sağlayan kullanın. SSH veya FTP kullanabilirsiniz.

1. Üzerinde **Hızlı Başlangıç** sayfasındaki **yapılandırma**seçin **ADCD**. Yönerge ADCD paket bağlanabilir önce tamamlanması gereken adımları belirten görünür. Bu, size daha önce yaptığımız yolu hedef dizine neden adlı açıklanmaktadır.

2. Tıklatın doğru dizinleri için tüm görüntüleri karşıya yüklendi varsayılarak **ADCD GÖRÜNTÜDEN** (7. adım aşağıdaki ekran görüntüsünde gösterilen) sağ alt köşesinde görüntülenen bağlantı.

     ![IBM zD & T Enterprise Edition - yapılandırma ADCD ekranı](media/05-adcd.png)

## <a name="create-the-image"></a>Görüntü oluşturma

Önceki yapılandırma adımında tamamlandığında, **ADCD bileşenlerini kullanarak görüntü oluşturma** sayfası görüntülenir.

1. Bu birimi farklı paketleri görüntülemek için (Bu durumda Kas 2017) birim seçin.

2. Bu Tanıtım için seçin **müşteri bilgileri denetim sistemi (CICS) - 5.3**.

3. İçinde **görüntü adı** gibi görüntü için bir ad yazın **MyCICS görüntü**.

4. Seçin **görüntü oluşturma** sağ alt düğmesi.

     ![IBM zD & T Enterprise Edition - ADCD bileşenleri ekranı'nı kullanarak görüntü oluşturma](media/06-adcd.png)

5. Görüntülenen penceresinde görüntü belirten başarıyla dağıtıldı öğesini **görüntülerini**.

6. Üzerinde **görüntü bir hedef ortama dağıtmak** sayfasında, önceki sayfada oluşturduğunuz resim seçin (**MyCICS görüntü**) ve daha önce oluşturduğunuz hedef ortamı (**MyCICS**).

7. Sonraki ekranda, VM (yani değil ztadmin kimlik) için kimlik bilgilerinizi sağlayın.

8. Özellikler bölmesinde sayısını girin **merkezi işlemci (CPs)** , miktarını **sistem bellek (GB)** ve **dağıtım dizinine** çalışan görüntüsü. Bu Tanıtım olduğundan, küçük tutun.

9. Emin olun kutusunun seçili için **sorunu IPL komut z/OS için otomatik olarak Dağıt**.

     ![Özellikleri ekran](media/07-properties.png)

10. Seçin **tam**.

11. Seçin **dağıtma görüntü** gelen **görüntü bir hedef ortama dağıtmak** sayfası.

Görüntünüz artık dağıtabilirsiniz ve 3270 terminal öykünücü tarafından bağlanmasını hazırdır.

> [!NOTE]
> Yeterli disk alanı yok belirten bir hata alırsanız, bölge 151 Gb gerektirdiğini unutmayın.

Tebrikler! Şimdi, Azure üzerinde bir IBM ana bilgisayar ortamı çalışıyor.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Ana bilgisayar geçişi: konusundaki söylentiler ve bilgiler](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [Azure üzerinde IBM DB2 pureScale](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
- [Sorun giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Azure'a geçiş için ana bilgisayar demystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
