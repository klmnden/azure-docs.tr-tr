### <a name="noconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı yok

#### <a name="to-add-additional-address-prefixes"></a>Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. IP adres alanı Ekle *ek adres aralığı Ekle* kutusu.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="to-remove-address-prefixes"></a>Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. Tıklatın **'...'** kaldırmak istediğiniz önek içeren satırı üzerinde.
3. Tıklatın **kaldırmak**.
4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

### <a name="withconnection"></a>Yerel ağ geçidinin IP adresi ön eklerini değiştirmek için - ağ geçidi bağlantısı var

Ağ geçidi bağlantınız varsa ve yerel ağ geçidinizde bulunan IP adresi ön eklerini eklemek veya kaldırmak istiyorsanız aşağıdaki adımları sırasıyla uygulamanız gerekir. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. IP adresi öneklerini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.

#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **bağlantıları**.
2. Tıklatın **...**  her bağlantı için satırda ardından **silmek**.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-address-prefixes"></a>2. Adres öneklerini değiştirme.

Başka adres ön ekleri eklemek için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. IP adres alanı ekleyin.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

Adres ön eklerini kaldırmak için:

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. Tıklatın **...**  kaldırmak istediğiniz önek içeren satırı üzerinde.
3. Tıklatın **kaldırmak**.
4. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="3-recreate-the-connection"></a>3. Bağlantısını yeniden oluşturun.

1. Sanal ağ geçidi için sanal ağınızı gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi olarak **ayarları** 'yi tıklatın **bağlantıları**.
3. Tıklatın **+ Ekle** açmak için **Bağlantı Ekle** dikey.
4. Bağlantınızı yeniden oluşturun.
5. Tıklatın **Tamam** bağlantı oluşturmak için.