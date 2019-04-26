---
title: Microsoft Azure Data Box Heavy'ye genel bakış | Verilerde Microsoft Docs
description: Muazzam miktarlarda verinin Azure’a aktarılmasını sağlayan bir hibrit çözüm olan Azure Data Box açıklanır
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 0a5b7f93f9ac6cc5b1076881727a42fd5b95ff4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306198"
---
# <a name="what-is-azure-data-box-heavy-preview"></a>Azure Data Box Heavy nedir? (Önizleme)

Microsoft Azure Data Box hibrit çözümü, yüzlerce terabayt veriyi Azure'a hızlı, uygun maliyetli ve güvenilir bir şekilde göndermenizi sağlar. Size kargoyla 1 PB depolama kapasitesine sahip özel bir depolama cihazı gönderilerek güvenli veri aktarımı hızlandırılır. Ulaşım sırasında verileri korumalı ve güvenli tutmak için cihaz sağlamlaştırılmıştır.

Data Box Heavy şu anda önizleme aşamasındadır ve Azure portalı üzerinde cihaz istemek için kaydolabilirsiniz. Cihaz veri merkezinize ulaştıktan sonra, yerel web kullanıcı arabirimini kullanarak cihazı ayarlayabilirsiniz. Sunucularınızdaki verileri cihaza kopyalayın ve cihazı Azure'a geri gönderin. Azure veri merkezinde, verileriniz cihazdan Azure'a otomatik olarak yüklenir. Sürecin tamamı Azure portalındaki Data Box hizmeti tarafından uçtan uca izlenir.


> [!IMPORTANT]
> - Data Box Heavy önizleme aşamasındadır. Bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
> - Cihaz istemek için [Önizleme portalına](https://aka.ms/) kaydolun.
> - Önizleme sırasında Data Box Heavy, ABD ve Avrupa Birliği ülkelerindeki müşterilere gönderilebilir. Daha fazla bilgi için bkz. [Bölge kullanılabilirliği](#region-availability).

## <a name="use-cases"></a>Uygulama alanları

Data Box Heavy, ağ bağlantısının sınırlı olduğu veya hiç olmadığı senaryolarda 500 TB'tan büyük boyutlu veri aktarımları için idealdir. Veri taşıma işlemi tek seferlik, düzenli veya başta toplu veri aktarımı ve ardından düzenli aktarımlardan oluşabilir. Burada Data Box Heavy'nin veri aktarımı için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

 - **Tek seferlik geçiş**: Büyük miktardaki şirket içi verilerinin Azure'a taşınmasıdır. 
     - Çevrimiçi bir medya kitaplığı oluşturmak için bir medya kitaplığını çevrimdışı teyplerden Azure'a taşıma.
     - VM grubunuzu, SQL sunucunuzu ve uygulamalarınızı Azure'a geçirme
     - HDInsight kullanarak derin analiz ve raporlama yapmak için geçmiş verilerini Azure'a taşıma

 - **İlk toplu aktarım**: Data Box Heavy (seed) ile toplu veri aktarımı yapıldıktan sonra ağ üzerinden artımlı aktarım işlemlerinin gerçekleştirilmesidir. 
     - Örneğin, ilk büyük geçmiş yedeklemesini Azure'a taşımak için Commvault ve Data Box Heavy gibi yedekleme çözümü iş ortakları kullanılır. İşlem tamamlandıktan sonra, artımlı veriler ağ üzerinden Azure depolamasına aktarılır.

 - **Düzenli yüklemeler**: Düzenli olarak oluşturulan büyük miktarlardaki verilerin Azure'a taşınması gerektiği durumlardır. Enerji keşfinde petrol kuyularında ve rüzgar enerjisini santrallerinde oluşturulan videolar örnek olarak verilebilir.      

## <a name="benefits"></a>Avantajlar

Data Box Heavy, muazzam miktarlarda veriyi ağ bağlantısını çok az etkileyerek veya hiç etkilemeden Azure'a taşımak için tasarlanmıştır. Çözümün şöyle avantajları vardır:

- **Hız**: Data Box Heavy yüksek performanslı 40 Gb/sn ağ arabirimlerini kullanır.

- **Güvenli**: Data Box Heavy cihaz, veriler ve hizmet için yerleşik güvenlik önlemlerine sahiptir.
    - Cihaz, üzerinde oynanmaya karşı korumalı vidalar ve üzerinde oynandığını belli eden çıkartmalarla korunan, sağlamlaştırılmış bir cihaz kasasına sahiptir. 
    - Cihazdaki veriler her zaman AES 256 bit ile şifrelenir.
    - Cihaz kilidi yalnızca Azure portalından alınan parolayla açılabilir.
    - Hizmet, Azure güvenlik özelliklerinin koruması altındadır.
    - Verileriniz Azure'a yüklendikten sonra cihazdaki diskler NIST 800-88r1 standartlarına göre silinir.


<!--## Features and specifications

The Data Box Heavy device has the following features in this release.

| Specifications                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Weight                                                  | < 50 lbs.                |
| Dimensions                                              | Device - Width: 309.0 mm Height: 430.4 mm Depth: 502.0 mm |            
| Rack space                                              | 7 U when placed in the rack on its side (cannot be rack-mounted)|
| Cables required                                         | 1 X power cable (included) <br> 2 RJ45 cables <br> 2 X SFP+ Twinax copper cables|
| Storage capacity                                        | 100 TB <br> 80 TB usable capacity after RAID 5 protection|
| Network interfaces                                      | 2 X 1 GbE interface - MGMT, DATA 3. <br> MGMT - for management, not user configurable, used for initial setup <br> DATA3 - for data, user configurable, and is dynamic by default <br> MGMT and DATA 3 can also work as 10 GbE <br> 2 X 10 GbE interface - DATA 1, DATA 2 <br> Both are for data, can be configured as dynamic (default) or static |
| Data transfer media                                     | RJ45, SFP+ copper 10 GbE Ethernet  |
| Security                                                | Rugged device casing with tamper-proof custom screws <br> Tamper-evident stickers placed at the bottom of the device|
| Data transfer rate                                      | Up to 80 TB in a day over 10 GbE network interface        |
| Management                                              | Local web UI - one-time initial setup and configuration <br> Azure portal - day-to-day device management        |-->

## <a name="components"></a>Bileşenler

Data Box Heavy aşağıdaki bileşenleri içerir:

* **Data Box Heavy cihazı**: Verileri güvenle depolayan, dış yüzeyi sağlamlaştırılmış bir fiziksel cihaz. Bu cihazın 800 TB kullanılabilir depolama kapasitesi vardır. 

    
* **Data Box hizmeti**: Azure portalının, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Heavy cihazını yönetmenize olanak tanıyan bir uzantısı. Data Box Heavy cihazınızın gündelik yönetimini gerçekleştirmek için Data Box hizmetini kullanın. Hizmetin görevleri, siparişleri oluşturma ve yönetmeyi, uyarıları görüntülemeyi ve yönetmeyi, paylaşımları yönetmeyi içerir.  

* **Yerel web kullanıcı arabirimi**: Yerel ağa bağlanabilmesi için cihazı yapılandırmak ve ardından Data Box hizmetine kaydetmek için kullanılan, web tabanlı kullanıcı arabirimi. Yerel web kullanıcı arabirimini cihazı kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için de kullanın.


## <a name="the-workflow"></a>İş akışı

Tipik iş akışı aşağıdaki adımlardan oluşur:

1. **Sipariş**: Azure portaldan bir sipariş oluşturup gönderim bilgilerini ve verileriniz için hedef Azure depolama hesabını belirtin. Cihaz varsa, Azure cihazı hazırlar, gönderir ve bir gönderi takip numarası iletir.

2. **Alma**: Cihaz teslim edildiğinde, belirtilen kabloları kullanarak cihazı ağa ve güç kaynağına kabloyla bağlayın. Cihazı açın ve bağlayın. Cihaz ağını yapılandırın ve verileri kopyalamak istediğiniz konak bilgisayarına paylaşımları takın.

3. **Veri kopyalama**: Verileri Data Box Heavy paylaşımlarına kopyalayın.

4. **İade etme**: Cihazı hazırlayın, kapatın ve Azure veri merkezine geri gönderin.

5. **Karşıya yükleme**: Veriler otomatik olarak cihazdan Azure'a kopyalanır. Cihazın diskleri Ulusal Standart ve Teknoloji Kurumu (NIST) yönergelerine uygun ve güvenli bir şekilde silinir.

Bu işlem boyunca tüm durum değişiklikleri e-posta ile bildirilir. 

## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Heavy, hizmetin dağıtıldığı bölge, cihazın gönderildiği ülke ve verileri aktardığınız hedef Azure depolama hesabı temelinde verileri aktarabilir. 

- **Hizmet kullanılabilirliği** - Bu sürümde, Data Box Heavy şu bölgelerde kullanılabilir:
    - Birleşik Devletler'deki tüm genel bulut bölgeleri: Orta Batı ABD, Batı ABD2, Batı ABD, Orta Güney ABD, Orta ABD, Orta Kuzey ABD, Doğu ABD ve Doğu ABD2.
    - Avrupa Birliği: Batı Avrupa ve Kuzey Avrupa.
    - Birleşik Krallık: UK Güney ve UK Batı.
    - Fransa: Fransa Orta ve Fransa Güney.

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, hizmetin kullanılabildiği tüm Azure bölgelerinde sağlanır. 

## <a name="sign-up"></a>Kaydolma

Data Box Heavy önizleme aşamasındadır ve kaydolmanız gerekir. Data Box Heavy'ye kaydolmak için aşağıdaki adımları izleyin:

1. Azure portalında oturum açın: https://aka.ms/azuredatabox.
2. Yeni kaynak oluşturmak için **+** işaretine tıklayın. **Azure Data Box** için arama yapın. **Azure Data Box** hizmetini seçin.

    <!--![The Data Box Heavy sign up 1]()-->

3. **Oluştur**’a tıklayın.

    <!--![The Data Box Heavy sign up 2]()-->

4. Data Box Heavy önizlemesinde kullanmak istediğiniz aboneliği seçin. Data Box Heavy kaynağını dağıtmak istediğiniz bölgeyi seçin. **Data Box Heavy** seçeneğinde **Kaydol**'a tıklayın.

   <!--![The Data Box Heavy sign up 3]()-->

5. Verilerin bulunduğu ülke, zaman aralığı ve veri aktarımı için hedef Azure hizmeti, ağ bant genişliği ve veri aktarım sıklığı hakkındaki soruları yanıtlayın. Gizlilik ve koşulları gözden geçirdikten sonra, Microsoft sizinle iletişim kurmak için e-posta adresinizi kullanabilir onay kutusunu seçin.

    <!--![The Data Box Heavy sign up 4]()-->

Kaydolduktan ve önizlemeyi etkinleştirdikten sonra Data Box Heavy siparişi verebilirsiniz.




