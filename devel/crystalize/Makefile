PORTNAME=	crystalize
CATEGORIES=	devel
DIST_SUBDIR=	aloli-crystal

MAINTAINER=	philippe@aloli.fr
COMMENT=	Package a Ruby (Rails) application into a single executable
WWW=		https://github.com/aloli-crystal/crystalize

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	crystal:lang/crystal
# Bibliothèques liées (ligne d'édition de liens de crystal build pour
# FreeBSD, yaml pour les configurations) ; OpenSSL : système de base.
LIB_DEPENDS=	libgc-threaded.so:devel/boehm-gc-threaded \
		libpcre2-8.so:devel/pcre2 \
		libyaml-0.so:textproc/libyaml
# `crystalize build` extrait la branche de l'application par git ; les
# ports produits par `crystalize port` n'en ont pas besoin (--from-dist).
RUN_DEPENDS=	git:devel/git

USES=		pkgconfig ssl
USE_GITHUB=	yes
GH_ACCOUNT=	aloli-crystal
GH_PROJECT=	crystalize

# Sources : archive du projet (Tools/shards-tuples,
# SHARDS_TUPLES_DIST_SUBDIR=aloli-crystal) ; aucun shard à l'exécution.
.include "${.CURDIR}/Makefile.shards"

PLIST_FILES=	bin/crystalize

_CRYSTALIZE_ENV=	HOME=${WRKDIR} \
			CRYSTAL_CACHE_DIR=${WRKDIR}/crystal-cache

do-build:
	${MKDIR} ${WRKSRC}/bin
	cd ${WRKSRC} && ${SETENV} ${_CRYSTALIZE_ENV} crystal build --release --no-debug \
		-o bin/crystalize src/cli.cr

# Le lanceur des applications est une copie de ce binaire : une charge
# utile lui est ajoutée en fin de fichier, il peut donc être allégé ici.
do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/bin/crystalize ${STAGEDIR}${PREFIX}/bin

.include <bsd.port.mk>
