---
title: Micro odak Enterprise Server 4.0 ve kurumsal Geliştirici 4.0 Azure'a yükleme | Microsoft Docs
description: Micro odak geliştirme kullanarak IBM z/OS ana iş yüklerinizi yeniden barındırma ve Azure sanal makineleri (VM'ler) ortamında test edin.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 45d6f8606c665d78783f987c2f2b49a77801639c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304597"
---
# <a name="install-micro-focus-enterprise-server-40-and-enterprise-developer-40-on-azure"></a>Micro odak Enterprise Server 4.0 ve kurumsal Geliştirici 4.0 Azure'a yükleme

Bu makalede nasıl ayarlandığı gösterilmektedir [Micro odak Enterprise Server 4.0](https://www.microfocus.com/documentation/enterprise-developer/es30/) ve [Micro odak Kurumsal Geliştirici 4.0](https://www.microfocus.com/documentation/enterprise-developer/ed_30/) azure'da.

Azure'da ortak bir iş yükü geliştirme ve test ortamıdır. Bu nedenle ekonomik ve kolay dağıtmak ve kapatabilirsiniz olduğundan bu yaygın bir senaryodur. Kurumsal sunucusuyla Micro odak en büyük anabilgisayar rehosting platformlarından biri kullanılabilir oluşturdu. Z/OS iş yükleri üzerinde daha ucuz bir x86 çalıştırabileceğiniz Windows veya Linux sanal makineleri (VM'ler) kullanarak azure'da bir platform.

Bu kurulum, Microsoft SQL Server 2017 yüklü olan Azure Market'teki Windows Server 2016 görüntüsü çalıştıran Azure Vm'leri kullanır. Bu kurulum, Azure Stack için de geçerlidir.

Visual Studio Community (ücretsiz) ya da Microsoft Visual Studio 2017 veya sonraki sürümleri çalıştıran kurumsal Geliştirici kuruluş sunucusu için karşılık gelen geliştirme ortamı olan veya Eclipse. Bu makalede, Visual Studio 2017'yle birlikte gelen veya üzeri yüklü olan bir Windows Server 2016 sanal makine kullanarak dağıtma gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şu önkoşulların denetleyin:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Micro odak yazılım ve bir geçerli lisans (veya deneme lisansı). Mevcut bir Micro odak müşterisi iseniz Micro odak temsilcinize başvurun. Aksi takdirde, [bir deneme sürümü isteyin](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

- Belgeleri alma [Enterprise Server ve kurumsal Geliştirici](https://www.microfocus.com/documentation/enterprise-developer/#").

> [!NOTE]
> Azure Vm'lerine erişim denetimi bir siteden siteye sanal özel ağ (VPN) tüneli veya bir Sıçrama kutusu ayarlamanız iyi bir uygulamadır.

## <a name="install-enterprise-server"></a>Enterprise Server’ı yükleme

1. Daha iyi güvenlik ve yönetilebilirlik için yalnızca bu proje için yeni bir kaynak grubu oluşturmayı göz önünde bulundurun — Örneğin, **RGMicroFocusEntServer**. Adın ilk kısmı Azure'da listesindeki nokta kolaylaştırmak için kaynak türünü seçmek için kullanın.

2. Sanal makine oluşturur. Azure Market'ten sanal makine ve istediğiniz işletim sistemi seçin. Önerilen bir Kurulum şu şekildedir:

    - **Kuruluş Sunucusu**: (2 Vcpu ve 16 GB bellek ile) Windows Server 2016 ve SQL Server 2017 yüklü ES2 v3 VM'yi seçin. Bu görüntü Azure Market'te kullanılabilir. Kuruluş sunucusu, Azure SQL veritabanı de kullanabilirsiniz.

    - **Kurumsal Geliştirici**: B2ms VM, Windows 10 ve yüklü Visual Studio ile (2 Vcpu ve 8 GB bellek) seçin. Bu görüntü Azure Market'te kullanılabilir.

3. İçinde **Temelleri** bölümünde, kullanıcı adı ve parola girin. Seçin **abonelik** ve **konum/bölge** VM'ler için kullanmak istiyorsunuz. Seçin **RGMicroFocusEntServer** kaynak grubu için.

4. Birbiriyle iletişim kurabilmesi iki sanal makine aynı sanal ağa ekleyin.

5. Ayarların geri kalanı için varsayılan değerleri kabul edin. Kullanıcı adı ve parola Yöneticisi bu VM'lerin Oluştur unutmayın.

6. Sanal makine oluşturulduğunda, gelen bağlantı noktalarını 9003, açma 86 ve HTTP için 80 ve kurumsal Server makinesinde RDP için 3389 ve geliştirici makinesinde 3389.

7. Kurumsal Server sanal makinesi, Azure portalında oturum açmak için ES2 v3 VM'yi seçin. Git **genel bakış** seçin ve bölüm **Connect** bir RDP oturumu başlatmak için. Sanal makine için oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.

8. Aşağıdaki iki dosyada RDP oturumundan yükleyin. Windows kullandığınız için sürükleyin ve RDP oturumu dosyalarına bırakın:

    - **ES\_40. exe**, kuruluş sunucusu yükleme dosyası.

    - **mflic**, karşılık gelen lisans dosyası — kurumsal sunucu olmadan yükü olmaz.

9. Yüklemeyi başlatmak için dosyaya çift tıklayın. İlk penceresinde yükleme konumunu seçin ve son kullanıcı lisans sözleşmesini kabul edin.

     ![Micro odak Kurumsal Sunucusu Kurulum Ekranı](media/01-enterprise-server.png)

     Kurulum tamamlandığında, aşağıdaki ileti görünür:

     ![Micro odak Kurumsal Sunucusu Kurulum Ekranı](media/02-enterprise-server.png)

### <a name="check-for-updates"></a>Güncelleştirmeleri denetle

Yükleme sonrasında gibi Kurumsal sunucusu ile birlikte Microsoft C++ yeniden dağıtılabilir ve .NET Framework yüklü önkoşulları itibaren hiçbir ek güncelleştirmeleri denetlemek emin olun.

### <a name="upload-the-license"></a>Lisans Yükleme

1. Micro odak lisans Yönetim'i başlatın.

2. Tıklayın **Başlat** \> **Micro odak Lisans Yöneticisi'ni** \> **lisans Yönetim**ve ardından **yükleyin** sekmesi. Karşıya yüklemek için lisans biçim türünü seçin: Lisans dosyası veya bir 16 karakterlik lisans kodu. Örneğin, bir dosya içinde **lisans dosyası**, göz atın **mflic** dosya karşıya daha önce seçin ve VM **Lisansları Yükle**.

     ![Micro odak lisans yönetimi iletişim kutusu](media/03-enterprise-server.png)

3. Kuruluş Sunucusu yüklendiğini doğrulayın. Bu URL'yi kullanarak bir tarayıcı Enterprise Server Yönetim sitesinden başlatmayı deneyin <http://localhost:86/> . Kurumsal sunucu yönetimi sayfasına gösterildiği gibi görüntülenir.

     ![Kurumsal Sunucu Yönetimi sayfası](media/04-enterprise-admin.png)

## <a name="install-enterprise-developer-on-the-developer-machine"></a>Kurumsal Geliştirici Geliştirici makineye yükleyin.

1. Daha önce oluşturduğunuz kaynak grubunu seçin (örneğin, **RGMicroFocusEntServer**), ardından developer görüntüsünü seçin.

2. Sanal makineye oturum açmak için Git **genel bakış** seçin ve bölüm **Connect**. Bu oturum açma, bir RDP oturumu başlatır. Sanal makine için oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.

3. RDP oturumundan (Sürükle, bırak, isterseniz) aşağıdaki iki dosya yükle:

    - **edvs2017.exe**, kuruluş sunucusu yükleme dosyası.

    - **mflic**, karşılık gelen lisans dosyası (Kurumsal Geliştirici yüklemeyecek olmadan).

4. Çift **edvs2017.exe** yüklemeyi başlatmak için dosya. İlk penceresinde yükleme konumunu seçin ve son kullanıcı lisans sözleşmesini kabul edin. İsterseniz, seçin **yükleme Rumba 9.5** büyük olasılıkla gerekir bu terminal öykünücü yüklemek için.

     ![Micro odak Kurumsal geliştirici Visual Studio 2017 Kurulum iletişim kutusu](media/04-enterprise-server.png)

5. Kurulum tamamlandıktan sonra aşağıdaki ileti görünür:

     ![Kurulum başarılı iletisi](media/05-enterprise-server.png)

6. Kuruluş Sunucusu için yaptığınız gibi Micro odak Lisans Yöneticisi'ni başlatın. Seçin **Başlat** \> **Micro odak Lisans Yöneticisi'ni** \> **lisans Yönetim**, tıklatıp **Yükleme**sekmesi.

7. Karşıya yüklemek için lisans biçim türünü seçin: Lisans dosyası veya bir 16 karakterlik lisans kodu. Örneğin, bir dosya içinde **lisans dosyası**, göz atın **mflic** dosya karşıya daha önce seçin ve VM **Lisansları Yükle**.

     ![Micro odak lisans yönetimi iletişim kutusu](media/07-enterprise-server.png)

Kurumsal Geliştirici yüklediğinde, Micro odak geliştirme ve test ortamı azure'da dağıtımını tamamlandı!

## <a name="next-steps"></a>Sonraki adımlar

- [Banka Demo uygulamasını ayarlama](./demo.md)
- [Kuruluş Sunucusu Docker kapsayıcılarında çalıştırın](./run-enterprise-server-container.md)
- [Ana bilgisayar uygulaması geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
