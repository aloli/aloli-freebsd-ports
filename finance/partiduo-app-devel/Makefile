# Version de développement : branche development de partiduo-dist (dépôts
# partiduo-* sur development), destinée au serveur de test. Une seule
# version par serveur : elle s'installe aux mêmes emplacements que
# partiduo-app et l'exclut.
PKGNAMESUFFIX=	-devel

COMMENT=	Invoicing and accounting for small businesses (development version)

CONFLICTS_INSTALL=	partiduo-app

# Sans --release : construction rapide, en parallèle (serveur de test).
PARTIDUO_OPTIMIZE=

MASTERDIR=	${.CURDIR}/../partiduo-app

.include "${MASTERDIR}/Makefile"
