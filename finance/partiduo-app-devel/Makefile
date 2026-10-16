# Version de développement : branche development de partiduo-dist (dépôts
# partiduo-* sur development), destinée au serveur de test. Une seule
# version par serveur : elle s'installe aux mêmes emplacements que
# partiduo-app et l'exclut.
PKGNAMESUFFIX=	-devel

COMMENT=	Invoicing and accounting for small businesses (development version)

CONFLICTS_INSTALL=	partiduo-app

# Sans optimisation (serveur de test) mais en un seul module LLVM : sans
# --single-module, l'édition de liens reçoit des milliers de fichiers objets
# et dépasse ARG_MAX (« Argument list too long », builder, 16 oct. 2026).
PARTIDUO_OPTIMIZE=	--single-module

MASTERDIR=	${.CURDIR}/../partiduo-app

.include "${MASTERDIR}/Makefile"
