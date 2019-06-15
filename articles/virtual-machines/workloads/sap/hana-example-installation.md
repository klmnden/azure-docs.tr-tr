---
title: SAP HANA (büyük örnekler) azure'da HANA yükleme | Microsoft Docs
description: Nasıl HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4629894933507bda7359fb034c4079d38100029
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62098496"
---
# <a name="install-hana-on-sap-hana-on-azure-large-instances"></a>SAP HANA (büyük örnekler) azure'da HANA yükleyin

SAP HANA (büyük örnekler) azure'da HANA yüklemek için önce aşağıdakileri yapmanız gerekir:
- Microsoft, sizin için bir SAP HANA büyük örneği dağıtmak için tüm verileri sağlayın.
- SAP HANA büyük örneği Microsoft'tan alırsınız.
- Şirket içi ağınıza bağlı bir Azure sanal ağı oluşturun.
- ExpressRoute bağlantı hattı HANA büyük örnekler için aynı Azure sanal ağına bağlayın.
- HANA büyük örnekleri için bir Sıçrama kutusu kullandığınız bir Azure sanal makinesine yükleyin.
- Atlama kutusundan, HANA büyük örneği birimine bağlanabildiğinden emin olun ve bunun tersi de geçerlidir.
- Tüm gerekli paketleri ve düzeltme eklerinin yüklü olup olmadığını kontrol edin.
- Kullanmakta olduğunuz işletim sistemine HANA yüklemesi hakkında belgeler ve SAP notları okuyun. Tercih ettiğiniz HANA yayın işletim sistemi sürüm üzerinde desteklendiğinden emin olun.

Sonraki bölümde, HANA yükleme paketleri atlama kutusunu sanal makineye yükleme bir örnek gösterilmektedir. Bu durumda, işletim sistemi Windows ' dir.

## <a name="download-the-sap-hana-installation-bits"></a>SAP HANA yüklemesi indirme
HANA büyük örneği birimleri doğrudan internet'e bağlı değildir. SAP'den HANA büyük örneği sanal makineye doğrudan yükleme paketlerini indiremez. Bunun yerine Sıçrama kutusu sanal makineye paketlerini indirin.

SAP S-kullanıcı veya SAP Market erişmenize olanak sağlayan diğer kullanıcı ihtiyacınız var.

1. Oturum açın ve gidin [SAP Service Marketplace](https://support.sap.com/en/index.html). Seçin **yazılımı indirin** > **yükleme ve yükseltme** > **alfabetik dizin tarafından**. Ardından **altında H-SAP HANA Platform sürümü** > **SAP HANA Platform sürümü 2.0** > **yükleme**. Aşağıdaki ekran görüntüsünde gösterilen dosyalarını indirin.

   ![Dosyaları indirmek için ekran görüntüsü](./media/hana-installation/image16_download_hana.PNG)

2. Bu örnekte, SAP HANA 2.0 yükleme paketleri indirilen. Azure'da atlama kutusunu sanal makine, aşağıda gösterildiği gibi bu dizine kendiliğinden arşivleri genişletin.

   ![Kendi kendine ayıklanan arşiv ekran görüntüsü](./media/hana-installation/image17_extract_hana.PNG)

3. Arşivleri ayıklanan gibi ayıklama (Bu durumda, 51052030) tarafından oluşturulan dizine kopyalayın. Dizin, HANA büyük örneği birim /hana/shared birimden oluşturduğunuz bir dizine kopyalayın.

   > [!Important]
   > Alanı sınırlıdır ve başka işlemler tarafından kullanılması gerekir çünkü yükleme paketleri kök ya da önyükleme LUN, kopyalamayın.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA HANA büyük örneği biriminde yükleyin
SAP HANA yüklemek için kullanıcı kök olarak oturum açın. Yalnızca kök SAP HANA yüklemek için yeterli izinlere sahiptir. /Hana/shared üzerinden kopyaladığınız dizinle izinleri ayarlayın.

```
chmod –R 744 <Installation bits folder>
```

SAP HANA grafik kullanıcı arabirimi Kurulum kullanılarak yüklemek istiyorsanız, gtk2 paket HANA büyük örnekler üzerinde yüklü olması gerekir. Yüklü olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

```
rpm –qa | grep gtk2
```

(Sonraki adımlarda grafik kullanıcı arabirimi ile SAP HANA Kurulum gösteriyoruz.)

Yükleme dizinine gidin ve HDB_LCM_LINUX_X86_64 alt dizinine gidin. 

Bu dizine dışında başlatın:

```
./hdblcmgui 
```
AT noktada, ekranlar yüklemesi için veri sağlayan bir dizi ilerlemeyi. Bu örnekte, biz SAP HANA veritabanı sunucusu ve SAP HANA istemci bileşenlerini yüklüyorsunuz. Bu nedenle bizim seçimdir **SAP HANA veritabanı**.

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, SAP HANA veritabanı seçilmedi](./media/hana-installation/image18_hana_selection.PNG)

Sonraki ekranda seçin **yeni sisteme yüklemek**.

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, yeni seçilen sistemi yükleyin](./media/hana-installation/image19_select_new.PNG)

Ardından, yüklemek için kullanabileceğiniz birkaç ek bileşenler arasında seçin.

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, ek bileşenlerin listesi](./media/hana-installation/image20_select_components.PNG)

Burada, SAP HANA Client ve SAP HANA Studio seçin. Biz de bir ölçek büyütme örneğini yükleyin. Ardından **tek ana bilgisayar sistemi**. 

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, seçili tek ana bilgisayar sistemi](./media/hana-installation/image21_single_host.PNG)

Ardından, bazı veriler sağlar.

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, Sistem özellikleri alanları tanımlamak için](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> HANA sistemi kimliği (SID) olarak aynı SID, HANA büyük örneği dağıtım sıralı olduğunda Microsoft sağlanan sağlamanız gerekir. Farklı bir SID seçme yüklemenin farklı birimlerde erişim izin sorunları nedeniyle başarısız olmasına neden olur.

Yükleme yolu için /hana/shared dizini kullanın. Sonraki adımda, HANA veri dosyaları ve HANA günlük dosyaları için konumlar sağlar.


![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, verilerin ve günlük alan alanlar](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Sistem Özellikleri (önce iki ekran) tanımlandığında, belirtilen SID, bağlama noktaları SID'si eşleşmelidir. Yoksa uyuşmazlık, geri dönün ve SID bağlama noktalarına sahip değerine ayarlayın.

Sonraki adımda, ana bilgisayar adını gözden geçirin ve sonunda düzeltin. 

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, ana bilgisayar adı](./media/hana-installation/image24_review_host_name.PNG)

Sonraki adımda, ayrıca, HANA büyük örneği dağıtım sıralı Microsoft'a verdiğiniz veri gerekir. 

![Ekran görüntüsü, SAP HANA ile yaşam döngüsü yönetimi, sistem yöneticisine alanları tanımlamak için](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Aynı sağlamak **sistem yönetici kullanıcı Kimliğini** ve **kullanıcı grubu kimliği** birim dağıtım siparişi gibi Microsoft'a sağlanan. Aksi takdirde, HANA büyük örneği birim üzerinde SAP HANA yüklemesi başarısız olur.

Sonraki iki ekrana burada gösterilmez. SAP HANA veritabanı sistem kullanıcının parolasını ve parola sapadm kullanıcıya vermek sağlıyor. İkinci SAP HANA veritabanı örneği bir parçası olarak yüklenen SAP konak aracısı için kullanılır.

Parola tanımladıktan sonra bir onay ekranı görürsünüz. tüm veriler listelenir ve yükleme işlemine devam denetleyin. Yükleme ilerleme durumu, bunun gibi belgelerin bir ilerleme durumu ekranı ulaşana:

![Ekran SAP HANA yaşam döngüsü yönetimi ekranında, Yükleme İlerleme göstergesi](./media/hana-installation/image27_show_progress.PNG)

Yükleme tamamlandıktan, bunun gibi bir ekranı görmeniz gerekir:

![Yüklemeyi gösteren ekran görüntüsü SAP HANA yaşam döngüsü yönetimi ekranında, tamamlandı](./media/hana-installation/image28_install_finished.PNG)

SAP HANA örneği artık kullanım için en fazla ve çalışır ve hazır olmalıdır. SAP HANA Studio'dan bağlanabilir olması gerekir. Ayrıca denetleyin ve en son güncelleştirmeleri uygulamak emin olun.


## <a name="next-steps"></a>Sonraki adımlar

- [SAP HANA büyük örnekleri azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md)

