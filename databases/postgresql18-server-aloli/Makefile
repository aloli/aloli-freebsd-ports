# Serveur sans JIT (LLVM), aloli
PKGNAMESUFFIX=	18-server-aloli
COMMENT=	PostgreSQL database server (without LLVM JIT, aloli)

MASTERDIR=	${PORTSDIR}/databases/postgresql18-server

# Paquet MAISON : nom distinct de tout paquet officiel (jamais de chevauchement).
CONFLICTS_INSTALL=	postgresql18-server

# Sans compilation JIT des requêtes : l'option LLVM tire llvm19 (~1 Go) et,
# par lui, python3xx, pour un gain réservé aux grosses requêtes analytiques.
# OPTIONS_EXCLUDE et non OPTIONS_DEFAULT : le port maître ajoute LLVM à
# OPTIONS_DEFAULT après l'inclusion.
OPTIONS_EXCLUDE=	LLVM

.include "${MASTERDIR}/Makefile"
