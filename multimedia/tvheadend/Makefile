# Created by: Bernhard Froehlich <decke@FreeBSD.org>
# $FreeBSD$

PORTNAME=	tvheadend
PORTVERSION=	4.0.8
DISTVERSIONPREFIX=	v
PORTREVISION=	5
CATEGORIES=	multimedia

MAINTAINER=	decke@FreeBSD.org
COMMENT=	TV streaming server supporting DVB, IPTV and V4L

LICENSE=	GPLv3

BROKEN_powerpc64=	Does not build

BUILD_DEPENDS=	bash:shells/bash \
		${LOCALBASE}/include/linux/videodev2.h:multimedia/v4l_compat

LIB_DEPENDS=	libcurl.so:ftp/curl \
		liburiparser.so:net/uriparser
RUN_DEPENDS=	dtv-scan-tables>=0:multimedia/dtv-scan-tables

USE_GITHUB=	yes

USES=		execinfo gmake pkgconfig python shebangfix
GNU_CONFIGURE=	yes
MAKE_JOBS_UNSAFE=	yes
USE_RC_SUBR=	${PORTNAME}

SHEBANG_FILES=	${WRKSRC}/Autobuild.sh \
		${WRKSRC}/configure \
		${WRKSRC}/support/apt-update \
		${WRKSRC}/support/configure.inc \
		${WRKSRC}/support/getmuxlist \
		${WRKSRC}/support/version

TVHUSER?=	tvheadend
TVHGROUP?=	tvheadend

USERS=		${TVHUSER}
GROUPS=		${TVHGROUP} webcamd
SUB_LIST+=	TVHUSER="${TVHUSER}" \
		TVHGROUP="${TVHGROUP}"

CONFIGURE_ENV+=	PLATFORM=freebsd
CONFIGURE_ARGS+=--disable-dvbscan --disable-zlib
CFLAGS+=	-I${LOCALBASE}/include -Wno-conversion -Wno-int-to-pointer-cast
LDFLAGS+=	-L${LOCALBASE}/lib -lexecinfo -lssl -lcrypto -lz

OPTIONS_DEFINE=	AVAHI DVBCSA HDHOMERUN INOTIFY TRANSCODING XMLTV
OPTIONS_DEFAULT=DVBCSA HDHOMERUN INOTIFY

AVAHI_LIB_DEPENDS=	libavahi-client.so:net/avahi-app

DVBCSA_DESC=		Replace internal ffdecsa with dvbcsa
DVBCSA_LIB_DEPENDS=	libdvbcsa.so:multimedia/libdvbcsa
DVBCSA_CONFIGURE_ENABLE=	dvbcsa

HDHOMERUN_DESC=		Native support for HDHomeRun network tuners
HDHOMERUN_LIB_DEPENDS=	libhdhomerun.so:multimedia/libhdhomerun
HDHOMERUN_CONFIGURE_ENABLE=	hdhomerun_client

INOTIFY_DESC=		File system notifications support
INOTIFY_LIB_DEPENDS=	libinotify.so:devel/libinotify
INOTIFY_CONFIGURE_ENABLE=	inotify
INOTIFY_LDFLAGS=	-linotify

TRANSCODING_DESC=	Transcoding for mobile and web clients
TRANSCODING_LIB_DEPENDS=	libavcodec.so:multimedia/ffmpeg
TRANSCODING_CONFIGURE_ENABLE=	libav

XMLTV_RUN_DEPENDS=	p5-xmltv>=0:textproc/p5-xmltv

# CLANG also needs -Wno-microsoft which GCC 4.2 does not know
CFLAGS+=	-Wno-microsoft \
		-Wno-tautological-constant-out-of-range-compare

post-patch:
	@${REINPLACE_CMD} 's|0.0.0~unknown|${PORTVERSION}-${GH_TAGNAME}|' \
		${WRKSRC}/support/version
	@${REINPLACE_CMD} 's|-g||' \
		${WRKSRC}/Makefile
	@${REINPLACE_CMD} 's|-ldl||' \
		${WRKSRC}/Makefile
	# ignore gcc warnings on FreeBSD 9.X, and clang warnings on FreeBSD 10+
	@${REINPLACE_CMD} 's|-Werror||' \
		${WRKSRC}/Makefile
	# Remove -Wunused-result because gcc 4.2 on FreeBSD 9.x does not know it
	@${REINPLACE_CMD} 's|-Wunused-result||' \
		${WRKSRC}/Makefile
	# to suppress clang CC flags on FreeBSD 10+
	# @${REINPLACE_CMD} 's|clang|clang-options-disabled|' \
	# to enable clang CC flags, because clang is identified as 'cc' on FreeBSD 10+
	@${REINPLACE_CMD} 's|clang|cc|' \
		${WRKSRC}/Makefile

.include <bsd.port.mk>
