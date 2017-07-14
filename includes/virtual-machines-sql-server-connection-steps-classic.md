### Sanal makinenin DNS adını belirleme
<a id="determine-the-dns-name-of-the-virtual-machine" class="xliff"></a>
Başka bir bilgisayardan SQL Server Veritabanı Altyapısı’na bağlanmak için, sanal makinenin Etki Alanı Adı Sistemi (DNS) adını biliyor olmalısınız. (İnternet, sanal makineyi tanımlamak için bu adı kullanır. IP adresini kullanabilirsiniz, ancak Azure yedeklilik veya bakım nedeniyle kaynakları taşıdığında IP adresi değişebilir. DNS adı yeni IP adresine yeniden yönlendirilebileceği için değişmez.)  

1. Azure Portal’da (veya önceki adımda), **Sanal makineler (klasik)** öğesini seçin.
2. SQL VM’nizi seçin.
3. **Sanal makine** dikey penceresinde, sanal makineye ilişkin **DNS adı** değerini kopyalayın.
   
    ![DNS adı](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### Başka bir bilgisayardan Veritabanı Altyapısına bağlanma
<a id="connect-to-the-database-engine-from-another-computer" class="xliff"></a>
1. İnternet'e bağlı bir bilgisayarda SQL Server Management Studio’yu açın.
2. **Sunucuya Bağlan** veya **Veritabanı Altyapısına Bağlan** iletişim kutusundaki **Sunucu adı** kutusuna, sanal makinenin DNS adını (önceki görevde belirlenen ad) ve *DNSAdı,bağlantınoktası* biçiminde bir genel uç nokta bağlantı noktası numarası (**mysqlvm.cloudapp.net,57500** gibi) girin.
   
    ![SSMS kullanarak bağlanma](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Daha önce oluşturduğunuz genel uç nokta bağlantı noktası numarasını anımsamıyorsanız, bu numarayı **Sanal makine** dikey penceresinin **Uç noktalar** alanında bulabilirsiniz.
   
    ![Genel Bağlantı Noktası](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. **Kimlik Doğrulaması** kutusunda **SQL Server Kimlik Doğrulaması**’nı seçin.
4. **Oturum Açma** kutusuna, önceki görevlerden birinde oluşturduğunuz oturum açma kimliğinin adını yazın.
5. **Parola** kutusuna, önceki görevlerden birinde oluşturduğunuz oturum açma kimliğinin parolasını yazın.
6. **Bağlan**'a tıklayın.

