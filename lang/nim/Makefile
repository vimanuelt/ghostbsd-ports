# Created by: Neal Nelson <ports@nicandneal.net>
# $FreeBSD$

PORTNAME=	nim
PORTVERSION=	1.2.6
CATEGORIES=	lang
MASTER_SITES=	https://nim-lang.org/download/

MAINTAINER=	ports@nicandneal.net
COMMENT=	Nim programming language

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/copying.txt

BROKEN_mips64=		fails to build: build.sh: clang: not found
BROKEN_sparc64=		fails to build: build.sh: clang: not found

TEST_DEPENDS=	node>=0:www/node

USES=		compiler tar:xz
USE_LDCONFIG=	yes

TOOLS_DESC=	Build and install nim tools

OPTIONS_DEFINE=	DOCS TOOLS
OPTIONS_DEFAULT=	DOCS TOOLS

TEST_BINARY_ALIAS=	node=nodejs

OPTIONS_SUB=	yes

MAKE_JOBS_UNSAFE=	yes

.include <bsd.port.pre.mk>

TLIST=	nimfind nimgrep nimpretty nimsuggest testament

.if ${ARCH} == "powerpc64"
.  if defined(PPC_ABI) && ${PPC_ABI} == ELFv1
EXTRA_PATCHES=	${PATCHDIR}/elfv1-patch-build.sh
.  endif
.endif

post-patch:
.if defined(PPC_ABI) && ${PPC_ABI} == ELFv1
	@${REINPLACE_CMD} -e '/cc =/s/clang/gcc/' ${WRKSRC}/config/nim.cfg
.endif

do-build:
	cd ${WRKSRC} && ${SETENV} CC="${CC}" LINKER="${CC}" \
		COMP_FLAGS="${CPPFLAGS} ${CFLAGS}" LINK_FLAGS="${LDFLAGS}" \
		${SH} build.sh
	cd ${WRKSRC} && bin/nim c --parallelBuild=${MAKE_JOBS_NUMBER} \
		--nimcache=${WRKDIR}/nimcache -d:release koch
	cd ${WRKSRC} && ./koch boot --parallelBuild=${MAKE_JOBS_NUMBER} \
		-d:release --nimcache=${WRKDIR}/nimcache
	cd ${WRKSRC} && bin/nim c --parallelBuild=${MAKE_JOBS_NUMBER} \
		-d:release --nimcache=${WRKDIR}/nimcache --app:lib \
		-d:createNimRtl lib/nimrtl.nim

do-build-TOOLS-on:
	cd ${WRKSRC} && ./koch toolsNoNimble --parallelBuild=${MAKE_JOBS_NUMBER} \
		-d:release --nimcache=${WRKDIR}/nimcache

do-install:
	# Install nim compiler
	${INSTALL_PROGRAM} ${WRKSRC}/bin/nim ${STAGEDIR}${PREFIX}/bin
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/nim
	# Install lib nim rtl
	${INSTALL_LIB} ${WRKSRC}/lib/libnimrtl.so ${STAGEDIR}${PREFIX}/lib
	# Install nim lib files
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/${PORTNAME}
	(cd ${WRKSRC}/lib && ${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/${PORTNAME} "! -name libnimrtl.so")
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/${PORTNAME}/compiler
	(cd ${WRKSRC}/compiler && ${COPYTREE_SHARE} . ${STAGEDIR}${PREFIX}/lib/${PORTNAME}/compiler "! -name nim ! -name nim1")
	# Configuration files
	${MKDIR} ${STAGEDIR}${ETCDIR}
.for file in nim.cfg nimdoc.cfg nimdoc.tex.cfg
	${INSTALL_DATA} ${WRKSRC}/config/${file} ${STAGEDIR}${ETCDIR}
.endfor

do-install-DOCS-on:
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	(cd ${WRKSRC}/doc && ${COPYTREE_SHARE} . ${STAGEDIR}${DOCSDIR})

do-install-TOOLS-on:
.for t in ${TLIST}
	${INSTALL_PROGRAM} ${WRKSRC}/bin/${t} ${STAGEDIR}${PREFIX}/bin
.endfor

do-test:
	cd ${WRKSRC} && ${SETENV} ./koch test all -d:release \
	--nimcache=${WRKDIR}/nimcache --putenv:"PATH=${PATH}"

.include <bsd.port.post.mk>
