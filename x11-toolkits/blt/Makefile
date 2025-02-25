PORTNAME=	blt
PORTVERSION=	2.5.3
PORTREVISION=	5
CATEGORIES=	x11-toolkits tk
MASTER_SITES=	SF/wize
DISTNAME=	${PORTNAME}-src-${PORTVERSION}

MAINTAINER=	doconnor@gsoft.com.au
COMMENT=	Extension to the Tk toolkit
WWW=		https://wiki.tcl-lang.org/page/BLT

LICENSE=	MIT

WRKSRC=		${WRKDIR}/${PORTNAME}${PORTVERSION:R}

USES=		gmake jpeg shebangfix tk:tea xorg zip
USE_XORG+=	x11
SHEBANG_FILES=	demos/scripts/xcolors.tcl
SHEBANG_LANG=	bltwish
bltwish_OLD_CMD=../bltwish
bltwish_CMD=	${PREFIX}/bin/bltwish # installed by this port, so not LOCALBASE
USE_LDCONFIG=	yes
CFLAGS+=	-fPIC -I${TCL_INCLUDEDIR}/generic \
		-I${TK_INCLUDEDIR}/generic \
		-I${TK_INCLUDEDIR}/unix
MAKE_JOBS_UNSAFE=yes

PLIST_SUB+=	BLTVER=${PORTVERSION:R} \
		BLTSHORTVER=${PORTVERSION:R:S/.//}

CONFIGURE_ARGS=	--enable-jpeg=${LOCALBASE} \

post-patch:
	${FIND} ${WRKSRC} -name Makefile.in | ${XARGS} \
	    ${REINPLACE_CMD} -e '/^INSTALL_ROOT/s|$$|$${DESTDIR}|'
	${FIND} ${WRKSRC}/demos -name "*.tcl" | ${XARGS} \
	    ${REINPLACE_CMD} -e 's|../src/bltwish|${LOCALBASE}/bin/bltwish|'

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/*
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/*.so

.include <bsd.port.pre.mk>

.if ${TCL_VER} > 8.5
CFLAGS+=	-DUSE_INTERP_RESULT -DUSE_INTERP_ERRORLINE
.endif

.if ${OPSYS} == FreeBSD && ${OSVERSION} >= 1400091
CFLAGS+=	-Wno-error=incompatible-function-pointer-types
.endif

.include <bsd.port.post.mk>
