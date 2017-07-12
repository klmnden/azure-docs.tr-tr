
<a id="to-configure-remote-management-on-cloud-appliance" class="xliff"></a>

#### Bulut gerecinde uzaktan yönetimi yapılandırmak için

1. StorSimple Cihaz Yöneticisi hizmetinizde **Cihazlar**’a tıklayın. Hizmete bağlı cihazlar listesinden bulut gerecinizi seçin ve tıklayın.
    ![StorSimple bulut gereci seçme](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage1.png)

2. **Ayarlar > Güvenlik** seçeneğine gidip **Güvenlik ayarları** dikey penceresini açın.

     ![StorSimple güvenlik ayarları](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage2.png)

3. **Uzaktan Yönetim** bölümüne gidin. **Uzaktan yönetim**  kutusuna tıklayın.
     ![StorSimple uzaktan yönetim](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage3.png)

4. **Uzaktan yönetim** dikey penceresinde:

    1. **Uzaktan yönetimi etkinleştir** seçeneğinin etkin olduğundan emin olun.
    2. Varsayılan olan HTTPS üzerinden bağlanmaktır. HTTP kullanarak bağlanmayı seçebilirsiniz. HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir. HTTP’nin etkin olduğundan emin olun.
    3. Dikey pencerenin üstündeki komut çubuğundan **... Daha fazla**’ya ve ardından **Sertifikayı indir**’e tıklayarak bir uzaktan yönetim sertifikası indirin. Bu dosyanın kaydedileceği konumu belirtebilirsiniz. Bu sertifika, bulut gerecine bağlanmak için kullandığınız istemci veya konak makineye yüklenmelidir.

        ![Uzaktan yönetim dikey penceresi](./media/storsimple-8000-configure-remote-management-http-device/sca-remote-manage4.png)
5. **Kaydet**’e tıklayın ve sorulduğunda değişiklikleri onaylayın.