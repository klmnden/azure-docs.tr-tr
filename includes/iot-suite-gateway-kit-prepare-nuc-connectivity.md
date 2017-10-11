## <a name="prepare-your-intel-nuc"></a>Intel NUC hazırlama

Donanım kurulumu tamamlamak için aktarmanız gerekir:

- Intel NUC Seti'nde bulunan güç kaynağı bağlayın.
- Intel NUC Ethernet kablosu kullanarak ağınıza bağlayın.

Intel NUC ağ geçidi cihazınız donanım Kurulumu tamamladınız.

### <a name="sign-in-and-access-the-terminal"></a>Oturum açma ve terminal erişim

Intel NUC terminal ortamda erişmek için iki seçeneğiniz vardır:

- Klavye ve monitör, Intel NUC bağlı varsa, kabuk doğrudan erişebilirsiniz. Varsayılan kimlik bilgileri kullanıcı adı olan **kök** ve parola **kök**.

- Masaüstü makinenizden SSH kullanarak, Intel NUC Kabuğu erişin.

#### <a name="sign-in-with-ssh"></a>Oturum SSH oturum

SSH oturum oturum açmanız, Intel NUC IP adresi gerekiyor. Klavye ve monitör, Intel NUC bağlı varsa, `ifconfig` IP adresini bulmak için komutu. Alternatif olarak, ağınızdaki aygıtların adreslerini listelemek için yönlendiriciniz bağlayın.

Kullanıcı adıyla oturum **kök** ve parola **kök**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>İsteğe bağlı: Intel NUC üzerinde bir klasör paylaşın

İsteğe bağlı olarak, masaüstü ortamınızı Intel NUC üzerinde bir klasör paylaşın isteyebilirsiniz. Bir klasör paylaşımı sağlar, tercih edilen Masaüstü metin düzenleyicisi kullanın (gibi [Visual Studio Code](https://code.visualstudio.com/) veya [Sublime Text](http://www.sublimetext.com/)) kullanmak yerine, Intel NUC dosyalarını düzenlemek için `nano` veya `vi`.

Bir klasörü Windows ile paylaşmak için Samba sunucu üzerinde Intel NUC yapılandırın. Alternatif olarak, Masaüstü makinenizde SFTP istemci ile Intel NUC SFTP sunucusu kullanın.
