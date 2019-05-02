---
title: Azure Log Analytics aracısını karma Windows bilgisayarlar için yapılandırma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure dışında çalıştıran Windows bilgisayarlar için Log Analytics aracısını dağıtmayı ve Log Analytics ile veri toplamayı etkinleştirmeyi öğreneceksiniz.
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
ms.date: 04/09/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 99be0cee9c939ed200bd74c94e88c3fcd989e25b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722886"
---
# <a name="configure-the-log-analytics-agent-for-windows-computers-in-a-hybrid-environment"></a>Karma bir ortamda, Windows bilgisayarlar için Log Analytics aracısını yapılandırma
[Azure Log Analytics](../../azure-monitor/platform/agent-windows.md) verileri doğrudan fiziksel veya sanal Windows bilgisayarlarınızdan ayrıntılı analiz ve bağıntı için tek bir depoda toplayabilir. Bir veri merkezine ya da başka bir bulut ortamında, log Analytics'e veri toplayabilirsiniz. Bu hızlı başlangıçta birkaç kolay adımda Windows bilgisayarınızı nasıl yapılandırabileceğiniz ve veri toplayabileceğiniz gösterilmektedir.  Azure Windows sanal makineleri hakkında daha fazla bilgi için bkz: [Azure sanal makineleri hakkında veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md).  

Desteklenen yapılandırma anlamak için bkz: [desteklenen Windows işletim sistemleri](../../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems) ve [ağ güvenlik duvarı yapılandırması](../../azure-monitor/platform/log-analytics-agent.md#network-firewall-requirements).
 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
1. Azure portalda **Tüm hizmetler**’i seçin. Arama kutusuna **Log Analytics**. Siz yazarken liste, girişinize göre üzerinde. Seçin **Log Analytics**:

    ![Azure portal](media/quick-collect-windows-computer/azure-portal-01.png)
  
2. Seçin **Oluştur**ve ardından bu ayrıntıları sağlayın:

   * Yeni bir ad girin **Log Analytics çalışma alanı**. Şuna benzeyen **DefaultLAWorkspace**.
   * Seçin bir **abonelik** bağlamak için. Varsayılan bir kullanmak istediğiniz değilse, başka bir listeden seçin.
   * İçin **kaynak grubu**, bir veya daha fazla Azure sanal makinesi içeren mevcut bir kaynak grubunu seçin.  
   * VM’lerinizin dağıtıldığı **Konum**’u seçin. Bir listesine buradan ulaşabilirsiniz [Log Analytics kullanılabildiği bölgeler](https://azure.microsoft.com/regions/services/).  
   * 2 Nisan 2018'den sonra oluşturulmuş bir abonelikte çalışma alanı oluşturuyorsanız, çalışma alanı otomatik olarak kullanacak **GB başına** fiyatlandırma planı. Fiyatlandırma katmanı seçme mümkün olmayacaktır. 2 Nisan 2018'den önce oluşturulan bir aboneliği veya mevcut bir EA kaydına bağlı bir abonelikte çalışma alanı oluşturuyorsanız, kullanmak istediğiniz fiyatlandırma katmanını seçin. Bkz: [Log Analytics fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/) katmanları hakkında daha fazla bilgi için.

        ![Log Analytics kaynak oluştur](media/quick-collect-windows-computer/create-loganalytics-workspace-02.png)<br>  

3. Gerekli bilgileri girdikten sonra **Log Analytics çalışma alanı** bölmesinde **Tamam**.  

Bilgilerin doğrulanıp çalışma alanının oluşturulması sırasında altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

## <a name="get-the-workspace-id-and-key"></a>Çalışma alanı kimliği ve anahtarını alma
İzleme Aracısı Windows için Microsoft yüklemeden önce kimliği ve anahtarına ihtiyacınız Log Analytics çalışma alanınız için. Kurulum Sihirbazı'nı düzgün şekilde yapılandırın ve Log Analytics ile iletişim kurmasını sağlamak için bu bilgiler gerekiyor.  

1. Azure portalının sol üst köşedeki seçin **tüm hizmetleri**. Arama kutusuna **Log Analytics**. Siz yazarken liste, girişinize göre üzerinde. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde, daha önce oluşturduğunuz çalışma alanını seçin. (Bunu adlı **DefaultLAWorkspace**.)
3. Seçin **Gelişmiş ayarlar**:

    ![Log Analytics Gelişmiş ayarlar](media/quick-collect-windows-computer/log-analytics-advanced-settings-01.png)
  
4. **Bağlı Kaynaklar**’ı seçin ve ardından **Windows Sunucuları**’nı seçin.
5. Sağa değerleri kopyalayın **çalışma alanı kimliği** ve **birincil anahtar**. Bunları, sık kullanılan düzenleyiciye yapıştırın.

## <a name="install-the-agent-for-windows"></a>Windows için aracıyı yükleme
Aşağıdaki adımlar, yükleyin ve Log Analytics için aracıyı Azure hem de Azure Kamu'da yapılandırın. Aracıyı bilgisayarınıza yüklemek için Microsoft Monitoring Agent Kurulumu program'ı kullanacaksınız.

1. Önceki adımlar, kümesinden işleme devam **Windows sunucuları** sayfasında **Windows Agent'ı indir** indirmek istediğiniz sürümü. Windows işletim sisteminin işlemci mimarisine uygun sürümünü seçin.
2. Aracıyı bilgisayarınıza yüklemek için Kurulum'u çalıştırın.
2. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.
3. **Lisans Koşulları** sayfasında, lisansı okuyun ve sonra **Kabul Ediyorum**’u seçin.
4. **Hedef Klasör** sayfasında, varsayılan yükleme klasörünü değiştirin veya aynı şekilde bırakın ve daha sonra **İleri**’yi seçin.
5. Üzerinde **Aracı Kurulum Seçenekleri** sayfasında, aracıyı Azure Log Analytics'e bağlayın ve ardından **sonraki**.
6. Üzerinde **Azure Log Analytics** sayfasında, aşağıdaki adımları tamamlayın:
   1. Yapıştırın **çalışma alanı kimliği** ve **çalışma alanı anahtarı (birincil anahtar)** daha önce kopyaladığınız. Bilgisayarın Azure kamu'daki bir Log Analytics çalışma alanına raporlama yapması gerekiyorsa seçin **Azure ABD kamu** içinde **Azure bulut** listesi.  
   2. Bilgisayarın, Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa **Gelişmiş** seçeneğini belirleyip ara sunucunun URL’sini ve bağlantı noktası numarasını sağlayın. Ara sunucunuz kimlik doğrulaması gerektiriyorsa, proxy sunucusu ile kimlik doğrulaması için kullanıcı adını ve parolasını girin ve ardından **sonraki**.  
7. Seçin **sonraki** yapılandırma ayarları ekledikten sonra:

    ![Microsoft Monitoring Agent Kurulumu](media/quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)

8. **Yüklemeye Hazır** sayfasında, tercihlerinizi gözden geçirip **Yükle**’yi seçin.
9. Üzerinde **yapılandırma başarıyla tamamlandı** sayfasında **son**.

Kurulum ve yükleme tamamlandığında, Microsoft Monitoring Agent Denetim Masası'nda görünür. Yapılandırmanızı gözden geçirebilir ve aracının Log Analytics'e bağlandığını doğrulayabilirsiniz. Bağlandığında **Azure Log Analytics** sekmesinde aracı bu iletiyi görüntüler: **Microsoft Monitoring Agent Microsoft Log Analytics hizmetine başarıyla bağlandı.**<br><br> ![MMA bağlantı durumu](media/quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Olay ve performans verilerini toplama
Log Analytics uzun süreli analiz ve raporlama için performans sayaçları ve Windows olay günlüğü, belirttiğiniz olayları toplayabilir. Belirli bir koşul algıladığında eylem da yararlanabilirsiniz. Windows olay günlüğünden olayları toplamayı yapılandırmak ve birkaç ortak performans sayacı ile başlamak için bu adımları izleyin.  

1. Azure portalının sol alt köşedeki seçin **diğer hizmetler**. Arama kutusuna **Log Analytics**. Siz yazarken liste, girişinize göre üzerinde. **Log Analytics**’i seçin.
2. Seçin **Gelişmiş ayarlar**:

    ![Log Analytics Gelişmiş ayarlar](media/quick-collect-windows-computer/log-analytics-advanced-settings-01.png)
 
3. **Veri**’yi seçin ve ardından **Windows Olay Günlükleri**’ni seçin.  
4. Bir olay günlüğü, günlük adını girerek ekleyin. Girin **sistem**ve ardından artı işaretini seçin (**+**).  
5. Tabloda **hata** ve **uyarı** derecesidir.
6. Seçin **Kaydet** sayfanın üstünde.
7. Seçin **Windows performans sayaçları** bir Windows bilgisayarda performans sayaçlarını toplamayı etkinleştirmek için.
8. Yeni bir Log Analytics çalışma alanı için Windows performans sayaçlarını ilk kez yapılandırırken, birkaç ortak sayacı hızlı bir şekilde oluşturmak için bu seçeneği verilir. Yanında bir onay kutusu her seçeneği listelenir:

    ![Windows performans sayaçları](media/quick-collect-windows-computer/windows-perfcounters-default.png).
    
    Seçin **Seçili performans sayaçlarını Ekle**. Sayaçları eklenir ve on saniye koleksiyon örnek aralığı ile ayarlanır.

9. Seçin **Kaydet** sayfanın üstünde.

## <a name="view-collected-data"></a>Toplanan verileri görüntüleme
Veri toplama etkinleştirdikten sonra hedef bilgisayardan verileri görmek için basit bir günlük araması çalıştıralım.  

1. Azure portalında, seçilen çalışma alanı seçin **günlük araması** Döşe.  
2. Üzerinde **günlük araması** bölmesinde sorgu kutusu girin **Perf** ve ardından Enter tuşuna basın veya sorgu kutusunun sağındaki arama düğmesine seçin:
 
    ![Log Analytics günlük araması](media/quick-collect-windows-computer/log-analytics-portal-queryexample.png)

    Örneğin, bu resimdeki sorgu 735 performans kaydı döndürdü:

    ![Log Analytics günlük araması sonucu](media/quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bilgisayarınızdan aracıyı kaldırıp, bunlara artık ihtiyacınız kalmadığında Log Analytics çalışma alanını silebilirsiniz.  

Aracıyı kaldırmak için aşağıdaki adımları tamamlayın:

1. Denetim Masası'nı açın.
2. **Programlar ve Özellikler**'i açın.
3. İçinde **programlar ve Özellikler**seçin **Microsoft Monitoring Agent** seçip **kaldırma**.

Önceden oluşturduğunuz Log Analytics çalışma alanını silmek için onu seçip, kaynak sayfasında **Sil**:

![Log Analytics çalışma alanını silme](media/quick-collect-windows-computer/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Sonraki adımlar
İşletimsel toplayacağınızı ve performans verilerini Windows bilgisayarınızda kolayca başlayabilirsiniz keşfetmeye, analiz ve veriler üzerinde çalışan, toplama, için *ücretsiz*.  

Görüntülemek ve verileri çözümlemek öğrenmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [Log Analytics’te verileri görüntüleme veya analiz etme](tutorial-viewdata.md)
