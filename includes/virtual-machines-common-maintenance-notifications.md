
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Portalda bakım için zamanlanmış görünüm VM'ler

Planlı bakım wave zamanlanmış ve bildirimler gönderilir sonra yaklaşan Bakımı wave tarafından etkilenen sanal makinelerin listesini görebilirsiniz. 

Azure Portalı'nı kullanın ve bakım için zamanlanmış VM'ler arayın.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol gezinti bölmesinde tıklatın **sanal makineleri**.

3. Sanal makineler bölmesinde **sütunları** düğmesi kullanılabilir sütunlar listesi açın.

4. Seçin ve aşağıdaki sütunlar ekleyin:

   **Bakım** -VM için bakım durumunu gösterir. Olası değerler şunlardır:
      
      | Değer | Açıklama |
      |-------|-------------|
      | Şimdi başlayın | VM bakım başlatmak imkan tanıyan Self Servis bakım penceresinde kendiniz'dir. Nasıl bakım, VM başlatmak aşağıya bakın | 
      | Zamanlanmış | VM bakım başlatmasını seçeneği ile bakım için zamanlandı. Bu görünümde otomatik zamanlanmış penceresini seçerek veya VM tıklatarak bakım penceresi öğrenin | 
      | tamamlandı | Başarıyla başlatıldı ve VM üzerinde bakım tamamlandı. | 
      | Atlandı| Bakım hiçbir başarı ile başlatmak için seçtiniz. Azure VM için bakım iptal etti ve daha sonraki bir zaman yeniden | 
      | Daha sonra yeniden deneyin| Bakım başlatmak için seçmiş olduğunuz ve Azure, isteği gerçekleştirmek mümkün değildi. Bu durumda, bir süre sonra yeniden deneyebilirsiniz. | 
   
   **Bakım profesyonel etkin** -Vm'leriniz kendi kendine Bakımı başlatabilirsiniz zaman penceresini gösterir.
   
   **Bakım zamanlanmış** -Azure VM'yi bakım tamamlamak için yeniden başlattığınızda zaman penceresini gösterir. 




## <a name="notification-and-alerts-in-the-portal"></a>Bildirim ve Portalı'nda uyarıları

Azure, abonelik sahibi ve ikincil sahipler grubu için bir e-posta göndererek planlı bakım için bir zamanlama iletişim kurar. Azure etkinlik günlüğü uyarıları oluşturarak bu iletişim için ek alıcılar ve kanalları ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure etkinlik günlüğü ile abonelik etkinliğini izleme] (../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüde seçin **İzleyici**. 
3. İçinde **İzleyicisi - etkinlik günlüğü** bölmesinde, **uyarıları**.
4. İçinde **İzleyicisi - uyarıları** bölmesinde tıklatın **+ Ekle etkinliği günlük uyarı**.
5. Bilgileri tamamlayın **etkinlik günlüğü uyarı Ekle** sayfasında ve kümesinde aşağıdaki emin olun **ölçütleri**: **türü**: Bakım **durum**: Tüm (ayarlanmamış durumu etkin veya çözümlenmiş) **düzeyi**: tüm
    
Etkinlik günlüğü uyarılarının nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [etkinlik günlüğü uyarı oluşturma](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Bakım portalından VM'yi Başlat

VM ayrıntıları bakarken bakım ile ilgili daha fazla ayrıntı görmeye olacaktır.  
VM'yi bir planlı bakım wave eklenirse VM Ayrıntılar görünümü üst kısmında, yeni bir bildirim Şerit eklenir. Ayrıca, mümkün olduğunda bakım başlatmak için yeni bir seçenek eklenir. 


Bakım bildirim planlı bakım Bakım Sayfası daha fazla ayrıntı görmek için tıklayın. Buradan, yapabilirsiniz **bakımını Başlat** VM üzerinde.

Bakım başladıktan sonra sanal makinenin yeniden başlatılması ve Bakım durumu birkaç dakika içinde sonucu yansıtacak şekilde güncelleştirilir.

Bakım başlayabileceğiniz penceresi Kaçırıldı, VM'yi Azure tarafından yeniden başlatılacak zaman penceresi görmeye devam edersiniz. 
