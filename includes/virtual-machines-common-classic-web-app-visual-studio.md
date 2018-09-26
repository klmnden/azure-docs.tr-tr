

Azure için bir web uygulaması projesi oluşturduğunuzda, Azure'da bir sanal makine sağlayabilirsiniz. Ek yazılım ile sanal makineyi yapılandırın veya sanal makine Tanılama veya hata ayıklama amaçları için kullanın.

Bir web uygulaması oluşturduğunuzda bir sanal makine oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'da **dosya** > **yeni** > **proje** > **Web**seçin **ASP.NET Web uygulaması** (altında **Visual C#** veya **Visual Basic** düğümleri).
2. İçinde **yeni ASP.NET projesi** iletişim kutusunda, istediğiniz web uygulaması türünü seçin ve emin olun (sağ alt köşedeki) iletişim kutusunda Azure bölümünde **bulutta Barındır** onay kutusu Seçili (Bu onay kutusu etiketli **uzak kaynaklar Oluştur** bazı yüklemeler olarak).
   
    ![][0]
3. Bu örnekte, Microsoft Azure, altındaki aşağı açılan listesinde seçin **sanal makine (v1)** ve ardından **Tamam** düğmesi.
4. İstenirse Azure'da oturum açın. **Sanal makine oluştur** iletişim kutusu görüntülenir.
   
    ![][2]
5. İçinde **DNS adı** kutusunda, sanal makine için bir ad girin. DNS adı Azure'da benzersiz olması gerekir. Girdiğiniz ad, kullanılabilir durumda değilse, kırmızı bir ünlem görünür.
6. İçinde **görüntü** listesinde, sanal makine için temel almak istediğiniz görüntüyü seçin. Standart Azure sanal makine görüntüleri veya Azure'a yüklediğiniz görüntünüzü birini seçebilirsiniz.
7. Bırakın **IIS etkinleştirmek ve Web dağıtımı** farklı bir web sunucusuna yüklemeyi planladığınız sürece onay kutusu. Web dağıtımı devre dışı bırakırsanız Visual Studio'dan yayımlama mümkün olmayacaktır. IIS ve Web dağıtımı paketlenmiş Windows Server görüntülerini, kendi özel Görüntüleriniz dahil birine ekleyebilirsiniz.
8. İçinde **boyutu** listesinde, sanal makine boyutu seçin.
9. Bu sanal makine için oturum açma kimlik bilgilerini belirtin. Makinede Uzak Masaüstü aracılığıyla erişmesine gerekeceği için bunları not edin.
10. İçinde **konumu** listesinde, sanal makineyi barındırmak için bir bölge seçin.
11. Tıklayın **Tamam** sanal makine oluşturmaya başlamak için düğme. İşlemin ilerlemesini izleyebilirsiniz **çıkış** penceresi.
    
    ![][3]
12. Sanal makine sağladığında, yayımlanan betikler oluşturulan bir **PublishScripts** çözümünüzdeki düğümü. Yayımlanan betik çalıştıran ve Azure'da bir sanal makine sağlar. **Çıkış** pencere durumu gösterilir. Komut dosyası, sanal makine kurmak için aşağıdaki eylemleri gerçekleştirir:
    
    * Zaten mevcut değilse sanal makine oluşturur.
    * İle başlayan bir ada sahip bir depolama hesabı oluşturur `devtest`, ancak yalnızca yoksa zaten bu tür bir depolama hesabı belirtilen bölgede.
    * Bir bulut hizmeti sanal makine için bir kapsayıcı oluşturur ve bir web rolü web uygulaması oluşturur.
    * Web dağıtımı, sanal makineyi yapılandırır.
    * Sanal makine üzerinde IIS ve ASP.NET yapılandırır.
    
    ![][4]
13. (İsteğe bağlı) Yeni sanal makineye bağlanabilirsiniz. İçinde **Sunucu Gezgini**, genişletme **sanal makineler** düğümü, oluşturduğunuz sanal makine için ve kısayol menüsünde düğümü seçin, **Connect ile Uzak Masaüstü**. Alternatif olarak **Cloud Explorer** seçebileceğiniz **portalında açın** kısayol menüsünde ve orada sanal makineye bağlanın.
    
    ![][5]

## <a name="next-steps"></a>Sonraki adımlar
Oluşturduğunuz yayımlanan betikler özelleştirmek istiyorsanız, daha ayrıntılı bilgi konumunda okuma [yayımlamak için geliştirme ve Test ortamları için Windows PowerShell betiklerini kullanarak](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
