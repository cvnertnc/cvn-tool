```markdown
# CVN Aracı Kurulumu ve Sistem İmajı İşlemleri

Bu kılavuz, sağlanan komutları kullanarak sistem imajlarını nasıl kuracağınızı ve işlemleri nasıl gerçekleştireceğinizi açıklamaktadır.

## Gereksinimler

Ubuntu/Linux ortamında aşağıdaki yazılımların yüklü olduğundan emin olun:
- `.br` dosyalarını sıkıştırmak ve açmak için `brotli`
- `sdat2img.py` betiğini çalıştırmak için Python
- Dizinleri karşılaştırmak için `diff`
- Dosyaları kopyalamak ve silmek için `cp` ve `rm`
```

### Adım 1: Sistem İmajlarını Bağlama

Aşağıdaki komutları kullanarak sistem, sistem-mod, ve vendor imajlarını bağlayın:

```bash
cd /mnt/c/Users/KADİR/Desktop/cvn-tool
sudo mount -o loop system.img /mnt/c/Users/KADİR/Desktop/cvn-tool/ID
sudo mount -o loop system-mod.img /mnt/c/Users/KADİR/Desktop/cvn-tool/system-mod
sudo mount -o loop vendor.img /mnt/c/Users/KADİR/Desktop/cvn-tool/vendor
```

### Adım 2: Dizinler Arasında Gezinme ve Karşılaştırma

Bağlanan imajlar arasında gezinebilir ve dizinleri karşılaştırabilirsiniz:

```bash
cd /mnt/c/Users/KADİR/Desktop/cvn-tool/ID
cd /mnt/c/Users/KADİR/Desktop/cvn-tool/system-mod
cd /mnt/c/Users/KADİR/Desktop/cvn-tool/vendor
ls -l /mnt/c/Users/KADİR/Desktop/cvn-tool/system

sudo diff -r /mnt/c/Users/KADİR/Desktop/cvn-tool/system/system/app /mnt/c/Users/KADİR/Desktop/cvn-tool/system-mod/system/app
```

### Adım 3: Dosya Kopyalama

Sistem imajındaki dosyaları aşağıdaki komutlarla kopyalayın:

```bash
sudo mkdir -p /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/
sudo cp -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/system /mnt/c/Users/KADİR/Desktop/cvn-tool
sudo cp -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/system /mnt/c/Users/KADİR/Desktop/cvn-tool/ID
sudo cp -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/data-app /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system
sudo cp -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/bootanimation.zip /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/media
```

### Adım 4: Gereksiz Dosyaları Silme

Aşağıdaki komutları kullanarak belirli uygulamaları ve dosyaları sistem imajından silebilirsiniz:

```bash
sudo rm -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/app/PlayAutoInstallStubApp
sudo rm -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/app/wps_lite
sudo rm -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/product/app/Chrome
sudo rm -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/priv-app/GlobalMinusScreen
sudo rm -rf /mnt/c/Users/KADİR/Desktop/cvn-tool/ID/system/priv-app/GlobalMinusScreenBase
```

### Adım 5: WSL Kapama

WSL'yi kapatmak için:

```bash
wsl --shutdown
```

### Adım 6: Brotli ile Dosya Açma ve Dönüştürme

`brotli` kullanarak sıkıştırılmış dosyaları açın:

```bash
brotli --decompress --output=system.new.dat system.new.dat.br
brotli -d -o system.new.dat system.new.dat.br

brotli --decompress --output=vendor.new.dat vendor.new.dat.br
brotli -d -o vendor.new.dat vendor.new.dat.br
```

### Adım 7: SDAT Dosyalarını IMG Formatına Dönüştürme

Python betiği kullanarak SDAT dosyalarını `.img` formatına dönüştürün:

```bash
python sdat2img.py system.transfer.list system.new.dat system.img
python sdat2img.py vendor.transfer.list vendor.new.dat vendor.img
```

Bu adımları takip ederek sistem imajlarını işleyebilir ve gerekli işlemleri gerçekleştirebilirsiniz.

Bu dosya, adım adım kurulum ve kullanım talimatlarını içermektedir.