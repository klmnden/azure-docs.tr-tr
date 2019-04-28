---
title: Veri aktarımı için bir Azure çözümü seçme | Microsoft Docs
description: Veri boyutları ve kullanılabilir ağ bant genişliği, ortamınızdaki göre veri aktarımı için bir Azure çözümünü nasıl seçeceğinizi öğrenin
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 12/10/2018
ms.author: alkohli
ms.openlocfilehash: 4e2a182493b1e9de3d2ba9d586a9560e42fe0ecb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61484083"
---
# <a name="choose-an-azure-solution-for-data-transfer"></a>Veri aktarımı için bir Azure çözümü seçin

Bu makalede bazı genel Azure veri aktarımına genel bakış çözümleri sağlar. Makale ayrıca kullanıma ortamınızı aktarmak istediğiniz veri boyutu, ağ bant genişliğini bağlı olarak önerilen seçeneklere bağlantılarını içerir.

## <a name="types-of-data-movement"></a>Veri taşıma türü

Veri aktarımı, çevrimdışı veya ağ bağlantısı üzerinden olabilir. Çözümünüzü yapılandırmanıza bağlı olarak seçin:

- **Veri boyutu** -veri aktarımı için hedeflenen boyutu
- **Aktarım sıklığı** -tek seferlik veya düzenli aralıklarla veri alımı ve
- **Ağ** – verileri için kullanılabilir bant genişliği ortamınıza aktarın.

Veri taşıma, aşağıdaki türde olabilir:

- **Sprint'in cihazları kullanarak çevrimdışı aktarımı** -fiziksel dağıtılamasa cihazlar çevrimdışı tek seferde toplu veri aktarımı yapmak istediğinizde kullanın. Microsoft, bir disk veya güvenli bir özel cihaz size gönderir. Alternatif olarak, satın alın ve kendi disklerinizi gönderin. Cihaza veri kopyalayın ve ardından nerede veriler karşıya yüklendikten Azure'a gönderin.  Bu durum için kullanılabilir seçenekler, Data Box Disk, Data Box, veri kutusu yoğun ve içeri/dışarı aktarma (kendi diskler kullanan) bulunur.

- **Ağ aktarım** -verilerinizi ağ bağlantınız üzerinden Azure'a aktarın. Bu, birçok bakımdan yapılabilir.

    - **Grafik arabirim** -bazen sadece birkaç dosya aktarımı ve veri aktarımı otomatik hale getirmek gerekmez, Azure Depolama Gezgini gibi bir grafik arabirim araç veya bir web tabanlı inceleme aracı Azure portalında seçebilirsiniz.
    - **Komut dosyası veya programlı aktarımı** -biz sağlayın veya bizim REST API'ler/SDK'lar doğrudan çağrı en iyi duruma getirilmiş yazılım araçlarını kullanabilirsiniz. AzCopy, Azure PowerShell ve Azure CLI kullanılabilir komut satırı araçları şunlardır. Programlama arabirimi, .NET, Java, Python, düğüm/JS, C++, Go, PHP veya Ruby için Sdk'lardan birini kullanın.
    - **Şirket içi cihazlar** -biz, veri merkezinde yer alır ve ağ üzerinden veri aktarımı iyileştirir bir fiziksel veya sanal cihaz sağlayın. Bu cihazlar, sık kullanılan dosyaları yerel önbelleğini de sağlar. Fiziksel cihazın veri kutusu Edge ve sanal cihaz ağ geçidiyle kutusu. Hem kalıcı olarak şirket içinde çalıştırın ve ağ üzerinden Azure'a bağlanın.
    - **Yönetilen veri işlem hattı** -düzenli olarak çeşitli Azure Hizmetleri, şirket içi veya iki birleşimi arasında dosyaları aktarmak için bir bulut işlem hattı ayarlayın. Azure Data Factory, ayarlamak ve veri işlem hatlarını yönetmek ve analiz için verilerin dönüştürmek için kullanın.

Aşağıdaki görselde veri boyutu, aktarımı ve aktarım sıklığı için hedeflenen aktarımı için kullanılabilir ağ bant genişliği bağlı Araçlar çeşitli Azure veri aktarımı seçmek için yönergeleri gösterilmektedir.

![Azure veri aktarım araçları](media/storage-choose-data-transfer-solution/azure-data-transfer-options-3.png)

**Üst sınırları çevrimdışı aktarımı cihazların - Data Box Disk, Data Box ve veri kutusu ağır bir cihaz türünde birden fazla siparişi yerleştirerek genişletilebilir.*

## <a name="selecting-a-data-transfer-solution"></a>Bir veri aktarım çözümü seçme

Bir veri aktarım çözümü seçmenize yardımcı olması için aşağıdaki soruları yanıtlayın:

- Kullanılabilir ağ bant genişliği sınırlı veya var olmayan ve büyük veri kümelerini aktarmak istersiniz?
  
    Yanıt Evet ise, bkz: [Senaryo 1: Olmadan büyük veri kümelerini aktarma veya düşük ağ bant genişliği](storage-solution-large-dataset-low-network.md).
- Büyük veri kümeleri, ağ üzerinden aktarmak istediğiniz ve bir orta yüksek ağ bant genişliğine sahip?

    Yanıt Evet ise, bkz: [Senaryo 2: Büyük veri kümeleriyle Orta yüksek ağ bant genişliğini aktarım](storage-solution-large-dataset-moderate-high-network.md).
- Bazen ağ üzerinden yalnızca birkaç dosya aktarmak istiyor musunuz?

    Yanıt Evet ise bkz [Senaryo 3: Orta ağ bant genişliği sınırlı küçük veri kümelerini aktarma](storage-solution-small-dataset-low-moderate-network.md).
- Zaman içinde nokta veri aktarımı için düzenli aralıklarla arıyorsunuz?

    Yanıt Evet ise, özetlenen komut dosyası/programlı seçenekleri kullanın [Senaryo 4: Düzenli aralıklarla veri aktarımları](storage-solution-periodic-data-transfer.md).
- Devam eden, sürekli veri aktarımı için mı arıyorsunuz?

    Yanıt Evet ise, seçenekleri kullanın [Senaryo 4: Düzenli aralıklarla veri aktarımları](storage-solution-periodic-data-transfer.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama Gezgini giriş yapın](https://azure.microsoft.com/resources/videos/introduction-to-microsoft-azure-storage-explorer/).
- [AzCopy genel bir bakış edinin](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10).
- [Azure PowerShell'i Azure depolama ile kullanma](https://docs.microsoft.com/azure/storage/common/storage-powershell-guide-full)
- [Azure CLI ile Azure depolama kullanma](https://docs.microsoft.com/azure/storage/common/storage-azure-cli)
- Hakkında bilgi edinin:

    - [Azure Data Box, Azure Data Box Disk ve Azure veri kutusu ağır çevrimdışı aktarımları](https://docs.microsoft.com/azure/databox/).
    - [Azure veri kutusu ağ geçidi ve Azure veri kutusu Edge çevrimiçi aktarımları](https://docs.microsoft.com/azure/databox-online/).
- [Azure Data Factory nedir öğrenin](https://docs.microsoft.com/azure/data-factory/copy-activity-overview).
- Veri aktarımı için REST API'lerini kullanma

    - [. NET'te](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [Java'da](https://docs.microsoft.com/java/api/overview/azure/storage/client)
