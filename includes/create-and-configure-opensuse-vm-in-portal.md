1. [Klasik Azure portalında](http://manage.windowsazure.com) oturum açın.  
2. Pencerenin altındaki komut çubuğunda **yeni**.
3. Altında **işlem**, tıklatın **sanal makine**ve ardından **Galeri'den**.
   
    ![Yeni bir sanal makine oluşturma][Image1]
4. Altında **SUSE** grup, bir OpenSUSE sanal makine görüntüsü seçin ve ardından devam etmek için oka tıklayın.
5. İlk **sanal makine yapılandırması** sayfa:
   
   * Tür a **sanal makine adı**, "testlinuxvm" gibi. Adı gerekir 3 ile 15 karakter arasında içeren, yalnızca harf, rakam ve tire içerebilir ve gerekir bir harfle başlamalı ve harf veya sayı ile bitmelidir.
   * Doğrulama **katmanı** ve çekme bir **boyutu**. Katman aralarından seçim yapabileceğiniz boyutları belirler. Boyutu, yanı sıra gibi kaç tane veri diskleri, yapılandırma seçenekleri ekleyebilirsiniz kullanma maliyetini etkiler. Ayrıntılar için bkz [sanal makineler için Boyutlar](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Tür a **yeni bir kullanıcı adı**, veya varsayılanı kabul **azureuser**. Bu ad Sudoers listesi dosyasına eklenir.
   * Hangi tür karar **kimlik doğrulaması** kullanmak için. Genel parola yönergeler için bkz: [güçlü parolalar](http://msdn.microsoft.com/library/ms161962.aspx).
6. Sonraki **sanal makine yapılandırması** sayfa:
   
   * Varsayılan kullanmak **yeni bir bulut hizmeti oluşturma**.
   * İçinde **DNS adı** "testlinuxvm" gibi adresi bir parçası olarak kullanmak için benzersiz bir DNS adı yazın.
   * İçinde **bölge/benzeşim grubu/sanal ağ** kutusunda, bu sanal görüntü nerede barındırılacağı bir bölge seçin.
   * Altında **uç noktaları**, SSH bitiş noktasını saklayın. Başkalarının şimdi, ekleyin veya eklemek, değiştirmek veya sanal makine oluşturulduktan sonra silebilirsiniz.
     
     > [!NOTE]
     > Bir sanal makinenin bir sanal ağ kullanmasını istiyorsanız, **gerekir** sanal makine oluşturduğunuzda, sanal ağ belirtin. Sanal makineyi oluşturduktan sonra bir sanal makine bir sanal ağa ekleyemezsiniz. Daha fazla bilgi için bkz: [Virtual Network'e genel bakış](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. Son üzerinde **sanal makine yapılandırması** sayfasında, varsayılan ayarları koruyun ve ardından tamamlamak için onay işaretine tıklayın.

Portal altında yeni bir sanal makine listeler **sanal makineleri**. Durum olarak bildirilen sırada **(hazırlama)**, sanal makine ayarlanıyor. Ne zaman durum bildirilir olarak **çalıştıran**, sonraki adıma geçin.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
SSH veya PuTTY bağlanması bilgisayardaki işletim sistemine bağlı olarak sanal makineye bağlanmak için kullanacağınız:

* Linux çalıştıran bir bilgisayardan SSH kullanın. Komut istemine yazın:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Kullanıcının parolasını yazın.
* Windows çalıştıran bir bilgisayardan PuTTY kullanın. Yüklü yoksa, indirin [PuTTY indirme sayfası][PuTTYDownload].
  
    Kaydet **putty.exe** bilgisayarınızdaki bir dizine. Bir komut istemi açın, o klasöre gidin ve Çalıştır **putty.exe**.
  
    "Testlinuxvm.cloudapp.net" gibi ana bilgisayar adını yazıp "22" için **bağlantı noktası**.
  
    ![PuTTY ekran][Image6]  

## <a name="update-the-virtual-machine-optional"></a>Sanal makineyi (isteğe bağlı) güncelleştirin
1. Sanal makineye bağlandıktan sonra sistem güncelleştirmelerini ve düzeltme eklerini isteğe bağlı olarak yükleyebilirsiniz. Güncelleştirmeyi çalıştırmak için şunu yazın:
   
    `$ sudo zypper update`
2. Seçin **yazılım**, ardından **çevrimiçi güncelleştirme** kullanılabilir güncelleştirmeleri listesi. Seçin **kabul** yüklemeyi başlatmak ve tüm yeni kullanılabilir düzeltme eklerinin (dışında isteğe bağlı olanlar için) uygulamak için.
3. Yükleme tamamlandıktan sonra seçin **son**.  Sisteminizi güncel sunulmuştur.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
