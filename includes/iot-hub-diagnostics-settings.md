### <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarları ile günlük kaydını etkinleştir

1. Oturum [Azure portal] [ lnk-portal] ve IOT Hub'ına gidin.
1. Seçin **tanılama ayarlarını**.
1. Seçin **tanılamayı açın**.

   ![Tanılamayı açın][1]

1. Tanılama ayarları bir ad verin.
1. Seçmek istediğiniz günlükleri göndermek. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
   * Bir depolama hesabına arşivle
   * Bir olay hub'ına akış
   * Günlük analizi için Gönder
1. İzlemek istediğiniz işlemleri seçin ve bu işlemler için günlüklerini etkinleştirin. Tanılama ayarları hakkında rapor işlemleri şunlardır:
   * Bağlantılar
   * Cihaz telemetrisi
   * Bulut-cihaz iletilerini
   * Aygıt Kimliği işlemleri
   * Dosya yüklemeleri
   * İleti yönlendirme
   * Bulut-cihaz çifti işlemleri
   * Cihaz bulut çifti işlemleri
   * Twin işlemleri
   * İş işlemleri
   * Doğrudan yöntemler  
1. Yeni ayarları kaydedin. 

Tanılama ayarları PowerShell ile etkinleştirmek istiyorsanız, aşağıdaki kodu kullanın:

```
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra günlükler yapılandırılmış arşivleme hedef görünür **tanılama ayarları** dikey. Tanılama yapılandırma hakkında daha fazla bilgi için bkz: [toplamak ve azure kaynaklarınızdan günlük verilerini tüketen][lnk-diagnostics-settings].

<!-- Images -->
[1]: ./media/iot-hub-diagnostics-settings/turnondiagnostics.png

<!-- Links -->
[lnk-portal]: https://portal.azure.com
[lnk-diagnostics-settings]: ../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
