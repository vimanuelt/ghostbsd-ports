# Created by: Thierry Thomas <thierry@pompo.net>
# $FreeBSD$

PORTNAME=	v_sim
PORTVERSION=	3.7.2
PORTREVISION=	12
CATEGORIES=	science
MASTER_SITES=	http://inac.cea.fr/sp2m/L_Sim/V_Sim/download/

MAINTAINER=	thierry@FreeBSD.org
COMMENT=	Visualization of atomic structures

LICENSE=	CeCILL
LICENSE_NAME=	CeCILL
LICENSE_FILE=	${WRKSRC}/COPYING
LICENSE_PERMS=	dist-mirror dist-sell pkg-mirror pkg-sell auto-accept

LIB_DEPENDS=	libfontconfig.so:x11-fonts/fontconfig	\
		libfreetype.so:print/freetype2	\
		libftgl.so:graphics/ftgl

USES=		compiler:c11 fortran gl gmake gnome libtool localbase:ldflags \
		pkgconfig tar:bzip2 xorg
USE_XORG=	x11
USE_GNOME=	gtk20 intltool cairo
USE_GL=		gl glu
GNU_CONFIGURE=	yes
CONFIGURE_ARGS=	--disable-gtk-doc --with-html-dir=${PREFIX}/share/doc --disable-python-module
CPPFLAGS+=	${CFLAGS}
USE_LDCONFIG=	yes
INSTALLS_ICONS=	yes
INSTALL_TARGET=	install-strip

OPTIONS_DEFINE=	DOCS NLS EXAMPLES

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MNLS}
USES+=		gettext
PLIST_SUB+=	NLS=""
.else
USES+=		gettext-tools
PLIST_SUB+=	NLS="@comment "
.endif

.if !defined(WITHOUT_CDF)
LIB_DEPENDS+=	libnetcdf.so:science/netcdf
CONFIGURE_ARGS+=--with-netcdf=${LOCALBASE}
PLIST_SUB+=	CDF=""
.else
CONFIGURE_ARGS+=--without-netcdf
PLIST_SUB+=	CDF="@comment "
.endif

.if !defined(WITHOUT_BABEL)
LIB_DEPENDS+=	libopenbabel.so:science/openbabel
CONFIGURE_ARGS+=--with-openbabel
PLIST_SUB+=	BABEL=""
.else
CONFIGURE_ARGS+=--without-openbabel
PLIST_SUB+=	BABEL="@comment "
.endif

pre-configure:
	@${REINPLACE_CMD} -e 's|$$datadir/doc/$$PACKAGE/examples|${EXAMPLESDIR}|'	\
		-e 's| CFLAGS="-O2"|#CFLAGS="-O2"|;s|Werror ansi pedantic-errors||'	\
		-e 's| FCFLAGS="-O2"|FCFLAGS=${FFLAGS}|;s|$${docdir}/examples|${EXAMPLESDIR}|'	\
		${WRKSRC}/${CONFIGURE_SCRIPT}

pre-configure-NLS-off:
	@${REINPLACE_CMD} -e '/^	po /d'	\
		${WRKSRC}/Makefile.in

pre-configure-DOCS-off:
	@${REINPLACE_CMD} -e '/^	Documentation/d'	\
		${WRKSRC}/Makefile.in

pre-configure-EXAMPLES-off:
	@${REINPLACE_CMD} -e '/^	examples/d'	\
		${WRKSRC}/Makefile.in
	@${FIND} ${WRKSRC}/lib/plug-ins -name Makefile.in | ${XARGS} \
		${REINPLACE_CMD} -e	\
		's|install-v_simexamplesDATA install-v_simpixmapsDATA|install-v_simpixmapsDATA|'
	@${REINPLACE_CMD} -e 's|: install-v_simexamplesDATA|:|'	\
		${WRKSRC}/lib/python/Makefile.in

.include <bsd.port.mk>
