<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-to-change-the-service-data-encryption-key-in-the-azure-classic-portal"></a>1. adım: Azure Klasik Portalı'nda hizmet verileri şifreleme anahtarı değiştirmek için bir aygıt yetkilendirme
Genellikle, cihaz yönetici Hizmet Yöneticisi hizmeti veri şifreleme anahtarları değiştirmek için bir aygıt yetkilendirmek ister. Hizmet Yöneticisi, ardından anahtarı değiştirmek için aygıtı yetkilendirecek.

Bu adım, Azure Klasik Portalı'nda gerçekleştirilir. Hizmet Yöneticisi bir cihaz yetki verilmesi uygun olan aygıtları görüntülenen listesinden seçebilirsiniz. Cihaz sonra hizmet verileri şifreleme anahtar değiştirme işlemini başlatmak için yetkili.

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Hangi cihazların hizmeti veri şifreleme anahtarları değiştirme yetkisi?
Hizmet verileri şifreleme anahtarı değişiklikleri başlatmak için yetkili önce bir aygıt aşağıdaki ölçütleri karşılamalıdır:

* Aygıt hizmeti veri şifreleme anahtarı değişiklik yetkilendirme için uygun olması için çevrimiçi olması gerekir.
* Anahtar değişikliği başlatılmaz, 30 dakika sonra yeniden aynı aygıt yetki verebilir.
* Anahtar değişikliği daha önceden yetkili aygıt tarafından başlatılmamış olması koşuluyla farklı bir cihaz yetki verebilir. Yeni cihaz yetkilendirildikten sonra eski aygıt değişikliği başlatamaz.
* Hizmet verileri şifreleme anahtarı geçiş işlemi sürerken bir aygıtı yetkilendirilemiyor.
* Başkalarının olmamasına karşın bazı hizmetine kayıtlı cihazlar üzerinde şifrelemeyi gezinirken bir aygıtı yetki verebilir. Böyle durumlarda, hizmet verileri şifreleme anahtarı tamamladınız olanları değiştirmek uygun aygıtlardır.

> [!NOTE]
> Klasik Azure portalında StorSimple sanal cihazlar anahtar değişikliği başlatmak için yetkili aygıtlar listesinde gösterilmez.
> 
> 

Seçin ve hizmet verileri şifreleme anahtarı değişikliğini başlatmak için bir aygıt yetkilendirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-authorize-a-device-to-change-the-key"></a>Anahtarı değiştirmek için bir aygıt yetkilendirmek için
1. Hizmet panosu sayfasında, tıklatın **değişiklik hizmeti veri şifreleme anahtarı**.
   
    ![Değişiklik hizmeti şifreleme anahtarı](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. İçinde **değişiklik hizmeti veri şifreleme anahtarı** iletişim kutusunda, seçin ve hizmet verileri şifreleme anahtarı değişikliğini başlatmak için bir aygıt yetkilendirmek. Aşağı açılan liste yetkilendirilebilir uygun aygıtı yok.
3. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>2. adım: Hizmet verileri şifreleme anahtarı değişikliğini başlatmak StorSimple için Windows PowerShell'i kullanın
Bu adım, StorSimple arabirimi yetkili StorSimple cihazında için Windows PowerShell'de gerçekleştirilir.

> [!NOTE]
> Anahtar geçişi tamamlanana kadar hiçbir işlem, StorSimple Yöneticisi hizmetiniz Azure Klasik portalında gerçekleştirilebilir.
> 
> 

Windows PowerShell arabirimine bağlamak için cihaz seri konsoluna kullanıyorsanız, aşağıdaki adımları gerçekleştirin.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Hizmet verileri şifreleme anahtarı değişikliğini başlatmak için
1. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut istemine yazın:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Cmdlet başarıyla tamamlandıktan sonra yeni bir hizmet verileri şifreleme anahtarı alır. Kopyalayın ve bu anahtarı kullanmak için bu işlem 3. adımında kaydedin. Bu anahtar StorSimple Yöneticisi hizmetine kayıtlı kalan tüm aygıtlar güncelleştirmek için kullanılır.
   
   > [!NOTE]
   > Bu işlem, StorSimple cihaz yetkilendirme dört saat içinde başlatılmalıdır.
   > 
   > 
   
   Bu yeni anahtarı, ardından hizmetle kaydedilen tüm aygıtlara edilmesini hizmetine gönderilir. Bir uyarı sonra hizmet panosunda görüntülenir. Hizmet kayıtlı cihazlarda tüm işlemleri devre dışı bırakır ve cihaz Yöneticisi daha sonra diğer cihazlarda hizmet verileri şifreleme anahtarı güncelleştirmeniz gerekir. Ancak, g/ç (ana bilgisayardan verileri buluta gönderme) kesilmiş olabilir değil.
   
   Hizmete kayıtlı tek bir aygıtta varsa, geçiş işlemi tamamlanmıştır ve sonraki adıma atlayabilirsiniz. Birden çok aygıt hizmete kayıtlı varsa, 3. adıma geçin.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>3. adım: hizmet verileri şifreleme anahtarı başka StorSimple cihazlar üzerinde güncelleştir
StorSimple Yöneticisi hizmetinize kayıtlı birden çok aygıt varsa, bu adımları StorSimple Cihazınızı Windows PowerShell arabiriminde gerçekleştirilmesi gerekir. 2. adımda elde edilen anahtar: hizmet verileri şifreleme anahtarı değişikliğini başlatmak StorSimple için Windows PowerShell'i kullanın, StorSimple Yöneticisi hizmetine kayıtlı tüm kalan StorSimple Cihazınızı güncelleştirmek için kullanılmalıdır.

Hizmet verileri şifreleme aygıtınızda güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-the-service-data-encryption-key"></a>Hizmet verileri şifreleme anahtarı güncelleştirmek için
1. StorSimple için Windows PowerShell konsoluna bağlanmak için kullanın. Tam erişimle oturum açmak için 1 seçeneğini belirleyin.
2. Komut istemine yazın:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Makalesinde aldığınız hizmet verileri şifreleme anahtarı sağlayan [2. adım: hizmet verileri şifreleme anahtarı değişikliğini başlatmak StorSimple için Windows PowerShell'i kullanın](#to-initiate-the-service-data-encryption-key-change).

