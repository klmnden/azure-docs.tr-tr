### <a name="windows-powershell"></a>Windows PowerShell

#### <a name="install-powershell"></a>PowerShell yükleme
En son PowerShell makinenizde yüklü değilse, yükleyin. 

1. Web tarayıcısında [Azure SDK İndirmeleri ve SDK'lar](https://azure.microsoft.com/downloads/) sayfasına gidin. 
2. **Komut satırı araçları** -> **PowerShell** bölümünde **Windows yüklemesi**'ne tıklayın. 
3. PowerShell’i yüklemek için **MSI** dosyasını çalıştırın. 

Ayrıntılı yönergeler için bkz. [PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>PowerShell’de oturum açın

1. Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.

    ![PowerShell’i başlatın](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız aynı Azure kullanıcı adını ve parolasını girin:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Birden çok Azure aboneliğiniz varsa, bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
