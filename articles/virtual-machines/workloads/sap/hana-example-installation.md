---
title: SAP HANA (büyük örnekler) azure'da HANA yükleme | Microsoft Docs
description: Nasıl HANA (büyük örnek) Azure üzerinde SAP HANA yükleyin.
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
ms.openlocfilehash: 76a7ce99799b85d81aa6e127ebe1e57e2df3e59a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164770"
---
# <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>HANA büyük örnekler üzerinde SAP HANA yükleme örneği

Bu bölümde, bir HANA büyük örneği birim üzerinde SAP HANA yükleneceği gösterilmektedir. Sahip olduğumuz başlangıç durumu gibi görünür:

- Microsoft tüm verileri bir SAP HANA büyük örneği dağıtmak için sağladığınız.
- SAP HANA büyük örneği Microsoft'tan aldığınız.
- Şirket içi ağınıza bağlı olan Azure sanal ağı oluşturduğunuz.
- Aynı Azure sanal ağı için HANA büyük örnekler için ExpressRotue devre bağlı.
- Azure HANA büyük örnekleri için bir Sıçrama kutusu kullandığınız sanal yüklediğiniz.
- HANA büyük örneği birim ve tersi Sıçrama kutusundan bağlanabildiğinden emin yaptığınız.
- Tüm gerekli paketleri ve düzeltme eklerinin yüklü olup olmadığını kontrol.
- SAP notları ve belgeler HANA yüklemesi kullanıyorsanız ve HANA yayın tercih ettiğiniz işletim sisteminde desteklendiğinden emin yapılan işletim sistemi ile ilgili makaleyi yayın.

Sonraki sıralarında gösterilene atlama kutusuna bu durumda bir Windows işletim sisteminde, HANA büyük örneği birim paketler kopyasını ve Kurulum dizisini çalıştıran VM, HANA yükleme paketlerinin indirilir.

## <a name="download-of-the-sap-hana-installation-bits"></a>SAP HANA yükleme indirme
HANA büyük örneği birimleri doğrudan internet bağlantısı yoksa HANA büyük örneği VM SAP'den yükleme paketleri doğrudan indiremez. Eksik doğrudan internet bağlantısı üstesinden gelmek için atlama kutusunu gerekir. Atlama kutusuna VM paketleri karşıdan yükleyin.

HANA yükleme paketleri indirmek için bir SAP S-kullanıcı veya SAP Market erişmenize olanak sağlayan diğer kullanıcı gerekir. Oturum açtıktan sonra bu ekranları sırasıyla gidin:

Git [SAP Service Marketplace](https://support.sap.com/en/index.html) > karşıdan yükleme yazılımı tıklayın > yükleme ve yükseltme > alfabetik dizin tarafından > – H altında SAP HANA Platform sürümü > SAP HANA Platform sürümü 2.0 > yükleme > indirin Aşağıdaki dosyaları

![HANA yüklemesi indirin](./media/hana-installation/image16_download_hana.PNG)

SAP HANA 2.0 yükleme paketleri indirilen tanıtım durumda. Azure atlama çubuğundaki VM, kendi kendine ayıklanan arşivleri dizine aşağıda gösterildiği gibi genişletin.

![HANA yüklemesi ayıklayın](./media/hana-installation/image17_extract_hana.PNG)

Arşivleri ayıklanan gibi çıkarma durumda 51052030, yukarıda oluşturduğunuz bir dizine /hana/shared birim içine HANA büyük örneği birimine tarafından oluşturulan dizine kopyalayın.

> [!Important]
> Alanı sınırlıdır ve başka işlemler tarafından kullanılması gereken yükleme paketleri kök ya da önyükleme LUN kopyalamayın.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA HANA büyük örneği biriminde yükleyin
SAP HANA yüklemek için kullanıcı kök olarak oturum açmanız gerekir. Yalnızca kök SAP HANA yüklemek için yeterli izinlere sahiptir.
Üzerinden/hana paylaşılan / kopyaladığınız dizin izinlerini ayarlamak için yapmanız gereken ilk şey var. İzinleri ayarlamanız gerekir

```
chmod –R 744 <Installation bits folder>
```

Grafik Kurulum kullanılarak SAP HANA'ya yüklemek istiyorsanız, gtk2 paket HANA büyük örnekler üzerinde yüklü olması gerekir. Komutu ile yüklü olup olmadığını denetleyin

```
rpm –qa | grep gtk2
```

Daha fazla adımlarda size grafik kullanıcı arabirimi ile SAP HANA Kurulum gösteren. Sonraki adım olarak yükleme dizinine gidin ve HDB_LCM_LINUX_X86_64 alt dizinine gidin. Başlatma

```
./hdblcmgui 
```
Bu dizini. Artık, bir dizi ekranlar arasında veri sağlayacağınız için gerek duyduğunuz senaryolara gerçekleşir. Gösterilen durumda biz SAP HANA veritabanı sunucusu ve SAP HANA istemci bileşenlerini yüklüyorsunuz. Bu nedenle bizim 'SAP HANA veritabanı' aşağıda gösterildiği gibi seçimdir

![HANA yüklemede seçin](./media/hana-installation/image18_hana_selection.PNG)

Sonraki ekranda, yeni sistemi yükleyin' seçeneğini seçin

![Yeni yükleme HANA seçin](./media/hana-installation/image19_select_new.PNG)

Bu adımdan sonra ayrıca SAP HANA veritabanı sunucusuna yüklenebilir çeşitli ek bileşenler arasındaki seçmeniz gerekir.

![Ek HANA bileşenleri seçin](./media/hana-installation/image20_select_components.PNG)

Bu belgede, SAP HANA Client ve SAP HANA Studio seçtik. Ayrıca bir ölçek büyütme örneği yüklediğimiz. Bu nedenle 'Tek konak System' seçim yapması sonraki ekranda, 

![Ölçek büyütme yüklemesi seçin](./media/hana-installation/image21_single_host.PNG)

Sonraki ekranda, bazı veri sağlamanız gerekir.

![SAP HANA SID sağlayın](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> HANA sistemi kimliği (SID) olarak aynı SID, HANA büyük örneği dağıtım sıralı olduğunda Microsoft sağladığınız sağlamanız gerekir. Farklı bir SID seçerek farklı birimlerde erişim izin sorunları nedeniyle başarısız yükleme yapar

Yükleme olarak paylaşılan dizin directory/hana/kullanırsınız. Sonraki adımda, HANA veri dosyaları ve HANA günlük dosyaları için konumlar sağlamanız gerekir.


![HANA günlük konumu girin](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Verileri olarak tanımlayın ve günlük dosyaları zaten bu ekranı önce ekran seçimdeki seçtiğiniz SID içeren bağlama noktalarını birlikte gelen birimler gerekir. SID yazdığınız bir uyuşmazlık, ekranda önce geri dönüp SID bağlama noktalarına sahip değerine ayarlayın.

Sonraki adımda, ana bilgisayar adını gözden geçirin ve sonunda düzeltin. 

![Gözden geçirme ana bilgisayar adı](./media/hana-installation/image24_review_host_name.PNG)

Sonraki adımda, ayrıca, HANA büyük örneği dağıtım sıralı Microsoft'a verdiğiniz veri gerekir. 

![Sistem kullanıcısı sağlamak fazla](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Sipariş birim dağıtım Microsoft sağladığınız olarak aynı sistem kullanıcı kimliği ve kullanıcı grubu kimliği'nı sağlamanız gerekir. Çok aynı kimlikleri vermek başarısız olursa, HANA büyük örneği birim üzerinde SAP HANA yüklemesi başarısız olur.

Biz, bu belgede, SAP HANA veritabanı sapadm kullanıcının parolasını ve Sistem kullanıcısı için parola sağlamanız gereken görünmüyorsa, sonraki iki ekrana içinde SAP HANA datab bir parçası olarak yüklenen SAP konak aracısı için kullanılır ase örneği.

Parola tanımladıktan sonra bir onay ekranı başlanan bir uyarıdır. tüm veriler listelenir ve yükleme işlemine devam denetleyin. Yükleme ilerleme durumu gibi bir aşağıdaki belgeler bir ilerleme durumu ekranı ulaşın

![Yükleme ilerleme durumunu denetleme](./media/hana-installation/image27_show_progress.PNG)

Yükleme tamamlandıktan gibi aşağıdakine benzer bir resmi gerekir

![Yükleme tamamlandı](./media/hana-installation/image28_install_finished.PNG)

Bu noktada, SAP HANA örneği kullanım için hazır ve çalışır ve hazır olmalıdır. SAP HANA Studio'dan bağlanabilir olması gerekir. Ayrıca SAP HANA'ın en son düzeltme eklerinin denetleyin ve bu düzeltme ekleri uygulama emin olun.


**Sonraki adımlar**

- Başvuru [SAP HANA büyük örnekleri azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).

