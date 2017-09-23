UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <Veri aktarımı için kullanılacak IP adresi] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametreler:

* /ServerMode: Zorunlu. Hem yapılandırma hem de işlem sunucularının yükleneceğini veya yalnızca işlem sunucusunun yüklenmesi gerektiğini belirtir. Giriş değerleri: CS, PS.
* InstallLocation: Zorunlu. Bileşenlerin yüklendiği klasör.
* /MySQLCredsFilePath. Zorunlu. MySQL sunucusu kimlik bilgilerinin depolandığı dosya yolu. Dosya şu biçimde olmalıdır:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Zorunlu. Kasa kimlik bilgileri dosyasının konumu
* /EnvType. Zorunlu. Yüklemenin türü. Değerler: VMware, NonVMware
* /PSIP ve /CSIP. Zorunlu. İşlem sunucusu ve yapılandırma sunucusunun IP adresi.
* /PassphraseFilePath. Zorunlu. Parola dosyasının konumu.
* /BypassProxy. İsteğe bağlı. Yapılandırma sunucusunun bir ara sunucu olmadan Azure'a bağlandığını belirtir.
* /ProxySettingsFilePath. İsteğe bağlı. Ara sunucu ayarları (Varsayılan ara sunucu kimlik doğrulaması gerektirir ya da özel bir ara sunucu). Dosya şu biçimde olmalıdır:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. İsteğe bağlı. Çoğaltma verileri için bağlantı noktası numarası.
* SkipSpaceCheck. İsteğe bağlı. Önbellek için alan denetimini atlama.
* AcceptThirdpartyEULA. Zorunlu. Üçüncü taraf EULA belgesini kabul eder.
* ShowThirdpartyEULA. Zorunlu. Üçüncü taraf EULA belgesini görüntüler. Giriş olarak sağlanırsa, diğer tüm parametreler göz ardı edilir.
