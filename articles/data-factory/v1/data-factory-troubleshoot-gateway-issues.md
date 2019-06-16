---
title: Veri Yönetimi ağ geçidi sorunlarını giderme | Microsoft Docs
description: Veri Yönetimi ağ geçidi için ilgili sorunları gidermek için ipuçları sağlar.
services: data-factory
author: nabhishek
manager: craigg
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/01/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 0559d89bd691323a95713d518df05e58283cef39
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61253760"
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi kullanımıyla ilgili sorunları giderme
Bu makalede, veri yönetimi ağ geçidi kullanarak sorunlarını giderme hakkında bilgi sağlar.

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [barındırılan Data factory'de tümleştirme çalışma zamanını](../create-self-hosted-integration-runtime.md).

Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale ağ geçidi hakkında ayrıntılı bilgi için. Bkz: [şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) veri ağ geçidi kullanarak bir şirket içi SQL Server veritabanındaki verileri Microsoft Azure Blob depolama alanına taşıyarak, makale kılavuz.

## <a name="failed-to-install-or-register-gateway"></a>Yükleyemedi veya ağ geçidini kaydetme
### <a name="1-problem"></a>1. Sorun
Yükleme ve bir ağ geçidi, özellikle, ağ geçidi yükleme dosyası indirilirken kaydetme bu hata iletisi görürsünüz.

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Nedeni
Ağ geçidini Yüklemeye çalıştığınız makine bir ağ sorunu nedeniyle İndirme Merkezi'nden en son ağ geçidi yükleme dosyasını indirmek başarısız oldu.

#### <a name="resolution"></a>Çözüm
Ayarları bilgisayardaki ağ bağlantısı block olup olmadığını görmek için güvenlik duvarı proxy sunucunuzun ayarları denetleyin [İndirme Merkezinden](https://download.microsoft.com/)ve sonra ayarları güncelleştirmek uygun şekilde.

Alternatif olarak, en son ağ geçidini yükleme dosyasını indirebilirsiniz [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) diğer makinelere indirme Merkezi'ne erişebilirsiniz. Ardından, ağ geçidi ana bilgisayarına yükleyici dosyasını kopyalayın ve el ile yükleyin ve ağ geçidi güncelleştirmek için çalıştırın.

### <a name="2-problem"></a>2. Sorun
Tıklayarak bir ağ geçidini Yüklemeye çalıştığınız olduğunda bu hatayı görmeye **doğrudan bu bilgisayara yüklemek** Azure portalında.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Nedeni
Bir ağ geçidi, makinede zaten yüklü.

#### <a name="resolution"></a>Çözüm
Makinede mevcut ağ geçidini kaldırın ve tıklayın **doğrudan bu bilgisayara yüklemek** yeniden bağlanın.

### <a name="3-problem"></a>3. Sorun
Yeni bir ağ geçidi kaydedilirken bu hatayı görebilirsiniz.

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a>Nedeni
Aşağıdaki nedenlerden biri için bu iletiyi görebilirsiniz:

* Ağ geçidi anahtarının biçimi geçersiz.
* Ağ geçidi anahtarı geçersiz hale getirildi.
* Ağ geçidi anahtarı portaldan üretildi.  

#### <a name="resolution"></a>Çözüm
Portaldan doğru ağ geçidi anahtarı kullanarak doğrulayın. Gerekirse, bir anahtar ve ağ geçidini kaydetmek için anahtarını kullanın.

### <a name="4-problem"></a>4. Sorun
Bir ağ geçidi kaydedilirken, aşağıdaki hata iletisini görebilirsiniz.

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![İçerik veya anahtarının biçimi geçersiz](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Nedeni
İçerik veya giriş ağ geçidi anahtarının biçimi doğru değil. Nedenlerden biri, anahtarın yalnızca bir kısmını portaldan kopyaladığınız veya geçersiz bir anahtar kullandığınız olabilir.

#### <a name="resolution"></a>Çözüm
Portalda bir ağ geçidi anahtarı oluşturun ve tüm anahtar kopyalamak için Kopyala düğmesini kullanın. Ardından ağ geçidini kaydetmek için bu penceresine yapıştırın.

### <a name="5-problem"></a>5. Sorun
Bir ağ geçidi kaydedilirken, aşağıdaki hata iletisini görebilirsiniz.

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Ağ geçidi anahtarı geçersiz veya boş.](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi anahtar yeniden oluşturulacak veya Azure portalında bir ağ geçidi silinmiş olabilir. Veri Yönetimi ağ geçidi Kurulum en son değilse de oluşabilir.

#### <a name="resolution"></a>Çözüm
Veri Yönetimi ağ geçidi Kurulum en son sürüm ise bulabilirsiniz en son sürümü Microsoft denetleyin [İndirme Merkezinden](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Kurulumu, geçerli / latest ve ağ geçidi, portalı hala mevcut olduğundan, Azure portalında, ağ geçidi anahtarı yeniden ve tüm anahtar kopyalamak için Kopyala düğmesini kullanın ve ardından ağ geçidini kaydetmek için bu pencereyi yapıştırın. Aksi takdirde, ağ geçidini yeniden oluşturun ve baştan başlayın.

### <a name="6-problem"></a>6. Sorun
Bir ağ geçidi kaydedilirken, aşağıdaki hata iletisini görebilirsiniz.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Ağ geçidi anahtarı geçersiz veya boş.](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Nedeni
Bu hata, ağ geçidi silinmiş olabilir veya ilişkili bir ağ geçidi anahtar yeniden oluşturulacak nedeniyle gerçekleşebilir.

#### <a name="resolution"></a>Çözüm
Ağ geçidi silinmiş olması durumunda, portaldan ağ geçidini yeniden oluşturmak, tıklayın **kaydetme**, anahtar portaldan kopyalayın, yapıştırın ve ağ geçidi kaydetmeyi deneyin.

Ağ geçidi hala var, ancak anahtarıyla yeniden, ağ geçidini kaydetmek için yeni anahtarı kullanın. Anahtar yoksa, Portalı'ndan yeniden tuşunu yeniden oluşturun.

### <a name="7-problem"></a>7. Sorun
Bir ağ geçidi kaydettirmekte zaman için bir sertifika yolu ve parola girmeniz gerekebilir.

![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi, diğer makinelere önce kaydedildi. Bir ağ geçidi ilk kayıt sırasında bir şifreleme sertifikası ağ geçidi ile ilişkilendirilmiş. Sertifika kendi ağ geçidi tarafından oluşturulan veya kullanıcı tarafından sağlanan.  Bu sertifika, veri deposunun (bağlı hizmet) kimlik bilgilerini şifrelemek için kullanılır.  

![Sertifikayı dışarı aktarma](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Ağ geçidi farklı bir konak makinesi üzerinde geri yüklerken, daha önce bu sertifika ile şifrelenmiş kimlik bilgilerinin şifresini çözmek Bu sertifika için Kayıt Sihirbazı'nı ister.  Bu sertifika olmadan kimlik bilgilerini yeni ağ geçidi tarafından şifresi çözülemiyor ve bu yeni ağ geçidi ile ilişkili sonraki kopyalama etkinliği yürütme başarısız olur.  

#### <a name="resolution"></a>Çözüm
Kimlik bilgisi sertifikası özgün ağ geçidi makineden kullanarak dışa aktardığınız varsa **dışarı** düğmesini **ayarları** sekmesinde Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde, sertifikayı kullanın Burada.

Bu aşamada, ağ geçidi kurtarırken atlayamazsınız. Sertifika yoksa, portaldan ağ geçidini silin ve yeni bir ağ geçidi yeniden oluşturmanız gerekir.  Ayrıca, kullanıcıların kimlik bilgilerini yeniden girildi tarafından ağ geçidine ilgili tüm bağlantılı Hizmetleri güncelleştirin.

### <a name="8-problem"></a>8. Sorun
Aşağıdaki hata iletisini görebilirsiniz.

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Nedeni
Ağ geçidi, proxy kimlik doğrulaması parola veya Internet kaynaklarına erişmek için bir HTTP proxy'sinin değişir ancak uygun şekilde güncelleştirilmez gerektiren bir ortamda olduğunda bu hata oluşur, ağ geçidi'nde.

#### <a name="resolution"></a>Çözüm
Bu makalenin Proxy sunucusu konuları bölümünde yer alan yönergeleri izleyin ve veri yönetimi ağ geçidi Yapılandırma Yöneticisi ile proxy ayarlarını yapılandırın.

## <a name="gateway-is-online-with-limited-functionality"></a>Ağ geçidi sınırlı işlevsellikle çevrimiçi
### <a name="1-problem"></a>1. Sorun
Sınırlı işlevsellikle çevrimiçi olarak ağ geçidinin durumunu görürsünüz.

#### <a name="cause"></a>Nedeni
Ağ geçidinin durumunu çevrimiçi olarak sınırlı işlevsellikle aşağıdakilerden birini görürsünüz:

* Ağ geçidi, Azure Service Bus üzerinden bulut hizmetine bağlanamıyor.
* Bulut hizmeti, ağ geçidi üzerinden Service Bus ile bağlantı kuramıyor.

Ağ geçidi sınırlı işlevsellikle çevrimiçi olduğunda ya da şirket içi veri depolarından veri kopyalamak için veri işlem hatları oluşturmak için Data Factory Kopyalama Sihirbazı'nı kullanmayı mümkün olmayabilir. Geçici bir çözüm olarak, portal, Visual Studio veya Azure PowerShell, Data Factory Düzenleyicisi'ni kullanabilirsiniz.

#### <a name="resolution"></a>Çözüm
Bu sorunun çözümü (sınırlı işlevsellikle çevrimiçi) ağ geçidi bulut hizmeti ya da başka bir şekilde bağlantı kurulamıyor üzerinde temel alır. Aşağıdaki bölümlerde, bu çözümleri sağlayın.

### <a name="2-problem"></a>2. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Gateway cannot connect to cloud service through service bus`

![Ağ geçidi, bulut hizmetine bağlanamıyor](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni
Ağ geçidi, Service Bus üzerinden bulut hizmetine bağlanamıyor.

#### <a name="resolution"></a>Çözüm
Ağ geçidi çevrimiçine almak için aşağıdaki adımları izleyin:

1. IP adresi, ağ geçidi makinesi ve kurumsal güvenlik duvarının giden kuralları sağlar. Windows olay günlüğünden IP adresleri bulabilirsiniz (kimliği 401 ==): Bir yuva erişim izinlerini XX tarafından yasaklanmış bir şekilde erişmek için girişimde bulunuldu. XX. XX. XX:9350.
1. Ağ geçidi üzerinde proxy ayarlarını yapılandırın. Ayrıntılar için Proxy sunucusu dikkat edilecek noktalar bölümüne bakın.
1. Giden bağlantı noktası 5671 ve 9350-9354 hem Windows Güvenlik Duvarı ağ geçidi makinesinde ve kurumsal Güvenlik Duvarı'nı etkinleştirin. Bağlantı noktalarını ve ayrıntı için güvenlik duvarı bölümüne bakın. Bu adım isteğe bağlıdır, ancak performans meselesi öneririz.

### <a name="3-problem"></a>3. Sorun
Aşağıdaki hatayı görürsünüz.

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a>Nedeni
Ağ bağlantısı geçici bir hata.

#### <a name="resolution"></a>Çözüm
Ağ geçidi çevrimiçine almak için aşağıdaki adımları izleyin:

1. Birkaç dakika bekleyin, bağlantı kayboldu hatası olduğunda otomatik olarak kurtarılır.
1. Sorun devam ederse, ağ geçidi hizmetini yeniden başlatın.

## <a name="failed-to-author-linked-service"></a>Bağlı hizmet oluşturmak başarısız oldu
### <a name="problem"></a>Sorun
Yeni bağlı hizmet kimlik bilgilerini girin veya mevcut bir bağlı hizmeti için kimlik bilgilerini güncelleştirmek için portalda kimlik bilgisi Yöneticisi'ni kullanmaya çalıştığınızda bu hatayı görebilirsiniz.

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

Bu hatayı gördüğünüzde, Ayarlar sayfasında, veri yönetimi ağ geçidi Yapılandırma Yöneticisi aşağıdaki ekran görüntüsüne benzer görünebilir.

![Veritabanına ulaşılamıyor](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Nedeni
SSL sertifikası bir ağ geçidi makinesinde kesilmiş olabilir. Ağ geçidi bilgisayar SSL şifreleme için kullanılan sertifika şu anda yüklenemiyor. Olay günlüğünde aşağıdaki iletiye benzer bir hata iletisi de görebilirsiniz.

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Çözüm
Sorunu çözmek için aşağıdaki adımları izleyin:

1. Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'ni başlatın.
2. **Ayarlar** sekmesine geçin.  
3. Tıklayın **değiştirme** SSL sertifikasını değiştirilecek düğmesi.

   ![Değişiklik sertifika düğmesi](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. SSL sertifikası olarak yeni bir sertifika seçin. Sizin tarafınızdan oluşturulan herhangi bir SSL sertifikası veya herhangi bir kuruluşun kullanabilirsiniz.

   ![Sertifika belirtin](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Kopyalama etkinliği başarısız
### <a name="problem"></a>Sorun
Portalda bir işlem hattı ayarladıktan sonra aşağıdaki "UserErrorFailedToConnectToSqlserver" hatası fark edebilirsiniz.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a>Nedeni
Bu farklı nedenlerden kaynaklanıyor ve risk azaltma buna göre değişir.

#### <a name="resolution"></a>Çözüm
Bir SQL veritabanına bağlanmadan önce veri yönetimi ağ geçidi istemci tarafında TCP/1433 numaralı bağlantı noktası üzerinden giden TCP bağlantılarına izin.

Hedef veritabanının Azure SQL veritabanı, SQL Server için Güvenlik Duvarı ayarlarını Azure de denetleyin.

Şirket içi veri deposuna olan bağlantıyı sınamak için aşağıdaki bölüme bakın.

## <a name="data-store-connection-or-driver-related-errors"></a>Veri deposu bağlantısı veya sürücü ile ilgili hataları
Verileri Bağlantısı veya sürücü ile ilgili hatalar görürseniz, aşağıdaki adımları tamamlayın:

1. Ağ geçidi makinesinde veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni başlatın.
2. Geçiş **tanılama** sekmesi.
3. İçinde **Bağlantıyı Sına**, ağ geçidi Grup değerlerini ekleyin.
4. Tıklayın **Test** için şirket içi veri kaynağına ağ geçidi makine kimlik bilgileri ve bağlantı bilgilerini kullanarak bağlanıp bağlanamadığınızı görün. Bir sürücü yükledikten sonra bağlantı testi yine başarısız olursa, ağ geçidini son değişiklikleri alması için yeniden başlatın.

![Tanılama sekmesi içinde Bağlantıyı Sına](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Ağ geçidi günlükleri
### <a name="send-gateway-logs-to-microsoft"></a>Ağ geçidi günlükleri Microsoft'a gönder
Ağ geçidi sorunlarını giderme konusunda yardım almak için Microsoft Support başvurduğunuzda gateway günlüklerinizi paylaşmak istemediğiniz sorulabilir. Ağ Geçidi'nın yayınlanmasıyla birlikte, gerekli ağ geçidi günlükleri iki düğme tıklamaları veri yönetimi ağ geçidi Yapılandırma Yöneticisi ile paylaşabilirsiniz.    

1. Geçiş **tanılama** sekmesi veri yönetimi ağ geçidi Yapılandırma Yöneticisi.

    ![Veri Yönetimi ağ geçidi Tanılama sekmesi](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Tıklayın **günlükleri Gönder** aşağıdaki iletişim kutusunu görmek için.

    ![Veri Yönetimi ağ geçidi Gönder günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (İsteğe bağlı) Tıklayın **günlükleri görüntüleyebilir** gözden geçirmek için günlükleri Olay Görüntüleyicisi.
4. (İsteğe bağlı) Tıklayın **gizlilik** Microsoft web hizmetleri gizlilik bildirimi gözden geçirmek için.
5. Olduğunuzda olduklarınız ile yüklemek için tıklayın **günlükleri Gönder** gerçekten günlükleri son yedi günden sorun giderme amacıyla Microsoft'a gönderilecek. Aşağıdaki ekran görüntüsünde gösterildiği gibi günlükleri gönderme işleminin durumunu görmeniz gerekir.

    ![Veri Yönetimi ağ geçidi gönderme durumu günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. İşlem tamamlandıktan sonra aşağıdaki ekran görüntüsünde gösterildiği gibi bir iletişim kutusu görürsünüz.

    ![Veri Yönetimi ağ geçidi gönderme durumu günlüğe kaydeder](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Kaydet **rapor kimliği** ve Microsoft Support paylaşın. Rapor Kimliği, sorun giderme için yüklediğiniz ağ geçidi günlüklerini bulmak için kullanılır.  Rapor Kimliği, Olay Görüntüleyicisi'ni de kaydedilir.  Olay Kimliği "25" bakarak bulun ve tarih ve saat denetleyin.

    ![Veri Yönetimi ağ geçidi gönderme günlükleri Raporu Kimliği](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Ağ geçidi ana makinede arşiv ağ geçidi günlükleri
Burada, ağ geçidiyle ilgili sorunları varsa ve ağ geçidi günlükleri doğrudan paylaşamaz bazı senaryolar vardır:

* El ile ağ geçidi yüklemeniz ve ağ geçidi kaydedin.
* Anahtar yeniden oluşturuldu veri yönetimi ağ geçidi Configuration Manager ile ağ geçidini kaydetmek deneyin.
* Günlükleri göndermek deneyin ve ağ geçidi ana bilgisayar hizmeti bağlanamaz.

Bu senaryolar için ağ geçidi günlükleri zip dosyası olarak kaydedin ve Microsoft Destek ekibiyle iletişime geçtiğinizde paylaşabilirsiniz. Örneğin, ağ geçidi olarak kaydetme sırasında bir hata alırsanız aşağıdaki ekran görüntüsünde gösterilir.   

![Veri Yönetimi ağ geçidi kayıt hatası](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Tıklayın **arşiv ağ geçidi günlükleri** arşivlemek ve günlükleri kaydedin bağlamak ve zip dosyası Microsoft desteği ile paylaşın.

![Veri Yönetimi ağ geçidi arşiv günlükleri](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Ağ geçidi günlükleri bulun
Windows olay günlüklerindeki ayrıntılı ağ geçidi günlüğü bilgilerini bulabilirsiniz.

1. Windows Başlat **Olay Görüntüleyicisi'ni**.
2. Günlüklerde bulun **uygulama ve hizmet günlükleri** > **veri yönetimi ağ geçidi** klasör.

   Ağ geçidi ile ilgili sorunları gidermeye çalışıyorsanız, hata düzeyi olaylarına bakın, Olay Görüntüleyicisi'ni arayın.

![Veri Yönetimi ağ geçidi Olay Görüntüleyicisi'nde günlüğe kaydeder.](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
