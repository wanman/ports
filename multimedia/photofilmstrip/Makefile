# Created by: Charlie Kester <corky1951@comcast.net>
# $FreeBSD$

PORTNAME=	photofilmstrip
PORTVERSION=	3.0.2
CATEGORIES=	multimedia python
MASTER_SITES=	SF/photostoryx/${PORTNAME}/${PORTVERSION}
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	bofh@FreeBSD.org
COMMENT=	Creates movies out of your pictures

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3
RUN_DEPENDS=	mencoder:multimedia/mencoder \
		mplayer:multimedia/mplayer

USES=		python gettext
USE_PYTHON=	distutils autoplist
USE_WX=		3.0+
WX_COMPS=	python:lib
ALL_TARGET=	compile
NO_ARCH=	yes

.include <bsd.port.mk>
