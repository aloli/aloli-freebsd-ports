PORTNAME=	partiduo-connect
CATEGORIES=	security
DIST_SUBDIR=	partiduo

MAINTAINER=	philippe@aloli.fr
COMMENT?=	Partiduo identity provider shared by all instances (OpenID Connect)
WWW=		https://github.com/aloli-partiduo/partiduo-admin

LICENSE=	AGPLv3+
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	crystal:lang/crystal
# Bibliothèques liées (ligne d'édition de liens de crystal build pour
# FreeBSD) ; OpenSSL et zlib : système de base.
LIB_DEPENDS=	libgc-threaded.so:devel/boehm-gc-threaded \
		libgmp.so:math/gmp \
		libpcre2-8.so:devel/pcre2 \
		libyaml.so:textproc/libyaml

USES=		pkgconfig ssl
USE_GITHUB=	yes
GH_ACCOUNT=	aloli-partiduo
GH_PROJECT=	partiduo-admin

# Un seul connect par serveur : partiduo-connect (branche production de
# partiduo-admin) et partiduo-connect-devel (branche development, port
# esclave) s'installent aux mêmes emplacements.
CONFLICTS_INSTALL?=	partiduo-connect-devel

USE_RC_SUBR=	partiduo_connect
SUB_FILES=	pkg-message
SUB_LIST=	PARTIDUO_CONNECT_HOME=${PARTIDUO_CONNECT_HOME} \
		PKGNAME=${PKGBASE}

# Tout ce que les programmes lisent à l'exécution est rangé à côté d'eux
# (bin/, share/partiduo-connect/{assets,artifacts}, voir
# config/connect/settings/production.cr de partiduo-admin).
PARTIDUO_CONNECT_HOME=	${PREFIX}/lib/partiduo-connect

# Sources : celles du port finance/partiduo-admin de la même branche (même
# dépôt, mêmes shards : Makefile.shards, distinfo), comme
# sysutils/partiduo-agent.
PARTIDUO_ADMIN_SOURCES?=	${.CURDIR}/../../finance/partiduo-admin
DISTINFO_FILE=	${PARTIDUO_ADMIN_SOURCES}/distinfo
.include "${PARTIDUO_ADMIN_SOURCES}/Makefile.shards"

# Marten : pas de précompilation de sa ligne de commande (postinstall de
# shards, non lancé ici) ; ses programmes sont construits directement.
_PARTIDUO_ENV=	HOME=${WRKDIR} \
		CRYSTAL_CACHE_DIR=${WRKDIR}/crystal-cache \
		SKIP_MARTEN_CLI_PRECOMPILATION=1
# Réglages exigés par la configuration de production pour lancer
# partiduo-connect-manage à la construction (aucune base n'est ouverte,
# aucune clé de signature n'est lue).
_PARTIDUO_MANAGE_ENV=	${_PARTIDUO_ENV} \
		MARTEN_ENV=production \
		MARTEN_SECRET_KEY=construction \
		DATABASE_URL="postgres:///construction?host=/nonexistent"

_PARTIDUO_PROGRAMS=	partiduo-connect:src/connect.cr \
			partiduo-connect-manage:connect_manage.cr

do-build:
	${MKDIR} ${WRKSRC}/bin
.for _p in ${_PARTIDUO_PROGRAMS}
	cd ${WRKSRC} && ${SETENV} ${_PARTIDUO_ENV} crystal build --release --no-debug \
		-o bin/${_p:C/:.*//} ${_p:C/^[^:]*://}
.endfor

do-install:
	${MKDIR} ${STAGEDIR}${PARTIDUO_CONNECT_HOME}/bin \
		${STAGEDIR}${PARTIDUO_CONNECT_HOME}/share/partiduo-connect \
		${STAGEDIR}${PREFIX}/etc/partiduo-connect \
		${STAGEDIR}${PREFIX}/etc/newsyslog.conf.d \
		${STAGEDIR}/var/db/partiduo-connect
.for _p in ${_PARTIDUO_PROGRAMS}
	${INSTALL_PROGRAM} ${WRKSRC}/bin/${_p:C/:.*//} ${STAGEDIR}${PARTIDUO_CONNECT_HOME}/bin
.endfor
# Fichiers statiques d'abord : une fois artifacts/ présent, il devient la
# racine des applications (config/connect/settings/production.cr).
	cd ${WRKSRC} && ${SETENV} ${_PARTIDUO_MANAGE_ENV} \
		PARTIDUO_CONNECT_ASSETS_ROOT=${STAGEDIR}${PARTIDUO_CONNECT_HOME}/share/partiduo-connect/assets \
		bin/partiduo-connect-manage collectassets --no-input
	cd ${WRKSRC} && ${SETENV} ${_PARTIDUO_MANAGE_ENV} \
		bin/partiduo-connect-manage collectartifacts --no-input \
		--dest-path ${STAGEDIR}${PARTIDUO_CONNECT_HOME}/share/partiduo-connect/artifacts
	${LN} -s ${PARTIDUO_CONNECT_HOME}/bin/partiduo-connect-manage \
		${STAGEDIR}${PREFIX}/bin/partiduo-connect-manage
	${INSTALL_DATA} ${FILESDIR}/connect.env.sample \
		${STAGEDIR}${PREFIX}/etc/partiduo-connect/connect.env.sample
	${INSTALL_DATA} ${FILESDIR}/partiduo-connect.newsyslog.conf \
		${STAGEDIR}${PREFIX}/etc/newsyslog.conf.d/partiduo-connect.conf.sample

post-install:
	cd ${STAGEDIR}${PREFIX} && ${FIND} lib/partiduo-connect \( -type f -o -type l \) | \
		${SORT} >> ${TMPPLIST}
	${ECHO_CMD} bin/partiduo-connect-manage >> ${TMPPLIST}
	${ECHO_CMD} "@sample(root,partiduo-connect,0640) etc/partiduo-connect/connect.env.sample" >> ${TMPPLIST}
	${ECHO_CMD} "@sample etc/newsyslog.conf.d/partiduo-connect.conf.sample" >> ${TMPPLIST}
# Répertoire de travail du compte partiduo-connect ; conservé s'il n'est
# pas vide.
	${ECHO_CMD} "@dir(partiduo-connect,partiduo-connect,0750) /var/db/partiduo-connect" >> ${TMPPLIST}

.include <bsd.port.mk>
