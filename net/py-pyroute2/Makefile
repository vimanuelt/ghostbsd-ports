# Created by: Po-Chuan Hsieh <sunpoet@FreeBSD.org>
# $FreeBSD$

PORTNAME=	pyroute2
PORTVERSION=	0.5.13
CATEGORIES=	net python
MASTER_SITES=	CHEESESHOP
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	sunpoet@FreeBSD.org
COMMENT=	Python Netlink library

LICENSE=	APACHE20 GPLv2+
LICENSE_COMB=	dual
LICENSE_FILE_APACHE20=	${WRKSRC}/LICENSE.Apache.v2
LICENSE_FILE_GPLv2+ =	${WRKSRC}/LICENSE.GPL.v2

USES=		python
USE_PYTHON=	autoplist concurrent distutils

NO_ARCH=	yes

OPTIONS_DEFINE=	SS2
SS2_DESC=	Socket statistics

SS2_RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}psutil>=5.0<6:sysutils/py-psutil@${PY_FLAVOR}

.include <bsd.port.mk>
