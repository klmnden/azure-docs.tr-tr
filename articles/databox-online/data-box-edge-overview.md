---
title: Microsoft Azure Data Box Edge'e genel bakış | Microsoft Docs
description: Azure ağ tabanlı aktarım için fiziksel cihaz kullanan bir depolama çözümü olan Azure Data Box Edge açıklanır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 31a50d1dab0d5b62ab4dd549127e9092148bd88c
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438096"
---
# <a name="what-is-azure-data-box-edge"></a>Azure Data Box Edge nedir? 

Azure Data Box Edge verileri işlemenizi ve ağ üzerinden Azure'a göndermenizi sağlayan bir depolama çözümüdür. Bu makalede Data Box Edge çözümüne, avantajlarına, önemli özelliklerine ve bu cihazı dağıtabileceğiniz senaryolara genel bir bakış sağlanır. 

Data Box Edge güvenli veri aktarımını hızlandırmak için Microsoft tarafından sağlanan bir fiziksel cihaz kullanır. Fiziksel cihaz sizde durur ve NFS ile SMB protokollerini kullanarak buna verileri yazarsınız. 

Data Box Edge'de Data Box Gateway'in tüm özellikleri bulunur. Bunlara ek olarak Data Box, verileri Azure blok blobuna, sayfa blobuna veya Azure Dosyalarına taşırken analiz etmeye, işlemeye veya filtrelemeye yardımcı olan AI özellikli uç bilgi işlem özellikleriyle donatılmıştır.  

## <a name="use-cases"></a>Uygulama alanları

Azure Data Box Edge ağ veri aktarımı yetenekleri olan, AI özellikli bir uç bilgi işlem cihazıdır. Burada Data Box Edge'in veri aktarım için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

- **Verileri önceden işleme** - Verilerin oluşturulduğu noktanın yakınında kalarak hızla sonuç almak için verileri şirket içinden veya IoT cihazlarından analiz edin. Daha gelişmiş işleme veya daha derin analiz gerçekleştirmek için Data Box Edge veri kümesini eksiksiz bir biçimde buluta aktarır.  Önceden işleme şu amaçlarla kullanılabilir: 

    - Verileri toplama.
    - Örneğin Kişisel Bilgileri (PII) kaldırmak için verilerde değişiklik yapma.
    - Bulutta daha derin analiz için gereken verilerin alt kümesini oluşturma ve aktarma.
    - IoT Olaylarını analiz etme ve bunlara yanıt verme. 

- **Çıkarım Azure Machine Learning** - Data Box Edge ile, veriler buluta gönderilmeden önce üzerinde işlem yapılabilecek hızlı sonuçlar almak için Machine Learning (ML) modellerini çalıştırabilirsiniz. ML modellerinizi yeniden eğitmeye ve geliştirmeye devam etmek için tam veri kümesi aktarılır.

- **Verileri ağ üzerinden Azure'a aktarma** - Daha fazla işlem ve analiz yapmak için veya arşiv amacıyla Data Box Edge kullanarak verileri kolayca ve hızla Azure'a aktarın. 

## <a name="benefits"></a>Avantajlar

Data Box Edge'in şöyle avantajları vardır:

- **Kolay veri aktarımı**- Verileri Azure depolamasında içeri ve dışarı taşımayı, yerel ağ paylaşımıyla çalışma kadar kolaylaştırır.  
- **Yüksek performans** - Azure'un içine ve dışına yüksek performanslı aktarımlar sağlar. 
- **Hızlı erişim** - Şirket içi dosyalara hızlı erişim için en son kullanılan dosyaları önbelleğe alır.  
- **Sınırlı bant genişliği kullanımı** - Yoğun iş saatlerinde kullanımı sınırlandırmak amacıyla ağ kısıtlandığında bile veriler Azure'a yazılabilir.  
- **Verileri dönüştürme** - Verilerin Azure taşınırken analiz edilmesine, işlenmesine veya filtrelenmesine olanak tanır.

## <a name="key-capabilities"></a>Temel işlevler

Data Box Edge'in şöyle özellikleri vardır:

|Özellik |Açıklama  |
|---------|---------|
|Yüksek performans     | Tümüyle otomatik ve son derece iyileştirilmiş veri aktarımı ve bant genişliği.|
|Desteklenen protokoller     | Veri alımında standart SMB ve NFS protokolleri için destek. <br> Desteklenen sürümler hakkında daha fazla bilgi için bkz. [Data Box Edge sistem gereksinimleri](https://aka.ms/dbe-docs).|
|Bilgi işlem       |Verilerin analizine, işlenmesine, filtrelenmesine olanak tanır.|
|Veri erişimi     | Bulutta ek veri işleme için bulut API'lerini kullanarak Azure Depolama Blobları ve Azure Dosyaları'ndan doğrudan veri erişimi.|
|Hızlı erişim     | En son kullanılan dosyalara hızlı erişim için cihazda yerel önbellek.|
|Çevrimdışı karşıya yükleme     | Bağlantısız mod, çevrimdışı karşıya yükleme senaryolarını destekler.|
|Veri yenileme     | Yerel dosyaları buluttaki en son sürümle yenileme olanağı.|
|Şifreleme    | Verileri yerel olarak şifrelemek ve *https* üzerinden buluta veri aktarımının güvenliğini sağlamak için BitLocker desteği.       |
|Dayanıklılık     | Yerleşik ağ dayanıklılığı.        |


## <a name="components"></a>Bileşenler

Data Box Edge çözümü Data Box Edge kaynağından, Data Box Edge fiziksel cihazından ve yerel bir web kullanıcı arabiriminden oluşur.

* **Data Box Edge fiziksel cihazı** - Microsoft tarafından sağlanan ve Azure'a veri gönderecek şekilde yapılandırılabilen 1U rafa takılan sunucu. 
    
* **Data Box Edge kaynağı** – Azure portalında, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Edge cihazını yönetmenize olanak tanıyan bir kaynak. Data Box Edge kaynağını kullanarak kaynakları oluşturun ve yönetin, cihazlarla uyarıları görüntüleyin ve yönetin, paylaşımları yönetin.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Daha fazla bilgi için, [Data Box Edge cihazınızı yönetmek için Data Box Edge hizmetini kullanma](https://aka.ms/dbe-docs) konusuna gidin.

* **Data Box yerel web kullanıcı arabirimi** - Tanılama çalıştırmak, Data Box Edge cihazını kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için yerel web kullanıcı arabirimini kullanın.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Web tabanlı kullanıcı arabirimini kullanma hakkında bilgi için, [Data Box'ınızı yönetmek için web tabanlı kullanıcı arabirimini kullanma](https://aka.ms/dbe-docs) konusuna gidin.


## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Edge fiziksel cihazı, Azure kaynağı ve verileri aktardığınız hedef depolama hesabının üçünün de aynı bölgede bulunması gerekmez.

- **Kaynak kullanılabilirliği** - Bu sürümde, Data Box Edge kaynağı şu bölgelerde kullanılabilir:
    - **Amerika Birleşik Devletleri** -Doğu ABD
    - **Avrupa Birliği** - Batı Avrupa
    - **Asya Pasifik** - Güneydoğu Asya
    
    Veri kutusu ağ geçidi de Azure kamu bulutunda dağıtılabilir. Daha fazla bilgi için [Azure Government nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, tüm Azure bölgelerinde sağlanır. 

    En iyi performansı elde etmek için, depolama hesaplarının Data Box verilerini depoladığı bölgeler cihazın bulunduğu yere yakın konumlandırılmalıdır. Cihazdan uzağa konumlandırılan depolama hesabı uzun gecikme sürelerine ve daha yavaş bir performansa yol açar. 


## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Edge sistem gereksinimlerini](https://aka.ms/dbe-docs) gözden geçirin.
- [Data Box Edge sınırlarını](https://aka.ms/dbe-docs) anlayın.
- Azure portalda [Azure Data Box Edge](https://aka.ms/dbe-docs)’i dağıtın.




