PORTNAME=	jawas
CATEGORIES=	sysutils databases
DIST_SUBDIR=	aloli-crystal

MAINTAINER=	philippe@aloli.fr
COMMENT=	Backup tool for MySQL/MariaDB, PostgreSQL and SQLite databases
WWW=		https://github.com/aloli-crystal/jawas

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	crystal:lang/crystal
# Bibliothèques liées (ligne d'édition de liens de crystal build pour
# FreeBSD) ; OpenSSL (SHA-256) : système de base. Les outils de dump
# (mysqldump, pg_dump, sqlite3) ne sont pas imposés : chaque serveur
# installe ceux de ses moteurs (voir pkg-message) ; gzip, bzip2, xz et
# zstd font partie du système de base.
LIB_DEPENDS=	libgc-threaded.so:devel/boehm-gc-threaded \
		libpcre2-8.so:devel/pcre2

USES=		pkgconfig ssl
USE_GITHUB=	yes
GH_ACCOUNT=	aloli-crystal
GH_PROJECT=	jawas

# Sources : archive du projet et de ses shards (Tools/shards-tuples,
# SHARDS_TUPLES_DIST_SUBDIR=aloli-crystal).
.include "${.CURDIR}/Makefile.shards"

PLIST_FILES=	bin/jawas \
		"@sample etc/jawas/config.toml.sample"

_JAWAS_ENV=	HOME=${WRKDIR} \
		CRYSTAL_CACHE_DIR=${WRKDIR}/crystal-cache

do-build:
	${MKDIR} ${WRKSRC}/bin
	cd ${WRKSRC} && ${SETENV} ${_JAWAS_ENV} crystal build --release --no-debug \
		-o bin/jawas src/cli.cr
	${WRKSRC}/bin/jawas sample > ${WRKSRC}/config.toml.sample

# jawas cherche /usr/local/etc/jawas/config.toml (après la configuration
# XDG de l'utilisateur et /etc/jawas/config.toml).
do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/bin/jawas ${STAGEDIR}${PREFIX}/bin
	${MKDIR} ${STAGEDIR}${PREFIX}/etc/jawas
	${INSTALL_DATA} ${WRKSRC}/config.toml.sample \
		${STAGEDIR}${PREFIX}/etc/jawas/config.toml.sample

.include <bsd.port.mk>
