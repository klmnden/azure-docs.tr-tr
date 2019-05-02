---
title: include dosyası
description: include dosyası
services: cost-management
author: bandersmsft
ms.service: cost-management
ms.topic: include
ms.date: 09/17/2018
ms.author: banders
manager: dougeby
ms.custom: include file
ms.openlocfilehash: 1ffa56caebf16b588dffaba249a844915f9f44c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027294"
---
## <a name="view-cost-data"></a>Maliyet verilerini görüntüleme

Cloudyn'in sunduğu Azure Maliyet Yönetimi, bulut kaynak verilerinizin tamamına erişmenizi sağlar. Pano halinde sunulan raporlarda hem standart hem de özel raporları sekmeler halinde görüntüleyebilirsiniz. Aşağıda maliyet verilerini doğrudan görebileceğiniz popüler panolar ve rapor örnekleri gösterilmiştir.

![Yönetim panosu](./media/cost-management-create-account-view-data/mgt-dash.png)

Bu örnekte Yönetim panosu Contoso şirketinin tüm bulut kaynaklarına ait toplam maliyetleri göstermektedir. Contoso şirketi Azure, AWS ve Google hizmetlerini kullanmaktadır. Panolar bir bakışta görülebilecek bilgiler ve raporlarda gezinmek için pratik bir yol sunmaktadır.  

Panoda yer alan bir raporun amacı konusunda bilginiz yoksa **i** simgesinin üzerine gelerek açıklamasına göz atabilirsiniz. Panoda yer alan bir raporun tamamını görüntülemek için üzerine tıklamanız yeterlidir.

Raporları görüntülemek için portalın en üst bölümünde yer alan raporlar menüsünü de kullanabilirsiniz. Şimdi Contoso'nun son 30 gün içindeki Azure kaynağı harcamalarına göz atalım. **Maliyet** > **Maliyet Analizi** > **Gerçek Maliyet Analizi**’ne tıklayın. Varsa raporunuzdaki etiket, grup veya filtre değerlerini silin.

![Gerçek Maliyet Analizi](./media/cost-management-create-account-view-data/actual-cost-01.png)

Bu örnekte toplam maliyet 122.273 USD, bütçe ise 290.000 USD’dir.

Şimdi rapor biçimini değiştirerek grupları ve filtreleri Azure maliyetlerini gösterecek şekilde ayarlayalım. **Date Range** (Tarih Aralığı) için son 30 günü seçin. Sağ üst bölümde sütun simgesine tıklayarak çubuk grafik biçimini seçin ve Groups (Gruplar) bölümünde **Provider** (Sağlayıcı) öğesini belirleyin. Ardından **Provider** (Sağlayıcı) için **Azure** filtresi oluşturun.

![Filtrelenmiş Gerçek Maliyet Analizi](./media/cost-management-create-account-view-data/actual-cost-02.png)

Bu örnekte son 30 güne ait Azure kaynakları toplam maliyeti 3.309 USD olmuştur.

Provider (Azure) (Sağlayıcı) çubuğuna sağ tıklayın ve **Resource types** (Kaynak türleri) detayına gidin.

![detaya gitme](./media/cost-management-create-account-view-data/actual-cost-03.png)

Aşağıdaki resimde Contoso'nun ödemesi gereken Azure kaynakları maliyeti gösterilmiştir. Toplam 3.309 USD’dir. Bu örnekte maliyetin yarısına yakını Standard_A1 sanal makineleri için, diğer yarısı ise çeşitli Azure hizmetleri ve sanal makine örnekleri için tahakkuk ettirilmiştir.

![kaynak türleri](./media/cost-management-create-account-view-data/actual-cost-04.png)

Bir kaynağı tüketen maliyet varlıklarını ve hizmetleri görüntülemek için kaynak türüne sağ tıklayıp **Cost Entities** (Maliyet Varlıkları) öğesini seçin. Aşağıdaki örnek görüntüde Kaynak türü olarak Yerel Olarak Yedekli Depolama ayarlanmıştır. Contoso|Azure/Kullanılan depolama 15.65 USD. Mühendislik | Kullanılan Azure Depolama 164.25 USD. Paylaşılan Altyapı | Azure/Kullanılan Depolama 116.58 USD. Hizmetler için toplam maliyet 296 USD'dir.

![maliyet varlıkları ve hizmetler](./media/cost-management-create-account-view-data/actual-cost-05.png)

Bulut faturalama verilerinizi görüntüleme hakkında öğretici bir video izlemek için bkz. [Cloudyn Azure Maliyet Yönetimi ile bulut faturalama verilerinizi çözümleme](https://youtu.be/G0pvI3iLH-Y).
