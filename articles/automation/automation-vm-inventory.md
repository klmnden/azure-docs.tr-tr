---
title: "Bir Azure sanal makinesini stok toplama ile yönetme | Microsoft Docs"
description: "Bir sanal makineyi stok toplama ile yönetme"
services: automation
keywords: "stok, otomasyon, değişiklik, izleme"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: article
manager: carmonm
ms.openlocfilehash: 7b0e39e98a81231b68414f36ac5c1fc0897304a1
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Bir Azure sanal makinesini stok toplama ile yönetme

Bir Azure sanal makinesinin kaynak sayfasından sanal makine için stok izlemeyi etkinleştirebilirsiniz. Bu yöntem, stok toplamayı ayarlama ve yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

## <a name="before-you-begin"></a>Başlamadan önce
Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
Bir Azure sanal makineniz yoksa bir [sanal makine](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Sanal makine kaynak sayfasından stok toplamayı etkinleştirme

1. Azure portalının sol tarafında **Sanal makineler**'i seçin.
2. Sanal makine listesinden bir sanal makine seçin.
3. **Kaynak** menüsünde **İşlemler** altında **Sayım (Önizleme)** öğesini seçin.  
    Pencerenin üst kısmında, çözümün etkin olmadığını bildiren bir başlık görünür. 
4. Çözümü etkinleştirmek için başlığı seçin.
5. Veri günlüklerinizi depolamak için bir Log Analytics çalışma alanı seçin.  
    Bu bölge için kullanabileceğiniz bir çalışma alanı yoksa, varsayılan bir çalışma alanı ve otomasyon hesabı oluşturmanız istenir. 
6. Bilgisayarınızı eklemeye başlamak için **Etkinleştir**'i seçin.

   ![Ekleme seçeneklerini görüntüleme](./media/automation-vm-inventory/inventory-onboarding-options.png)  
    Çözümün etkinleştirildiği durum çubuğunda bildirilir. Bu işlemin tamamlanması 15 dakika sürebilir. Bu süre boyunca pencereyi kapatabilir veya açık tutabilirsiniz; çözüm etkinleştirildiğinde sizi bilgilendirecektir. Dağıtım durumunu bildirimler bölmesinden izleyebilirsiniz.

   ![Eklemeden hemen sonra stok çözümünü görüntüleme](./media/automation-vm-inventory/inventory-onboarded.png)

Dağıtım tamamlandığında durum çubuğu kaybolur. Sistem stok verilerini toplamaktadır ve veriler henüz görünmeyebilir. Tam bir veri toplama işlemi 24 saat sürebilir.

## <a name="configure-your-inventory-settings"></a>Stok ayarlarınızı yapılandırma

Varsayılan olarak yazılım, Windows hizmetleri ve Linux daemon'ları toplama işlemi için yapılandırılmıştır. Windows kayıt defteri ve dosya stoğunu toplamak için stok toplama ayarlarını yapılandırın.

1. **Sayım (Önizleme)** görünümünde, pencerenin üst kısmındaki **Ayarları Düzenle** düğmesini seçin.
2. Yeni bir toplama ayarı eklemek için **Windows Kayıt Defteri**, **Windows Dosyaları** ve **Linux Dosyaları** sekmelerini seçerek eklemek istediğiniz ayar kategorisine gidin. 
3. Pencerenin en üstündeki **Ekle**'yi seçin.
4. Her bir ayar özelliğinin ayrıntılarını ve açıklamalarını görüntülemek için [Stok belgeleri sayfasını](https://aka.ms/configinventorydocs) ziyaret edin.

## <a name="disconnect-your-virtual-machine-from-management"></a>Sanal makinenizin yönetim bağlantısını kesme

Sanal makinenizi stok yönetiminden kaldırmak için:

1. Azure portalının sol tarafındaki bölmeden **Log Analytics**'i ve sanal makineyi eklerken kullandığınız çalışma alanını seçin.
2. **Log Analytics** penceresinin **Kaynak** menüsünün **Çalışma Alanı Veri Kaynakları** kategorisinden **Sanal makineler**'i seçin. 
3. Listeden bağlantısını kesmek istediğiniz sanal makineyi seçin. Sanal makinenin yanında, **OMS Bağlantısı** sütunda **Bu çalışma alanı** ifadesini içeren bir yeşil onay işareti bulunur. 
4. Sonraki sayfanın en üstünde **Bağlantıyı kes**'i seçin.
5. Onay penceresinde **Evet**'i seçin.  
    Bu eylem makinenin yönetim paneli bağlantısını keser.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinelerinizdeki dosya ve kayıt defteri ayarlarında yapılan değişiklikleri yönetme hakkında bilgi almak için bkz. [Değişiklik İzleme çözümüyle ortamınızdaki yazılım değişikliklerini izleme](../log-analytics/log-analytics-change-tracking.md).
* Sanal makinelerinizde Windows ve paket güncelleştirmelerini yönetme hakkında bilgi almak için bkz. [OMS'de Güncelleştirme Yönetimi çözümü](../operations-management-suite/oms-solution-update-management.md).
