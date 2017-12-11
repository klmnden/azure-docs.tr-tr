## <a name="create-a-self-hosted-ir"></a>Şirket içinde barındırılan IR oluşturma

Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server verilerini Azure blob depolama alanına kopyalayan bileşendir. 

1. Tümleştirme çalışma zamanının adı için bir değişken oluşturun. Benzersiz bir ad kullanın ve adı not edin. Daha sonra bu öğreticide kullanacaksınız. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Oluşturulan tümleştirme çalışma zamanının durumunu almak için aşağıdaki komutu çalıştırın. **Durum** özelliği değerinin **NeedRegistration** olarak ayarlı olduğunu onaylayın. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Şirket içinde barındırılan tümleştirme çalışma zamanını bulutta Data Factory hizmetine kaydetmek üzere **kimlik doğrulaması anahtarlarını** almak için aşağıdaki komutu çalıştırın. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Sonraki adımda makinenize yüklediğiniz şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek için anahtarların birini (çift tırnak işaretlerini hariç tutarak) kopyalayın.  

## <a name="install-integration-runtime"></a>Tümleştirme çalışma zamanını yükleme
1. Makinenizde zaten **Microsoft Integration Runtime** varsa, **Program Ekle veya Kaldır** seçeneğini kullanarak programı kaldırın. 
2. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine [indirin](https://www.microsoft.com/download/details.aspx?id=39717) ve yüklemeyi çalıştırın. 
3. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazına Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.  
4. **Son Kullanıcı Lisans Anlaşması** sayfasında koşulları ve lisans anlaşmasını kabul edin ve **İleri**'ye tıklayın. 
5. **Hedef Klasör** sayfasında **İleri**'ye tıklayın. 
6. **Microsoft Tümleştirme Çalışma Zamanı Yüklemeye Hazır** bölümünde **Yükle**'ye tıklayın. 
7. Bilgisayarın kullanılmadığında uyku veya hazırda bekletme moduna geçecek şekilde yapılandırıldığını bildiren bir uyarı iletisi görürseniz, **Tamam**'a tıklayın. 
8. **Güç Seçenekleri** penceresini görürseniz kapatın ve kurulum penceresine geçin. 
9. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazı Tamamlandı** sayfasında **Son**'a tıklayın.
10. **Tümleştirme Çalışma Zamanını (Şirket içinde barındırılan) Kaydet** sayfasında, önceki bölümde kaydettiğiniz anahtarı yapıştırın ve **Kaydol**'a tıklayın. 

   ![Tümleştirme çalışma zamanını kaydetme](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. Şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla kaydedildiğinde aşağıdaki iletiyi görürsünüz:

   ![Başarıyla kaydedildi](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. **Yeni Tümleştirme Çalışma Zamanı (Şirket içinde barındırılan) Düğümü** sayfasında **İleri**'ye tıklayın. 

    ![Yeni Tümleştirme Çalışma Zamanı Düğümü sayfası](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. **İntranet İletişim Kanalı**'nda **Atla**'ya tıklayın. Çok düğümlü bir tümleştirme çalışma zamanı ortamında düğüm içi iletişimin güvenliğini sağlamak için TLS/SSL sertifikası seçebilirsiniz. 

    ![İntranet iletişim kanalı sayfası](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. **Tümleştirme Çalışma Zamanını (Şirket içinde barındırılan) Kaydet** sayfasında **Configuration Manager'ı Başlat**'a tıklayın. 
6. Düğüm bulut hizmetine bağlandığında şu sayfayı görürsünüz:

   ![Düğüm bağlı](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Şimdi SQL Server veritabanınıza yönelik bağlantıyı test edin.

    ![Tanılama sekmesi](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - **Configuration Manager** penceresinde **Tanılama** sekmesine geçin.
    - **Veri kaynağı türü** olarak **SqlServer**’ı seçin.
    - **Sunucu** adını girin.
    - **Veritabanı** adını girin. 
    - **Kimlik doğrulaması** modunu seçin. 
    - **Kullanıcı** adını girin. 
    - Kullanıcı adı için **parola** girin.
    - Tümleştirme çalışma zamanının SQL Server’a bağlanabildiğini onaylamak için **Test**’e tıklayın. Bağlantı başarılı olursa yeşil bir onay işareti görürsünüz. Başarılı olmazsa, hata ile ilişkili hata iletisini görürsünüz. Sorunları giderin ve tümleştirme çalışma zamanının SQL Server’a bağlanabildiğinden emin olun.    

    > [!NOTE]
    > Bu değerleri not edin (kimlik doğrulama türü, sunucu, veritabanı, kullanıcı, parola). Daha sonra bu öğreticide kullanacaksınız. 
    
