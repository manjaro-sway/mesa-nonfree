# Maintainer: Jan de Groot <jgc@archlinux.org>
# Maintainer: Andreas Radke <andyrtr@archlinux.org>

pkgbase=mesa
pkgname=('opencl-mesa' 'ati-dri' 'intel-dri' 'nouveau-dri' 'svga-dri' 'mesa' 'mesa-libgl')
pkgver=10.2.0rc3
pkgrel=2
arch=('i686' 'x86_64')
makedepends=('python2' 'libxml2' 'libx11' 'glproto' 'libdrm' 'dri2proto' 'dri3proto' 'presentproto' 
             'libxshmfence' 'libxxf86vm'  'libxdamage' 'libvdpau' 'wayland' 'elfutils' 'llvm' 'systemd'
             'libomxil-bellagio' 'libclc' 'clang')
url="http://mesa3d.sourceforge.net"
license=('custom')
#source=(ftp://ftp.freedesktop.org/pub/mesa/${pkgver}/MesaLib-${pkgver}.tar.bz2
source=(ftp://ftp.freedesktop.org/pub/mesa/10.2/MesaLib-10.2.0-rc3.tar.bz2
        LICENSE)
md5sums=('266264483d5f1e3513f48f95392c15a2'
         '5c65a0fe315dd347e09b1f2826a1df5a')

build() {
  cd ${srcdir}/?esa-*

  autoreconf -vfi # our automake is far too new for their build system :)

  ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --with-dri-driverdir=/usr/lib/xorg/modules/dri \
    --with-gallium-drivers=r300,r600,radeonsi,nouveau,svga,swrast \
    --with-dri-drivers=i915,i965,r200,radeon,nouveau,swrast \
    --with-egl-platforms=x11,drm,wayland \
    --enable-llvm-shared-libs \
    --enable-egl \
    --disable-gallium-egl \
    --enable-gbm \
    --enable-gallium-gbm \
    --enable-gallium-llvm \
    --enable-shared-glapi \
    --enable-glx-tls \
    --enable-dri \
    --enable-glx \
    --enable-osmesa \
    --enable-gles1 \
    --enable-gles2 \
    --enable-texture-float \
    --enable-xa \
    --enable-vdpau \
    --enable-dri3 \
    --enable-omx \
    --enable-opencl --enable-opencl-icd \
    --with-clang-libdir=/usr/lib

    # --help
    # --disable-gallium-egl : not well maintained/expose more bugs, so prefer egl_dri2 (FS#40096)

  make

  # fake installation
  mkdir $srcdir/fakeinstall
  make DESTDIR=${srcdir}/fakeinstall install
}

package_opencl-mesa() {
  pkgdesc="OpenCL support for AMD/ATI Radeon mesa drivers"
  depends=('mesa-libgl' "mesa=${pkgver}" 'libcl' 'libclc' 'clang')
  optdepends=('opencl-headers: headers necessary for OpenCL development')
  
  install -m755 -d ${pkgdir}/etc
  mv -v ${srcdir}/fakeinstall/etc/OpenCL ${pkgdir}/etc/
  
  install -m755 -d ${pkgdir}/usr/lib/
  mv -v ${srcdir}/fakeinstall/usr/lib/lib*OpenCL* ${pkgdir}/usr/lib/

  install -m755 -d "${pkgdir}/usr/share/licenses/opencl-mesa"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/opencl-mesa/"
}

package_ati-dri() {
  pkgdesc="Mesa drivers for AMD/ATI Radeon"
  depends=('mesa-libgl' 'libtxc_dxtn' 'libomxil-bellagio' "mesa=${pkgver}")
  conflicts=('xf86-video-ati<6.9.0-6')

  install -m755 -d ${pkgdir}/usr/lib/vdpau/
  mv -v ${srcdir}/fakeinstall/usr/lib/vdpau/libvdpau_{r600,radeonsi}.* ${pkgdir}/usr/lib/vdpau/

  install -m755 -d ${pkgdir}/usr/lib/xorg/modules/dri
  mv -v ${srcdir}/fakeinstall/usr/lib/xorg/modules/dri/{r200,r300,r600,radeon,radeonsi}_dri.so ${pkgdir}/usr/lib/xorg/modules/dri/
  
  install -m755 -d ${pkgdir}/usr/lib/gallium-pipe
  mv -v ${srcdir}/fakeinstall/usr/lib/gallium-pipe/pipe_{r300,r600,radeonsi}* ${pkgdir}/usr/lib/gallium-pipe/
  
  # I don't think it's currently useful to package libomx_r600
  install -m755 -d ${pkgdir}/usr/lib/bellagio
  # mv -v ${srcdir}/fakeinstall/usr/lib/bellagio/libomx_{r600,radeonsi}.so ${pkgdir}/usr/lib/bellagio/
  mv -v ${srcdir}/fakeinstall/usr/lib/bellagio/libomx_radeonsi.so ${pkgdir}/usr/lib/bellagio/

  install -m755 -d "${pkgdir}/usr/share/licenses/ati-dri"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/ati-dri/"
}

package_intel-dri() {
  pkgdesc="Mesa drivers for Intel"
  depends=('mesa-libgl' 'libtxc_dxtn' "mesa=${pkgver}")

  install -m755 -d ${pkgdir}/usr/lib/xorg/modules/dri
  mv -v ${srcdir}/fakeinstall/usr/lib/xorg/modules/dri/{i915,i965}_dri.so ${pkgdir}/usr/lib/xorg/modules/dri/

  install -m755 -d "${pkgdir}/usr/share/licenses/intel-dri"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/intel-dri/"
}

package_nouveau-dri() {
  pkgdesc="Mesa drivers for Nouveau"
  depends=('mesa-libgl' 'libtxc_dxtn' "mesa=${pkgver}")

  install -m755 -d ${pkgdir}/usr/lib/vdpau/
  mv -v ${srcdir}/fakeinstall/usr/lib/vdpau/libvdpau_nouveau.* ${pkgdir}/usr/lib/vdpau/

  install -m755 -d ${pkgdir}/usr/lib/xorg/modules/dri
  mv -v ${srcdir}/fakeinstall/usr/lib/xorg/modules/dri/nouveau_{dri,vieux_dri}.so ${pkgdir}/usr/lib/xorg/modules/dri/
  
  install -m755 -d ${pkgdir}/usr/lib/gallium-pipe
  mv -v ${srcdir}/fakeinstall/usr/lib/gallium-pipe/pipe_nouveau* ${pkgdir}/usr/lib/gallium-pipe/

  # I don't think it's currently useful to package it
  #install -m755 -d ${pkgdir}/usr/lib/bellagio
  #mv -v ${srcdir}/fakeinstall/usr/lib/bellagio/libomx_nouveau.so ${pkgdir}/usr/lib/bellagio/
  
  install -m755 -d "${pkgdir}/usr/share/licenses/nouveau-dri"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/nouveau-dri/"
}

package_svga-dri() {
  pkgdesc="Gallium3D VMware guest GL driver"
  depends=('libdrm' 'expat' 'llvm-libs')

  install -m755 -d ${pkgdir}/usr/lib/xorg/modules/dri
  mv -v ${srcdir}/fakeinstall/usr/lib/xorg/modules/dri/vmwgfx_dri.so ${pkgdir}/usr/lib/xorg/modules/dri/

  install -m755 -d ${pkgdir}/usr/lib/gallium-pipe
  mv -v ${srcdir}/fakeinstall/usr/lib/gallium-pipe/pipe_vmwgfx* ${pkgdir}/usr/lib/gallium-pipe/
  
  install -m755 -d "${pkgdir}/usr/share/licenses/svga-dri"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/svga-dri/"
}

package_mesa() {
  pkgdesc="an open-source implementation of the OpenGL specification"
  depends=('libdrm' 'libvdpau' 'wayland' 'libxxf86vm' 'libxdamage' 'libxshmfence' 'systemd' 'elfutils' 'llvm-libs')
  optdepends=('opengl-man-pages: for the OpenGL API man pages')
  provides=('libglapi' 'osmesa' 'libgbm' 'libgles' 'libegl' 'khrplatform-devel')
  conflicts=('libglapi' 'osmesa' 'libgbm' 'libgles' 'libegl' 'khrplatform-devel')
  replaces=('libglapi' 'osmesa' 'libgbm' 'libgles' 'libegl' 'khrplatform-devel')

  install -m755 -d ${pkgdir}/etc
  mv -v ${srcdir}/fakeinstall/etc/drirc ${pkgdir}/etc
  
  install -m755 -d ${pkgdir}/usr/lib/{gallium-pipe,gbm}
  mv -v ${srcdir}/fakeinstall/usr/lib/lib{OSMesa,gbm,glapi,wayland-egl,xatracker}.so* ${pkgdir}/usr/lib/
  mv -v ${srcdir}/fakeinstall/usr/lib/gallium-pipe/pipe_swrast* ${pkgdir}/usr/lib/gallium-pipe/
  mv -v ${srcdir}/fakeinstall/usr/lib/gbm/gbm_gallium_drm* ${pkgdir}/usr/lib/gbm/

  mv -v ${srcdir}/fakeinstall/usr/include ${pkgdir}/usr
  mv -v ${srcdir}/fakeinstall/usr/lib/pkgconfig ${pkgdir}/usr/lib/
  
  install -m755 -d ${pkgdir}/usr/lib/xorg/modules/dri
  mv -v ${srcdir}/fakeinstall/usr/lib/xorg/modules/dri/swrast_dri* ${pkgdir}/usr/lib/xorg/modules/dri/
  
  install -m755 -d ${pkgdir}/usr/lib/mesa
  # move libgl/EGL/glesv*.so to not conflict with blobs - may break .pc files ?
  mv -v ${srcdir}/fakeinstall/usr/lib/libGL.so* 	${pkgdir}/usr/lib/mesa/
  mv -v ${srcdir}/fakeinstall/usr/lib/libEGL.so* 	${pkgdir}/usr/lib/mesa/
  mv -v ${srcdir}/fakeinstall/usr/lib/libGLES*.so*	${pkgdir}/usr/lib/mesa/

  install -m755 -d "${pkgdir}/usr/share/licenses/mesa"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/mesa/"
}

package_mesa-libgl() {
  pkgdesc="Mesa 3-D graphics library"
  depends=("mesa=${pkgver}")
  provides=("libgl=${pkgver}")
  replaces=('libgl')
 
  # See FS#26284
  install -m755 -d "${pkgdir}/usr/lib/xorg/modules/extensions"
  ln -s libglx.xorg "${pkgdir}/usr/lib/xorg/modules/extensions/libglx.so"

  ln -s /usr/lib/mesa/libGL.so.1.2.0 ${pkgdir}/usr/lib/libGL.so.1.2.0
  ln -s libGL.so.1.2.0	             ${pkgdir}/usr/lib/libGL.so.1
  ln -s libGL.so.1.2.0               ${pkgdir}/usr/lib/libGL.so

  ln -s /usr/lib/mesa/libEGL.so.1.0.0 ${pkgdir}/usr/lib/libEGL.so.1.0.0
  ln -s libEGL.so.1.0.0	              ${pkgdir}/usr/lib/libEGL.so.1
  ln -s libEGL.so.1.0.0               ${pkgdir}/usr/lib/libEGL.so

  ln -s /usr/lib/mesa/libGLESv1_CM.so.1.1.0 ${pkgdir}/usr/lib/libGLESv1_CM.so.1.1.0
  ln -s libGLESv1_CM.so.1.1.0	            ${pkgdir}/usr/lib/libGLESv1_CM.so.1
  ln -s libGLESv1_CM.so.1.1.0               ${pkgdir}/usr/lib/libGLESv1_CM.so

  ln -s /usr/lib/mesa/libGLESv2.so.2.0.0 ${pkgdir}/usr/lib/libGLESv2.so.2.0.0
  ln -s libGLESv2.so.2.0.0               ${pkgdir}/usr/lib/libGLESv2.so.2
  ln -s libGLESv2.so.2.0.0               ${pkgdir}/usr/lib/libGLESv2.so

  install -m755 -d "${pkgdir}/usr/share/licenses/mesa-libgl"
  install -m644 "${srcdir}/LICENSE" "${pkgdir}/usr/share/licenses/mesa-libgl/"
}
