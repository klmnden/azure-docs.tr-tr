## <a name="deployment-customization"></a>Dağıtım özelleştirme

Dağıtım işlemi, anında .zip dosyası hazır Çalıştır uygulama içerdiğini varsayar. Varsayılan olarak, hiçbir özelleştirmeleri çalıştırılır. Aşağıdaki uygulama ayarlarınızı ekleyerek ile sürekli tümleştirme elde aynı yapı işlemleri etkinleştirebilirsiniz:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

.Zip itme dağıtım, bu ayar kullanıldığında **false** varsayılan olarak. Varsayılan değer **true** sürekli tümleştirme dağıtımlar için. Ayarlandığında **doğru**, dağıtımla ilgili ayarlarınızı dağıtımı sırasında kullanılır. Bu ayarları uygulama ayarları veya içinde ayarlanabilir bir `.deployment` zip dosyasının kök dizininde bulunan yapılandırma dosyası. Daha fazla bilgi için bkz: [depo ve dağıtım ile ilgili ayarları](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) dağıtım başvurusu.