### 安装
```
# 先安装libhtp
$ cd libhtp
$ ./autogen.sh
$ ./configure --enable-rust=yes --enable-gccmarch-native=no CPPFLAGS=-I/usr/include/ CFLAGS=-g
$ cd ..

# 编译安装suricata
$ ./autogen.sh
$ ./configure --enable-rust=yes --enable-gccmarch-native=no --enable-dpdk=yes --enable-libmagic=yes CPPFLAGS=-I/usr/include/ CFLAGS=-g --sysconfdir=/etc --localstatedir=/var --enable-unittests   # 编译选项
$ make -j8
$ make install full
install -d "/etc/suricata/"
install -d "/var/log/suricata/files"
install -d "/var/log/suricata/certs"
install -d "/var/run/"
install -m 770 -d "/var/run/suricata"
install -m 770 -d "/var/lib/suricata/data"

# 更新suricata-update
$ suricata-update
# 规则文件在 /var/lib/suricata/rules/suricata.rules

# 安装完成后
# sc文件在/usr/local/bin/目录下 (代码目录src/.libs/下也会生成一份)
# 配置文件在/etc/suricata/目录下 (代码目录下也会生成一份)
# 日志文件在/var/log/suricata/目录下
# 启动suricata
$ /usr/local/bin/suricata -c /etc/suricata/suricata.yaml -i enp0s3
# 启动suricata的守护进程
$ /usr/local/bin/suricata -c /etc/suricata/suricata.yaml -i enp0s3 -D
```

#### 可能会遇到的编译问题
```
checking for cbindgen... no
  Warning: cbindgen too old or not found, it is required to
      generate header files.
  To install: cargo install --force cbindgen
configure: error: cbindgen required

# A: 安装 cbindgen
$ cargo install --force cbindgen
```

#### ./configure 完成得到的结果
```
Suricata Configuration:
  AF_PACKET support:                       yes
  AF_XDP support:                          yes
  DPDK support:                            yes
  eBPF support:                            no
  XDP support:                             no
  PF_RING support:                         no
  NFQueue support:                         no
  NFLOG support:                           no
  IPFW support:                            no
  Netmap support:                          no
  DAG enabled:                             no
  Napatech enabled:                        no
  WinDivert enabled:                       no

  Unix socket enabled:                     yes
  Detection enabled:                       yes

  Libmagic support:                        yes
  libjansson support:                      yes
  hiredis support:                         no
  hiredis async with libevent:             no
  PCRE jit:                                yes
  GeoIP2 support:                          no
  JA3 support:                             yes
  JA4 support:                             yes
  Non-bundled htp:                         no
  Hyperscan support:                       no
  Libnet support:                          yes
  liblz4 support:                          yes
  Landlock support:                        yes
  Systemd support:                         yes

  Rust strict mode:                        no
  Rust compiler path:                      /usr/bin/rustc
  Rust compiler version:                   rustc 1.70.0 (90c541806 2023-05-31) (built from a source tarball)
  Cargo path:                              /usr/bin/cargo
  Cargo version:                           cargo 1.70.0

  Python support:                          yes
  Python path:                             /usr/bin/python3
  Install suricatactl:                     yes
  Install suricatasc:                      yes
  Install suricata-update:                 no, not bundled

  Profiling enabled:                       no
  Profiling locks enabled:                 no
  Profiling rules enabled:                 no

  Plugin support (experimental):           yes
  DPDK Bond PMD:                           yes

Development settings:
  Coccinelle / spatch:                     no
  Unit tests enabled:                      yes
  Debug output enabled:                    no
  Debug validation enabled:                no
  Fuzz targets enabled:                    no

Generic build parameters:
  Installation prefix:                     /usr/local
  Configuration directory:                 /etc/suricata/
  Log directory:                           /var/log/suricata/

  --prefix                                 /usr/local
  --sysconfdir                             /etc
  --localstatedir                          /var
  --datarootdir                            /usr/local/share

  Host:                                    x86_64-pc-linux-gnu
  Compiler:                                gcc (exec name) / g++ (real)
  GCC Protect enabled:                     no
  GCC march native enabled:                no
  GCC Profile enabled:                     no
  Position Independent Executable enabled: no
  CFLAGS                                   -g -fPIC -DOS_LINUX -std=c11 -I/usr/include/dpdk -I/usr/include/dpdk/../x86_64-linux-gnu/dpdk -include rte_config.h -march=corei7 -I/usr/include/libnl3 -I/usr/include/dbus-1.0 -I/usr/lib/x86_64-linux-gnu/dbus-1.0/include  -I${srcdir}/../rust/gen -I${srcdir}/../rust/dist -I../rust/gen
  PCAP_CFLAGS                               -I/usr/include
  SECCFLAGS

To build and install run 'make' and 'make install'.

You can run 'make install-conf' if you want to install initial configuration
files to /etc/suricata/. Running 'make install-full' will install configuration
and rules and provide you a ready-to-run suricata.

To install Suricata into /usr/bin/suricata, have the config in
/etc/suricata and use /var/log/suricata as log dir, use:
./configure --prefix=/usr/ --sysconfdir=/etc/ --localstatedir=/var/
```