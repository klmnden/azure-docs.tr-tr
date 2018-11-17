---
title: System Center Operations Manager ile hizmet eşlemesi tümleştirme | Microsoft Docs
description: Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen bir Azure çözümüdür. Bu makalede, otomatik olarak Operations Manager dağıtılmış uygulama diyagramları oluşturmak için hizmet eşlemesi kullanarak açıklanmaktadır.
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
ms.author: bwren
ms.openlocfilehash: b590a6ce6d92c78f2020d5ef7cffe6d7f277433c
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51828715"
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>System Center Operations Manager ile hizmet eşlemesi tümleştirme
  > [!NOTE]
  > Bu özellik genel Önizleme aşamasındadır.
  > 
  
Hizmet Eşlemesi, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini otomatik olarak bulur ve hizmetler arasındaki iletişimi eşler. Hizmet eşlemesi, kritik hizmetleri sunan birbirine sistemleri düşünme yolu sunucularınızın görüntülemenize olanak sağlar. Hizmet eşlemesi, tüm TCP bağlantılı mimarisi yanı sıra bir aracının yüklenmesi gereken herhangi bir yapılandırma arasında sunucuları, işlemler ve bağlantı noktaları arasındaki bağlantıları gösterir. Daha fazla bilgi için [hizmet eşleme belgeleri]( service-map.md).

Hizmet eşlemesi ile System Center Operations Manager arasındaki bu tümleştirme sayesinde, hizmet eşlemesi dinamik bağımlılık maps'a temel alan bir Operations Manager dağıtılmış uygulama diyagramları otomatik olarak oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* Bir Operations Manager yönetim grubu (2012 R2 veya üzeri) sunucu kümesini yönetiyor.
* Log Analytics çalışma alanı etkin hizmet eşlemesi çözümü ile.
* Operations Manager ve hizmet eşlemesi için veri gönderimi yönetilen sunucuları (en az bir tane) kümesi. Windows ve Linux sunucuları desteklenir.
* Log Analytics çalışma alanı ile ilişkili Azure aboneliğine erişiminiz olan bir hizmet sorumlusu. Daha fazla bilgi için Git [hizmet sorumlusu oluşturma](#create-a-service-principal).

## <a name="install-the-service-map-management-pack"></a>Hizmet eşlemesi Yönetim Paketi yükleyin
Microsoft.SystemCenter.ServiceMap Yönetim Paketi grubu (Microsoft.SystemCenter.ServiceMap.mpb) içeri aktararak, Operations Manager ve hizmet eşlemesi arasında tümleştirme sağlar. Yönetim Paketi karşıdan yükleyebileceğiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=55763). Paket, aşağıdaki yönetim paketlerini içerir:
* Microsoft hizmet eşlemesi uygulama görünümleri
* Microsoft System Center hizmet eşlemesi iç
* Microsoft System Center Service Map geçersiz kılmaları
* Microsoft System Center hizmet eşlemesi

## <a name="configure-the-service-map-integration"></a>Hizmet eşlemesi tümleştirmesini yapılandırma
Yeni bir düğüm, hizmet eşlemesi Yönetim paketini yükledikten sonra **hizmet eşlemesi**, altında görüntülenen **Operations Management Suite** içinde **Yönetim** bölmesi. 

>[!NOTE]
>[Operations Management Suite, hizmetler koleksiyonu](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand) şimdi parçası olan Log Analytics dahil, [Azure İzleyici](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md).

Hizmet eşlemesi tümleştirmesini yapılandırmak için aşağıdakileri yapın:

1. Yapılandırma Sihirbazı'nı açmak için **Service Map genel bakış** bölmesinde tıklayın **çalışma ekleme**.  

    ![Hizmet eşlemesi genel bakış bölmesi](media/service-map-scom/scom-configuration.png)

2. İçinde **bağlantı yapılandırması** penceresinde, Kiracı adı veya kimliği, uygulama kimliği (kullanıcı adı veya ClientID olarak da bilinir) ve hizmet sorumlusu parolasını girin ve ardından **sonraki**. Daha fazla bilgi için Git [hizmet sorumlusu oluşturma](#creating-a-service-principal).

    ![Bağlantı Yapılandırması penceresi](media/service-map-scom/scom-config-spn.png)

3. İçinde **abonelik seçimi** penceresinde Azure aboneliği, Azure kaynak grubu (Log Analytics çalışma alanı içeren bir) ve Log Analytics çalışma alanı seçin ve ardından **sonraki**.

    ![Operations Manager yapılandırma çalışma alanı](media/service-map-scom/scom-config-workspace.png)

4. İçinde **makine grubu seçimi** penceresinde Operations Manager'a eşitlemek istediğiniz hangi hizmet eşlemesi makine gruplarını seçin. Tıklayın **makine gruplarını Ekle/Kaldır**, grupları listesinden seçim **kullanılabilir makine grupları**, tıklatıp **Ekle**.  Grupları seçme işiniz bittiğinde, tıklayın **Tamam** tamamlanması.
    
    ![Operations Manager yapılandırma makine grupları](media/service-map-scom/scom-config-machine-groups.png)
    
5. İçinde **sunucu seçimi** penceresinde, yapılandırdığınız hizmet eşleme sunucuları grubu ile Operations Manager ve hizmet eşlemesi arasında eşitlemek istediğiniz sunucuları. Tıklayın **sunucuları ekleme/kaldırma**.   
    
    Tümleştirme bir dağıtılmış uygulama diyagramını bir sunucu için oluşturmak sunucu olması gerekir:

    * Operations Manager tarafından yönetilen
    * Hizmet eşlemesi tarafından yönetilen
    * Hizmet eşleme sunucuları grubu içinde listelenen

    ![Operations Manager Yapılandırma grubu](media/service-map-scom/scom-config-group.png)

6. İsteğe bağlı: Log Analytics ile iletişim kurmak için yönetim sunucusu kaynak havuzu seçin ve ardından **çalışma alanı Ekle**.

    ![Operations Manager yapılandırma kaynak havuzu](media/service-map-scom/scom-config-pool.png)

    Bunu yapılandırmak ve Log Analytics çalışma alanı kaydetmek için bir dakika sürebilir. Yapılandırıldıktan sonra Operations Manager ilk hizmet eşlemesi eşitleme başlatır.

    ![Operations Manager yapılandırma kaynak havuzu](media/service-map-scom/scom-config-success.png)


## <a name="monitor-service-map"></a>İzleyici hizmet eşlemesi
Log Analytics çalışma alanı bağlandıktan sonra hizmet eşlemesi, yeni bir klasör görüntülenen **izleme** Operations Manager konsolunda bölmesinin.

![Operations Manager izleme bölmesinde](media/service-map-scom/scom-monitoring.png)

Hizmet eşlemesi klasörü dört düğümünüz vardır:
* **Etkin uyarılar**: tüm etkin uyarıları Operations Manager ve hizmet eşlemesi arasındaki iletişimle ilgili listeler.  Bu uyarıları Not, Log Analytics için Operations Manager eşitlendiğinden uyarır. 

* **Sunucuları**: yapılandırılmış izlenen sunuculara listeler için hizmet eşlemesi eşitleme.

    ![Operations Manager izleme sunucuları bölmesi](media/service-map-scom/scom-monitoring-servers.png)

* **Makine grubu bağımlılık görünümleri**: hizmet eşlemesinden eşitlenen tüm makine grupları listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir grubu tıklayabilirsiniz.

    ![Operations Manager dağıtılmış uygulama diyagramını](media/service-map-scom/scom-group-dad.png)

* **Server bağımlılık görünümleri**: hizmet eşlemesinden eşitlenen tüm sunucuları listeler. Dağıtılmış uygulama diyagramını görüntülemek için herhangi bir sunucu tıklayabilirsiniz.

    ![Operations Manager dağıtılmış uygulama diyagramını](media/service-map-scom/scom-dad.png)

## <a name="edit-or-delete-the-workspace"></a>Düzenleme veya çalışma alanını silme
Düzenleyebilir veya aracılığıyla yapılandırılan çalışma alanını silmek **Service Map genel bakış** bölmesinde (**Yönetim** bölmesinde > **Operations Management Suite**  >  **Hizmet eşlemesi**).

>[!NOTE]
>[Operations Management Suite, hizmetler koleksiyonu](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/azure-monitor-rebrand.md#retirement-of-operations-management-suite-brand) şimdi parçası olan Log Analytics dahil, [Azure İzleyici](https://github.com/MicrosoftDocs/azure-docs-pr/pull/azure-monitor/overview.md).

Şu an için yalnızca bir Log Analytics çalışma alanı yapılandırabilirsiniz.

![Operations Manager çalışma alanını düzenleme bölmesi](media/service-map-scom/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Kuralları ve geçersiz kılmaları yapılandırma
Bir kural _Microsoft.SystemCenter.ServiceMapImport.Rule_, düzenli aralıklarla hizmet eşlemesinden bilgileri getirmek için oluşturulur. Eşitleme zamanlamalarını değiştirmek için kural geçersiz kılma yapılandırabilirsiniz (**yazma** bölmesinde > **kuralları** > **Microsoft.SystemCenter.ServiceMapImport.Rule**) .

![Operations Manager geçersiz kılan özellikler penceresi](media/service-map-scom/scom-overrides.png)

* **Etkin**: etkinleştirmek veya Otomatik Güncelleştirmeler devre dışı bırakın. 
* **IntervalMinutes**: güncelleştirmeler arasındaki süre sıfırlayın. Varsayılan aralığı bir saattir. Sunucu haritalar daha sık eşitleme istiyorsanız, değeri değiştirebilirsiniz.
* **TimeoutSeconds**: İstek zaman aşımına uğramadan önce sürenin uzunluğunu sıfırlayın. 
* **TimeWindowMinutes**: veri sorgulamak için zaman penceresi sıfırlayın. Varsayılan 60 dakikalık bir penceredir. Hizmet eşlemesi tarafından izin verilen en yüksek değer 60 dakikadır.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

Geçerli tasarım aşağıdaki sorunlar ve kısıtlamalar sunar:
* Yalnızca tek bir Log Analytics çalışma alanına bağlanabilir.
* Hizmet eşleme sunucuları grubu için el ile sunucu eklemesi mümkün olsa da **yazma** bölmesinde, bu sunucular için eşlemeler değil eşitlenen hemen.  Bunlar arasındaki hizmet eşlemesi sonraki eşitleme döngüsü sırasında eşitlenir.
* Yönetim Paketi tarafından oluşturulan dağıtılmış uygulama diyagramları için herhangi bir değişiklik yaparsanız, bu değişiklikleri büyük olasılıkla bir sonraki eşitlemede hizmet eşlemesi ile yazılır.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma
Hizmet sorumlusu oluşturma hakkında resmi Azure belgeleri için bkz:
* [PowerShell kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Azure CLI kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Azure portalını kullanarak bir hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Geri Bildirim
Herhangi bir Geri bildiriminiz bizim için hizmet eşlemesi ya da bu belgeleri konusunda var? Ziyaret bizim [User Voice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), buradan özellikler önerme veya mevcut önerilere oy vermek.
