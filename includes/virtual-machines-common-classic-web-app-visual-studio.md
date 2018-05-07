

Azure için web uygulaması projesi oluşturduğunuzda, azure'da sanal makine sağlayabilirsiniz. Sanal makine ile ek yazılım yapılandırma veya sanal makine hata ayıklama veya tanılama amaçları için kullanın.

Bir web uygulaması oluşturduğunuzda, bir sanal makine oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'da sırasıyla **dosya** > **yeni** > **proje** > **Web**ve ardından seçin **ASP.NET Web uygulaması** (altında **Visual C#** veya **Visual Basic** düğümler).
2. İçinde **yeni ASP.NET projesi** iletişim kutusunda, istediğiniz web uygulamasının türü seçin ve iletişim kutusunda (sağ alt köşedeki) Azure bölümünde olduğundan emin olun **bulutta Barındır** onay kutusu Seçili (Bu onay kutusunu etiketli **uzak kaynaklar Oluştur** bazı yüklemeler içinde).
   
    ![][0]
3. Bu örneğin Microsoft Azure altında aşağı açılan listesinde seçin **sanal makine (v1)** ve ardından **Tamam** düğmesi.
4. İstenirse Azure oturumu açın. **Sanal makine oluşturma** iletişim kutusu görüntülenir.
   
    ![][2]
5. İçinde **DNS adı** kutusunda, sanal makine için bir ad girin. DNS adı Azure içinde benzersiz olmalıdır. Girdiğiniz ad, kullanılabilir durumda değilse, kırmızı bir ünlem işareti görüntülenir.
6. İçinde **görüntü** listesinde, sanal makine temel görüntü seçin. Standart Azure sanal makine görüntülerini ya da Azure'a yüklediğiniz görüntünüzü herhangi birini seçebilirsiniz.
7. Bırakın **IIS etkinleştirmek ve Web dağıtımı** farklı bir web sunucusuna yüklemek planlamıyorsanız onay kutusu. Web dağıtımı devre dışı bırakırsanız Visual Studio'dan yayımlamak mümkün olmayacaktır. IIS ve Web dağıtımı herhangi özel görüntülerinizi de dahil olmak üzere paketlenmiş Windows Server görüntülerinin ekleyebilirsiniz.
8. İçinde **boyutu** listesinde, sanal makine boyutunu seçin.
9. Bu sanal makine için oturum açma kimlik bilgilerini belirtin. Uzak Masaüstü aracılığıyla makine erişmelerini gerekir çünkü bunları not edin.
10. İçinde **konumu** listesinde, sanal makineyi barındırmak için bir bölge seçin.
11. Tıklatın **Tamam** sanal makine oluşturmaya başlamak için düğmesi. İşlemin ilerlemesini izleyebilirsiniz **çıkış** penceresi.
    
    ![][3]
12. Sanal makine sağladığında, yayımlanan betikleri oluşturulan bir **PublishScripts** çözümünüzdeki düğümü. Yayımlanan betik çalıştırır ve azure'da bir sanal makine sağlar. **Çıkış** penceresi durumunu gösterir. Komut dosyası, sanal makineyi hazırlamak için aşağıdaki eylemleri gerçekleştirir:
    
    * Sanal makine zaten yoksa oluşturur.
    * İle başlayan bir ada sahip bir depolama hesabı oluşturur `devtest`, ancak yalnızca hiç zaten varsa bu tür bir depolama hesabı belirtilen bölgede.
    * Sanal makine için bir kapsayıcı olarak bir bulut hizmeti oluşturur ve web uygulaması için bir web rolü oluşturur.
    * Web dağıtımı sanal makinede yapılandırır.
    * IIS ve ASP.NET sanal makinede yapılandırır.
    
    ![][4]
13. (İsteğe bağlı) Yeni bir sanal makineye bağlanabilir. İçinde **Sunucu Gezgini**, genişletin **sanal makineleri** düğümü, oluşturduğunuz sanal makine için ve kısayol menüsünde düğümü seçin, seçin **Connect ile Uzak Masaüstü'nü**. Alternatif olarak, içinde **Cloud Explorer** seçebileceğiniz **portalında açık** kısayol menüsünde ve orada sanal makineye bağlanın.
    
    ![][5]

## <a name="next-steps"></a>Sonraki adımlar
Oluşturduğunuz yayımlanan betikleri özelleştirmek istiyorsanız, daha ayrıntılı bilgi okuma [geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
