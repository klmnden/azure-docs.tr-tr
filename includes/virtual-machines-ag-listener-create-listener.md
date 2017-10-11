Bu adımda, yük devretme kümesi Yöneticisi'ni ve SQL Server Management Studio kullanılabilirlik grubu dinleyicisi el ile oluşturun.

1. Birincil çoğaltmayı barındıran düğümden diğerine yük devretme kümesi Yöneticisi'ni açın.

2. Seçin **ağlar** düğüm ve Not küme ağ adı. Bu ad, PowerShell Betiği $ClusterNetworkName değişkeninde kullanılır.

3. Küme adını genişletin ve ardından **rolleri**.

4. İçinde **rolleri** bölmesinde, kullanılabilirlik grubu adını sağ tıklatın ve ardından **kaynak ekleme** > **istemci erişim noktası**.
   
    ![Kullanılabilirlik grubu için istemci erişim noktası Ekle](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. İçinde **adı** kutusunda, bu yeni dinleyici için bir ad oluşturun, **sonraki** iki kez tıkladıktan sonra **son**.  
    Dinleyici veya kaynağı çevrimiçi bu noktada çıkarır değil.

6. Tıklatın **kaynakları** sekmesini tıklatın ve ardından yeni oluşturduğunuz istemci erişim noktası'nı genişletin. 
    Kümenizdeki her bir küme ağı için IP adresi kaynağı görüntülenir. Bu yalnızca Azure çözümünü ise, yalnızca bir IP adresi kaynak görüntülenir.

7. Aşağıdakilerden birini yapın:
   
   * Karma bir çözüm yapılandırmak için:
     
        a. Şirket içi alt ağa karşılık gelen IP adresi kaynağı sağ tıklayın ve ardından **özellikleri**. Ağ adı ve IP adresi adını not edin.
   
        b. Seçin **statik IP adresi**, kullanılmayan bir IP adresi atayın ve ardından **Tamam**.
 
   * Yalnızca Azure çözümünü yapılandırmak için:

        a. Azure alt ağa karşılık gelen IP adresi kaynağı sağ tıklayın ve ardından **özellikleri**.
       
       > [!NOTE]
       > Dinleyici çakışan bir IP adresi DHCP tarafından seçilen nedeniyle çevrimiçine alınamıyor. daha sonra başarısız olursa, bu Özellikler penceresinde geçerli bir statik IP adresi yapılandırabilirsiniz.
       > 
       > 

       b. Aynı **IP adresi** Özellikler penceresi, değişiklik **IP adresi adı**.  
        Bu ad PowerShell Betiği $IPResourceName değişkeninde kullanılır. Çözümünüzün birden fazla Azure sanal ağı yayılırsa, her IP kaynağı için bu adımı yineleyin.

