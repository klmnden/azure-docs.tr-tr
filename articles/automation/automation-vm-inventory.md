---
title: Bir Azure sanal makinesini stok toplama ile yönetme | Microsoft Docs
description: Bir sanal makineyi stok toplama ile yönetme
services: automation
ms.service: automation
ms.component: change-inventory-management
keywords: stok, otomasyon, değişiklik, izleme
author: jennyhunter-msft
ms.author: jehunte
ms.date: 03/30/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 9f9c157fc8d1674aefcfea2ddea2aab3a4e86444
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Bir Azure sanal makinesini stok toplama ile yönetme

Bir Azure sanal makinesinin kaynak sayfasından sanal makine için stok izlemeyi etkinleştirebilirsiniz. Bu yöntem, stok toplamayı ayarlama ve yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

## <a name="before-you-begin"></a>Başlamadan önce

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu makale, çözümü yapılandırmak için bir VM olduğunu varsayar. Bir Azure sanal makineniz yoksa bir [sanal makine](../virtual-machines/windows/quick-create-portal.md) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Sanal makine kaynak sayfasından stok toplamayı etkinleştirme

1. Azure portalının sol tarafında **Sanal makineler**'i seçin.
2. Sanal makine listesinden bir sanal makine seçin.
3. Üzerinde **kaynak** menüsü altında **Operations**seçin **stok**.
4. Veri günlüklerinizi depolamak için bir Log Analytics çalışma alanı seçin.
    Bu bölge için kullanabileceğiniz bir çalışma alanı yoksa, varsayılan bir çalışma alanı ve otomasyon hesabı oluşturmanız istenir.
5. Bilgisayarınızı eklemeye başlamak için **Etkinleştir**'i seçin.

   ![Ekleme seçeneklerini görüntüleme](./media/automation-vm-inventory/inventory-onboarding-options.png)

    Çözümün etkinleştirildiği durum çubuğunda bildirilir. Bu işlemin tamamlanması 15 dakika sürebilir. Bu süre boyunca pencerenizi kapatabilir veya açık tutabilirsiniz ve çözüm etkin olduğunda sizi uyarır. Dağıtım durumunu bildirimler bölmesinden izleyebilirsiniz.

   ![Eklemeden hemen sonra stok çözümünü görüntüleme](./media/automation-vm-inventory/inventory-onboarded.png)

Dağıtım tamamlandığında durum çubuğu kaybolur. Sistem stok verilerini toplamaktadır ve veriler henüz görünmeyebilir. Tam bir veri toplama işlemi 24 saat sürebilir.

## <a name="configure-your-inventory-settings"></a>Stok ayarlarınızı yapılandırma

Varsayılan olarak yazılım, Windows hizmetleri ve Linux daemon'ları toplama işlemi için yapılandırılmıştır. Windows kayıt defteri ve dosya stoğunu toplamak için stok toplama ayarlarını yapılandırın.

1. İçinde **stok** görünümü, select **ayarlarını Düzenle** pencerenin üstündeki düğmesi.
2. Yeni bir toplama ayarı eklemek için **Windows Kayıt Defteri**, **Windows Dosyaları** ve **Linux Dosyaları** sekmelerini seçerek eklemek istediğiniz ayar kategorisine gidin.
3. Uygun kategoriyi seçin ve tıklatın **Ekle** pencerenin üstündeki.

Aşağıdaki tablolar için çeşitli kategorileri yapılandırılabilir her bir özellik hakkında bilgi sağlar.

### <a name="windows-registry"></a>Windows Kayıt Defteri

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Windows Kayıt Defteri Anahtarı   | Dosyanın denetleneceği yol. Örneğin: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup"      |

### <a name="windows-files"></a>Windows Dosyaları

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu girin     | Dosyanın denetleneceği yol. Örneğin: “c:\temp\myfile.txt”

### <a name="linux-files"></a>Linux Dosyaları

|Özellik  |Açıklama  |
|---------|---------|
|Etkin     | Ayarın uygulanmış olup olmadığını belirler        |
|Öğe Adı     | İzlenecek dosyanın kolay adı        |
|Grup     | Dosyaları mantıksal bir biçimde gruplandırmaya yönelik grup adı        |
|Yolu Gir     | Dosyanın denetleneceği yol. Örneğin: “/etc/*.conf”       |
|Yol Türü     | İzlenecek öğenin türü için olası değerler: Dosya ve Dizin        |
|Özyineleme     | İzlenecek öğe aranırken özyinelemenin kullanılıp kullanılmadığını belirler.        |
|Sudo Kullan     | Bu ayar, öğe denetlenirken sudonun kullanılıp kullanılmadığını belirler.         |
|Bağlantılar     | Bu ayar, dizinleri dolaşırken sembolik bağlantıların nasıl ele alındığını belirler.<br> **Yoksay** - Sembolik bağlantıları yoksayar ve başvurulan dosyaları veya dizinleri içermez<br>**İzle** - Özyineleme sırasında sembolik bağlantıları izler ve başvurulan dosyaları veya dizinleri de içerir<br>**Yönet** - Sembolik bağlantıları izler ve döndürülen içeriğin işlenmesinde değişiklik yapılmasına olanak sağlar      |

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
* Windows ve paket güncelleştirmesi, sanal makinelerde yönetme hakkında bilgi edinmek için [Azure Güncelleştirme Yönetim çözümüne](../operations-management-suite/oms-solution-update-management.md).
