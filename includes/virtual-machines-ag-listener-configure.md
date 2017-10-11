Kullanılabilirlik grubu dinleyicisi SQL Server kullanılabilirlik grubu dinlediği bir IP adresi ve ağ adı değil. Kullanılabilirlik grubu dinleyicisi oluşturmak için aşağıdakileri yapın:

1. <a name="getnet"></a>Küme ağ kaynağı adını alın.

    a. Birincil çoğaltmayı barındıran Azure sanal makine için bağlanmak için RDP kullanın. 

    b. Yük Devretme Kümesi Yöneticisi'ni açın.

    c. Seçin **ağlar** düğümü ve Not küme ağ adı. Bu adı kullanmak `$ClusterNetworkName` PowerShell Betiği değişkeni. Aşağıdaki görüntüde küme ağ adı olan **küme ağ 1**:

   ![Küme ağ adı](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>İstemci erişim noktası ekleyin.  
    İstemci erişim noktası bir kullanılabilirlik grubuna veritabanlarına bağlanmak için uygulamaları kullanan ağ adıdır. İstemci erişim noktası, yük devretme kümesi Yöneticisi'nde oluşturun.

    a. Küme adını genişletin ve ardından **rolleri**.

    b. İçinde **rolleri** bölmesinde, kullanılabilirlik grubu adını sağ tıklatın ve ardından **kaynak ekleme** > **istemci erişim noktası**.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. İçinde **adı** kutusunda, bu yeni dinleyici için bir ad oluşturun. 
   Yeni dinleyici uygulamaları SQL Server kullanılabilirlik grubundaki veritabanlarına bağlanmak için kullandığınız ağ adı adıdır.
   
    d. Dinleyiciyi oluşturmayı tamamlamak için tıklatın **sonraki** iki kez tıkladıktan sonra **son**. Dinleyici veya kaynağı çevrimiçi bu noktada çıkarır değil.

3. <a name="congroup"></a>IP kaynağı kullanılabilirlik grubu için yapılandırın.

    a. Tıklatın **kaynakları** sekmesini tıklatın ve ardından oluşturduğunuz istemci erişim noktası'nı genişletin.  
    İstemci erişim noktası çevrimdışıdır.

   ![İstemci erişim noktası](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. IP kaynağı sağ tıklayın ve ardından Özellikler'i tıklatın. IP adresini not alın ve bunu kullanmak `$IPResourceName` PowerShell Betiği değişkeni.

    c. Altında **IP adresi**, tıklatın **statik IP adresi**. IP adresi Azure portalında yük dengeleyici adresi ayarlarken kullandığınız aynı adresi olarak ayarlayın.

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>SQL Server kullanılabilirlik grubu kaynağı istemci erişim noktasında bağımlı hale getirin.

    a. Yük Devretme Kümesi Yöneticisi'nde **rolleri**, kullanılabilirlik grubunuzun'a tıklayın.

    b. Üzerinde **kaynakları** sekmesinde, altında **diğer kaynakları**kullanılabilirlik kaynak grubuna sağ tıklayın ve ardından **özellikleri**. 

    c. Bağımlılıklar sekmesinde istemci erişim noktası (dinleyici) kaynağın adını ekleyin.

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. **Tamam** düğmesine tıklayın.

5. <a name="listname"></a>İstemci erişim noktası kaynak IP adresine bağımlı olun.

    a. Yük Devretme Kümesi Yöneticisi'nde **rolleri**, kullanılabilirlik grubunuzun'a tıklayın. 

    b. Üzerinde **kaynakları** sekmesinde, istemci erişim noktası kaynağa altında sağ **sunucu adı**ve ardından **özellikleri**. 

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Tıklatın **bağımlılıkları** sekmesi. IP adresi bir bağımlılığı olduğundan emin olun. Değilse, bir bağımlılık IP adresi ayarlayın. Listelenen birden fazla kaynak varsa, IP adreslerini veya değil olduğunu doğrulayın ve, bağımlılıkları. **Tamam** düğmesine tıklayın. 

   ![IP kaynağı](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Dinleyici adına sağ tıklayın ve ardından **çevrimiçine**. 

    >[!TIP]
    >Bağımlılıkların doğru bir şekilde yapılandırıldığını doğrulayabilirsiniz. Yük Devretme Kümesi Yöneticisi'nde, kullanılabilirlik grubu, rollere sağ tıklayın, Git **diğer eylemler**ve ardından **bağımlılık raporunu göster**. Bağımlılıklar doğru şekilde yapılandırıldığında, ağ adı üzerinde kullanılabilirlik grubu bağlıdır ve ağ adı IP adresine bağımlı. 


6. <a name="setparam"></a>Küme parametreleri PowerShell'de ayarlayın.
    
    a. Aşağıdaki PowerShell komut dosyası, SQL Server örnekleri birine kopyalayın. Değişkenleri ortamınız için güncelleştirin.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Küme düğümlerinden birinin PowerShell betiğini çalıştırarak küme parametreleri ayarlayın.  

    > [!NOTE]
    > SQL Server örnekleri farklı bölgelerde bulunuyorsa, PowerShell Betiği iki kez çalıştırmanız gerekir. İlk kez kullanmak `$ILBIP` ve `$ProbePort` ilk bölgesinden. İkinci kez kullanmak `$ILBIP` ve `$ProbePort` ikinci bölgesinden. Küme ağ adı ve küme IP kaynak adı aynıdır. 
