---
title: System Center Operations Manager ile hizmet Haritası tümleştirme | Microsoft Docs
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede dağıtılmış uygulama diyagramları Operations Manager'da otomatik olarak oluşturmak için hizmet eşlemesi kullanarak anlatılmaktadır.
services: monitoring
documentationcenter: ''
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: 6fbc49584b040f952fdff147207864d2d1f6377e
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887877"
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>System Center Operations Manager ile hizmet Haritası tümleştirme
  > [!NOTE]
  > Bu özellik genel önizlemede değil.
  > 
  
Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Hizmet eşlemesi, bunları, kritik hizmet sunmak birbirine bağlı sistemler olarak düşünme yolu sunucularınızı görüntülemenizi sağlar. Hizmet eşlemesi herhangi TCP bağlı mimarisi yanı sıra bir aracının yüklenmesi gereken herhangi bir yapılandırma boyunca sunucuları, işlemleri ve bağlantı noktaları arasındaki bağlantıları gösterir. Daha fazla bilgi için bkz: [hizmet Haritası belgelerine]( monitoring-service-map.md).

İle tümleştirme arasında hizmet Haritası ve System Center Operations Manager, Operations Manager'da, hizmet eşlemesinde dinamik bağımlılık eşlemeleri temel alan dağıtılmış uygulama diyagramları otomatik olarak oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Bir Operations Manager yönetim grubu (2012 R2 veya sonrası) sunucular kümesi yönetme.
* Günlük analizi çalışma alanı etkin hizmet Haritası çözümle.
* Operations Manager ve hizmet eşlemesi için verileri gönderilirken tarafından yönetilen sunucular kümesi (en az bir tane). Windows ve Linux sunucuları desteklenir.
* Günlük analizi çalışma alanı ile ilişkili Azure abonelik erişimi olan bir hizmet sorumlusu. Daha fazla bilgi için Git [bir hizmet sorumlusu oluşturma](#creating-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Hizmet eşlemesi Yönetimi paketini yükleyin
Operations Manager ile hizmet Haritası arasında tümleştirme Microsoft.SystemCenter.ServiceMap Yönetim Paketi (Microsoft.SystemCenter.ServiceMap.mpb) alarak etkinleştirin. Yönetim Paketi karşıdan yükleyebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=55763). Paket aşağıdaki yönetim paketlerini içerir:
* Microsoft hizmet eşlemesi uygulama görünümleri
* Microsoft System Center hizmet Haritası iç
* Microsoft System Center hizmeti harita geçersiz kılmaları
* Microsoft System Center hizmet eşlemesi

## <a name="configure-the-service-map-integration"></a>Hizmet eşlemesi tümleştirmesini yapılandırma
Hizmet eşlemesi Yönetim Paketi, yeni bir düğüm yükledikten sonra **hizmet Haritası**, altında görüntülenen **Operations Management Suite** içinde **Yönetim** bölmesi. 

Hizmet eşlemesi tümleştirmesini yapılandırmak için aşağıdakileri yapın:

1. Yapılandırma Sihirbazı'nı açmak için **hizmet eşlemesi genel bakış** bölmesinde tıklatın **çalışma Ekle**.  

    ![Hizmet eşlemesi genel bakış bölmesinde](media/monitoring-service-map/scom-configuration.png)

2. İçinde **bağlantı yapılandırması** penceresinde, Kiracı adı veya kimliği, uygulama kimliği (kullanıcı adı veya istemci kimliği olarak da bilinir) ve hizmet sorumlusu parolasını girin ve ardından **sonraki**. Daha fazla bilgi için Git [bir hizmet sorumlusu oluşturma](#creating-a-service-principal).

    ![Bağlantı Yapılandırması penceresi](media/monitoring-service-map/scom-config-spn.png)

3. İçinde **abonelik seçimi** penceresinde, Azure abonelik, Azure kaynak grubu (günlük analizi çalışma alanı içeren bir) ve günlük analizi çalışma alanı seçin ve ardından **sonraki**.

    ![Operations Manager yapılandırma çalışma alanı](media/monitoring-service-map/scom-config-workspace.png)

4. İçinde **makine Grup Seçimi** penceresinde, Operations Manager'a eşitlemek istediğinizden hangi hizmet eşlemesi makine grupları seçin. Tıklatın **Ekle/Kaldır makine grupları**, gruplar listesinden seçim **kullanılabilir makine grupları**, tıklatıp **Ekle**.  Grupları seçmek bittiğinde tıklatın **Tamam** tamamlamak için.
    
    ![Operations Manager yapılandırma makine grupları](media/monitoring-service-map/scom-config-machine-groups.png)
    
5. İçinde **sunucu seçimi** penceresinde, yapılandırdığınız hizmet eşlemesi sunucuları grubu Operations Manager ile hizmet Haritası arasında eşitlemek istediğiniz sunucuları ile. Tıklatın **sunucuları Ekle/Kaldır**.   
    
    Bir sunucu için bir dağıtılmış uygulama diyagram derleme tümleştirmesi için sunucu olması gerekir:

    * Operations Manager tarafından yönetilen
    * Hizmet eşlemesi tarafından yönetilen
    * Hizmet eşlemesi sunucuları grubu listelenen

    ![Operations Manager Yapılandırma grubu](media/monitoring-service-map/scom-config-group.png)

6. İsteğe bağlı: Günlük analizi ile iletişim kurmak için yönetim sunucusu kaynak havuzu seçin ve ardından **çalışma alanı Ekle**.

    ![Operations Manager yapılandırma kaynak havuzu](media/monitoring-service-map/scom-config-pool.png)

    Yapılandırmak ve günlük analizi çalışma alanı kaydetmek için bir dakika sürebilir. Bunu yapılandırıldıktan sonra Operations Manager ilk hizmet Haritası eşitleme başlatır.

    ![Operations Manager yapılandırma kaynak havuzu](media/monitoring-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>İzleyici hizmet eşlemesi
Günlük analizi çalışma alanı bağlandıktan sonra yeni bir klasör, hizmet Haritası görüntülenen **izleme** Operations Manager Konsolu bölmesi.

![Operations Manager izleme bölmesi](media/monitoring-service-map/scom-monitoring.png)

Hizmet eşlemesi klasörü dört düğüm vardır:
* **Etkin uyarılar**: tüm etkin uyarıları Operations Manager ve hizmet eşlemesi arasındaki iletişimle ilgili listeler.  Bu uyarılar olmayan Not Operations Manager'a eşitlenmiş günlük analizi uyarır. 

* **Sunucuları**: yapılandırılmış izlenen sunucuları listeler hizmet eşlemesinden eşitlenecek.

    ![Operations Manager izleme sunucuları bölmesi](media/monitoring-service-map/scom-monitoring-servers.png)

* **Makine grubu bağımlılık görünümleri**: hizmet eşlemesinden eşitlenen tüm makine grupları listeler. Dağıtılmış uygulama diyagramı görüntülemek için herhangi bir grubu tıklatabilirsiniz.

    ![Operations Manager dağıtılmış uygulama diyagramı](media/monitoring-service-map/scom-group-dad.png)

* **Server bağımlılık görünümleri**: hizmet eşlemesinden eşitlenen tüm sunucuları listeler. Dağıtılmış uygulama diyagramı görüntülemek için herhangi bir sunucu tıklatabilirsiniz.

    ![Operations Manager dağıtılmış uygulama diyagramı](media/monitoring-service-map/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Düzenleme veya çalışma alanını silme
Düzenleme veya yapılandırılmış çalışma alanını kullanarak silme **hizmet eşlemesi genel bakış** bölmesinde (**Yönetim** bölmesinde > **Operations Management Suite**  >  **Hizmet eşlemesi**). Şu an için yalnızca bir günlük analizi çalışma alanı yapılandırabilirsiniz.

![Operations Manager çalışma alanını Düzenle bölmesi](media/monitoring-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Kuralları ve geçersiz kılmalar yapılandırın
Bir kural _Microsoft.SystemCenter.ServiceMapImport.Rule_, düzenli olarak bilgi hizmeti eşlemesinden getirmek için oluşturulur. Eşitleme zamanlamalarını değiştirmek için geçersiz kılmaları kural yapılandırabilirsiniz (**yazma** bölmesinde > **kuralları** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) .

![Operations Manager geçersiz kılan özellikler penceresi](media/monitoring-service-map/scom-overrides.png)

* **Etkin**: etkinleştirmek veya Otomatik Güncelleştirmeler devre dışı. 
* **IntervalMinutes**: güncelleştirmeler arasındaki süre sıfırlayın. Varsayılan zaman aralığı bir saattir. Sunucu eşlemeleri daha sık eşitlemek isterseniz, değeri değiştirebilir.
* **TimeoutSeconds**: İstek zaman aşımına uğramadan önce geçen süreyi sıfırlayın. 
* **TimeWindowMinutes**: veri sorgulama için zaman penceresi sıfırlayın. Varsayılan değer 60 dakikalık ' dir. Hizmet eşlemesi tarafından izin verilen en büyük değer 60 dakikadır.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Geçerli tasarım, aşağıdaki sorunlar ve sınırlamalar sunar:
* Yalnızca tek bir günlük analizi çalışma alanına bağlanabilir.
* Hizmet eşlemesi sunucuları grubu için el ile sunucuları ekleyebilseniz **yazma** bölmesinde, bu sunucular için eşlemeleri olmayan eşitlenen hemen.  Bunlar sonraki eşitleme döngüsü sırasında hizmet eşlemesinden eşitlenir.
* Dağıtılmış uygulama Yönetim Paketi tarafından oluşturulan diyagramları için herhangi bir değişiklik yaparsanız, bu değişiklikleri olasılıkla hizmet eşlemesi ile sonraki eşitleme üzerinde üzerine yazılır.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma
Bir hizmet sorumlusu oluşturma hakkında daha fazla resmi Azure belgeler için bkz:
* [PowerShell kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Azure CLI kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Geri Bildirim
Tüm geri bildirim bize hizmet haritası veya bu belge hakkında var? Ziyaret bizim [kullanıcı sesi sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), buradan özellikleri önermek veya varolan önerilere oy verin.
