## <a name="prepare-your-raspberry-pi"></a>Böğürtlenli Pi hazırlama

### <a name="install-raspbian"></a>Raspbian yükleyin

Raspberry Pi'yi kullanarak ilk kez kullanıyorsanız Seti'nde bulunan SD kart üzerinde NOOBS kullanarak Raspbian işletim sistemini yüklemeniz gerekir. [Raspberry Pi yazılım Kılavuzu] [ lnk-install-raspbian] , Raspberry Pi'yi bir işletim sistemi yüklemeyi açıklar. Bu öğretici, Raspberry Pi'yi Raspbian işletim sisteminin yüklü olduğu varsayılır.

> [!NOTE]
> Dahil SD kart [Raspberry Pi 3 için Microsoft Azure IOT Starter Kit] [ lnk-starter-kits] yüklü NOOBS zaten. Bu kartından Raspberry Pi'yi önyükleme ve Raspbian işletim sistemi yüklemek seçin.

Donanım kurulumu tamamlamak için aktarmanız gerekir:

- Raspberry Pi'yi Seti'nde bulunan güç kaynağı bağlayın.
- Raspberry Pi'yi kit içinde bulunan Ethernet kablosu kullanarak ağınıza bağlayın. Alternatif olarak, ayarlayabilirsiniz [kablosuz bağlantı] [ lnk-pi-wireless] Raspberry Pi'yi için.

Raspberry Pi'yi donanım Kurulumu tamamladınız.

### <a name="sign-in-and-access-the-terminal"></a>Oturum açma ve terminal erişim

Raspberry Pi'yi terminal ortamda erişmek için iki seçeneğiniz vardır:

- Klavye ve monitör, Raspberry Pi'yi bağlı varsa, bir terminal penceresi erişmek için Raspbian GUI kullanabilirsiniz.

- Masaüstü makinenizden SSH kullanarak, Raspberry Pi'yi komut satırında erişin.

#### <a name="use-a-terminal-window-in-the-gui"></a>GUI içinde bir terminal penceresi kullanın

Kullanıcı adı Raspbian için varsayılan kimlik bilgileri olan **PI** ve parola **raspberry**. GUI görev çubuğunda, başlatabilirsiniz **Terminal** gibi bir izleyici arar simgesini kullanarak yardımcı programı.

#### <a name="sign-in-with-ssh"></a>Oturum SSH oturum

SSH, Raspberry Pi'yi komut satırı erişimi için kullanabilirsiniz. Makaleyi [SSH (Secure Shell)] [ lnk-pi-ssh] , Raspberry Pi'yi SSH yapılandırma ve bağlanması açıklar [Windows] [ lnk-ssh-windows] veya [ Linux ve Mac OS][lnk-ssh-linux].

Kullanıcı adıyla oturum **PI** ve parola **raspberry**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>İsteğe bağlı: Raspberry Pi'yi üzerinde bir klasör paylaşın

İsteğe bağlı olarak, masaüstü ortamınızı ile Raspberry Pi'yi üzerinde bir klasör paylaşın isteyebilirsiniz. Bir klasör paylaşımı sağlar, tercih edilen Masaüstü metin düzenleyicisi kullanın (gibi [Visual Studio Code](https://code.visualstudio.com/) veya [Sublime Text](http://www.sublimetext.com/)) kullanmak yerine, Raspberry Pi'yi dosyalarını düzenlemek için `nano` veya `vi`.

Bir klasörü Windows ile paylaşmak için Samba sunucu üzerinde Raspberry Pi'yi yapılandırın. Alternatif olarak, yerleşik kullanın [SFTP](https://www.raspberrypi.org/documentation/remote-access/) masaüstünüzde SFTP istemci ile sunucu.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/