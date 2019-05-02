---
title: Microsoft Azure Data Box Gateway'e genel bakış | Microsoft Docs
description: Azure’a veri aktarabilmenizi sağlayan bir sanal gereç depolama çözümü olan Azure Data Box Gateway'i açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: overview
ms.date: 04/08/2019
ms.author: alkohli
ms.openlocfilehash: 2905cae6d5d97062d5a2fc1da41d55290d89fb4a
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924518"
---
# <a name="what-is-azure-data-box-gateway"></a>Azure veri kutusu ağ geçidi nedir?

Azure Data Box Gateway, Azure rahatça veri göndermenize olanak tanıyan bir depolama çözümüdür. Bu makalede Azure Data Box Gateway çözümüne, avantajlarına, önemli özelliklerine ve bu cihazı dağıtabileceğiniz senaryolara genel bir bakış sağlanır.

Data Box Gateway, sanallaştırılmış ortamınızda veya hiper yöneticinizde sağlanan sanal makineye dayalı bir sanal cihazdır. Sanal cihaz sizde durur ve NFS ile SMB protokollerini kullanarak buna verileri yazarsınız. Ardından cihaz verilerinizi Azure blok blobuna veya Azure Dosyaları'na aktarır.

## <a name="use-cases"></a>Uygulama alanları

Bulutta arşivleme, olağanüstü durum kurtarma gibi durumlarda veya verilerinizin bulut ölçeğinde işlenmesi gerektiğinde, verileri buluta aktarmak için Data Box Gateway'den yararlanılabilir. Burada Data Box Gateway'in veri aktarımı için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

- **Bulutta arşivleme** - Data Box Gateway'i kullanarak güvenli ve verimli bir yöntemle yüzlerce terabaytlık veriyi Azure depolamaya kopyalayın. Arşivleme senaryoları için veriler tek seferlik veya sürekli olarak alınabilir.

- **Sürekli veri alımı** -sürekli olarak veri boyutundan bağımsız olarak bulut kopyalamak için bir aygıt halinde veri alma. Veri ağ geçidi cihazı için yazıldıkça cihaz verileri Azure Depolama'ya yükler.  

- **İlk toplu aktarımı Artımlı aktarım tarafından izlenen** -toplu kullanım Data Box'a ağ üzerinden bir çevrimdışı moda (ilk çekirdek) ve veri kutusu ağ geçidi artımlı aktarımları (devam eden akış) için aktarın.

Daha fazla bilgi için Git [Azure veri kutusu ağ geçidi kullanım örnekleri](data-box-gateway-use-cases.md).

## <a name="benefits"></a>Avantajlar

Data Box Gateway'in şöyle avantajları vardır:

- **Kolay veri aktarımı**- Verileri Azure depolamasında içeri ve dışarı taşımayı, yerel ağ paylaşımıyla çalışma kadar kolay hale getirir.  
- **Yüksek performanslı** - Azure'a ve Azure'dan yüksek performanslı aktarımlarla ağda veri taşıma yükünü ortadan kaldırır.
- **Hızlı erişim ve yüksek veri alımı ücretlerine iş saatlerinde** -veri kutusu ağ geçidi sanal cihaz sağlandığında, yerel kapasite boyutu tanımlayan bir yerel önbellek vardır. Veri disk boyutu olarak başına belirtilmelidir [sanal cihaz en düşük gereksinimler](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). Yerel önbellek aşağıdaki avantajları sağlar:
    - Yerel önbellek, yüksek hızda veri alımı sağlar. Yüksek veri miktarı en yüksek çalışma saatleri sırasında alınır, önbellek verileri tutmak ve buluta yükleyin.
    - Yerel önbellek, belirli bir eşiği kadar hızlı okuma erişimi verir. Cihaz 50-%60 olana kadar tam, CİHAZDAN tüm okuma Hızlı hale getirecek önbellekten da erişilir. Cihaz üzerindeki kullanılan alanın bu eşiğin üzerinde giden sonra cihaz, yerel dosyaları kaldırmak başlar.
 
- **Sınırlı bant genişliği kullanımı** - Yoğun iş saatlerinde kullanımı sınırlandırmak amacıyla ağ kısıtlandığında bile veriler Azure'a yazılabilir.  

## <a name="key-capabilities"></a>Temel işlevler

Data Box Gateway'in şöyle özellikleri vardır:

|Özellik |Açıklama  |
|---------|---------|
|Hız     | Tümüyle otomatik ve son derece iyileştirilmiş veri aktarımı ve bant genişliği.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Data Box Gateway sistem gereksinimleri](data-box-gateway-system-requirements.md).|
|Veri erişimi     | Cihaz tarafından gönderilen verileri bulutta olduğunda, daha fazla bulut API'lerini doğrudan erişerek değiştirilebilir.|
|Hızlı erişim     | En son kullanılan dosyalara hızlı erişim için cihazda yerel önbellek.|
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımının güvenliğini sağlamak için BitLocker desteği       |
|Dayanıklılık     | Yerleşik ağ dayanıklılığı        |


## <a name="specifications"></a>Belirtimler

Data Box Gateway sanal cihazının belirtimleri şöyledir:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |
| Bellek  |En az 8 GB|
| Kullanılabilirlik|Tek düğüm|
| Diskler|İşletim sistemi diski: 250 GB <br> Veri diski: 2 TB en, ölçülü kaynak sağlanan ve SSD'ler ile desteklenir|
| Ağ arabirimleri |1 veya daha çok sanal ağ arabirimi|
| Yerel dosya paylaşımı protokolleri|SMB ve NFS  |
| Güvenlik|Cihaza ve verilere erişimin kilidini açmak için kimlik doğrulaması <br> Kullanım halindeki veriler AES-256 bit şifrelemesi kullanılarak şifrelenir|
| Yönetim|Yerel web kullanıcı arabirimi - Cihazın ilk kurulumu, tanılaması ve güç yönetimi <br> Azure portalı - Data Box Gateway cihazlarının gündelik yönetimi       |

## <a name="components"></a>Bileşenler

Data Box Gateway çözümü Data Box Gateway kaynağından, Data Box Gateway sanal cihazından ve yerel bir web kullanıcı arabiriminden oluşur.

- **Data Box Gateway sanal cihazı** - Sanallaştırılmış ortamınızda veya hiper yöneticinizde sağlanan sanal makineye dayalı olan ve Azure'a veri göndermenizi sağlayan cihaz.
    
- **Data Box Gateway kaynağı** – Azure portalında, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Gateway cihazını yönetmenize olanak tanıyan bir kaynak. Veri kutusu ağ geçidi kaynağı, cihaz, paylaşımları, kullanıcılar ve Uyarıları görüntülemek ve yönetmek için kullanın. Daha fazla bilgi için bkz. nasıl [Azure portalını kullanarak yönetme](data-box-gateway-manage-shares.md).

- **Veri kutusu yerel web kullanıcı Arabirimi** - tanılama Çalıştır, kapatma ve cihazı yeniden başlatmak, bir destek paketi oluşturmak için yerel web kullanıcı arabirimini kullanın veya bir hizmet isteği için Microsoft Support başvurun. Daha fazla bilgi için bkz. nasıl [yerel web kullanıcı arabirimini kullanarak yönetme](data-box-gateway-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Bölge kullanılabilirliği

Veri kutusu ağ geçidi fiziksel cihazı, Azure kaynak ve hedef depolama hesabına veri aktarımı tümü aynı bölgede olması gerekir.

- **Kaynak kullanılabilirliği** -bu sürüm için veri kutusu ağ geçidi kaynak genel bulut desteği aşağıdaki bölgelerde kullanılabilir:
    - **Amerika Birleşik Devletleri** -Doğu ABD
    - **Avrupa Birliği** - Batı Avrupa
    - **Asya Pasifik** -Güney Doğu Asya

    Veri kutusu ağ geçidi de Azure kamu bulutunda dağıtılabilir. Daha fazla bilgi için [Azure Government nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır.

    En iyi performansı elde etmek için, depolama hesaplarının Data Box verilerini depoladığı bölgeler cihazın bulunduğu yere yakın konumlandırılmalıdır. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar.


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Gateway sistem gereksinimlerini](data-box-gateway-system-requirements.md) gözden geçirin.
- [Data Box Gateway sınırlarını](data-box-gateway-limits.md) anlayın.
- Azure portalında [Azure Data Box Gateway](data-box-gateway-deploy-prep.md)’i dağıtın.

