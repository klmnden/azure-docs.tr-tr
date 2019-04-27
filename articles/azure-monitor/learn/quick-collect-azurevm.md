---
title: Azure Sanal Makineleri hakkında veri toplama | Microsoft Docs
description: Log Analytics Aracısı VM Uzantısını etkinleştirmeyi ve Log Analytics ile Azure VM’lerinizden veri toplamayı etkinleştirmeyi öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: e135c7fa9907d218ed32b6bdb0fd60da0ecf1851
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540775"
---
# <a name="collect-data-about-azure-virtual-machines"></a>Azure Sanal Makineleri hakkında veri toplama
[Azure Log Analytics](../../azure-monitor/log-query/log-query-overview.md), doğrudan Azure sanal makinelerinizden ve ortamınızdaki diğer kaynaklardan verileri ayrıntılı analiz ve bağıntı için tek bir depoda toplayabilir.  Bu hızlı başlangıçta birkaç kolay adımda Azure Linux veya Windows VM’lerinizi nasıl yapılandırabileceğiniz ve veri toplayabileceğiniz gösterilmektedir.  
 
Bu hızlı başlangıçta mevcut bir Azure sanal makinenizin olduğu varsayılmaktadır. Yoksa VM hızlı başlangıçlarımızı izleyerek bir [Windows VM](../../virtual-machines/windows/quick-create-portal.md) veya bir [Linux VM](../../virtual-machines/linux/quick-create-cli.md) oluşturabilirsiniz.

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.

    ![Azure portal](media/quick-collect-azurevm/azure-portal-01.png)<br>  

2. **Oluştur**’a tıklayın, ardından şu öğeler için seçim yapın:

   * Yeni **Log Analytics çalışma alanı** için *DefaultLAWorkspace* gibi bir ad sağlayın. OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır.  
   * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
   * **Kaynak Grubu** için, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin.  Ek bilgi için bkz. [Log Analytics’in sunulduğu bölgeler](https://azure.microsoft.com/regions/services/).
   * 2 Nisan 2018 tarihinden sonra oluşturulan yeni bir abonelikte çalışma alanı oluşturuyorsanız bu, otomatik olarak *GB başına* fiyatlandırma planını kullanır ve fiyatlandırma katmanı seçme seçeneği kullanılamaz.  2 Nisan’dan önce oluşturulmuş mevcut bir abonelik için veya mevcut bir EA kaydına bağlı aboneliğe yönelik çalışma alanı oluşturuyorsanız, tercih ettiğiniz fiyatlandırma katmanını seçin.  Katmanlar hakkında daha fazla bilgi için bkz. [Log Analytics Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).
  
        ![Log Analytics kaynak dikey penceresi oluşturma](media/quick-collect-azurevm/create-loganalytics-workspace-02.png) 

3. **Log Analytics çalışma alanı** bölmesinde gerekli bilgileri girdikten sonra **Tamam**’a tıklayın.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="enable-the-log-analytics-vm-extension"></a>Log Analytics VM Uzantısını etkinleştirme

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Zaten Azure’da dağıtılan Windows ve Linux sanal makineler için, Log Analytics aracısını Log Analytics VM Uzantısı ile yüklersiniz. Uzantıyı kullanmak yükleme işlemini kolaylaştırır ve aracıyı belirttiğiniz Log Analytics çalışma alanına veri göndermek üzere otomatik olarak yapılandırır. Ayrıca aracı otomatik olarak yükseltilerek her zaman en yeni özellik ve düzeltmelere sahip olmanız sağlanır. Devam etmeden önce VM çalışıyor olun Aksi takdirde işlemi başarıyla tamamlamak başarısız olur.  

>[!NOTE]
>Linux için Log Analytics aracısı birden fazla Log Analytics çalışma alanına raporlamak için yapılandırılamaz. 

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde, daha önceden oluşturduğunuz *DefaultLAWorkspace* çalışma alanını seçin.
3. Sol menüde, Çalışma Alanı Veri Kaynakları altında, **Sanal makineler**’e tıklayın.  
4. **Sanal makine** listesinden aracıyı yüklemek istediğiniz bir sanal makine seçin. VM için **Log Analytics bağlantı durumunun** **Bağlı değil** olduğuna dikkat edin.
5. Sanal makinenizin ayrıntılarında, **Bağlan**’ı seçin. Aracı otomatik olarak yüklenir ve Log Analytics çalışma alanınız için yapılandırılır. Bu işlem birkaç dakika sürer. Bu süre boyunca **Durum** **Bağlanıyor** olarak görünür.
6. Aracıyı yükleyip bağlandıktan sonra, **Log Analytics bağlantı durumu** **Bu çalışma alanı** olarak güncelleştirilir.

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama
Log Analytics uzun süreli analiz ve raporlama için belirttiğiniz Windows olay günlükleri veya Linux Syslog ve performans sayaçlarından olayları toplayarak belirli bir koşul algılandığında işlem yapabilir.  Windows sistem günlüğü ve Linux Syslog’dan olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

### <a name="data-collection-from-windows-vm"></a>Windows VM’den veri toplama
1. **Gelişmiş ayarlar**’ı seçin.

    ![Log Analytics Gelişmiş Ayarlar](media/quick-collect-azurevm/log-analytics-advanced-settings-01.png)

3. **Veri**’yi seçin ve ardından **Windows Olay Günlükleri**’ni seçin.  
4. Bir olay günlüğü eklemek için günlüğün adını yazın.  **Sistem** yazıp artı işaretine **+** tıklayın.  
5. Tabloda, **Hata** ve **Uyarı** önem derecelerini işaretleyin.   
6. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.
7. Bir Windows bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Windows Performans Verileri**’ni seçin. 
8. Yeni bir Log Analytics çalışma alanı için Windows Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.

    ![Varsayılan Windows performans sayaçları seçildi](media/quick-collect-azurevm/windows-perfcounters-default.png)

    **Seçili performans sayaçlarını ekle**’ye tıklayın.  Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.
  
9. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.

### <a name="data-collection-from-linux-vm"></a>Linux VM’den veri toplama

1. **Syslog**’u seçin.  
2. Bir olay günlüğü eklemek için günlüğün adını yazın.  **Syslog** yazıp artı işaretine **+** tıklayın.  
3. Tabloda, **Bilgiler**, **Bildirim** ve **Hata Ayıklama** önem derecelerinin işaretini kaldırın. 
4. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.
5. Bir Linux bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için **Linux Performans Verileri**’ni seçin. 
6. Yeni bir Log Analytics çalışma alanı için Linux Performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturma seçenekleri sunulur. Her birinin yanında bir onay kutusu görüntülenir.

    ![Varsayılan Windows performans sayaçları seçildi](media/quick-collect-azurevm/linux-perfcounters-default.png)

    **Seçili performans sayaçlarını ekle**’ye tıklayın.  Eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.  

7. Yapılandırmayı kaydetmek için sayfanın en üstünde yer alan **Kaydet**’e tıklayın.

## <a name="view-data-collected"></a>Toplanan verileri görüntüleyin
Veri toplamayı etkinleştirdiyseniz, şimdi hedef VM’lerden verileri görmek için basit bir günlük araması örneği çalıştıralım.  

1. Azure portalında, Log Analytics’e gidip önceden oluşturduğunuz çalışma alanını seçin.
2. **Günlük Araması** kutucuğuna tıklayın ve Günlük Araması bölmesinde, sorgu alanında `Perf` yazıp Enter tuşuna basın veya sorgu alanının sağındaki arama düğmesine tıklayın.

    ![Log Analytics günlük araması sorgu örneği](./media/quick-collect-azurevm/log-analytics-portal-perf-query.png) 

Örneğin, aşağıdaki resimdeki sorgu 735 performans kaydı döndürdü.  Sonuçlarınız önemli ölçüde daha az olacaktır. 

![Log Analytics günlük araması sonucu](media/quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında, Log Analytics çalışma alanını silin. Bunu yapmak için, önceden oluşturduğunuz Log Analytics çalışma alanını seçin ve kaynak sayfasında **Sil**’e tıklayın.


![Log Analytics kaynağını silme](media/quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Windows veya Linux sanal makinelerinizden işletimsel verileri ve performans verilerini topluyorsunuz ve *ücretsiz* olarak topladığınız verileri kolayca keşfetmeye, analiz etmeye ve verilerde işlem gerçekleştirmeye başlayabilirsiniz.  

Verileri görüntüleme ve analiz etmeyi öğrenmek için, öğreticiye devam edin.   

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](../../azure-monitor/learn/tutorial-viewdata.md)
