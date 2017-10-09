---
title: "Stok toplama ile Azure VM yönetme | Microsoft Docs"
description: "Stok toplama ile VM yönetme"
services: automation
keywords: "stok, otomasyon, değişiklik, izleme"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: hero-article
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5291d112c2cdf157543fe301d5a5c80f3fe561ae
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Bir Azure sanal makinesini stok toplama ile yönetme

Bir Azure sanal makinesi için makinenin kaynak sayfasından stok izlemesi etkinleştirilebilir. Bu yöntem, stok toplamayı ayarlama ve yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
Bir Azure sanal makineniz yoksa, başlamadan önce bir [sanal makine](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Sanal makine kaynak sayfasından stok toplamayı etkinleştirme

1. Soldaki ekrandan **Sanal makineler**'i seçin.
1. Listeden bir sanal makine seçin.
1. **İşlemler** altındaki kaynak menüsünden **Stok (Önizleme)** öğesini seçin.
1. Sayfanın üst kısmında, çözümün etkin olmadığını bildiren bir başlık görünür. Çözümü etkinleştirmek için başlığa tıklayın.
1. Veri günlüklerinizi depolamak için bir Log Analytics Çalışma Alanı seçin. Bu bölge için kullanabileceğiniz bir çalışma alanı yoksa, varsayılan bir çalışma alanı ve Otomasyon hesabı oluşturmanız istenir. Bilgisayarınızı eklemeye başlamak için **Etkinleştir**’e tıklayın.

   ![Ekleme seçeneklerini görüntüleme](./media/automation-vm-inventory/inventory-onboarding-options.png)  

1. Çözümün etkinleştirildiği durum çubuğunda bildirilir. Bu işlemin tamamlanması 15 dakika sürebilir. Bu süre boyunca dikey pencereyi kapatabilir veya açık tutabilirsiniz; çözüm etkinleştirildiğinde size bildirilecektir. Dağıtım durumunu bildirimler bölmesinden izleyebilirsiniz.

   ![Eklemeden hemen sonra stok çözümünü görüntüleme](./media/automation-vm-inventory/inventory-onboarded.png)

1. Dağıtım tamamlandığında durum çubuğu kaybolur. Bu nokta sistem hala envanter verilerini toplamaktadır ve veriler henüz görünmeyebilir. Tam bir veri toplama işlemi 24 saat sürebilir.

## <a name="configure-your-inventory-settings"></a>Stok ayarlarınızı yapılandırma

Varsayılan olarak Yazılım, Windows Hizmetleri ve Linux Daemon’ları toplama işlemi için yapılandırılmıştır. Windows Kayıt Defteri ve Dosya stoğunu toplamak için stok toplama ayarlarını yapılandırın.

1. **Stok (Önizleme)** görünümünde, sayfanın üst kısmındaki **Ayarları Düzenle** düğmesini seçin.
2. Yeni bir toplama ayarı eklemek için **Windows Kayıt Defteri**, **Windows Dosyaları** ve **Linux Dosyaları** yazan sekmeleri kullanarak eklemek istediğiniz ayar kategorisine gidin. Sayfanın üstündeki **Ekle**'ye tıklayın.
3. Her bir ayar özelliğinin ayrıntılarını ve açıklamalarını görüntülemek için [Stok belgeleri sayfasını](https://aka.ms/configinventorydocs) ziyaret edin.

## <a name="disconnecting-your-virtual-machine-from-management"></a>Sanal makinenizin yönetim bağlantısını kesme

Makinenizi stok yönetiminden kaldırmak için:

1. Azure portalında sol taraftaki menüden **Log Analytics**’e tıklayın ve sanal makinenizi eklerken kullandığınız çalışma alanını tıklayarak seçin.
1. Log Analytics sayfanızda kaynak menüsündeki **Çalışma Alanı Veri Kaynakları** kategorisi altında bulunan **Sanal makineler**’i seçin. 
1. Listeden bağlantısını kesmek istediğiniz sanal makineyi seçin. Metnin yanında, **OMS Bağlantısı** sütunda "Bu çalışma alanı" ifadesini içeren bir yeşil onay işareti bulunur. Makinenin yönetim bağlantısını kesmek için sonraki sayfanın üstündeki **Bağlantıyı Kes**’e ve onay iletişim kutusunda **Evet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinelerinizdeki dosya ve kayıt defteri ayarlarında yapılan değişiklikleri yönetme hakkında bilgi almak için bkz. [Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md).
* Sanal makinelerinizde Windows ve paket güncelleştirmelerini yönetme hakkında bilgi almak için bkz. [Güncelleştirme Yönetimi](../operations-management-suite/oms-solution-update-management.md)

