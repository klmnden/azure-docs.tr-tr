## <a name="prepare-for-akv-integration"></a>AKV tümleştirme için hazırlama
SQL Server VM'nize yapılandırmak için Azure anahtar kasası tümleştirmeyi kullanmak için birkaç önkoşul vardır: 

1. [Azure PowerShell'i yükleme](#install-azure-powershell)
2. [Bir Azure Active Directory oluşturun](#create-an-azure-active-directory)
3. [Bir anahtar kasası oluşturma](#create-a-key-vault)

Aşağıdaki bölümlerde bu önkoşulları ve daha sonra PowerShell cmdlet'lerini çalıştırmak için toplamanız gereken bilgiler açıklanmaktadır.

### <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
En son Azure PowerShell SDK'sı yüklediğinizden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

### <a name="register-an-application-in-your-azure-active-directory"></a>Bir uygulamayı Azure Active Directory'yi kaydetme
İlk olarak, şunlara sahip olmanız gerekir bir [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) aboneliğinizde. Birçok avantaj arasında anahtar kasanızı belirli kullanıcılar ve uygulamalar için izni olanak sağlar.

Ardından, bir uygulamanın AAD ile kaydedin. Bu, VM ihtiyaç duyacağı anahtar kasanızı erişimi olan bir hizmet sorumlusu hesabı verir. Azure anahtar kasası makalede, bu adımları bulabilirsiniz [bir uygulamayı Azure Active Directory ile kaydetme](../articles/key-vault/key-vault-get-started.md#register) bölüm veya ekran görüntüleri ile adımları görebilirsiniz **uygulama bölümü için bir kimlik alma**  , [bu blog gönderisine](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Bu adımları gerçekleştirmeden önce SQL VM üzerinde Azure anahtar kasası tümleştirmeyi etkinleştirdiğinizde, daha sonra gerekli bu kaydı sırasında aşağıdaki bilgileri toplamak gerektiğini unutmayın.

* Uygulama eklendikten sonra bulma **uygulama kimliği** üzerinde **kayıtlı uygulama** dikey.   
    Uygulama Kimliği daha sonra atanan **$spName** Azure anahtar kasası tümleştirmeyi etkinleştirmek için PowerShell betiğini parametresinde (hizmet asıl adı). 
* Anahtarınızı oluşturduğunuzda, bu adımları sırasında gizli anahtarınız için aşağıdaki ekran görüntüsünde gösterildiği gibi kopyalayın. Bu anahtar sırrı daha sonra atanan **$spSecret** PowerShell Betiği parametresinde (hizmet sorumlusu gizli).  

* Uygulama Kimliğini ve parolasını SQL Server'da bir kimlik bilgisi oluşturmak için de kullanılır. 

* Bu yeni istemci kimliği aşağıdaki erişim izinlerine sahip yetkilendirmeniz gerekir: **şifrelemek**, **şifresini**, **wrapKey**, **unwrapKey**, **oturum**, ve **doğrulayın**. Bu gerçekleştirilir [kümesi AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) cmdlet'i. Daha fazla bilgi için bkz: [anahtar veya gizli kullanmak için uygulamayı yetkilendirmeniz](../articles/key-vault/key-vault-get-started.md#authorize).

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Azure anahtar kasası, VM şifreleme için kullanacağınız anahtarları depolamak için kullanmak için bir anahtar kasası erişim gerekir. Anahtar kasanız zaten ayarlamadıysanız içindeki adımları izleyerek oluşturun [Azure anahtar kasası ile çalışmaya başlama](../articles/key-vault/key-vault-get-started.md) konu. Bu adımları gerçekleştirmeden önce SQL VM üzerinde Azure anahtar kasası tümleştirmeyi etkinleştirdiğinizde, bu kurulum sırasında toplamanız gereken bazı bilgiler daha sonra gerekli olduğuna dikkat edin.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Bir anahtar kasası adımı Oluştur aldığınızda, döndürülen Not **vaultUri** anahtar kasası URL'si özelliği. Aşağıda, anahtar kasası adıdır ContosoKeyVault, bu adımda sağlanan örnekte bu nedenle anahtar kasası URL'si olacaktır https://contosokeyvault.vault.azure.net/.

Anahtar kasası URL'si daha sonra atanan **$akvURL** Azure anahtar kasası tümleştirmeyi etkinleştirmek için PowerShell komut parametresi.

Anahtar kasası oluşturulduktan sonra anahtar Kasası'na bir anahtar eklemek ihtiyacımız, bu anahtar olacaktır biz bir asimetrik anahtar oluşturduğunuzda, refered SQL Server'da daha sonra oluşturun.
