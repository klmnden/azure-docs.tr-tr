1. Uzak Masaüstü Bağlantısı’nı kullanarak İşlem Sunucusu sanal makinesine bağlanın.
2. Masaüstündeki kısayola tıklayarak cspsconfigtool.exe aracını başlatabilirsiniz. (İşlem sunucusunda ilk kez oturum açıyorsanız araç otomatik olarak başlatılır.)
  * Yapılandırma Sunucusunun tam adı (FQDN) veya IP Adresi
  * Yapılandırma sunucusunun dinleme yaptığı bağlantı noktası. Değer 443 olmalıdır
  * Yapılandırma sunucusuna bağlanmak için bağlantı parolası.
  * Bu İşlem Sunucusu için yapılandırılacak Veri Aktarımı bağlantı noktası. Varsayılan değeri ortamınızdaki farklı bir bağlantı noktası numarasıyla değiştirmediyseniz olduğu gibi bırakın.

    ![İşlem Sunucusunu Kaydetme](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Yapılandırmayı kaydetmek için kaydet düğmesine tıklayın.
4. Kayıt tamamlandıktan sonra İşlem Sunucusu, Yapılandırma sunucunuzun altında görünmeye başlar ve bundan sonra yeniden çalışma için kullanılabilir.

> [!TIP]
> İşlem Sunucusu yapılandırma yardımcı programı, sanal makinenin masaüstündeki **cspsconfigtool** kısayoluna çift tıklanarak başlatılabilir.


<!--HONumber=Feb17_HO4-->


