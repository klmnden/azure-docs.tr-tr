Sanal ağınıza dağıtılmış VM’ye, sanal makinenizle bir Uzak Masaüstü Bağlantısı oluşturarak bağlanabilirsiniz. Sanal makinenize bağlanabildiğinizi doğrulamanın en iyi yolu, bilgisayar yerine özel IP adresini kullanarak bağlantı kurmaktır. Bu yolla, ad çözünürlüğünün düzgün şekilde yapılandırılıp yapılandırılmadığını değil, bağlantı kurup kuramadığınızı test edersiniz.

1. Özel IP adresini bulun. Bir VM'nin özel IP adresini bulmanın birden çok yolu vardır. Aşağıda Azure portalına ve PowerShell'e yönelik adımları gösterdik.

  - Azure portalı - Sanal makinenizi Azure portalında bulun. VM’nin özelliklerini görüntüleyin. Özel IP adresi listelenir.

  - PowerShell - Örneği kullanarak kaynak gruplarınızdaki VM’lerin ve özel IP adreslerinin listesini görüntüleyin. Bu örneği kullanmadan önce değiştirmeniz gerekmez.

    ```powershell
    $VMs = Get-AzureRmVM
    $Nics = Get-AzureRmNetworkInterface | Where VirtualMachine -ne $null

    foreach($Nic in $Nics)
    {
      $VM = $VMs | Where-Object -Property Id -eq $Nic.VirtualMachine.Id
      $Prv = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAddress
      $Alloc = $Nic.IpConfigurations | Select-Object -ExpandProperty PrivateIpAllocationMethod
      Write-Output "$($VM.Name): $Prv,$Alloc"
    }
    ```

2. Sanal ağınıza VPN bağlantısı kullanarak bağlandığınızdan emin olun.
3. Görev çubuğundaki arama kutusuna "RDP" veya "Uzak Masaüstü Bağlantısı" yazıp Uzak Masaüstü Bağlantısı’nı seçerek **Uzak Masaüstü Bağlantısı**’nı açın. Ayrıca PowerShell'de 'mstsc' komutunu kullanarak Uzak Masaüstü Bağlantısı’nı açabilirsiniz. 
4. Uzak Masaüstü Bağlantısı'nda VM’nin özel IP adresini girin. Diğer ayarları düzenlemek için "Seçenekleri Göster" öğesine tıklayabilir, ardından bağlanabilirsiniz.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>VM ile RDP bağlantısı sorunlarını giderme

VPN bağlantınız üzerinden bir sanal makineye bağlanmakta sorun yaşıyorsanız aşağıdaki kontrolleri yapın:

- VPN bağlantınızın başarılı olduğunu doğrulayın.
- VM’nin özel IP adresine bağlandığınızı doğrulayın.
- Bilgisayar adı yerine özel IP adresini kullanarak VM ile bağlantı kurabiliyorsanız, DNS’i düzgün yapılandırdığınızdan emin olun. VM’ler için ad çözümlemesinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [VM'ler için Ad Çözümlemesi](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
- RDP bağlantıları hakkında daha fazla bilgi için bkz. [Bir VM ile Uzak Masaüstü bağlantılarında sorun giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md).