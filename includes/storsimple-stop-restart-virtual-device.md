#### <a name="to-stop-and-start-a-virtual-device"></a>Banal cihazı durdurmak ve başlatmak için
Sanal cihazı durdurmak için adına tıklayın ve ardından **Kapat**’a tıklayın. Sanal cihaz kapatıldığı sırada **Durduruluyor** durumundadır. Sanal cihaz kapatıldıktan sonra **Durduruldu** durumundadır.

Sanal cihazı durdurmak ve başlatmak için aşağıdaki cmdlet'leri kullanın.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

#### <a name="to-restart-a-virtual-device"></a>Sanal cihazı yeniden başlatmak için
Sanal cihaz çalışırken yeniden başlatmak istediğinizde cihazın adına tıklayın ve ardından **Yeniden Başlat**’a tıklayın. Sanal cihaz yeniden başlatıldığı sırada **Yeniden başlatılıyor** durumundadır. Sanal cihaz kullanımınıza hazır olduğunda **Çalışıyor** durumundadır.

Sanal cihazı yeniden başlatmak için aşağıdaki cmdlet'i kullanın.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`

