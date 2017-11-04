---
title: "Azure sanal makineler hakkında veri toplamak | Microsoft Docs"
description: "OMS Aracısı VM uzantısı etkinleştirmek ve Azure Vm'leriniz günlük analizi ile veri toplamayı etkinleştirmek öğrenin."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 2dec744b512a86a30cec1f334e265572fa7acc3e
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-about-azure-virtual-machines"></a>Veri toplama hakkında Azure sanal makineler
[Azure günlük analizi](log-analytics-overview.md) veri ayrıntılı bir analiz ve bağıntı için tek bir depoda doğrudan Azure sanal makinelerinizi ve diğer kaynakları ortamınızdaki toplayabilirsiniz.  Bu hızlı başlangıç yapılandırmak ve veri Azure Linux ya da Windows VM'ler ile birkaç kolay adımı gösterilmiştir.  
 
Bu Hızlı Başlangıç, mevcut bir Azure sanal makine olduğunu varsayar. Değil yapabiliyorsanız [bir Windows VM oluşturma](../virtual-machines/windows/quick-create-portal.md) veya [bir Linux VM oluşturma](../virtual-machines/linux/quick-create-cli.md) bizim VM quickstarts aşağıdaki.

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.<br> ![Azure portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br>  
2. Tıklatın **oluşturma**ve ardından aşağıdaki öğeler için seçenekleri seçin:

  * İçin yeni bir ad **OMS çalışma**, gibi *DefaultLAWorkspace*. 
  * Varsayılan seçili abonelik uygun değilse açılan listeden bağlanacak bir **Abonelik** seçin.
  * İçin **kaynak grubu**, bir veya daha fazla Azure sanal makineleri varolan bir kaynak grubu seçin.  
  * Seçin **konumu** Vm'leriniz dağıtılır.  Ek bilgi için bkz: [günlük analizi bulunan bölgelere](https://azure.microsoft.com/regions/services/).
  * Üç farklı seçebilirsiniz **fiyatlandırma katmanlarına** günlük analizi, ancak bulacağınızı seçmek için bu hızlı başlangıç için **ücretsiz** katmanı.  Belirli katmanları hakkında ek bilgi için bkz: [günlük analizi fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Üzerinde gerekli bilgileri girdikten sonra **OMS çalışma** bölmesinde tıklatın **Tamam**.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz. 

## <a name="enable-the-log-analytics-vm-extension"></a>Log Analytics VM uzantısı etkinleştir
Windows ve Linux zaten Azure'da dağıtılan sanal makineleri için günlük analizi VM uzantısı ile günlük analizi Aracısı'nı yükleyin.  Uzantısını kullanarak yükleme işlemini basitleştirir ve belirttiğiniz için günlük analizi çalışma alanına veri göndermek için aracı otomatik olarak yapılandırır. Aracı ayrıca otomatik olarak en son özellikleri ve düzeltmeleri sahip olduktan yükseltilir.

Üstte yer alan günlük analizi kaynak sayfanızın yükseltmek için davet Portalı'nda başlığı fark edebilirsiniz.  Bu hızlı başlangıç amaçları doğrultusunda yükseltme gerekli değildir.<br>

![Azure Portalı'ndaki bildirim günlük analizi yükseltme](media/log-analytics-quick-collect-azurevm/log-analytics-portal-upgradebanner.png).    
1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
2. Günlük analizi çalışma alanları, listeden seçin *DefaultLAWorkspace* daha önce oluşturduğunuz.
3. Çalışma alanı veri kaynakları altında sol menüsünde tıklatın **sanal makineleri**.  
4. Listesinde **sanal makineleri**, aracıyı yüklemek istediğiniz sanal makineyi seçin. Dikkat **OMS bağlantı durumu** VM, olup olmadığını gösterir **bağlı**.
5. Sanal makineniz için Ayrıntılar seçin **Bağlan**. Aracıyı otomatik olarak yüklenir ve günlük analizi çalışma alanınız için yapılandırılır. Bu işlem hangi sırada birkaç dakika sürer **durum** olan **bağlanıyor**.
6. Yükleyip aracısına bağlanmak sonra **OMS bağlantı durumu** güncelleştirilir **bu çalışma**.

## <a name="collect-event-and-performance-data"></a>Olay ve performans verileri toplama
Günlük analizi Windows olay günlüklerini veya Linux Syslog ve uzun vadeli çözümleme ve raporlama için belirttiğiniz performans sayaçları olaylarını toplamak ve belirli bir koşula algılandığında adımları uygulayın.  Windows Sistem günlüğüne ve Linux Syslog ve birkaç ortak performans sayaçları olaylar toplama başlamak yapılandırmak için aşağıdaki adımları izleyin.  

### <a name="data-collection-from-windows-vm"></a>Windows VM veri koleksiyonunu
1. Seçin **Gelişmiş ayarları**.<br> ![Günlük analizi Gelişmiş ayarları](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br> 
3. Seçin **veri**ve ardından **Windows olay günlüklerini**.  
4. Bir olay günlüğü, günlük adını yazarak ekleyin.  Tür **sistem** ve artı işaretini tıklatın  **+** .  
5. Tabloda önem derecelerine denetleyin **hata** ve **uyarı**.   
6. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.
7. Seçin **Windows performans verilerini** bir Windows bilgisayarda performans sayacı toplamayı etkinleştirmek için. 
8. Windows performans sayaçlarını yeni bir günlük analizi çalışma alanı için ilk yapılandırırken hızla birçok ortak sayaçları oluşturma seçeneği verilir. Bunlar, her yanındaki onay kutusunu ile listelenir.<br> ![Seçili varsayılan Windows performans sayaçlarını](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Tıklatın **Seçili performans sayaçlarını Ekle**.  Bunlar eklenir ve on ikinci koleksiyon örnekleme aralığı ile hazır.  
9. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.

### <a name="data-collection-from-linux-vm"></a>Linux VM'den veri toplama

1. Seçin **Syslog**.  
2. Bir olay günlüğü, günlük adını yazarak ekleyin.  Tür **Syslog** ve artı işaretini tıklatın  **+** .  
3. Tabloda önem derecelerine işaretini **bilgisi**, **bildirimi** ve **hata ayıklama**. 
4. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.
5. Seçin **Linux performans verilerini** bir Windows bilgisayarda performans sayacı toplamayı etkinleştirmek için. 
6. Linux performans sayaçları yeni bir günlük analizi çalışma alanı için ilk yapılandırırken hızla birçok ortak sayaçları oluşturma seçeneği verilir. Bunlar, her yanındaki onay kutusunu ile listelenir.<br> ![Seçili varsayılan Windows performans sayaçlarını](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br> Tıklatın **Seçili performans sayaçlarını Ekle**.  Bunlar eklenir ve on ikinci koleksiyon örnekleme aralığı ile hazır.  
7. Tıklatın **kaydetmek** yapılandırmayı kaydetmek için sayfanın üstündeki.

## <a name="view-data-collected"></a>Toplanan görünüm verileri
Veri toplama etkinleştirdiyseniz, VM'ler hedef bazı verileri görmek için basit günlük arama örneği çalıştırmak olanak sağlar.  

1. Azure portalında günlük Analizi'ne gidin ve daha önce oluşturduğunuz çalışma alanını seçin.
2. Tıklatın **günlük arama** döşeme ve günlük arama bölmesinde Sorgu alan türü `Type=Perf` ve ardından isabet girin veya sorgu alanının sağındaki arama düğmesini tıklatın.<br> ![Günlük analizi arama sorgusu örneğinde oturum](./media/log-analytics-quick-collect-azurevm/log-analytics-portal-queryexample.png)<br> Örneğin, aşağıdaki resimde sorguda 78,000 performans kayıtları döndürdü.  Sonuçlarınızı önemli ölçüde daha az olur.<br> ![Günlük analizi arama sonucu oturum](media/log-analytics-quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olduğunda, günlük analizi çalışma alanı silin. Bunu yapmak için daha önce ve kaynak sayfası tıklatıldığında oluşturduğunuz günlük analizi çalışma alanı seçin **silmek**.<br> ![Günlük analizi kaynağı silme](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık, işletimsel topluyorsunuz ve performans verilerini, Windows veya Linux sanal makinelerden yapabilecekleriniz kolayca keşfetme, çözümleme ve için topladığınız veri alma eylemini başlamak *ücretsiz*.  

Görüntüleme ve verileri çözümleme öğrenmek için Öğreticisine devam edin.   

> [!div class="nextstepaction"]
> [Görüntülemek veya günlük analizi veri çözümleme](log-analytics-tutorial-viewdata.md)
