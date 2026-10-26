PORTNAME=	clevis-zfs
CATEGORIES=	security
DIST_SUBDIR=	aloli-crystal

MAINTAINER=	philippe@aloli.fr
COMMENT=	Tang client for unattended unlock of ZFS encrypted datasets at boot
WWW=		https://github.com/aloli-crystal/clevis-zfs

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	crystal:lang/crystal
# Bibliothèques liées (ligne d'édition de liens de crystal build pour
# FreeBSD) ; OpenSSL : système de base.
LIB_DEPENDS=	libgc-threaded.so:devel/boehm-gc-threaded \
		libpcre2-8.so:devel/pcre2

USES=		pkgconfig ssl
USE_GITHUB=	yes
GH_ACCOUNT=	aloli-crystal
GH_PROJECT=	clevis-zfs

# Sources : archive du projet et de ses shards (Tools/shards-tuples,
# SHARDS_TUPLES_DIST_SUBDIR=aloli-crystal).
.include "${.CURDIR}/Makefile.shards"

PLIST_FILES=	etc/rc.d/clevis_zfs \
		sbin/clevis-zfs

_CLEVIS_ENV=	HOME=${WRKDIR} \
		CRYSTAL_CACHE_DIR=${WRKDIR}/crystal-cache

# Script rc.d du projet : chemins en /usr/local, ramenés au PREFIX.
post-patch:
	${REINPLACE_CMD} 's|/usr/local|${PREFIX}|g' ${WRKSRC}/etc/rc.d/clevis_zfs

do-build:
	${MKDIR} ${WRKSRC}/bin
	cd ${WRKSRC} && ${SETENV} ${_CLEVIS_ENV} crystal build --release --no-debug \
		-o bin/clevis-zfs src/cli.cr

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/bin/clevis-zfs ${STAGEDIR}${PREFIX}/sbin
	${INSTALL_SCRIPT} ${WRKSRC}/etc/rc.d/clevis_zfs \
		${STAGEDIR}${PREFIX}/etc/rc.d/clevis_zfs

.include <bsd.port.mk>
