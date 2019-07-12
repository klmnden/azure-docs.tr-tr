---
title: Microsoft Azure Data Box Heavy'ye genel bakış | Verilerde Microsoft Docs
description: Muazzam miktarlarda verinin Azure’a aktarılmasını sağlayan bir hibrit çözüm olan Azure Data Box açıklanır
services: databox
documentationcenter: NA
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: overview
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 0f4657cdd71a104ca111f62a6e9757b5a33b46e8
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592309"
---
# <a name="what-is-azure-data-box-heavy"></a>Azure Data Box Heavy nedir?

Azure veri kutusu ağır yüzlerce terabayt boyutunda veriyi hızlı, ucuz, Azure'da ve güvenilir bir şekilde göndermenizi sağlar. Veriler, veri kutusu ağır bir cihaz ile veri doldurun ve Microsoft'a geri gönderir 1-PB depolama kapasitesi ile sevkiyat tarafından Azure'a aktarılır. Cihaz koruma ve iletim sırasında verilerinizi güvenli bir rugged büyük/küçük harf sahiptir.

Cihaz, veri merkezinde alındıktan sonra yerel web kullanıcı arabirimini kullanarak ayarlayabilirsiniz. Sunucularınızdaki verileri cihaza kopyalayın ve cihazı Azure'a geri gönderin. Azure veri merkezinde, Azure depolama hesaplarınızda verilerinizi karşıya yüklendi. Azure portalında tüm uçtan uca işlem izleyebilirsiniz.


> [!IMPORTANT]
> - Bir cihaz istemek için de kaydolun [Azure portalında](https://portal.azure.com).


## <a name="use-cases"></a>Uygulama alanları

Veri kutusu yoğun ağ bağlantısı verileri Azure'a karşıya yüklemek için yeterli olduğu yüzlerce terabayt, içinde veri boyutları için idealdir. Veri taşıma işlemi tek seferlik, düzenli veya başta toplu veri aktarımı ve ardından düzenli aktarımlardan oluşabilir. Burada Data Box Heavy'nin veri aktarımı için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

 - **Tek seferlik geçiş**: Büyük miktardaki şirket içi verilerinin Azure'a taşınmasıdır.
     - Ortam Kitaplığı çevrimdışı bantlardan çevrimiçi medya kitaplığı oluşturmak için Azure'a taşıyın.
     - Bir VM grubu, SQL server ve uygulamaları azure'a geçirin.
     - Geçmiş verileri ayrıntılı analiz ve HDInsight'ı kullanarak raporu için Azure'a taşıyın.

 - **İlk toplu aktarım**: Data Box Heavy (seed) ile toplu veri aktarımı yapıldıktan sonra ağ üzerinden artımlı aktarım işlemlerinin gerçekleştirilmesidir.
     - Örneğin, veri kutusu yoğun ve yedekleme çözümleri iş ortağı ilk büyük geçmiş yedekleme Azure'a taşımak için kullanılır. İşlem tamamlandıktan sonra, artımlı veriler ağ üzerinden Azure depolamasına aktarılır.

 - **Düzenli yüklemeler**: Düzenli olarak oluşturulan büyük miktarlardaki verilerin Azure'a taşınması gerektiği durumlardır. Enerji keşfinde petrol kuyularında ve rüzgar enerjisini santrallerinde oluşturulan videolar örnek olarak verilebilir.

## <a name="benefits"></a>Avantajlar

Veri kutusu ağır oldukça büyük miktardaki verileri Azure'a herhangi bir etkisi çok az ağınızda taşımak için tasarlanmıştır. Çözümün şöyle avantajları vardır:

- **Hızı** -veri kutusu ağır yüksek performanslı 40 GB/sn ağ arabirimleri kullanır.

- **Güvenlik** -cihaz, veri ve hizmet için yerleşik güvenlik korumaları veri kutusu ağır sahip.
    - Cihaz, üzerinde oynanmaya karşı korumalı vidalar ve üzerinde oynandığını belli eden çıkartmalarla korunan, sağlamlaştırılmış bir cihaz kasasına sahiptir.
    - Cihazdaki veriler her zaman AES 256 bit ile şifrelenir.
    - Cihaz kilidi yalnızca Azure portalından alınan parolayla açılabilir.
    - Hizmet, Azure güvenlik özelliklerinin koruması altındadır.
    - Verilerinizi Azure'a karşıya yüklendikten sonra cihaz disklerde temiz Ulusal Standartlar ve teknoloji kurumu (NIST) 800-88r1 standartlara uygun olarak temizlenir.


## <a name="features-and-specifications"></a>Özellikler ve belirtimler

Veri kutusu ağır cihaz bu sürümde aşağıdaki özelliklere sahiptir.

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Ağırlık                                                  | yaklaşık 500 lbs. <br>Taşıma için tekerlek kilitleme üzerinde cihaz|
| Boyutlar                                              | Genişlik: 26 inç Yükseklik: 28 inç uzunluğu: 48 inç yükseklik |
| Raf alanı                                              | Rafa monte edilen olamaz|
| Gerekli kablolar                                         | 4 üzerindeki olan bağlılığımızı temel V 120 / 10 A güç kablosu (NEMA 5-15) dahil <br> Cihaz kadar 240 V power destekler ve C-13 power kutularını <br> Uyumlu ağ kablosu kullanın [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html)  |
| Güç                                                    | Her iki cihaz düğümleri arasında paylaşılan 4 yerleşik güç kaynağı birimi (PSUs) <br> 1\.200 watt tipik power Çiz|
| Depolama kapasitesi                                        | ~ 1-PB ham, 14 TB 70 diskler <br> 770 TB kullanılabilir kapasite|
| Düğüm sayısı                                          | (500 TB her) cihaz başına 2 bağımsız düğüm |
| Düğüm başına ağ arabirimleri                             | Düğüm başına 4 ağ arabirimleri <br><br> MGMT, DATA3 <ul><li> 2 x 1 GbE ağ arabirimleri </li><li> Yönetim ve ilk kurulum değil kullanıcı tarafından yapılandırılabilir MGMT içindir. </li><li> Varsayılan olarak kullanıcı tarafından yapılandırılabilir ve Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) DATA3 olduğu</li><li>1 GbE ağ arabirimleri de 10 GbE arabirimleri yapılandırılabilir</li></ul>Veri1, veri2 veri arabirimleri <ul><li>2 x 40 GbE arabirimleri </li><li> Kullanıcı (varsayılan) DHCP veya statik için yapılandırılabilir</li></ul>|


## <a name="components"></a>Bileşenler

Data Box Heavy aşağıdaki bileşenleri içerir:

* **Data Box Heavy cihazı**: Verileri güvenle depolayan, dış yüzeyi sağlamlaştırılmış bir fiziksel cihaz. Bu cihaz 770 TB kullanılabilir depolama kapasitesine sahiptir.
    
* **Data Box hizmeti**: Azure portalının, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Heavy cihazını yönetmenize olanak tanıyan bir uzantısı. Data Box hizmeti, veri kutusu ağır Cihazınızı yönetmek için kullanın. Hizmetin görevleri, siparişleri oluşturma ve yönetmeyi, uyarıları görüntülemeyi ve yönetmeyi, paylaşımları yönetmeyi içerir.  

* **Yerel web kullanıcı arabirimi**: Yerel ağa bağlanabilmesi için cihazı yapılandırmak ve ardından Data Box hizmetine kaydetmek için kullanılan, web tabanlı kullanıcı arabirimi. Yerel web kullanıcı arabirimini cihazı kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için de kullanın.


## <a name="the-workflow"></a>İş akışı

Tipik iş akışı aşağıdaki adımlardan oluşur:

1. **Sipariş**: Azure portaldan bir sipariş oluşturup gönderim bilgilerini ve verileriniz için hedef Azure depolama hesabını belirtin. Cihaz varsa, Azure cihazı hazırlar, gönderir ve bir gönderi takip numarası iletir.

2. **Alma**: Cihaz teslim edildiğinde, belirtilen kabloları kullanarak cihazı ağa ve güç kaynağına kabloyla bağlayın. Cihazı açın ve bağlayın. Cihaz ağını yapılandırın ve verileri kopyalamak istediğiniz konak bilgisayarına paylaşımları takın.

3. **Veri kopyalama**: Verileri Data Box Heavy paylaşımlarına kopyalayın.

4. **İade etme**: Cihazı hazırlayın, kapatın ve Azure veri merkezine geri gönderin.

5. **Karşıya yükleme**: Veriler otomatik olarak cihazdan Azure'a kopyalanır. Cihazın diskleri Ulusal Standart ve Teknoloji Kurumu (NIST) yönergelerine uygun ve güvenli bir şekilde silinir.

Bu işlem boyunca tüm durum değişikliklerini şirket e-posta ile bildirim alırsınız.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Veri kutusu ağır hizmet dağıtılan bölgeye göre veri aktarabilir cihazı sevk ülke/bölge ve ' % s'hedef Azure depolama hesabı veri aktarma burada.

- **Hizmet kullanılabilirliği** - Bu sürümde, Data Box Heavy şu bölgelerde kullanılabilir:
    - Birleşik Devletler'deki tüm genel bulut bölgeleri: Orta Batı ABD, Batı ABD2, Batı ABD, Orta Güney ABD, Orta ABD, Orta Kuzey ABD, Doğu ABD ve Doğu ABD2.
    - Avrupa Birliği: Batı Avrupa ve Kuzey Avrupa.
    - Birleşik Krallık: UK Güney ve UK Batı.
    - Fransa: Fransa Orta ve Fransa Güney.

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, hizmetin kullanılabildiği tüm Azure bölgelerinde sağlanır.

En güncel bilgiler için bölge kullanılabilirliği için veri kutusu ağır, Git [bölgelere göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all).

## <a name="sign-up"></a>Kaydolma

Veri kutusu ağır için kaydolmak için aşağıdaki adımları uygulayın:

1. Azure portalında oturum açın: https://portal.azure.com.
2. Tıklayın **+ kaynak Oluştur** yeni bir kaynak oluşturmak için. **Azure Data Box** için arama yapın. **Azure Data Box** hizmetini seçin.
3.           **Oluştur**'a tıklayın.
4. Veri kutusu ağır için kullanmak istediğiniz aboneliği seçin. Data Box Heavy kaynağını dağıtmak istediğiniz bölgeyi seçin. **Data Box Heavy** seçeneğinde **Kaydol**'a tıklayın.
5. Veri ile ilgili sorularını ikametgahınızda ülke/bölge, zaman çerçevesini, hedef Azure hizmeti için veri aktarımı, ağ bant genişliği ve veri aktarım sıklığı. Gizlilik ve koşulları gözden geçirdikten sonra, Microsoft sizinle iletişim kurmak için e-posta adresinizi kullanabilir onay kutusunu seçin.

Açıldıktan sonra bir veri kutusu ağır sipariş edebilirsiniz.

    
