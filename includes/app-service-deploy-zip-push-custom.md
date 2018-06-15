## <a name="deployment-customization"></a>Dağıtım özelleştirme

Dağıtım işlemi, anında .zip dosyası hazır Çalıştır uygulama içerdiğini varsayar. Varsayılan olarak, hiçbir özelleştirmeleri çalıştırılır. İle sürekli tümleştirme elde aynı yapı işlemlerini etkinleştirmek için uygulamanızın ayarlarını aşağıdakileri ekleyin:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

.Zip itme dağıtım kullandığınızda, bu ayar olan **false** varsayılan olarak. Varsayılan değer **true** sürekli tümleştirme dağıtımlar için. Ayarlandığında **doğru**, dağıtımla ilgili ayarlarınızı dağıtımı sırasında kullanılır. Uygulama ayarları veya .zip dosyanızın kök dizininde bulunan .deployment yapılandırma dosyasında bu ayarları yapılandırabilirsiniz. Daha fazla bilgi için bkz: [depo ve dağıtım ile ilgili ayarları](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) dağıtım başvurusu.