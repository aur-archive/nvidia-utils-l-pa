# Maintainer:  Jordan Johnston <triplesquarednine@gmail.com>
# Contributor: Ng Oon-Ee <ngoonee.talk@gmail.com>
# Contributor: Dan Vratil <vratil@progdansoft.com>
# Contributor: James Rayner <iphitus@gmail.com>
# Contributor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-utils-l-pa
pkgver=313.18
pkgrel=1
pkgdesc="NVIDIA beta drivers utilities and libraries."
arch=('i686' 'x86_64')
[ "$CARCH" = "i686"   ] && ARCH=x86 && _srcname=NVIDIA-Linux-x86-${pkgver}
[ "$CARCH" = "x86_64" ] && ARCH=x86_64 && _srcname=NVIDIA-Linux-x86_64-${pkgver}-no-compat32
url="http://www.nvidia.com/"
depends=('xorg-server' 'pangox-compat')
optdepends=('gtk2: nvidia-settings' 'pkgconfig: nvidia-xconfig')
conflicts=('libgl' 'nvidia-utils' 'opencl-nvidia' 'libcl')
provides=('libgl' "nvidia-utils=${pkgver}" "libcl" "opencl-nvidia")
license=('custom')
install=nvidia.install
#source=("http://us.download.nvidia.com/XFree86/Linux-$ARCH/${pkgver}/${_srcname}.run"
source=("ftp://download.nvidia.com/XFree86/Linux-${ARCH}/${pkgver}/${_srcname}.run"
  	"20-nvidia.conf")
optdepends=('cuda-toolkit: CUDA toolkit with cuda headers'
	    'opencl-headers: OpenCL header files')
backup=('etc/X11/xorg.conf.d/20-nvidia.conf')

[ "$CARCH" = "i686" ] && md5sums=('705e43f37f9de5cf873679691469ce42')
[ "$CARCH" = "x86_64" ] && md5sums=('cd8645ab96a669b4875046214f6797ba')

md5sums=('fa17a260793a38b4b8ae367db2e03b39'
         '646e64d99c44eb24b02b28defe182317')

build() {
	cd "${srcdir}"

	if [ -d ${_srcname} ]; then
 	  rm -rf ${_srcname}
	fi	

	# Extract the source
	sh ${_srcname}.run --extract-only
}

package() {

	cd "${srcdir}/${_srcname}"
	# Create install dirs
	mkdir -p "${pkgdir}"/usr/{lib,bin,share/applications,share/pixmaps,share/man/man1}
	mkdir -p "${pkgdir}"/usr/lib/xorg/modules/{extensions,drivers}
	mkdir -p "${pkgdir}/usr/lib/vdpau"
	mkdir -p "${pkgdir}/usr/share/licenses/nvidia"
	mkdir -p "${pkgdir}/etc/OpenCL/vendors"
	mkdir -p "${pkgdir}/etc/X11/xorg.conf.d"

	# Install OpenCL configuration
	install nvidia.icd "${pkgdir}/etc/OpenCL/vendors"

	# Install libraries
	install {libGL,libnvidia-cfg,libnvidia-compiler,libnvidia-glcore,libcuda,tls/libnvidia-tls,libnvidia-wfb,libnvcuvid,libnvidia-ml}.so.${pkgver} "${pkgdir}/usr/lib/"
  	install -m755 libvdpau_nvidia.so.${pkgver} "${pkgdir}/usr/lib/vdpau/"
	install -m755 libOpenCL.so.1.0.0 "${pkgdir}/usr/lib"
	#install {libXvMCNVIDIA.a,libXvMCNVIDIA.so.${pkgver}} "${pkgdir}/usr/lib/"
        install nvidia_drv.so "${pkgdir}/usr/lib/xorg/modules/drivers"
        install libglx.so.${pkgver} "${pkgdir}/usr/lib/xorg/modules/extensions"

	# Install manpages
	install -m644 nvidia-{settings,xconfig,smi}.1.gz ${pkgdir}/usr/share/man/man1/

	# Install license
        install -m644 LICENSE "${pkgdir}/usr/share/licenses/nvidia/"
        ln -s nvidia "${pkgdir}/usr/share/licenses/nvidia-utils"
	
	# Install readme
        install -D -m644 README.txt "${pkgdir}/usr/share/doc/nvidia/README"
	
	# Install .desktop file
	install -m644 nvidia-settings.desktop "${pkgdir}/usr/share/applications/"
	# Fix nvidia .desktop file
	sed -e 's:__UTILS_PATH__:/usr/bin:' -e 's:__PIXMAP_PATH__:/usr/share/pixmaps:' -i "${pkgdir}/usr/share/applications/nvidia-settings.desktop"

	# Install pixmaps
	install -m644 nvidia-settings.png "${pkgdir}/usr/share/pixmaps/"
	
	# Install binaries
	install -m755 nvidia-{settings,xconfig,smi,bug-report.sh} "${pkgdir}/usr/bin/"

	# Create symlinks
	cd ${pkgdir}/usr/lib/
	ln -s libOpenCL.so.1.0.0 libOpenCL.so.1
	ln -s libOpenCL.so.1 libOpenCL.so
	ln -s libGL.so.${pkgver} libGL.so.1
	ln -s libGL.so.${pkgver} libGL.so
	ln -s libnvidia-glcore.so.${pkgver} libnvidia-glcore.so.1
	ln -s libnvidia-glcore.so.${pkgver} libnvidia-glcore.so
	ln -s libnvidia-cfg.so.${pkgver} libnvidia-cfg.so.1
	ln -s libnvidia-cfg.so.${pkgver} libnvidia-cfg.so
	ln -s libnvidia-compiler.so.${pkgver} libnvidia-compiler.so.1
	ln -s libnvidia-compiler.so.${pkgver} libnvidia-compiler.so
	ln -s libnvidia-tls.so.${pkgver} libnvidia-tls.so.1
	ln -s libnvidia-tls.so.${pkgver} libnvidia-tls.so
	ln -s libcuda.so.${pkgver} libcuda.so.1
	ln -s libcuda.so.${pkgver} libcuda.so
	ln -s libvdpau_nvidia.so.${pkgver} vdpau/libvdpau_nvidia.so.1
	ln -s libvdpau_nvidia.so.${pkgver} vdpau/libvdpau_nvidia.so
	ln -s libnvcuvid.so.${pkgver} libnvcuvid.so.1
	ln -s libnvcuvid.so.${pkgver} libnvcuvid.so
	#ln -s libXvMCNVIDIA.so.${pkgver} libXvMCNVIDIA_dynamic.so.1
	ln -s libnvidia-ml.so.${pkgver} libnvidia-ml.so.1
	ln -s libnvidia-ml.so.${pkgver} libnvidia-ml.so
	cd "${pkgdir}/usr/lib/xorg/modules/extensions"
	ln -s libglx.so.${pkgver} libglx.so

	find "${pkgdir}/usr" -type d -exec chmod 755 {} \;
	#chmod 644 "${pkgdir}/usr/lib/libXvMCNVIDIA.a"

	# Install nvidia file for xorg autodection
	install -D -m644 $srcdir/20-nvidia.conf "${pkgdir}/etc/X11/xorg.conf.d/20-nvidia.conf"
}
