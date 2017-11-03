---
title: "Veri Yönetimi ağ geçidi sorunlarını giderme | Microsoft Docs"
description: "Veri Yönetimi ağ geçidi ile ilgili sorunları gidermek için ipuçları verilmektedir."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: eee8ee3af5918ddbe7393ff2574833f798ffcb19
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi kullanımıyla ilgili sorunları giderme
Bu makalede, veri yönetimi ağ geçidi kullanarak sorunlarını giderme hakkında bilgi sağlar.

> [!NOTE]
> Bu makale, genel olarak kullanılabilir (GA) olduğu Azure Data Factory, 1 sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [tümleştirmesi çalışma zamanı veri fabrikasında sürüm 2'kendi kendini barındıran](../create-self-hosted-integration-runtime.md).

Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) ağ geçidi hakkında ayrıntılı bilgi için makalenin. Bkz: [şirket içi ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) veri ağ geçidi'ni kullanarak bir şirket içi SQL Server veritabanından Microsoft Azure Blob Depolama birimine taşıma makale kılavuz.

## <a name="failed-to-install-or-register-gateway"></a>Yüklemek veya ağ geçidini kaydetmek başarısız oldu
### <a name="1-problem"></a>1. Sorun
Yüklerken ve ağ geçidi yükleme dosyası indirilirken bir ağ geçidi özellikle kaydetme bu hata iletisini görürsünüz.

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Nedeni
Ağ geçidini Yüklemeye çalıştığınız makine bir ağ sorunu nedeniyle İndirme Merkezi'nden en son ağ geçidi yükleme dosyasını karşıdan yüklemek başarısız oldu.

#### <a name="resolution"></a>Çözüm
Ayarları bilgisayardan ağ bağlantısı engelleme olup olmadığını görmek için güvenlik duvarı proxy sunucu ayarlarını kontrol edin [İndirme Merkezinden](https://download.microsoft.com/)ve güncelleştirme ayarları buna göre.

Alternatif olarak, en son ağ geçidi'nden yükleme dosyasını karşıdan yükleyebileceğiniz [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) diğer makinelere Yükleme Merkezi'nden erişebilirsiniz. Ardından, yükleyici dosyasını ağ geçidi ana bilgisayara kopyalayın ve el ile yükleyin ve ağ geçidi'ni çalıştırın.

### <a name="2-problem"></a>2. Sorun
Tıklayarak bir ağ geçidi yüklemek girişimde bulunduğunuzda bu hatayı görmek **doğrudan bu bilgisayar Yükle** Azure portalında.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Nedeni
Bir ağ geçidi makinede zaten yüklü.

#### <a name="resolution"></a>Çözüm
Makinedeki mevcut ağ geçidi kaldırın ve tıklatın **doğrudan bu bilgisayar Yükle** yeniden bağlanın.

### <a name="3-problem"></a>3. Sorun
Yeni bir ağ geçidi kaydederken, bu hatayı görebilirsiniz.

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a>Nedeni
Aşağıdaki nedenlerden birinden dolayı bu iletiyi görebilirsiniz:

* Ağ geçidi anahtarı biçimi geçersiz.
* Ağ geçidi anahtarı geçersiz kılındı.
* Ağ geçidi anahtarı portalından yeniden.  

#### <a name="resolution"></a>Çözüm
Portal doğru ağ geçidi anahtarı kullanarak doğrulayın. Gerekirse, bir anahtarı yeniden oluşturmak ve ağ geçidini kaydetmek için kayıt anahtarını kullanın.

### <a name="4-problem"></a>4. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![İçerik veya anahtar biçimi geçersiz](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Nedeni
İçerik veya giriş ağ geçidi anahtarı biçimi doğru değil. Nedenlerden biri dolayısıyla anahtar yalnızca bir kısmını portalından kopyalandığından veya geçersiz bir anahtar kullandığınız olabilir.

#### <a name="resolution"></a>Çözüm
Portalda bir ağ geçidi anahtarı oluşturun ve tüm anahtar kopyalamak için Kopyala düğmesini kullanın. Ardından ağ geçidini kaydetmek için bu penceresine yapıştırın.

### <a name="5-problem"></a>5. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Ağ geçidi anahtarı geçersiz veya boş değil](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi anahtar yeniden oluşturulacak veya Azure portalında ağ geçidi silinmiş olabilir. Veri Yönetimi ağ geçidi Kurulum en son değilse de oluşabilir.

#### <a name="resolution"></a>Çözüm
Veri Yönetimi ağ geçidi Kurulum en son sürüm ise bulabilirsiniz en son sürümünü Microsoft denetleyin [İndirme Merkezinden](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Kurulum, geçerli / son ise ve ağ geçidi portalında hala var, Azure portalında ağ geçidi anahtarını yeniden ve tüm anahtar kopyalamak için Kopyala düğmesini kullanın ve ağ geçidini kaydetmek için bu pencerede yapıştırın. Aksi takdirde, ağ geçidini yeniden oluşturun ve baştan başlayın.

### <a name="6-problem"></a>6. Sorun
Bir ağ geçidi kaydederken aşağıdaki hata iletisini görebilirsiniz.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Ağ geçidi anahtarı geçersiz veya boş değil](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Nedeni
Bu hata, ağ geçidi silinmiş olabilir ya da ilişkili ağ geçidi anahtarı yeniden olmadığından gerçekleşebilir.

#### <a name="resolution"></a>Çözüm
Ağ geçidi sildiyseniz Portalı'ndan Ağ Geçidi yeniden oluşturmak, tıklatın **kaydetmek**portaldan anahtarı kopyalayın, yapıştırın ve ağ geçidini kaydetmeyi deneyin.

Ağ geçidi hala var, ancak kendi anahtar yeniden oluşturulacak yeni anahtarı ağ geçidini kaydetmek için kullanın. Anahtar yoksa, yeniden portalından tuşunu yeniden oluşturun.

### <a name="7-problem"></a>7. Sorun
Bir ağ geçidi kaydedilirken yolu ve parola için bir sertifika girmeniz gerekebilir.

![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi, önce diğer makinelere kaydedildi. Bir ağ geçidi ilk kaydı sırasında bir şifreleme sertifikası ağ geçidi ile ilişkilendirilmiş. Sertifika ağ geçidi tarafından otomatik olarak oluşturulan veya kullanıcı tarafından sağlanan.  Bu sertifika, veri deposu (bağlantılı hizmeti) kimlik bilgilerini şifrelemek için kullanılır.  

![Sertifika verme](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Farklı bir konak makinesi üzerinde ağ geçidi geri yüklerken, daha önce bu sertifikayla şifrelenmiş kimlik bilgileri şifresini çözmek Bu sertifika için Kayıt Sihirbazı'nı sorar.  Bu sertifika olmadan kimlik bilgilerini yeni ağ geçidi tarafından şifresi çözülemiyor ve bu yeni ağ geçidi ile ilişkili sonraki kopyalama etkinliği yürütmeleri başarısız olur.  

#### <a name="resolution"></a>Çözüm
Kimlik bilgileri sertifikası özgün ağ geçidi makineden kullanarak dışa aktardığınız varsa **verme** düğmesini **ayarları** sekmesinde Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde, sertifika kullan Burada.

Bu aşamada, ağ geçidi kurtarırken atlayamazsınız. Sertifika yoksa portaldan ağ geçidini silin ve yeni bir ağ geçidi yeniden oluşturmanız gerekir.  Ayrıca, ağ geçidi için kimlik bilgilerini yeniden girme ilişkili tüm bağlantılı Hizmetleri güncelleştirin.

### <a name="8-problem"></a>8. Sorun
Aşağıdaki hata iletisini görebilirsiniz.

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Nedeni
Bu hata, ağ geçidiniz Internet kaynakların ya da proxy'nın kimlik doğrulaması parola erişmek için bir HTTP proxy değiştirilir, ancak uygun şekilde güncelleştirilmez gerektiren bir ortamda olduğunda oluşur. ağ geçidiniz içinde.

#### <a name="resolution"></a>Çözüm
' Ndaki yönergeleri izleyin [Proxy sunucusu hususları](#proxy-server-considerations) bu bölümü makalesi ve veri yönetimi ağ geçidi Yapılandırma Yöneticisi ile proxy ayarlarını yapılandırın.

## <a name="gateway-is-online-with-limited-functionality"></a>Ağ geçidi ile sınırlı işlevsellik çevrimiçi duruma
### <a name="1-problem"></a>1. Sorun
Sınırlı işlevlerle çevrimiçi olarak ağ geçidi durumunu görebilir.

#### <a name="cause"></a>Nedeni
Ağ geçidi durumunun çevrimiçi olarak sınırlı işlevlerle aşağıdaki nedenlerden birinden dolayı görürsünüz:

* Ağ geçidi, Azure Service Bus aracılığıyla bulut hizmetine bağlanamıyor.
* Bulut hizmeti, Service Bus aracılığıyla ağ geçidine bağlanamıyor.

Ağ geçidi ile sınırlı işlevsellik çevrimiçi olduğunda, şirket içi veri depolarına bilgisayardan veya veri kopyalamak için verileri ardışık düzen oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanmak mümkün olmayabilir. Geçici bir çözüm olarak, portal, Visual Studio veya Azure PowerShell Data Factory düzenleyici kullanabilirsiniz.

#### <a name="resolution"></a>Çözüm
Bu sorun için çözüm (sınırlı işlevsellik ile çevrimiçi) ağ geçidi bulut hizmeti veya diğer bir yol için bağlantı kuramıyor üzerinde temel alır. Aşağıdaki bölümler bu çözümleri sağlar.

### <a name="2-problem"></a>2. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Gateway cannot connect to cloud service through service bus`

![Ağ geçidi bulut hizmetine bağlanamıyor](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi, Service Bus aracılığıyla bulut hizmetine bağlanamıyor.

#### <a name="resolution"></a>Çözüm
Ağ geçidi çevrimiçine almak için şu adımları izleyin:

1. IP adresi ağ geçidi makinesindeki ve kurumsal güvenlik duvarı giden kuralları izin verir. IP adresleri Windows olay günlüğünden bulabilirsiniz (kimliği 401 ==): bir yuva, erişim izinleri XX Yasak bir şekilde erişmek için bir girişimde bulunuldu. XX. XX. XX:9350.
* Ağ geçidinde proxy ayarlarını yapılandırın. Bkz: [Proxy sunucusu hususları](#proxy-server-considerations) ayrıntıları bölümü.
* Giden bağlantı noktası 5671 ve 9350-9354 hem Windows Güvenlik Duvarı ağ geçidi bilgisayarında ve kurumsal Güvenlik Duvarı'nı etkinleştirin. Bkz: [bağlantı noktalarını ve Güvenlik Duvarı](#ports-and-firewall) ayrıntıları bölümü. Bu adım isteğe bağlıdır, ancak performans açıklamasında öneririz.

### <a name="3-problem"></a>3. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a>Nedeni
Ağ bağlantısı geçici bir hata.

#### <a name="resolution"></a>Çözüm
Ağ geçidi çevrimiçine almak için şu adımları izleyin:

1. Birkaç dakika bekleyin, hata kaldırılmıştır olduğunda bağlantısı otomatik olarak kurtarılacak.
* Sorun devam ederse, ağ geçidi hizmeti yeniden başlatın.

## <a name="failed-to-author-linked-service"></a>Bağlantılı hizmet yazmak başarısız oldu
### <a name="problem"></a>Sorun
Yeni bir bağlı hizmeti için kimlik bilgilerini girin veya mevcut bir bağlı hizmeti için kimlik bilgilerini güncelleştirmek için portalda kimlik bilgisi Yöneticisi'ni kullanmaya çalıştığınızda bu hatayı görebilirsiniz.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Bu hata gördüğünüzde, veri yönetimi ağ geçidi Yapılandırma Yöneticisi'nin Ayarları sayfası aşağıdaki ekran görüntüsü gibi görünebilir.

![Veritabanına erişilemiyor](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Nedeni
SSL sertifikası ağ geçidi makinesinde kesilmiş olabilir. Ağ geçidi bilgisayarı SSL şifreleme için kullanılan sertifika şu anda yüklenemiyor. Olay günlüğünde aşağıdaki iletiye benzer bir hata iletisi de görebilirsiniz.

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Çözüm
Sorunu çözmek için şu adımları izleyin:

1. Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'ni başlatın.
2. Geçiş **ayarları** sekmesi.  
3. Tıklatın **değiştirmek** SSL sertifikasını değiştirmek için düğmesi.

   ![Değişiklik sertifika düğmesi](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. SSL sertifikası olarak yeni bir sertifika seçin. Sizin tarafınızdan oluşturulan herhangi bir SSL sertifikası veya herhangi bir kuruluştaki kullanabilirsiniz.

   ![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopyalama etkinliği başarısız
### <a name="problem"></a>Sorun
Ardışık Düzen portalında kurduktan sonra aşağıdaki "UserErrorFailedToConnectToSqlserver" hatası fark edebilirsiniz.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a>Nedeni
Bu farklı nedenlerden kaynaklanabilir ve azaltma buna göre değişir.

#### <a name="resolution"></a>Çözüm
Bir SQL veritabanına bağlanmadan önce bağlantı noktası TCP/1433 veri yönetimi ağ geçidi istemci tarafında üzerinden giden TCP bağlantılarını sağlar.

Hedef veritabanının Azure SQL veritabanını, SQL Server için Güvenlik Duvarı ayarlarını Azure de kontrol edin.

Şirket içi veri deposuna bağlantıyı sınamak için aşağıdaki bölüme bakın.

## <a name="data-store-connection-or-driver-related-errors"></a>Veri deposu bağlantısı veya sürücüsü ile ilgili hataları
Veri deposunda bağlantı veya sürücü ilgili hatalar görürseniz, aşağıdaki adımları tamamlayın:

1. Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi ağ geçidi makinede başlatın.
2. Geçiş **tanılama** sekmesi.
3. İçinde **Bağlantıyı Sına**, ağ geçidi Grup değerlerini ekleyin.
4. Tıklatın **Test** , şirket içi veri kaynağına ağ geçidi makineden bağlantı bilgilerini ve kimlik bilgilerini kullanarak bağlanabildiğinizi görmek için. Bir sürücü yükledikten sonra bağlantı testi yine başarısız olursa, ağ geçidini son değişiklikleri alması için yeniden başlatın.

![Tanılama sekmesinde bağlantıyı Sına](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Ağ geçidi günlükleri
### <a name="send-gateway-logs-to-microsoft"></a>Ağ geçidi günlüklerini Microsoft'a gönderme
Ağ geçidi sorunlarını giderme konusunda yardım almak için Microsoft Support başvurduğunuzda, ağ geçidi günlüklerinizi paylaşmak istenebilir. Ağ geçidi sürümle birlikte, gerekli ağ geçidi günlüklerini iki düğme tıklama veri yönetimi ağ geçidi Yapılandırma Yöneticisi ile paylaşabilirsiniz.    

1. Geçiş **tanılama** sekmesini veri yönetimi ağ geçidi Yapılandırma Yöneticisi.

    ![Veri Yönetimi ağ geçidi Tanılama sekmesi](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Tıklatın **günlükleri Gönder** aşağıdaki iletişim kutusunu görmek için.

    ![Veri Yönetimi ağ geçidi Gönder günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (İsteğe bağlı) Tıklatın **günlüklerini görüntülemek** gözden geçirmek için günlükleri Olay Görüntüleyicisi'ni.
4. (İsteğe bağlı) Tıklatın **gizlilik** Microsoft web hizmetleri gizlilik bildirimi gözden geçirmek için.
5. Olduğunuzda, nelerdir ile karşıya yüklemek, tıklatın **günlükleri Gönder** gerçekten günlükleri son yedi gün Microsoft'a sorun giderme için gönderilecek. Aşağıdaki ekran görüntüsünde gösterildiği gibi gönderme günlükleri işlemin durumunu görmeniz gerekir.

    ![Veri Yönetimi ağ geçidi gönderme durumu günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. İşlem tamamlandıktan sonra aşağıdaki ekran görüntüsünde gösterildiği gibi bir iletişim kutusu görürsünüz.

    ![Veri Yönetimi ağ geçidi gönderme durumu günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Kaydet **rapor kimliği** ve Microsoft Support paylaşın. Rapor Kimliği, sorun giderme için karşıya ağ geçidi günlüklerini bulmak için kullanılır.  Rapor Kimliği, Olay Görüntüleyicisi'ni de kaydedilir.  Olay Kimliği "25" bakarak bulun ve tarih ve saat denetleyin.

    ![Veri Yönetimi ağ geçidi Gönderme Raporu Kimliği günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Ağ geçidi ana makinede arşiv gateway günlükleri
Burada ağ geçidi sorunları varsa ve ağ geçidi günlüklerini doğrudan paylaşamaz bazı senaryolar vardır:

* El ile ağ geçidini yükleyin ve ağ geçidini kaydedin.
* Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi yeniden anahtar ile ağ geçidini kaydetmeyi deneyin.
* Günlükleri tekrar göndermeye çalıştığında ve ağ geçidi ana bilgisayar hizmetine bağlanamaz.

Bu senaryolarda, ağ geçidi günlüklerini bir zip dosyası olarak kaydetmek ve Microsoft desteğe başvurduğunuzda paylaşın. Örneğin, ağ geçidi olarak kaydettiğiniz sırada bir hata alırsanız, aşağıdaki ekran görüntüsünde gösterilen.   

![Veri Yönetimi ağ geçidi kayıt hatası](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Tıklatın **arşiv gateway günlükleri** arşivlemek ve günlükleri kaydedin bağlamak ve zip dosyası Microsoft desteği ile paylaşabilirsiniz.

![Veri Yönetimi ağ geçidi arşiv günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Ağ geçidi günlüklerini bulun
Ayrıntılı ağ geçidi günlük bilgilerini Windows olay günlüklerini bulabilirsiniz.

1. Windows Başlat **Olay Görüntüleyicisi'ni**.
2. Günlüklerde bulun **uygulama ve hizmet günlükleri** > **veri yönetimi ağ geçidi** klasör.

 Ağ geçidi ile ilgili sorunları gidermeye çalışıyorsanız, hata düzeyi olayları için Olay Görüntüleyicisi'ni arayın.

![Veri Yönetimi ağ geçidi Olay Görüntüleyicisi'nde günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
