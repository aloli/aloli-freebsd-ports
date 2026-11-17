# Serveur PostgreSQL 19 sans JIT (LLVM), aloli — canal -devel.
#
# Destiné au SERVEUR DE TEST : valider la chaîne 18 → 19 (paquet,
# `beryl pg-upgrade --to 19`, applications) avant tout serveur de
# production. Promu ensuite en postgresql19-server-aloli.
PKGNAMESUFFIX=	19-server-aloli-devel
COMMENT=	PostgreSQL database server (without LLVM JIT, aloli, development)

# Esclave DIRECT de databases/postgresql19-server : depuis PostgreSQL 19,
# c'est lui le port maître (postgresql18-server l'inclut à son tour), ses
# distinfo, pkg-plist-server et files/ servent tels quels. Aucune version
# copiée ici : celle de l'arbre de poudriere (branche trimestrielle), donc
# du même âge que le client officiel postgresql19-client qu'il tire
# (USES=pgsql). 17 nov. 2026 : 19beta4 sur 2026Q4 (19rc2 sur main, 19.0
# annoncée pour le 19 nov.). Entre deux pré-versions, le format des données
# peut changer : un serveur de test passé en 19beta4 repasse par pg_upgrade
# (ou dump/restore) pour la 19.0.
MASTERDIR=	${PORTSDIR}/databases/postgresql19-server

# Paquet MAISON : nom distinct de tout paquet officiel. Les autres versions
# majeures (dont postgresql18-server-aloli) sont déjà exclues par le port
# maître (CONFLICTS postgresql1[^9]*).
CONFLICTS_INSTALL=	postgresql19-server postgresql19-server-aloli

# Sans compilation JIT des requêtes : l'option LLVM tire llvm19 (~1 Go) et,
# par lui, python3xx, pour un gain réservé aux grosses requêtes analytiques.
# OPTIONS_EXCLUDE et non OPTIONS_DEFAULT : le port maître ajoute LLVM à
# OPTIONS_DEFAULT après l'inclusion.
OPTIONS_EXCLUDE=	LLVM

.include "${MASTERDIR}/Makefile"
