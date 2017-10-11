### <a name="gwipnoconnection"></a>Yerel ağ geçidi IP adresi - ağ geçidi bağlantı değiştirmek için

Ağ geçidi bağlantısı olmayan bir yerel ağ geçidini değiştirmek için örneği kullanın. Bu değeri değiştirirken aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **yapılandırma**.
2. İçinde **IP adresi** kutusunda, IP adresini değiştirin.
3. Tıklatın **kaydetmek** ayarları kaydetmek için.

### <a name="gwipwithconnection"></a>Var olan ağ geçidi bağlantısı yerel ağ geçidi ağ geçidi IP adresi - değiştirmek için

Bir bağlantısı olan bir yerel ağ geçidi değiştirmek için önce bağlantıyı kaldırmanız gerekir. Bağlantı kaldırıldıktan sonra ağ geçidi IP adresini değiştirebilir ve yeni bir bağlantı oluşturabilirsiniz. Aynı zamanda adres ön eklerini de değiştirebilirsiniz. Bunun sonucunda, VPN bağlantınızda kesinti oluşur. Ağ geçidi IP adresini değiştirirken, VPN ağ geçidini silmeniz gerekmez. Yalnızca bağlantıyı kaldırmanız gerekir.
 
#### <a name="1-remove-the-connection"></a>1. Bağlantıyı kaldırın.

1. Yerel ağ geçidi kaynağına içinde **ayarları** 'yi tıklatın **bağlantıları**.
2. Tıklatın **...**  bağlantı için satırda ardından **silmek**.
3. Tıklatın **kaydetmek** ayarlarınızı kaydetmek için.

#### <a name="2-modify-the-ip-address"></a>2. IP adresini değiştirin.

Aynı zamanda adres ön eklerini de değiştirebilirsiniz.

1. İçinde **IP adresi** kutusunda, IP adresini değiştirin.
2. Tıklatın **kaydetmek** ayarları kaydetmek için.

#### <a name="3-recreate-the-connection"></a>3. Bağlantısını yeniden oluşturun.

1. Sanal ağ geçidi için sanal ağınızı gidin. (Olmayan yerel ağ geçidi.)
2. Sanal ağ geçidi olarak **ayarları** 'yi tıklatın **bağlantıları**.
3. Tıklatın **+ Ekle** açmak için **Bağlantı Ekle** dikey.
4. Bağlantınızı yeniden oluşturun.
5. Tıklatın **Tamam** bağlantı oluşturmak için.