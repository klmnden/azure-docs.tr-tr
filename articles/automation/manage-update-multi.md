---
title: "Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme | Microsoft Docs"
description: "Güncelleştirmeleri yönetmek için Azure sanal makineleri ekleyin."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: 89bf87f27fdf276068cba261fc6ae1660307e0b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme

Güncelleştirme yönetimi, Azure sanal makineleriniz için güncelleştirme ve yamaları yönetmenize olanak tanır.
[Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan sanal makineleri kolayca ekleyebilir, kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve dağıtım sonuçlarını inceleyerek güncelleştirmelerin Güncelleştirme yönetimi etkin olan tüm sanal makinelere başarıyla uygulandığını doğrulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzdaki adımları tamamlamak için şunlar gerekir:

* Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
* Azure Resource Manager sanal makinesi (Klasik değil). VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

## <a name="enable-update-management-for-azure-virtual-machines"></a>Azure sanal makineleri için güncelleştirme yönetimini etkinleştirme

1. Azure portalında Otomasyon hesabınızı açın.
2. Ekranın sol tarafından **Güncelleştirme yönetimi**’ni seçin.
3. Ekranın üstündeki **Azure VM Ekle**’ye tıklayın.
    ![VM Ekleme](./media/manage-update-multi/update-onboard-vm.png)
4. Eklemek için bir sanal makine seçin. **Güncelleştirme Yönetimini Etkinleştirme** ekranı görüntülenir.
5. **Etkinleştir**’e tıklayın.

   ![Güncelleştirme yönetimini etkinleştirme](./media/manage-update-multi/update-enable.png)

Güncelleştirme yönetimi, sanal makineniz için etkinleştirilir.

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın.
Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak bir veya daha fazla sanal makine için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdakileri belirtin:

* **Ad** - Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad belirtin.
* **İşletim Sistemi Türü** - Windows veya Linux'u seçin.
* **Güncelleştirilecek bilgisayarlar** - Güncelleştirmek istediğiniz sanal makineleri seçin.

  ![Güncelleştirilecek sanal makineleri seçme](./media/manage-update-multi/update-select-computers.png)

* **Güncelleştirme sınıflandırması** - Güncelleştirme dağıtımının dağıtıma dahil edeceği yazılım türlerini seçin. Sınıflandırma türleri şunlardır:
  * Kritik güncelleştirmeler
  * Güvenlik güncelleştirmeleri
  * Güncelleştirme paketleri
  * Özellik paketleri
  * Hizmet paketleri
  * Tanım güncelleştirmeleri
  * Araçlar
  * Güncelleştirmeler
* **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilir ya da farklı bir zaman belirtebilirsiniz.
   Ayrıca, dağıtımın bir kez gerçekleşeceğini belirtebilir veya yinelenen bir zamanlama ayarlayabilirsiniz. Yinelenen bir zamanlama ayarlamak için Yineleme altında Yinelenen seçeneğine tıklayın.

   ![Güncelleştirme Zamanlama Ayarları ekranı](./media/manage-update-multi/update-set-schedule.png)

* **Bakım penceresi (dakika)** - Güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin.  Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanmış** tablosunda yeni oluşturduğunuz dağıtım zamanlamasının gösterildiğine dikkat edin.

> [!WARNING]
> Yeniden başlatma gerektiren güncelleştirmeler için sanal makine otomatik olarak yeniden başlatılır.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başlatıldıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.

![Dağıtım durumunu güncelleştirme ](./media/manage-update-multi/update-view-results.png)

Tamamlanan güncelleştirme dağıtımına tıklayarak bu güncelleştirme dağıtımının panosunu görebilirsiniz.

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve sanal makinedeki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* Denenmedi - tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* Başarılı - güncelleştirme başarılı oldu
* Başarısız - güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’e tıklayın.

Hedef sanal makinede güncelleştirme dağıtımını yönetmekten sorumlu runbook’un iş akışını görmek için **Çıktı** kutucuğuna tıklayın.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* Güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [Güncelleştirme Yönetimi](../operations-management-suite/oms-solution-update-management.md).