

## <a name="connect-to-azure-sql-database-using-sql-server-authentication"></a>SQL Server Kimlik Doğrulamasını kullanarak Azure SQL Veritabanı'na bağlanma
Aşağıdaki adımlarda, SSMS ile bir Azure SQL sunucusuna ve veritabanına nasıl bağlanacağınız gösterilmektedir. Sunucunuz ve veritabanınız yoksa oluşturmak için bkz. [Birkaç dakika içinde SQL veritabanı oluşturma](../articles/sql-database/sql-database-get-started.md).

1. Windows arama kutusuna **Microsoft SQL Server Management Studio** yazın ve masaüstü uygulamasına tıklayarak SSMS'yi başlatın.
2. **Sunucuya Bağlan** penceresinde, aşağıdaki bilgileri girin (SSMS zaten çalışıyorsa **Bağlan > Veritabanı Altyapısı**'na tıklayın ve **Sunucuya Bağlan** penceresini açın):
   
   * **Sunucu türü**: Varsayılan veritabanı altyapısıdır; bu değeri değiştirmeyin.
   * **Sunucu adı**: Azure SQL Veritabanı sunucunuzun tam adını şu biçimde girin: *&lt;sunucuadı>*.**veritabanı.windows.net**
   * **Kimlik doğrulama türü**: Bu makalede, **SQL Server Kimlik Doğrulamasını** kullanarak nasıl bağlanacağınız gösterilmiştir. Azure Active Directory ile bağlanma hakkında ayrıntılı bilgi için bkz. [Active Directory tümleşik kimlik doğrulamasını kullanarak bağlanma](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-integrated-authentication), [Active Directory parola kimlik doğrulamasını kullanarak bağlanma](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-password-authentication) ve [Active Directory Evrensel Kimlik Doğrulamasını kullanarak bağlanma](../articles/sql-database/sql-database-ssms-mfa-authentication.md).
   * **Kullanıcı adı**: Sunucu üzerinde bir veritabanına erişimi olan bir kullanıcının (örneğin, sunucuyu oluştururken belirttiğiniz *sunucu yöneticisi*) adını girin . 
   * **Parola**: Belirtilen kullanıcıya ilişkin parolayı (örneğin, sunucuyu oluştururken belirlediğiniz *parola*) girin.
     
       ![SQL Server Management Studio: SQL Database sunucusuna bağlanma](./media/sql-database-sql-server-management-studio-connect-server-principal/connect.png)
3. **Bağlan**'a tıklayın.
4. Varsayılan olarak, yeni sunucularda tanımlı [güvenlik duvarı kuralları](../articles/sql-database/sql-database-firewall-configure.md) yoktur, bu nedenle başlangıçta istemcilerin bağlanmasına izin verilmez. Sunucunuzda, belirli IP adresinizin bağlanmasına izin veren bir güvenlik duvarı kuralı yoksa SSMS, kullanımınız için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturmak isteyip istemediğinizi sorar.
   
    **Oturum aç**'a tıklayın ve sunucu düzeyinde bir güvenlik duvarı kuralı oluşturun. Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturabilmek için Azure yöneticisi olmanız gerekir.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/newfirewallrule.png)
5. Azure SQL veritabanınıza başarıyla bağlandıktan sonra **Nesne Gezgini** açılır ve artık [yönetim görevleri gerçekleştirmek ve veri sorgulamak](../articles/sql-database/sql-database-manage-azure-ssms.md) üzere veritabanınıza erişebilirsiniz.
   
     ![yeni sunucu düzeyinde güvenlik duvarı](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)

## <a name="troubleshoot-connection-failures"></a>Bağlantı hatalarını giderme
Sunucu adında yapılan hatalar ve ağ bağlantısı sorunları, bağlantıların başarısız olmasının en yaygın nedenleri arasında yer alır. <*servername*> değerinin veritabanı değil, sunucu adı olduğunu ve tam sunucu adını girmeniz gerektiğini unutmayın: `<servername>.database.windows.net`

Ayrıca kullanıcı adında ve parolada herhangi bir yazım hatası veya fazladan boşluk olmadığını doğrulayın. (Kullanıcı adları büyük/küçük harfe duyarlı değildir ancak parolalar duyarlıdır.) 

Ayrıca protokol ve bağlantı noktası numarasını, şunun gibi bir sunucu adıyla birlikte açıkça belirleyebilirsiniz: `tcp:servername.database.windows.net,1433`

Ağ bağlantısı sorunları, bağlantı hatalarına ve zaman aşımlarına da neden olabilir. Yeniden bağlanmayı denerseniz (sunucu adının, kimlik bilgilerinin ve güvenlik duvarı kurallarının doğru olduğundan eminseniz) sorun çözülebilir.

Bağlantı sorunları ile ilgili ayrıntılar ve daha fazla bilgi için bkz. [SQL Veritabanı için SQL bağlantı sorunlarını ve geçici sorunları giderme, tanılama ve önleme](../articles/sql-database/sql-database-connectivity-issues.md).



<!--HONumber=Nov16_HO2-->


