
1. İçinde **karma bağlantılar** dikey penceresinde, oluşturulan, ardından yeni karma bağlantıyı tıklatarak **dinleyicisi Kur**.
   
    ![Dinleyici Kur'a tıklayın](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. **Karma bağlantı özelliklerini** dikey pencere açılır. Altında **şirket içi karma Bağlantı Yöneticisi**, seçin **indirme ve el ile yapılandırma**indirilen HybridConnectionManager.msi paketi kaydedin ve ağ geçidi bağlantı dizesini kopyalayın.
   
    ![Yüklemek için burayı tıklatın](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Bir yönetici komut isteminden yükleyiciyi başlatmak için aşağıdaki komutu yazın:
   
        start HybridConnectionManager.msi
4. Yükleyici çalıştıktan sonra tıklatın **şimdi değil**, %ProgramFiles%\Microsoft\HybridConnectionManager klasöre göz atın, HCMConfigWizard.exe çalıştırın ve tıklatın **Evet** içinde **kullanıcı hesabı Denetim** iletişim.
5. Daha önce kopyaladığınız karma bağlantı dizesini yapıştırın ve tıklayın **Tamam**. 
   
    ![Yükleme](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Yükleme tamamlandığında tıklayın **Kapat**.
   
    ![Kapat'ı tıklatın](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Üzerinde **karma bağlantılar** dikey penceresinde **durum** sütun şimdi gösterir **bağlı**. 
   
    ![Bağlı durumu](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

