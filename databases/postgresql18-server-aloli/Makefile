# Serveur sans JIT (LLVM), aloli
PKGNAMESUFFIX=	18-server-aloli
COMMENT=	PostgreSQL database server (without LLVM JIT, aloli)

# Depuis PostgreSQL 19, databases/postgresql18-server n'est plus un port
# maître : son Makefile pose ses variables puis inclut
# "${.CURDIR}/../postgresql19-server/Makefile". Inclus depuis cet overlay,
# ${.CURDIR} désignerait /overlays/aloli/databases/… et l'inclusion
# échouerait. On reproduit donc ce que fait postgresql18-server : ses
# variables, puis le Makefile de postgresql19-server, avec MASTERDIR sur
# postgresql18-server pour en garder distinfo, pkg-plist-* et files/.
PG18=		${PORTSDIR}/databases/postgresql18-server
MASTERDIR=	${PG18}

# Version et révision lues dans postgresql18-server : elles le suivent
# sans copie à tenir à jour.
DISTVERSION!=	sed -n 's/^DISTVERSION?=[[:space:]]*//p' ${PG18}/Makefile
PORTREVISION!=	sed -n 's/^PORTREVISION?=[[:space:]]*//p' ${PG18}/Makefile

# Le reste, repris de postgresql18-server (18.6).
MAINTAINER=	pgsql@FreeBSD.org
INSTALL_DIRS=	src/common src/timezone src/backend \
		src/backend/utils/mb/conversion_procs \
		src/backend/snowball src/backend/replication/libpqwalreceiver \
		src/backend/replication/pgoutput \
		src/bin/initdb src/bin/pg_ctl \
		src/bin/pg_archivecleanup src/bin/pg_basebackup \
		src/bin/pg_checksums src/bin/pg_combinebackup \
		src/bin/pg_controldata src/bin/pg_resetwal src/pl \
		src/bin/pg_rewind \
		src/bin/pg_test_fsync src/bin/pg_test_timing \
		src/bin/pg_waldump src/bin/pg_walsummary src/bin/pg_upgrade
PG_USER=	postgres
PG_GROUP=	postgres
PG_UID=		770

# Paquet MAISON : nom distinct de tout paquet officiel (jamais de chevauchement).
CONFLICTS_INSTALL=	postgresql18-server

# Sans compilation JIT des requêtes : l'option LLVM tire llvm19 (~1 Go) et,
# par lui, python3xx, pour un gain réservé aux grosses requêtes analytiques.
# OPTIONS_EXCLUDE et non OPTIONS_DEFAULT : le port maître ajoute LLVM à
# OPTIONS_DEFAULT après l'inclusion.
OPTIONS_EXCLUDE=	LLVM

.include "${PORTSDIR}/databases/postgresql19-server/Makefile"
