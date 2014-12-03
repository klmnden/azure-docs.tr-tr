1. Azure [Yönetim Portalı](http://manage.windowsazure.com)'nda oturum açın. Henüz aboneliğiniz yoksa [Ücretsiz Deneme](http://www.windowsazure.com/tr-+tr/pricing/free-trial/) teklifine bakın.

2. Pencerenin alt kısmındaki komut çubuğunda **Yeni** seçeneğini tıklayın.

3. **İşlem** bölümünde, **Sanal Makine** öğesini ve ardından **Galeri'den** seçeneğini tıklayın.

	![Navigate to From Gallery in the Command Bar](./media/virtual-machines-create-WindowsVM/fromgallery.png)
	
4. Birinci ekran, Görüntü Galerisi'ndeki listelerden birini kullanarak sanal makineniz için **Bir Görüntü Seçme** olanağı sunar. (Kullanılabilir görüntüler, kullanmakta olduğunuz aboneliğe göre değişebilir.) Devam etmek için oku tıklayın.

	![Choose an image](./media/virtual-machines-create-WindowsVM/chooseimage.png)

5. İkinci ekran, bilgisayar adını, boyutu, yönetici kullanıcı adını ve parolasını seçmenize olanak tanır. Azure Sanal Makineler'i yalnızca denemek istiyorsanız, alanları aşağıdaki resimde gösterildiği gibi doldurun. Aksi halde, uygulamanızı veya iş yükünüzü çalıştırmak için gerekli olan katmanı ve boyutu seçin. Bu alanı doldurmanıza yardımcı olacak bazı ayrıntılar şunlardır: 
	
	- **Yeni Kullanıcı Adı**, sunucuyu yönetmek için kullandığınız yönetici hesabına karşılık gelir. Bu hesap için benzersiz bir parola oluşturun ve bu parolayı unutmayın. **Kullanıcı adı ve parola, sanal makinede oturum açarken kullanılacaktır.**. 

	- Bir sanal makinenin boyutu, ekleyebileceğiniz veri diski sayısı gibi yapılandırma seçeneklerinin yanı sıra kullanım maliyetlerine de etki eder. Ayrıntılar için bkz: [Azure için Sanal Makine ve Bulut Hizmeti Boyutları](http://go.microsoft.com/fwlink/p/?LinkId=466520).

	![Configure the properties of the virtual machine](./media/virtual-machines-create-WindowsVM/vmconfiguration.png)



6. Üçüncü ekran, ağ, depolama ve kullanılabilirlik kaynaklarını yapılandırmanıza olanak tanır. Bu alanı doldurmanıza yardımcı olacak bazı ayrıntılar şunlardır: 
	

	- **Bulut Hizmeti DNS Adı**, sanal makineyle iletişim kurmak için kullanılan URI'nin bir parçası haline gelen küresel DNS adıdır. Azure'da kullanacağınız benzersiz bir bulut hizmeti adı belirlemeniz gerekir. Bulut hizmetleri, [birden çok sanal makine](http://www.windowsazure.com/tr-+tr/documentation/articles/cloud-services-connect-virtual-machine/) kullanan senaryolar için önemlidir.
 
	- **Bölge/Benzeşim Grubu/Sanal Ağ** için, konumunuza uygun bir bölge kullanın. Bunun yerine bir sanal ağ da belirtebilirsiniz.
 
	>[WACOM.NOTE] Sanal makinenin bir sanal ağı kullanmasını istiyorsanız, sanal makineyi oluştururken sanal ağı da belirtmeniz **gerekir**. VM'i oluşturduktan sonra, sanal makineyi bir sanal ağa dahil edemezsiniz. Daha fazla bilgi için [Azure Sanal Ağ'a Genel Bakış](http://go.microsoft.com/fwlink/p/?LinkID=294063) bölümüne bakın.

	- Uç noktaları yapılandırmayla ilgili ayrıntılar için, [Bir Sanal Makinede Uç Noktaları Ayarlama](http://www.windowsazure.com/tr-+tr/documentation/articles/virtual-machines-set-up-endpoints/) bölümüne bakın.

	![Configure the connected resources of the virtual machine](./media/virtual-machines-create-WindowsVM/resourceconfiguration.png)

7. Dördüncü yapılandırma ekranı, VM Aracısı'nı ve kullanılabilir uzantılardan bazılarını yapılandırmanıza olanak tanır. Sanal makineyi oluşturmak için onay işaretine tıklayın.


	![Configure VM Agent and extensions for the virtual machine](./media/virtual-machines-create-WindowsVM/agent-and-extensions.png)

	>[WACOM.NOTE] VM aracısı, sanal makineyi yönetmenize veya sanal makine ile etkileşim kurmanıza yardımcı olabilecek uzantıları yüklemeniz için gerekli ortamı sağlar. Ayrıntılar için, [Uzantıları Kullanma](http://go.microsoft.com/FWLink/p/?LinkID=390493) bölümüne bakın.  
    
8. Sanal makine oluşturulduktan sonra, Yönetim Portalı yeni sanal makineyi **Sanal Makineler** bölümünde listeler. Ayrıca, ilgili bulut hizmeti ve depolama hesabı da oluşturulur ve bu bölümlerde listelenir. Sanal makineler de, bulut hizmetleri de otomatik olarak başlatılır ve Yönetim Portalı'nda durumları **Çalışıyor** olarak görünür. 

	![Configure VM Agent and the endpoints of the virtual machine](./media/virtual-machines-create-WindowsVM/vmcreated.png)
