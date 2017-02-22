1. Uzak Masaüstü Bağlantısı kullanarak işlem sunucusu sanal makinesine bağlanın.
2. Oturum açma tamamlandıktan sonra işlem sunucusu Yapılandırma aracının otomatik olarak başlatıldığını ve aşağıdakileri girmenizi istediğini görürsünüz
  * Yapılandırma Sunucusunun tam adı (FQDN) veya IP Adresi
  * Yapılandırma sunucusunun dinleme yaptığı bağlantı noktası. Değer 443 olmalıdır
  * Yapılandırma sunucusuna bağlanmak için bağlantı parolası.
  * Bu işlem sunucusu için yapılandırılacak Veri Aktarımı bağlantı noktası. Varsayılan değeri ortamınızdaki farklı bir bağlantı noktası numarasıyla değiştirmediyseniz olduğu gibi bırakın.

    ![İşlem sunucusunu kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Yapılandırmayı kaydetmek için kaydet düğmesine tıklayın.
4. Kayıt tamamlandıktan sonra işlem sunucusu Yapılandırma sunucunuzun altında görünmeye başlar ve bundan sonra yeniden çalışma için kullanılabilir.

> [!TIP]
> İşlemci sunucusu yapılandırma yardımcı programı, sanal makinenin masaüstündeki **cspsconfigtool** kısayoluna tıklanarak başlatılabilir.


<!--HONumber=Feb17_HO1-->


