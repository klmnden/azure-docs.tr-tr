1. Sanal ağ geçidinizin dikey penceresine gidip bu dikey pencereyi açın. Buraya çeşitli yollardan gidebilirsiniz. Örneğimizde "VNet1GW" ağ geçidine gitmek için **TestVNet1 -> Genel bakış -> Bağlı cihazlar -> VNet1GW** yolunu kullandık.
2. VNet1GW dikey penceresinde **Bağlantılar**'a tıklayın. Bağlantılar dikey penceresinin üstündeki **+Ekle**’ye tıklayarak **Bağlantı ekle** dikey penceresini açın.

    ![Siteden Siteye bağlantısı oluşturma](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection1.png)

3. **Bağlantı ekle** dikey penceresinde bağlantınızı oluşturmak için değerleri girin.

  - **Ad:** Bağlantınızı adlandırın. Örneğimizde **VNet1toSite2** kullandık.
  - **Bağlantı türü:** **Siteden siteye (IPSec)** seçeneğini belirleyin.
  - **Sanal ağ geçidi:** Bu ağ geçidinden bağlandığınız için değer sabittir.
  - **Yerel ağ geçidi:** **Bir yerel ağ geçidi seçin**'e tıklayıp kullanmak istediğiniz yerel ağ geçidini seçin. Örneğimizde **Site2** kullandık.
  - **Paylaşılan Anahtar:** Buradaki değer, yerel şirket için VPN cihazınız için kullandığınız değerle eşleşmelidir. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
  - **Abonelik**, **Kaynak Grubu** ve **Konum** için kalan değerler sabittir.

4. Bağlantınızı oluşturmak için **Tamam**’a tıklayın. Ekranda yanıp sönen *Bağlantısı Oluşturuluyor* yazısını göreceksiniz.
5. Bağlantıyı sanal ağın **Bağlantılar** dikey penceresinde görüntüleyebilirsiniz. *Bilinmiyor* Durumu *Bağlanıyor* olarak ve ardından *Başarılı* olarak değişir.