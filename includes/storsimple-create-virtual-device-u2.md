#### <a name="to-create-a-virtual-device"></a>Sanal cihazı oluşturmak için
1. Azure portalında **StorSimple Yöneticisi** hizmetine gidin.
2. **Cihazlar** sayfasına gidin. **Cihazlar** sayfasının altında **Sanal cihaz oluştur**’a tıklayın.
3. **Sanal Cihaz Oluştur** iletişim kutusunda aşağıdaki ayrıntıları belirtin.
   
    ![StorSimple sanal cihaz oluşturma](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Ad** – sanal cihazınız için benzersiz bir ad.
   2. **Model** - Sanal cihazın modelini seçin. Bu alan, yalnızca Update 2 veya sonraki bir sürümünü çalıştırıyorsanız kullanılabilir. 8010 cihaz modeli 30 TB Standart Depolama sunarken 8020 modeliyse 64 TB Premium Storage sunar. 8010’u belirtin
   3. öğe düzeyinde alma senaryolarını yedeklerden dağıtmak için. Yüksek performanslı, düşük gecikme süreli iş yükleri dağıtmak için veya olağanüstü durum kurtarma için ikincil bir aygıt olarak kullanılması için 8020’yi seçin.
   4. **Sürüm** - Sanal cihazın sürümünü seçin. 8020 cihaz modeli seçiliyse sürüm alanı kullanıcıya gösterilmez. Bu hizmetle kaydedilen tüm fiziksel cihazlar Update 1 (veya sonraki sürümler) çalıştırıyorsa bu seçenek yoktur. Bu alan yalnızca, Update 1 öncesi ve Update 1 karışımı, aynı hizmetle kaydedilmiş fiziksel cihazlarınız varsa kullanıma açılır. Var sayalım ki, sanal cihazın sürümü hangi fiziksel cihazın devredileceğini veya buradan kopyalanacağını saptayacaktır; sanal cihazın uygun bir sürümünü oluşturmanız önemlidir. Seçin:
      
      * Version Update 0.3, Update 0.3 veya önceki sürümleri çalıştıran fiziksel cihazdan devrederseniz veya DR. 
      * Version Update 1, Update 1 (veya sonraki sürümler) çalıştıran fiziksel cihazdan devrederseniz veya kopyalarsınız. 
   5. **Sanal Ağ** – Bu sanal cihazla kullanmak istediğiniz bir sanal ağ belirtin. Premium Storage (Update 2 veya sonraki sürümler) kullanılıyorsa, Premium Storage hesabıyla desteklenen bir sanal ağ seçmelisiniz. Desteklenmeyen sanal ağlar açılan listede gri olacaktır. Desteklenmeyen sanal ağ seçerseniz bir uyarı alırsınız. 
   6. **Sanal Cihaz Oluşturma için Depolama Hesabı** – Sağlama sırasında sanal cihazın bir görüntüsü tutmak için depolama hesabı seçin. Bu depolama hesabı sanal cihaz ve sanal ağla aynı bölgede olmalıdır. Fiziksel veya sanal aygıt tarafından veri depolama için kullanılmamalıdır. Varsayılan olarak, yeni depolama hesabı bu amaçla oluşturulur. Ancak, bu kullanım için uygun bir depolama hesabınızın zaten olduğunu biliyorsanız, bunu listeden seçebilirsiniz. Premium sanal aygıt oluşturuluyorsa, açılan listede yalnızca Premium Storage hesapları görüntülenir. 
      
      > [!NOTE]
      > Sanal cihaz yalnızca Azure Storage hesaplarıyla çalışabilir. Amazon, HP ve OpenStack (fiziksel cihaz için desteklenir) gibi başka bulut hizmeti sağlayıcıları StorSimple sanal cihazı için desteklenmez.
      > 
      > 
   7. Sanal cihazda depolanan verilerin Microsoft veri merkezinde barındırılacağını anladığınızı belirtmek için onay işaretine tıklayın. Yalnızca bir fiziksel cihaz kullandığınızda, şifreleme anahtarınızın cihazınızla tutulur; Bu nedenle, Microsoft şifresini çözemez. 
      
       Sanal cihaz kullandığınızda, hem şifreleme anahtarı, hem de şifre çözme anahtarı Microsoft Azure’da depolanır. Daha fazla bilgi için bkz. [sanal cihaz kullanımıyla ilgili güvenlik konuları](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Sanal cihaz oluşturmak için onay simgesine tıklayın. Cihazın hazır olması yaklaşık 30 dakika sürebilir.
      
      ![StorSimple sanal cihaz oluşturma aşaması](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)



<!--HONumber=Jan17_HO1-->


