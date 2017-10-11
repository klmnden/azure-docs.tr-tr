## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayın:

- Önceden yapılandırılmış Uzaktan izleme çözümü örneği Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetlerini yapılandırır.
- Bilgisayarınız ve Uzaktan izleme çözümü ile iletişim kurmak için aygıtınızı ayarlayın.
- Uzaktan izleme çözümüne bağlama için örnek aygıt kodunu güncelleştirin ve çözüm panosunda görüntüleyebilirsiniz sanal telemetriyi gönderin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

> [!NOTE]
> Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].

### <a name="required-software"></a>Gerekli yazılım

Komut satırı Raspberry Pi'yi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](http://www.putty.org/).
- Çoğu Linux dağıtımları ve Mac OS komut satırı SSH yardımcı programı içerir. Daha fazla bilgi için bkz: [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-hardware"></a>Gerekli donanım

Komut satırı Raspberry Pi'yi üzerinde uzaktan bağlanmak etkinleştirmek için bir masaüstü bilgisayar.

[Microsoft IOT Starter Kit Raspberry Pi 3] [ lnk-starter-kits] veya eşdeğer bileşenleri. Bu öğretici Seti'nden aşağıdaki öğeleri kullanır:

- Böğürtlenli Pi 3
- MicroSD kartı (ile NOOBS)
- Bir USB Mini kablosu
- Ethernet kablosu

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/