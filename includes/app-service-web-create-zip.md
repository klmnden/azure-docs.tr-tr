## <a name="create-a-project-zip-file"></a>Proje ZIP dosyası oluşturma

Hâlâ örnek projenin kök dizininde bulunduğunuzdan emin olun. Projenizdeki tüm öğeleri içeren bir ZIP arşivi oluşturun. Aşağıdaki komut terminalinizdeki varsayılan aracı kullanmaktadır:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

Daha sonra bu ZIP dosyasını Azure'a yükleyip App Service'te dağıtabilirsiniz.